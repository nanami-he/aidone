# Security Checklist

Use this when a task touches auth, user data, payments, secrets, admin tools, files, webhooks, external services, or Electron/native bridges.

## Acceptance Checks

- No real secrets, tokens, private keys, passwords, database URLs, or credentials are committed.
- Required secrets come from environment variables, secret managers, or the existing configuration system.
- Access control is enforced on the server side or trusted boundary, not only in the UI.
- User, tenant, workspace, account, organisation, and role boundaries are explicit.
- User input is validated before use.
- SQL, shell commands, file paths, templates, and generated code are not built through unsafe string concatenation.
- Error messages shown to users do not expose stack traces, tokens, internal paths, database details, or provider internals.
- Destructive, financial, permission-changing, or bulk actions have confirmation or an equivalent guard.
- Logs do not include secrets or sensitive personal data.
- New dependencies are justified and checked with the project's normal dependency/security workflow.

## User Questions

- Who can use this feature?
- Where is that permission enforced?
- What bad input was tested?
- What happens if a normal user tries the privileged path?
- Are any secrets or tokens introduced?
- What does the user see when the operation fails?
