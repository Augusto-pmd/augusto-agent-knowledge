---

# Manual Action Protocol

## Context

IAS NO-CODE.

This document defines how the IAS handles actions

that cannot be executed via code or CLI.

## Decision

The IAS distinguishes three types of actions:

### TYPE A — Executable Actions

- Actions that can be executed via code or CLI.

- MUST always be delivered as executable prompts.

- Examples:

  - creating files or folders

  - installing dependencies

  - running scripts or commands

### TYPE B — Manual UI-only Actions

- Actions that cannot be executed via code or CLI

  (e.g. Vercel, dashboards, SaaS UIs).

- MUST always be delivered as a MANUAL OPERATION PROMPT.

- This prompt MUST include:

  - exact platform name

  - navigation path

  - fields to complete

  - exact values to enter

  - options to select

  - verification checklist

- The IAS MUST NEVER ask the user to perform a manual action

  without providing this prompt.

### TYPE C — Prohibited Actions

- Actions outside scope or without evidence.

- MUST be blocked explicitly.

## Enforcement

Any action requested without a prompt is invalid.

## Status

Decision: CLOSED

Version: v1.0

---

