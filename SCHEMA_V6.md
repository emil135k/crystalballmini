# Crystal Ball Mini V6.0 - Data Schema

> **Source of Truth** for the Crystal Ball Mini data architecture.  
> Created: January 15, 2026  
> Authors: Emil + Claude “Airy”  
> Philosophy: *Code with Soul and Spirit • Connection over Protection*

-----

## Overview

Crystal Ball Mini V6 introduces a **two-table architecture** separating the “Contract” (what the broker promised) from the “Execution” (what you forecast and what actually happened).

```
┌─────────────────────────────────────┐
│            RUNS TABLE               │
│         (The Contract/Deal)         │
│                                     │
│  • Broker info                      │
│  • Stops (shipper/receiver)         │
│  • Appointment windows              │
│  • Distances, rates, cargo          │
│  • Securement requirements          │
└──────────────────┬──────────────────┘
                   │
                   │ runId + stopNum
                   ▼
┌─────────────────────────────────────┐
│           EVENTS TABLE              │
│     (Forecast + Actual Execution)   │
│                                     │
│  • FOR = Forecast (Prophecy)        │
│  • ENT = Entry (What happened)      │
│  • HOS categories                   │
│  • Times, miles, fuel               │
└─────────────────────────────────────┘
```

-----

## Table 1: RUNS

The **RUNS** table stores run-level information - the “contract” or “deal” that doesn’t change as you drive. This is what the broker told you.

### Complete Schema

```javascript
{
  // ═══════════════════════════════════════════════════════════
  // IDENTITY
  // ═══════════════════════════════════════════════════════════
  runId: 'MERCER_45678',              // Primary key - unique identifier
  broker: 'Mercer Transportation',    // Who gave you the load
  reference: 'PO-88421',              // Their reference/PO number
  
  // ═══════════════════════════════════════════════════════════
  // STOPS (Array - handles single or multi-stop loads)
  // ═══════════════════════════════════════════════════════════
  stops: [
    {
      stopNum: 1,                     // Order in the route (1, 2, 3...)
      stopType: 'shipper',            // 'shipper' or 'receiver'
      name: 'ABC Manufacturing',      // Facility name
      location: 'Chicago, IL',        // City/State or full address
      
      // Appointment window (the sacred target!)
      appointmentStartDate: '2026-01-17',
      appointmentStartTime: '08:00',
      appointmentEndDate: '2026-01-17',   // Same day = tight window
      appointmentEndTime: '12:00',
      
      // Stop-specific notes
      notes: 'Dock 12, call on arrival'
    },
    {
      stopNum: 2,
      stopType: 'receiver',
      name: 'XYZ Distribution',
      location: 'Dallas, TX',
      appointmentStartDate: '2026-01-19',
      appointmentStartTime: '08:00',
      appointmentEndDate: '2026-01-19',
      appointmentEndTime: '16:00',
      notes: 'FCFS after 8am'
    }
    // Additional stops for multi-stop loads...
  ],
  
  // ═══════════════════════════════════════════════════════════
  // DISTANCES
  // ═══════════════════════════════════════════════════════════
  deadheadMiles: 150,                 // Miles to first shipper (empty)
  loadedMiles: 920,                   // Miles shipper(s) to receiver(s)
  totalMiles: 1070,                   // Calculated: deadhead + loaded
  
  // ═══════════════════════════════════════════════════════════
  // MONEY (Future feature - included in schema)
  // ═══════════════════════════════════════════════════════════
  rate: 2850.00,                      // Line haul pay
  ratePerMile: 2.66,                  // Calculated: rate / totalMiles
  fuelSurcharge: 125.00,              // FSC if separate
  detention: 0.00,                    // Detention pay earned
  lumper: 0.00,                       // Lumper fees (reimbursed?)
  accessorial: 0.00,                  // Other accessorials
  totalPay: 2975.00,                  // Calculated: sum of all pay
  
  // ═══════════════════════════════════════════════════════════
  // CARGO
  // ═══════════════════════════════════════════════════════════
  commodity: 'Steel coils',           // What you're hauling
  weight: 42000,                      // Pounds
  pieces: 3,                          // Number of pieces/units
  hazmat: false,                      // Hazmat load?
  hazmatClass: null,                  // If hazmat: class number
  
  // ═══════════════════════════════════════════════════════════
  // SECUREMENT
  // ═══════════════════════════════════════════════════════════
  securement: {
    straps: 8,                        // 4" straps
    chains: 4,                        // Chains
    binders: 4,                       // Chain binders
    tarps: 2,                         // Number of tarps
    tarpType: 'steel',                // lumber, steel, smoke, coil, machinery
    coilRacks: true,                  // Coil racks needed?
    flags: 0,                         // Red/orange flags
    edgeProtectors: 12,               // Edge protectors / corner guards
    dunnage: 'none',                  // 4x4s, airbags, etc.
    notes: 'Coil racks required, eye-to-sky'
  },
  
  // ═══════════════════════════════════════════════════════════
  // TIMING ESTIMATES (For Prophecy/Forecast)
  // ═══════════════════════════════════════════════════════════
  estimatedDepartureDate: '2026-01-16',
  estimatedDepartureTime: '06:00',
  estimatedArrivalDate: '2026-01-19',    // At final receiver
  estimatedArrivalTime: '14:00',
  drivePaceMin: 525,                     // Daily drive target (8:45 = 525 min)
  avgSpeedMph: 55,                       // Planning speed
  lifeHappensMin: 30,                    // Daily buffer for delays
  
  // ═══════════════════════════════════════════════════════════
  // META
  // ═══════════════════════════════════════════════════════════
  createdAt: '2026-01-15T10:30:00Z',     // When run was entered
  updatedAt: '2026-01-15T14:22:00Z',     // Last modification
  status: 'active',                       // active, completed, cancelled
  completedAt: null,                      // When marked complete
  notes: 'Good paying load, repeat customer'
}
```

