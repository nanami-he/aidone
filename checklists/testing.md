# Testing Checklist

Use this when a task adds behaviour, fixes a bug, changes data flow, touches auth, modifies API contracts, or changes user-visible workflows.

## Acceptance Checks

- Tests describe behaviour, not implementation trivia.
- New features include tests for the main success path and important failure paths.
- Bug fixes include a regression test when practical.
- Permissions, validation, and destructive actions are tested when relevant.
- API changes include tests or contract checks for success and error responses.
- UI changes have either automated tests or clear manual verification steps.
- If tests are not practical, the agent states why and gives manual verification steps.
- Existing tests still pass, or failures are reported honestly.

## User Questions

- Which behaviour is covered by tests?
- Which important path is only manually verified?
- What command proves this works?
- Did the test fail before the fix when this was a bug?
