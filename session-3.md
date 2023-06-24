# Session 3

## constants:
- Constants are bound to a name and cannot change, similar to #immutable variables.
- Constants are declared using the `const` keyword, not `let`.
- `mut` cannot be used with constants, they're always #immutable.
- The type of the constant value must always be annotated.
- Constants can be declared in any scope, including the global scope.
- Constants can only be set to a constant expression, not the result of a value that could only be computed at runtime.

## shadowing
- In Rust, you can declare a new variable with the same name as a previous one. This is known as "shadowing" (#shadowing).
- The first variable is shadowed by the second.
- The second variable takes precedence until it's shadowed or the scope ends.

```
fn main() {
    let x = 5; // Declare a variable 'x' and bind it to the value 5

    let x = x + 1; // Shadow 'x' with a new 'x' which is the previous 'x' plus 1

    {
        let x = x * 2; // Within a new scope, shadow 'x' with a new 'x' which is the previous 'x' times 2
        println!("The value of x in the inner scope is: {}", x); // Print the value of 'x' in the inner scope
    }

    println!("The value of x is: {}", x); // Print the value of 'x' in the outer scope
}
```

- The program first binds x to 5, then shadows x to x + 1 (so x is now 6).
- Inside an inner scope, x is again shadowed and is now x * 2 (so x is now 12).
- After exiting the inner scope, x reverts to the last value in the outer scope (6).
- Running the program results in the following output:

```
The value of x in the inner scope is: 12
The value of x is: 6
```

- Shadowing is a Rust concept that allows redefinition of a variable with the let keyword. This differs from mut which allows in-place mutation of a variable's value.
```
let x = 5;
let x = x + 1; // This is shadowing
```
- Shadowing provides a compile-time safety. If we try to reassign to a variable without using the let keyword, the compiler throws an error.
- Shadowing allows changing a variable's type. For example, a variable can start as a string and then be shadowed as a number.
```
let spaces = "   ";
let spaces = spaces.len(); // From string to usize
```
- If we attempt to mutate a variable’s type using mut, we’ll receive a compile-time error. Rust doesn't allow mutation of a variable's type.
```
let mut spaces = "   ";
spaces = spaces.len(); // This will cause an error
```

In the above example, Rust expected spaces to be &str but found usize.


## type errs
- Every value in Rust belongs to a specific #data type. Rust needs to know the type of data to work with it effectively.
- Two subsets of data types are #scalar and #compound.
- Rust is a #statically typed language. It needs to know the types of all variables at compile time.
- The #compiler can usually infer the type based on the value and its use.
- If multiple types are possible, a #type annotation is required, e.g. let guess: u32 = "42".parse().expect("Not a number!");
- If a type annotation is not provided when needed, Rust will display an error message.
Example error when type annotation is omitted:
```
$ cargo build
   Compiling no_type_annotations v0.1.0 (file:///projects/no_type_annotations)
error[E0282]: type annotations needed
 --> src/main.rs:2:9
  |
2 |     let guess = "42".parse().expect("Not a number!");
  |         ^^^^^
  |
help: consider giving `guess` an explicit type
  |
2 |     let guess: _ = "42".parse().expect("Not a number!");
  |              +++
```

## scalar types
- #Scalar: A scalar type in Rust represents a single value. There are four primary scalar types in Rust: integers, floating-point numbers, Booleans, and characters.
- #Integer: An integer is a number without a fractional component. In Rust, integers can be signed (with a minus sign, can represent negative numbers) or unsigned (no minus sign, only positive numbers and zero). For example, i32 for signed 32-bit integers and u32 for unsigned 32-bit integers.
- #Floating-Point: These are numbers with a decimal point. Rust has two primitive types for floating point numbers: f32 (Single-precision float) and f64 (Double-precision float).
- #Boolean: A boolean type in Rust has two possible values: true or false.
- #Character: A character type in Rust represents a Unicode Scalar Value, which means it can represent a lot more than just ASCII. Defined with single quotes like 'a'.

## compound types
- #Compound: Compound types can group multiple values into one type. Rust has two primitive compound types: tuples and arrays.
- #Tuple: A tuple is a collection of several values of different types. It is constructed using parentheses, , separated, like let tup: (i32, f64, u8) = (500, 6.4, 1);.
- #Array: An array is a collection of multiple values of the same type, with a fixed length. It is constructed using square brackets, , separated, like let arr: [i32; 5] = [1, 2, 3, 4, 5];.\

## integer types
- An integer is a number without a fractional component.
- Examples: u32, i32
- Integer types in Rust can be signed (i) or unsigned (u).
  - u for unsigned (only positive)
  - i for signed (positive and negative)
- Each type takes up a specific number of bits: 8, 16, 32, 64, 128.
- For example, u32 is an unsigned integer taking up 32 bits.
- Rust offers arch-dependent integer types: isize, usize.
- Depends on the architecture of the computer your program is running on.
- 64 bits if you’re on a 64-bit architecture and 32 bits if you’re on a 32-bit architecture.
Integer range:

Integer Types in Rust:
```
Length	Signed	Unsigned
8-bit	i8	u8
16-bit	i16	u16
32-bit	i32	u32
64-bit	i64	u64
128-bit	i128	u128
arch	isize	usize
```

## #integer literals
An integer literal is a direct representation of an integer value that's written into your source code. In Rust, you can express integer literals in a few different formats:

- Base 10 (Decimal): 42
- Base 16 (Hexadecimal): 0x2A
- Base 8 (Octal): 0o52
- Base 2 (Binary): 0b101010
- Byte (u8 only): b'A'
- You can also append a type suffix to integer literals to specify their types explicitly, such as 57u8, 42u32, 127i8, etc.

Furthermore, Rust allows the use of underscores (_) as visual separators in numeric literals, which can make larger numbers easier to read. For instance, 1_000_000 is the same as 1000000 but more readable.


## #integer overflow
Variables of type u8 can hold values between 0 and 255. An attempt to assign a value outside of this range results in integer overflow. #IntegerOverflow

## floats
- Rust has two types of #floating-point numbers: #f32 and #f64
  - f32: 32 bits in size, single-precision float
  - f64: 64 bits in size, double-precision float (default type)
- Floating-point types in Rust are always signed.
- They are used for representing numbers with decimal points.
- Follow the #IEEE-754 standard.
- Example of use:

  ```rust
  fn main() {
      let x = 2.0; // f64
      let y: f32 = 3.0; // f32
  }

## chars
- Rust's #char type is used to represent alphabetic characters.
- Declare `char` values using single quotes. E.g., `'z'`, `'ℤ'`, `'😻'`.
  ```rust
  fn main() {
      let c = 'z';
      let z: char = 'ℤ'; // with explicit type annotation
      let heart_eyed_cat = '😻';
  }

- char type is 4 bytes in size.
- Represents a Unicode Scalar Value, enabling it to represent more than just ASCII:
  - Accented letters, Chinese, Japanese, Korean characters, emoji, zero-width spaces.
  - Unicode Scalar Values range: U+0000 to U+D7FF and U+E000 to U+10FFFF.
- A "character" in human intuition may not align with what char represents in Rust due to Unicode specifications.


## Tuple
- A #tuple is a compound type used to group together a variety of types.
  - Fixed length, cannot grow or shrink.
  - Types of values don't have to be the same.
- Created by writing a comma-separated list of values inside parentheses.
  - Optional type annotations can be added.
- Example of use:
  ```rust
  fn main() {
      let tup: (i32, f64, u8) = (500, 6.4, 1);
  }

- Tuples are considered single compound elements.
- You can get individual values out of a tuple using #pattern_matching, a process known as #destructuring.


```
fn main() {
    let tup = (500, 6.4, 1);

    let (x, y, z) = tup;

    println!("The value of y is: {}", y);
}
```

- We can also access a tuple element directly by using a period (.) followed by the index of the value we want to access. For example:

```
fn main() {
    let x: (i32, f64, u8) = (500, 6.4, 1);

    let five_hundred = x.0;

    let six_point_four = x.1;

    let one = x.2;
}
```

### arrays
- #Arrays in Rust
  - A collection of multiple values of the same type.
  - Fixed length, unlike vectors.
  - Elements are written as a comma-separated list inside square brackets.
  - Example: `let a = [1, 2, 3, 4, 5];`
- Allocation and Use
  - Useful when you want data on the #stack, not the #heap.
  - Suitable when you need a fixed number of elements.
  - Less flexible than the #vector type, which can grow or shrink.
- Array Declaration
  - The type of an array is declared using square brackets, with the type of each element, a semicolon, and then the number of elements.
  - Example: `let a: [i32; 5] = [1, 2, 3, 4, 5];`
    - `i32`: Type of each element.
    - `5`: Number of elements in the array.


- Attempting to access an #array element past its length in #Rust causes a #panic (runtime error).
  - Example code:
    ```rust
    use std::io;
    fn main() {
        let a = [1, 2, 3, 4, 5];
        println!("Please enter an array index.");
        let mut index = String::new();
        io::stdin()
            .read_line(&mut index)
            .expect("Failed to read line");
        let index: usize = index
            .trim()
            .parse()
            .expect("Index entered was not a number");
        let element = a[index];
        println!("The value of the element at index {index} is: {element}");
    }
    ```
  - Running with valid indices (0-4) prints corresponding array value.
  - Invalid indices cause a panic with a message like: `thread 'main' panicked at 'index out of bounds: the len is 5 but the index is 10', src/main.rs:19:19`
- This demonstrates Rust's #memory-safety. Unlike many low-level languages, Rust checks array indices for validity, preventing invalid memory access.
- To handle errors more gracefully (avoiding panics), see Rust's advanced error handling discussed in Chapter 9.


## #functions
- Functions are defined with the `#fn` keyword followed by the function name, parentheses `()` and a block `{}`.
  - Example: `fn hello() {}`
- Parameters are specified within the parentheses, with their type.
  - Example: `fn greet(name: String) {}`
- For multiple parameters, use a comma to separate them.
  - Example: `fn add(x: i32, y: i32) {}`
- Functions in Rust return a value, which is defined by the '->' symbol followed by the type.
  - Example: `fn square(x: i32) -> i32 {}`
- The return value of a function is the value of the final expression in the block. The `return` keyword can be used to return early.
  - Example: 
    ```rust
    fn square(x: i32) -> i32 {
      x * x
    }
    ```
- Rust does not require parentheses around the return value.
- Private functions (only accessible within the same module) are the default, use `#pub` before `#fn` to make a function public.
  - Example: `pub fn hello() {}`


## Statements and Expressions




