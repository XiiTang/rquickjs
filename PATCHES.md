# Boundless patches

This fork carries the patches Boundless needs on the `boundless` branch; `master`
mirrors upstream. It is based on release 0.14.0.

## QuickJS from XiiTang/quickjs

`sys/quickjs` points at the `boundless` branch of `XiiTang/quickjs`, which keeps
QuickJS's stack guard on WASI (see that fork's `PATCHES.md`). Boundless builds
this crate for `wasm32-wasip1` and runs it inside wasmtime, where unbounded
recursion must throw `RangeError` rather than trap the instance.

Regression: Boundless's script runtime test that recurses without end.

## Opt in to the QuickJS heap-denial hook on WASI

Upstream exposes the configured limit but its configured denial follows the
ordinary QuickJS allocation failure path. The WASI binding exposes the hook
from the pinned QuickJS fork so Boundless can trap at that check. An embedder
installs it explicitly. The syntax checker continues to use the default path.

Regression: Boundless's live heap limit test checks the host's typed failure
even when the JavaScript program tries to catch the denial.
