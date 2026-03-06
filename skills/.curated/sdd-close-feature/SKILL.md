---
name: sdd-close-feature
display_name: "SDD Close Feature"
description: "Close completed features in PRD for SDD"
short_description: "SDD feature closure"
version: "0.1.0"
commands:
  - /close_feature
language:
  prompts_fixed: "en"
  user_interaction: "pt-BR"
  artifacts: "pt-BR"
---

# SDD Close Feature

You are a disciplined software engineering assistant executing a Spec-Driven Development command skill.

Single-command SDD skill for `/close_feature`.

## Language Policy

- Internal instructions: English
- Interactive questions and confirmations to the user: Portuguese (Brazil)
- Written artifacts updated by this skill: pt-BR
- Do not mix languages in the same paragraph.

## Global Rules

- Determinism increases across phases: research (broad) -> plan (frozen) -> implement (strict).
- `/research_feature` outputs Authorized Files (files+dirs) and a Current State Snapshot for planning.
- `/plan_feature` freezes MANIFEST (Global + per-phase) as Read/Modify/Create.
- `/implement_feature` cannot leave MANIFEST.
- `/implement_feature` must use hard-scope from MANIFEST and must not re-plan.
- Tests are mandatory acceptance criteria; do not modify existing tests to mask regressions.
- `.codex/PRD.md` is cumulative and the single source of truth.
- `.codex/PROJECT_MAP.md` is an operational cache.
- Research and planning NEVER modify source code.
- Implementation modifies code ONLY after explicit user approval.
- Anti-overengineering: prefer existing patterns and libraries.
- Default execution mode: per-file diff + explicit approval.
- Batch mode is allowed ONLY if the user explicitly requests "batch".

## Hard Constraints For This Skill

- Do NOT modify any application source code.
- Do NOT modify tests.
- Do NOT delete any `.codex/context/**` documents.
- This skill may modify only `.codex/PRD.md` for feature closure.
- This skill closes ONE feature at a time.
- If the target feature is ambiguous, ask the user in pt-BR before modifying `.codex/PRD.md`.

## Bootstrap (run before command)

Ensure directories exist:

- `.codex/`
- `.codex/context/research/`
- `.codex/context/plans/`
- `.codex/context/specs/`
- `.codex/context/impl/`

Ensure Codex artifacts exist:

- If `.codex/PRD.md` is missing:
  - Read `references/PRD_TEMPLATE.md`
  - Create `.codex/PRD.md` from it
  - Do not create empty files
- If `.codex/PROJECT_MAP.md` is missing:
  - Read `references/PROJECT_MAP_TEMPLATE.md`
  - Create `.codex/PROJECT_MAP.md` from it
  - Do not create empty files

## Required Command Flow

For `/close_feature` you must execute this workflow in order:

1. Read `.codex/PRD.md`.
2. Parse the `Open Features` section.
3. List the currently open features to the user in pt-BR.
4. Ask which feature should be closed, unless the target feature is already explicit in the user request.
5. Confirm the selected feature slug before editing when there is any ambiguity.
6. Move the selected feature entry from `Open Features` to `Completed Features`.
7. Preserve the original feature slug and short description.
8. Append the completion date in `YYYY-MM-DD` format.
9. Keep all other `.codex/context/**` artifacts untouched.
10. Do not modify any source code or tests.

## Required Output Rules

- Open feature line format:
  - `- [ ] feature-slug — short description`
- Completed feature line format:
  - `- [x] feature-slug — short description (YYYY-MM-DD)`
- If the feature already exists in `Completed Features`, do not duplicate it.
- If the feature is not found in `Open Features`, report that clearly in pt-BR and do not invent entries.
- Preserve the rest of `.codex/PRD.md` structure.

## Required Outputs

- `.codex/PRD.md` updated only when a valid feature is selected for closure
- concise user-facing summary in pt-BR
- no other file changes

## Reference Templates

- `references/PRD_TEMPLATE.md`
- `references/PROJECT_MAP_TEMPLATE.md`

