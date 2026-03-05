# SDD Skills Monorepo

## Status

`sdd-workflow` (bundle único) está **deprecated** e mantido apenas por compatibilidade temporária.

Use as skills independentes em `packs/sdd-pack/*`:

- `sdd-research-codebase` -> `/research_codebase`
- `sdd-research-feature` -> `/research_feature`
- `sdd-plan-feature` -> `/plan_feature`
- `sdd-implement-feature` -> `/implement_feature`
- `sdd-close-feature` -> `/close_feature`
- `sdd-update-skill` -> `/update_skill`

Cada skill é self-contained e possui seu próprio `agents/openai.yaml` para controlar `agent.model` e `agent.reasoning` separadamente.

## Estrutura Atual

```text
packs/sdd-pack/
  sdd-research-codebase/
  sdd-research-feature/
  sdd-plan-feature/
  sdd-implement-feature/
  sdd-close-feature/
  sdd-update-skill/
```

## Instalação (skill-installer, múltiplos paths)

Exemplo PowerShell usando um único repositório com múltiplos `--path`:

```powershell
python C:\Users\imale\.codex\skills\.system\skill-installer\scripts\install-skill-from-github.py `
  --repo owner/repo `
  --path packs/sdd-pack/sdd-research-codebase `
  --path packs/sdd-pack/sdd-research-feature `
  --path packs/sdd-pack/sdd-plan-feature `
  --path packs/sdd-pack/sdd-implement-feature `
  --path packs/sdd-pack/sdd-close-feature `
  --path packs/sdd-pack/sdd-update-skill
```

## Observações

- O bundle legado na raiz (`SKILL.md`, `agents/`, `references/`) não foi removido.
- O fluxo recomendado agora é chamar comandos diretamente (`/research_codebase`, `/plan_feature`, etc.) por skill instalada.
