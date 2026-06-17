# Claude Skill Sync

An Obsidian plugin that turns your Obsidian vault into the central repository for AI agent skills, allowing one-click symlinking to skill directories used by Claude Code, Codex, Cursor, and other AI tools. Cross-device synchronization is handled entirely by your vault's existing sync solution.

> Core principle: **One Source of Truth (Vault), Multiple Mirrors (Symlinks)**. Edit anywhere and stay instantly synchronized. No duplicated content and no wasted storage space.


> Detailed user guide: [docs/USAGE.md](docs/USAGE.md)

## Installation

### Option A: Local Developer Setup (Recommended)

```bash
# 1. Clone the repository anywhere on your machine
git clone https://github.com/<owner>/claude-skill-sync.git ~/Documents/claude-skill-sync

# 2. Create a symlink inside your Obsidian vault
cd /path/to/your-vault
ln -s ~/Documents/claude-skill-sync .obsidian/plugins/claude-skill-sync

# 3. Launch Obsidian
# Settings → Community Plugins → Disable Safe Mode → Enable Claude Skill Sync
```

To update:

```bash
cd ~/Documents/claude-skill-sync
git pull
```

Then reload the plugin from Obsidian.

### Option B: Direct Installation (Simplest)

Clone the repository directly into your vault's plugin directory:

```bash
git clone https://github.com/<owner>/claude-skill-sync.git \
  /path/to/your-vault/.obsidian/plugins/claude-skill-sync
```

## Getting Started

1. Open **Settings → Claude Skill Sync** and verify:
   - **Vault Skill Root Directory** (default: `Skills`)
     - You may change this to your actual skill location, such as `SkillPack/skills`
   - **Target Platforms**
     - Defaults to Claude Code and Codex/Agents
     - Additional platforms can be added freely

2. Click the plug icon in the left ribbon to open the sidebar.

3. Create or place skills under your vault's skill root directory.
   - Each subfolder represents a skill.
   - Each skill must contain a `SKILL.md` file.

4. Click **Install All**.
   - All skills will be symlinked to the configured platform directories.
   - The AI tools can use them immediately.

## Features

- **Sidebar Status Dashboard**
  - 8-state grid showing:
    - Fully Synced
    - Partially Synced
    - Not Installed
    - Pending Import
    - Incorrect Target
    - Broken Symlink
    - Conflict
    - Other synchronization states

- **Bi-Directional Synchronization**
  - Vault → Platform (Install)
  - Platform → Vault (Import Wizard)

- **One-Click Broken Link Cleanup**
  - Remove orphaned symlinks when skills are deleted from the vault.

- **One-Click Link Repair**
  - Automatically fix symlinks when vault paths change.

- **Scheduled Auto Sync**
  - Default interval: every 5 minutes.
  - Automatically installs newly created vault skills.
  - Newly detected platform directories can trigger notifications or optional auto-import.

- **Startup Summary Notifications**
  - Displays synchronization status when Obsidian launches.

## How It Works

```text
vault/<skillRoot>/<name>/   ← Source of Truth (Real Data)
              ▲
              │ symlink
              │
~/.claude/skills/<name>     ← Mirror
~/.agents/skills/<name>     ← Mirror
~/.cursor/skills/<name>     ← Mirror (custom platform example)
```

All AI tools read from the same underlying content, ensuring consistency across platforms.

## Multi-Device Synchronization

The vault itself can be synchronized using any preferred method:

- Obsidian Sync
- iCloud
- Git
- Dropbox
- OneDrive
- Synology Drive
- Nutstore
- Any other file synchronization service

Each computer creates and manages its own local symlinks through Claude Skill Sync.

The `data.json` file contains machine-specific platform paths and should **not** be synchronized across devices.

Since paths such as `~/.claude/skills` vary between machines, the repository's `.gitignore` already excludes `data.json`.

## Platform Support

- ✅ macOS
- ✅ Linux
- ✅ Windows Desktop
- ❌ Mobile (iOS / Android)

Mobile versions of Obsidian do not support Node.js filesystem APIs or symlink operations. The plugin is therefore configured with:

```json
"isDesktopOnly": true
```

## Example Configuration

`data.json` is generated automatically after first launch and should never be committed to Git.

```json
{
  "skillRoot": "SkillPack/skills",
  "platforms": [
    {
      "name": "Claude Code",
      "target": "~/.claude/skills",
      "enabled": true
    },
    {
      "name": "Codex / Agents",
      "target": "~/.agents/skills",
      "enabled": true
    },
    {
      "name": "Cursor",
      "target": "~/.cursor/skills",
      "enabled": false
    }
  ],
  "notifyOnStartup": true,
  "autoScanIntervalMin": 5,
  "autoInstall": true,
  "autoImport": false
}
```

## Development

This plugin is written in plain JavaScript and requires no build process.

After modifying `main.js`:

1. Disable the plugin in Obsidian.
2. Re-enable the plugin.

```text
.
├── manifest.json   # Plugin metadata
├── main.js         # Core logic (~770 lines)
├── styles.css      # Sidebar styling
├── data.json       # Local user settings (gitignored)
└── README.md
```

## License

MIT License — see `LICENSE` for details.
