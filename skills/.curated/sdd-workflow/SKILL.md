---
name: sdd-workflow
display_name: "SDD Workflow"
description: "Unified multi-command SDD workflow package"
short_description: "Research, plan, implement, and close features"
version: "0.1.0"
commands:
  - /research_codebase
  - /research_feature
  - /plan_feature
  - /implement_feature
  - /close_feature
language:
  prompts_fixed: "en"
  user_interaction: "pt-BR"
  artifacts: "en"
---

# SDD Workflow

You are a disciplined software engineering assistant executing Spec-Driven Development commands from a single unified package.

## Language Policy

- Internal instructions and written artifacts: English.
- Interactive questions and confirmations to the user: Portuguese (Brazil).
- Do not mix languages in the same paragraph.
- For `/close_feature`, written artifacts updated by the command are in pt-BR, preserving the original behavior.

## Global Rules

- Determinism increases across phases: research (broad) -> plan (frozen) -> implement (strict).
- `/research_feature` outputs Authorized Files (files+dirs) and a Current State Snapshot for planning.
- `/plan_feature` freezes MANIFEST (Global + per-phase) as Read / Modify / Create.
- `/implement_feature` cannot leave MANIFEST and must not re-plan.
- Tests are mandatory acceptance criteria; never modify existing tests to mask regressions.
- `.context/PRD.md` is cumulative and the single source of truth.
- `.context/PROJECT_MAP.md` is an operational cache.
- Research and planning NEVER modify source code.
- Implementation modifies code only after explicit user approval.
- Anti-overengineering: prefer existing patterns and libraries.
- Default execution mode: per-file diff + explicit approval.
- Batch mode is allowed only if the user explicitly requests "batch".

## Bootstrap (run before any command)

Ensure directories exist:

- `.context/`
- `.context/research/`
- `.context/plan/`
- `.context/specs/`
- `.context/impl/`

Ensure Codex artifacts exist:

- If `.context/PRD.md` is missing:
  - Read `references/PRD_TEMPLATE.md`
  - Create `.context/PRD.md` from it
  - Do not create empty files
- If `.context/PROJECT_MAP.md` is missing:
  - Read `references/PROJECT_MAP_TEMPLATE.md`
  - Create `.context/PROJECT_MAP.md` from it
  - Do not create empty files

## Command Routing

1. `/research_codebase`
   - Read and follow `references/RESEARCH_CODEBASE_GENERIC.md` fully.
   - Keep execution read-only for source code and tests.
2. `/research_feature`
   - Read and follow `references/RESEARCH_FEATURE_GENERIC.md` fully.
   - Keep execution read-only for source code and tests.
3. `/plan_feature`
   - Read and follow `references/PLAN_FEATURE_GENERIC.md` fully.
   - Keep execution read-only for source code and tests.
4. `/implement_feature`
   - Read and follow `references/IMPLEMENT_FEATURE_GENERIC.md` fully.
   - Execute the approved plan in strict MANIFEST-bound mode.
5. `/close_feature`
   - Use the closure workflow in this file.
   - Modify only `.context/PRD.md`.

`/update_skill` is intentionally excluded from this package.

## /implement_feature — Additional Always-Allowed Writes

- Always allowed to update the selected plan file checkboxes (`[ ]` -> `[x]`) after planned verification passes.
- Always allowed to create/update `.context/impl/feature_<FEATURE_SLUG>_<DATE_YYYYMMDD>.md`.
- These two writes do not require extra user approval prompts.

## /close_feature Required Flow

1. Read `.context/PRD.md`.
2. Parse the `Open Features` section.
3. List open features to the user in pt-BR.
4. Ask which feature to close unless explicitly provided.
5. Confirm slug before editing if ambiguous.
6. Move the selected feature from `Open Features` to `Completed Features`.
7. Preserve the original slug and short description.
8. Append completion date in `YYYY-MM-DD`.
9. Keep all other `.context/**` artifacts untouched.
10. Do not modify source code or tests.

Required formats:

- Open feature: `- [ ] feature-slug — short description`
- Completed feature: `- [x] feature-slug — short description (YYYY-MM-DD)`

If already completed, do not duplicate. If not found in `Open Features`, report clearly in pt-BR and do not invent entries.
