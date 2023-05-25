# Instruction to set up windows 10 build enviroment

---

1. Install Msys2 https://www.msys2.org/ into default directory on C:/ disc,
in this example is used default C:\msys64\ path to install.

After installation open MSYS2 MSYS console (in purple collor) instead UCRT console
(the UCRT console  is openning by default after instalation, just close it).
MSYS console will be using gcc compiler. UCRT console using some compiler for compability with Visual Studio,
but we will be using VS Code and it's has some troubles for using UCRT (it's possible to resolve I think).

In MSYS2 MSYS console run:

```sh
pacman -Syu
```
Repeat it command several times for fully update.
Also try

```sh
pacman -Su
```

---

Install the binaryes (with  gcc compiler environment):

```sh
pacman -S --needed base-devel mingw-w64-x86_64-toolchain

```

---

Add msys2 into system path:
- Run search and start typing "envi". Search must suggest open "Edit the System enviroment variables".
- In open window at "Advanced" tab, click on "Environment Variables..." it must open enviroment variables window.
- In Environment Variables window, at lower area named as System variables, find and select "Path", then click edit.
- Must was opened another window in which you must click "New" button and past the path to binarys. In common ways it must be C:\msys64\mingw64\bin.
- Reopen any terminals that was opend before adding and check that binary is visible, by commands like:

```sh
$ gcc --version
$ g++ --version
$ gdb --version
```

---

2. Install git for windows https://git-scm.com/
While install use default options.

---

3. install CMake https://cmake.org/download/
While installing additionaly choose to add Cmake into system path.
I recomend to set the CMake path above at the path to MSYS2 package.
	
Check that CMake is visible, open CMD and type:
		```sh cmake --version ```

---

4. Install VSCode https://code.visualstudio.com/

Install c++ extensions pack (default by microsoft).

Make setting.json with content:

```json
{
    "editor.rulers": [
        100
    ],
    "editor.minimap.enabled": false,
    "cmake.sourceDirectory": "${workspaceFolder}",
    "cmake.additionalCompilerSearchDirs": [
        "C:\\msys64\\mingw64\\bin"
    ],
    "cmake.generator": "MinGW Makefiles"
}
```

Configure run (debug) task for gdb/lldb. Modify the content, for example:

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "(gdb) Launch",
            "type": "cppdbg",
            "request": "launch",
            "program": "${workspaceFolder}/build/test.exe",
            "args": [],
            "stopAtEntry": false,
            "cwd": "${fileDirname}",
            "environment": [],
            "externalConsole": false,
            "MIMode": "gdb",
            "miDebuggerPath": "C:/msys64/mingw64/bin/gdb.exe",
            "setupCommands": [
                {
                    "description": "Enable pretty-printing for gdb",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                },
                {
                    "description": "Set Disassembly Flavor to Intel",
                    "text": "-gdb-set disassembly-flavor intel",
                    "ignoreFailures": true
                }
            ]
        }

    ]
}
```

---

ITS THE END OF ALL WHAT YOU NEED TO BUILDING UNDER THE WINDOWS.
BELOW IS THE ADDITIONAL INFO.

---

[Link to source](https://github.com/libjxl/libjxl/blob/main/doc/developing_in_windows_msys.md?ysclid=li2zalvsvq373528469)

## Developing for Windows with MSYS2

[MSYS2](https://www.msys2.org/) ("minimal system 2") is a software distribution and a development platform based on MinGW and Cygwin.  It provides a  Unix-like environment to build code on Windows.  These instructions were written with a 64-bit instance of Windows 10 running on a VM.  They may also work on native instances of Windows and other versions of Windows.

### Build Environments

MSYS2 provides multiple development [environments](https://www.msys2.org/docs/environments/).  By convention, they are referred to in uppercase.  They target slightly different platforms, runtime libraries, and compiler toolchains.  For example, to build for 32-bit Windows, use the MINGW32 environment.  For interoperability with Visual Studio projects, use the UCRT64 environment.

Since all of the build environments are built on top of the MSYS environment, **all updates and package installation must be done from within the MSYS environment**.  After making any package changes, `exit` all MSYS2 terminals and restart the desired build-environment.  This reminder is repeated multiple times throughout this guide.

* **MINGW32:**  To compile for 32-bit Windows (on 64-bit Windows), use packages from the `mingw32` group.  Package names are prefixed with `mingw-w64-i686`.  The naming scheme may be different on the 32-bit version of MSYS2.

* **MINGW64:**  This is the primary environment to building for 64-bit Windows.  It uses the older MSVCRT runtime, which is widely available across Windows systems.  Package names are prefixed with `mingw-w64-x86_64`.

* **UCRT64:**  The Universal C Runtime (UCRT) is used by recent versions of Microsoft Visual Studio.  It ships by default with Windows 10.  For older versions of Windows, it must be provided with the application or installed by the user.  Package names are prefixed with `mingw-w64-ucrt-x86_64`.

* **CLANG64:** Unfortunately, the `gimp` packages are not available for the CLANG64 environment.  However, `libjxl` will otherwise build in this environment if the appropriate packages are installed.  Packages are prefixed with `mingw-w64-clang-x86_64`.

### Install and Upgrade MSYS2

Download MSYS2 from the homepage.  Install at a location without any spaces on a drive with ample free space.

Toward the end of installation, select the option to run MSYS2 now.  A command-line window will open.  Run the following command, and answer the prompts to update the repository and close the terminal.

```bash
pacman -Syu
```

Now restart the MSYS environment and run the following command to complete updates:

```bash
pacman -Su
```

### Package Management

Packages are organized in groups, which share the build environment name, but in lower case.  Then they have name prefixes that indicate which group they belong to.  Consider this package search: `pacman -Ss cmake`

```
mingw32/mingw-w64-i686-cmake
mingw64/mingw-w64-x86_64-cmake
ucrt64/mingw-w64-ucrt-x86_64-cmake
clang64/mingw-w64-clang-x86_64-cmake
msys/cmake
```

We can see the organization `group/prefix-name`.  When installing packages, the group name is optional.

```bash
pacman -S mingw-w64-x86_64-cmake
```