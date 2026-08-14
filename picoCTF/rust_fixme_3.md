# Rust Fixme 3
| **Platform** | picoCTF 2025 |
| **Category** | General Skills |
| **Difficulty** | Easy |
| **Author** | Taylor McCampbell |
| **Topic** | Rust / Unsafe Rust / Raw Pointers / Cargo |

## Challenge

The challenge says:

```text
Have you heard of Rust? Fix the syntax errors in this Rust file to print the flag!
```

The hint says:

```text
Read the comments...darn it!
```

The challenge cannot be solved using the picoCTF webshell, so the Rust project needs to be downloaded and executed locally.

## Step 1 – Download the Rust Project

Click the **"here"** link next to:

```text
Download the Rust code here
```

Download and extract the project.

The extracted directory contains:

```text
Cargo.toml
Cargo.lock
src/
```

The Rust source file is:

```text
src/main.rs
```

## Step 2 – Open the Project

Open the extracted project folder in VS Code.

Then open:

```text
src/main.rs
```

The hint says:

```text
Read the comments...darn it!
```

The comments in the source code specifically explain that an unsafe Rust operation is being used.

## Step 3 – Compile the Program

Open the VS Code terminal and run:

```bash
cargo build
```

The compiler reports an error similar to:

```text
error[E0133]: call to unsafe function
`std::slice::from_raw_parts` is unsafe and requires unsafe function or block
```

The problematic operation is:

```rust
let decrypted_slice =
    std::slice::from_raw_parts(decrypted_ptr, decrypted_len);
```

`std::slice::from_raw_parts` is an unsafe function, so Rust requires it to be executed inside an `unsafe` block.

## Step 4 – Read the Comments

Immediately above the problematic code, the source contains comments explaining unsafe Rust.

The code is intended to look like:

```rust
unsafe {
    // Decrypt the flag operations
    let decrypted_buffer = xrc.decrypt_vec(encrypted_buffer);

    // Creating a pointer
    let decrypted_ptr = decrypted_buffer.as_ptr();
    let decrypted_len = decrypted_buffer.len();

    // Unsafe operation
    let decrypted_slice =
        std::slice::from_raw_parts(decrypted_ptr, decrypted_len);

    borrowed_string.push_str(
        &String::from_utf8_lossy(decrypted_slice)
    );
}
```

However, the `unsafe {` and closing `}` have been commented out.

## Step 5 – Uncomment the `unsafe` Block

Find:

```rust
// unsafe {
```

Change it to:

```rust
unsafe {
```

At the end of the block, find:

```rust
// }
```

Change it to:

```rust
}
```

So the relevant section becomes:

```rust
unsafe {
    // Decrypt the flag operations
    let decrypted_buffer = xrc.decrypt_vec(encrypted_buffer);

    // Creating a pointer
    let decrypted_ptr = decrypted_buffer.as_ptr();
    let decrypted_len = decrypted_buffer.len();

    // Unsafe operation: calling an unsafe function
    let decrypted_slice =
        std::slice::from_raw_parts(decrypted_ptr, decrypted_len);

    borrowed_string.push_str(
        &String::from_utf8_lossy(decrypted_slice)
    );
}
```

## Step 6 – Compile Again

Save the file and run:

```bash
cargo build
```

The project should now compile successfully.

## Step 7 – Run the Program

Run:

```bash
cargo run
```

The program decrypts the encrypted bytes and prints:

```text
Using memory unsafe languages is a: PARTY FOUL! Here is your flag: picoCTF{n0w_y0uv3_f1x3d_1h3m_411}
```

## Why This Works

Rust normally prevents potentially unsafe memory operations.

The function:

```rust
std::slice::from_raw_parts()
```

creates a slice from a raw pointer and a length.

Because raw pointers can be dangerous if they are invalid, Rust requires this operation to be explicitly placed inside an:

```rust
unsafe {
    ...
}
```

block.

The challenge intentionally commented out the `unsafe` block.

The hint:

```text
Read the comments...darn it!
```

points directly toward this mistake.

## Important Rust Concepts

### 1. Unsafe Rust

Rust normally provides strong memory-safety guarantees.

Some operations cannot be completely verified by the compiler, so they must be explicitly marked as unsafe:

```rust
unsafe {
    // unsafe operations
}
```

### 2. Raw Pointers

The program obtains a pointer using:

```rust
let decrypted_ptr = decrypted_buffer.as_ptr();
```

This pointer is then used with:

```rust
std::slice::from_raw_parts(
    decrypted_ptr,
    decrypted_len
);
```

Because this involves raw-pointer manipulation, the operation is unsafe.

### 3. Compiler Error E0133

The compiler error:

```text
error[E0133]
```

indicates that an unsafe operation was used outside an unsafe context.

The solution is to place the operation inside an `unsafe` block.

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

## Required Code Change

```diff
- // unsafe {
+ unsafe {

    let decrypted_buffer = xrc.decrypt_vec(encrypted_buffer);

    let decrypted_ptr = decrypted_buffer.as_ptr();
    let decrypted_len = decrypted_buffer.len();

    let decrypted_slice =
        std::slice::from_raw_parts(decrypted_ptr, decrypted_len);

    borrowed_string.push_str(
        &String::from_utf8_lossy(decrypted_slice)
    );

- // }
+ }
```

## Complete Solution

```text
Download the Rust project
        ↓
Extract the project
        ↓
Open src/main.rs
        ↓
Run cargo build
        ↓
Compiler reports E0133
        ↓
Read the comments in the source
        ↓
Find the commented unsafe block
        ↓
Uncomment "unsafe {"
        ↓
Uncomment the closing "}"
        ↓
Run cargo build again
        ↓
Compilation succeeds
        ↓
Run cargo run
        ↓
Program decrypts the data
        ↓
Flag is printed
```

## One-Line Solution

```bash
cargo build && cargo run
```

## Flag

```text
picoCTF{n0w_y0uv3_f1x3d_1h3m_411}
```

## Tools Used

- VS Code
- Rust
- Cargo
- `rustc`
- `cargo build`
- `cargo run`

## Key Learning

- Rust uses `unsafe` blocks for operations that cannot be fully verified for memory safety.
- `std::slice::from_raw_parts()` is an unsafe function.
- Raw-pointer operations require careful handling.
- Compiler error `E0133` indicates an unsafe operation was used outside an unsafe context.
- Reading source-code comments can provide important clues in CTF challenges.
- `cargo build` is useful for identifying Rust compilation errors.
- `cargo run` builds and executes a Rust project.
