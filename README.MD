# Instruction to set up win10 build enviroment

1) Install Msys2 https://www.msys2.org/ into default directory on C:/ disc.

	In msys2 console run:
	
	$ pacman -Syu
	repeat it several times for fully update.
	Also try
	$ pacman -Su
	
	pacman -S --needed base-devel mingw-w64-x86_64-toolchain

    Add msys2 into system path:
    Run search and start typing "envi". Search must suggest open "Edit the System enviroment variables".
    In open window at "Advanced" tab, click on "Environment Variables..." it must open enviroment variables window.
    In Environment Variables window, at lower area named as System variables, find and select "Path", then click edit.
    Must open another window in which you must click "New" button and past the path to binarys. In common ways it must be C:\msys64\mingw64\bin.
    Reopen any terminals that was opend before adding and check that binary is visible, by commands like:
        $ gcc --version
        $ g++ --version
        $ gdb --version
		
2) Install git for windows https://git-scm.com/
	While install use default options.

3) install CMake https://cmake.org/download/
	While installing additionaly choose to add Cmake into system path.
	
	Check that CMake is visible, open CMD and type:
		$ cmake --version
	
3) Install VSCode https://code.visualstudio.com/

    Install c++ extensions pack (default by microsoft).

    Make setting.json with content

    ~~~
    {
        "cmake.mingwSearchDirs": [
            "C:\\mingw-w64\\mingw64\\bin"
        ],
        "cmake.generator": "MinGW Makefiles"
    }
    ~~~

    Configure run (debug) task for gdb/lldb. Modify the content, for example:

    ~~~
    {
        // Use IntelliSense to learn about possible attributes.
        // Hover to view descriptions of existing attributes.
        // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
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
    ~~~