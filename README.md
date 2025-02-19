![Rust](https://github.com/yewstack/implicit-clone/actions/workflows/rust.yml/badge.svg)
[![Latest Version](https://img.shields.io/crates/v/implicit-clone.svg)](https://crates.io/crates/implicit-clone)
![License](https://img.shields.io/crates/l/implicit-clone)
[![Docs.rs](https://docs.rs/implicit-clone/badge.svg)](https://docs.rs/implicit-clone)
[![LOC](https://tokei.rs/b1/github/yewstack/implicit-clone)](https://github.com/yewstack/implicit-clone)
[![Dependency Status](https://deps.rs/repo/github/yewstack/implicit-clone/status.svg)](https://deps.rs/repo/github/yewstack/implicit-clone)

<!-- cargo-rdme start -->

# ImplicitClone

A library that introduces the marker trait [`ImplicitClone`](https://docs.rs/implicit-clone/latest/implicit_clone/trait.ImplicitClone.html) which allows
reproducing the behavior of the trait [`Copy`][std::marker::Copy] but calls the
[`Clone`][std::clone::Clone] implementation instead and must be implemented in the host
library.

The idea is that you must implement this trait on types that are cheap to clone
([`std::rc::Rc`][std::rc::Rc] and [`std::sync::Arc`][std::sync::Arc] types are
automatically implemented). Then the host library must use the trait
[`ImplicitClone`](https://docs.rs/implicit-clone/latest/implicit_clone/trait.ImplicitClone.html) to allow their users to pass objects that will be
cloned automatically.

This crate is in the category `rust-patterns` but this is actually a Rust anti-pattern. In Rust
the user should always handle borrowing and ownership by themselves. Nevertheless, this pattern
is sometimes desirable. For example, UI frameworks that rely on propagating properties from
ancestors to children will always need to use Rc'ed types to allow every child component to
update. This is the case in React-like framework like Yew.

This crates also provide a few convenient immutable types for handling cheap-to-clone string,
array and maps which you can find in the modules [`sync`](https://docs.rs/implicit-clone/latest/implicit_clone/sync/) and
[`unsync`](https://docs.rs/implicit-clone/latest/implicit_clone/unsync/). Those types implement [`ImplicitClone`](https://docs.rs/implicit-clone/latest/implicit_clone/trait.ImplicitClone.html) and
hold only types that implement [`ImplicitClone`](https://docs.rs/implicit-clone/latest/implicit_clone/trait.ImplicitClone.html) as well. **One big
particularity: iterating on these types yields clones of the items and not references.** This
can be particularly handy when using a React-like framework.

[std::marker::Copy]: https://doc.rust-lang.org/std/marker/trait.Copy.html
[std::clone::Clone]: https://doc.rust-lang.org/std/clone/trait.Clone.html
[std::rc::Rc]: https://doc.rust-lang.org/std/rc/struct.Rc.html
[std::sync::Arc]: https://doc.rust-lang.org/std/sync/struct.Arc.html

<!-- cargo-rdme end -->
