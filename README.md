# Veracode SCA Agent-Based Scan to Application Profile Link Script

This repository contains a Python script that links Veracode SCA Agent-Based projects to a corresponding application profile. The script automates the process of associating a project scanned by the Veracode SCA agent with its corresponding application profile using the Veracode APIs.

## Use-Case Example

This script can be used within a CI/CD pipeline as a post-build step after running an upload & scan within an application profile AND an SCA Agent-Based scan within a workspace. Here's an example of utilizing this script in a Jenkins Pipeline which builds the application, sends the built application to Veracode for an upload & scan, runs an Agent-Based scan, and links the two together:

```groovy
pipeline {
    agent any

    stages {
        
        stage('Git') {
            steps {
                git 'https://github.com/tsaekao/nodegoat.git'
            }
        }
        stage('Package Application') {
            steps {
                sh 'curl -fsS https://tools.veracode.com/veracode-cli/install | sh'
                sh './veracode package -s . -o veracode-artifact -a trust'
            }
        }

        stage('Upload SAST') {
            when {
                expression {
                    return true // Set this to true if you want to run this stage
                }
            }
            steps {
                withCredentials([usernamePassword(credentialsId: '3dd8eb7f-547e-4b79-8a6c-f39954a06c63', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    veracode applicationName: 'nodegoat',
                    // createSandbox: false,
                    // sandboxName: 'JenkinsFile',
                    criticality: 'VeryHigh',
                    scanName: '$buildnumber',
                    uploadIncludesPattern: 'veracode-artifact/',
                    vid: USER,
                    vkey: PASS
                    sh "pwd"
                    echo "sast"
                }
            }
        }

        stage('SCA Agent-Based Scan') {
            steps {
                withCredentials([string(credentialsId: 'SRCCLR_API_TOKEN', variable: 'SRCCLR_API_TOKEN')]) {
                    catchError(buildResult: 'SUCCESS', message: 'SUCCESS') {
                        sh "srcclr scan . --allow-dirty" 
                        echo "SCA Agent-Based Scan"
                    }
                }
            }
        }

        stage('Link SCA Project to Application Profile') {
            steps {
                sh '''
                    apt-get update && apt-get install -y python3 python3-pip || true
                    pip3 install veracode-api-signing --upgrade
                '''
                withCredentials([usernamePassword(credentialsId: '3dd8eb7f-547e-4b79-8a6c-f39954a06c63', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh '''
                        python3 link_sca_project.py --workspace_name "Jenkins Demo" --project_name "tsaekao/nodegoat" --application_profile "nodegoat" --api_id "${USER}" --api_key "${PASS}"
                    '''
                }
            }
        }
    }
    post { 
        always { 
            cleanWs()
        }
    }
}
```

## Prerequisites

- **Python 3.x**: Ensure you have Python 3 installed on your system.
- **Veracode API Credentials**: Generate your `VERACODE_API_KEY_ID` and `VERACODE_API_KEY_SECRET`. Refer to the [Veracode API Documentation](https://docs.veracode.com/r/c_getting_started_with_the_veracode_api) for guidance.

## Installation

1. **Clone the Repository**

   ```bash
   git clone https://github.com/tsaekao/veracode-scripts.git
   cd veracode-scripts
   ```

2. **Install Dependencies**

   Install the required Python packages using `pip`:

   ```bash
   pip install -r requirements.txt
   ```

## Script Usage

The script requires the following parameters:

- `--workspace_name`: The name of the SCA workspace.
- `--project_name`: The name of the SCA project.
- `--application_profile`: The name of the Veracode application profile.
- `--api_id`: Your Veracode API ID.
- `--api_key`: Your Veracode API Key.

### Running the Script

Run the script using the following command:

   ```bash
   python link_sca_project.py \
      --workspace_name "YourWorkspaceName" \
      --project_name "YourProjectName" \
      --application_profile "YourAppProfileName" \
      --api_id "YourVeracodeAPIID" \
      --api_key "YourVeracodeAPIKey"
   ```

**Note**: Replace the placeholder text with your actual workspace name, project name, application profile name, API ID, and API key.

### Example

   ```bash
   python link_sca_project.py \
      --workspace_name "MyWorkspace" \
      --project_name "MySCAScanProject" \
      --application_profile "MyAppProfile" \
      --api_id "123abc456def789ghi" \
      --api_key "your_api_key_here"
   ```

## Obtaining Veracode API Credentials

1. Log in to your Veracode account.
2. Navigate to **API Credentials** under your user profile.
3. Generate a new set of API credentials.
4. Store your `API ID` and `API Key` securely.

## Dependencies

The script relies on the following Python packages:

- `requests`: For making HTTP requests to the Veracode API.
- `veracode-api-signing`: For HMAC authentication with Veracode APIs.

These dependencies are specified in the `requirements.txt` file.

## Understanding the Script

The script performs the following steps:

1. **Fetch Workspace GUID**: Retrieves the GUID of the specified SCA workspace.
2. **Fetch Project GUID**: Retrieves the GUID of the specified SCA project within the workspace.
3. **Fetch Application GUID**: Retrieves the GUID of the specified Veracode application profile.
4. **Link Project to Application**: Links the SCA project to the application profile using the Veracode API.

## Error Handling

- If the workspace, project, or application profile is not found, the script will raise a `ValueError` with an appropriate message.
- HTTP errors from API requests are raised using `response.raise_for_status()`.

## Veracode API Documentation

- [Getting Started with Veracode APIs](https://docs.veracode.com/r/c_getting_started_with_the_veracode_api)
- [Veracode REST APIs](https://docs.veracode.com/r/c_rest_apis)
- [Veracode SCA API Overview](https://docs.veracode.com/r/c_srcclr_api_getting_started)

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
