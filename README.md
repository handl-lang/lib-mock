# lib-mock

Spies, stubs and mock records for the handl test ecosystem — a
vitest/jest-inspired surface in pure handl.

Handl values are immutable (ADR-0072 deterministic dataflow): there is
no mutable cell a spy could append to behind the caller's back. A spy
therefore **threads its journal** — `mock.call(s, args)` returns
`{ value, spy }`, where `value` is the impl's result and `spy` is the
updated record carrying the new call entry. Hand the returned `spy` to
the next call and to the inspectors.

## Surface

```text
mock.spy(f)               wrap an arrow            -> { target, value, calls }
mock.stub(v)              constant-returning spy   -> { target: null, value, calls }
mock.mock(rec)            record of arrows -> record of spies
mock.override(m, n, f)    swap one member of a mock record
mock.call(s, args)        -> { value, spy }   (args is an Array)
mock.calls(s)             the journal: [ { args, result }, ... ]
mock.call_count(s)        number of recorded calls
mock.called(s)            Bool
mock.called_with(s, args) Bool — any call whose args == args
mock.nth_call(s, n)       journal entry (0-based) or null
mock.last_call(s)         last journal entry or null
mock.results(s)           array of recorded results, oldest first
mock.assert_called(s)     ok/err Result record (lib-test shape)
mock.assert_not_called(s)
mock.assert_call_count(s, n)
mock.assert_called_with(s, args)
```

The `assert_*` helpers mint the same canonical `__ctor__` `data.Result`
records lib-test's `assert.*` helpers produce (`ok`/`err`, fields
`message`/`assertion`/`expected`/`actual`), so `match r { ok(v): …,
err(e): … }` dispatches identically.

## Layout

```text
src/mock.hnl        the library (arrow dialect, ~> exports)
src/mock.test.hnl   the .test.hnl suite
pkg.hnl             package manifest — modules, deps, description, license
workflows/gate.yaml               bupha entry (handl-gates)
scripts/hnl-gate.sh the gate
```

## Gate

```bash
HNL=../host-cli/target/debug/hnl scripts/hnl-gate.sh
```

or with the dogfood build:

```bash
HANDL_EXT_DIR=../.ci-target/dogfood/release \
  ../.ci-target/dogfood/release/hnl test src/mock.test.hnl
```

## License

MIT.
