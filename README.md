# Connected Slice Development

**Small real features. Fast feedback. Honest integration.**

A connected slice reaches a real host consumer. Its core is independently buildable only when that boundary makes sense. The aim is to learn quickly without confusing “package tests pass” with “the app works.”

- Start with one observable behavior and its real host path.
- Reuse an existing module; add a package only for an independent capability.
- Test the core quickly, connect the real adapter early, and retest affected consumers when behavior changes.
- Keep host-only UI and lifecycle changes in the host.

## Install

```sh
npx skills add Sage-Nord/connected-slice-development
```

Read [the skill](SKILL.md) for the full workflow and SwiftPM, Gradle/KMP, TypeScript, and Rust checks.
