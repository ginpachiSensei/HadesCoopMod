# Hades coop mod (community fork)

Play Hades with a friend! Two players fight through the Underworld together in one game,
each with their own hero, boons and controller.

This is a community fork of [TheNormalnij/HadesCoopMod](https://github.com/TheNormalnij/HadesCoopMod),
based on its latest code (v0.4.3 plus the keepsake crash fix). The original mod does all the heavy
lifting; this fork adds fixes for problems that came up in play and tests them in the real game.

## What this fork fixes

- **Softlock when player 1 dies with a reward on the floor.** Each player gets their own reward,
  handed out one after the other. If player 1 died while their reward was waiting (for example the
  first chamber's boon), nobody could pick it up, player 2's reward never appeared and the exits never
  opened. Now a dead player's waiting reward is removed and the next player's reward spawns.
- **Fights skipped after player 1 dies.** A room whose fight had not started yet ran it as the dead
  player 1: no enemies spawned and the room counted as cleared. The fight now starts for the living
  player, and either player can walk in to start it.
- **Mod files saved with Windows line endings did not load** (in the bundled mod loader). If you edited
  `config.lua` with an editor that saves CRLF line endings, the change was silently ignored. The release
  ships a loader built with the fix; the change is in [patches/](patches/).

## Install

1. Back up your saves: `Documents\Saved Games\Hades`.
2. Download `HadesCoop-<version>.zip` from [Releases](../../releases).
3. Open the game folder (Steam: right-click Hades > Manage > Browse local files).
4. Copy the zip's `x64` folder into `Hades/x64` (or `Hades/x64Vk` if you play with Vulkan) and its
   `Content/Mods` folder into `Hades/Content/Mods`. Replace files when asked.

To uninstall, delete `Content/Mods/TN_CoopMod`, `Content/Mods/TN_MainMenuApi`, and
`x64/HadesModCore.dll` and `x64/Xinput9_1_0.dll`.

## Play

1. Start Hades and choose **Select gamemode > Coop**.
2. Press a button on player 1's controller, then on player 2's. Only player 1 may use keyboard and mouse;
   player 2 needs a gamepad.
3. Pick your save and play. Co-op starts in the courtyard; player 2 joins there.

### Playing with a friend who is not in the room

There is no online mode: the game runs on the host's PC and the friend streams it.

- **Steam Remote Play Together** with [RemotePlayWhatever](https://github.com/m4dEngi/RemotePlayWhatever)
  (Hades does not offer Remote Play Together by itself). The host starts Hades, then RemotePlayWhatever,
  and invites a Steam friend. The friend does not need to own Hades.
- **[Parsec](https://parsec.app/)** works the same way.

The friend must use a **gamepad**: their keyboard would control player 1.

## Tested

Steam version, build 10929685, Windows 11, with an automated test suite that drives the real game with
virtual controllers:

- co-op starts with two heroes; each controller moves only its own hero;
- both players take their first-chamber boons, the fight spawns, the exits open;
- player 1 dies before taking their boon: only player 2's boon is offered, the fight spawns, the exits open
  (the same test stays locked on v0.4.3);
- player 1 dead, player 2 plays on alone for 10 rooms (boons, Chaos, Daedalus Hammer, Centaur Heart,
  Sisyphus, mini-bosses, shop);
- quit mid-run with player 1 dead and load again: same room, same boons for both players, player 2 still
  controllable.

Not tested yet: boss fights, the end of a run, the Epic Games Store and Vulkan versions.

# Build

## Using CMake for Windows x64

```powershell
cmake -A x64 . -B build_msvc
cmake --build build_msvc --config Release
```

Binary files are located in the `bin` folder.

## Using [Visual Studio](https://visualstudio.microsoft.com/) GUI

You need to install cmake in the Visual Studio Installer to build the project.
Open the project in VS and click Build -> Build All in the top menu.
Check `bin` folder for `HadesCoopGame.dll`.

# How to load the mod from the repository folder

Follow these steps if you want to develop the mod and save changes in git:

1. Create the folder `Hades/Hades/Content/ModModules/TN_CoopMod/`
2. Copy the repository into `Hades/Hades/Content/ModModules/TN_CoopMod/dev`
3. Create `Hades/Hades/Content/ModModules/TN_CoopMod/init.lua` with the following content:
```lua
ModRequire "dev/game/scripts/init.lua"
```
4. Create `Hades/Hades/Content/ModModules/TN_CoopMod/meta.sjson` with the following content:
```sjson
{
	Name = "Coop mod"
	Library = "dev/bin/HadesCoopGame.dll"
	Author = "Uladzislau 'TheNormalnij' Nikalayevich"
}
```
5. Now you can make changes directly in the repository and load them in the game.

# Credits and license

- Uladzislau "TheNormalnij" Nikalayevich: the original [HadesCoopMod](https://github.com/TheNormalnij/HadesCoopMod),
  [HadesModLoader](https://github.com/TheNormalnij/HadesModLoader) and
  [HadesMainMenuAPI](https://github.com/TheNormalnij/HadesMainMenuAPI). You can support his work on
  [his page](https://thenormalnij.de/donate).
- Fork fixes and tests: [ginpachiSensei](https://github.com/ginpachiSensei).

MIT license, see [LICENSE.txt](LICENSE.txt). Hades belongs to Supergiant Games; you need your own copy.
