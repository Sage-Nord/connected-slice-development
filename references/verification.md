# Verification: Fast Feedback and Real Behavior

## Keep two graphs separate

The **declared build graph** decides what must be compiled or rebuilt. The **behavior/consumer graph** decides which outcomes need retesting. They overlap, but are not interchangeable: a core implementation may change while its public signature or TypeScript `.d.ts` stays identical. A consumer may need a behavior test even when the build tool correctly skips recompiling it.

Declare real build inputs and dependencies; missing edges can make cached or incremental results misleading. Do not select behavior checks solely from an unchanged API, a passing core suite, or an "affected" build list. See [Bazel's dependency model](https://bazel.build/concepts/dependencies) for the actual-versus-declared distinction.

## Run the smallest trustworthy ladder

1. **Expected behavior.** Name the input, observable outcome, and one counterexample. For a bug, reproduce the original symptom. Watch the new check fail for the intended fault, not a missing fixture or typo. If a safe reproduction is impossible, state what was observed instead of claiming a verified red test.
2. **Fast loop.** Test/compile the independent core if there is one; for host-only work, use a focused real host check. Iterate there without running unrelated targets after every edit.
3. **Connected loop.** At the *first* working behavior, exercise the real host path: its production adapter and affected consumer where present. Assert raw units, invalid input, error/cancellation semantics, and relevant side effects. Rerun this check for host-visible changes even if types did not change. A fake may help a core unit test but cannot replace the real boundary check.
4. **Affected platform and delivery.** Run checks for the actual UI/lifecycle, persistence, network, concurrency, migration, or deployment behavior changed. A macOS or JVM test does not establish iOS behavior. Check production packaging/linking when changed; follow existing mandatory CI, security, and release gates. Expand to broader suites when shared behavior or risk warrants it.

Test **the test**: a previously passing check is not proof it would catch the fault. Confirm the changed check failed before the fix (or safely reintroduce the fault and watch it fail), then passed after the fix. Do not manufacture a red run in a log or trust a success report without its result.

## If a connected check is expensive

Look for the existing host command in CI, test scripts, manifests, or project schemes. Run the smallest real host test target or add a thin test around the **production** adapter and consumer. A test that copies host mapping into a standalone fixture does not verify that mapping. If no small real-host check exists, run the available host check when feasible and report its cost; if it cannot run, explicitly leave host behavior unverified. Do not declare the feature complete on core tests alone or waive required release checks because the deadline is near.

## Report the actual boundary

State the behavior checked, core and host commands/results **when applicable**, affected platform/production checks, and remaining gaps. For example: "Core: 3 passed; real host adapter: 1 passed; iOS UI: not run; release build: not run." That is evidence for a core and a host mapper, **not** a claim that the iOS app is ready.
