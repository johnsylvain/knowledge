# Ownership

Ownership is a central feature of Rust, and is how Rust manages memory. Rust doesn't have a garbage collector, instead it uses the concept of ownership to determine when to return memory that isn't used anymore when it goes out of scope.

### Rules:
* Each value in Rust has a variable that's called its owner.
* There can only be one owner at a time.
* When the owner goes out of scope, the value will be dropped.

Variables that are stored in the stack are dropped when they go out of scope.

```rust
{                   // s is not valid here, it's not declared
  let s = "hello";  // s is valid from this point forward

  // do stuff with s
}                   // the scope is over, s is dropped and no longer valid
```

More complex data types (ones with a dynamic size) are stored on the heap.

```rust
let s1 = String::from("hello");
let s2 = s1; // reference to s1 is "moved" to s2

// Rust invalidates `s1`

println!("{}, world!", s1);
```

Data that is stored on the stack can be copied because they are of a fixed size.

Function arguments behave the same way. They are invalidated when they go out of scope. Return values of functions transfer ownership the same way as complex data types.

When a complex data type is passed into a function, the value moves into the function and the original variable is invalidated.

```rust
fn main() {
  let s = String::from("hello");  // s comes into scope

  takes_ownership(s);             // s's value moves into the function
                                  // s is no longer valid here
}

fn takes_ownership(some_string: String) {
  println!("{}", some_string);
}
```

