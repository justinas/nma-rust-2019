# Containers & iterators

## Vector
Vector or `Vec` is a dynamic array, just as in C++.

```rust
fn main() {
    let names = vec!["Antanas", "Beata", "Cezaris", "Daiva", "Ernestas"];
    println!("{}", names[1]); // You can index a vector
    println!("{:?}", &names[2..4]); // You can slice a vector using ranges
}
```

Mutability impacts vectors: you can only modify mutable vectors.


```rust
fn main() {
    let mut names = vec!["Antanas", "Beata", "Cezaris", "Daiva", "Ernestas"];
    println!("{:?}", names);

    names.push("Fausta"); // append
    names.remove(4); // remove by index
    names[1] = "Bronius"; // replace 
    println!("{:?}", names);
}
```

`Vec<String>` holds strings, `Vec<bool>` holds bools, etc.

## Iterating over containers

Just like you can [loop over a range](basics.md#loops),
you can iterate anything that is an *iterator*.
Vector is one of those iterator types.

```rust
fn main() {
    let names = vec!["Antanas", "Beata", "Cezaris", "Daiva", "Ernestas"];
    for name in names {
        println!("I got: {}", name);
    }
}
```

Iterating this way will *move* items out of the vector and leave the vector unusable!

```rust
fn main() {
    let names = vec!["Antanas", "Beata", "Cezaris", "Daiva", "Ernestas"];
    for name in names {
        println!("I got: {}", name);
    }
    println!("{:?}", names); // ERROR!
}
```

You can iterate over mutable or immutable references:

```rust
fn main() {
    let mut names = vec!["Antanas", "Beata", "Cezaris", "Daiva", "Ernestas"];
    for name in &mut names {
        println!("I got a mutable reference to: {}", name);
        if *name == "Beata" {
            *name = "Bronius"; // replace in vector
        }
    }

    for name in &names {
        println!("I got an immutable reference to: {}", name);
    }
}
```
