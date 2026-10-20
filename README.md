# TaskbarGroups

My copy of [TaskBar Groups by Pike](https://github.com/PikeNote/taskbar-groups-pike-beta) (v0.2.2.0, Jan 2023), kept here so I can make my own changes. It pins a single taskbar icon that pops up a small tray of apps.

The original project's README is in `README.upstream.md`. MIT License (see `LICENSE`).

## Just using it
I don't need to build anything to use it. Download `Taskbar.Groups.exe` from the [releases page](https://github.com/PikeNote/taskbar-groups-pike-beta/releases), put it in a folder that stays put, run it, create a group, then right-click the group's shortcut → **Pin to taskbar**.

## Building my own version
1. Install **Visual Studio Community** (free) with the **.NET desktop development** workload. That includes .NET Framework 4.7.2.
2. Open `TaskbarGroups.sln`.
3. Set the configuration to **Release**, then **Build → Build Solution**.
4. The output goes to `main\bin\Release\` (the editor app) and `backgroundClient\bin\Release\` (the pop-up). Build output is gitignored.

## Layout
- `main/`: the editor where you create and edit groups (`client.csproj`).
- `backgroundClient/`: the process that shows the pop-up tray when a pinned group is clicked.
- `*/lib/`: bundled third-party DLLs the projects reference.
