# dotnet-for-maui Action

This action sets up .NET for your runner and installs the MAUI workload.

## Inputs

#### Required

- `dotnet-version`: Version of .NET to use

#### Optional

- `additional-restore-args`: Additional arguments to pass to `dotnet restore`
- `maui-project-directory`: MAUI Project directory

## Usage

```yaml
- name: .NET Setup for MAUI Android
  uses: samuelgustin/maui-workflows/.github/actions/dotnet/setup-maui@main
  with:
    dotnet-version: 9.0.x
    additional-restore-args: "src/MauiApp.csproj"
    maui-project-directory: "src/"

- name: .NET Setup for MAUI iOS
  uses: samuelgustin/maui-workflows/.github/actions/dotnet-for-maui@main
  with:
    dotnet-version: 9.0.x
    additional-restore-args: "-r ios-arm64 src/MauiApp.csproj"
    maui-project-directory: "src/"
```

## References

[Building and testing .NET](https://docs.github.com/en/actions/use-cases-and-examples/building-and-testing/building-and-testing-net)
