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