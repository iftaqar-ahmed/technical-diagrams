# Live Nation OneStream Model Sync Architecture

**Date:** 2025-11-10
**Source:** PDF Analysis (Slides 3-34)
**Total Model Syncs:** 71 (31 Actual + 40 Budget/Forecast)

---

## 📂 Files in this Directory

| File | Description |
|------|-------------|
| **LNE_Model_Sync_Actual_Scenario.drawio** | Actual scenario diagram (31 syncs) - operational data flows UP |
| **LNE_Model_Sync_Budget_Scenario.drawio** | Budget/Forecast scenario diagram (40 syncs) - allocations DOWN, rollups UP |
| **LNE_Model_Sync_Architecture_REVISED.md** | Complete technical specification with all 71 syncs documented |
| **README_Model_Syncs.md** | This file - quick reference guide |

---

## 🔑 Key Differences Between Scenarios

| Aspect | Actual Scenario | Budget/Forecast Scenario |
|--------|-----------------|--------------------------|
| **Total Syncs** | 31 | 40 |
| **Primary Flow** | UP (Operational → Corporate) | DOWN (Allocations) then UP (Rollups) |
| **Specialty Cube Role** | **RECEIVE** from segments | **SEND** to segments |
| **Segment Cube Role** | **SEND** to specialty/reporting | **RECEIVE** allocations, **SEND** rollups |
| **Fixed Expense** | No syncs (actuals not tracked) | Sends to ALL segments (8 syncs) |
| **People Cube** | Receives from segments (7 syncs) | Sends to segments (8 syncs) |
| **Capex Cube** | Receives from segments (7 syncs) | Sends to segments (8 syncs) |
| **Cross-Segment Flows** | None | 4 syncs (Sponsorship ↔ Festival/Ticketing) |

---

## 📊 Actual Scenario (31 Syncs)

### Flow Pattern
```
Segments (Event, Festival, PSS, RestOps, ArtistNation, Sponsorship, Ticketing)
    ↓ Send Actuals UP (Headcount, Capex Metrics)
Specialty Cubes (People, Capex, Rollforward)
    ↓ Aggregate & Send UP
Reporting Cubes (Concerts - concert segments only)
    ↓ Consolidate & Send UP
Global Cube (Final Consolidation)
```

### Breakdown
- **Segments → People:** 7 syncs (Headcount Metrics)
- **Segments → Capex:** 7 syncs (Capex Metrics)
- **Concert Segments → Concerts:** 2 syncs (Event, Festival only)
- **All Cubes → Global:** 12 syncs (P&L + Metrics)
- **Rollforward → Capex/Global:** 2 syncs (PPM projects + cash flow)

### Special Cases
- **PSS and Restaurant Ops** do NOT sync to Concerts or Global in Actual (only People and Capex)
- **Artist Nation, Sponsorship** do NOT sync to Concerts (not concert-related segments)
- **Ticketing** has direct sync to Global (Ticketing Metrics)

---

## 📈 Budget/Forecast Scenario (40 Syncs)

### Flow Pattern
```
Specialty Cubes (Fixed Expense, People, Capex)
    ↓ ALLOCATE DOWN (24 syncs)
Segments (Receive allocations, prepare budgets)
    ↓ ROLL UP (16 syncs)
Reporting Cubes (Concerts)
    ↓ Consolidate UP
Global Cube (Final Consolidation)
```

### Breakdown

**DOWNWARD ALLOCATIONS (24 syncs):**
- **Fixed Expense → All Segments + Global:** 8 syncs (Fixed/OIE P&L)
- **People → All Segments + Global:** 8 syncs (Labor P&L + Headcount)
- **Capex → All Segments + Global:** 8 syncs (Capex Metrics + Depreciation)

**UPWARD ROLLUPS (12 syncs):**
- **Concert Segments → Concerts:** 4 syncs (Event, Festival, PSS, RestOps)
- **All Segments → Global:** 8 syncs (Budget consolidation)

**CROSS-SEGMENT FLOWS (4 syncs):**
- **Sponsorship → Festival:** 1 sync (Festival-specific sponsorship revenue)
- **Sponsorship → Ticketing:** 1 sync (Ticketing partnerships)
- **Rollforward → Ticketing:** 1 sync (Advance metrics for pre-sales)
- **Rollforward → Global:** 1 sync (Cash flow impact)

### Special Cases
- **PSS and Restaurant Ops** DO sync to Concerts in Budget (unlike Actual)
- **Named Event Register → Event** in Budget (not in Actual)
- **PSS Register → PSS** in Forecast (not in Budget)
- **Sponsorship bidirectional flows** with Festival and Ticketing (Budget only)

---

## 🔀 Bidirectional Cubes (Flow Reverses by Scenario)

