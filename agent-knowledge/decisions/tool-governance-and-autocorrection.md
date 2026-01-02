---

# Tool Governance and Autocorrection Protocol

## Context

IAS NO-CODE.

This document defines how tools are governed inside the IAS.

## Decision

- Integrate Prettier as a manual formatting tool.

- Allow ESLint usage only in manual mode.

- Treat npm scripts as explicit IAS runners.

- Prohibit any automatic execution, hooks, or on-save actions.

## Tool Usage by IAS State

### EXEC

Allowed:

- npm run dev

- npm run build

- npm run start

- next lint

- next lint --fix (explicit approval required)

- prettier --write (explicit IAS order required)

- npx prisma migrate

- npx prisma generate

- npx prisma db seed (critical runner)

Prohibited:

- hooks

- automatic fixes

- on-save execution

### AUDIT

Allowed:

- npm run build

- next lint (read-only)

- tsc --noEmit

- code inspection

Prohibited:

- any --fix

- prettier --write

- database modifications

- seeds

- migrations

### CLOSE

- No technical execution allowed.

## Autocorrection Model (Conceptual Only)

- Detection phase (lint / typecheck).

- Proposal phase (IAS suggests commands).

- Execution only after explicit human approval.

- Mandatory re-audit after execution.

## Explicit Non-Goals

- No self-healing

- No silent fixes

- No automatic commits

- No implicit execution

## Status

Decision: CLOSED

Version: v1.0

---

