# Boundless patches

This fork carries the patches Boundless needs on the `boundless` branch; `master`
mirrors upstream. It is release 0.14.0 with one change.

## QuickJS from XiiTang/quickjs

`sys/quickjs` points at the `boundless` branch of `XiiTang/quickjs`, which keeps
QuickJS's stack guard on WASI (see that fork's `PATCHES.md`). Boundless builds
this crate for `wasm32-wasip1` and runs it inside wasmtime, where unbounded
recursion must throw `RangeError` rather than trap the instance.

Regression: Boundless's script runtime test that recurses without end.

## Enable the private Boundless WASI allocation hook

The QuickJS gitlink also carries the optional terminal allocation hook. Enable
it with `boundless-terminal-allocation` on the Boundless Program guest's
rquickjs dependency. This define only applies to the WASI build. Upstream
builds do not require an application import to report memory exhaustion.

Boundless's private guest ABI provides the import. The host terminates and
reports from the Instance owner at the original refusal, including when
QuickJS would otherwise catch or absorb the allocator's null. See the
QuickJS patch record for the allocator and bootstrap witnesses.