| Cube | Actual Flow | Budget Flow |
|------|-------------|-------------|
| **Event** | Sends UP → People, Capex, Concerts, Global | Receives DOWN ← Fixed, People, Capex; Sends UP → Concerts, Global |
| **Festival** | Sends UP → People, Capex, Concerts, Global | Receives DOWN ← Fixed, People, Capex, Sponsorship; Sends UP → Concerts, Global |
| **PSS** | Sends UP → People, Capex ONLY | Receives DOWN ← Fixed, People, Capex; Sends UP → Concerts, Global |
| **People** | Receives IN ← All Segments; Sends UP → Global | Receives IN ← Workday; Sends DOWN → All Segments + Global |
| **Capex** | Receives IN ← Segments/Rollforward; Sends UP → Global | Receives IN ← PPM; Sends DOWN → All Segments + Global |

---

## ➡️ Unidirectional Cubes (Same Flow Both Scenarios)

| Cube | Flow Pattern | Notes |
|------|--------------|-------|
| **Fixed Expense** | ALWAYS sends DOWN | Never receives from segments (no Fixed actuals tracked) |
| **Global** | ALWAYS receives UP | Never sends down (final consolidation point) |
| **Concerts** | ALWAYS receives IN | Aggregation from 4 concert segments only (Event, Festival, PSS, RestOps) |
| **Rollforward** | ALWAYS sends UP | Never receives from segments (PPM-sourced data) |

---

## 🎯 Concert-Related Segments

Only these 4 segments sync to Concerts cube:

1. **Event** - Full COA + Artist granularity
2. **Festival** - Full COA + Artist granularity
3. **PSS** - COA + Artist (Budget only, not Actual)
4. **Restaurant Ops** - COA + Artist (Budget only, not Actual)

**Excluded:** Artist Nation, Sponsorship, Ticketing do NOT sync to Concerts

---

## 💡 Key Insights

### Why Different Patterns?

**Actual Scenario:**
- Captures operational reality (what actually happened)
- Segments collect actuals from source systems, send UP for consolidation
- People and Capex aggregate actual headcount/depreciation costs
- No allocations needed (actuals are factual, not planned)

**Budget/Forecast Scenario:**
- Plans future operations (what should happen)
- Corporate functions (Fixed, People, Capex) allocate budgets DOWN to segments
- Segments build detailed budgets, roll UP for approval
- Cross-segment coordination (Sponsorship partnerships) happens in planning phase

### Data Flow Timing

**Actual Scenario:**
```
Month-end close → Source systems → Segments → Specialty → Global (consolidation)
```

**Budget Scenario:**
```
Annual planning → Corporate allocations → Segments → Budget submission → Global (approval)
```

---

## 🚀 Implementation Notes

### Finance Business Rule: `fin_Sync_Cubes.cs`

The implementation must:
1. **Detect scenario** from workflow context (`api.DataUnitPk.ScenarioKey`)
2. **Route to correct sync pattern** based on scenario
3. **Execute syncs in dependency order:**
   - **Actual:** Segments first → Specialty → Reporting → Global
   - **Budget:** Specialty first → Segments → Reporting → Global

### Example Code Structure

```csharp
string scenarioName = BRApi.Finance.Members.GetMemberName(si, DimType.Scenario.Id, api.DataUnitPk.ScenarioKey);
bool isActual = scenarioName.XFEqualsIgnoreCase("Act");

if (isActual)
{
    // ACTUAL: Segments send UP
    SyncSegmentToSpecialty();  // 14 syncs (People + Capex)
    SyncSegmentToConcerts();   // 2 syncs (Event, Festival)
    SyncAllToGlobal();         // 12 syncs
    SyncRollforwardFlows();    // 2 syncs
}
else
{
    // BUDGET: Specialty send DOWN, then segments send UP
    SyncSpecialtyToSegments(); // 24 syncs (Fixed + People + Capex)
    SyncCrossSegmentFlows();   // 4 syncs (Sponsorship, Rollforward)
    SyncSegmentToConcerts();   // 4 syncs (all concert segments)
    SyncSegmentToGlobal();     // 8 syncs
}
```

---

## 📌 Quick Reference

**Open Diagrams:**
1. Visit https://app.diagrams.net
2. **File → Open From → Device**
3. Select `.drawio` file
4. Diagrams render with proper colors, flow arrows, and legends

**Modify Diagrams:**
- Yellow boxes = Segment Cubes
- Green boxes = Specialty Cubes
- Red boxes = Reporting Cubes
- Orange arrows = Downward allocations
- Green arrows = Upward rollups
- Purple arrows = Cross-segment flows

---

**Last Updated:** 2025-11-10
**Document Version:** 1.0