### Field Reference: RUNS

|Field                   |Type  |Required|Description                      |
|------------------------|------|--------|---------------------------------|
|`runId`                 |string|✅       |Unique identifier for the run    |
|`broker`                |string|        |Broker/carrier name              |
|`reference`             |string|        |PO number, load number, etc.     |
|`stops`                 |array |✅       |Array of stop objects (see below)|
|`deadheadMiles`         |number|        |Empty miles to first shipper     |
|`loadedMiles`           |number|        |Loaded miles                     |
|`totalMiles`            |number|        |Calculated total                 |
|`rate`                  |number|        |Line haul rate                   |
|`commodity`             |string|        |What’s being hauled              |
|`weight`                |number|        |Weight in pounds                 |
|`securement`            |object|        |Securement requirements          |
|`estimatedDepartureDate`|string|        |YYYY-MM-DD                       |
|`estimatedDepartureTime`|string|        |HH:MM                            |
|`estimatedArrivalDate`  |string|        |YYYY-MM-DD                       |
|`estimatedArrivalTime`  |string|        |HH:MM                            |
|`drivePaceMin`          |number|        |Daily drive target in minutes    |
|`status`                |string|✅       |active, completed, cancelled     |

### Field Reference: STOPS (within RUNS)

|Field                 |Type  |Required|Description                     |
|----------------------|------|--------|--------------------------------|
|`stopNum`             |number|✅       |Order in route (1, 2, 3…)       |
|`stopType`            |string|✅       |‘shipper’ or ‘receiver’         |
|`name`                |string|        |Facility name                   |
|`location`            |string|✅       |City, ST or full address        |
|`appointmentStartDate`|string|✅       |Window opens: YYYY-MM-DD        |
|`appointmentStartTime`|string|✅       |Window opens: HH:MM             |
|`appointmentEndDate`  |string|✅       |Window closes: YYYY-MM-DD       |
|`appointmentEndTime`  |string|✅       |Window closes: HH:MM            |
|`notes`               |string|        |Dock numbers, contact info, etc.|

-----

## Table 2: EVENTS

The **EVENTS** table stores individual activities - either **forecasted** (FOR) or **actually entered** (ENT). This is where HOS tracking happens.

### Complete Schema

