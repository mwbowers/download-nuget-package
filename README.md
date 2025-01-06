[![Test Download NuGet Package Action](https://github.com/mwbowers/download-nuget-package/actions/workflows/test-action.yml/badge.svg)](https://github.com/mwbowers/download-nuget-package/actions/workflows/test-action.yml)

# Download NuGet Package Action

This GitHub Action downloads a specified NuGet package copies it to a specified directory. It supports both public and private NuGet feeds, and you can specify the version of the package to be downloaded.

## Features
- Downloads NuGet packages from specified sources.
- Supports public (nuget.org) and private feeds (via `nuget.config`).
- Allows specifying a version of the package.
- Copies the downloaded `.nupkg` file to a target directory.

## Inputs

### `package-name` (required)
The name of the NuGet package to download. This can be the full package name (including the version) or just the package name (e.g., `Newtonsoft.Json`).

### `package-directory` (required)
The directory where the downloaded `.nupkg` file will be copied. This directory must exist prior to running the action.

### `version` (optional)
The version of the NuGet package to download. If not specified, the latest version will be downloaded. The version format should match the NuGet versioning (e.g., `12.0.3`).

### `nuget-config` (optional)
Path to the `nuget.config` file to use for downloading the package from a private feed. If not specified, the default public NuGet feed (`nuget.org`) will be used.

## Outputs

- **`package-path`**: The path of the `.nupkg` file that was downloaded and copied to the specified directory.

## Example Usage

### Basic Example (Default Public Feed)
This example demonstrates how to download a package from `nuget.org` without specifying a version:

```yaml
jobs:
  download_nuget_package:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Download NuGet Package
        uses: ./github/actions/download-nuget-package
        with:
          package-name: 'Newtonsoft.Json'
          package-directory: './test-packages'
```

### Example with Version and Private Feed
This example demonstrates how to download a specific version of a package from a private NuGet feed using a `nuget.config` file:

```yaml
jobs:
  download_nuget_package:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Download NuGet Package
        uses: ./github/actions/download-nuget-package
        with:
          package-name: 'ASNA.QSys.MonaServer'
          version: '5.0.28'
          nuget-config: './config/nuget.config'
          package-directory: './test-packages'
```

### Example with Version Specified
This example demonstrates how to download a specific version of a package from `nuget.org`:

```yaml
jobs:
  download_nuget_package:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Download Specific Version of Package
        uses: ./github/actions/download-nuget-package
        with:
          package-name: 'Newtonsoft.Json'
          version: '12.0.3'
          package-directory: './test-packages'
```

## How It Works
1. The action checks if a nuget.config file is provided. If so, it uses it.
2. The action downloads the specified package or version.
3. The downloaded .nupkg file is copied to the target directory (package-directory).
4. If the package cannot be found or downloaded, the action will fail.

## Error Handling
If the package is not found or the version is incorrect, the action will exit with a non-zero exit code and display an error message:

```
No package found matching <package-name>*.nupkg
```

If a specific version is requested and not found, the error message will reflect that:

```
No package found matching <package-name>.<version>*.nupkg
```

## Contributing
Feel free to fork this repository and create a pull request with any improvements or fixes. Please ensure your changes are covered by tests, and that all tests pass before submitting a pull request.

## License
This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for more information.
