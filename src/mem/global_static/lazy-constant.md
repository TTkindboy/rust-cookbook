## Declare lazily evaluated constant

[![std-badge]][std] [![cat-caching-badge]][cat-caching] [![cat-rust-patterns-badge]][cat-rust-patterns]

Declares a lazily evaluated constant [`HashMap`] using [`LazyLock`]. The [`HashMap`] will
be evaluated once and stored behind a global static reference.

```rust,edition2018
use std::sync::LazyLock;
use std::collections::HashMap;

static PRIVILEGES: LazyLock<HashMap<&'static str, Vec<&'static str>>> = LazyLock::new(|| {
    let mut map = HashMap::new();
    map.insert("James", vec!["user", "admin"]);
    map.insert("Jim", vec!["user"]);
    map
});

fn show_access(name: &str) {
    let access = PRIVILEGES.get(name);
    println!("{}: {:?}", name, access);
}

fn main() {
    let access = PRIVILEGES.get("James");
    println!("James: {:?}", access);

    show_access("Jim");
}
```

[`HashMap`]: https://doc.rust-lang.org/std/collections/struct.HashMap.html
[`LazyLock`]: https://doc.rust-lang.org/std/sync/struct.LazyLock.html
