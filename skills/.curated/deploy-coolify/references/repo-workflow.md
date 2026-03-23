# Repo workflow

Preferred sequence:

1. Preview:

```bash
python scripts/deploy_coolify.py --dry-run
```

2. First deploy with GitHub bootstrap:

```bash
python scripts/deploy_coolify.py --bootstrap-github
```

3. Incremental deploy:

```bash
python scripts/deploy_coolify.py
```

4. Private GitHub bootstrap:

```bash
python scripts/deploy_coolify.py --bootstrap-github --github-private
```

5. List GitHub Apps:

```bash
python scripts/deploy_coolify.py --list-github-apps
```
