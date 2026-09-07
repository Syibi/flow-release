# Changelog

All notable changes to this project will be documented in this file.

---
## [0.8.4] - 2026-09-07

### Bug Fixes & Reliability
- **Obsidian Startup & Restart Index Race Condition Fix**:
  - Memperbaiki masalah data tampak kosong/ter-reset saat Obsidian dibuka ulang (*cold restart*).
  - Menghubungkan listener event native Obsidian `app.metadataCache.on('resolved')` dan `app.workspace.onLayoutReady()` sehingga Flow Tracker secara otomatis memicu `rescan()` menyeluruh begitu Obsidian selesai membaca seluruh file dan frontmatter vault.
  - Menambahkan command baru di Command Palette: `Flow: Rescan Vault & Refresh Flow Index` untuk sinkronisasi manual kapan pun dibutuhkan.

---
## [0.8.3] - 2026-09-07

### UI/UX & Anti-Slop Redesign
- **Breadcrumb Navigation**:
  - Menambahkan navigasi breadcrumb dinamis (`Projects / [Project Name] / [Epic Name]`) di bagian atas tampilan detail Proyek dan Epic, menggantikan tombol *Back* konvensional.
- **Metric Summary Bar**:
  - Menambahkan bar ringkasan status (`.flow-metric-summary-bar` & `.flow-metric-pill`) untuk status tugas (`Todo`, `Active`, `Blocked`, `Done`) dengan styling ringkas dan modern, menggantikan deretan badge status yang meregang.
- **Focus Queue Slot Indicators**:
  - Mengganti radio button default pada antrean slot Pomodoro (`Focus Queue`) dengan nomor slot berdesain rapi (`#1`, `#2`, `#3`) yang aktif bersesuaian dengan task terpilih.
  - Menambahkan `tabular-nums` pada countdown digital Pomodoro untuk mencegah layout jitter saat detik berjalan.
- **Keyboard Accessibility (`:focus-visible`)**:
  - Menambahkan styling cincin fokus keyboard kontras (`2px solid var(--interactive-accent)`) dengan offset 2px untuk seluruh elemen interaktif: tombol aksi, tab navigasi, selector filter, input pencarian, kartu kanban, kartu inbox, kartu project & epic.
- **Kanban & Eisenhower Feedback**:
  - Animasi transisi halus dan highlight bingkai putus-putus berbingkai (`border-radius: 8px`) saat kartu di-drag antar kolom atau kuadran (`.drag-over`).
  - Kursor kartu kanban eksplisit menggunakan `cursor: grab` dan `cursor: grabbing`.
  - Penambahan indikator kartu kosong yang rapi (`.kanban-column-empty`) untuk kolom Kanban dan kuadran Eisenhower yang belum memiliki tugas.
- **Native Obsidian Design**:
  - Menghapus `@import` font eksternal Google Fonts (menggunakan font sistem Obsidian `var(--font-interface)`).
  - Menghapus gradient ungu/indigo artifisial dan menggantinya dengan Obsidian CSS tokens (`var(--interactive-accent)`, `var(--background-modifier-*)`).

### Bug Fixes
- **Due Date Badge Legibility & Contrast**:
  - Memperbaiki warna badge tenggat waktu di `timeUtils.ts`: mengganti variabel Obsidian `--background-modifier-error` (yang solid merah opak) dengan latar translucent alpha `rgba(239, 68, 68, 0.16)` sehingga teks status tanggal (Overdue/Today/Tomorrow) selalu terbaca jelas dengan kontras tinggi di tema Dark maupun Light.
- **Due Date Visibility in Projects & Epics**:
  - Menambahkan kolom Due Date lengkap beserta header tabel pada daftar issue di tampilan detail Proyek dan detail Epic (`ProjectsView.tsx`).
- **Kanban Card Meta Alignment**:
  - Membatasi lebar pill project (`max-width: 140px`) dengan ellipsis agar badge due date tidak tergeser keluar kartu.
- **Badge Vertical Stretch Fix**:
  - Memperbaiki flex alignment dan menambahkan `height: fit-content` pada `.badge` agar pill status tugas (`Todo`, `Active`, `Blocked`, `Done`) tidak meregang secara vertikal.

---
## [0.8.2] - 2026-09-07

