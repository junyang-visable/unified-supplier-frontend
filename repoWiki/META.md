# META

<!-- Auto-managed by knowledge-base skills. Do not edit manually. -->

```
schema_version:      1
generated:           2026-07-15
last_synced:         2026-07-15
last_commit:         9c326bb
repoWiki_tree_hash:  untracked
```

---

## Fields

- `schema_version` — integer; increment when KB directory structure changes
- `generated` — ISO date when KB was first initialized
- `last_synced` — ISO date of the most recent skill write operation
- `last_commit` — short commit SHA at sync time; changes on rebase
- `repoWiki_tree_hash` — git tree object hash of the `./repoWiki/` directory at sync time; only changes when KB file content changes, survives clean rebase

---

## How to Capture Values

```
git rev-parse --short HEAD   → last_commit
git rev-parse HEAD:repoWiki  → repoWiki_tree_hash
```

If `./repoWiki/` is not yet committed, use `"untracked"` for `repoWiki_tree_hash`.
