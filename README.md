## Veracode SCA Agent-Based Project to Application Profile Link Script

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
