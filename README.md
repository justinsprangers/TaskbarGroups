# TaskbarGroups

My copy of [TaskBar Groups by Pike](https://github.com/PikeNote/taskbar-groups-pike-beta) (v0.2.2.0, Jan 2023), kept here so I can make my own changes. It pins a single taskbar icon that pops up a small tray of apps.

The original project's README is in `README.upstream.md`. MIT License (see `LICENSE`).

## Just using it
I don't need to build anything to use it. Download `Taskbar.Groups.exe` from the [releases page](https://github.com/PikeNote/taskbar-groups-pike-beta/releases), put it in a folder that stays put, run it, create a group, then right-click the group's shortcut → **Pin to taskbar**.

## My changes
- **Import a Start menu folder.** In the group editor, click **"or import a Start menu folder"** under *Add new shortcut*. Pick a folder, and every app in it is added to the group. If the group has no name yet, it's named after the folder.
  - This works with folders in the Start menu's **All apps** list. Those are real folders in `%APPDATA%\Microsoft\Windows\Start Menu\Programs` and `C:\ProgramData\Microsoft\Windows\Start Menu\Programs`, and both appear in the picker's sidebar. It doesn't work with folders made in the **Pinned** section, because Windows doesn't store those as real folders.
  - To make a custom group managed from the Start menu, create a folder under `%APPDATA%\Microsoft\Windows\Start Menu\Programs` and put app shortcuts in it.
  - Uninstall shortcuts are skipped, and the group's 20-app limit still applies.

## Building my own version
1. Install **Visual Studio Community** (free) with the **.NET desktop development** workload. That includes .NET Framework 4.7.2.
2. Open `TaskbarGroups.sln`. If references show as missing, right-click the solution → **Restore NuGet Packages**.
3. Set the configuration to **Release**, then **Build → Build Solution**.
4. The app is `main\bin\Release\Taskbar Groups.exe`. The pop-up helper builds into `main\Resources\` and is packed inside that exe. Build output is gitignored.
   - Command-line alternative: `"C:\Program Files\Microsoft Visual Studio\18\Community\MSBuild\Current\Bin\MSBuild.exe" TaskbarGroups.sln /restore /p:Configuration=Release /p:RestorePackagesConfig=true`
   - If the build complains about "mark of the web", unblock the files once in PowerShell: `Get-ChildItem C:\Claude\TaskbarGroups -Recurse -File | Unblock-File`

## Layout
- `main/`: the editor where you create and edit groups (`client.csproj`).
- `backgroundClient/`: the process that shows the pop-up tray when a pinned group is clicked.
- `*/lib/`: bundled third-party DLLs the projects reference.
