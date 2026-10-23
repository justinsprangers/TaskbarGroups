# TaskbarGroups

A modified version of TaskBar Groups. It pins a single taskbar icon that pops up a small tray of apps.

## Credit
This is a fork of [TaskBar Groups by Pike](https://github.com/PikeNote/taskbar-groups-pike-beta) (PikeNote), starting from v0.2.2.0 (Jan 2023). That project is itself a continuation of the original [taskbar-groups](https://github.com/tjackenpacken/taskbar-groups) by Jack Schierbeck (tjackenpacken). All the core functionality is theirs. The changes listed below are mine.

The first commit in this repo is the unmodified upstream source. Diff against it to see exactly what I changed. The upstream README is kept in `README.upstream.md`. MIT License (see `LICENSE`), with the original copyright notice kept.

## Just using it
There's no prebuilt release of this fork yet, so build it yourself (see below). To use the original unmodified app instead, download `Taskbar.Groups.exe` from [Pike's releases page](https://github.com/PikeNote/taskbar-groups-pike-beta/releases). Either way: put the exe in a folder that stays put, run it, create a group, then right-click the group's shortcut → **Pin to taskbar**.

## My changes
- **Import a Start menu folder.** In the group editor, click **"or import a Start menu folder"** under *Add new shortcut*. Pick a folder, and every app in it is added to the group. If the group has no name yet, it's named after the folder.
  - This works with folders in the Start menu's **All apps** list. Those are real folders in `%APPDATA%\Microsoft\Windows\Start Menu\Programs` and `C:\ProgramData\Microsoft\Windows\Start Menu\Programs`, and both appear in the picker's sidebar. It doesn't work with folders made in the **Pinned** section, because Windows doesn't store those as real folders.
  - To make a custom group managed from the Start menu, create a folder under `%APPDATA%\Microsoft\Windows\Start Menu\Programs` and put app shortcuts in it.
  - Uninstall shortcuts are skipped, and the group's 20-app limit still applies.
  - Store apps and installed websites (which have no `.exe`) work too. For a folder of those, make shortcuts that point to `shell:AppsFolder\<AppID>`. In PowerShell, `Get-StartApps` lists the app IDs.
- **Fixed: crash when adding shortcuts on scaled displays** (Windows display scaling above 100%).
- **Fixed: tiny icons for installed websites.** Store apps and installed websites now use their large Start menu icon instead of the padded Store logo. To pick up a fix like this for an existing group, delete the group and import it again, because the app keeps a saved copy of each icon.

## Building my own version
1. Install **Visual Studio Community** (free) with the **.NET desktop development** workload. That includes .NET Framework 4.7.2.
2. Open `TaskbarGroups.sln`. If references show as missing, right-click the solution → **Restore NuGet Packages**.
3. Set the configuration to **Release**, then **Build → Build Solution**.
4. The app is `main\bin\Release\Taskbar Groups.exe`. The pop-up helper builds into `main\Resources\` and is packed inside that exe. Build output is gitignored.
   - Command-line alternative: `"C:\Program Files\Microsoft Visual Studio\18\Community\MSBuild\Current\Bin\MSBuild.exe" TaskbarGroups.sln /restore /p:Configuration=Release /p:RestorePackagesConfig=true`
   - If the build complains about "mark of the web", unblock the files once in PowerShell: `Get-ChildItem <path to this folder> -Recurse -File | Unblock-File`

## Layout
- `main/`: the editor where you create and edit groups (`client.csproj`).
- `backgroundClient/`: the process that shows the pop-up tray when a pinned group is clicked.
- `*/lib/`: bundled third-party DLLs the projects reference.
