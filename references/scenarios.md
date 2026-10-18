# Work Scenarios

Use the [core loop](../SKILL.md#work-one-slice) for every route. A *host* is the real entry point and consumer: an app screen, service endpoint, CLI command, or public library API. It need not be a large application.

## Start a new project

1. Pick the first behavior a user or caller can observe. Create the smallest real host entry point and its focused test using the project's build conventions; a runnable skeleton is enough. Do not start with a library graph and postpone the app.
2. Keep UI, lifecycle, delivery, and wiring in that host. Extract a core only after a host-independent capability and a useful isolated test seam are clear. Do not prescribe a package count or copy host models into a "sandbox."
3. Connect that first behavior through the production source and its actual adapter. Run the real host check; add platform and production checks appropriate to what the project already ships. Grow new units as capabilities stabilize, not for each ticket.

Example: for "show eligible seats after login," establish the login-to-seats route first. If the API supplies a raw deadline but neither its units nor the eligibility policy, build the reversible host path and request those facts; do not fabricate eligibility logic.

## Build a new feature

Trace input through the consumer to a visible result. Write a success example and one meaningful failure, boundary, or cancellation example. Reuse the capability's unit if it exists; otherwise decide whether the rule can stand independently. Wire and test the first real host behavior before elaborating the core. For a risky rollout, use existing flags or migration conventions rather than claiming a green package makes deployment safe.

## Change an existing feature

Identify current consumers and behavior that must remain, then state the changed behavior with an old-case and new-case example. Keep the change in the existing unit; check host mapping and affected consumers even for an implementation-only edit or unchanged API. If persisted data, public APIs, or deployment formats change, follow the project's compatibility and migration process. A type-compatible change can still break callers' expectations.

## Fix a bug

Reproduce the reported symptom on the real path, or establish the closest safe reproduction when production-only conditions prevent a local one. Find whether the fault is in input mapping, core logic, orchestration, platform effects, or the caller. Add the smallest check that fails **because of that fault**; for a boundary bug, make it exercise the production adapter and consumer. Fix the cause in its owning unit, rerun the reproduction and affected checks, and report any unreproduced or untested behavior. Do not call a test "red" because its label says so: observe the failing assertion before the fix.

## Deliver a business ticket

Turn the ticket into one or two observable input-to-outcome examples, including a counterexample. Confirm who owns the rule, what errors or cancellations do, and any dates, units, or authorization conditions. Ask for a decision when multiple behaviors fit; record the answer or explicit assumption in the ticket or PR. While blocked, do only reversible work that does not bake in the guessed rule. Short examples replace a *speculation gate*, not binding contracts, product decisions, or required design reviews. Once behavior is settled, choose its owner and run the same core-and-host checks as any other slice.
