<div align="center">
  <img width="128" src="https://raw.githubusercontent.com/SpikeHD/Dorion/main/src-tauri/icons/icon.png" alt="Dorion icon">
  <h1>Dorion (JannickDEV build)</h1>
  <p><b>A custom build of <a href="https://github.com/SpikeHD/Dorion">Dorion</a>, the small and fast Discord client, with a curated <a href="https://github.com/JannickDEV/Illegalcord">Illegalcord</a> built in.</b></p>
  <p>
    <a href="https://github.com/JannickDEV/Dorion/releases/latest">Download</a> ·
    <a href="#whats-different">What's different</a> ·
    <a href="#setup">Setup</a> ·
    <a href="#safety-and-risks">Safety and risks</a> ·
    <a href="#credits-and-license">Credits</a>
  </p>
</div>

> [!IMPORTANT]
> This is an **unofficial, personal build** I share with friends. It is not made, reviewed or supported by
> SpikeHD (Dorion), ImHisako (Illegalcord), Equicord, Vencord or Discord. Don't report problems with it to
> those projects. Report them [here](#reporting-problems).

> [!WARNING]
> **Using any modified Discord client breaks Discord's [Terms of Service](https://discord.com/terms).**
> This build removes Illegalcord's most blatant rule-breaking plugins (selfbots, Nitro snipers, mass mentions,
> spoofing, member scanning and so on), which makes a ban much less likely. It does not make one impossible.
> Use it at your own risk, and preferably not on an account you can't afford to lose.

## What's different

Compared with official Dorion:

