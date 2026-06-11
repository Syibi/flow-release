# Changelog

All notable changes to this project will be documented in this file.

---

## [0.4.2] - 2026-06-11

### Added
- **Interactive Tag Pills (Chips)**: Redesigned the tags input field to automatically convert text to separate interactive tag pills upon typing a comma, pressing `Enter` or `Tab`, or clicking outside the input (blur). Supports removing individual tags by clicking their delete (`×`) icon or pressing `Backspace` on an empty input.
- **Robust Tag Fallbacks**: Added a legacy tags parser to ensure existing comma-separated strings inside frontmatter metadata, local drafts, or raw markdown view mode are automatically parsed and loaded as tag pills.

### Changed
- **Issue Editor Layout Alignment**:
  - Moved the ADHD Energy Level dropdown to replace the Tags input field in Row 4.
  - Aligned the columns of ADHD Energy Level, Estimate (Pomodoros), and Logged Pomodoros to have equal/aligned width (`1fr 1fr 1fr`).
  - Moved the Tags input below the checkboxes to act as a single, full-width field.
  - Refined the checkboxes row to display planning checkboxes in a clean horizontal grid layout.
- **Pomodoro Timer & Sidebar**: Refined timer persistence using localStorage epoch targets across sidebar unmounts/reloads.
- **Backlog Filters & List UI**: Simplified task list filters to a single dropdown, enabled horizontal scroll navigation for tags/states filters, and added a responsive table-to-card layout for small screens.

---

## [0.4.1] - 2026-06-10

### Added
- **Horizontal Pagination for Daily Reports**: Refactored the Recent Daily Reports list in the Productivity tab into a horizontal, stretch-aligned grid row. Displays 4 cards at a time with sleek Prev/Next pagination buttons to eliminate vertical scroll clutter.
- **Collapsible Navigation Tab Bar**: Redesigned the main header tabs using the "Active Tab Text Only" pattern. Inactive tabs collapse into compact icons with native hover tooltips, and the active tab expands smoothly to fill a stable-width container, preventing adjacent element layout shifts.

### Changed
- **Minimalist Icon Cleanup**: Replaced empty dashboard state target emoji with a styled Lucide check SVG icon.
- **Emoji Removal**: Stripped all casual emojis and emoticons from the Kanban Board, Weekly Review tabs, Pomodoro Timer, Issue Editor, and Daily Note review markdown templates to maintain a professional, developer-focused aesthetic.

### Fixed
- **Changelog Type Error**: Fixed a TypeScript compilation warning in `src/main.ts` by passing the `FlowPlugin` instance (which implements `Component`) to `ChangelogModal` for markdown rendering.

---

## [0.4.0] - 2026-06-10

### Added
- **Productivity & Consistency Dashboard**: A new dedicated dashboard tab displaying current streak, best streak, active days consistency percentage, total focus time, and task planning accuracy.
- **Contribution Heatmap Calendar**: A standard 12-week (84 days) grid contribution calendar aligned to Monday-Sunday columns, featuring Month headers (e.g. *May, Jun*) and Weekday labels (*Mon, Wed, Fri*). Uses adaptive theme colors based on `color-mix` with `var(--interactive-accent)` and `var(--background-primary)`.
- **Planned Tasks Highlights**: Contribution calendar cells are highlighted as active (level-1) on days with planned tasks, even if no Pomodoros have run yet, allowing users to track consistency through scheduling.
- **Sleek Daily Reports Timeline**: A chronological feed of recent daily note reports with visual linear progress bars tracking focus completions and task completion rates.
- **Timezone-Safe Local Rollover**: Replaced all UTC-based date operations with local timezone formatting. All daily notes sync, reflections, and streak roll-overs now occur exactly at midnight **00:00 local time** (instead of 07:00 AM UTC).
- **Accidental Close Protection**: Double-click behavior on the issue editor overlay background is now required to close the modal, preventing accidental loss of drafts.
- **Git Release Automation**: Added a custom `release.mjs` script that builds, bumps, syncs, commits in the private repo, and publishes compiled artifacts to the public `flow-release` repository.

### Changed
- **Minimalist Projects & Epics Layout**: Redesigned Projects and Epics views using clean Graph Card layouts without emoticons/emojis, using native styling rules.
- **Pomodoro Timer Behaviors**: Active tasks are preserved across breaks instead of automatically advancing, recommended task triggers are capped based on capacity/estimates, and duplicate selections are automatically filtered out.

---

## [0.3.1] - 2026-06-05

### Added
- **Focus Sidebar**: Added a dedicated collapsible sidebar (350px wide) housing the Pomodoro Timer and task queue selectors.
- **WIP Limit Check**: Enforces a configurable work-in-progress limit to prevent cognitive overload.

### Changed
- **Settings View**: Updated options to easily customize Inbox, Issues, Projects, Epics, Docs, and Daily Notes directories.

---

## [0.1.0] - 2026-05-15

### Added
- **Initial Release**: Core personal project operating system for Obsidian, featuring Markdown-native Kanban boards, task schemas, daily note log sync, and weekly reviews.
