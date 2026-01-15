# CURRENT_STATUS.md

## 📍 Crystal Ball Mini - Current Status

**Last Updated:** 2026-01-15  
**Current Version:** 5.1 (V5.2 in progress)

-----

# 🔥 SESSION: January 15, 2026

## V5.2 Fixes In Progress

### 1. 🔴 Mobile Field Sizing

- Fields bunching up on phone (portrait mode)
- Need better responsive CSS for small screens
- Fix: `grid-template-columns` collapse to single column on narrow screens

### 2. 🔴 Simplify ADD/EDIT UI

- Remove “Event Entry” header with ADD/EDIT toggle at top - REDUNDANT
- Already have “Enter Record” and “Clear” at bottom
- Move EDIT to button row: `[✓ Enter Record] [Clear] [✏️ Edit]`
- Or: Button text changes to “Update Record” when editing

### 3. 🔴 Table Columns - Separate LEG from Event

- Currently: `ID` shows `L1-S1` (leg embedded)
- Should be: `LEG | Event | Category | Type | Start...`
- Cleaner, sortable

### 4. 🟡 Run ID Validation

- Should be required (or warn if empty)
- Data becomes orphaned without it
- Need ability to update Run ID on existing records

### 5. 🟡 Delete Section Escape

- No way to cancel/escape once delete section shows
- Add Cancel button
- Switching to ADD mode should fully reset

-----

## ✅ V5.1 Completed This Session

- ✅ All Entry Tab functions implemented
- ✅ `submitRecord()` working
- ✅ `clearForm()` working
- ✅ Time auto-calc (Start+Duration→End, handles overnight)
- ✅ S/D toggle for Stop vs Drive events
- ✅ Leg/Event navigation with ◀▶ buttons
- ✅ Edit mode with `loadRecord()`
- ✅ Delete with confirmation checkbox
- ✅ Table renders entered records
- ✅ Click row to edit
- ✅ “Theory” → “Prophecy” renamed
- ✅ “— Regular” → “Event” in Stop Type

-----

## 🔮 GitHub Workflow CONFIRMED

**Self-bootstrapping works!**

1. Paste: `https://raw.githubusercontent.com/emil135k/crystalballmini/main/CRYSTAL_BALL_INDEX.md`
1. Claude fetches it
1. All other URLs now in context
1. Claude can fetch any file

-----

# 📜 PREVIOUS SESSIONS

## January 14, 2026

- Documented missing Entry Tab functions
- Created AI reference files for GitHub
- Verified web_fetch works with raw GitHub URLs
- Identified “Theory” → “Prophecy” rename
- Emil recovering from H3N2, truck getting transmission in Louisville

## Earlier Sessions

- Built complete Trip Generator (V5)
- Fixed ETA calculation bugs
- Added shipper/receiver date comparison
- Implemented FIFO recap visualization

-----

# 📊 TAB STATUS

|Tab     |Status             |Notes                          |
|--------|-------------------|-------------------------------|
|Entry   |🟡 V5.2 fixes needed|Functions work, UI needs polish|
|Prophecy|✅ Working          |Trip generator functional      |
|Timeline|⚠️ Basic            |Shows events, needs enhancement|
|Recaps  |⚠️ Basic            |Placeholder, needs real calc   |
|Settings|✅ Working          |Saves to localStorage          |

-----

# 🐛 KNOWN BUGS

|Bug                   |Severity|Status  |
|----------------------|--------|--------|
|Mobile fields bunching|🔴 High  |V5.2 fix|
|ADD/EDIT redundant    |🟡 Medium|V5.2 fix|
|No delete escape      |🟡 Medium|V5.2 fix|
|Run ID not required   |🟡 Medium|V5.2 fix|

-----

# 💡 BACKLOG

- [ ] Voice input for hands-free entry
- [ ] Integration with actual ELD data
- [ ] Multi-run planning (chain loads)
- [ ] Fuel stop optimization
- [ ] PWA manifest and service worker
- [ ] Biopsy feature (actual vs predicted)

-----

*See SESSION_2026-01-15.md for detailed session notes*