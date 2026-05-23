# Creating a Lua mod

A UE4SS Lua mod is simply a `main.lua` script that runs inside the game process. UE4SS exposes a set of global functions that let you find game objects, read and write their properties, and respond to game events. Key patterns and functions that you'll see used often in LUA mods are:

| Function Name                                                                                                     | Description                                                                                                                                                                                                                                      |
|-------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [`FindAllOf(className)`](https://docs.ue4ss.com/dev/lua-api/global-functions/findallof.html)                      | Searches for all live instances of a given class, returning a table you can iterate over. This is how we'll locate the player's survival attribute set.                                                                                          |
| [`FindFirstOf(className)`](https://docs.ue4ss.com/dev/lua-api/global-functions/findfirstof.html)                  | Searches for and returns the first instance of the given class. Does not return static objects.                                                                                                                                                  |
| [`NotifyOnNewObject(path, callback)`](https://docs.ue4ss.com/dev/lua-api/global-functions/notifyonnewobject.html) | Fires a callback whenever a new instance of the specified class is created. We use this to detect when the player character spawns or respawns, so we can acquire a fresh reference to the attribute set at the right moment.                    |
| [`LoopAsync(intervalMs, callback)`](https://docs.ue4ss.com/dev/lua-api/global-functions/loopasync.html)           | Runs a function repeatedly on a background thread at a given interval. Because game objects may not exist the moment your mod loads, polling is a common and reliable way to find and persist a reference to something.                          |
| [`ExecuteInGameThread(callback)`](https://docs.ue4ss.com/dev/lua-api/global-functions/executeingamethread.html)   | Runs code on the main game thread. This is important because directly modifying various properties from a background thread is unsafe - UE4SS requires you to wrap those operations here.                                                        |
| [`StaticFindObject(path)`](https://docs.ue4ss.com/dev/lua-api/global-functions/staticfindobject.html)             | Finds and returns a static (non-instance) object in memory that inherits from UObject, such as a UClass or UFunction. Essential for calling static functions.                                                                                    |
| [`RegisterHook(function, callback)`](https://docs.ue4ss.com/dev/lua-api/global-functions/registerhook.html)       | Registers a hook using the full name of a function. The callback will be called when the function is executed, providing the given context and any desired parameters. This is analagous to Harmony prefix and postfix patches in Unity modding. |

With those building blocks in mind, the structure of our mod should start to make sense.

## Mod set up

We create our mod directly in the UE4SS mod folder so that we can build and test it quickly. Later on, we can move it into our development folder and get it all checked in to GitHub.

So, for now:

1. Start by creating a folder in `\Subnautica2\Subnautica2\Binaries\Win64\ue4ss\Mods`, call it `Subnautica2CheatMod`.
2. Within that folder, create an empty file called `enabled.txt` - this tells UE4SS to load and activate our mod.
3. Within this folder create a folder called `scripts`.

4. Within that folder, create a file called `main.lua`.

5. Open this file in a VS code.

