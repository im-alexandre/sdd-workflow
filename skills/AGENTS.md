# Repository Guidelines

## Project Structure & Module Organization

This repository is a curated bundle of Codex skills. The main content lives under [`.curated/`](D:\skills_codex\skills\.curated), with maintained SDD packages centered on `sdd-workflow/` (multi-command workflow) and `sdd-update-skill/` (separate update command). Each skill typically contains:

- `SKILL.md`: command behavior, workflow, and constraints
- `agents/openai.yaml`: agent-facing metadata
- `references/*.md`: templates or operating procedures

Generated working artifacts live under [`.context/`](D:\skills_codex\skills\.context) and should include `PRD.md`, `PROJECT_MAP.md`, and research/plan/impl/specs subdirectories. Do not treat `.context/` as source.

## Build, Test, and Development Commands

There is no traditional build system in this repository. Useful local commands are:

- `git status --short` — inspect pending changes
- `git log -5 --pretty=format:'%h %s'` — review recent commit style
- `Get-ChildItem -Force .curated` — inspect available skills

Operational usage happens through Codex commands defined by the skills, for example `/research_codebase`, `/research_feature`, `/plan_feature`, `/implement_feature`, and `/close_feature`.

## Coding Style & Naming Conventions

Use Markdown and YAML consistent with existing files. Keep prose direct and procedural. Prefer short sections, flat bullet lists, and explicit constraints.

- Skill directories: kebab-case, e.g. `sdd-workflow`
- Artifact files: fixed patterns like `feature_<slug>_<timestamp>.md`
- YAML keys: lowercase with underscores where already used

Preserve existing template structure when updating reference files.

## Testing Guidelines

This repository does not include an automated test suite. Validation is procedural:

- confirm skill flows remain internally consistent
- keep command names, file patterns, and `.context/` paths aligned
- verify examples and referenced filenames actually exist

When adding a skill, review its `SKILL.md`, `agents/openai.yaml`, and any `references/` files together.

## Commit & Pull Request Guidelines

Recent history shows very short commit subjects, sometimes minimal (`-`) and sometimes descriptive (`Add deploy-coolify skill`). Prefer the descriptive form: imperative, concise, and scoped to one change.

Pull requests should include:

- a short summary of the skill or document change
- affected paths, such as `.curated/<skill>/` or `.context/`
- rationale for new or changed commands
- sample usage when behavior changes materially

## Agent-Specific Notes

Do not modify application source code here; this repository primarily defines workflow instructions. Keep changes localized, reuse existing templates, and avoid inventing new directory conventions unless the repository already needs them.
