# C++ mod setup

There's quite a bit of setup required before you can start coding your C++ mod. Thankfully, the steps are well documented and you only have to do this once, when initially setting up your environment.

!!! note
    This page borrows extensively from the official UE4SS documentation for creating C++ mods. I've tweaked and tailored the steps specifically for Subnautica 2, but you'll still find a lot of very useful and relevant information in the official docs:

    - [Creating a C++ Mod - UE4SS Documentation](https://docs.ue4ss.com/dev/guides/creating-a-c%2B%2B-mod.html)
    - [Installing a C++ Mod - UE4SS Documentation](https://docs.ue4ss.com/dev/guides/installing-a-c%2B%2B-mod.html)

## Prerequisites

Before starting this section, make sure you've already completed the earlier setup pages:

- [UE4SS](../../setting-up/install-and-setup/ue4ss.md) is installed and working with Subnautica 2.
- [Visual Studio 2022](../../setting-up/install-and-setup/visual-studio.md) is installed with the relevant workloads and components.
- [GitHub Desktop](../../setting-up/install-and-setup/creating-a-git-repo.md#install-github-desktop) is installed.
- Your [GitHub account is linked](../../setting-up/install-and-setup/creating-a-git-repo.md#link-your-github-account-to-epic-games) to your Epic Games account for Unreal Engine source access.
- [Rust](../../setting-up/install-and-setup/rust.md) is installed.
- You can launch the game and see the UE4SS console.
- You have a modding workspace where you can keep your source code under Git.

The official UE4SS guide uses the main UE4SS repository in it's setup steps. For this guide, we'll use the Subnautica 2 fork instead:

- [Subnautica2Modding/Subnautica2-UE4SS](https://github.com/Subnautica2Modding/Subnautica2-UE4SS)

## Set up the UE4SS source

For this section, I recommend using command-line `git` rather than GitHub Desktop. The UE4SS source tree has recursive submodules, including the Unreal Engine source dependency, and I found that GitHub Desktop got itself in a bit of a muddle when cloning the UE4SS repository. I believe in part due to authentication requirements for the Unreal Engine submodule dependencies.

Go to the start menu, and start typing `Developer PowerShell` and you should see an icon for `Developer PowerShell for VS`. Click the icon to run PowerShell. We'll use this for the `git` and `cmake` commands in this section, as it sets up paths and the Visual Studio build environment automatically.

First, check that `git` and `cmake` are both available:

```powershell
git --version
cmake --version
```

If `git` is not recognised, check you've properly installed [GitHub Desktop](../../setting-up/install-and-setup/creating-a-git-repo.md#install-github-desktop). If `cmake` is not recognised, open the [Visual Studio](../../setting-up/install-and-setup/visual-studio.md) Installer and check that you've selected all the right workloads and components.

Now change directory to your modding workspace. In my setup, that looks like this:

```powershell
cd E:\Dev\Subnautica-Modding\SN2\Subnautica2Mods\mods
```

Clone the Subnautica 2 UE4SS repository fork:

```powershell
git clone https://github.com/Subnautica2Modding/Subnautica2-UE4SS.git
cd Subnautica2-UE4SS
```

Before initialising the submodules, tell Git to use HTTPS for GitHub submodules. I had to do this in order to clone the sub-modules, as without it git was complaining about SSL host issues:

```powershell
git config --global url."https://github.com/".insteadOf "git@github.com:"
```

Now initialise the submodules:

```powershell
git submodule update --init --recursive
```

This step can take a while. It may also open a browser window asking you to log into GitHub. That is expected if Git needs authenticated access.

!!! important "Unreal Engine source access"

    The `deps/first/Unreal` submodule requires access to Epic's private Unreal Engine GitHub repository. If you have not already done this, link your GitHub account to your Epic Games account, then accept the email invitation to join the EpicGames GitHub organization.
    
    Without that access, Git may not be able to download the Unreal Engine submodule.

!!! tip "If submodule setup fails"

    If Git fails with `Host key verification failed` while cloning a submodule such as `patternsleuth`, Git is probably trying to use SSH before your machine is set up for GitHub SSH access. Run:
    
    ```powershell
    git config --global url."https://github.com/".insteadOf "git@github.com:"
    git submodule sync --recursive
    git submodule update --init --recursive
    ```
    
    If a failed attempt leaves a submodule in a broken state, you may see errors like `No such ref: HEAD` or `Unable to find current revision in submodule path`. Clear the failed submodule state and try again:
    
    ```powershell
    git submodule deinit -f deps/first/patternsleuth
    git submodule deinit -f deps/first/Unreal
    git submodule sync --recursive
    git submodule update --init --recursive
    ```
    
    If `deinit` says one of those submodules is not initialised, don't panic. Keep going with the remaining commands.

