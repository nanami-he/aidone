# AIDONE

**Don't trust "done". Ask for evidence.**

AIDONE is a product-facing acceptance protocol for AI-generated code. It helps founders, product people, designers, creators, and solo builders verify that AI-coded software is safe, maintainable, testable, and ready to hand to users.

The core file is [`AIDONE.md`](./AIDONE.md): a short checklist you can put in a project root and ask your AI coding agent to follow before it claims work is done.

## Before And After

Without AIDONE, an AI agent may end a task like this:

```text
Done. I implemented the settings page and everything should work now.
```

With AIDONE, the same handoff should look more like this:

```text
Product result: settings page now lets users export data and clear local data.
Files changed: SettingsView.tsx, db/export.ts, copy.ts.
AIDONE: P0 passed for secrets and input handling. P1 gap: clear-data action needs a second confirmation. Tests not added because this project has no test runner.
Verification: pnpm build passed. pnpm lint failed with 2 existing errors unrelated to this change.
Manual check: open Settings -> Export Data -> confirm JSON downloads.
Remaining risk: destructive clear-data flow should be tightened before sharing with users.
```

The point is not longer reporting. The point is evidence.

## Why This Exists

AI can write code that runs.

That does not mean the work is acceptable.

A feature can appear to work while still missing basic product engineering defaults:

- user-facing text is hardcoded instead of localizable
- loading, empty, error, and success states are missing
- secrets or URLs are hardcoded
- permissions are only enforced in the UI
- invalid inputs crash the app
- error screens expose stack traces
- tests are missing
- "ship-ready" is claimed without build, lint, or test evidence

AIDONE turns those hidden engineering expectations into explicit acceptance checks.

## Acceptance, Not Code Review

AIDONE is not another engineering best-practices list.

Code review asks: "Is this code well written?"

AIDONE asks: "Can a product owner accept this AI-generated work without reading every line of code?"

That is a different job. AIDONE turns engineering concerns into product-facing questions:

- If we change language later, where does copy live?
- If the API fails, what does the user see?
- If a normal user tries the admin path, what blocks them?
- If the agent says tests pass, which command proves it?
- If a destructive action exists, what prevents accidental damage?

## Who It Is For

Use AIDONE if you:

- vibe code with AI agents
- do not review every line of code yourself
- are a founder, product person, designer, creator, or non-specialist builder
- still need professional software defaults
- want evidence instead of confident completion claims

It is not a replacement for senior engineering review. It is a practical acceptance bar that makes AI agents report risk, evidence, and skipped checks.

## Quick Start

Copy [`AIDONE.md`](./AIDONE.md) into your project root.

Then add this to your `AGENTS.md`, `CLAUDE.md`, Cursor rules, or project instructions:

```md
Before claiming any coding task is done, follow `AIDONE.md`.

Use P0 and P1 by default.
Apply P2 when the task touches production, migrations, external services, background jobs, payments, auth, or user data.
If a check cannot be run, state why and provide the closest available verification.
```

When asking an AI agent to review code:

```text
Review this project with AIDONE. Do not only check whether the feature runs. Report P0, P1, and relevant P2 gaps with file references and verification evidence.
```

When asking an AI agent to implement code:

```text
Implement this using AIDONE. At the end, report files changed, checks run, P0/P1 acceptance status, manual verification steps, and remaining risk.
```

For short replies, ask for the compact handoff:

```text
Use AIDONE, but keep the final handoff compact: evidence, gaps, manual check.
```

Expected shape:

```text
Evidence: pnpm build passed; pnpm lint failed on 2 existing files.
Gaps: no tests; destructive action still needs second confirmation.
Manual check: Settings -> Export Data downloads JSON.
```

## The File Set

```text
AIDONE.md                 English core protocol
AIDONE.zh-CN.md           Chinese core protocol
llms.txt                  LLM-readable project index
examples/AGENTS.md        Example agent instruction
examples/CLAUDE.md        Example Claude Code instruction
examples/review-prompt.md Review prompt
examples/delivery-template.md Final delivery template
checklists/               Optional deep-dive checklists
references/sources.md     Source map and related standards
```

## Priority Levels

| Level | Meaning | Use When |
|---|---|---|
| P0 | Always required | Every non-trivial code change, including prototypes |
| P1 | Default product bar | Normal product work, UI, API, auth, data, settings, workflows |
| P2 | Relevant for risk | Production, migrations, background jobs, external services, scale |

## Related Standards

AIDONE fits into the emerging set of AI-era project files:

| File | Audience | Purpose |
|---|---|---|
| `README.md` | Humans | What this project is |
| `AGENTS.md` | Coding agents | How to work in this repository |
| `DESIGN.md` | Design agents | How the product should look and feel |
| `llms.txt` | External LLMs | Which project docs matter |
| `AIDONE.md` | AI coding agents and reviewers | What "done" must prove |

## License

MIT