### Features & Enhancements
- **Bilingual Internationalization (i18n)**:
  - Dukungan penuh dwibahasa (**English** & **Bahasa Indonesia**) di seluruh tampilan: Dashboard, Kanban Board, Eisenhower Matrix, Inbox, Heatmap Produktivitas, timer Pomodoro, banner refleksi/evaluasi, notifikasi WIP limit, dan modal laporan harian.
  - Pengaturan bahasa dapat diubah secara instan melalui menu *Settings -> Flow Tracker -> Language*.
  - Arsitektur i18n lokal, type-safe, zero-dependency via `src/utils/i18n.ts` dengan dukungan interpolasi parameter.
- **Quick Capture Task Modal**:
  - Dialog modal native Obsidian (`QuickCaptureModal`) untuk mencatat ide dan tugas baru ke Inbox secara instan tanpa perlu membuka view utama.
  - Terintegrasi dengan Obsidian Command Palette (`Quick Capture Task to Inbox`) dan dapat dipasang ke hotkey global.
- **Global Pomodoro Commands**:
  - Perintah global Obsidian baru untuk kontrol timer dari mana saja: `Toggle Focus Timer (Start / Pause)` dan `Skip Break Timer`.

### Testing & Maintenance
- Menambahkan test suite `tests/i18n.test.ts` (46 total tests passing).
- Memperbaiki penanganan `styles.css` pada pipeline rilis `release.ts`.

---
## [0.8.1] - 2026-09-05

### Performance & Optimizations
- **Board & Drag-and-Drop Performance**:
  - Precomputing smart score dan unresolved blocker mapping menggunakan `useMemo` sehingga tidak lagi dihitung berulang-ulang di setiap render kolom.
  - Memoisasi pengelompokan issue kolom Kanban dan kuadran Eisenhower.
  - Mengekstrak kartu issue ke komponen `KanbanCard` dengan `React.memo`, mencegah stutter dan re-render seluruh kartu saat drag-and-drop hover antar kolom.
- **Productivity & Heatmap Lookup**:
  - Mengganti pencarian linear berulang pada rendering 84 sel heatmap menjadi $O(1)$ Map lookups instan (`completedCountByDate`, `dailyNotesByDate`).
  - Mengoptimalkan kalkulasi statistik streak dan tasks taken dengan indexing `issuesById` Map ($O(1)$).
- **Dashboard & Timer Memoization**:
  - Memoisasi derived metrics, kalkulasi estimasi jadwal kerja, dan status pomodoro di `DashboardView`.
  - Memindahkan helper storage ke luar siklus render di `PomodoroTimer`.

### Refactoring & Architecture
- **Deduplikasi Daily Report & Standup Parser**:
  - Menyatukan ~400 baris duplikasi kode parsing standup, timeline aktivitas harian, dan markdown daily note antara `DailyReportModalView` dan `WeeklyReviewView` ke modul bersama `src/utils/dailyReportParser.tsx`.
  - Mengurangi ukuran bundle produksi plugin dari 441.4 KB menjadi 438.8 KB.
- **Ekstraksi Subtask & Checklist Utils**:
  - Memindahkan fungsi murni manipulasi checklist markdown (`parseSubtasks`, `toggleSubtask`, `deleteSubtask`, `addSubtask`, `editSubtask`, `parseNotes`, `updateNotesInBody`) dari `IssueEditor` ke `src/utils/subtaskUtils.ts`.
- **Plugin Lifecycle & Memory Cleanup**:
  - Menambahkan pembatalan timer debounce dan pembersihan listener array pada fungsi `onunload()` di `main.ts` untuk mencegah *memory leaks* saat plugin di-reload.
- **Type Safety**:
  - Memperbaiki pengetikan fungsi `calculateStreak` di `timeUtils.ts` agar aman dari tipe `any[]`.

### Testing & Tooling
- Menambahkan 7 unit tests baru untuk manipulasi subtask markdown di `tests/subtaskUtils.test.ts` (total 39/39 tests passing di Bun).

---
## [0.8.0] - 2026-08-16

