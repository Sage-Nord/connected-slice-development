---
name: connected-slice-development
description: Use when starting a software project, implementing or changing a feature, fixing a bug, or delivering a business ticket with AI, especially where slow builds or module boundaries could hide integration defects.
---

# Connected Slice Development

## The idea

**The strongest practical default for AI-driven teams:** deliver one small, real behavior at a time. Use an independently buildable core only for an honest boundary, and connect it to the production host early. Aim for the earliest trustworthy answer at the lowest total cost, not the fewest builds.

Types and isolated tests cannot supply missing business intent or prove host behavior. A slice is production code, not a prototype or OS security sandbox.

## Work one slice

1. **Name the behavior.** Trace the real input, consumer, and observable outcome. Write one concrete expected example, including a failure or edge case. For a bug, reproduce its symptom before changing code. Clarify unknown rules; do not invent a policy to meet a deadline. Skip speculative spec gates; honor required specifications and approvals.
2. **Choose the owner.** Keep UI, lifecycle, and wiring in the host. Reuse an existing unit for an existing capability. Create a new independently buildable unit only for a genuinely independent capability; never one per ticket. The host consumes the **same production source**.
3. **Define the boundary.** Make raw units, valid states, sync/async, errors, cancellation, and threading explicit where relevant. Validate external data at its real adapter; types exclude structural mistakes, not wrong business meaning.
4. **Connect early.** See a focused test fail for the intended reason, then make it pass. Use core tests when available; otherwise the smallest real host check. At the first working behavior, test the **real host path**, including its adapter and consumer where present, not a copy or fake. For a new project, establish a real entry point before extracting packages.
5. **Verify what changed.** Rerun affected consumer tests after behavior changes even if public types did not change. Check affected platforms and production packaging where relevant. Report exact core/host commands, results, and gaps. Package-green is not app-green.

## Choose a reference when needed

Read only the guide relevant to the current decision:

- [Work scenarios](references/scenarios.md): new project, feature, change, bug, business ticket.
- [Boundaries and contracts](references/boundaries.md): where code belongs, typed interfaces, real adapters, worked example.
- [Verification](references/verification.md): two feedback loops, test selection, CI, evidence and gaps.
- [Build-system recipes](references/platforms.md): SwiftPM/Xcode, Gradle/KMP, TypeScript, Rust, Bazel/Nx.
- [Team practice](references/team-practice.md): review, adoption, ownership, tradeoffs.

## Quick decisions

| Situation | Place the work |
|---|---|---|
| Host-bound behavior | Existing host target; test its real path |
| Existing capability | Existing unit plus affected host consumer |
| New independent capability | Small production unit plus early real-host check |

## Red Flags

Packages per ticket; a fake-only host check; made-up policy; an unproven "red" test; package-green called app-green; platform checks on a different platform; promised conflict-free merges or universally faster CI.
