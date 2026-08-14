# Rust Fixme 2
| **Platform** | picoCTF 2025 |
| **Category** | General Skills |
| **Difficulty** | Easy |
| **Author** | Taylor McCampbell |
| **Topic** | Rust / References / Borrowing |

## Challenge

The challenge says:

```text
The Rust saga continues? I ask you, can I borrow that, pleeeeeaaaasseeeee?
```

The challenge cannot be solved using the picoCTF webshell, so the Rust project must be downloaded and run locally.

## Step 1 – Download the Rust Project

Click the **"here"** link next to:

```text
Download the Rust code here
```

Download and extract the project.

The extracted directory contains files similar to:

```text
Cargo.toml
src/
```

## Step 2 – Open the Project

Open the extracted project folder in VS Code.

Open:

```text
src/main.rs
```

The source code contains an error involving Rust references and borrowing.

## Step 3 – Check Rust Installation

Open the VS Code terminal and run:

```bash
rustc --version
```

Then:

```bash
cargo --version
```

If both commands return version information, Rust and Cargo are installed correctly.

## Step 4 – Compile the Program

Run:

```bash
cargo build
```

The Rust compiler reports an error related to borrowing.

The hint points to Rust's references and borrowing system:

```text
https://doc.rust-lang.org/book/ch04-02-references-and-borrowing.html
```

The important concept is that Rust allows us to borrow a value using a reference instead of taking ownership of it.

## Step 5 – Fix the Borrowing Error

The problematic code attempts to pass a value where a reference is expected.

The fix is to borrow the value using `&`.

For example, change the function call from:

```rust
calculate_length(s)
```

to:

```rust
calculate_length(&s)
```

The `&` creates a reference to `s` instead of transferring ownership.

## Step 6 – Compile Again

After fixing the borrowing error, run:

```bash
cargo build
```

If there are no compilation errors, the Rust project has been fixed successfully.

## Step 7 – Run the Program

Run:

```bash
cargo run
```

The program executes successfully and prints the flag.

Output:

```text
picoCTF{4r3_y0u_h4v1n5_fun_y3l?}
```

## Why This Works

Rust uses an ownership and borrowing system to make memory management safe.

A function can borrow a value by receiving a reference:

```rust
&value
```

Instead of giving ownership of the value to the function.

For example:

```rust
let s = String::from("hello");
calculate_length(&s);
```

Here:

```text
s
 ↓
borrowed using &
 ↓
calculate_length(&s)
```

The original variable remains available after the function call.

## Important Concepts

### References

A reference allows us to access a value without taking ownership:

```rust
&value
```

### Borrowing

Passing a reference to a function is called borrowing:

```rust
function(&variable);
```

### Ownership

Rust normally transfers ownership when a value is passed by value.

Using a reference avoids that ownership transfer.

### Cargo

Cargo is Rust's package manager and build system.

Compile the project with:

```bash
cargo build
```

Run the project with:

```bash
cargo run
```

## Commands Used

Check Rust:

```bash
rustc --version
```

Check Cargo:

```bash
cargo --version
```

Compile:

```bash
cargo build
```

Run:

```bash
cargo run
```

## Complete Solution

```text
Download the Rust project
        ↓
Extract the project
        ↓
Open the folder in VS Code
        ↓
Open src/main.rs
        ↓
Run cargo build
        ↓
Read the borrowing error
        ↓
Use & to borrow the required value
        ↓
Run cargo build again
        ↓
Compilation succeeds
        ↓
Run cargo run
        ↓
Flag is printed
```

## One-Line Solution

```bash
cargo build && cargo run
```

## Flag

```text
picoCTF{4r3_y0u_h4v1n5_fun_y3l?}
```

## Tools Used

- VS Code
- Rust
- Cargo
- `rustc`
- `cargo build`
- `cargo run`

## Key Learning

- Rust uses ownership and borrowing to manage memory safely.
- `&` creates a reference to a value.
- Passing `&variable` borrows the value instead of transferring ownership.
- Rust compiler errors are useful for understanding ownership and borrowing problems.
- `cargo build` checks whether the project compiles.
- `cargo run` builds and executes the Rust program.