### Added
- **AI Agent Specification Auto-Generator (`FLOW.md`)**:
  - Otomatis membuat dan memperbarui file panduan `FLOW.md` di root vault saat plugin dimuat, diperbarui, atau saat pengaturan folder diubah.
  - Berisi struktur routing folder aktif, spesifikasi lengkap YAML Frontmatter (*Issues, Projects, Epics, Daily Notes*), enum validasi, batas WIP, dan *Action Recipes Cookbook* langkah-demi-langkah bagi AI coding assistant (Claude, Cursor, Antigravity, Copilot, ChatGPT).
  - **Auto-Sync `AGENTS.md`**: Otomatis mendeteksi file `AGENTS.md` di vault dan menyisipkan referensi ke `FLOW.md` agar agent langsung tersinkronisasi tanpa konfigurasi manual.
- **Quick Triage Actions**:
  - **Convert Selection to Issue**: Sorot teks atau checklist di editor mana pun (misal `INBOX.md`), klik kanan  *"Flow: Convert Selection to Issue"*. Otomatis membuat kartu issue baru di folder `Issues/` dan mengganti teks di editor dengan wikilink `[[ISSUE-xxx|Title]]`.
  - **Convert Note to Issue**: Mengonversi file catatan biasa langsung menjadi kartu Flow Issue terstruktur via context menu file atau Command Palette.
- **Vault Issue Validator & Diagnostics**:
  - Perintah baru `Flow Tracker: Validate Vault Issues & Schema` untuk mendeteksi error status, missing fields (`id`, `title`), dan broken dependency pada `blockedBy`.
- **AI & Agent Integration Settings**:
  - Section pengaturan baru di Tab Settings Flow untuk mengatur toggle auto-generate, custom path, dan tombol regenerate manual.

---
## [0.7.1] - 2026-07-12

### Added
- **Task Activation Flow**: Pengganti *Morning Activation* yang berupa *wizard* interaktif untuk membantu perencanaan harian. Muncul otomatis di awal hari kerja atau dapat dipanggil manual kapan saja.

---
## [0.7.0] - 2026-06-18

### Added
- **Task Activation Flow**: Fitur aktivasi tugas (pengganti *Morning Activation*) berupa *wizard* yang membantu mengambil langkah pertama. Akan muncul otomatis hanya jika dibuka *sebelum* jam kerja dimulai, atau bisa dipicu manual lewat tombol "Plan Your Day" di Dashboard kapan saja.
- **Energy-based Smart Recommendations**: Menggabungkan pilihan level energi Anda ke dalam algoritma rekomendasi Smart Score (+500 poin jika energinya cocok), agar tugas yang direkomendasikan selalu sesuai dengan kapasitas mental Anda saat itu.
- **Native SVG Badges**: Tampilan komponen UI dan lencana rekomendasi kini menggunakan Native SVG Icons Obsidian yang lebih profesional (menggantikan *emoji*).

---
## [0.6.4] - 2026-06-18

### Added
- **Global Pomodoro Widget**: Pomodoro timer kini bisa melayang (global widget) dan digunakan di luar tab utama.
- **Auto-Prefix File Names**: Setiap pembuatan file Project dan Epic baru akan otomatis memiliki prefix `[Project]` atau `[Epic]` pada nama filenya untuk mempermudah pencarian.
- **Factory Reset**: Menambahkan tombol bahaya "Factory Reset" di halaman Settings Obsidian native untuk keperluan *testing* / reset plugin.

### Changed
- **Strict Smart Score Sync**: Antrean (Focus Queue) pada Pomodoro Timer kini secara agresif akan me-reset dan mengurutkan ulang dirinya sendiri untuk *selalu* sesuai dengan urutan Smart Score tertinggi di "Today's Plan". Urutan hanya dipertahankan sementara (*preserved*) jika ada sesi *timer* yang sedang berjalan aktif.
- **Human-Readable Titles**: UI pada Kanban Board dan Modal kini memunculkan judul asli Project/Epic sebagai pengganti *raw ID* yang sulit dibaca.
- **Wikilink Graph Relations**: Mengubah cara plugin menyimpan *frontmatter* relasi menjadi *Wikilink* penuh (`[[path|ID]]`) agar Graph View Obsidian terbaca dengan sempurna (tanpa *node* abu-abu / *unresolved link*).

