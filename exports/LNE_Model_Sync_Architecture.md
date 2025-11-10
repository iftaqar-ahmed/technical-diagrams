# Live Nation OneStream Model Sync Architecture

**Date:** 2025-11-10
**Source:** Live Nation FP&A Solution Architecture Diagram
**Total Model Syncs:** 71

---

## 📋 Import Instructions for Draw.io

### Option 1: CSV Import (Recommended)
1. Open https://app.diagrams.net
2. **File** > **Import from** > **CSV**
3. Select `LNE_Model_Sync_DataFlow.csv`
4. Use default import settings
5. Click **Import**
6. **Arrange** > **Layout** > **Horizontal Flow** (optional for better layout)

### Option 2: Manual Layout Reference
Use this document as a reference for manually creating the diagram.

---

## 🏗️ Architecture Overview

### Cube Types

| Type | Cubes | Color | Purpose |
|------|-------|-------|---------|
| **Segment** | Event, Festival, PSS, Restaurant Ops, Artist Nation, Sponsorship, Ticketing | Yellow | Operational business units |
| **Specialty** | Fixed Expense, People, Capex, Rollforward | Blue | Corporate support functions |
| **Reporting** | Concerts, Global | Red | Aggregation and consolidation |

### Data Flow Patterns

| Scenario | Pattern | Color | Description |
|----------|---------|-------|-------------|
| **Actual** | Segments → Up | Green | Operational results flow to corporate |
| **Budget/Forecast** | Specialty → Down | Orange | Corporate allocations flow to segments |

---

## 🔄 Actual Scenario Flows (31 syncs)

### Pattern 1: Segments → People (7 syncs)
All segment cubes contribute headcount actuals to People Cube for workforce reporting.

```
Event → People (Headcount Metrics)
Festival → People (Headcount Metrics)
PSS → People (Headcount Metrics)
Restaurant Ops → People (Headcount Metrics)
Artist Nation → People (Headcount Metrics)
Sponsorship → People (Headcount Metrics)
Ticketing → People (Headcount Metrics)
```

### Pattern 2: Segments → Capex (7 syncs)
All segment cubes contribute capex actuals to Capex Cube for asset tracking.

```
Event → Capex (Capex Metrics)
Festival → Capex (Capex Metrics)
PSS → Capex (Capex Metrics)
Restaurant Ops → Capex (Capex Metrics)
Artist Nation → Capex (Capex Metrics)
Sponsorship → Capex (Capex Metrics)
Ticketing → Capex (Capex Metrics)
```

### Pattern 3: Concert Segments → Concerts (4 syncs)
Concert-related segments aggregate to Concerts reporting cube.

```
Event → Concerts (Show P&L + Metrics, COA + Artist)
Festival → Concerts (Show P&L + Metrics, COA + Artist)
PSS → Concerts (Show Metrics, COA + Artist)
Restaurant Ops → Concerts (Ops Metrics, COA + Artist)
```

### Pattern 4: All Cubes → Global (12 syncs)
All cubes contribute to Global for corporate consolidation.

```
Event → Global (Show P&L + Metrics)
Festival → Global (Show P&L + Metrics)
PSS → Global (P&L + Metrics)
Restaurant Ops → Global (P&L + Metrics)
Artist Nation → Global (P&L)
Sponsorship → Global (P&L)
Ticketing → Global (Ticketing Metrics)
Fixed Expense → Global (Fixed / OIE P&L)
People → Global (Headcount Metrics)
Capex → Global (Capex Metrics + Categories)
Rollforward → Global (Cash Flow Impact)
Concerts → Global (Consolidated P&L + Metrics)
```

### Pattern 5: Specialty Contributions (1 sync)
```
Rollforward → Capex (Capex Metrics from PPM projects)
```

---

## 📈 Budget/Forecast Scenario Flows (40 syncs)

### Pattern 1: Fixed Expense → Segments (8 syncs)
Fixed Expense allocates overhead to all segments and Global.

```
Fixed Expense → Event (Fixed / OIE P&L)
Fixed Expense → Festival (Fixed / OIE P&L)
Fixed Expense → PSS (Fixed / OIE P&L)
Fixed Expense → Restaurant Ops (Fixed / OIE P&L)
Fixed Expense → Artist Nation (Fixed / OIE P&L)
Fixed Expense → Sponsorship (Fixed / OIE P&L)
Fixed Expense → Ticketing (Fixed / OIE P&L)
Fixed Expense → Global (Fixed / OIE P&L)
```

### Pattern 2: People → Segments (8 syncs)
People allocates labor budget to all segments and Global.

```
People → Event (Labor P&L + Headcount)
People → Festival (Labor P&L + Headcount)
People → PSS (Labor P&L + Headcount)
People → Restaurant Ops (Labor P&L + Headcount)
People → Artist Nation (Labor P&L + Headcount)
People → Sponsorship (Labor P&L + Headcount)
People → Ticketing (Labor P&L + Headcount)
People → Global (Labor P&L + Headcount)
```

### Pattern 3: Capex → Segments (8 syncs)
Capex allocates depreciation budget to all segments and Global.

```
Capex → Event (Capex Metrics)
Capex → Festival (Capex Metrics)
Capex → PSS (Capex Metrics)
Capex → Restaurant Ops (Capex Metrics)
Capex → Artist Nation (Capex Metrics)
Capex → Sponsorship (Capex Metrics)
Capex → Ticketing (Capex Metrics)
Capex → Global (Capex Metrics + Categories)
```

