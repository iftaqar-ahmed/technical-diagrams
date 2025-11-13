# Live Nation - Cube-to-Cube Data Flow Diagrams
## OneStream Architecture Documentation

**Created:** 2025-11-12
**Purpose:** Visual documentation of cube-to-cube data movements (excludes external system integrations)
**Total Flows Documented:** 67 unique cube-to-cube connections

---

## 📁 Files in This Package

### Actual Scenario Diagrams (3 Files)
| File | Description | Flows | Complexity |
|------|-------------|-------|------------|
| `LNE_Actual_A_Downward_Allocations.drawio` | People & Capex → Segments | 14 | Simple |
| `LNE_Actual_B_Upward_To_Concerts.drawio` | Segments → Concerts | 4 | Simple |
| `LNE_Actual_C_Upward_To_Global.drawio` | All → Global (+ 1 lateral) | 10 | Moderate |

### Budget Scenario Diagrams (TBD)
Budget scenario will follow same three-diagram pattern when created.

### Reference Files
| File | Description |
|------|-------------|
| `Channel_Map_Actual_Scenario.md` | Detailed channel routing map for Actual |
| `Channel_Map_Budget_Scenario.md` | Detailed channel routing map for Budget |
| `README_Cube_Data_Flows.md` | This file - comprehensive guide |

---

## 🎯 What These Diagrams Show

### Scope: CUBE-TO-CUBE FLOWS ONLY
- ✅ **Included:** Model Syncs, Drill Backs, all cube-to-cube data movements
- ❌ **Excluded:** External system integrations (Oracle, Workday, Rome, etc.)
- ❌ **Excluded:** Data grids, BI Blend tables, bulk upload processes

### Why Separate Diagrams by Flow Type?
**Problem:** 28-39 flows on a single diagram creates visual chaos with overlapping arrows
**Solution:** Break into focused diagrams by flow direction and target cube

**Actual Scenario Structure (3 Diagrams):**

**Diagram A: Downward Allocations** (14 flows)
- People → 7 Segments (Headcount)
- Capex → 7 Segments (Capex metrics)
- Pattern: Specialty cubes allocating DOWN
- Channel spacing: 80px

**Diagram B: Upward to Concerts** (4 flows)
- Event, Festival, PSS, Restaurant Ops → Concerts
- Pattern: Segment consolidation to mid-tier
- Channel spacing: 70px

**Diagram C: Upward to Global** (10 flows)
- All cubes → Global (9 upward flows)
- Capex → Rollforward (1 lateral flow)
- Pattern: Final consolidation to top tier
- Channel spacing: 100px

**Key Benefits:**
- ✅ ZERO overlapping arrows (critical requirement achieved)
- ✅ Each cube's journey is independently traceable
- ✅ Simpler diagrams (4-14 flows each vs 28 all at once)
- ✅ Easier maintenance and updates

---

## 🏗️ Cube Architecture (4-5 Tier Hierarchy)