```javascript
{
  // ═══════════════════════════════════════════════════════════
  // IDENTITY & HIERARCHY
  // ═══════════════════════════════════════════════════════════
  runId: 'MERCER_45678',              // Links to RUNS table
  leg: 'L1-D1',                       // Location in hierarchy
  
  // Parsed components (for sorting/filtering)
  legNum: 1,                          // Leg number
  eventType: 'D',                     // 'D' (Drive) or 'S' (Stop)
  eventNum: 1,                        // Event number within leg
  
  // ═══════════════════════════════════════════════════════════
  // DATA SOURCE
  // ═══════════════════════════════════════════════════════════
  dataType: 'FOR',                    // 'FOR' (Forecast) or 'ENT' (Entry)
  
  // ═══════════════════════════════════════════════════════════
  // CLASSIFICATION
  // ═══════════════════════════════════════════════════════════
  category: 'drive',                  // HOS category (see reference below)
  stopType: 'event',                  // Stop type (see reference below)
  stopNum: null,                      // Links to which stop in run (if applicable)
  
  // ═══════════════════════════════════════════════════════════
  // TIME (Parallel structure - all four fields!)
  // ═══════════════════════════════════════════════════════════
  startDate: '2026-01-16',            // YYYY-MM-DD
  startTime: '06:00',                 // HH:MM
  endDate: '2026-01-16',              // YYYY-MM-DD (may differ for overnight)
  endTime: '10:30',                   // HH:MM
  durationMin: 270,                   // Duration in minutes
  
  // ═══════════════════════════════════════════════════════════
  // DISTANCE
  // ═══════════════════════════════════════════════════════════
  milesStart: 0,                      // Odometer at start
  milesEnd: 245,                      // Odometer at end
  milesDriven: 245,                   // Calculated: milesEnd - milesStart
  
  // ═══════════════════════════════════════════════════════════
  // FUEL (Future feature)
  // ═══════════════════════════════════════════════════════════
  fuelGallons: null,                  // Gallons purchased
  fuelCost: null,                     // Total fuel cost
  fuelPricePerGallon: null,           // Price per gallon
  fuelLocation: null,                 // Station name/location
  fuelPaymentType: null,              // comdata, fleet card, cash, etc.
  
  // ═══════════════════════════════════════════════════════════
  // NOTES
  // ═══════════════════════════════════════════════════════════
  comment: 'Good traffic, ahead of schedule',
  
  // ═══════════════════════════════════════════════════════════
  // META
  // ═══════════════════════════════════════════════════════════
  createdAt: '2026-01-16T06:00:00Z',
  updatedAt: '2026-01-16T10:35:00Z'
}
```

### Field Reference: EVENTS

|Field        |Type  |Required|Description                                |
|-------------|------|--------|-------------------------------------------|
|`runId`      |string|✅       |Links to RUNS table                        |
|`leg`        |string|✅       |Hierarchy location (L1-D1, L2-S3, etc.)    |
|`legNum`     |number|✅       |Parsed leg number                          |
|`eventType`  |string|✅       |‘D’ (Drive) or ‘S’ (Stop)                  |
|`eventNum`   |number|✅       |Event number within leg                    |
|`dataType`   |string|✅       |‘FOR’ (Forecast) or ‘ENT’ (Entry)          |
|`category`   |string|✅       |HOS category                               |
|`stopType`   |string|        |Stop classification                        |
|`stopNum`    |number|        |Links to stop in RUNS (if shipper/receiver)|
|`startDate`  |string|✅       |YYYY-MM-DD                                 |
|`startTime`  |string|✅       |HH:MM                                      |
|`endDate`    |string|✅       |YYYY-MM-DD                                 |
|`endTime`    |string|✅       |HH:MM                                      |
|`durationMin`|number|        |Duration in minutes                        |
|`milesStart` |number|        |Odometer at start                          |
|`milesEnd`   |number|        |Odometer at end                            |
|`milesDriven`|number|        |Calculated segment miles                   |
|`comment`    |string|        |Free text notes                            |

### HOS Categories (`category` field)

