% Langages systèmes 2 - Introduction to Rust
% Raphael Amiard - Adacore
% amiard@adacore.com

# Introduction to Rust

## Pattern matching

```rust
match e {
    // Matching on a struct inside an enum:
    Expr::BinOp {l, op, r} => ...

    Expr::Literal(1..=5) => ...
    //            ^ Match an integer between 1 and 5 included

    Expr::Literal(8) => ...
    //            ^ Match an integer of value exactly 8

    Expr::Literal(v) => ...
    // Matching on a tuple inside an enum

    Expr::Literal(_) => ...
    // Ignore the value
}
```

## Pattern matching

```rust
struct Point {
    x: i32, y: i32
}

// Pattern matching on a struct (kind of useless, gives warning used like that)
match p {
    Point {x, y} => ...
}

// Irrefutable pattern inside let
let Point {x, y} = p

// Will only enter the body of the if if the match worked.
if let Expr::BinOp(l, op, r) = expr {
    ...
}
```

## Option type

```rust
use std::option::Option;
fn get_data() -> Vec<i32> { ...  }

fn main() {
    let a = get_data();
    let val: Option<i32> = a.pop();
    //                     ^ Get last element of vec
    
    match val {
        Some(val) => println!("Got a value out of vector: {}", val),
        None => println!("No value")
    }
}
```

* **Extremely** common type to represent possibility of a value
* Will be found everywhere

## Result type

* Generic type
* Error type (`E`) is often a string (`&'static str`, a static string reference)

```rust
// Here is how the result type is defined in the stdlib
enum Result<T, E> {
   Ok(T),
   Err(E),
}
```

## Result type (2)

```rust
fn main() {
    let a = "10".parse::<i32>();

    // Handle either option, error or OK
    match a {
        Ok(val) => println!("{val}")
        Err(e) => println!("No value. Error: {e}")
    }

    // Only handle OK
    if let Some(val) = a {
        println!("{val}")
    }

    // Panic on error
    println!("{}", a.unwrap());
}
```

## Pattern matching: loops

```rust
fn get_data() -> Vec<i32> { ... }

fn main() {
    let data = get_data();

    //   Irrefutable pattern
    for (idx, val) in data.iter().enumerate() {
    }

    // Iterate while we can match the pattern
    while let Some(a) = data.pop() {
        ...
    }
}
```

## Pattern matching: let & functions

```rust
fn print_point_1(p: (i32, i32)) {
    let (a, b) = p;
    //  ^ This is a pattern
    println!("Current location: ({a}, {b})");
}

fn print_point_2((a, b): (i32, i32)) {
    //          ^ This is a pattern
    println!("Current location: ({a}, {b})");
}
```

# Rust traits & generics

## Traits & generics