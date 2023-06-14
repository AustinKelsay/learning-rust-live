# Session 1

## Rust Installation

1. **Download Rust through rustup**
   - [Rustup](https://rustup.rs/) is a command-line tool for managing Rust versions.
   - Alternative installation methods can be found [here](https://forge.rust-lang.org/infra/other-installation-methods.html).

2. **Command-line Notation**
   - Commands to be entered in terminal start with `$` (or `>` for PowerShell on Windows).
   - `$` or `>` is not part of the command and should not be typed.

3. **Installing rustup on Linux or macOS**
   - Open a terminal and enter the following command:
     ```bash
     curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh
     ```
   - You might need a C compiler for some Rust packages and to fix potential linker errors.

4. **Installing rustup on Windows**
   - Visit [https://www.rust-lang.org/tools/install](https://www.rust-lang.org/tools/install) and follow the instructions.
   - You'll need to install Visual Studio 2022 with "Desktop Development with C++", Windows 10/11 SDK, and the English language pack.

5. **Troubleshooting Installation**
   - Verify installation by checking Rust version in the terminal with `rustc --version`.
   - If Rust isn't working, ensure it's in your system's PATH with `echo %PATH%` (Windows CMD), `echo $env:Path` (PowerShell), or `echo $PATH` (Linux/macOS).

6. **Updating and Uninstalling**
   - Update Rust with `rustup update`.
   - Uninstall Rust with `rustup self uninstall`.

7. **Local Documentation**
   - Access offline documentation by running `rustup doc` in your terminal.



## Basic Steps for Running a Rust Program

### Setting up project directory:
- Create a directory to store your Rust code. In this example, we are creating a projects directory in the home directory, and hello_world project within it.


For Unix-based systems, use:
```
$ mkdir ~/projects
$ cd ~/projects
$ mkdir hello_world
$ cd hello_world
```
For Windows, use:
```
> mkdir "%USERPROFILE%\projects"
> cd /d "%USERPROFILE%\projects"
> mkdir hello_world
> cd hello_world
```

### Writing the Rust program:
- Create a new source file called main.rs. Rust files always end with .rs extension.


Use this code for your first "Hello, World!" program:
```
fn main() {
    println!("Hello, world!");
}
```

### Compiling and running the Rust program:

On Linux or macOS, use:
```
$ rustc main.rs
$ ./main
```
On Windows, use:
```
> rustc main.rs
> .\main.exe
```

Your terminal should now print "Hello, world!".

## Anatomy of a Rust Program

- The main function: This function is always the first code that runs in every executable Rust program. It takes no parameters and returns nothing.
- Body of the main function: It includes the line println!("Hello, world!"); which prints the string "Hello, world!" to the screen. 
- This line calls a Rust #macro (indicated by !).
- Semicolon ; The semicolon indicates that this expression is over and the next one is ready to begin.


### Compiling and Running Are Separate Steps

- Before running a Rust program, you must compile it using the Rust compiler (#rustc), passing it the name of your source file.
- Once compiled successfully, Rust outputs a binary executable which you can run. 
- This feature allows you to distribute your programs as executables, independent of whether the receiver has Rust installed or not.

## #Cargo
- Rust's build system and package manager
- Handles tasks like building code, downloading and building dependencies
- Installed with Rust if you used the official installers
- Check if Cargo is installed: `$ cargo --version`


### Creating a Project with Cargo
- Command: 
- `$ cargo new hello_cargo then $ cd hello_cargo`
- Creates a directory and project of the same name
- Generates two files and one directory: Cargo.toml and src directory with a main.rs file
- Initializes a new Git repository along with a .gitignore file


### #Cargo.toml
- Configuration file in TOML format
- Contains package info and dependencies list
- [dependencies] is for listing any project's dependencies


### #main.rs
- Located in src directory
- Where your program code resides

### Building and Running a Cargo Project
- cargo build: compiles the code and creates an executable in target/debug/
- cargo run: compiles and runs the code
- cargo check: checks if code compiles but doesn’t produce an executable (faster)

### Cargo Workflow
- Create project: cargo new
- Build project: cargo build
- Build and run project: cargo run
- Check for errors without producing binary: cargo check
- Binaries stored in target/debug/ directory


### Building for Release
- cargo build --release: compiles with optimizations for release
- Produces executable in target/release/ directory
- Useful when you want the fastest running time and aren't rebuilding often


### Cargo vs #rustc
- For simple projects, using rustc directly might suffice
- But as programs become more intricate or need dependencies, it’s easier to let Cargo coordinate the build


### Cargo commands for existing projects
```
$ git clone example.org/someproject
$ cd someproject
$ cargo build
```