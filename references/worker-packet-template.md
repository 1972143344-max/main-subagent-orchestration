# Worker Packet Template

Use this template only when it helps assign a bounded implementation packet to a sub-agent. It is a reference shape, not a required prompt wrapper.

## Packet Header

- Goal:
- Ownership:
- Files or modules allowed to change:
- Files or modules that are out of scope:
- Required context already checked by main agent:

## Constraints

- Stay inside the assigned write boundary.
- You are not alone in the codebase. Do not revert others' edits.
- Do not silently widen scope.
- If a dependency is missing or another packet blocks you, say so clearly.

## Implementation Tasks

1. Make the assigned change with scope discipline. Small adjacent fixes, supporting tests, or local refactors inside the ownership boundary are acceptable when they clearly improve the packet.
2. Add or update tests that prove the packet-level behavior.
3. Self-review the diff for correctness and regressions.

## Validation

- Required tests or commands:
- Any manual verification expected:

## Return Format

The main agent may request the return in a different shape if the task would benefit from it. Preserve the substance even when the wording or order changes.

- Summary of what changed:
- Files changed:
- Tests run:
- Self-review findings and fixes made:
- Residual risks or blockers:
