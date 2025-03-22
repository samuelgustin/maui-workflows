# xcode/setup Action

This action sets up Xcode for your runner to build iOS applications.

## Inputs

#### Required

- `provisioning-profile-base64`: Base64 encoded string of the iOS provisioning profile to be used
- `signing-certificate-base64`: Base64 encoded string of the iOS signing certificate to be used
- `signing-certificate-password`: The password for the iOS signing certificate
- `keychain-password`: The password for the iOS keychain (can be any value, just needs to be provided)

#### Optional

- `xcode-version`: The version of Xcode to install

## Repository Secrets

I suggest that you add the following secrets to your repository (or github organization).

- `SIGNING_CERTIFICATE_BASE64`
- `SIGNING_CERTIFICATE_PASSWORD`
- `PROVISIONING_PROFILE_BASE64`
- `KEYCHAIN_PASSWORD`

Use the following commands to get the base64 string of the files from their local directory:

```powershell
base64 -i BUILD_CERTIFICATE.p12 | pbcopy
base64 -i PROVISIONING_PROFILE.mobileprovision | pbcopy
```

The SIGNING_CERTIFICATE_PASSWORD is created by you when you export it.

## Usage

```yaml
- name: Xcode Setup for iOS Builds
  uses: samuelgustin/maui-workflows/.github/actions/xcode/setup@main
  with:
    xcode-version: ${{ inputs.xcode-version }}
    signing-certificate-base64: ${{ secrets.SIGNING_CERTIFICATE_BASE64 }}
    signing-certificate-password: ${{ secrets.SIGNING_CERTIFICATE_PASSWORD }}
    provisioning-profile-base64: ${{ secrets.PROVISIONING_PROFILE_BASE64 }}
    keychain-password: ${{ secrets.KEYCHAIN_PASSWORD }}
```

## References

- [setup-xcode Action](https://github.com/maxim-lobanov/setup-xcode)
- [Installing an Apple certificate on macOS runners for Xcode development](https://docs.github.com/en/actions/use-cases-and-examples/deploying/installing-an-apple-certificate-on-macos-runners-for-xcode-development)
