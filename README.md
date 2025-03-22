# Overview

This repository contains reusable GitHub actions & workflows for .NET MAUI.

The workflows to build iOS or Android MAUI apps can be used in conjuntion with any distribution workflow.

## Workflows

- [distribute-to-firebase](https://github.com/samuelgustin/maui-workflows/blob/main/.github/workflows/distribute-to-firebase.md)
- [maui-build-android](https://github.com/samuelgustin/maui-workflows/blob/main/.github/workflows/maui-build-android.md)
- [maui-build-ios](https://github.com/samuelgustin/maui-workflows/blob/main/.github/workflows/maui-build-ios.md)

## Actions

- [dotnet-for-maui](https://github.com/samuelgustin/maui-workflows/tree/main/.github/actions/dotnet/setup-maui)
- [xcode/setup](https://github.com/samuelgustin/maui-workflows/tree/main/.github/actions/xcode/setup)
- [xcode/cleanup](https://github.com/samuelgustin/maui-workflows/tree/main/.github/actions/xcode/cleanup)

## Examples

### **_Please read the README's from each workflow to make sure you fully configure everything in your project/repository_**

### MAUI iOS to Firebase

```yaml
name: Build & Deploy MAUI iOS

on:
  workflow_dispatch:

env:
  DOTNET_NOLOGO: true # Disable the .NET logo
  DOTNET_SKIP_FIRST_TIME_EXPERIENCE: true # Disable the .NET first time experience
  DOTNET_CLI_TELEMETRY_OPTOUT: true # Disable sending .NET CLI telemetry

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

  deploy-ios-to-firebase:
    uses: samuelgustin/maui-workflows/.github/workflows/distribute-to-firebase.yml@main
    needs: maui-buil-ios
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
```

### MAUI Android to Firebase

```yaml
name: Build & Deploy MAUI Android

on:
  workflow_dispatch:

env:
  DOTNET_NOLOGO: true # Disable the .NET logo
  DOTNET_SKIP_FIRST_TIME_EXPERIENCE: true # Disable the .NET first time experience
  DOTNET_CLI_TELEMETRY_OPTOUT: true # Disable sending .NET CLI telemetry

jobs:
  maui-build-android:
    uses: samuelgustin/maui-workflows/.github/workflows/maui-build-android.yml@main
    with:
      dotnet-version: "9.0.x"
      dotnet-publish-version: "9.0"
      java-version: "17"
      project-directory: "my-path/"
      project-name: "my_app"
      apk-filename: "my_app.apk"
      appsettings-filename: "appsettings.json"
      production-branch-name: "main"
      staging-branch-pattern: "release/"
    secrets:
      ANDROID_KEYSTORE_ALIAS: ${{ secrets.ANDROID_KEYSTORE_ALIAS }}
      ANDROID_KEYSTORE_PASSWORD: ${{ secrets.ANDROID_KEYSTORE_PASSWORD }}
      ANDROID_KEYSTORE_BASE64: ${{ secrets.ANDROID_KEYSTORE_BASE64 }}
      APPSETTINGS_BASE64: ${{ secrets.APPSETTINGS_BASE64 }}

  deploy-android-to-firebase:
    uses: samuelgustin/maui-workflows/.github/workflows/distribute-to-firebase.yml@main
    needs: maui-build-android
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
