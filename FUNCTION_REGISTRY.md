# FUNCTION_REGISTRY.md

## 🔧 Crystal Ball Mini - Function Reference

**Last Updated:** 2026-01-14  
**Version:** 5.0

-----

## ✅ WORKING FUNCTIONS

### Core Navigation

```javascript
switchTab(tabName)
// Switches between Entry, Prophecy, Timeline, Recaps, Settings
// Called by: tab buttons onclick
```

### Trip Generator (Prophecy Tab)

```javascript
generateFullTrip()
// Main function - creates complete trip timeline
// Reads: all form inputs (distances, dates, times)
// Writes: tripState.generatedTrip, displays results

generateLeg({ legNum, legMiles, startTime, avgSpeed, ... })
// Creates one leg's events (drives + stops)
// Returns: leg object with events array

displayTrip(trip)
// Renders trip to UI (summary, legs, timeline table)
// Updates: all display elements

setDrivePace(hours, minutes)
// Sets daily drive target (8:45, 10:00, 11:00)
// Updates: tripState.drivePaceMin, preview

updateTripPreview()
// Live preview of legs/miles/arrival
// Called by: distance and speed inputs
```

### Storage

```javascript
saveToStorage()
// Saves state to localStorage
// Key: 'crystalBallMini_v5'

loadFromStorage()
// Loads state from localStorage
// Returns: boolean (success)

exportJSON()
// Downloads state + trip as JSON file

importJSON(event)
// Uploads JSON file, merges into state
```

### Utilities

```javascript
formatTime(minutes)        // 525 → "8 hr 45 min"
formatTimeShort(minutes)   // 525 → "8:45"
formatTimeDisplay(minutes) // 525 → "8h 45m"
formatMinutesToTime(mins)  // 525 → "08:45"
parseTime(timeStr)         // "08:45" → 525
formatDayTime(day, mins)   // (2, 360) → "Day 2 06:00"
```

### Appointment Validation

```javascript
getWindowStatus(etaMin, earlyMin, lateMin)
// Compares ETA to time window (same day)
// Returns: HTML string with status

getDateWindowStatus(etaDate, etaTimeMin, apptDate, apptEarly, apptLate)
// Compares ETA to appointment (different dates)
// Returns: HTML string with early/late/on-time
```

-----

## ❌ MISSING FUNCTIONS (Entry Tab)

### Critical - Buttons Don’t Work

```javascript
submitRecord()
// NEEDED: Save form data to state.events
// Should: validate, save, advance stop number, clear form
// Called by: "Enter Record" button

clearForm()
// NEEDED: Reset form fields
// Should: keep run/leg context, clear time/miles/comment
// Called by: "Clear" button

autoCalcFromStart()
// NEEDED: When start time changes, recalc end if duration exists

autoCalcEndTime()
// NEEDED: When duration changes, calc end = start + duration

autoCalcDuration()
// NEEDED: When end changes, calc duration = end - start
// Must handle overnight (past midnight)
```

### High Priority - Navigation

```javascript
setMode(mode)
// NEEDED: Switch between 'add' and 'edit' modes
// Should: update UI, show/hide delete section

prevLeg() / nextLeg()
// NEEDED: Navigate leg number
// Should: update display, reset stop counter for new leg

prevStop() / nextStop()
// NEEDED: Navigate stop number within leg
// Should: update display

loadRecord()
// NEEDED: Load existing record at current L#-S# for editing
// Should: populate form, switch to edit mode
```

### Medium Priority

```javascript
executeDelete()
// NEEDED: Delete record after confirmation checkbox
// Should: remove from state.events, re-render

renderEntryTable()
// NEEDED: Display entered records in table
// Should: show all events, sorted by leg/stop
```

-----

## 🎯 IMPLEMENTATION PRIORITY

1. 🔴 `submitRecord()` - Nothing works without this
1. 🔴 `autoCalcFromStart/EndTime/Duration()` - Time entry is core
1. 🔴 `clearForm()` - Reset between entries
1. 🟡 `setMode()` - ADD/EDIT toggle
1. 🟡 `prevLeg/nextLeg/prevStop/nextStop()` - Navigation
1. 🟡 `renderEntryTable()` - See what you entered
1. 🟢 `loadRecord()` - Edit existing
1. 🟢 `executeDelete()` - Remove mistakes

-----

## 📝 FUNCTION SIGNATURES (To Implement)

```javascript
// Entry Tab - Core
function submitRecord() {
  // 1. Get form values
  // 2. Validate required fields
  // 3. Build event object with L#-S#/D# id
  // 4. Push to state.events
  // 5. saveToStorage()
  // 6. Auto-advance stop number
  // 7. clearForm() but keep context
  // 8. renderEntryTable()
}

function clearForm() {
  // Clear: category, stopType, times, duration, miles, comment
  // Keep: runId, legNum, stopNum
}

function setMode(mode) {
  // mode: 'add' or 'edit'
  // Update state.currentMode
  // Toggle button styles
  // Show/hide delete section
  // Update modeStatus text
}

function autoCalcEndTime() {
  const start = parseTime(document.getElementById('inputStartTime').value);
  const duration = parseInt(document.getElementById('inputDuration').value);
  if (start !== null && duration) {
    const end = (start + duration) % (24 * 60);
    document.getElementById('inputEndTime').value = formatMinutesToTime(end);
  }
}
```

-----

*See ARCHITECTURE.md for data structures*