### Pattern 4: Concert Segments → Concerts (4 syncs)
Concert-related segments aggregate to Concerts reporting cube.

```
Event → Concerts (P&L + Metrics, COA + Artist)
Festival → Concerts (P&L + Metrics, COA + Artist)
PSS → Concerts (P&L + Metrics, COA + Artist)
Restaurant Ops → Concerts (P&L + Metrics, COA + Artist)
```

### Pattern 5: Segments → Global (8 syncs)
All segments contribute budget to Global consolidation.

```
Event → Global (P&L + Metrics)
Festival → Global (P&L + Metrics)
PSS → Global (P&L + Metrics)
Restaurant Ops → Global (P&L + Metrics)
Artist Nation → Global (P&L)
Sponsorship → Global (P&L)
Ticketing → Global (P&L + Ticketing Metrics)
Concerts → Global (Consolidated P&L + Metrics)
```

### Pattern 6: Cross-Segment Flows (4 syncs)
Special business relationships between segments.

```
Sponsorship → Festival (P&L for Festival-specific sponsorship)
Sponsorship → Ticketing (P&L for ticketing partnerships)
Ticketing → Sponsorship (Reverse allocation)
Rollforward → Ticketing (Advance Metrics for ticket pre-sales)
```

---

## 📊 Dependency Order (Execution Sequence)

### Actual Scenario
```
TIER 1: Segments generate actuals
   ↓
TIER 2: Specialty cubes aggregate (People, Capex)
   ↓
TIER 3: Reporting cubes aggregate (Concerts)
   ↓
TIER 4: Global consolidates all
```

### Budget/Forecast Scenario
```
TIER 1: Specialty cubes allocate (Fixed, People, Capex)
   ↓
TIER 2: Segments receive allocations
   ↓
TIER 3: Cross-segment flows (Sponsorship ↔ Festival/Ticketing)
   ↓
TIER 4: Reporting cubes aggregate (Concerts)
   ↓
TIER 5: Global consolidates all
```

---

## 🎯 Key Insights

### Bidirectional Cubes
These cubes have **opposite flows** in Actual vs Budget:
- **Segment Cubes:** Send in Actual, Receive in Budget
- **People Cube:** Receives in Actual, Sends in Budget
- **Capex Cube:** Receives in Actual, Sends in Budget

### Unidirectional Cubes
These cubes always flow in the same direction:
- **Fixed Expense:** Always sends down (never receives from segments)
- **Global:** Always receives up (never sends down)
- **Concerts:** Always receives from 4 segments (aggregation only)

### Cross-Segment Relationships
- **Sponsorship ↔ Festival:** Festival-specific sponsorship deals
- **Sponsorship ↔ Ticketing:** Ticketing partnership revenue
- **Rollforward → Ticketing:** Ticket advance/pre-sale tracking

---

## 📈 Statistics

| Metric | Count |
|--------|-------|
| Total Cubes | 13 |
| Segment Cubes | 7 |
| Specialty Cubes | 4 |
| Reporting Cubes | 2 |
| Total Model Syncs | 71 |
| Actual Syncs | 31 |
| Budget/Forecast Syncs | 40 |
| Global Inbound Syncs | 24 (12 per scenario) |
| People Cube Total Syncs | 16 |
| Capex Cube Total Syncs | 17 |

---

## 🔧 Implementation Notes

### Finance Business Rule: `fin_Sync_Cubes.cs`
This rule handles all 71 Model Syncs based on:
1. **Scenario Parameter:** Actual vs Budget/Forecast
2. **Cube Parameter:** Which source/target cube pair
3. **Dynamic Formulas:** GetDataBufferUsingFormula() with cube-specific POVs

### Dashboard Trigger
Users can initiate syncs from dashboard:
- **Extender Rule:** `LNE_SolutionHelper_SyncData` (sets parameters)
- **Data Management:** Executes `fin_Sync_Cubes` Finance BR
- **Parameter:** `prm_FXE_SyncData_Cube` (Event, Festival, PSS, etc.)

### Formula Pattern
```csharp
// Source Formula
string sourceFormula = "Cb#[SourceCube]:S#Act_Prod:V#Periodic:A#[Accounts].Base";
var buffer = api.Data.GetDataBufferUsingFormula(sourceFormula);

// Destination POV
var destination = api.Data.GetExpressionDestinationInfo("Cb#[TargetCube]:O#Import");

// Execute Sync
api.Data.SetDataBuffer(buffer, destination);
```

---

## 📁 Related Files

- **CSV for Draw.io:** `LNE_Model_Sync_DataFlow.csv`
- **Finance BR:** `Business_Rules/Finance/86358_Sync_Cubes.cs`
- **Source PDF:** `temp/LNE_ FP&A_Solution Architecture Diagram.pdf`
- **Master Sync Table:** This document (section above)

---

## 🚀 Next Steps

1. ✅ Import CSV into draw.io for visualization
2. ⏳ Enhance `fin_Sync_Cubes.cs` with all 71 syncs
3. ⏳ Define source/destination formulas for each sync
4. ⏳ Create dashboard parameter UI for user-triggered syncs
5. ⏳ Test syncs in dependency order

---

**End of Document**
