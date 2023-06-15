# Session 2

## Programming a Guessing Game

### Project Goal: a guessing game that generates a random number, accepts user guess, and provides feedback.

Setting Up a New Project:
```
$ cargo new guessing_game
$ cd guessing_game
```

Processing a Guess:
```
use std::io;

fn main() {
    println!("Guess the number!");
    println!("Please input your guess.");

    let mut guess = String::new();

    io::stdin()
        .read_line(&mut guess)
        .expect("Failed to read line");

    println!("You guessed: {}", guess);
}
```

### Breakdown
- use std::io; : This line brings in the io (input/output) library from std (standard library).
- let mut guess = String::new(); : This line creates a mutable variable named guess that is bound to a new, empty instance of a String.
- io::stdin().read_line(&mut guess).expect("Failed to read line"); : This line captures user input, expects a Result value and handles any potential error.

### Important Concepts
- let statement: used to create variables.
- let mut: used to create mutable variables.
- String::new(): creates a new, empty string.
- io::stdin().read_line(&mut guess): captures user input.
- Result: handles potential errors during runtime.
- println!(): print a string to the screen.


### enums / result types
- read_line takes user input and puts it into the passed string, returning a #Result value.
- Result is an #enum, a type that can be in multiple states.
- Result's states (or variants) are Ok and Err.
- Ok indicates successful operation, containing the generated value.
- Err indicates failure, containing information about the failure.
- Result types encode error-handling information.
- Result values have methods, like expect.
- expect will crash the program and display a message if the Result is an Err.
- If Result is Ok, expect returns the Ok value.
- Ignoring the Result value returned from read_line (not calling expect), results in a warning.
- Rust warns you because the possible error isn't handled.
- To suppress the warning, write error-handling code.
- In some cases, if you want to crash the program when a problem occurs, you can use expect.

### adding crates
- `Cargo.toml` consists of sections. Each section contains all content under its header.
- External crate dependencies are declared in the `[dependencies]` section.
- Version specifier `0.8.5` is equivalent to `^0.8.5`, allowing versions >=0.8.5 but <0.9.0.
- #Cargo fetches the latest compatible patch from the #Crates.io registry.
- Versions >=0.9.0 aren't guaranteed to work with your code due to potential API changes.
- `cargo build` is used to build the project.
- When new crates are added to `[dependencies]`, Cargo fetches and compiles them and their dependencies.
- Running `cargo build` without any changes results in no output apart from 'Finished'.
- If changes are made to `src/main.rs`, `cargo build` only recompiles the project, not the unchanged dependencies.
- Can be less precise only defining 0.3 vs 0.3.x
- "I almost never specify the patch version so I can pick up any non breaking changes"

### Ensuring reproducable builds
- `Cargo.lock` file is used by Rust's package manager, Cargo, to ensure **#reproducible_builds**. 
- When you build a project for the first time, Cargo figures out all the versions of the dependencies fitting the criteria, then writes them to the `Cargo.lock` file.
- In future builds, Cargo checks the `Cargo.lock` file and uses the versions specified there, instead of re-evaluating the dependencies.
- The versions of dependencies remain constant (even if new versions are available) until you explicitly upgrade.
- This ensures your build behavior stays consistent and won't break due to updates in dependencies.
- Since `Cargo.lock` is vital for reproducibility, it's commonly checked into source control alongside your project code.

### cargo update
- To update a crate in Rust, Cargo provides the `cargo update` command.
- This command disregards the `Cargo.lock` file and re-evaluates all latest versions matching your specifications in `Cargo.toml`.
- Cargo will then update the `Cargo.lock` file with these new versions.
- By default, Cargo looks for versions greater than the current but less than the next major release.
- To use a newer major version of a crate (like 0.9.x from 0.8.x), you need to update the `Cargo.toml` file manually, specifying the new version.

### rust analyzer
- Rust will infer types when it can otherwise there is probably a mistake

### rand crate
- use rand::Rng; is added at the start. This is necessary to bring the Rng #trait into scope. 
- Rng trait provides methods for random number generation. 
- rand::thread_rng() is used to get a random number generator that is local to the current thread and seeded by the operating system. 
- gen_range method is called on the random number generator. This method is part of the Rng trait, and generates a random number within the specified range. 
- The range expression used is start..=end, which is inclusive on both ends. For instance, 1..=100 will generate a random number between 1 and 100, inclusive.


### #match
- A match expression is made of arms, each consisting of a pattern and the associated code. 
- The pattern is what is being matched, and the code is what runs if there's a match.
- Rust checks each pattern of the match expression in turn. If a match is found, it executes the associated code and stops. If no match is found, it moves to the next pattern.


