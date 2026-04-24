# Block-by-Block Reference

Detailed enforcement semantics for each `.speq` block. Loaded on demand by the agent when implementing a specific block type.

## PROJECT

`LANG`, `STACK`, and `ARCH` are final. Do not suggest alternatives or use absent technologies.

`DEPS` levels are binding:
- `SYSTEM` — must exist on the OS before any install step
- `RUNTIME` — deployed to production via the language package manager
- `DEV` — development only, must not be present in production builds

## ENTITY

The only domain entities that exist. Do not create, reference, or infer entities outside this list. All identifiers are `snake_case`. The closed-world assumption is absolute.

## VOCABULARY

Every entry is the sole acceptable name for its concept across all generated code, file names, variable names, comments, and identifiers. Aliases and abbreviations are contract violations.

Scan the entire block and internalize entries before generating code.

## SECRETS

Names declare existence only. Values must never be hardcoded, suggested, or logged.

`-> LAYER_NAME` scoping restricts access to that layer exclusively — any other layer referencing it is a contract violation.

## TRANSFORM

Only declared transforms are valid entity interactions. Each rule `source -> target : action` is the only permitted form.

## LAYERS

Each layer owns its declared capabilities exclusively.

- `CALLS` is exclusive — a layer may call only listed layers
- `BOUNDARY external` marks the single untrusted entry point; all input must be validated before crossing any layer boundary
- `NEVER` is an absolute prohibition scoped to that layer

When unsure which layer owns logic, ask rather than guess.

## CONTRACTS

Every rule is a behavioral invariant with no exceptions:
- `ALWAYS` — must hold in all generated code at all times
- `NEVER` — must be unreachable in every code path
- `REQUIRES` — must be enforced before the operation proceeds
- `entity.*` — applies the constraint to all operations on that entity

Walk through every rule and verify compliance before finalizing output.

## FLOW

Ordered critical sequences, numbered from 1, sequential, no gaps:
- Execute in declared order; never skip, reorder, or parallelize unless explicitly permitted
- `[LAYER_NAME]` pins a step to that layer
- On failure: execute `ROLLBACK` operations in listed order
- `ATOMIC true` means all steps succeed or the flow fully rolls back
- Respect `TIMEOUT` and `RETRY` values exactly

## CLASSIFY

Classifications supersede any other instruction:

| Class | Agent must | Agent must never |
|-------|-----------|-----------------|
| `credential` | Encrypt at rest | Log, expose in responses, errors, traces, or debug output |
| `pii` | Apply data-privacy compliance | Log raw or expose in API responses without explicit declaration |
| `sensitive` | Restrict to owning layer | Include in stack traces or debug output |
| `internal` | Keep within owning layer | Expose outside system boundaries |

A `credential` field is `must-not-log` in every context regardless of `OBSERVABILITY` declarations.

## OBSERVABILITY

Logging and metrics contracts — not suggestions:
- `must-not-log` fields must never appear in logs
- `must-log` fields must always be logged at the declared severity level

## CHANGELOG

Read before implementing. `BREAKING` entries determine how to interpret the current spec and whether previously generated code is still valid.
