# Team Practice and the Practical Ideal

## What "best" means here

Connected Slice Development is a **strong default for AI-driven teams**, not a theorem that one architecture beats every repository. Its ideal result: the next relevant compiler or test result arrives quickly **and** the first real consumer checks each changed behavior before it spreads. Separate independent core work by condition and time from the host checks that prove integration; do not "solve" slow builds by postponing the only check that can find boundary errors.

This preserves the useful part of isolated-package development without making a package for every change. A small independently buildable core shortens some edit loops; an early host check catches mapper and orchestration failures; a small accepted behavior steers the agent better than a long speculative document. None of these can supply an unknown product decision or eliminate required platform checks.

## Adopt it in an existing team

- Start with one upcoming ticket or bug, its real consumer, and one expected example. Use existing test targets and module boundaries first; extract a capability only when it is independent and the host will consume the same source.
- Let the owning team agree on the interface's raw units, valid states, error/cancellation and async semantics. Keep the host mapper and its test near the consumer; do not let parallel agents quietly define incompatible versions of the same contract.
- Review both sides of a slice: the core decision (if any) and the actual host mapping/effects. In the PR or ticket, record the expected behavior, exact checks run, and gaps in a few lines. Keep required specifications, security reviews, and migration approvals where the organization needs them.
- CI should build the declared affected graph and run the affected **behavior** checks. Shared core changes may require consumer tests even without a signature change. Keep platform and release gates for behavior that depends on them.

For a new project, establish a real entry point and one working behavior before carving out multiple packages. For an existing project, do not reorganize unrelated code to adopt this method. AI agents can work on independent slices in parallel after the shared contract is settled; small slices reduce coordination pressure but do not guarantee conflict-free merges.

## When the loop is still slow or unsafe

If the core command configures or compiles the entire host, find a truly isolated build boundary **only if the capability is real**; do not copy host code to fake speed. If the host check dominates, improve the smallest real host test target rather than replace it with a mock. When a change touches shared infrastructure, schema, security, safety-critical behavior, or multiple platforms, widen verification and follow existing team gates. Record what could not be checked rather than claim an ideal result.

Faster local checks and cache hits are useful; neither is the goal by itself. The team should judge changes by time to a **trustworthy** answer, escaped boundary defects, and the ongoing cost of extra units. Do not make performance measurement a speculative prerequisite for starting a small real slice.
