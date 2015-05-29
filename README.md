`ioctl`
=======

[![Travis](https://img.shields.io/travis/cmr/ioctl.svg?style=flat-square)](https://travis-ci.org/cmr/ioctl)
[![Crates.io](https://img.shields.io/crates/v/ioctl.svg?style=flat-square)](https://crates.io/crates/ioctl)

[Documentation](https://cmr.github.io/ioctl)

Helpers for binding `ioctl`s in Rust. Currently supports Linux on all architectures
except SPARC and Alpha. Other platforms welcome!

This library is pretty low-level and messy. `ioctl` is not fun.

What is an `ioctl`?
===================

The `ioctl` function is the grab-bag system call on POSIX systems. Don't want
to add a new syscall? Make it an `ioctl`! `ioctl` refers to both the syscall,
and the commands that can be send with it. `ioctl` stands for "IO control",
and the commands are always sent to a file descriptor.

How do I get the magic numbers?
===============================

Look at your system's headers. For example, `/usr/include/linxu/input.h` has a
lot of lines defining macros which use `_IOR`, `_IOW`, `_IOC`, and `_IORW`.
These macros correspond to the `ior!`, `iow!`, `ioc!`, and `iorw!` macros
defined in this crate. Additionally, there is the `ioctl!` macro for
creating a wrapper around `ioctl` that is somewhat more type-safe.

Most `ioctl`s have no or little documentation. You'll need to scrounge through
the source to figure out what they do and how they should be used.

Example
=======

```rust
#[macro_use]
extern crate ioctl;

ioctl!(none drm_ioctl_set_master with b'd', 0x1e);
ioctl!(read ev_get_version with b'E', 0x01; u32);
ioctl!(write ev_set_repeat with b'E', 0x03; [u32; 2]);

fn main() {
    let mut x = 0;
    let ret = unsafe { ev_get_version(0, &mut x) };
    println!("returned {}, x = {}", ret, x);
}
```
