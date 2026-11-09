# Using multiple Discord accounts at once

Dorion can run several windows side by side, each signed in to a different Discord account. Each account
lives in its own **profile**, which is a separate set of browser data.

| Separate for each profile | Shared by all profiles |
|---|---|
| Discord login | Dorion settings (`config.json`) |
| Cache and local storage | Enabled client mods (Illegalcord and others) |
| Illegalcord plugin choices and plugin settings | "Allow Multiple Instances" and other toggles |
| | The Dorion version (one install) |

Your account's own Discord settings (theme, notifications, privacy) are stored by Discord, so they follow each account anyway.

## One-time setup

1. **Turn on multiple instances.** In Discord's settings, scroll to the **Dorion** section, open **Dorion Settings**
   and turn on **Allow Multiple Instances**.
2. **Create a profile for the second account.** In the same section, open **Profiles**. Under **Create Profile**,
   type a name (simple names work best, for example `second` or `alt`) and click **Create Profile**.

   > ⚠️ **Don't click "Save and Restart"** for this. That button changes which profile Dorion opens **by default**,
   > for every window, so your main account would stop opening when you start Dorion normally. Creating the profile is enough.
3. **Restart Dorion** by quitting it from the tray icon and opening it again. The tray menu is built when Dorion
   starts, so the new profile only shows up after a restart.

## Opening the second account

**From the tray (easiest):** right-click the Dorion tray icon and choose **Open profile: second**. A new window opens.
The first time, sign in with your second account. After that it stays signed in.

Every profile you create gets its own **Open profile: …** entry. (**Open new Window** opens another window of the
*same* profile, so the same account. You usually don't want that.)

**From a shortcut:** start Dorion with `--profile <name>`. This is handy for a desktop or taskbar shortcut that
opens the second account directly. Once **Allow Multiple Instances** is on, it works even while the main
window is already open.

<details>
<summary><b>Windows</b></summary>

1. Right-click your existing Dorion shortcut on the desktop or in the Start menu folder, and choose **Copy**, then **Paste**.
2. Right-click the copy, choose **Properties**, and in **Target** add ` --profile second` *after* the closing quote:
   ```
   "C:\...\Dorion.exe" --profile second
   ```
   Keep the path that's already there. Only add the part after it.
3. Rename the shortcut, for example "Dorion (second)". You can pin it to the taskbar.

</details>

<details>
<summary><b>macOS</b></summary>

Separate logins on macOS need **macOS 14 (Sonoma) or newer** and a build of this fork from after the macOS profile fix
(see [Troubleshooting](#troubleshooting)). Open the second account from Terminal:

```bash
open -n -a Dorion --args --profile second
```

To get a clickable app for it, open **Shortcuts**, create a shortcut with a **Run Shell Script** action containing the command
above, and add it to the Dock or menu bar.

</details>

<details>
<summary><b>Linux</b></summary>

```bash
Dorion --profile second
```

The command is `Dorion` (capital D) for the `.deb`/`.rpm`/portable builds. Some distribution packages call it `dorion`.

Flatpak: `flatpak run com.spikehd.dorion --profile second`.

For a menu entry, copy Dorion's `.desktop` file (usually `/usr/share/applications/Dorion.desktop`) to
`~/.local/share/applications/dorion-second.desktop`. Change `Name=` to something like `Dorion (second)` and add
`--profile second` to the end of the `Exec=` line.


</details>

**Portable version:** profiles are stored in a `profiles` folder next to `Dorion.exe`, and the shortcut steps are the same.

## Good to know

- **Changing Dorion settings:** all windows share one settings file. Change Dorion settings (not Discord
  settings) in one window only, then restart all windows. Changes made in two windows at the same time can overwrite each other.
- **Plugins:** Illegalcord's plugin list is per profile, so turn plugins on in each account's window separately.
- **Updates:** each window checks for updates. Accept the update in one window. Dorion restarts, and you
  reopen the second account from the tray.
- **Notifications** arrive from every open account. Mute one in Discord's notification settings if it gets noisy.
- **Deleting a profile** (Profiles → **Delete Selected Profile**) removes its login and plugin settings for good.
  Close that profile's window first. The `default` profile can't be deleted.
- Discord allows having more than one account. The usual client-mod risk described in the [README](../README.md#safety-and-risks)
  applies to each account you use with this build.

## Troubleshooting

| Problem | Fix |
|---|---|
| Clicking **Open profile: …** or the shortcut just brings the existing window to the front | **Allow Multiple Instances** is off, or Dorion wasn't restarted after turning it on |
| The tray has no **Open profile: …** entries | Turn on **Allow Multiple Instances**, create a profile, then fully restart Dorion |
| The second window shows the **same account** | It was opened without `--profile`, for example with **Open new Window**. On macOS, you need macOS 14+ and this fork's `v7.0.1` or newer. Official Dorion keeps all macOS profiles on one login |
| Dorion now always opens the wrong account | Someone clicked **Save and Restart** on a profile. In **Profiles**, select `default`, then click **Save and Restart** |
| Settings menus look different | Edit `config.json` while Dorion is closed (`%APPDATA%\dorion\` on Windows, `~/.config/dorion/` on Linux, `~/Library/Application Support/dorion/` on macOS) and set `"multi_instance": true` |
