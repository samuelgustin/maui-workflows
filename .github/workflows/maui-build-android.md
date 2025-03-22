# maui-build-android Workflow

This workflow will build a .NET MAUI Android application and store it in GitHub artifacts.

## Inputs

#### Required

- `project-name`: The name of the project to build
- `apk-filename`: The name of the APK file to upload
  - This will be the `ApplicationId` (bundle id) value from the .csproj file followed by `-Signed.apk` (com.compaynay.mauiapp-Signed.apk)

#### Optional

- `dotnet-version`: Version of .NET to use, defaults to 9.0.x
- `dotnet-publish-version`: Version of .NET to use for publishing, defaults to 9.0
- `java-version`: Version of Java to use
- `project-directory`: The directory of the project to build (e.g. MyApp/)
- `appsettings-filename`: The name of the appsettings file
- `production-branch-name`: The branch name to use for production environment
- `staging-branch-pattern`: The branch pattern to use for staging environment

## Repository Secrets

#### Required

- `ANDROID_KEYSTORE_FILENAME`: The Android keystore filename
- `ANDROID_KEYSTORE_BASE64`: The base64 encoded Android keystore
- `ANDROID_KEYSTORE_PASSWORD`: The Android keystore password

Use the following commands to get the base64 string of the files from their local directory:

- `base64 -i FILE_NAME | pbcopy`

Follow [this guide](https://developer.android.com/studio/publish/app-signing) to understand the Android secrets needed

#### Optional

- `APPSETTINGS_BASE64`: The base64 encoded appsettings file
  - Make this a GitHub Environment Secret with the following environment names:
    - `development`
    - `staging`
    - `production`

## Project Configuration

You will need to add the following information to your .csproj file.

```xml
<Version>1.00</Version>
```

```xml
<!-- Android Release -->
<PropertyGroup Condition="'$(Configuration)|$(TargetFramework)|$(Platform)'=='Release|net9.0-android|AnyCPU'">
  <AndroidKeyStore>True</AndroidKeyStore>
  <AndroidSigningKeyStore>YOUR_ANDROID_CERT_KEYSTORE_NAME</AndroidSigningKeyStore>
  <AndroidSigningKeyAlias>YOUR_ALIAS_NAME</AndroidSigningKeyAlias>
</PropertyGroup>
```

## Usage

```yaml
name: Build MAUI Android

on:
  workflow_dispatch:

jobs:
  maui-build-android:
    uses: samuelgustin/maui-workflows/.github/workflows/maui-build-android.yml@main
    with:
      dotnet-version: "9.0.x"
      dotnet-publish-version: "9.0"
      java-version: "11"
      project-directory: "my-path/"
      project-name: "my_app"
      apk-filename: "my_app.apk"
      appsettings-filename: "appsettings.json"
      production-branch-name: "main"
      staging-branch-pattern: "release/"
    secrets:
      ANDROID_KEYSTORE_FILENAME: ${{ secrets.ANDROID_KEYSTORE_FILENAME }}
      ANDROID_KEYSTORE_PASSWORD: ${{ secrets.ANDROID_KEYSTORE_PASSWORD }}
      ANDROID_KEYSTORE_BASE64: ${{ secrets.ANDROID_KEYSTORE_BASE64 }}
      APPSETTINGS_BASE64: ${{ secrets.APPSETTINGS_BASE64 }}
```

## References

- [setup-java Action](https://github.com/actions/setup-java/tree/v4/)
- [base64-to-file Action](https://github.com/timheuer/base64-to-file/tree/v1.2/)
- [Get CSProj Version Action](https://github.com/marketplace/actions/get-csproj-version)
