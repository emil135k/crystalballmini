# CURRENT_STATUS.md

## 📍 Crystal Ball Mini - Current Status

**Last Updated:** 2026-01-14  
**Session:** Emil recovering from H3N2, truck getting transmission in Louisville

-----

## ✅ WHAT’S WORKING

### Prophecy Tab (Trip Generator)

- [x] Distance inputs (deadhead + loaded)
- [x] Shipper/Receiver appointment windows
- [x] Secure/tarp time calculation
- [x] Start date/time configuration
- [x] Drive pace presets (8:45, 10:00, 11:00)
- [x] Life Happens buffer per day
- [x] Pre-trip/Post-trip time
- [x] Full trip generation with legs
- [x] ETA calculations with date comparison
- [x] Appointment window status (early/on-time/late)
- [x] Leg-by-leg breakdown display
- [x] Complete timeline table

### Recaps Tab

- [x] FIFO marble visualization concept
- [x] Cycle used/available display
- [x] 8-day window calculation

### Settings Tab

- [x] Average speed setting
- [x] Fudge factor setting
- [x] Timezone selection
- [x] Save/load settings

### Core Functions

- [x] Tab switching
- [x] localStorage persistence
- [x] Export/Import JSON
- [x] Time formatting utilities

-----

## ❌ WHAT’S BROKEN

### Entry Tab - CRITICAL

|Issue                   |Impact                            |
|------------------------|----------------------------------|
|`submitRecord()` missing|Enter button does nothing         |
|`clearForm()` missing   |Clear button does nothing         |
|Time auto-calc broken   |Can’t calculate Start/Duration/End|
|Mode toggle missing     |Can’t switch ADD/EDIT             |
|Navigation missing      |Can’t prev/next leg/stop          |

### UI Issues

|Issue                                   |Location          |
|----------------------------------------|------------------|
|“Theory” should be “Prophecy”           |Tab bar           |
|“Current Stop” should be “Current Event”|Entry form        |
|“— Regular” should be “Event”           |Stop Type dropdown|
|No S/D toggle                           |Entry form        |

-----

## 🎯 NEXT PRIORITIES

### Immediate (Session Goal)

1. [ ] Implement `submitRecord()`
1. [ ] Implement time auto-calc functions
1. [ ] Implement `clearForm()`
1. [ ] Rename Theory → Prophecy

### Short Term

1. [ ] Add S/D (Stop/Drive) toggle to Entry
1. [ ] Implement leg/stop navigation
1. [ ] Implement `setMode()` for ADD/EDIT
1. [ ] Connect Entry → Timeline display

### Medium Term

1. [ ] Biopsy feature (actual vs predicted comparison)
1. [ ] PWA manifest and service worker
1. [ ] Better mobile styling

-----

## 🔄 RECENT CHANGES

### 2026-01-14

- Documented missing Entry Tab functions
- Created AI reference files for GitHub
- Verified web_fetch works with raw GitHub URLs
- Identified “Theory” → “Prophecy” rename needed

### Previous Sessions

- Built complete Trip Generator (V5)
- Fixed ETA calculation bugs
- Added shipper/receiver date comparison
- Implemented FIFO recap visualization

-----

## 🐛 KNOWN BUGS

|Bug                          |Severity  |Notes                       |
|-----------------------------|----------|----------------------------|
|Entry buttons non-functional |🔴 Critical|Missing functions           |
|Time fields don’t auto-calc  |🔴 Critical|Missing functions           |
|Entered records don’t display|🟡 High    |`renderEntryTable()` missing|
|Timeline tab mostly empty    |🟡 High    |No events to display        |

-----

## 💡 IDEAS/BACKLOG

- [ ] Voice input for hands-free entry while driving
- [ ] Integration with actual ELD data
- [ ] Multi-run planning (chain loads)
- [ ] Fuel stop optimization
- [ ] Weather delay factors
- [ ] Traffic pattern learning

-----

## 📝 SESSION NOTES

**Emil’s Setup:**

- Owner-operator flatbed, 7 years
- Dog: Dakota (7 years old)
- Currently in Louisville, snowing
- Working from Hawk camper
- Truck getting new transmission

**Context:**

- Recovering from H3N2 (nasty, 3 relapses)
- Using voice interface extensively
- Building for trucker friends to test

-----

*Update this file at end of each session*