# lib-mock

Spies, stubs and mock records for the handl test ecosystem — a
vitest/jest-inspired surface in pure handl.

## Layout

| path | role |
| --- | --- |
| `src/mock.hnl` | the library |
| `src/mock.test.hnl` | the .hnl test suite |
| `pkg.hnl` | package manifest — `modules: ["mock"]` |
| `scripts/hnl-gate.sh`, `ci.sh` | the gate / bupha entry |

## Contract

- Handl values are immutable (ADR-0072): a spy **threads its journal**
  — `mock.call(s, args)` returns `{ value, spy }`; hand the returned
  `spy` to the next call and to the inspectors.
- Surface: `spy`/`stub`/`mock`/`override`/`call`/`calls`/
  `call_count`/`called`/`called_with`/`nth_call`/`last_call`/
  `results` plus `assert_*` helpers returning `lib-test`-shaped
  ok/err Result records. See README for the full list.
- No Rust crate; pure `.hnl`.

## Verification

```bash
HNL=../host-cli/target/debug/hnl scripts/hnl-gate.sh
```

## License

MIT.

<!-- myco:agent-awareness -->

## Agent awareness

- Session start / any work discussion: run `myco status --prompt`
  before proposing next steps — know the board first.
- User describes new work → offer to capture it (`myco todo add`,
  `issue new`, `proposal new`). Don't wait to be asked.
- Picking up a task: `myco resume <ID>` is the briefing.
- Wrapping up: `myco handoff <ID>` anything resumable; keep statuses
  honest (claimed → in-progress → blocked/done).

<!-- /myco:agent-awareness -->
