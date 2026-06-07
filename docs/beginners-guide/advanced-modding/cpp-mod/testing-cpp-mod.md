# Install and test our C++ mod

Deploying C++ mods is a little different to Lua mods, so let's look at that in a little more detail.

## Lua mod vs C++ mod structure

The Lua mod we created earlier looked something like this:

```text
📁Mods
└── 📁BeginnersGuideCheatMod
    └── 📁scripts
        ├── main.lua
    ├── enabled.txt
```

A UE4SS C++ mod is packaged differently. Instead of a `main.lua` script, UE4SS loads a compiled DLL from a `dlls` folder:

```text
📁Mods
└── 📁BeginnersGuideCppMod
    └── 📁dlls
        ├── main.dll
└── mods.txt
```

The important difference here is that UE4SS is no longer running your source file directly. You edit C++ source code in Visual Studio, build it, then deploy the compiled DLL into the mod folder. UE4SS expects the installed DLL to be called `main.dll`, even if your build output originally has a different name.

## Install and enable the mod

If you configured the `CMakeLists.txt` file correctly, including the `add_custom_command(TARGET ${TARGET} POST_BUILD` code, having built your mod DLL the build pipeline should also have created a folder structure in your game mods folder, and deployed your new mod DLL there as `main.dll`. Open Windows Explorer and check that this is the case. Your game folder should look something like this:

```text
📁Subnautica2
└── 📁Subnautica2
    └── 📁Binaries
        └── 📁Win64
            └── 📁ue4ss
                └── 📁Mods
                    └── 📁BeginnersGuideCppMod		<- same as Lua mods up to here
                        └── 📁dlls					<- folder created to hold your mod dll
                            └── main.dll			<- copied and renamed from your built dll
└── mods.txt
```

Open `mods.txt` in the UE4SS `Mods` folder and add your mod above the built-in keybinds section:

```text
BeginnersGuideCppCheatMod : 1
```

You may remember that for our Lua mod, we created an  `enabled.txt` file instead. Adding into `mods.txt` is another way to tell UE4SS what mods to load. It's useful to know about this, as not only does this give you explicit control over whether a mod is enabled, it also allows you to explicitly configure where it sits in the load order. This can be very important if you have lots of mods that have overlapping functionality.

## Test the mod

Launch the game and watch the UE4SS console. If everything is working, you should see your mod being discovered and loaded, followed by your own log message:

```text
[15:22:53.7375544] Starting C++ mod 'BeginnersGuideCppCheatMod'
[15:22:53.7376488] [BeginnersGuideCppCheatMod] C++ mod loaded!
[15:23:00.2625063] [BeginnersGuideCppCheatMod] Unreal initialised.
```

!!! tip "If you don't see the message"
    If you do not see the message, or you see errors or failures in the logs, check:

    - The mod folder name matches what UE4SS expects in `mods.txt`.
    - The DLL is inside the `dlls` folder.
    - The DLL has been renamed to `main.dll`.
    - The DLL was built for `Game__Shipping__Win64`.
    - `mods.txt` includes `BeginnersGuideCppCheatMod : 1`.

## A note on debugging

C++ modding is much easier when you build in very small steps. Get the mod loading first. Then add one piece of behaviour at a time.

When something goes wrong, remove the last change and prove the mod still loads. It sounds boring, but a tiny mistake in C++ can turn into a crash before you get a useful error message, so treat each working build as a checkpoint.

## What comes next

At this point we've created a pretty boring mod, but also a achieved an important milestone: a compiled C++ mod that UE4SS can load into Subnautica 2.

The next step is to use the same investigation process from the Lua mod:

1. Find a useful class or function in UE4SS Live View, the generated types, or FModel.
2. Work out whether it is safe to access from C++.
3. Add a small hook or runtime change.
4. Build, install, launch, and test.

