# C++/scripting mods

So far we've been working with Lua, which is a brilliant way to get started because it lets you experiment quickly and reload your code while the game is running. C++ mods are the next step up. They still run through UE4SS, but instead of being interpreted Lua scripts, they are compiled DLLs that UE4SS loads into the game process.

That makes them more powerful, but also a wee bit more complex and less forgiving! A Lua mistake usually means a script error in the UE4SS console. A C++ mistake can crash the whole game. Don't let that put you off, though!

!!! warning "Work in progress!"

    This section is an early draft while Subnautica 2 modding is still settling down. The exact UE4SS C++ build files, generated headers, and game SDK details may change as UE4SS and the game are updated.

## What we're going to build

For this first C++ mod, we'll keep things simple to begin with. We will:

1. Create a UE4SS C++ mod project.
2. Build a DLL.
3. Install the DLL into the game's UE4SS `Mods` folder.
4. Confirm that UE4SS loads the mod and prints a message to the console.

That might sound boring and you'd be right to think so! This is essentially the C++ equivalent of getting a Lua `print` statement working. However, once you can reliably build, load, and debug a compiled mod, you have the foundation for doing more interesting things.

## Why use C++?

C++ mods are useful when you need something that Lua cannot comfortably do, or when you want tighter integration with Unreal's native systems.

Some common reasons to use C++ are:

- You want better performance than a Lua script can provide.
- You need to work with lower-level Unreal Engine types and functions.
- You want to use the UE4SS [C++ API](https://docs.ue4ss.com/cpp-api.html) instead of the [Lua API](https://docs.ue4ss.com/lua-api.html).

For many gameplay tweaks, Lua is still the friendlier choice. If your mod is just changing a few values or reacting to a small number of events, Lua may be all you need.

