# Rust

Rust is required if you want to build UE4SS from source, which is part of the C++ mod workflow later in this guide. You don't need Rust for simple Lua mods.

## Install Rust

1. Go to the official Rust install page: [https://www.rust-lang.org/tools/install](https://www.rust-lang.org/tools/install)

2. Download and run `rustup-init.exe` - I chose the 64-bit option.

3. When prompted, choose the default installation option.

4. When the installer finishes, open a command prompt and run these commands to check everything is installed correctly:

    ```powershell
    rustc --version
    cargo --version
    ```

If both commands print a version number, Rust is installed and ready.
