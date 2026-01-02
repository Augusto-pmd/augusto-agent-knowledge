---

# IAS Project Bootstrap Pattern

## Purpose

Define the mandatory baseline for any project governed by the IAS.

## Applies To

- Any new or existing project declared as IAS-governed.

## Mandatory Tooling

Every IAS project MUST:

- Use npm scripts as the only task runner.

- Integrate Prettier as a manual formatter.

- Use ESLint in manual mode only.

- Prohibit hooks, on-save actions, and implicit execution.

## Tool Governance

- All tools are manual.

- All execution requires explicit IAS prompt.

- No project may introduce automatic correction.

## Repository Rules

- Tooling lives in the project repo.

- Governance lives in augusto-agent-knowledge.

- The IAS Engine repo remains tooling-free.

## State Awareness

- EXEC: tools allowed as defined.

- AUDIT: read-only tools only.

- CLOSE: no execution.

## Non-Negotiables

- No deviation without explicit IAS decision.

- No project-specific exceptions.

## Status

Pattern: ACTIVE

Version: v1.0

---

