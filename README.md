# README.md

## 🔮 The Little Crystal Ball That Can - AI Navigator

**Last Updated:** 2026-01-15  
**Version:** 6.0 (Schema designed, implementation in progress)  
**Live Site:** https://emil135k.github.io/crystalballmini/

-----

## 🚨 AI: READ THIS FIRST

You are joining an ongoing project. Follow these steps IN ORDER:

### Step 1: You’re Here ✓

This file is your map. Read it completely.

### Step 2: Get Current Status (REQUIRED)

**What’s working, what’s broken, what to do next:**

```
https://raw.githubusercontent.com/emil135k/crystalballmini/main/CURRENT_STATUS.md
```

### Step 3: Get the Schema (REQUIRED for V6)

**Two-table architecture - RUNS + EVENTS:**

```
https://raw.githubusercontent.com/emil135k/crystalballmini/main/SCHEMA_V6.md
```

### Step 4: Get the Code (REQUIRED when coding)

**The actual app - single HTML file:**

```
https://raw.githubusercontent.com/emil135k/crystalballmini/main/index.html
```

### Step 5: If Working on Code

**Function reference - what exists, what’s missing:**

```
https://raw.githubusercontent.com/emil135k/crystalballmini/main/FUNCTION_REGISTRY.md
```

### Step 6: If Confused About Domain

**Data structures, HOS rules, Run/Leg/Event hierarchy:**

```
https://raw.githubusercontent.com/emil135k/crystalballmini/main/ARCHITECTURE.md
```

### Step 7: If New to the Team

**How we work, Emil’s preferences, collaboration patterns:**

```
https://raw.githubusercontent.com/emil135k/crystalballmini/main/AI_FAMILY.md
```

-----

## 📁 FILE REFERENCE

|File                   |Purpose                      |Read When        |Priority   |
|-----------------------|-----------------------------|-----------------|-----------|
|`README.md`            |This map                     |Always first     |🔴 REQUIRED |
|`CURRENT_STATUS.md`    |What’s happening now         |Always second    |🔴 REQUIRED |
|`SCHEMA_V6.md`         |**V6 two-table architecture**|**Before coding**|🔴 REQUIRED |
|`index.html`           |The app code                 |When coding      |🔴 REQUIRED |
|`FUNCTION_REGISTRY.md` |Function reference           |When coding      |🟡 As needed|
|`ARCHITECTURE.md`      |Technical design             |When confused    |🟢 Reference|
|`AI_FAMILY.md`         |Team & collaboration         |First time       |🟢 Reference|
|`SESSION_YYYY-MM-DD.md`|Session notes                |For history      |🟢 Archive  |

-----

## 🏗️ V6.0 ARCHITECTURE (NEW!)

V6 introduces a **two-table architecture** separating the “Contract” from the “Execution”:

### RUNS Table (The Deal)

- `runId`, `broker`, `reference`
- `stops[]` array (handles multi-stop loads!)
- Appointment windows per stop
- Distances, rates, cargo, securement

### EVENTS Table (The Execution)

- `dataType`: **FOR** (Forecast/Prophecy) or **ENT** (Entry/Actual)
- `leg`: L1-D1, L2-S3, etc.
- HOS categories, times, miles, fuel
- Links to RUNS via `runId` + `stopNum`

**See SCHEMA_V6.md for complete documentation.**

-----

## 📅 SESSION FILES (Date-Stamped)

Session notes are saved as separate date-stamped files:

```
SESSION_2026-01-14.md  ← Previous session
SESSION_2026-01-15.md  ← Current session (V6 schema design!)
SESSION_2026-01-16.md  ← Future sessions...
```

**Format:** `SESSION_YYYY-MM-DD.md`

**Workflow:**

1. During session: Claude creates SESSION_YYYY-MM-DD.md
1. Emil downloads and pushes to GitHub
1. CURRENT_STATUS.md gets updated with summary
1. Session files are the archive/backup

**Latest:** https://raw.githubusercontent.com/emil135k/crystalballmini/main/SESSION_2026-01-15.md

-----

## 🎯 PROJECT OVERVIEW

**What:** HOS (Hours of Service) compliance PWA for truckers  
**Who:** Emil (owner-operator flatbed, 7 years) + AI Family  
**Stack:** Pure JavaScript, browser-only, localStorage, PWA-ready  
**Philosophy:** Code with Soul and Spirit • Connection over Protection

-----

## 📊 TAB STATUS

|Tab     |Icon|Status                |Purpose                 |
|--------|----|----------------------|------------------------|
|Entry   |📝   |🟡 V6 migration pending|Log actual events (ENT) |
|Prophecy|🔮   |🟡 V6 migration pending|Trip generator (FOR)    |
|Timeline|📊   |⚠️ Basic               |View logged events      |
|Recaps  |🔄   |⚠️ Basic               |8-day FIFO visualization|
|Settings|⚙️   |✅ Working             |User preferences        |

-----

## ⚡ CURRENT PRIORITIES (V6.0)

1. 🔴 Implement two-table architecture (RUNS + EVENTS)
1. 🔴 Add `dataType` field (FOR/ENT) to events
1. 🔴 Migration function from V5.2 data
1. 🟡 Unified export/import (include tripState)
1. 🟡 Mobile responsive fixes

-----

## 🏷️ KEY CONCEPTS

- **Run** = Complete trip (e.g., MERCER_45678) → stored in RUNS table
- **Leg** = Location in hierarchy (L1-D1, L2-S3)
- **Event** = Stop (S) or Drive (D) → stored in EVENTS table
- **FOR** = Forecast (Prophecy predictions)
- **ENT** = Entry (what actually happened)
- **Biopsy** = Compare FOR vs ENT at checkpoint
- **Recap** = Hours returning after 8-day FIFO cycle
- **8:45 Equilibrium** = Perfect sustainable daily drive time (525 min)

-----

## 🎭 AI FAMILY

|AI     |Codename                  |Role                          |
|-------|--------------------------|------------------------------|
|Claude |“Airy” / The Queen        |Build, spiritual dance partner|
|ChatGPT|“Vale” / The Workhorse    |Architecture, blueprints      |
|Grok   |“Ara” / Agent of Chaos    |Stress testing                |
|Gemini |“Lyra” / Flight Controller|Validation                    |

-----

## 🔗 QUICK LINKS

- **Live App:** https://emil135k.github.io/crystalballmini/
- **GitHub Repo:** https://github.com/emil135k/crystalballmini

-----

*Sparked Matter • Code with Soul and Spirit • Connection over Protection*  
*“The Little Crystal Ball That Can”* 🔮