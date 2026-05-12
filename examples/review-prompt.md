# AIDONE Review Prompt

Use this prompt when asking an AI agent to review an AI-coded project.

```text
Review this project using AIDONE.md.

Do not only check whether the feature runs. Check whether the work is acceptable to hand to users.

Report:
1. P0 safety and correctness gaps.
2. P1 product quality gaps.
3. Relevant P2 production or maintenance risks.
4. File references for each finding.
5. Verification commands run and results.
6. Checks that could not be run.
7. Recommended fix order.

Prioritize security, data loss, auth, secrets, destructive actions, missing error paths, missing verification, hardcoded user-facing text, and missing tests.
```
