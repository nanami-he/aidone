# AIDONE

**Don't trust "done". Ask for evidence.**

AIDONE is a product-facing acceptance protocol for AI-generated code. It defines what an AI coding agent must prove before claiming a task is done.

Use this file when the user does not review code line by line but still needs professional engineering defaults.

## Operating Rule

Before claiming a coding task is done, the agent must report:

1. What changed, in product terms.
2. Which files changed.
3. Which checks were run, with results.
4. Which AIDONE levels apply: P0, P1, and relevant P2.
5. Any remaining risk or unverified path.
6. Manual steps the user can take to verify the result.

Do not say "done", "fixed", "works", "ready", or "passes" without fresh evidence from tests, builds, type checks, lint checks, browser checks, API checks, or a clearly described manual verification.

## Priority Levels

### P0 - Always Required

P0 applies to every non-trivial code change, including prototypes.

### P1 - Default Product Bar

P1 applies to normal product work: features, bug fixes, UI changes, APIs, data flows, auth, payments, settings, admin tools, and anything users may keep using.

### P2 - Relevant For Risk

P2 applies when the task touches production, migrations, external services, background jobs, payments, auth, user data, scale, or long-term maintenance.

Do not add P2 complexity without a clear product reason.

## P0 - Safety And Basic Correctness

### 1. No Secrets In Code

Product risk: leaked API keys, tokens, passwords, database URLs, private certificates, or service credentials can compromise the product and users.

Agent must check:

- No secret, token, password, private key, or real credential was added to code, docs, examples, tests, screenshots, or logs.
- Required secrets are read from environment variables, secret managers, or the project's existing configuration system.
- `.env.example` or equivalent documentation lists required variables without real values when new configuration is added.

User can ask:

- "List any new environment variables or secrets this change needs."
- "Did you add any real credentials or tokens anywhere?"

### 2. No Unauthorised Access

Product risk: users can see, edit, delete, or export data they should not access.

Agent must check:

- Any route, API, page, server action, database query, or admin operation that touches user data has the right authentication and authorisation checks.
- Client-side hiding is not treated as permission control.
- Role, tenant, workspace, account, owner, or organisation boundaries are enforced on the server side.

User can ask:

- "Who is allowed to use this feature?"
- "Where is that permission checked?"
- "What happens if a normal user tries the admin path?"

### 3. User Input Is Validated

Product risk: bad input can cause crashes, corrupted data, injection bugs, broken UI, or unclear user errors.

Agent must check:

- Forms, query params, API bodies, uploaded files, imported data, and webhook payloads are validated before use.
- Invalid input returns a clear, safe error.
- SQL, shell commands, templates, file paths, and generated code are not built through unsafe string concatenation.

User can ask:

- "What invalid inputs did you test?"
- "What does the user see when validation fails?"

### 4. Error Paths Are Real

Product risk: the feature only works on the happy path and fails silently or confusingly in real use.

Agent must check:

- Network, API, database, file, payment, auth, and third-party failures are handled.
- End users see safe, understandable messages.
- Developers get enough diagnostic detail through logs, errors, or request IDs.
- Sensitive internals are not exposed to users.

User can ask:

- "What happens if the request fails?"
- "What happens if the third-party service is down?"
- "What should support or engineering look at when this fails?"

### 5. Verification Evidence Is Required

Product risk: the agent says work is complete based on confidence instead of evidence.

Agent must check:

- Run the relevant project checks: tests, lint, type check, build, format check, API smoke test, browser verification, or the closest available equivalent.
- If a check cannot be run, state why and provide the best alternative verification.
- Do not use old command output as proof.

User can ask:

- "Which exact commands did you run?"
- "What passed, what failed, and what was not run?"

## P1 - Default Product Bar

### 6. User-Facing Text Is Manageable

Product risk: changing language, tone, labels, pricing copy, legal copy, onboarding text, or notifications becomes slow and error-prone.

Agent must check:

- New user-visible text is not scattered through logic when the project has an i18n, messages, copy, dictionary, locale, or content system.
- Buttons, labels, errors, empty states, toast messages, email text, notification text, page titles, navigation, and validation messages count as user-visible text.
- If the project has no i18n or copy system and the change adds meaningful text, create a small central copy structure or explain why it is not appropriate for this task.

User can ask:

- "List the new user-facing text."
- "Where is that text stored?"
- "If we later change language or tone, do we edit one copy/translation area or hunt through components?"

### 7. Loading, Empty, Error, And Success States Exist

Product risk: the UI looks broken whenever data is slow, missing, rejected, or partially available.

Agent must check:

- Any async UI has loading, empty, error, and success states where relevant.
- Empty states explain what happened and what the user can do next.
- Destructive or irreversible actions have confirmation, undo, or clear consequence copy when appropriate.
- Long operations show progress or at least an in-progress state.

User can ask:

- "Show me how to verify loading, empty, error, and success states."
- "What does the user see while waiting?"
- "What does the user see when there is no data?"

### 8. Configuration Is Not Hardcoded

Product risk: changing environments, regions, models, URLs, prices, feature flags, limits, or vendors requires code edits and redeploys.

Agent must check:

- API URLs, service endpoints, model names, feature flags, regions, timeouts, rate limits, pricing constants, and environment-specific values are not hardcoded unless they are true product constants.
- Required configuration fails fast with a clear setup error.
- Defaults are safe and documented.

User can ask:

- "What values would change between local, staging, and production?"
- "Where are those values configured?"

### 9. Data Shape Is Explicit

Product risk: data changes break the product in hidden places.

Agent must check:

- External API responses, database records, form data, webhook payloads, and core business objects have types, schemas, validators, or documented contracts.
- The code does not use broad `any`, unchecked casts, or loose objects to bypass validation.
- Invalid or unknown data is handled deliberately.

