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
    // Can not create a person with no last name
    // or an unspecified Rust knowledge level :)
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

`mut`, applied to structs:

```rust
struct Person {
    first_name: String,
    last_name: String,
    knows_rust: bool,
}

fn main() {
    let mut me = Person {
        first_name: String::from("Justinas"),
        last_name: String::from("Stankevičius"),
        knows_rust: false,
    };

    // learn some Rust
    me.knows_rust = true;

    if me.knows_rust {
        println!("Wow, good job!");
    }
}
```

See also: [make your type printable](tips.md#make-your-type-printable).
