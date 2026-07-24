# ADR: <short title>

**Date:** <date>
**Status:** Proposed
**Type:** Greenfield | Brownfield

## Context
- What the user is trying to achieve, in plain terms
- Why now / what triggered this
- (Brownfield only) what exists today that this builds on or changes

## Database Changes
> Omit this section if there are none.

| Table/Field | Change | Notes |
|---|---|---|
| e.g. `users.last_login` | Add column | Nullable timestamp, defaults null |

## API Changes
> Omit this section if there are none.

| Endpoint | Change | Notes |
|---|---|---|
| `POST /api/login` | New | Returns JWT |

## Front End Changes
> Omit this section if there are none.

- One bullet per change, plain language, one line each

## Environment Changes
> Omit this section if there are none.

| Variable/Config | Value/Purpose |
|---|---|
| `AUTH_SECRET` | Signs JWTs |

## Architecture Changes
> Omit this section if there are none.

- One bullet per structural change, one line each

## Open Questions
> Omit this section if there are none.

- Anything left unconfirmed with the user