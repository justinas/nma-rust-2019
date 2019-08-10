# Struct(ure)s in Rust.

A Rust struct is similar to a struct in C.

```rust
struct Person {
    first_name: String,
    last_name: String,
    knows_rust: bool,
}

fn main() {
    // All the fields must be initialized.
    // Can not create a person with no last name or no year of birth.
    let me = Person {
        first_name: String::from("Justinas"),
        last_name: String::from("Stankevičius"),
        knows_rust: true,
    };

    println!("Hello, {}!", me.first_name);

    if me.knows_rust {
        println!("Wow, good job!");
    }
}
```

Note: we have to use `String::from`
because, as mentioned in the basics,
a string literal like `"Justinas"` is of a different type, spelled `&str`.
Let's just put up with this annoyance for now.

