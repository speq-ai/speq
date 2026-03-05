<p align="center">
  <img src="assets/banner.svg" alt="enthropic"/>
</p>

<p align="center">
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-MIT-lightgrey.svg" alt="MIT"/></a>
  &nbsp;
  <a href="SPEC.md"><img src="https://img.shields.io/badge/spec-v0.2-ffafff?style=flat&labelColor=0f0f1a" alt="Spec v0.2"/></a>
</p>

---

A `.enth` file is the architectural contract of your project. You write it once. Every AI session reads it before writing a single line of code. Same spec, two machines, two agents: architecturally identical output.

Natural language is inherently ambiguous — the same words mean different things across models, sessions, and prompts. A `.enth` file has a grammar, a parser, and a validator. It cannot be misread.

<p align="center">
  <a href="SPEC.md"><img src="https://img.shields.io/badge/-Read%20the%20Spec-ffafff?style=for-the-badge&labelColor=0f0f1a" alt="Read the Spec"/></a>
  &nbsp;&nbsp;
  <a href="https://github.com/Enthropic-spec/enthropic-tools"><img src="https://img.shields.io/badge/-CLI%20Tool-3a3a5c?style=for-the-badge&labelColor=0f0f1a" alt="CLI Tool"/></a>
</p>

## Why

Vibe coding generates entropy. The AI fills every undeclared decision arbitrarily — naming, layers, security — differently every session. Enthropic collapses that space upfront.

The root cause of AI inconsistency is not the model. It's the missing source of truth.

## Examples

Annotated `.enth` files covering different domains and complexity levels: [examples/](examples/)

---

This project is in early development. The format already works, but there is meaningful work left on semantic precision and completeness. Criticism and contributions are very welcome — read [CONTRIBUTING.md](CONTRIBUTING.md) to get involved.
