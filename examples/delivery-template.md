# AIDONE Delivery Template

AI coding agents should use this shape after completing implementation work.

```md
## Product Result

[What changed from the user's point of view.]

## Files Changed

- `path/to/file`: [why it changed]

## AIDONE Check

- P0 safety/correctness: [pass/gap with detail]
- P1 product quality: [pass/gap with detail]
- P2 risk areas: [not applicable/pass/gap]

## Verification

- `[command]`: [result]
- Manual check: [steps and result]

## Remaining Risk

[None, or specific unverified paths.]
```

## Compact Version

Use this when the user wants short handoffs:

```md
Evidence: [commands/manual checks and results]
Gaps: [failed checks, skipped checks, or remaining risks]
Manual check: [one or two user-verifiable steps]
```
