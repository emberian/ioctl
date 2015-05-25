`ioctl`
=======

[![Travis](https://img.shields.io/travis/cmr/ioctl.svg?style=flat-square)](https://travis-ci.org/cmr/ioctl)
[![Crates.io](https://img.shields.io/crates/v/ioctl.svg?style=flat-square)](https://crates.io/crates/ioctl)

[Documentation](https://cmr.github.io/ioctl)

Helpers for binding `ioctl`s in Rust. Currently supports Linux on all architectures
except SPARC and Alpha. Other platforms welcome!

This library is pretty low-level and messy. `ioctl` is not fun.

Example
=======

```rust
#[macro_use]
extern crate ioctl;

ioctl!(read ev_get_version with b'E', 0x01; u32);
ioctl!(write ev_set_repeat with b'E', 0x03; [u32; 2]);

fn main() {
    let mut x = 0;
    let ret = unsafe { ev_get_version(0, &mut x) };
    println!("returned {}, x = {}", ret, x);
}
```
