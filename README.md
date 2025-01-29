# Initial ROM Build Tester

A GitHub Action designed to test the buildability of Android ROMs within an hour. This action streamlines the process of configuring the environment, syncing the source code, and initiating a test build for Android ROMs.

## Features

- **Quick Buildability Testing**: Ensures that an Android ROM can be initialized, synced, and built within a specified time window.
- **Customizable Inputs**: Supports various ROM configurations and build commands.
- **Automated Environment Setup**: Installs all necessary dependencies for building Android ROMs.
- **Error Handling**: Logs failures and provides helpful debug information.
- 
## How It Works

The action automates the following steps:
1. **Environment Setup**: Installs dependencies required for Android ROM builds.
2. **ROM Sync**: Initializes and syncs the ROM repository and local manifests.
3. **Build Test**: Compiles the ROM for a specified target device.
4. **Build Status Report**: Outputs whether the build succeeded or failed.



## Usage

### Prerequisites

Before using this action, ensure:
- You have a valid `repo init` command for the ROM you want to test.
- The `LOCAL_MANIFEST` is either:
  - A direct URL to an XML manifest file.
  - A Git repository containing local manifest(s).

### Example Workflow: `test-build.yml`

Create a workflow file in `.github/workflows/test-build.yml`:

```yml
name: Test ROM Build

on:
  workflow_dispatch:
    inputs:
      REPO_INIT_CMD:
        description: 'Repo init command (e.g., repo init -u https://github.com/LineageOS/android.git -b lineage-20.0)'
        required: true
      MANIFEST_BRANCH:
        description: 'ROM Branch (e.g., lineage-20.0, android-13.0.0_r41)'
        required: true
      LOCAL_MANIFEST:
        description: 'Raw local manifest link or repository URL'
        required: true
      LOCAL_MANIFEST_BRANCH:
        description: 'Local Manifest Branch (optional)'
        required: false
        default: ''
      LUNCH_COMMAND:
        description: 'Lunch command (e.g., lunch lineage_blossom-userdebug)'
        required: true
        default: 'lunch lineage_blossom-userdebug'
      BUILD_COMMAND:
        description: 'Build target (e.g., mka bacon)'
        required: false
        default: 'mka bacon'
      TEST_BUILD_DURATION:
        description: 'Maximum build test duration in minutes'
        required: false
        default: '60'

jobs:
  test-build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Repository
        uses: actions/checkout@v3

      - name: Run Initial ROM Build Tester
        uses: mlm-games/initial-rom-build-tester@main
        with:
          REPO_INIT_CMD: ${{ inputs.REPO_INIT_CMD }}
          MANIFEST_BRANCH: ${{ inputs.MANIFEST_BRANCH }}
          LOCAL_MANIFEST: ${{ inputs.LOCAL_MANIFEST }}
          LOCAL_MANIFEST_BRANCH: ${{ inputs.LOCAL_MANIFEST_BRANCH }}
          LUNCH_COMMAND: ${{ inputs.LUNCH_COMMAND }}
          BUILD_COMMAND: ${{ inputs.BUILD_COMMAND }}
          TEST_BUILD_DURATION: ${{ inputs.TEST_BUILD_DURATION }}
```



## Inputs

| **Input Name**         | **Description**                                                                                     | **Required** | **Default**                    |
|-|--|--|--|
| `REPO_INIT_CMD`         | The `repo init` command to initialize the ROM repository.                                            | Yes          | N/A                            |
| `MANIFEST_BRANCH`       | The branch of the ROM manifest to use (e.g., `lineage-20.0`, `android-13.0.0_r41`).                  | Yes          | N/A                            |
| `LOCAL_MANIFEST`        | URL or Git repository for the local manifest.                                                       | Yes          | N/A                            |
| `LOCAL_MANIFEST_BRANCH` | The branch of the local manifest repository (leave blank if not applicable).                        | No           | `''`                           |
| `LUNCH_COMMAND`         | The lunch or breakfast command to select the build target (e.g., `lunch lineage_blossom-userdebug`).| Yes          | `lunch lineage_blossom-userdebug` |
| `BUILD_COMMAND`         | The build command to execute (e.g., `mka bacon`).                                                   | No           | `mka bacon`                    |
| `TEST_BUILD_DURATION`   | Maximum duration (in minutes) to test the build process.                                            | No           | `60`                           |



## Outputs

The action sets the following environment variable to indicate the build status:
- `BUILD_STATUS`: `success` if the build succeeded, or `failure` if the build failed or timed out.



## How to Run

1. Navigate to the **Actions** tab in your GitHub repository.
2. Select the **Test ROM Build** workflow.
3. Click **Run workflow** and provide the required inputs:
   - `REPO_INIT_CMD`
   - `MANIFEST_BRANCH`
   - `LOCAL_MANIFEST`
   - Any other optional inputs as needed.
4. Monitor the workflow execution logs to view the build progress.



## Example Inputs

Here’s an example of the workflow inputs for testing a LineageOS ROM:

- **REPO_INIT_CMD**: `repo init -u https://github.com/LineageOS/android.git -b lineage-20.0`
- **MANIFEST_BRANCH**: `lineage-20.0`
- **LOCAL_MANIFEST**: `https://github.com/username/local_manifest.git`
- **LOCAL_MANIFEST_BRANCH**: `main`
- **LUNCH_COMMAND**: `lunch lineage_blossom-userdebug`
- **BUILD_COMMAND**: `mka bacon`
- **TEST_BUILD_DURATION**: `90`



## Debugging

If the workflow fails:
1. Check the logs for errors during the `repo init`, `repo sync`, or build steps.
2. Ensure the `LOCAL_MANIFEST` input is valid (a direct XML link or Git repository).
3. Verify that the `REPO_INIT_CMD` and `LUNCH_COMMAND` are correct for your ROM.
4. Increase the `TEST_BUILD_DURATION` if the build times out.

To enable debug mode, set the repository secret `ACTIONS_STEP_DEBUG` to `true`.



## Repository Structure

```
.
├── .github/
│   ├── workflows/
│   │   └── test-build.yml       # GitHub Actions workflow file
├── repos/
│   └── initial-rom-build-tester/
│       ├── action.yml           # GitHub Action definition
│       ├── README.md            # Documentation for the action (this file)
│       ├── setup-scripts/       # Optional setup scripts (if needed)
```



## Contributing

Contributions are welcome! If you encounter issues or want to enhance the action, feel free to open an issue or submit a pull request.



## License

This project is licensed under the GPL 3.0 License. See the [LICENSE](LICENSE) file for details.



## Acknowledgments

Special thanks to the Android ROM developer community for the tools, resources, and inspiration to create this action. 😊

 
