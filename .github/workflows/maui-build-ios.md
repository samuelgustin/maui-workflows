# maui-build-ios Workflow

This workflow will build a .NET MAUI iOS application and store it in GitHub artifacts.

## Inputs

#### Required

- `project-name`: The name of the project to build
- `ipa-filename`: The name of the IPA file to upload

#### Optional

- `macos-runner-name`: The type of runner to use
- `xcode-version`: Version of Xcode to use, defaults to 16.2.0
- `dotnet-version`: Version of .NET to use, defaults to 9.0.x
- `dotnet-publish-version`: Version of .NET to use for publishing, defaults to 9.0
- `project-directory`: The directory of the project to build (e.g. MyApp/)
- `appsettings-filename`: The name of the appsettings file
- `production-branch-name`: The branch name to use for production environment'
- `staging-branch-pattern`: The branch pattern to use for staging environment

## Repository Secrets

#### Required

- `IOS_SIGNING_CERTIFICATE_NAME`: The iOS signing certificate name
- `IOS_SIGNING_CERTIFICATE_BASE64`: The base64 encoded iOS signing certificate
- `IOS_SIGNING_CERTIFICATE_PASSWORD`: The iOS signing certificate password
- `IOS_PROVISIONING_PROFILE_BASE64`: The base64 encoded iOS provisioning profile
- `IOS_PROVISIONING_PROFILE_NAME`: The iOS provisioning profile name
- `IOS_KEYCHAIN_PASSWORD`: The iOS keychain password

Use the following command examples to get the base64 string of the files from their local directory:

```powershell
base64 -i BUILD_CERTIFICATE.p12 | pbcopy
base64 -i PROVISIONING_PROFILE.mobileprovision | pbcopy
```

- The `IOS_SIGNING_CERTIFICATE_PASSWORD` is created by you when you export it.
- You may want to have both a development & distribution certificate in the same repo. If so, make the secrets environment secrets and then use the same `IOS_SIGNING_CERTIFICATE_PASSWORD`.

Follow [this guide](https://developer.apple.com/help/account/certificates/certificates-overview) for Apple certificate & provisioning profile information

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
<!-- iOS Release -->
<PropertyGroup Condition="'$(Configuration)|$(TargetFramework)|$(Platform)'=='Release|net9.0-ios|AnyCPU'">
  <!-- CHANGE THESE VALUES -->
  <IpaPackageName>my_app.ipa</IpaPackageName>
  <CodesignProvision>YOUR_PROVISION_PROFILE_NAME</CodesignProvision>
  <CodesignKey>YOUR_CERTIFICATE_NAME</CodesignKey>

  <!-- TURN OFF THE INTERPRETER IF YOU DON'T NEED IT (its fine to leave on if you don't know) -->
  <!-- https://github.com/dotnet/maui/issues/13019 -->
  <UseInterpreter>true</UseInterpreter>

  <RuntimeIdentifier>ios-arm64</RuntimeIdentifier>
  <CreatePackage>false</CreatePackage>
  <BuildIpa>true</BuildIpa>
  <ArchiveOnBuild>true</ArchiveOnBuild>
</PropertyGroup>
```

## Usage

```yaml
name: Build MAUI iOS

on:
  workflow_dispatch:

jobs:
  maui-build-ios:
    uses: samuelgustin/maui-workflows/.github/workflows/maui-build-ios.yml@main
    with:
      macos-runner-name: "macos-15"
      xcode-version: "16.2"
      codesigning-identity: "my_codesigning_identity"
      codesigning-provisioning-profile-name: "distribution_profile_name"
      dotnet-version: "9.0.x"
      dotnet-publish-version: "9.0"
      project-directory: "my-path/"
      project-name: "my_app"
      ipa-filename: "my_app.ipa"
      appsettings-filename: "appsettings.json"
      production-branch-name: "main"
      staging-branch-pattern: "release/"
    secrets:
      IOS_SIGNING_CERTIFICATE_BASE64: ${{ secrets.IOS_SIGNING_CERTIFICATE_BASE64 }}
      IOS_SIGNING_CERTIFICATE_PASSWORD: ${{ secrets.IOS_SIGNING_CERTIFICATE_PASSWORD }}
      IOS_PROVISIONING_PROFILE_BASE64: ${{ secrets.IOS_PROVISIONING_PROFILE_BASE64 }}
      IOS_KEYCHAIN_PASSWORD: ${{ secrets.IOS_KEYCHAIN_PASSWORD }}
      APPSETTINGS_BASE64: ${{ secrets.APPSETTINGS_BASE64 }}
```

## References

- [GitHub Tag Action](https://github.com/marketplace/actions/github-actions-create-tag)
- [Setup Node Action](https://github.com/actions/setup-node/releases/tag/v4.3.0)
- [Get CSProj Version Action](https://github.com/marketplace/actions/get-csproj-version)
