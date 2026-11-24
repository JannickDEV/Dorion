# JannickDEV/Dorion fork

Tracks `SpikeHD/Dorion`. Changes compared to upstream:

- **Illegalcord client mod** (`src-tauri/src/injection/client_mod.rs`): loads `browser.js`/`browser.css`
  from the `JannickDEV/Illegalcord` `latest` release.
- **Profile launchers** (`tray.rs`, `profiles.rs`): with multi-instance on, the tray has an
  "Open profile: <name>" entry for every other profile. "Open new Window" keeps the current profile.
- **Profile-aware cache clearing** (`cache.rs`): respects `--profile`.
- **Separate macOS logins per profile** (`main.rs`, `profiles.rs`): WKWebView ignores `data_directory`, so non-default
  profiles get their own `data_store_identifier` (macOS 14+, ID derived from the profile name). The default profile keeps
  the standard store.
- **Linux: profiles can run side by side** (`main.rs`, `tray.rs`): CEF locks its root cache folder, and every instance
  shared one, so a second profile exited at once. Non-default profiles now use their own data folder
  (`~/.cache/com.spikehd.dorion/cef/dorion-<profile>`) as CEF root, so logins stay where they were. Instances started from
  the tray no longer inherit `DORION_CEF_ARG_REEXEC`, so they re-exec with `--no-sandbox` like a normal start.
- **Multi-account guide** (`docs/MULTIPLE_ACCOUNTS.md`).
- **Updates come from this fork** (`updater/src/main.rs`, `src-tauri/src/release.rs`): the updater and the
  update check use `JannickDEV/Dorion` releases.
- **Fork release workflow** (`.github/workflows/fork-release.yml`): after each Build on `main`, it publishes a
  release of that commit: `v<version>` for the first build of a version, `v<version>-<short commit>` for later fork-only
  changes. Don't create releases by hand; run this workflow from the Actions tab instead.
- **Commit-based update check** (`src-tauri/src/release.rs`): an update is offered when the latest release was built
  from a different commit than the running app (`GIT_HASH`), so fork-only fixes reach users without a version bump.
- **Releases don't wait for macOS** (`.github/workflows/build.yml`): Windows, Linux and Flatpak packaging run even
  when a macOS build failed, which is usually GitHub having no macOS runner available. `fork-release.yml` publishes once the
  Windows installers and Linux `.deb` exist, and later adds missing files (the `.dmg`s) from a build of the same commit.
- **macOS auto-retry** (`.github/workflows/retry-macos.yml`): if a Build on `main` failed only in macOS jobs, the failed
  jobs are re-run after 5, 15 and 30 minutes, up to 3 retries. It doesn't retry when another platform failed or when a newer
  commit has been built since.
- **Safety check** (`scripts/forkSafetyCheck.mjs`, `.github/fork-safety.json`): the scanner the daily sync
  runs on upstream changes before merging.
- **README.md**: rewritten for people who use this build (download, setup, risks, credits). For general
  Dorion documentation it links to upstream's README.

## Manual upstream sync

```bash
git remote add upstream https://github.com/SpikeHD/Dorion.git
git fetch upstream
node scripts/forkSafetyCheck.mjs HEAD upstream/main
git merge upstream/main
```

When resolving conflicts in the files listed above (also `forkOwnedFiles` in `.github/fork-safety.json`), keep the fork's changes.
