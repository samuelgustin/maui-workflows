# distribute-to-firebase Workflow

This workflow will setup firebase-tools from npm and distribute and artifact to Firebase AppDistribution.

A tag will be created - `v1.0.0+123-ios` or `v1.0.0+123-android` for runs on the `production-branch-name`.

## Inputs

#### Required

- `artifact-name`: The name of the artifact to upload
- `firebase-app-id`: The Firebase App ID
- `project-name`: The name of the project to build
- `application-platform`: The application platform (iOS or Android)

#### Optional

- `firebase-groups`: The Firebase groups to release the app to
- `project-directory:` The directory of the project to build (e.g. MyApp/)
- `production-branch-name`: The branch name to use for production environment

## Repository Secrets

#### Required

- `FIREBASE_TOKEN`: Your Firebase token for authentication

Follow [this guide](https://firebase.google.com/docs/cli#cli-ci-systems-firebase-token) to generate a Firebase token

## Project Configuration

You will need to add the following information to your .csproj file.

```xml
<Version>1.00</Version>
```

## Usage

```yaml
name: Build & Deploy MAUI iOS

on:
  workflow_dispatch:

env:
  DOTNET_NOLOGO: true # Disable the .NET logo
  DOTNET_SKIP_FIRST_TIME_EXPERIENCE: true # Disable the .NET first time experience
  DOTNET_CLI_TELEMETRY_OPTOUT: true # Disable sending .NET CLI telemetry

jobs:
  # You can customize the build portion here...

  deploy-ios-to-firebase:
    uses: samuelgustin/workflows/.github/workflows/distribute-to-firebase.yml@main
    with:
      artifact-name: "my_app.ipa"
      firebase-groups: "some_group"
      firebase-app-id: "12345"
      project-directory: "my_path/"
      project-name: "my_app"
      application-platform: "ios"
      production-branch-name: "main"
    secrets:
      FIREBASE_TOKEN: ${{ secrets.FIREBASE_TOKEN }}

  deploy-android-to-firebase:
    uses: samuelgustin/workflows/.github/workflows/distribute-to-firebase.yml@main
    with:
      artifact-name: "my_app.apk"
      firebase-groups: "some_group"
      firebase-app-id: "12345"
      project-directory: "my_path/"
      project-name: "my_app"
      application-platform: "android"
      production-branch-name: "main"
    secrets:
      FIREBASE_TOKEN: ${{ secrets.FIREBASE_TOKEN }}
```

## References

- [GitHub Tag Action](https://github.com/marketplace/actions/github-actions-create-tag)
- [Setup Node Action](https://github.com/actions/setup-node/releases/tag/v4.3.0)
- [Get CSProj Version Action](https://github.com/marketplace/actions/get-csproj-version)
