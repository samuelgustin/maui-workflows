# xcode/cleanup Action

This action cleans up Xcode form your self hosted runner.

## Usage

```yaml
- name: Clean up keychain and provisioning profile for self hosted runners
    if: ${{ !contains(runner.name, 'Github Actions') }}
    uses: samuelgustin/maui-workflows/.github/actions/xcode/cleanup@main
```

## References

[Required clean-up on self-hosted runners](https://docs.github.com/en/actions/use-cases-and-examples/deploying/installing-an-apple-certificate-on-macos-runners-for-xcode-development#required-clean-up-on-self-hosted-runners)
