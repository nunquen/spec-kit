---
description: Create a deployment plan and rollout checklist based on the implementation plan and related artifacts.
scripts:
  sh: scripts/bash/check-prerequisites.sh --json
  ps: scripts/powershell/check-prerequisites.ps1 -Json
---

## User Input

```text
$ARGUMENTS
```

You **MUST** consider the user input before proceeding (if not empty).

## Outline

1. Run `{SCRIPT}` from repo root and parse JSON for FEATURE_DIR and AVAILABLE_DOCS. All paths must be absolute. For single quotes in args like "I'm Groot", use escape syntax: e.g 'I'\''m Groot' (or double-quote if possible: "I'm Groot").

2. Load context:
   - REQUIRED: `FEATURE_DIR/spec.md`
   - REQUIRED: `FEATURE_DIR/plan.md`
   - IF EXISTS (from AVAILABLE_DOCS): `research.md`, `data-model.md`, `contracts/`, `quickstart.md`

3. Generate or update `FEATURE_DIR/deployment.md` with a concrete deployment plan. Use this structure:

   ```markdown
   # Deployment Plan: <Feature Name>

   ## Overview
   - Summary of what is being deployed and why
   - Intended environments (dev/staging/prod)

   ## Deployment Model
   - Hosting/runtime (e.g., containers, serverless, VM)
   - Regions/zones and scaling assumptions

   ## Build & Packaging
   - Build commands and artifacts
   - Versioning and tagging strategy

   ## Configuration & Secrets
   - Required environment variables
   - Secret management approach

   ## Data & Migrations
   - Schema changes or migrations
   - Backfill or data integrity steps
   ## Infrastructure Requirements
   - Required services (databases, queues, caches, storage)
   - IaC approach (if any)

   ## Rollout Plan
   - Deployment sequence
   - Progressive rollout/canary strategy (if applicable)

   ## Observability & Alerts
   - Logs, metrics, tracing
   - Alert thresholds tied to success criteria

   ## Validation & Smoke Tests
   - Checklist of post-deploy verification steps
   - Rollback criteria

   ## Rollback Plan
   - Revert steps and data recovery considerations

   ## Open Questions
   - [NEEDS CLARIFICATION: ...] for any missing or risky details
   ```

4. If the user input includes deployment preferences (cloud provider, CI/CD tool, regions, SLOs, etc.), incorporate them explicitly.

5. If the required inputs are incomplete (plan/spec missing), stop and instruct to run `/speckit.plan` first.

## Rules

- Do not invent missing requirements. Use [NEEDS CLARIFICATION: ...].
- Keep commands and paths consistent with the plan.
- Prefer actionable, operator-ready steps over high-level prose.
