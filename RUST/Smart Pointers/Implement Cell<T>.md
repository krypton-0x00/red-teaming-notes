```rust
use std::cell::UnsafeCell;
struct Cell<T> {
    value: UnsafeCell<T>,
}

impl<T> Cell<T> {
    pub fn new(value: T) -> Self {
        Cell {
            value: UnsafeCell::new(value),
        }
    }
    pub fn get(&self) -> T
    where
        T: Copy,
    {
        unsafe { *self.value.get() }
    }
    pub fn set(&self, new_val: T) {
        unsafe { *self.value.get() = new_val };
    }
}

fn main() {
    let cl = Cell::new(5);
    cl.set(17);
    println!("cl = {}", &cl.get());
}
```