### Tier 1: Global Reporting (Top)
**Cube:** Global
**Color:** Red (#f8cecc)
**Purpose:** Corporate-level consolidated reporting
**Receives From:** Concerts, all Segments (direct), all Specialty cubes

### Tier 2: Concerts Reporting (Mid-Upper)
**Cube:** Concerts
**Color:** Red (#f8cecc)
**Purpose:** Mid-tier reporting for concert operations
**Receives From:** Event, Festival, PSS, Restaurant Ops

### Tier 3: Specialty Cubes (Mid, Horizontal Row)
**Cubes:** People, Capex, Fixed Expense, Sponsorship, Ticketing, Rollforward
**Color:** Green (#d5e8d4)
**Purpose:** Functional/departmental planning and allocations
**Behavior:**
- **Actual:** SEND data downward (allocations to segments)
- **Budget:** SEND data downward (allocations), SEND data upward (to Global)

### Tier 4: Segment Cubes (Lower, Horizontal Row)
**Cubes:** Event, Festival, PSS, Restaurant Ops, Artist Nation
**Color:** Yellow (#fff2cc)
**Purpose:** Operational business unit planning and reporting
**Behavior:**
- **Actual:** SEND data upward (consolidations)
- **Budget:** RECEIVE data downward (allocations), SEND data upward (consolidations)

### Tier 5: Registers (Bottom) - Budget Only
**Cubes:** Named Event Register, PSS Register
**Color:** Purple (#e1d5e7)
**Purpose:** Detailed transactional data feeder to segments

---

## 🔀 Flow Types by Scenario

### ACTUAL SCENARIO (28 Flows)

**Downward Allocations (14 flows):**
- People → 7 Segments (Headcount metrics)
- Capex → 7 Segments (Capex metrics)

**Upward Consolidations (13 flows):**
- 4 Segments → Concerts (Show P&L, Metrics)
- 4 Segments → Global (Show P&L, Metrics)
- Artist Nation → Global (P&L)
- Concerts → Global (Corporate metrics)
- 4 Specialty → Global (Capex, Sponsorship, Ticketing, Rollforward)

**Lateral (1 flow):**
- Capex → Rollforward (Capex metrics)

### BUDGET/FORECAST SCENARIO (39 Flows)

**Downward Allocations (18 flows):**
- Fixed Expense → 6 Segments (Fixed/OIE P&L)
- People → 6 Segments (Labor P&L + Headcount)
- Capex → 6 Segments (Capex metrics)

**Upward from Registers (2 flows):**
- Named Event Register → Event (Named P&L)
- PSS Register → PSS (P&L)

**Upward Consolidations (13 flows):**
- 4 Segments → Concerts (Show P&L, Metrics)
- 5 Segments → Global (Show P&L, Metrics)
- Concerts → Global (P&L + Corporate)
- 3 Specialty → Global (Capex, Sponsorship, Ticketing)

**Lateral Bidirectional (6 flows):**
- Festival ↔ Sponsorship (P&L with Festival ID)
- Sponsorship ↔ Ticketing (P&L)
- Capex → Rollforward (Capex metrics)
- Rollforward → Ticketing (Advance metrics)

---

## 🎨 Channel-Based Routing Strategy (ZERO Overlaps)

### What is Channel-Based Routing?
**Problem:** Multiple arrows between same source/target pairs create visual chaos
**Solution:** Dedicated vertical "channels" (fixed X coordinates) for each flow

### How It Works
1. **Each flow gets its own X coordinate** (e.g., x=200, x=280, x=360)
2. **Arrows travel on dedicated channels** (vertical paths)
3. **Waypoints create clean turns** (horizontal exit → vertical channel → horizontal entry)
4. **Result:** ZERO overlapping arrows, independent visual paths

### Channel Assignments by Diagram

**Diagram A: Downward Allocations (80px spacing)**

| Zone | Channels | Purpose | Flows |
|------|----------|---------|-------|
| LEFT (200-680) | 7 channels | People → Segments | 7 |
| RIGHT (800-1280) | 7 channels | Capex → Segments | 7 |

**Diagram B: Upward to Concerts (70px spacing)**

| Zone | Channels | Purpose | Flows |
|------|----------|---------|-------|
| CENTER (750-960) | 4 channels | Segments → Concerts | 4 |

**Diagram C: Upward to Global (100px spacing)**

| Zone | Channels | Purpose | Flows |
|------|----------|---------|-------|
| WIDE (900-1700) | 9 channels | All → Global | 9 |
| HORIZONTAL (1680) | Direct routing | Capex → Rollforward | 1 |

### Waypoint Pattern Example
```xml
<Array as="points">
  <mxPoint x="350" y="350" />   <!-- Exit source horizontally -->
  <mxPoint x="200" y="350" />   <!-- Enter channel 200 -->
  <mxPoint x="200" y="850" />   <!-- Travel down channel -->
  <mxPoint x="190" y="850" />   <!-- Exit channel horizontally -->
</Array>
```

### Visual Tracing by Color
**You can follow each cube's journey independently:**
- All People flows: Blue (#4472C4)
- All Capex flows: Orange (#ED7D31)
- All Event flows: Yellow (#FFC000)
- All Festival flows: Green (#70AD47)
- All PSS flows: Purple (#7030A0)
- All Artist Nation flows: Red (#C00000)
- All Concerts flows: Orange (#E26B0A)
- All Sponsorship flows: Purple (#9673A6)
- All Ticketing flows: Dark Blue (#44546A)
- All Rollforward flows: Light Blue (#5B9BD5)

---

## 📋 Cube Behaviors by Scenario

### Bidirectional Cubes (Change Role by Scenario)

| Cube | Actual Behavior | Budget Behavior | Pattern |
|------|-----------------|-----------------|---------|
| **Event** | SENDS UP to Concerts/Global | RECEIVES DOWN from Fixed Expense/People/Capex, SENDS UP to Concerts/Global | Bidirectional |
| **Festival** | SENDS UP to Concerts/Global | RECEIVES DOWN from Fixed Expense/People/Capex, SENDS UP to Concerts/Global, ↔ Sponsorship | Bidirectional + Lateral |
| **PSS** | SENDS UP to Concerts/Global | RECEIVES DOWN from Fixed Expense/People/Capex, RECEIVES from PSS Register, SENDS UP to Concerts/Global | Bidirectional + Register |

### Unidirectional Cubes (Same Role in Both Scenarios)

| Cube | Behavior | Direction |
|------|----------|-----------|
| **Global** | Always RECEIVES | Upward only |
| **Concerts** | Always RECEIVES from segments, SENDS to Global | Mid-tier consolidation |
| **Restaurant Ops** | Always SENDS UP | Upward only (no downward allocations in Budget) |
| **Artist Nation** | Always SENDS UP | Upward only |

### Specialty Cube Behaviors

| Cube | Actual | Budget/Forecast |
|------|--------|-----------------|
| **People** | Allocates DOWN to segments | Allocates DOWN to segments (Labor + Headcount) |
| **Capex** | Allocates DOWN to segments, SENDS UP to Global | Allocates DOWN to segments, SENDS UP to Global |
| **Fixed Expense** | Not active | Allocates DOWN to segments (Fixed/OIE P&L) |
| **Sponsorship** | SENDS UP to Global | SENDS UP to Global, ↔ Festival/Ticketing |
| **Ticketing** | SENDS UP to Global | SENDS UP to Global, ↔ Sponsorship, RECEIVES from Rollforward |
| **Rollforward** | SENDS UP to Global, RECEIVES from Capex | SENDS to Ticketing, RECEIVES from Capex |

---

## 🛠️ How to Use These Diagrams

### Opening the Diagrams
1. Open `.drawio` files in [Draw.io](https://app.diagrams.net/) (free, web-based)
2. Or install Draw.io Desktop app
3. Files are editable - update as architecture changes

### Reading the Diagrams
1. **Start at the bottom** (Segments or Registers)
2. **Follow colored arrows** to see data movement
3. **Trace individual cubes** using color coding
4. **Check labels** for data types (P&L, Metrics, Headcount, etc.)

### Updating the Diagrams
1. **Add new cube:** Place in appropriate tier, update Channel Map
2. **Add new flow:** Assign next available channel X coordinate
3. **Verify no overlaps:** Visual inspection of all routing zones
4. **Update README:** Document new flow in tables above

### Maintenance Best Practices
- **Keep channel spacing** (40-80px) to prevent future overlaps
- **Use waypoints** for all flows (no direct diagonal lines)
- **Color code consistently** (each cube's flows = same color)
- **Update both diagrams** when model syncs change behavior
- **Regenerate channel maps** if >5 flows added to a zone

---

## 📊 Flow Statistics

### By Scenario
- **Actual:** 28 flows (14 down, 13 up, 1 lateral)
- **Budget:** 39 flows (18 down, 2 registers up, 13 up, 6 lateral)
- **Total Unique:** 67 cube-to-cube connections

### By Cube Type
- **Segments:** 23 outbound connections (to Concerts/Global)
- **Specialty:** 32 outbound connections (to Segments/Global/Other Specialty)
- **Global:** 14 inbound connections (receives from all cube types)
- **Concerts:** 5 inbound connections (receives from segments only)

### By Data Type
- **P&L:** 25 flows
- **Metrics:** 18 flows (Show, Capex, Ticketing, Corporate, Ops)
- **Headcount:** 12 flows
- **Multi-type:** 12 flows (P&L + Metrics, Labor + Headcount, etc.)

---

## 🚨 Common Pitfalls & Solutions

### Problem: Arrows Overlap
**Solution:** Check Channel Map, assign flows to unused X coordinates

### Problem: Can't Trace a Cube's Journey
**Solution:** Use color coding - all flows from one cube share the same color

### Problem: Diagram Too Complex
**Solution:** View one scenario at a time, focus on one tier at a time

### Problem: Need to Add a New Cube
**Solution:**
1. Determine tier (Reporting/Specialty/Segment/Register)
2. Add cube to appropriate tier with 340px+ spacing
3. Assign dedicated channels (40-80px apart)
4. Update Channel Map document
5. Update this README

### Problem: Scenario Flow Direction Changed
**Solution:** OneStream model sync behavior can change - update BOTH diagrams

---

## 📖 Related Documentation

**For Full Architecture (Including External Systems):**
- See: `temp/LNE_ FP&A_Solution Architecture Diagram.pdf` (45 pages, 338.3KB)
- Contains: Inbound integrations, Outbound feeds, BI Blend, Bulk Upload, Drill Back

**For Model Sync Implementation:**
- See: `LNE_Model_Sync_Actual_Scenario_SIMPLIFIED.drawio` (23 syncs)
- See: `LNE_Model_Sync_Budget_Scenario_SIMPLIFIED.drawio` (40 syncs)
- See: `README_Model_Syncs.md` (Model Sync-specific documentation)

**For Channel Routing Details:**
- See: `Channel_Map_Actual_Scenario.md` (28 flows mapped)
- See: `Channel_Map_Budget_Scenario.md` (39 flows mapped)

---

## 🎓 Key Takeaways

1. **Three-Diagram Approach:** Focused diagrams prevent overlaps (A: Downward, B: Upward to Concerts, C: Upward to Global)
2. **ZERO Overlaps Guaranteed:** Wide channel spacing (70-100px) with proper waypoint routing
3. **Color-Coded Traceability:** Follow each cube's journey independently across diagrams
4. **Cube-to-Cube ONLY:** External systems excluded (see full PDF for those)
5. **15 Cubes Total:** 2 Reporting, 6 Specialty, 5 Segments, 2 Registers
6. **28 Actual Flows:** 14 downward allocations + 4 upward to Concerts + 10 upward to Global
7. **Simpler Maintenance:** Update one diagram at a time instead of complex multi-flow diagrams

---

## 📝 Document History

| Date | Version | Changes | Author |
|------|---------|---------|--------|
| 2025-11-12 | 1.0 | Initial creation - Actual scenario diagrams (Option 2: 3-diagram approach) | Claude Code |

### Design Evolution
**Initial Approach (Rejected):** Single comprehensive diagram per scenario with all 28 flows
- **Problem:** Arrows overlapped despite channel planning
- **User Feedback:** "I DO NOT WANT the connectors to overlap over each other"

**Final Approach (Option 2):** Three focused diagrams by flow type
- **Diagram A:** Downward allocations only (14 flows)
- **Diagram B:** Upward to Concerts only (4 flows)
- **Diagram C:** Upward to Global + lateral (10 flows)
- **Result:** ZERO overlaps, each cube journey independently traceable
- **Channel Spacing:** Increased from 40px to 70-100px for maximum separation

---

## 💬 Questions & Support

For questions about:
- **Diagram interpretation:** Reference this README
- **Channel strategy:** See Channel Map files
- **Full architecture:** See original PDF in temp/ folder
- **Implementation details:** Check CLAUDE.md LESSONS LEARNED section

**Maintained by:** Live Nation OneStream Implementation Team
**Last Updated:** 2025-11-12
