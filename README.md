# Deploy NuGet Package Action

This GitHub Action automates the deployment of a NuGet package to a specified NuGet URL (such as GitHub Packages or nuget.org). It pushes a `.nupkg` artifact directly from your workflow, ideal for publishing .NET libraries or internal packages as part of your CI/CD process.

---

## Features

- Deploys a NuGet package (`.nupkg`) to a specified NuGet feed using `dotnet nuget push`.
- Supports authentication using a token.
- Skips uploading if the package version already exists (`--skip-duplicate`).
- Adds deployment details to the GitHub job summary.

---

## Inputs

| Name                   | Description                                                      | Required |
|------------------------|------------------------------------------------------------------|----------|
| `nuget-file-path`      | Full path to the NuGet package to deploy.                        | Yes      |
| `nuget-url`            | URL of the NuGet feed (e.g., GitHub Packages, nuget.org).        | Yes      |
| `nuget-package-name`   | Name of the NuGet package for this release.                      | Yes      |
| `nuget-package-version`| Version number of the NuGet package for this release.            | Yes      |
| `token`                | GitHub token for authentication to upload the package.           | Yes      |

---

## Usage

Add this action as a step after your build, pack, or release process.

### Example Workflow

```yaml
name: Deploy NuGet Package

on:
  release:
    types: [published]

jobs:
  deploy-nuget:
    runs-on: windows-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v6
      
      - name: Deploy NuGet Package
        uses: lee-lott-actions/deploy-nuget-package@v1
        with:
          nuget-file-path: 'src/bin/Release/MyLib.1.0.0.nupkg'
          nuget-url: 'https://nuget.pkg.github.com/your-org/index.json'
          nuget-package-name: 'MyLib'
          nuget-package-version: '1.0.0'
          token: ${{ secrets.GITHUB_TOKEN }}
```

---

## Notes

- The action uses [`dotnet nuget push`](https://learn.microsoft.com/en-us/dotnet/core/tools/dotnet-nuget-push) and requires the `.nupkg` file path to be correct and available on the runner.
- The provided token should have `write:packages` or equivalent permission for GitHub Packages or the relevant NuGet feed.
- The action does **not** fail on duplicate version uploads; it will just continue if the package version has already been published (`--skip-duplicate`).

---
