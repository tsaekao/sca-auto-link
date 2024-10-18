# Veracode SCA Agent-Based Project to Application Profile Link Script

This repository contains a Python script that links Veracode SCA Agent-Based projects to a corresponding application profile. The script automates the process of associating a project scanned by the Veracode SCA agent with its corresponding application profile using the Veracode APIs.

## Features

- **Automated Linking**: Streamlines the process of linking SCA projects to application profiles.
- **HMAC Authentication**: Utilizes Veracode's HMAC authentication for secure API communication.
- **Error Handling**: Provides clear error messages for missing workspaces, projects, or application profiles.

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
