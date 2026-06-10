# Flow — Personal Project OS for Obsidian

A local-first, Markdown-native project management plugin for [Obsidian](https://obsidian.md). Flow transforms your vault into a structured personal operating system — purpose-built for developers and builders who need focused, distraction-resistant task execution.

All data is stored as plain Markdown files with YAML frontmatter. No proprietary databases. No sync lock-in.

---

## Features

| Module | Description |
|---|---|
| **Kanban Board** | Drag-and-drop issue cards across Todo / In Progress / Blocked / Done columns |
| **Visual Issue Editor** | Edit issue metadata via structured form or raw Markdown mode |
| **Daily Planning** | Mark tasks for today's focus; auto-syncs to your Daily Note |
| **Weekly Review** | Structured weekly evaluation with a completion checklist and log |
| **Pomodoro Timer** | 3-slot focus queue with auto-advance and session auto-logging |
| **Task Dependencies** | `blockedBy` field — visually locks tasks awaiting resolution |
| **WIP Limit** | Configurable cap on concurrent In Progress tasks |
| **Energy Planning** | Low / High energy classification with dashboard filter |
| **Project Health** | Dynamic health scoring per project: Healthy · At Risk · Stuck |
| **Knowledge Links** | `related` wiki-links open directly in Obsidian from the issue editor |

---

## Installation

### Via BRAT *(Recommended)*

[BRAT](https://github.com/TfTHacker/obsidian42-brat) is a community plugin that enables installing and auto-updating plugins directly from GitHub.

1. Install **BRAT** from Obsidian Community Plugins
2. Go to **Settings → Community Plugins → BRAT → Add Beta Plugin**
3. Enter the public release repository URL:
   ```
   https://github.com/Syibi/flow-release
   ```
4. Click **Add Plugin** — BRAT will download and activate the plugin automatically

To update to the latest release: **BRAT → Check for updates**

---

### Manual Installation

1. Navigate to the latest files in this repository.
2. Download `main.js`, `styles.css`, and `manifest.json`.
3. Copy all three files into your vault at:
   ```
   [YourVault]/.obsidian/plugins/obsidian-flow-tracker/
   ```
4. Restart Obsidian and enable the plugin under **Settings → Community Plugins**

---

## Configuration

After activation, configure your folder paths under **Settings → Flow Tracker**:

| Setting | Default | Purpose |
|---|---|---|
| Inbox Folder | `2. WORK/INBOX` | Quick-capture landing zone |
| Issues Folder | `Issues` | Active task files |
| Projects Folder | `Projects` | Project definition files |
| Epics Folder | `Epics` | Epic / sub-project files |
| Docs Folder | `Docs` | Reference documents |
| Daily Notes Folder | `Daily Notes` | Daily note files |
| WIP Limit | `3` | Maximum concurrent In Progress tasks |

Flow automatically creates any missing folders on first load.

---

## Issue Schema

Each issue is a Markdown file with a YAML frontmatter block:

```yaml
---
id: ISSUE-001
type: issue
title: Implement OAuth Login
status: in-progress       # todo | in-progress | blocked | done
priority: high            # high | medium | low | normal
project: "[[PROJECT-APP]]"
epic: "[[EPIC-AUTH]]"
created: 2026-06-09
due: 2026-06-20
estimate: 4               # estimated Pomodoro sessions
logged: 2                 # completed Pomodoro sessions
difficulty: medium        # easy | medium | hard
energy: high              # low | high
today: true               # include in today's Daily Plan
blockedBy:
  - "[[ISSUE-002]]"
related:
  - "[[oauth-research]]"
tags:
  - backend
  - auth
---

# Context

...

# Notes

...
```

---

## License

MIT © 2026 Syibi
