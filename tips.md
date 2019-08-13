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

## Make your types cloneable

Cloning is the easiest, although not very efficient way
to avoid "use of moved value" errors.

```rust
// directive to compiler to auto-generate a `clone()` method for this type.
#[derive(Clone)]
struct Person {
    first_name: String
}

fn main() {
    let person1 = Person { first_name: String::from("Justinas") };
    let person2 = person1.clone();
}
```

This can be mixed with other `derive` directives.
If you nest your types (struct in a struct, enum in a struct, etc.),
you have to add this directive for both children and parent types.

```rust
#[derive(Clone, Debug)]
enum Occupation {
    Student,
    PartTimeWorker,
    FullTimeWorker,
}

#[derive(Clone, Debug)]
struct Person {
    first_name: String,
    occupation: Occupation,
}

fn main() {
    let person1 = Person {
        first_name: String::from("Justinas"),
        occupation: Occupation::Student,
    };
    let mut person2 = person1.clone(); // cloneable
    person2.occupation = Occupation::FullTimeWorker;

    println!("{:?}", person1); // printable
    println!("{:?}", person2);
}
```