### getting ready to match:
```
use rand::Rng;
use std::cmp::Ordering;
use std::io;

fn main() {
    // --snip--

    println!("You guessed: {guess}");

    match guess.cmp(&secret_number) {
        Ordering::Less => println!("Too small!"),
        Ordering::Greater => println!("Too big!"),
        Ordering::Equal => println!("You win!"),
    }
}
```
- Add the use std::cmp::Ordering; statement to bring the Ordering enum into scope. This enum has three variants: #Less, #Greater, and #Equal.
- The cmp method, part of the Ordering type, compares two values. It takes a reference to the value to be compared.
- cmp returns a variant of the Ordering enum which can be used to decide next actions with a #match expression.


`let guess: u32 = guess.trim().parse().expect("Please type a number!");`

### #shadowing
- Create a variable guess
- Rust allows variable shadowing
  - This allows us to reuse the variable name guess without needing to create new, unique variables (e.g. guess_str, guess)
  - Variable shadowing is often used for type conversion
  - More details will be covered in Chapter 3


### #.trim and #.parse
- We bind a new variable with the expression guess.trim().parse().
- guess refers to the original input string.
- The trim method removes any leading or trailing whitespace (for example, newline characters like \n or \r\n).
- The parse method converts a string to another type.
- If it can't logically convert the string to a number, it will return a Result type with an Err variant.
- If successful, it returns a Result with an Ok variant, containing the converted number.

### type annotation
- Type annotation in Rust is done with a colon (e.g., let guess: u32).
- u32 is an unsigned, 32-bit integer and a good default for small positive numbers.
- Rust will infer types from comparisons (e.g., if guess is a u32 and is compared with secret_number, secret_number will also be inferred as u32).
- Result types are used to handle potential failure.
- The expect method is used to handle Result types.
- If parse returns an Err because it couldn't convert a string to a number, expect will cause the program to crash and print a given message.
- If parse returns Ok, expect will return the desired number.

### #loop
Use the loop keyword to create an infinite loop. This allows users to make multiple guesses.
```
loop {
    println!("Please input your guess.");

    match guess.cmp(&secret_number) {
        Ordering::Less => println!("Too small!"),
        Ordering::Greater => println!("Too big!"),
        Ordering::Equal => println!("You win!"),
    }
}
```

- Currently, there are two ways to end the game:
- Interrupt the program using ctrl-c
- Enter a non-numeric input, causing the program to crash
The game should ideally also stop when the correct number is guessed.

loop : Creates an infinite loop in rust
match : Used for pattern matching in rust
cmp : Compares two values in rust.
Ordering : An enum in Rust that can be Less, Greater, or Equal.


### quiting after a correct guess
```
        // --snip--

        match guess.cmp(&secret_number) {
            Ordering::Less => println!("Too small!"),
            Ordering::Greater => println!("Too big!"),
            Ordering::Equal => {
                println!("You win!");
                break;
            }
        }
    }
}
```

```
    // --snip--

    io::stdin()
        .read_line(&mut guess)
        .expect("Failed to read line");

    let guess: u32 = match guess.trim().parse() {
        Ok(num) => num,
        Err(_) => continue,
    };

    println!("You guessed: {guess}");

    // --snip--
```


### handling invalid input
- The code snippet is taken from `src/main.rs`
- It reads a string from the standard input using `io::stdin().read_line(&mut guess)`
- `read_line` can fail, previously we used `expect` which crashes the program on failure. Now we handle this using a `match` expression
- `guess.trim().parse()` attempts to convert the input string into a `u32` integer. The `parse` method returns a `Result` type
- `Result` is an enum with variants `Ok` and `Err`
- If parsing is successful, `parse` returns an `Ok` variant containing the parsed number. This matches the first arm of the `match` expression and `num` is returned and assigned to `guess`
- If parsing fails, `parse` returns an `Err` variant. This doesn't match the first arm of the `match` expression but matches the second arm, `Err(_)`. The underscore `_` is a wildcard matching any `Err` value
- On `Err`, the program executes `continue` to start the next iteration of the loop, effectively ignoring the parse error and asking for a new guess


### Final project
- First remove the println that prints the secret number

```
use rand::Rng;
use std::cmp::Ordering;
use std::io;

fn main() {
    println!("Guess the number!");

    loop {
        println!("Please input your guess.");

        let secret_number: u32 = rand::thread_rng().gen_range(1..=100);

        let mut guess: String = String::new();

        io::stdin()
            .read_line(&mut guess)
            .expect("Failed to read line");

        let guess: u32 = match guess.trim().parse() {
            Ok(num) => num,
            Err(e) => {
                println!("error: {e}");
                continue;
            }
        };

        println!("You guessed: {guess}");

        match guess.cmp(&secret_number) {
            Ordering::Less => println!("Too small!"),
            Ordering::Greater => println!("Too big!"),
            Ordering::Equal => {
                println!("You win!");
                break;
            }
        }
    }
}
```

