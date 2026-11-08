# Squadronista (TC / Dalamud API 13)

This branch targets the Traditional Chinese (Taiwan) client through [yanmucorp/Dalamud](https://github.com/yanmucorp/Dalamud) API 13 and .NET 9, for use with [cycleapple/XIVTCLauncher](https://github.com/cycleapple/XIVTCLauncher).

Upstream Squadronista tracks official Dalamud API 14 / .NET 10. The only intended change here is project targeting. Squadron UI reading still uses the upstream `FFXIVClientStructs` layout and has not been adjusted for the TC client.

## Build

`Dalamud.NET.Sdk` 13.1.0 does not ship Dalamud. It compiles against the assemblies in `DALAMUD_HOME`, or, if that is unset, the official launcher dev folder (`%AppData%\XIVLauncher\addon\Hooks\dev\` on Windows). XIVTCLauncher stores yanmucorp Dalamud under `%AppData%\FFXIVSimpleLauncher\Dalamud\` (the `Injector` directory is the one that contains `Dalamud.Injector.exe` and `Dalamud.dll`).

1. Install the .NET 9 SDK. `global.json` asks for `9.0.100` and rolls forward inside the 9.0 feature band, which covers the 9.0.3xx SDK that matches Dalamud's .NET 9.0.3 runtime.
2. Set `DALAMUD_HOME` to the directory that contains `Dalamud.dll` from yanmucorp Dalamud `13.0.0.16` (a local build, a [release archive](https://github.com/yanmucorp/Dalamud/releases), or the launcher's downloaded copy).
3. From this directory, run `dotnet build -c Release`.

On Linux or macOS, pass `-p:EnableWindowsTargeting=true` as well. The project targets `net9.0-windows7.0`.

`lib/LLib.dll` (assembly 6.4.0.0) is the existing checked-in .NET 9 binary. It references Dalamud 13.0.0.2 and `FFXIVClientStructs` 0.0.6402.0. A current yanmucorp Dalamud 13.0.0.16 install ships `FFXIVClientStructs` 0.0.6966.0. The reference still compiles, so this branch leaves the DLL in place. If a later compiler error says an LLib type no longer matches, replace `lib/LLib.dll` with a build from the same yanmucorp tree. Do not treat a successful compile as proof that LLib's UI helpers match the TC client.

Loading the built plugin is a separate step. Copy the Release output into an XIVTCLauncher dev-plugin folder and confirm squadron mission text in game. `FFXIVClientStructs` offsets are not validated by this retarget.
