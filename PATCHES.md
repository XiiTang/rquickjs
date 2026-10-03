# Boundless patches

This fork carries the patches Boundless needs on the `boundless` branch; `master`
mirrors upstream. It is release 0.14.0 with one change.

## QuickJS from XiiTang/quickjs

`sys/quickjs` points at the `boundless` branch of `XiiTang/quickjs`, which keeps
QuickJS's stack guard on WASI (see that fork's `PATCHES.md`). Boundless builds
this crate for `wasm32-wasip1` and runs it inside wasmtime, where unbounded
recursion must throw `RangeError` rather than trap the instance.

Regression: Boundless's script runtime test that recurses without end.
