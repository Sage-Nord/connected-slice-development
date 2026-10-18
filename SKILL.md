---
name: connected-slice-development
description: Use when adding or changing behavior in a slow-building application and considering an independently buildable module or package (SwiftPM, Gradle/KMP, TypeScript projects, Rust workspaces).
---

# Connected Slice Development

## Overview

A connected slice is one small, host-visible production behavior, not an isolated prototype. Give it an independently buildable core only when there is a real boundary. Wire and test its actual host path early; core tests alone never prove the feature works.

## When to Use

- New host-independent parsers, state machines, validation, or business rules.
- Changes to an existing independent unit: **reuse it**, not a new package per ticket.

**Keep the slice in the host** for UI, lifecycle, wiring, or other behavior without an honest independent contract. Test the smallest real host target; do not invent protocols or copy host models for packaging.

## Workflow

1. **Trace the real seam.** Follow affected host input, consumer, and visible behavior. Start with an expected-behavior example or test; ask about unknown intent. Define the code interface: source units, valid states, types, sync/async, errors, cancellation, and threading when relevant. No speculative Markdown spec is required.
2. **Choose the production unit.** Reuse the module for an existing capability; create a package only for a genuinely independent one. Follow repository build conventions. The host must consume the *same source*. Filtering tests need not filter compilation.
3. **Build the core when useful.** Use enums, newtypes, and validating constructors to exclude structural invalid states; inject narrow dependencies, not the app. See a focused test fail for the intended reason before making it pass. Types cannot prove raw units, time-dependent facts, or unknown business rules.
4. **Connect early.** At the first working behavior, wire the **real host adapter and consumer** where applicable; test mapping, invalid inputs, and effects there. For host-only slices, test the real host path directly. Never defer integration or substitute a fake. If no small host check exists, report that gap.
5. **Use two loops when applicable.** Run core tests during edits; rerun real adapter and affected consumer tests for mapping, interface, or behavior changes, **even with unchanged public types**. Check platform behavior on that platform; check release linking if packaging changes. Build graphs do not select behavior tests.
6. **Report the boundary.** Give the production unit, interface, exact core and host commands/results, platform or release checks, and gaps. Do not promise fixed test times, conflict-free merges, or faster CI.

## Quick Reference

| Ecosystem | Independent loop | Connected check |
|---|---|---|
| SwiftPM | `swift test` in the package | Real app adapter test; `--filter` may still compile other targets |
| Gradle/KMP | Focused module or standalone build | Host mapping and affected platforms, not JVM alone |
| TypeScript | `tsc -b` plus focused tests | Consumer tests even with unchanged `.d.ts` |
| Rust | `cargo test -p capability` | Dependent tests; rebuilds may include dependents |

## Example

`SeatAccess` tests `status(until: Date, at: Date)`. At `now = 3s`, the host's raw `deadlineMs = 2_500` means `.expired`. Interpreting it as seconds compiles and passes core tests, but fails the **real adapter** test.

## Red Flags

New package for the same capability; copied host code; fake-only or deferred host check; package-green reported as app-green; platform behavior checked elsewhere; a new test never observed failing for its target bug.
