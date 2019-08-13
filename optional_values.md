# Optional values

Instead of using `NULL` pointers
or special values (e.g. `0`, `-1`, etc.)
Rust uses something called an ["option type"](http://en.wikipedia.org/wiki/Option_type).

You could make an option type yourself by using an enum

```rust
enum MaybeInteger {
    None,
    Some(i64),
}
```

However, Rust already includes a ready-to-use option type:

```rust
enum Option<T> {
    None,
    Some(T),
}
```

`T` here means that `Option` can be used with any type of your choice.
When you use `Option<String>`,
the compiler will generate something like this "under-the-hood":

```rust
enum Option_String {
    None,
    Some(String),
}
```

## Example

A user registering on a website must specify their first name,
but year of birth is optional.

```rust
struct User {
    first_name: String,
    year_of_birth: Option<u64>
}

fn print_year_of_birth(user: User) {
    match user.year_of_birth {
        Some(year) => println!("Year of birth: {}", year),
        None => println!("Year of birth is unknown"),
    }
}

fn main() {
    let person1 = User {
        first_name: String::from("Antanas"), 
        year_of_birth: Some(1984),
    };
    let person2 = User {
        first_name: String::from("Beata"), 
        year_of_birth: None, // unknown
    };
    print_year_of_birth(person1);
    print_year_of_birth(person2);
}
```

## Example: error handling

```rust
fn unsafe_divide(a: i64, b: i64) -> i64 {
    a / b
}

fn safe_divide1(a: i64, b: i64) -> Option<i64> {
    if b == 0 {
        None
    } else {
        Some(a / b)
    }
}

fn safe_divide2(a: i64, b: i64) -> Option<i64> {
    match b {
        0 => None,
        _ => Some(a/b),
    }
}

fn main() {
    let a = 1;
    let b = 0;

    // unsafe_divide(a, b); // CRASH!
    safe_divide1(a, b); // does not crash
    safe_divide2(a, b); // does not crash

    // In order to use the result, match.

    match safe_divide2(a, b) {
        Some(result) => println!("{} divided by {} is {}", a, b, result),
        None => println!("You tried to divide {} by zero!", a),
    }
}
```