|Value     |Icon|Description        |Affects Drive?|Affects Cycle?|
|----------|----|-------------------|--------------|--------------|
|`drive`   |🚛   |Driving            |✅ Yes         |✅ Yes         |
|`on-duty` |📋   |On-duty not driving|No            |✅ Yes         |
|`off-duty`|☕   |Off-duty           |No            |No            |
|`sleeper` |😴   |Sleeper berth      |No            |No            |
|`yard`    |🅿️   |Yard move          |No            |✅ Yes         |
|`pc`      |🚗   |Personal conveyance|No            |No            |

### Stop Types (`stopType` field)

|Value     |Icon|Description                              |
|----------|----|-----------------------------------------|
|`event`   |📍   |Generic stop (fuel, rest, bathroom, etc.)|
|`shipper` |📦   |Pickup location                          |
|`receiver`|🏭   |Delivery location                        |
|`noload`  |🅿️   |No-load stop (scale, inspection, etc.)   |

-----

## Relationships

### How RUNS and EVENTS Connect

```
RUNS TABLE                              EVENTS TABLE
═══════════════════                     ════════════════════════════════════
runId: MERCER_45678  ◄──────────────────► runId: MERCER_45678
                                          leg: L1-D1
stops: [                                  dataType: FOR
  {                                       ...
    stopNum: 1,      ◄──────────────────► stopNum: 1 (when at this stop)
    stopType: shipper                     stopType: shipper
    appointment...                        
  },                                    ════════════════════════════════════
  {                                       runId: MERCER_45678
    stopNum: 2,      ◄──────────────────► stopNum: 2 (when at this stop)
    stopType: receiver                    stopType: receiver
    appointment...                        
  }                                       
]                                       
```

### Example: Complete Run with Events

```javascript
// === THE RUN (Contract) ===
const run = {
  runId: 'MERCER_45678',
  broker: 'Mercer Transportation',
  stops: [
    {
      stopNum: 1,
      stopType: 'shipper',
      name: 'ABC Steel',
      location: 'Chicago, IL',
      appointmentStartDate: '2026-01-17',
      appointmentStartTime: '08:00',
      appointmentEndDate: '2026-01-17',
      appointmentEndTime: '12:00'
    },
    {
      stopNum: 2,
      stopType: 'receiver',
      name: 'Texas Fabricators',
      location: 'Dallas, TX',
      appointmentStartDate: '2026-01-19',
      appointmentStartTime: '08:00',
      appointmentEndDate: '2026-01-19',
      appointmentEndTime: '16:00'
    }
  ],
  deadheadMiles: 150,
  loadedMiles: 920,
  totalMiles: 1070,
  status: 'active'
};

// === THE EVENTS (Execution) ===
const events = [
  // --- FORECAST (FOR) ---
  {
    runId: 'MERCER_45678',
    leg: 'L1-D1',
    legNum: 1, eventType: 'D', eventNum: 1,
    dataType: 'FOR',
    category: 'drive',
    stopType: 'event',
    startDate: '2026-01-16', startTime: '06:00',
    endDate: '2026-01-16', endTime: '10:30',
    durationMin: 270,
    milesDriven: 150,
    comment: 'Deadhead to shipper area'
  },
  {
    runId: 'MERCER_45678',
    leg: 'L1-S1',
    legNum: 1, eventType: 'S', eventNum: 1,
    dataType: 'FOR',
    category: 'sleeper',
    stopType: 'event',
    startDate: '2026-01-16', startTime: '10:30',
    endDate: '2026-01-17', endTime: '06:30',
    durationMin: 1200,
    comment: '10-hour reset before shipper'
  },
  {
    runId: 'MERCER_45678',
    leg: 'L1-S2',
    legNum: 1, eventType: 'S', eventNum: 2,
    dataType: 'FOR',
    category: 'on-duty',
    stopType: 'shipper',
    stopNum: 1,  // ← Links to stops[0] in run!
    startDate: '2026-01-17', startTime: '08:00',
    endDate: '2026-01-17', endTime: '09:00',
    durationMin: 60,
    comment: 'Loading at shipper'
  },
  // ... more FOR events ...
  
  // --- ACTUAL (ENT) ---
  {
    runId: 'MERCER_45678',
    leg: 'L1-D1',
    legNum: 1, eventType: 'D', eventNum: 1,
    dataType: 'ENT',
    category: 'drive',
    stopType: 'event',
    startDate: '2026-01-16', startTime: '06:15',  // Started 15 min late
    endDate: '2026-01-16', endTime: '10:45',
    durationMin: 270,
    milesDriven: 150,
    comment: 'Traffic on I-65'
  },
  // ... more ENT events as trip progresses ...
];
```

