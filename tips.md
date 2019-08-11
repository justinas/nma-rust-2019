# Tips

## Make your type printable

```rust
// directive to compiler to auto-generate code that knows how to print Person when passed to println!()
#[derive(Debug)]
struct Person {
    first_name: String
}

fn main() {
    let me = Person { first_name: String::from("Justinas") };
    println!("{:?}", me);
}
```

```rust
// works for enums as well
#[derive(Debug)]
enum Direction {
    North,
    East,
    South,
    West,
}

fn main() {
    let where_to = Direction::North;
    println!("{:?}", where_to);
}
```
