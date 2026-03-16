# openclaw-android-fork

Template project for maintaining an OpenClaw Android fork with safe upstream sync + APK builds.

## Goals

- Keep `main` as a clean mirror of upstream (`openclaw/openclaw`)
- Keep custom changes in `custom/android-build`
- Build Android APKs in GitHub Actions without losing local customizations

## Branch model (important)

- `main` → upstream mirror only (safe to hard reset/sync)
- `custom/android-build` → your workflows/patches and build tweaks

Never develop directly on `main`.

## Workflows

### 1) Sync upstream (`.github/workflows/sync-upstream.yml`)
- Runs manually or on schedule
- Syncs only `main`
- Does **not** touch `custom/android-build`

### 2) Android build (`.github/workflows/android-build.yml`)
- Runs on pushes to `custom/android-build`
- Can be run manually (`workflow_dispatch`)
- Produces Debug APK artifact

## Typical update flow

1. Run **Sync upstream** (updates `main`)
2. Rebase custom branch:
   ```bash
   git checkout custom/android-build
   git fetch origin
   git rebase origin/main
   ```
3. Resolve conflicts (if any), push
4. Build workflow runs and uploads fresh APK

## Optional: signed release builds

For release signing, add GitHub Secrets and extend the build workflow:
- `ANDROID_KEYSTORE_BASE64`
- `ANDROID_KEY_ALIAS`
- `ANDROID_KEYSTORE_PASSWORD`
- `ANDROID_KEY_PASSWORD`

(Deliberately omitted for now to keep first setup simple and safe.)
