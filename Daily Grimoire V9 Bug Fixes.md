# Daily Grimoire V9 — Bug Fix Log

Debugging session: 2026-03-16

All fixes applied directly to `daily-grimoire-v9.html`.

---

## Bug 1 — `toggleTask` Infinite Recursion

**Symptom:** Checking off a daily deed caused a stack overflow, freezing the page. Auto-punch on task completion did not work.

**Root cause:** `toggleTask` was defined twice using `function` declarations. Because function declarations are hoisted, `_origToggleTask` captured the *second* definition rather than the first, creating a function that called itself indefinitely.

**Fix:** Removed the duplicate wrapper entirely. Merged the auto-punch logic directly into the single `toggleTask` function.

---

## Bug 2 — Bingo Free Space Never Marked, Breaking Win Detection

**Symptom:** Bingo lines through the centre square (row 2, column 2, both diagonals) could never be won, even when all other squares in the line were marked.

**Root cause:** The `marked` array was initialised as `Array(9).fill(false)`. The centre square (index 4) is a permanent free space but was never set to `true`, so any win pattern including index 4 evaluated as false.

**Fix:** Set `marked[4] = true` on card creation, on reshuffle, and when loading from localStorage. Also updated the progress counter to exclude the free space from the marked count.

---

## Bug 3 — Unmarking a Bingo Square Did Not Sync Linked Quest

**Symptom:** If a bingo square was unmarked after being manually marked, the linked quest card remained completed and could not be re-punched.

**Root cause:** `markBingoSquare` only synced in one direction — marking a square completed its linked quest, but the reverse path was missing.

**Fix:** Added the reverse sync: when a square is unmarked, the linked quest card's `completed` flag is reset to `false` and its punch count is decremented by one.

---

## Bug 4 — Create Bingo Modal Did Not Pre-Fill the Title

**Symptom:** Typing a title into the inline "Card Title" field on the Level Up view and clicking "+ Create Card" opened the modal with an empty title field.

**Root cause:** `openCreateBingoModal` read the inline input's value into a local variable but then set the modal field to `titleInput || ''`, which effectively always passed an empty string when the intent was to pre-fill.

**Fix:** The inline title input value is now correctly transferred to the modal's title field before it opens.

---

## Bug 5 — Bingo Card Edit Path Had Dead / Broken Pool Logic

**Symptom:** Editing a bingo card's title, colour, or reward worked, but pool comparison logic was nonsensical.

**Root cause:** The code computed `oldPoolStr = card.pool.join('|')` *after* already setting `card.pool = pool` (the new value), making the comparison always equal. The subsequent line set `card.pool = pool` a second time. The entire pool comparison block was dead code.

**Fix:** Removed the broken comparison. The edit path now cleanly updates `title`, `color`, `colorIdx`, `reward`, and `pool` without redundant or contradictory statements.

---

## Bug 6 — `checkDateChange()` Never Called on Page Load

**Symptom:** The history auto-archive feature (which archives the previous day when the app opens on a new calendar day) never fired.

**Root cause:** `checkDateChange()` existed and was correct but was missing from the init sequence at the bottom of the script.

**Fix:** Added `checkDateChange()` as the first call in the init sequence, before `load()` and `render()`.

---

## Bug 7 — Quest Card Edit Modal Had No Colour Picker

**Symptom:** Opening the edit modal for a quest card showed no way to change its colour. The colour was frozen at whatever was assigned on creation.

**Root cause:** The card edit modal HTML had fields for name, target, and reward but no colour picker. The supporting JS functions (`openCardModal`, `saveCardModal`) did not handle colour.

**Fix:** Added a six-dot colour picker to the card edit modal HTML. Added `selectCardColor()` function and wired `openCardModal` (pre-selects current colour) and `saveCardModal` (saves chosen colour) accordingly.

---

## Bug 8 — Bingo Celebration Showed Generic Text Instead of Card Reward

**Symptom:** Completing a bingo card always showed the message "Bingo achieved — a line of glory across the tome!" regardless of what reward text was set on the card.

**Root cause:** `showBingoCelebration` hardcoded the reward string rather than reading `card.reward`.

**Fix:** `showBingoCelebration` now displays `card.reward` if set, falling back to the default text only when no reward is defined.

---

## Bug 9 — `deleteTask` Did Not Persist When Clearing the Last Task

**Symptom:** Deleting a task when it was the only task in a time block appeared to clear it visually, but the blank task was not saved to localStorage. Refreshing the page could restore the old content.

**Root cause:** The branch of `deleteTask` that handles the single-task case called `render()` but omitted `save()`.

**Fix:** Added `save()` after `render()` in the single-task branch.

---

## Bug 10 — Bingo Win via Quest Completion Showed No Bingo Celebration

**Symptom:** Completing a quest that triggered a bingo win showed only the quest celebration overlay. The bingo celebration never appeared, even though the card was correctly marked complete.

**Root cause:** In `punchCard()`, when a bingo win was detected, the code issued a toast notification and then showed `showCelebration(card)` (the quest overlay). `showBingoCelebration` was never called.

**Fix:** When a bingo win is triggered from quest completion, the quest celebration fires first (at 400 ms), followed by the bingo celebration (at 2000 ms), giving the user time to read both.