| | Official Dorion | This build |
|---|---|---|
| Client mods | Shelter, Vencord, Equicord | Same, plus **Illegalcord** (curated, see below) |
| Multiple accounts | Profiles, opened one at a time | Every profile gets its own **"Open profile: …"** entry in the tray, so you can run two or more accounts side by side |
| Updates | From SpikeHD's releases | From **this fork's** releases, so updating never replaces this build with the official one |
| Upstream changes | — | Pulled in daily, **only after a security review** (see [How updates are checked](#how-updates-are-checked)) |

**Curated Illegalcord** is Illegalcord with 22 plugins removed. They downloaded and ran unverified programs, broke
Discord's rules in ways that commonly lead to bans, or spied on other users. The full list and the reasons
are in the [Illegalcord fork's README](https://github.com/JannickDEV/Illegalcord#removed-plugins).

## Download

Get the latest version from **[Releases](https://github.com/JannickDEV/Dorion/releases/latest)**:

| System | File |
|---|---|
| Windows (most PCs) | `Dorion_<version>_x64-setup.exe` (installer) or `Dorion_<version>_x64_en-US.msi` |
| Windows on ARM | `Dorion_<version>.arm64-setup.exe` |
| Windows, no install | `Dorion_<version>_win64_portable.zip` |
| macOS (Apple silicon / Intel) | `Dorion_<version>_aarch64.dmg` / `Dorion_<version>_x64.dmg` (when GitHub had macOS build capacity) |
| Linux | `.deb`, `.rpm`, `.flatpak` or `_amd64_portable.tar.gz` |

Only download from this repository's Releases page. Every file there was built by GitHub Actions from the
public source in this repository. If someone sends you a "Dorion" or "Illegalcord" file directly, don't run it.

> [!NOTE]
> These builds aren't code-signed. Windows SmartScreen may say *"Windows protected your PC"*. Click
> **More info → Run anyway** if the file came from the Releases page above. macOS may require
> right-click → **Open** the first time.

## Setup

1. **Install and sign in** as you would with the normal Discord app.
2. **Turn on Illegalcord:** open Discord settings, scroll to the **Dorion** section, then **Client Mods**. Enable
   **Illegalcord**, and make sure **Vencord** and **Equicord** are off; they conflict with each other. Shelter stays on.
   Restart Dorion, and Illegalcord's settings appear in the settings sidebar.
3. **Turn on auto update** in Dorion's settings. On Windows and macOS this installs new versions of this build
   automatically. On Linux you're notified, and you install the new package from Releases.
4. **Choose your plugins** in the Illegalcord settings. A few are on by default, such as DiscordHardened, StreamProof,
   the tutorial and announcements, and a few cosmetic ones. Everything else is opt-in. Turn on only what you need.

### Two accounts at once

1. In Dorion's settings, under **Profiles**, create a profile, for example `second`.
2. Turn on **multi-instance** and restart Dorion.
3. Right-click the Dorion tray icon and choose **Open profile: second**, then sign in with your other account.

Each profile has its own login, cache and plugin settings. You can also start a profile directly, for example from a
desktop shortcut: `Dorion.exe --profile second` (Windows) or `dorion --profile second` (Linux).

If a menu entry has a slightly different name in your version, edit `config.json` directly while Dorion is
closed. It's in `%APPDATA%\dorion\` on Windows and `~/.config/dorion/` on Linux:

```json
"client_mods": ["Shelter", "Illegalcord"],
"multi_instance": true
```

## Safety and risks

**What was checked.** Before building this, Illegalcord was reviewed line by line where it mattered: every
use of the Discord token, every network request, every place that starts another program, all native
(system-level) code, and the shipped binaries. No malware, token stealing or hidden data upload was found
in Illegalcord's own code. The risky parts were plugins that download and run other people's programs, and
plugins whose purpose breaks Discord's rules. Those were removed.

**What you should still know:**
- **Ban risk.** Some remaining plugins still change how Discord behaves, for example SilentDelete, SilentEdit,
  SilentGroupCall, FollowUser, GhostClient and IllegalMessageLogger. They're popular and low-risk, but any client mod
  is against Discord's rules.
- **Other people's privacy.** Message-logger plugins keep messages that others deleted. Be decent about it.
- **Network contacts.** When Discord starts, Illegalcord downloads badge lists from GitHub and `api.nightcord.st`,
  as Equicord and Vencord do with their own badge services. Those hosts see your IP address and nothing else.
  Plugins you turn on may contact the service they're built for (for example lyrics, translations or encrypted uploads).
- **Desktop-only plugins.** Dorion loads Illegalcord's browser build, so plugins that need native code
  (CustomDNS, SecureZipper, RamOptimizer, ClientIcon and others) are switched off or do nothing here.

### How updates are checked

Once a day, an automated Claude session (AI-assisted, run on my account) does the following:
1. Fetches new changes from official Dorion and from Illegalcord.
2. Runs [`scripts/forkSafetyCheck.mjs`](scripts/forkSafetyCheck.mjs). It flags new or changed binaries, token
   access, starting other programs, remote scripts, new network destinations, new dependencies, and changes to
   privileged code.
3. Reads every flagged change and every new plugin, and merges only if nothing unsafe was found. New
   rule-breaking plugins are removed before merging.

If anything is unclear, nothing gets merged and I get a notification to look at it myself. This is a strong filter,
but no review is a guarantee.

## Reporting problems

Open an issue in this repository, or message me (JannickDEV) directly. For security problems such as a suspicious file or
behaviour that looks like data being sent somewhere, **don't post details publicly**. Message me privately
first. Never send anyone your Discord token or password, and nobody legitimate will ask for them.

For general Dorion questions (installation problems, the "Oops! Something went wrong" screen, Linux and
Flatpak details, building it yourself), see the **[official Dorion README](https://github.com/SpikeHD/Dorion#readme)**.
It applies to this build too.

## For developers

Changes compared with upstream are listed in [FORK_NOTES.md](FORK_NOTES.md). Building works exactly as described in the
[official instructions](https://github.com/SpikeHD/Dorion#building). The client mod entry for Illegalcord is in
`src-tauri/src/injection/client_mod.rs`.

## Credits and license

- **[Dorion](https://github.com/SpikeHD/Dorion)** by SpikeHD and contributors: the entire client. This repository only adds a few small changes.
- **[Illegalcord](https://github.com/ImHisako/Illegalcord)** by ImHisako and contributors, based on
  **[Equicord](https://github.com/Equicord/Equicord)** and **[Vencord](https://github.com/Vendicated/Vencord)**.
- **[Shelter](https://github.com/uwu/shelter)**, which Dorion uses for its own settings and plugins.

Dorion is licensed under the [GNU GPL v3](LICENSE), and so is this build. The full source of every build is
in this repository.

Not affiliated with or endorsed by Discord Inc.
