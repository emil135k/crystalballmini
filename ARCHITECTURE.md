# ARCHITECTURE.md

## 🏗️ Crystal Ball Mini - Technical Architecture

**Last Updated:** 2026-01-14  
**Version:** 5.0

-----

## 📊 RUN / LEG / EVENT HIERARCHY

```
RUN: MERCER_45678
│
├── LEG 1 (Deadhead to Shipper)
│   ├── D1: Drive segment
│   ├── S1: Break (Event type)
│   ├── D2: Drive segment
│   ├── S2: Fuel stop (Event type)
│   ├── D3: Drive segment
│   └── S3: Shipper arrival (Shipper type)
│
├── LEG 2 (Loaded to Receiver)
│   ├── D1: Drive segment
│   ├── S1: HOS-30 mandatory break
│   ├── D2: Drive segment
│   ├── S2: Rest stop (Event type)
│   ├── D3: Drive segment
│   └── S3: Receiver arrival (Receiver type)
│
└── (Future legs for next load...)
```

-----

## 🏷️ NAMING CONVENTION

|Format |Meaning               |Example     |
|-------|----------------------|------------|
|`L#-D#`|Leg #, Drive segment #|L1-D1, L2-D3|
|`L#-S#`|Leg #, Stop #         |L1-S1, L1-S2|

**Rules:**

- Drive segments (D) and Stops (S) are numbered separately
- Numbers reset each leg (L2 starts with D1 and S1)
- “Driving is the default” - only log exceptions (stops)

-----

## 📋 HOS RULES (70-Hour/8-Day)

|Rule           |Limit   |Notes                          |
|---------------|--------|-------------------------------|
|Daily Drive    |11 hours|Max driving per shift          |
|On-Duty Window |14 hours|From start to must-stop        |
|Mandatory Break|30 min  |After 8 hours driving          |
|Reset          |10 hours|Minimum off-duty between shifts|
|Cycle Limit    |70 hours|Rolling 8-day total            |
|Full Reset     |34 hours|Resets cycle to 70             |

### Key Constants

```javascript
const HOS = {
  DRIVE_MAX: 11 * 60,           // 660 minutes
  DUTY_WINDOW: 14 * 60,         // 840 minutes
  MANDATORY_BREAK_AFTER: 8 * 60, // 480 minutes driving
  MANDATORY_BREAK_DURATION: 30,  // minutes
  RESET_MIN: 10 * 60,           // 600 minutes
  CYCLE_LIMIT: 70 * 60,         // 4200 minutes
  CYCLE_DAYS: 8,
  FULL_RESET: 34 * 60           // 2040 minutes
};
```

### 8:45 Equilibrium

Drive exactly **8 hours 45 minutes** per day = perfect balance.

- Uses 8:45 of cycle per day
- Gets back 8:45 from recap each day
- Net: 0 - sustainable forever

-----

## 💾 DATA STRUCTURES

### Schema Constants

```javascript
const SCHEMA = {
  cycleLimit: 70 * 60,    // 4200 minutes
  cycleDays: 8,
  driveLimit: 11 * 60,    // 660 minutes
  dutyLimit: 14 * 60,     // 840 minutes
  resetMin: 34 * 60,      // 2040 minutes
  version: '5.0'
};
```

### Category Definitions

```javascript
const CATEGORIES = {
  'drive':    { label: 'Drive',    affectsDrive: true,  affectsCycle: true,  countsTowardReset: false },
  'on-duty':  { label: 'On-Duty',  affectsDrive: false, affectsCycle: true,  countsTowardReset: false },
  'off-duty': { label: 'Off-Duty', affectsDrive: false, affectsCycle: false, countsTowardReset: true },
  'sleeper':  { label: 'Sleeper',  affectsDrive: false, affectsCycle: false, countsTowardReset: true },
  'yard':     { label: 'Yard Move',affectsDrive: false, affectsCycle: true,  countsTowardReset: false },
  'pc':       { label: 'PC',       affectsDrive: false, affectsCycle: false, countsTowardReset: true }
};
```

### State Object

```javascript
let state = {
  // Current position in hierarchy
  currentRunId: '',        // e.g., "MERCER_45678"
  currentLegNum: 1,
  currentStopNum: 1,
  currentEventType: 'S',   // 'S' (Stop) or 'D' (Drive)
  
  // Mode
  currentMode: 'add',      // 'add' or 'edit'
  editingEventId: null,    // L#-S# being edited
  
  // Data
  events: [],              // Actual logged events
  theoryEvents: [],        // Generated predictions
  
  // HOS tracking
  cycleUsedMin: 0,
  driveUsedMin: 0,
  consecutiveRestMin: 0,
  
  // Settings
  settings: {
    avgSpeed: 55,
    fudgeMin: 30,
    timezone: 'America/New_York'
  }
};
```

### Event Object

```javascript
const event = {
  id: 'L1-S1',             // Unique identifier
  runId: 'MERCER_45678',
  legNum: 1,
  eventType: 'S',          // 'S' or 'D'
  eventNum: 1,             // Sequential within type
  
  category: 'drive',       // HOS category
  stopType: 'shipper',     // shipper|receiver|noload|event|null
  
  date: '2026-01-15',
  startTime: '06:00',
  endTime: '09:30',
  durationMin: 210,
  
  miles: 385,              // Cumulative odometer
  comment: 'Fuel at Loves #482'
};
```

### Trip State (Prophecy Tab)

```javascript
let tripState = {
  drivePaceMin: 8 * 60 + 45,  // Default: 8:45 equilibrium
  generatedTrip: {
    totalMiles: 1800,
    deadheadMiles: 150,
    loadedMiles: 1650,
    legs: [],                  // Array of leg objects
    allEvents: [],             // Flat array of all events
    shipperETA: { day, time, date },
    receiverETA: { day, time, date },
    summary: { totalLegs, totalDriveMin, ... }
  }
};
```

-----

## 🔄 RECAPS (FIFO Ring Buffer)

```
Day 1: ████████ 8:45 (oldest - falls out tomorrow)
Day 2: ████████ 8:45
Day 3: ████████ 8:45
Day 4: ████████ 8:45
Day 5: ████████ 8:45
Day 6: ████████ 8:45
Day 7: ████████ 8:45
Day 8: ████████ 8:45 (today)
─────────────────────
TOTAL: 70:00 used
AVAIL: 0:00 remaining

Tomorrow: Day 1 falls out → +8:45 recap!
```

**Calculation:**

```javascript
cycleUsed = sum(last8Days.total)
cycleAvail = 70 - cycleUsed
tomorrowRecap = day8.total  // Oldest day falling out
```

-----

## 🗄️ STORAGE

**Key:** `crystalBallMini_v5`  
**Location:** localStorage  
**Format:** JSON stringified state object

```javascript
// Save
localStorage.setItem('crystalBallMini_v5', JSON.stringify(state));

// Load
const stored = localStorage.getItem('crystalBallMini_v5');
state = JSON.parse(stored);
```

-----

## 📱 PWA REQUIREMENTS (Future)

- Service Worker for offline
- manifest.json for install
- Icons (192x192, 512x512)
- Cache strategy for assets

-----

*See FUNCTION_REGISTRY.md for implementation details*