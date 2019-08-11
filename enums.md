## Basic enums
Just like C, Rust lets you define enums.

```rust
enum Location {
    Vilnius,
    Nida,
}
```

And you can assign explicit integer values to enum variants:

```rust
enum Location {
    Vilnius = 1
    Nida = 2,
}

fn main() {
    // Every enum is a type.
    let where_from = Location::Vilnius;
    let where_to : Location = Location::Nida;

    // You can cast enum to an integer.
    println!("{}", Location::Vilnius as u64);

    // But not integer to enum!
    // This way Rust ensures you can not construct
    // an invalid enum variant (e.g. some Location with the value 3).
    println!("{:?}", 2 as Location); // ERROR!
}
```

Just like with structs, you can tell the compiler
to [make your type printable](tips.md#make-your-type-printable).