### Fixed
- **Clean Task Hierarchy**: Memaksa hierarki Task -> Epic -> Project yang ketat. Jika task sudah tertaut pada Epic, relasi ke Project tidak lagi ditulis agar Graph View tidak semrawut (menghindari koneksi segitiga/ganda).
- **Epic Dropdown Filter**: Memperbaiki filter pada dropdown pilihan Epic di layar pembuatan Issue yang sebelumnya bocor (menampilkan epic dari project lain).
- **Form Reset**: Memperbaiki bug di mana modal "New Issue" memuat *state* form sisa dari penambahan issue sebelumnya.
- **Timezone Accuracy**: Memperbaiki bug *timestamp* yang menggunakan UTC pada laporan Daily Note. Sekarang semuanya menggunakan `window.moment()` untuk menyesuaikan dengan zona waktu lokal secara akurat.

---
## [0.6.3] - 2026-06-14

### Added
- **Settings Page**: remove redundant Settings tab for configuring the plugin.

## [0.6.2] - 2026-06-14

### Added
- **Klasifikasi File di Subfolder Terstruktur**: Memperbaiki bug di mana catatan/dokumen umum (tanpa `frontmatter.type`) yang diletakkan di dalam subfolder `Docs/` pada project, epic, atau folder issues terdeteksi salah sebagai task/issue dan bocor masuk ke dalam daftar tugas (Backlog/Task List). Klasifikasi kini memprioritaskan pencocokan segment jalur folder seperti `/docs/`, `/tasks/`, dan `/epics/`.

## [0.6.1] - 2026-06-14

### Added
- **ADHD-Friendly Micro-Prompts**: Placeholder refleksi harian dinamis dan deterministik berbasis tanggal untuk mencegah *blank-page anxiety*.
- **Widget Status Bar dengan Judul Tugas**: Menampilkan judul tugas aktif yang sedang dikerjakan secara langsung di status bar timer.
- **Visual Time Proximity Badges**: Indikator warna hangat/urgensi dinamis untuk tanggal jatuh tempo tugas di Kanban Board dan Backlog.
- **Celebration Confetti Animations**: Ledakan animasi partikel konfeti (canvas murni) saat sesi fokus selesai atau tugas dipindahkan ke "Done".
- **ADHD Capacity Buffer**: Rekomendasi "Kapasitas Aman" (buffer 30%) untuk menghindari *over-planning* saat perencanaan mingguan.
- **Pencatatan Pomodoro Hari Itu Saja (Isolasi)**: Menghitung total pomodoro harian langsung dari log aktivitas catatan harian untuk mencegah progress bar terisi otomatis akibat properti salinan/stale.
- **Stepper Wizard Weekly Review**: Alur perencanaan mingguan dirancang ulang menggunakan timeline 3 langkah (Closeout, Capacity, Planning) yang terstruktur.
- **Kartu Riwayat Performa Mingguan (Weekly History)**: Riwayat produktivitas dipisah ke kartu tersendiri dengan rentang tanggal lengkap untuk 4 minggu ke belakang.
- **Banner Pengingat Review Dinamis**: Banner otomatis hilang saat review mingguan selesai dicatat, dan memiliki opsi tutup (dismiss) per minggu.
- **Closeout Checklist Interaktif**: Tugas Overdue dan Blocked dapat langsung diklik untuk diedit, serta ditambahkan tautan cepat ke Inbox dan Backlog.
- **Penyatuan Siklus Baru**: Tombol pembuat siklus baru digabungkan ke tombol penyelesaian review demi alur kerja yang lebih ringkas.

### Changed
- **Dashboard Stats Simplification**: Menyederhanakan metrics progress bar dashboard (hanya menampilkan persentase) dan sub-header rencana harian untuk tampilan yang lebih bersih dan minimalis.

---

## [0.6.0] - 2026-06-14

### Added
- **Related Documents per Task**: Dari Task Editor, kini bisa membuat Canvas atau Note baru langsung terhubung ke task. Dokumen disimpan di folder `Docs/` milik project/epic terkait dan diberi nama otomatis `ISSUE-XXX - Nama.md/canvas`.
- **Vault Scan Approach for Related Docs**: Relasi dokumen dideteksi berdasarkan naming convention (`ISSUE-XXX - *`) tanpa perlu menyimpan frontmatter tambahan  lebih simpel, lebih robust, tidak ada race condition.
- **Real-time Related Docs List**: Daftar dokumen terkait auto-refresh saat ada file dibuat, dihapus, atau di-rename di vault (menggunakan vault event listeners).
- **Delete Button per Document**: Tombol  di setiap item dokumen untuk menghapus file dari vault dengan dialog konfirmasi.
- **Archive Tab (Projects & Epics)**: Tab "Archive" di ProjectsView untuk melihat project dan epic yang sudah berstatus `archived`.
- **Auto-Archive Tasks**: Sistem otomatis memindahkan task berstatus `done` ke folder arsip setelah jumlah hari yang dikonfigurasi di Settings.
- **Edit Project & Epic**: Tombol edit di modal detail project/epic untuk mengubah nama, status, dan properti lainnya.
- **Epic Status**: Menambahkan field `status` (`active` | `archived`) pada Epic.

