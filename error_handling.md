# Result and error handling

While we can handle errors using [optional values](optional_values.md),
an `Option<T>` only tells us whether the value is "present or not".

`Result<T, E>` is another sum type provided by Rust, defined as:

```rust
enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

`T` here represents a success value type,
while `E` is the error type.

Building upon an earlier example on a save division function:

```rust
fn safe_divide(a: i64, b: i64) -> Result<i64, String> {
    match b {
        0 => Err(format!("Tried to divide {} by zero", a)),
        _ => Ok(a / b),
    }
}

fn main() {
    println!("{:?}", safe_divide(1, 0));
    println!("{:?}", safe_divide(1, 1));

    // again, we can match to unwrap the value
    match safe_divide(1, 0) {
        Ok(val) => println!("Got a successful result: {}", val),
        Err(e) => println!("Got an error: {}", e),
    }
}
```

Error type `E` can be anything: a string, an integer (e.g. an error code), etc..
It is even possible to use a unit ("void") type: `Result<T, ()>`.
This way, you can still signal `Ok` or `Err`, even though no error information is carried.
Semantically this is equivalent to `Option<T>`.

Usually, `E` will be a type that implements a special standard library trait (interface)
`std::error::Error`. But for now, we do not have to worry about that.

## Propagating errors "up"
Often we do several fallible actions in a row,
and if one of them fails, we can not continue anymore.

Take for example a function that:

1. Reads a file,
2. Converts the retrieved content to an integer,
3. Does a calculation on it,
4. Then writes the result back to file.
5. Also returns the calculated result from the function.

If step 1, reading a file, fails,
we can not calculate anything and should just end this sequence.

Let's write out a *pseudocode* of this.

```rust
fn read_file(filename: &str) -> Result<String, IoError> {
    // ...
}

fn convert_to_integer(content: &str) -> Result<i64, ParseError> {
    // ...
}

fn calculate(argument: i64) -> i64 {
    // ...
}

fn write_file(filename: &str, result: &str) -> Result<(), IoError> {
    // ...
}

fn do_calculation(filename: &str) -> Result<i64, MyError> {
    let content = match read_file("filename") {
        Ok(c) => c,
        Err(e) => return Err(e),
    }

    let number = match convert_to_integer(content) {
        Ok(n) => n,
        Err(e) => return Err(e),
    }

    let result = calculate(number);

    match write_file("filename", &result.to_string()) {
        Ok(_) => Ok(result),
        Err(e) => Err(e),
    }
}
```

Kind of verbose, isn't it?
Besides, `read_file` and `write_file` return an `IoError`,
while `convert_to_integer` returns a `ParseError`.
How do we turn that into a unified `MyError` type?

Luckily, Rust has a `?` (question mark) operator
that helps us in two ways when applied to a `Result`.

1. It unwraps the success value if an error did not happen.
2. It returns from the entire function if an error is encountered
3. It automatically converts between compatible Error types.

Let's rewrite this using `?` operator.

```rust
fn read_file(filename: &str) -> Result<String, IoError> {
    // ...
}

fn convert_to_integer(content: &str) -> Result<i64, ParseError> {
    // ...
}

fn calculate(argument: i64) -> i64 {
    // ...
}

fn write_file(filename: &str, result: &str) -> Result<(), IoError> {
    // ...
}

fn do_calculation(filename: &str) -> Result<i64, Box<dyn std::error:Error>> {
    let content = read_file("filename")?;
    let number = convert_to_integer(content)?;
    let result = calculate(number);
    write_file("filename", &result.to_string())?;
    result
}
```

Much less repetition, but the behavior is still the same:
only continue if no error happens,
otherwise immediately return the error from the function.

For now, just know that `Box<dyn std::error:Error>`
is a magic type that is "compatible"
with anything that implements the standard `Error` interface
we've mentioned before.
Thus `?` can convert any suitable type into the `Box<...>`.

## Using `?` in main

We can also use `?` in the `main()` function.
Similarly, it will return from `main()` early
(that means exit the whole program).

As nothing is usually returned from `main()`,
we have to use `()` for success type `T`.

Let's use a real standard library function
to read a file and return from main if it doesn't exist.

```rust
use std::fs::read_to_string;

fn main() -> Result<(), Box<dyn std::error::Error>> {
    // read_to_string() returns a Result<String, std::io:Error>
    // If it's `Ok`, it unwraps the String value into `content`.
    // If it's `Err`, it returns that Err after turning it into a `Box<...>`.
    let content = read_to_string("Cargo.toml")?;

    // `main()` only continues with the following lines if content was successfully read
    println!("Cargo.toml contains:\n\n{}", content);

    // We have to return a `Result` from main, so just return an empty `()` at the end.
    Ok(())
}
```

Again, `?` is most useful when we do multiple fallible actions:

```rust
use std::fs::read_to_string;

fn main() -> Result<(), Box<dyn std::error::Error>> {
    let content = read_to_string("Cargo.toml")?;
    let number : i64 = content.trim().parse()?; // This will fail!
    println!("File contains this number: {}", number);
    Ok(())
}
```

Try:

1. Replacing `Cargo.toml` with a name of a non-existent file.
   It will instead fail earlier: on line 4.
2. Replacing `Cargo.toml` with a name of a file that actually contains a single integer.
   The program will finish successfully!
