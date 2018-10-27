# References

Allow you to reference a value without taking ownership. Use the `&` character to create a reference. References are also immutable by default.

```rust
fn main() {
  let s1 = String::from("hello");

  let len = calculate_length(&s1);

  println!("The length of {} is {}.", s1, len);
}

fn calculate_length(s: &String) -> usize {
  s.len()
}
```

## Mutable References

Use the `mut` keyword to make the reference mutable.

#### Guidelines
* You can only have one mutable reference to a piece of data in a particular scope.
* Don't combine mutable and immutable references

These guidelines prevent data races.

```rust
fn main() {
  let mut s = String::from("hello");

  change(&mut s);
}

fn change(some_string: &mut String) {
  some_string.push_str(", world");
}
```