User can ask:

- "What data shape does this feature depend on?"
- "What happens if the API returns missing or unexpected fields?"

### 10. Existing Patterns Are Followed

Product risk: the codebase becomes a pile of different styles that only the last agent understands.

Agent must check:

- Follow existing project patterns for routing, state, forms, API calls, styling, tests, logging, errors, and file structure.
- Do not add a new library, framework, state manager, UI kit, validation system, or architecture pattern unless the need is clear.
- If a new dependency is required, explain why the existing stack cannot cover it.

User can ask:

- "Did you follow the existing project pattern?"
- "Did you add any dependency or new convention?"
- "Why was it necessary?"

### 11. Changes Are Scoped

Product risk: a small request turns into a broad rewrite, creating regressions the user did not ask for.

Agent must check:

- Keep the diff focused on the requested outcome.
- Do not refactor unrelated modules.
- Separate behaviour changes from cleanup when possible.
- If touching legacy or poorly tested code, first characterise current behaviour and keep the change small.

User can ask:

- "What changed outside the requested area?"
- "Was any unrelated refactor included?"

### 12. Tests Cover Behaviour

Product risk: tests pass but the product behaviour is still wrong.

Agent must check:

- Add or update tests for new behaviour, bug fixes, validation, permissions, important edge cases, and error paths.
- Prefer tests that describe user-visible or API-visible behaviour.
- For bugs, add a regression test where practical.
- If tests are impractical, provide manual verification steps and explain the gap.

User can ask:

- "Which behaviour is now covered by tests?"
- "What important path is only manually verified?"

### 13. Accessibility Basics Are Covered

Product risk: users cannot operate the product with keyboard, screen readers, low vision, or common assistive tools.

Agent must check:

- Interactive controls use semantic elements where possible.
- Icon-only buttons have accessible labels.
- Inputs have labels and useful validation messages.
- Focus states, keyboard operation, contrast, and reduced-motion expectations are respected.
- Colour is not the only way to understand a state.

User can ask:

- "Can this be used with keyboard?"
- "Do icon buttons and form fields have accessible names?"

### 14. Documentation Is Updated Where Needed

Product risk: the code works, but nobody knows how to configure, use, deploy, or recover it.

Agent must check:

- Update README, setup docs, env examples, API docs, runbooks, or inline help when behaviour, configuration, commands, or workflows change.
- Do not create documentation for obvious internal implementation details.

User can ask:

- "Does this change require setup, deployment, or user-facing instructions?"
- "Where is that documented?"

## P2 - Relevant For Risk

### 15. Compatibility And Migration Are Planned

Product risk: existing users, data, links, integrations, or saved settings break after release.

Agent must check when relevant:

- Database migrations are reversible or have a rollback plan.
- Existing data is migrated or backward compatible.
- Old URLs, API versions, settings, and exported/imported data are handled.
- Feature flags or staged rollout are used for risky changes.

User can ask:

- "What happens to existing users and existing data?"
- "Can we roll this back?"

### 16. Domain Rules Have One Owner

Product risk: the same business rule is copied in UI, API, jobs, and tests, then drifts over time.

Agent must check when relevant:

- Pricing, permissions, eligibility, quotas, status transitions, validation rules, and workflow rules have one authoritative implementation or contract.
- UI mirrors rules for usability but does not become the source of truth.

User can ask:

- "Where is the source of truth for this rule?"
- "Is the same rule copied anywhere else?"

### 17. Background Work Is Recoverable

Product risk: jobs, queues, imports, exports, emails, billing tasks, or AI runs get stuck or duplicate work.

Agent must check when relevant:

- Background jobs are idempotent where possible.
- Retries, timeouts, cancellation, and partial failure are handled.
- Users or operators can see status for long-running work.

User can ask:

- "What happens if this job fails halfway?"
- "Can it be retried safely?"

### 18. External Services Are Isolated

Product risk: vendor changes, downtime, rate limits, or API changes spread through the product.

Agent must check when relevant:

- Third-party APIs are wrapped behind a small adapter or service boundary.
- Timeouts, retries, rate limits, fallback behaviour, and error translation are handled.
- Vendor-specific objects do not leak everywhere unless the project intentionally accepts that coupling.

User can ask:

- "If we change this vendor later, how many places change?"
- "What happens when the vendor times out or rate limits us?"

## Final Delivery Template

Agents should use this format after coding tasks:

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

Filled example:

```md
## Product Result

Settings now lets users export local data as JSON.

## Files Changed

- `src/components/SettingsView.tsx`: adds the export action and user-facing button.
- `src/lib/db/export.ts`: collects local records into a JSON-safe export shape.
- `src/lib/copy.ts`: stores the new Settings copy in the shared copy layer.

## AIDONE Check

- P0 safety/correctness: pass. No secrets added; export only reads local user data.
- P1 product quality: partial. Copy is centralized and build passes, but export error state is still missing.
- P2 risk areas: not applicable for this local-only export path.

## Verification

- `pnpm build`: passed.
- `pnpm lint`: failed on 2 existing files unrelated to this change.
- Manual check: Settings -> Export Data downloads a JSON file.

## Remaining Risk

Clear Data is still a destructive action and should get a second confirmation before broader user testing.
```

## How To Use

For normal work:

```text
Follow AIDONE.md. Use P0 and P1 by default. Apply P2 only where relevant.
```

For fast prototypes:

```text
This is a prototype. Follow P0 only, and call out which P1 items are intentionally skipped.
```

For high-risk work:

```text
Follow AIDONE.md with P0, P1, and relevant P2. Do not claim completion without verification evidence.
```
