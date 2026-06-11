# Changelog

All notable changes to this project will be documented in this file.

---

## [0.5.1] - 2026-06-11

### Fixed
- **Recent Daily Reports Sort Order**: Fixed the Productivity tab showing oldest daily notes first when paginating. Removed an erroneous `.reverse()` call so that the most recent notes always appear on the first page (newest-first order).

---

## [0.5.0] - 2026-06-11

### Added
- **Daily Report Modal**: New Obsidian modal (`DailyReportModal`) that opens a full daily report for any selected date. Displays session stats (focus pomodoros, total duration, completion %), completed and uncompleted task cards, a Refleksi Harian section, and an activity log with two tabs — Kronologis (timeline) and Report (task sessions with precise `startTime – endTime` and action notes). Footer provides quick access to the raw daily note and dashboard navigation.
- **Review Tab**: New dedicated **Review** main tab with two sub-tabs:
  - **Daily Review** — date switcher (`< Prev Day / Today / Next Day >`) for reviewing any date. Includes a reflection form (Yang Berjalan Lancar, Hambatan / Kendala, Rencana Besok) with **1-second auto-save** (no manual save button). Also shows a daily report card with task sessions and clickable task titles that open the IssueEditor.
  - **Weekly Review** — scope planner for `thisWeek` tasks and a weekly overview summary.
- **Workday Timeline**: New visual progress bar on the Dashboard showing elapsed time, planned focus time, free buffer, and overload segments across the work day. A floating "Sekarang (HH:MM)" badge with a vertical marker line shows real-time position.
- **Daily Focus Progress Bar**: Secondary progress bar below the timeline tracking logged vs estimated pomodoros for today's tasks.
- **Focus Queue**: Pomodoro sidebar now features a 3-slot Focus Queue that auto-fills from today's active tasks, auto-ejects completed tasks, and compacts empty slots. Supports manual slot assignment via dropdown.
- **Deep Work Overlay**: Full-screen blur overlay activates during focus sessions to minimize distraction. Includes active task display, session goal input, and minimize/restore control.
- **Session Goal**: Before each focus block, users can set a concrete intention ("Saya akan fokus sampai...") that is displayed during the session and cleared on completion.
- **Break Overlay**: Full-screen break overlay with breathing animation (inhale/hold/exhale cycle), offline activity prompt, and skip/dismiss controls.
- **Alarm System**: Looping audio tone and pulsing button when Pomodoro timer completes. "Matikan Alarm" silences and transitions to next mode.
- **Recommended Task Widget**: Suggests the highest Smart-Score backlog task not yet in the Focus Queue when capacity allows.
- **Target Tercapai Button**: One-click manual session completion inside the Deep Work overlay — increments logged pomodoro count, logs activity to daily note, and transitions to break mode with a celebratory C5-E5-G5 audio arpeggio.
- **Inbox Tab**: Quick-capture input for raw tasks and thoughts, processed later into issues via the IssueEditor.
- **End-of-Day Celebration State**: When the scheduled work day ends, Today's Plan is replaced with a celebration card showing daily stats and a shortcut to write daily reflection.

### Changed
- **Dashboard Redesign**: Replaced the old multi-column stat widget grid with a focused single-column layout (max 800px). Today's Plan and Timeline are the primary focal points; project health cards removed.
- **Reflection Labels**: All-caps saturated colored text in the Daily Report reflection section replaced with soft pill badges (muted background, low-opacity border) for cleaner visual hierarchy.
- **Report Tab**: Renamed "Daily Standup" tab to "Report"; now set as the default active tab in the Review view.
- **Recent Daily Reports Sort Order**: List on Dashboard now sorted oldest-to-newest (ascending chronological order).
- **Timer State Persistence**: Pomodoro timer state (mode, running/paused, remaining time) persisted to `localStorage` using epoch targets — survives sidebar unmounts and plugin reloads.
- **obsidianUtils**: `readReflectionFromDailyNote` and `saveReflectionToDailyNote` now accept an optional `dateStr` parameter for reading/writing to any date's daily note (not just today).

### Fixed
- **Modal Close Button**: Native Obsidian close button hidden inside `DailyReportModal`; replaced with a custom React button precisely positioned in the modal header with proper sizing, border, and hover effect.
- **Emoji Cleanup**: Removed `⏱️` from timing badges and `🎉` from task completion text in both the Daily Report Modal and Weekly Review views.
- **Auto-Clean Stale Tasks**: Completed tasks with a `completedDate` from a previous day are automatically removed from Today's Plan on Dashboard load.

---

## [0.4.4] - 2026-06-11

### Fixed
- **Task List View Pagination**: Fixed a layout bug where the pagination controls at the bottom of the table were pushed off-screen and hidden. Refactored the table component to keep the table header and pagination controls sticky at the top/bottom while making only the rows container scrollable.
- **Pomodoro Timer Load Glitch**: Fixed a race condition/glitch on reload where the timer was reset to 25:00 and paused. Guarded the auto-cleanup and sync effects from executing before the issues list index is successfully loaded.

---

## [0.4.3] - 2026-06-11

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
