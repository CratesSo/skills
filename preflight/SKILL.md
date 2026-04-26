---
name: preflight
description: "Run production-readiness preflight checks across security, database, deployment, and code."
---
Run a launch preflight audit. Verify production readiness across security, database, deployment, and code. Use repo evidence first, then ask only for facts that cannot be discovered locally.

# WORKFLOW

1. Inspect repo before asking questions:
   - Identify frontend, backend, API routes, auth/session code, database access, deployment config, environment docs, and package manager.
   - Prefer targeted reads/searches over broad scans.
   - Don't mark anything `PASS` without evidence.
2. Audit every checklist item below.
3. Classify each item as:
   - `PASS`: verified with concrete evidence.
   - `FAIL`: evidence shows the check is not satisfied.
   - `UNKNOWN`: not enough local evidence, or production/runtime state is required.
   - `N/A`: not applicable to this project, with a short reason.
4. Ask only for missing production facts that cannot be discovered from repo, such as:
   - production domain
   - hosting provider
   - SSL certificate state
   - firewall exposure
   - production environment variables
   - backup restore proof
   - staging test status
5. Don't fix issues unless user explicitly asks.

## CHECKLIST

### SECURITY

- [ ] No API keys or secrets in frontend code
- [ ] Every route checks authentication (audit all endpoints, not just obvious ones)
- [ ] HTTPS enforced everywhere, HTTP redirected
- [ ] CORS locked to your domain, not wildcard
- [ ] Input validated and sanitized server-side
- [ ] Rate limiting on auth and sensitive endpoints
- [ ] Passwords hashed with bcrypt or argon2
- [ ] Auth tokens have expiry
- [ ] Sessions invalidated on logout (server-side)

### DATABASE

- [ ] Backups configured and tested (test restore, not just backup)
- [ ] Parameterized queries everywhere, no string concatenation
- [ ] Separate dev and production databases
- [ ] Connection pooling configured
- [ ] Migrations in version control, not manual changes
- [ ] App uses a non-root DB user

### DEPLOYMENT

- [ ] All environment variables set on production server
- [ ] SSL certificate installed and valid
- [ ] Firewall configured (only 80/443 public)
- [ ] Process manager running (PM2, systemd)
- [ ] Rollback plan exists
- [ ] Staging test passed before production deploy

### CODE

- [ ] No console.logs in production build
- [ ] Error handling on all async operations
- [ ] Loading and error states in UI
- [ ] Pagination on all list endpoints
- [ ] npm audit run, critical issues resolved

## PARALLEL AUDIT BATCHES

When using subagents, split work by evidence surface, not by checklist heading. Keep each subagent on one non-overlapping surface and synthesize results yourself after all subagents are finished.

### Auth/Session Reviewer

Use `reviewer` for serious launch audits or `reviewer_mini` for smaller apps:

- Every route checks authentication
- Passwords hashed with bcrypt or argon2
- Auth tokens have expiry
- Sessions invalidated on logout server-side
- Rate limiting on auth endpoints

### API/data reviewer

Use `reviewer_mini`:

- Input validated and sanitized server-side
- Parameterized queries everywhere
- Pagination on all list endpoints
- Error handling on all async operations
- Rate limiting on sensitive non-auth endpoints

### Frontend Explorer

Use `explorer`:

- No API keys or secrets in frontend code
- No console.logs in production build
- Loading and error states in UI

### Database explorer

Use `explorer`. Mark production-only facts `UNKNOWN` unless there is repo, provider, or user evidence:

- Backups configured and tested
- Separate dev and production databases
- Connection pooling configured
- Migrations in version control
- App uses a non-root DB user

### Deployment explorer

Use `explorer`. Mark runtime infrastructure facts `UNKNOWN` unless verified from provider state, config, docs, or user confirmation:

- HTTPS enforced everywhere, HTTP redirected
- CORS locked to your domain
- All environment variables set on production server
- SSL certificate installed and valid
- Firewall configured
- Process manager running
- Rollback plan exists
- Staging test passed before production deploy

## SYNTHESIS RULES

- You own final matrix.
- Deduplicate overlapping findings.
- Treat missing production evidence as `UNKNOWN`, not `PASS`.
- Prefer smallest concrete next action for each `FAIL` or launch-blocking `UNKNOWN`.

## OUTPUT CONTRACT

- If no blockers, say so clearly.
- If a section has no items, omit it.
- Use the visible Markdown body below as final output shape:

```md
## BLOCKERS
- **`<check>`**: `<FAIL or UNKNOWN>` - `<evidence or missing evidence>` - `<next action>`

## WARNINGS
- **`<check>`**: `<risk>` - `<evidence>` - `<next action>`

## PASSED
- **`<check>`**: `<evidence>`

## UNKNOWN / NEEDS CONFIRMATION
- **`<check>`**: `<what must be confirmed>`

## NEXT ACTIONS
1. `<smallest concrete action>`
2. `<next concrete action>`
```
