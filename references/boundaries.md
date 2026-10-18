# Boundaries and Contracts

## Choose the smallest honest unit

| Question | Decision |
|---|---|
| Does this capability already have a production module? | Change it there; do not create a package per ticket. |
| Can the behavior be specified and tested without app UI, lifecycle, or a copy of host models? | A focused core may help. If not, keep the slice in the host. |
| Does the build structure let this capability run without unrelated host work? | Prefer an existing focused target; use a standalone build only for an independent capability. |
| Can the real host consume that source and check its adapter early? | Make that path part of the slice. A fake consumer is not integration proof. |

A buildable unit is a **capability boundary**, not an OS security boundary or a unit of ticket ownership. New packages add version, dependency, CI, and ownership work; a module within an existing build may already provide a useful loop. No package count, speed target, or conflict-free merge is guaranteed.

## Make the contract explicit

Trace **raw input -> host validation/mapper -> typed core (if useful) -> host consumer/effect**. The host's composition point injects real storage, network, clock, UI, or other capabilities as needed; the core depends on narrow contracts, not the entire app. Put the rule with its owner: mapping in the adapter, reusable domain decisions in the core, UI/lifecycle effects in the host. Test host orchestration where its choices determine behavior.

Check the interface in both directions:

- Inputs: raw units, encoding, missing/invalid values, time source, time zones, and boundary inclusivity. Convert once at the boundary; use validated types so the core need not handle impossible structural states.
- Operations: sync/async shape, thrown errors versus returned outcomes, cancellation propagation, ordering, and threading or actor requirements. Do not turn a failed mapping or request into a success-shaped default.
- Outputs: explicit states and errors, observable host effects, persistence or network changes, and compatibility with existing consumers.

Types can reject an invalid *shape*. They cannot tell whether an unlabelled integer is seconds or milliseconds or what a business rule should do at expiry. A mapper can compile while being wrong.

## A real adapter test

In a small macOS SwiftPM fixture, `SeatAccess` core tests covered active, grace, and expired states. The actual host receives seconds for one timestamp and milliseconds for another:

```swift
let expiresAt = Date(timeIntervalSince1970: seat.expiresAtSeconds)
let graceDeadline = Date(
    timeIntervalSince1970: TimeInterval(seat.graceDeadlineMilliseconds) / 1_000
)
let status = SeatAccess.status(
    expiresAt: expiresAt, graceDeadline: graceDeadline, now: now
)
```

The host test uses `expiresAtSeconds = 1_700_000_000`, `graceDeadlineMilliseconds = 1_700_000_060_000`, and `now = 1_700_000_061` seconds; it expects `.expired`. Removing `/ 1_000` still compiles and leaves every core test green, but the host test returns `.grace` and fails. Restore the conversion and rerun the **real** host test. This demonstrates the seam in that tiny fixture, not a universal build-time result.
