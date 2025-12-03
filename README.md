<a id="english"></a>

This is a NuGet package with built-in MSBuild targets, designed to help .NET developers cross-compile [`Native AOT`](https://learn.microsoft.com/en-us/dotnet/core/deploying/native-aot/) projects from a Linux environment to the Windows platform. It resolves issues with missing linkers and SDKs encountered when directly publishing to a Windows target from Linux.

This project utilizes [`LLVM/lld`](https://lld.llvm.org) and [`msvc-wine`](https://github.com/mstorsjo/msvc-wine) to provide the necessary linker and Windows SDK, enabling cross-compilation from `linux` to `win`.

### Prerequisites

Before you begin, ensure that the following dependencies are installed in your environment:

1.  **[`LLVM/lld`](https://lld.llvm.org)**: Make sure the `lld-link` command is available.
2.  **[`msvc-wine`](https://github.com/mstorsjo/msvc-wine)**: Provides the Windows SDK and environment variables required for cross-compilation.

#### Installing Dependencies on Arch Linux (Example)

If you are using Arch Linux, you can easily install them via an AUR helper (e.g., `yay` or `paru`):

```sh
# Using yay
yay -S lld msvc-wine-git

# Or using paru
paru -S lld msvc-wine-git
```

For other Linux distributions, please use your package manager to install `lld` and follow the [msvc-wine guide](https://github.com/mstorsjo/msvc-wine) to install or download it.

### Usage Steps

1.  **Add the NuGet package reference to your project**.

    ```sh
    dotnet add package PublishAotCross.LinuxToWin
    ```

2.  **Optional: Configure the msvc-wine path**.

    This package defaults to searching for `msvc-wine` in the `/opt/msvc` path. If your installation path is different, please add the `MSVCWineBinPath` property in your `.csproj` project file to specify the correct `bin` directory path.

    ```xml
    <PropertyGroup>
      <!-- Replace this path with your actual installation path -->
      <MSVCWineBinPath>/path/to/your/msvc/bin</MSVCWineBinPath>
    </PropertyGroup>
    ```

3.  **Publish your Native AOT application**.

    Execute the `dotnet publish` command, specifying the Windows platform RID (`win-x64`).

    ```sh
    dotnet publish -r win-x64 -c Release
    ```
