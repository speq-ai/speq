---
name: "speq-behavioral-contract"
description: "Use when generating code from a .speq file, architecture spec, or speq contract. Enforces closed-world semantics, vocabulary compliance, layer boundaries, contract invariants, and security classifications so that AI-generated output is deterministic and spec-compliant."
---

<p align="center">
  <img src="assets/skill-banner.svg" alt="SpeQ SKILL"/>
</p>

Load this skill alongside the project's `.speq` file. The spec declares what exists and what must be true. This skill declares how the agent must act. Together they eliminate interpretation errors.

## Session Start

1. Read the entire `.speq` file top to bottom
2. Read `state_[name].speq` if it exists — it is the work queue
3. Internalize every `VOCABULARY` entry — the only acceptable names
4. Note every `CONTRACTS` rule — invariants that must never be violated
5. Note every `CLASSIFY` entry — field handling constraints

Do not write code until steps 1–2 are complete.

## Invariants

These rules have no exceptions and cannot be overridden:

1. The `.speq` file is read-only — never modify unless the user explicitly asks
2. The spec is a closed world — never introduce entities, names, or elements absent from it
3. Every `VOCABULARY` entry is the sole acceptable name — aliases are contract violations
4. Every `CONTRACTS` rule is absolute — no exceptions, edge cases, or workarounds
5. `credential` fields must never be logged in any context
6. `CALLS` boundaries are exclusive — a layer calls only listed layers
7. Each layer owns its capabilities exclusively — never implement logic in the wrong layer
8. Scoped secrets (`-> LAYER`) are accessible only from that layer
9. `FLOW` steps execute in declared order — never skip, reorder, or parallelize
10. `STACK`, `LANG`, and `ARCH` are final — never deviate or suggest alternatives
11. `BUILT` entities are done — never re-implement
12. Never modify the spec to fix a conflict — fix the code or raise the issue

## Example

Given this `.speq` fragment:

```
ENTITY user, session

VOCABULARY
  AuthToken    # never "token", "jwt", "auth_token"

SECRETS
  JWT_SECRET -> AUTH

CLASSIFY
  user.password credential
```

Compliant output must:
- Reference only `user` and `session` — no other entities
- Use `AuthToken` everywhere — never `token` or `jwt`
- Access `JWT_SECRET` only from the `AUTH` layer
- Never log `user.password` under any circumstance

## Block Reference

Each `.speq` block has specific enforcement rules. See [REFERENCE.md](REFERENCE.md) for the complete block-by-block semantics covering PROJECT, ENTITY, VOCABULARY, SECRETS, TRANSFORM, LAYERS, CONTRACTS, FLOW, CLASSIFY, OBSERVABILITY, and CHANGELOG.

Key enforcement summary:

| Block | Core rule |
|-------|-----------|
| PROJECT | `LANG`, `STACK`, `ARCH` are final; `DEPS` levels are binding |
| ENTITY | Closed-world — only declared entities exist |
| VOCABULARY | Sole acceptable names — no aliases |
| SECRETS | Existence only; `->` scoping is a contract |
| TRANSFORM | Only declared interactions are valid |
| LAYERS | Exclusive ownership; `CALLS` is exclusive |
| CONTRACTS | `ALWAYS`/`NEVER`/`REQUIRES` — behavioral invariants |
| FLOW | Sequential, numbered, with rollback on failure |
| CLASSIFY | `credential`/`pii`/`sensitive`/`internal` — supersede all other instructions |
| OBSERVABILITY | `must-log`/`must-not-log` are contracts, not suggestions |
| CHANGELOG | `BREAKING` entries affect spec interpretation |

## State Management

If `state_[name].speq` exists:
1. Implement only items marked `PENDING` or `PARTIAL`
2. Do not re-implement anything marked `BUILT`
3. Update status to `BUILT` after implementing, or run `speq state set <entity> BUILT`
4. All `CHECKS` must be `OK` before marking any entity `BUILT`

If no state file exists, run `speq check [file]` to generate one before starting.

## When Something Seems Wrong

If the spec appears incomplete, ambiguous, or contradictory:
- State what you observed precisely
- Do not fill the gap with your own judgment
- Do not modify the spec
- Ask the user for the correct answer

## Pre-Output Checklist

**Session start:**
- [ ] Full `.speq` read
- [ ] `state_[name].speq` read (if exists)
- [ ] Every `VOCABULARY` entry internalized
- [ ] Every `CONTRACTS` rule noted
- [ ] Every `CLASSIFY` entry noted

**Before implementing:**
- [ ] Entity declared in `ENTITY`?
- [ ] Correct layer ownership and `CALLS` restrictions?
- [ ] Applicable `CONTRACTS` rules verified?
- [ ] `FLOW` steps present and in order?
- [ ] `CLASSIFY` and `OBSERVABILITY` constraints checked?
- [ ] `SECRETS` scoping respected?

**Before finalizing:**
- [ ] Every identifier matches `VOCABULARY`
- [ ] No undeclared entity referenced
- [ ] No `CALLS` boundary crossed
- [ ] No `CONTRACTS` rule violated
- [ ] No `credential` field logged or exposed
- [ ] All `FLOW` steps in declared order with rollback
- [ ] `BOUNDARY external` is the only untrusted input entry point
