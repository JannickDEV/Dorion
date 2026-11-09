# JannickDEV/Dorion fork

Tracks `SpikeHD/Dorion`. Changes compared to upstream:

- **Illegalcord client mod** (`src-tauri/src/injection/client_mod.rs`): loads `browser.js`/`browser.css`
  from the `JannickDEV/Illegalcord` `latest` release.
- **Profile launchers** (`tray.rs`, `profiles.rs`): with multi-instance on, the tray has an
  "Open profile: <name>" entry for every other profile. "Open new Window" keeps the current profile.
- **Profile-aware cache clearing** (`cache.rs`): respects `--profile`.
- **Updates come from this fork** (`updater/src/main.rs`, `src-tauri/src/release.rs`): the updater and the
  update check use `JannickDEV/Dorion` releases.
- **Fork release workflow** (`.github/workflows/fork-release.yml`): after a successful Build on `main`, it publishes
  `v<version>` from that build, but only if the version has no release yet. Run it by hand from
  the Actions tab to publish the current version.
- **Safety check** (`scripts/forkSafetyCheck.mjs`, `.github/fork-safety.json`): the scanner the daily sync
  runs on upstream changes before merging.

## Manual upstream sync

```bash
git remote add upstream https://github.com/SpikeHD/Dorion.git
git fetch upstream
node scripts/forkSafetyCheck.mjs HEAD upstream/main
git merge upstream/main
```

When resolving conflicts in the files listed above, keep the fork's changes.