### Changed
- **Related Documents Icons**: Mengganti emoji () dengan ikon SVG konsisten dari Lucide (`FileText` untuk note, `Network` untuk canvas) agar seragam dengan seluruh UI plugin.
- **Blank File on Create**: File task, project, epic, note, dan canvas yang baru dibuat kini tidak lagi memiliki heading placeholder (`# Judul`). File dibuat kosong agar user bebas menulis konten sendiri.
- **triggerChange() Debounce**: UI refresh di-debounce 250ms untuk mengelompokkan event perubahan file yang terjadi cepat berturut-turut (misal saat batch archive) menjadi satu re-render saja  meningkatkan performa signifikan pada vault besar.

### Fixed
- **Blank UI on Window Reload**: Menambahkan listener `metadataCache.on('resolved')` sehingga UI selalu terisi data setelah Obsidian selesai mengindeks vault, menghilangkan kondisi tampilan kosong saat Ctrl+R.
- **editingIssue Sync**: State task yang sedang diedit di modal kini disinkronkan otomatis saat index vault diperbarui, mencegah data stale pada editor yang sedang terbuka.
- **Electron `prompt()` Compatibility**: Mengganti semua pemanggilan `prompt()` dengan UI input inline berbasis React state  kompatibel penuh dengan lingkungan Electron/Obsidian yang memblokir `prompt()`.
- **Epic Autofill on Edit**: Field epic kini ter-isi otomatis saat membuka task editor untuk task yang sudah memiliki epic terdaftar.
- **completedDate Preservation**: Field `completedDate` kini dijaga dengan benar saat menyimpan ulang task yang sudah berstatus `done`.

---

## [0.5.2] - 2026-06-13

### Changed
- **Typography & Design System**: Cleaned up the root typography scale (CSS variables) to follow modern UI density standards. Standardized all main tab headers (`<h2>`) across all views to use a consistent `--flow-text-4xl` size. Adjusted the Pomodoro timer font size ratios for better visual hierarchy. Replaced deprecated `--flow-text-md` variables with `--flow-text-base` across `styles.css`.
- **Productivity & Dashboard Layout**: Removed the redundant "Done Task Heatmap" from the Dashboard. Redesigned the "Current Streak" widget into a subtle stat-card on the Productivity tab. Implemented `ResizeObserver` on the Recent Daily Reports grid to make it fully responsive (dynamically wrapping 4 to 7 cards depending on parent width).
- **Task List/Backlog Readability**: Increased row padding and explicitly set the base font size to resolve cramped and illegible data rows.

### Fixed
- **Streak Calculation**: Corrected the logic so active days and streaks now increment correctly (by 1 day) whenever at least one task is completed, regardless of whether a Pomodoro timer was used.

---
## [0.5.1] - 2026-06-11

### Fixed
- **Recent Daily Reports Sort Order**: Fixed the Productivity tab showing oldest daily notes first when paginating. Removed an erroneous `.reverse()` call so that the most recent notes always appear on the first page (newest-first order).

---

## [0.5.0] - 2026-06-11

### Added
- **Daily Report Modal**: New Obsidian modal (`DailyReportModal`) that opens a full daily report for any selected date. Displays session stats (focus pomodoros, total duration, completion %), completed and uncompleted task cards, a Refleksi Harian section, and an activity log with two tabs  Kronologis (timeline) and Report (task sessions with precise `startTime  endTime` and action notes). Footer provides quick access to the raw daily note and dashboard navigation.
- **Review Tab**: New dedicated **Review** main tab with two sub-tabs:
  - **Daily Review**  date switcher (`< Prev Day / Today / Next Day >`) for reviewing any date. Includes a reflection form (Yang Berjalan Lancar, Hambatan / Kendala, Rencana Besok) with **1-second auto-save** (no manual save button). Also shows a daily report card with task sessions and clickable task titles that open the IssueEditor.
  - **Weekly Review**  scope planner for `thisWeek` tasks and a weekly overview summary.
