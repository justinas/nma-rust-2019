# Setting up for Rust development on Linux.

## Install Rust

1. Visit [Rustup home page](https://rustup.rs/). Run the recommended command in terminal.
2. Log out and log in again.
3. Open terminal and try it out:

```
cargo new sandbox # make a new Cargo project named "sandbox"
cd sandbox        # enter its folder
cargo run         # build and run your project!
```

## Install Visual Studio Code and configure it for Rust
1. Download and install [Visual Studio Code](https://code.visualstudio.com/#alt-downloads).
   Choose the suitable executable for your Linux distribution.

Then follow the steps in the Windows manual,
but instead of "C/C++", install extension "CodeLLDB" (important!).

## Configure Visual Studio Code for your Rust project

1. Relaunch VS Code. Open your project folder (e.g. `/home/justinas/sandbox`).
2. Press F5. Choose "LLDB".  You will get a warning that no build configurations exist. Click OK. Then you will get a dialog asking if you want to generate one, choose "Yes".
3. That's it! Press F5 again and your program will run.
