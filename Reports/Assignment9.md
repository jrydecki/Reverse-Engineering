# Assignment 9

## How it Works
My Anti-Virus works by looking for predetermined files in certain locations, along with looking for the njRAT-spawned processes.

### Scan
When the user selects `Scan`, my A/V first checks if any njRAT files are found including:
- `C:\njq8.exe`
- `C:\njRAT.exe`
- `%TEMP%\windows.exe`
- `%TEMP%\windows.tmp`
- `%USERPROFILE%\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup\ecc7c8c51c0850c1ec247c7fd3602f20.exe`

By specifing the environment variables (`%TEMP%` and `%USERPROFILE%`), this program is able to look for our version of njRAT on any Windows account, no matter their username. The scanner then looks through all the running processes on the computer and checks for a process running called `windows.exe`. By default, there should be no process called `windows.exe` and this is the name that our version of njRAT gives itself when running.

Any files found or process found is then remembered by the program and displays this information to the user.

### Remove
When the user selects `Remove`, first, the A/V gets that process that it detected during the scan phase (saved as a C# Process object in my case) and kills that process.

Next, the A/V program loops through all detected njRAT files and deletes them. Specifically, it removes the `ReadOnly` flag on the current file (if it is set, like on the `njq8.exe` file) , then deletes the file. 

After the process is stopped, and all these files are removed, njRAT should be removed.

## Anti-Virus Code
- Source Code Download: [SourceCode.zip](../Source/Assignment9/SourceCode.zip)
- Source Code Folder: [https://github.com/jrydecki/Reverse-Engineering/tree/main/Source/Assignment9](https://github.com/jrydecki/Reverse-Engineering/tree/main/Source/Assignment9)
- Executable Download: [Assignment9.exe.zip](../Source/Assignment9/Assignment9.exe.zip)

## Anti-Virus Demo
![njRAT Anti-Virus Demo](src/9-JacobRydecki-Antivirus.mp4)