- **Workday Timeline**: New visual progress bar on the Dashboard showing elapsed time, planned focus time, free buffer, and overload segments across the work day. A floating "Sekarang (HH:MM)" badge with a vertical marker line shows real-time position.
- **Daily Focus Progress Bar**: Secondary progress bar below the timeline tracking logged vs estimated pomodoros for today's tasks.
- **Focus Queue**: Pomodoro sidebar now features a 3-slot Focus Queue that auto-fills from today's active tasks, auto-ejects completed tasks, and compacts empty slots. Supports manual slot assignment via dropdown.
- **Deep Work Overlay**: Full-screen blur overlay activates during focus sessions to minimize distraction. Includes active task display, session goal input, and minimize/restore control.
- **Session Goal**: Before each focus block, users can set a concrete intention ("Saya akan fokus sampai...") that is displayed during the session and cleared on completion.
- **Break Overlay**: Full-screen break overlay with breathing animation (inhale/hold/exhale cycle), offline activity prompt, and skip/dismiss controls.
- **Alarm System**: Looping audio tone and pulsing button when Pomodoro timer completes. "Matikan Alarm" silences and transitions to next mode.
- **Recommended Task Widget**: Suggests the highest Smart-Score backlog task not yet in the Focus Queue when capacity allows.
- **Target Tercapai Button**: One-click manual session completion inside the Deep Work overlay  increments logged pomodoro count, logs activity to daily note, and transitions to break mode with a celebratory C5-E5-G5 audio arpeggio.
- **Inbox Tab**: Quick-capture input for raw tasks and thoughts, processed later into issues via the IssueEditor.
- **End-of-Day Celebration State**: When the scheduled work day ends, Today's Plan is replaced with a celebration card showing daily stats and a shortcut to write daily reflection.

### Changed
- **Dashboard Redesign**: Replaced the old multi-column stat widget grid with a focused single-column layout (max 800px). Today's Plan and Timeline are the primary focal points; project health cards removed.
- **Reflection Labels**: All-caps saturated colored text in the Daily Report reflection section replaced with soft pill badges (muted background, low-opacity border) for cleaner visual hierarchy.
- **Report Tab**: Renamed "Daily Standup" tab to "Report"; now set as the default active tab in the Review view.
- **Recent Daily Reports Sort Order**: List on Dashboard now sorted oldest-to-newest (ascending chronological order).
- **Timer State Persistence**: Pomodoro timer state (mode, running/paused, remaining time) persisted to `localStorage` using epoch targets  survives sidebar unmounts and plugin reloads.
- **obsidianUtils**: `readReflectionFromDailyNote` and `saveReflectionToDailyNote` now accept an optional `dateStr` parameter for reading/writing to any date's daily note (not just today).

### Fixed
- **Modal Close Button**: Native Obsidian close button hidden inside `DailyReportModal`; replaced with a custom React button precisely positioned in the modal header with proper sizing, border, and hover effect.
- **Emoji Cleanup**: Removed `` from timing badges and `` from task completion text in both the Daily Report Modal and Weekly Review views.
- **Auto-Clean Stale Tasks**: Completed tasks with a `completedDate` from a previous day are automatically removed from Today's Plan on Dashboard load.

---

## [0.4.4] - 2026-06-11

### Fixed
- **Task List View Pagination**: Fixed a layout bug where the pagination controls at the bottom of the table were pushed off-screen and hidden. Refactored the table component to keep the table header and pagination controls sticky at the top/bottom while making only the rows container scrollable.
- **Pomodoro Timer Load Glitch**: Fixed a race condition/glitch on reload where the timer was reset to 25:00 and paused. Guarded the auto-cleanup and sync effects from executing before the issues list index is successfully loaded.

---

## [0.4.3] - 2026-06-11

### Added
- **Interactive Tag Pills (Chips)**: Redesigned the tags input field to automatically convert text to separate interactive tag pills upon typing a comma, pressing `Enter` or `Tab`, or clicking outside the input (blur). Supports removing individual tags by clicking their delete (``) icon or pressing `Backspace` on an empty input.
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
