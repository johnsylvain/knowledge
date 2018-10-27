# Strings

* `&str` stack allocated fixed size string slice. Use for function arguments
* `String` heap allocated growable string

```rust
let s = String::from("hello");
```

### String slices

Is a reference to part of a `String`. String literals have a type of `&str`.

```rust
let s = String::from("hello");

let slice: &str = &s[..]; //=> "hello"
```
