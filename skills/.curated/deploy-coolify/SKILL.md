---
name: deploy-coolify
display_name: "Deploy Coolify"
description: "First deploy and incremental Coolify reconciliation for compose-based repositories"
short_description: "Compose-to-Coolify deploy flow"
version: "0.1.0"
language:
  prompts_fixed: "en"
  user_interaction: "pt-BR"
  artifacts: "pt-BR"
---

# Deploy Coolify

Use this skill when the user wants a first deploy or an incremental Coolify reconciliation for a repository that has:

- a root `docker-compose.yml`
- a deploy flow based on `scripts/deploy_coolify.py`
- `.env` as the source of truth for application variables

This skill is for:

- previewing what will become a native Coolify resource versus what stays in Compose
- generating or refreshing `docker-compose.coolify.yml`
- generating or refreshing `.env.coolify`
- running the repo deploy flow with a dry-run first and the real deploy only after confirmation

## Workflow

1. Confirm the repository contract exists.
2. Run `python scripts/deploy_coolify.py --dry-run`.
3. Summarize the three plan sections emitted by the script:
   - resources that will be created or updated in Coolify
   - services that stay in `docker-compose.coolify.yml`
   - local-only or obsolete resources that will not be changed automatically
4. If `.env.coolify` is missing required values, stop and point the user to the missing keys.
5. Ask for confirmation before any real deploy.
6. On approval, run `python scripts/deploy_coolify.py`.

## Guardrails

- Never skip the dry-run unless the user explicitly asks for it.
- Do not delete old Coolify resources automatically; the repo flow is additive by design.
- Use `--bootstrap-github` only when the repository still needs its remote GitHub bootstrap.
- Do not print secrets from `.env` or `.env.coolify`.
- If the repo does not match the expected contract, say so clearly and do not improvise a different deploy flow.

## References

- `references/repo-workflow.md`