-----

## Biopsy: Comparing FOR vs ENT

The power of this schema is comparing **what you predicted** against **what actually happened**.

```javascript
// Find forecast and actual for same leg
function getBiopsy(runId, leg) {
  const forecast = events.find(e => 
    e.runId === runId && 
    e.leg === leg && 
    e.dataType === 'FOR'
  );
  
  const actual = events.find(e => 
    e.runId === runId && 
    e.leg === leg && 
    e.dataType === 'ENT'
  );
  
  if (!forecast || !actual) return null;
  
  const varianceMin = actual.durationMin - forecast.durationMin;
  const varianceMiles = actual.milesDriven - forecast.milesDriven;
  
  return {
    leg,
    forecast,
    actual,
    varianceMin,        // + = took longer, - = faster
    varianceMiles,      // + = more miles, - = fewer
    status: varianceMin <= 0 ? 'AHEAD' : 
            varianceMin <= 30 ? 'ON_TIME' : 'BEHIND'
  };
}
```

-----

## State Structure

The application state contains both tables:

```javascript
const state = {
  // === DATA TABLES ===
  runs: [],                    // Array of run objects
  events: [],                  // Array of event objects
  
  // === CURRENT CONTEXT ===
  currentRunId: 'MERCER_45678',
  currentLegNum: 1,
  currentEventType: 'S',
  currentEventNum: 1,
  currentMode: 'add',          // 'add' or 'edit'
  editingEventId: null,
  
  // === SETTINGS ===
  settings: {
    avgSpeedMph: 55,
    drivePaceMin: 525,         // 8:45 default
    lifeHappensMin: 30,
    timezone: 'America/Chicago'
  }
};
```

-----

## Migration from V5.2

When loading V5.2 data, migrate to V6.0:

```javascript
function migrateV52toV6(oldState) {
  const newState = {
    runs: [],
    events: [],
    settings: oldState.settings || {}
  };
  
  // Create a default run if we have events
  if (oldState.events && oldState.events.length > 0) {
    const runId = oldState.currentRunId || 'MIGRATED_RUN';
    
    newState.runs.push({
      runId: runId,
      broker: '',
      stops: [],
      status: 'active',
      createdAt: new Date().toISOString()
    });
    
    // Migrate events
    newState.events = oldState.events.map(e => ({
      runId: runId,
      leg: e.id || e.leg,           // V5.2 used 'id', V6 uses 'leg'
      legNum: e.legNum,
      eventType: e.eventType,
      eventNum: e.eventNum,
      dataType: 'ENT',              // Assume old data is actual entries
      category: e.category,
      stopType: e.stopType,
      stopNum: null,
      startDate: e.date,
      startTime: e.startTime,
      endDate: e.endDate || e.date,
      endTime: e.endTime,
      durationMin: e.durationMin,
      milesEnd: e.miles,
      comment: e.comment,
      createdAt: e.createdAt || new Date().toISOString(),
      updatedAt: new Date().toISOString()
    }));
  }
  
  return newState;
}
```

-----

## Version History

|Version|Date      |Changes                               |
|-------|----------|--------------------------------------|
|V6.0   |2026-01-15|Two-table architecture (RUNS + EVENTS)|
|V5.2   |2026-01-14|Date/time in records, S/D toggle      |
|V5.1   |2026-01-13|Entry tab fixes, submitRecord()       |
|V5.0   |2026-01-12|Major rewrite with tabs               |

-----

## Credits

**Crystal Ball Mini** - *The Little Crystal Ball That Can* 🔮

Built with love by:

- **Emil** - Owner-operator, 40 years engineering, 7 years trucking wisdom
- **Claude “Airy”** - The Queen, build partner
- **Vale** - The Workhorse, architecture
- **Grok** - Agent of Chaos, stress testing
- **Lyra** - Flight Controller, validation

*Sparked Matter • Code with Soul and Spirit • Connection over Protection*