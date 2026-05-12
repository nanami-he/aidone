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

Use this when the user asks for a short handoff, when working in chat or a Slack-style channel, or when the full template would bury the signal. Three lines, in this order.

Template:

```md
Evidence: [commands run and their results; or the manual check performed]
Gaps: [P0/P1/P2 items that failed, were skipped, or remain risky]
Manual check: [one or two steps the user can run to verify]
```

Filled example:

```md
Evidence: pnpm test settings/export → 4/4 pass; pnpm build → ok.
Gaps: P1 empty state for "no settings yet" not implemented; no load test for >10MB exports.
Manual check: sign in, open Settings → Export Data, confirm JSON downloads and contains only your account.
```

Rules:

- Do not drop the `Gaps` line. If nothing is missing, write `Gaps: none`. Silence is not the same as "no gaps".
- Do not replace real commands with vague claims like "tests pass". Name the command and the result.
- If no checks were run, say so explicitly: `Evidence: no automated checks run in this project; manual verification only.`
- Compact does not mean optional. The user can always ask for the full template afterwards.
