# Branch policy

## Why this policy exists

To prevent upstream sync from overwriting your custom CI/build changes.

## Rules

1. `main` is an upstream mirror branch.
   - Safe to overwrite during sync.
   - No custom commits here.

2. `custom/android-build` is your working branch.
   - Contains workflows, patches, and any Android build customizations.
   - Protected from automated sync.

3. Upstream sync workflow must only update `main`.

4. Build workflow should run on `custom/android-build` (or manual dispatch), not on `main`.

## Recommended repo settings

- Protect `custom/android-build`:
  - Require PRs (optional)
  - Prevent force push (recommended)

- Keep `main` unprotected or lightly protected if your sync action needs force updates.

## Conflict strategy

When upstream changes conflict with custom patches:

```bash
git checkout custom/android-build
git fetch origin
git rebase origin/main
# resolve conflicts

git add .
git rebase --continue
git push --force-with-lease
```

Use `--force-with-lease` only on your custom branch after rebasing.
