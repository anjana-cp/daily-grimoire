# Changelog

All notable changes to The Daily Grimoire are recorded here.
Format: grimoire voice introduction per version, followed by a clear list of changes.

---

## [v9] — 2026-03-05

*The grimoire reaches beyond the device. A sync system is woven into the spine of the book — thy data now travels with thee, written into GitHub and pulled back whenever thou returnest from another machine.*

- **Added** GitHub Sync system — push all data to a `data.json` file in your GitHub repo
- **Added** Auto-pull on load — if the cloud copy is newer than local, data is pulled automatically
- **Added** ⚙ Settings modal — enter GitHub username, repo, token and file path
- **Added** Test Connection button to verify settings before saving
- **Added** Sync status bar with colour-coded dot indicator (grey / gold pulsing / green / red)
- **Added** Last synced timestamp shown in sync bar
- **Fixed** `loadBingoCards()` missing from init sequence — bingo cards now load correctly on open

---

## [v8] — 2026-03-05

*The quest cards are refined. The bingo card reveals its reward in the sacred centre, and the bonds between quests and their parent bingo card are made clear at a glance.*

- **Changed** Quest cards from bingo now show the bingo card's title as a badge instead of "From Bingo"
- **Changed** Bingo-sourced quest cards now default to 14 milestones (matching normal quest behaviour)
- **Changed** Bingo-sourced quest cards are fully editable — name, target, colour
- **Removed** Reward field hidden on bingo-sourced quest cards (not applicable)
- **Added** Reward field on bingo cards — displayed in the free space centre instead of "FREE"
- **Added** Two-line wrapping for long reward text in the free space medallion

---

## [v7] — 2026-03-05

*The Level Up board and the Quests are no longer separate kingdoms. Creating a bingo card now plants a quest for each of its eight squares — and when a quest is fulfilled, its square marks itself without ceremony.*

- **Added** Auto-create 8 quest cards when a new bingo card is created — one per non-free square
- **Added** Bingo square auto-marks when its linked quest is completed
- **Added** Quest auto-completes when its linked bingo square is manually marked
- **Added** Bingo win detection triggered from quest completion
- **Added** Toast notification when a bingo square is marked via quest completion
- **Added** Reshuffle removes old linked quests and creates fresh ones for new squares
- **Added** Deleting a bingo card removes all its linked quests
- **Added** `getBingoTitle()` helper for resolving bingo card name from ID
- **Fixed** `bingoSource` field normalised in `loadPunchCards()` to handle missing data

---

## [v6] — 2026-03-05

*The tome now remembers. Each day is chronicled automatically when a new one begins, and the history of all past days can be opened, read and reflected upon within The Chronicle.*

- **Added** History system — auto-archives previous day when a new date is detected on load
- **Added** 📜 Save Today button — manually snapshot the current day to history at any time
- **Added** ⟳ History button — opens The Chronicle modal
- **Added** History modal — lists all past days newest first with deed completion stats
- **Added** Day viewer — click any past day to read its full time blocks and deeds (read-only)
- **Added** Manual save badge — days saved manually are labelled to distinguish from auto-saves
- **Added** `todayKey()`, `formatDateKey()`, `loadHistory()` helper functions
- **Fixed** Unicode escape sequences causing full script parse failure
- **Fixed** Broken quote escaping in `renderHistoryList` onclick string

---

## [v5] — 2026-03-04

*The free space is no longer empty. A unicorn stands in the centre of every bingo card — drawn in the style of the 1804 Hunt Sampler — surrounded by watercolour leaf petals and a cipher wheel ring. The Punch Cards view is renamed Quests.*

- **Changed** Toggle label "Punch Cards" renamed to "Quests"
- **Added** Illustrated SVG free space medallion — unicorn silhouette, watercolour leaf petals, cross-stitch dot grid, cipher wheel dashed ring
- **Added** `getFreeSpaceContent()` function — generates SVG dynamically per card
- **Changed** Free space square CSS updated to display SVG rather than text

---

## [v4] — 2026-03-04

*A third view enters the tome — Level Up. Bingo cards can be conjured from a pool of goals, filled at random, and completed square by square. A line of victory across the card triggers the celebration.*

- **Added** Level Up view — third toggle segment alongside Daily Grimoire and Quests
- **Added** Bingo card creation — 3×3 grid, 8 items drawn at random from a user-defined pool
- **Added** Free space in the centre square
- **Added** Click to mark/unmark squares
- **Added** Win detection — rows, columns and both diagonals
- **Added** Completion celebration reusing the existing overlay
- **Added** Click card to open edit modal — change title, colour, pool
- **Added** Reshuffle button — regenerates grid from pool, resets progress
- **Added** Delete from modal
- **Added** 6 watercolour colour variants with colour picker in modal
- **Added** `shuffleArray()`, `buildSquares()`, `markBingoSquare()` functions
- **Added** localStorage persistence under `grimoire-bingo` key

---

## [v3] — 2026-03-04

*The quest cards are refined. The delete button is banished from the card face — clicking the card now opens an edit chamber. The linked state is made implicit rather than declared.*

- **Changed** Removed delete button from punch card face
- **Added** Click card to open edit/delete modal — colour-matched accent bar, editable name/target/reward
- **Changed** "+ Add to Today" button always shown — no linked state text or styling
- **Fixed** `esc()` function made null-safe — no longer throws on undefined card properties
- **Fixed** `loadPunchCards()` normalises all fields on load — handles cards saved before new fields existed

---

## [v2] — 2026-03-04

*The Quests view takes shape. Punch cards are born — each a watercolour card of milestones, linked to the daily grimoire, completing themselves when deeds are ticked.*

- **Added** Quests (Punch Cards) view — second toggle alongside Daily Grimoire
- **Added** Create quests with name, milestone target and reward
- **Added** 6 watercolour colour variants cycling automatically
- **Added** Punch and unpunch individual milestone holes
- **Added** + Add to Today — adds quest name as a deed in the daily blocks
- **Added** Auto-punch when a linked daily deed is ticked complete
- **Added** Completion celebration overlay with gold glow animation
- **Added** Toast notifications
- **Added** localStorage persistence under `grimoire-punchcards` key
- **Added** `switchView()` function for view toggling

---

## [v1] — 2026-03-03

*The grimoire is opened for the first time. Parchment, ink, illustrated borders — the dragon climbs, the owl watches, the raven glistens. Time blocks await their deeds.*

- **Added** Daily Grimoire time-blocking view — 8am to 8pm by default
- **Added** Add/remove hour blocks before and after
- **Added** Multiple deeds per time block with Enter to add
- **Added** Mark deeds complete — struck through in forest green
- **Added** Drag deeds between time blocks (tasks only — time labels fixed)
- **Added** Progress bar — deeds completed vs total
- **Added** Day name input with today's date as placeholder
- **Added** Illustrated SVG borders — dragon, sword, barn owl, raven, iris, bat, butterfly, skulls, potions, open book, candles, cross-stitch ornaments
- **Added** Watercolour Illuminated style — wash fills with feGaussianBlur, ink linework via feDisplacementMap
- **Added** localStorage persistence — auto-saves on every change
- **Added** Responsive layout — border panels hidden on mobile
- **Added** Print styles — controls hidden, clean output
- **Added** Accessibility — aria labels, aria-live progress bar
- **Added** UnifrakturMaguntia, Cinzel, IM Fell English typefaces
