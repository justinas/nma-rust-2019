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

## Enums as sum types
Unlike most languages, enum variants can carry data inside them.

```rust
enum Temperature {
    Celsius(f64),
    Fahrenheit(f64),
}
```

This allows us to create a safe type
that represents "either this or that",
as opposed to structs, that represent "this and that".

The variants can carry *different* types of data.
Enums can be composed with other types
and enum variants can be `match`-ed on.

Let's model some data types for an eshop.

```rust
enum DeliveryOption {
    StorePickup,
    TerminalPickup(u64), // u64 represents ID of the terminal
    HomeDelivery(String), // String represents the address
}

struct Order {
    customer_name: String,
    delivery: DeliveryOption,
    // A list of items, etc.
}

fn print_order(order: Order) {
    println!("An order by {}", order.customer_name);

    print!("The customer would like");
    match order.delivery {
        DeliveryOption::StorePickup => println!("to pick up the items in store."),
        DeliveryOption::TerminalPickup(id) => println!("the items to be shipped to terminal {}", id),
        DeliveryOption::HomeDelivery(address) => println!("the items to be delivered to their home address: {}", address),
    }
}

fn main() {
    let order1 = Order {
        customer_name: String::from("Antanas"),
        delivery: DeliveryOption::TerminalPickup(339),
    };
    let mut order2 = Order {
        customer_name: String::from("Beata"),
        delivery: DeliveryOption::StorePickup,
    };
    order2.delivery = DeliveryOption::HomeDelivery(String::from("Liepų 1, Vilnius")); // changed her mind
    print_order(order1);
    print_order(order2);
}
```
