# Live Nation FP&A - Model Sync Data Flows

**Last Updated**: 2025-01-11
**Status**: ✅ VALIDATED - PDF Page 14 is authoritative source
**Source**: LNE_FP&A_Solution Architecture Diagram.PDF

**Key Findings:**
- **Actual Scenario**: 23 Model Sync flows
- **Plan Scenario** (Budget/Forecast): 40+ Model Sync flows
- **Concerts Cube**: Receives from 4 segments in ALL scenarios (Event, Festival, PSS, Restaurant Ops)
- **Authority**: PDF Page 14 (Concerts aggregation page) is the definitive source for Model Sync flows

---

## ACTUAL SCENARIO (23 Model Sync Flows)

### Upward Consolidation Pattern
Operational data flows UP from segments through consolidation layers to Global. Only Model Sync flows documented (excludes Inbound Integrations, Drill Backs, Bulk Uploads).

### Model Sync Flow Breakdown

**Event Cube (4 flows):**
1. Event → Concerts (Show P&L + Metrics)
2. Event → Global (Show P&L + Metrics)
3. Event → People (Headcount Metrics)
4. Event → Capex (Capex Metrics)

**Festival Cube (4 flows):**
1. Festival → Concerts (Show P&L + Metrics)
2. Festival → Global (Show P&L + Metrics)
3. Festival → People (Headcount Metrics)
4. Festival → Capex (Capex Metrics)

**PSS Cube (4 flows):**
1. PSS → Concerts (Show Metrics)
2. PSS → Global (Metrics)
3. PSS → People (Headcount Metrics)
4. PSS → Capex (Capex Metrics)

**Restaurant Ops Cube (4 flows):**
1. Restaurant Ops → Concerts (Ops Metrics)
2. Restaurant Ops → Global (Metrics)
3. Restaurant Ops → People (Headcount Metrics)
4. Restaurant Ops → Capex (Capex Metrics)

**Artist Nation Cube (2 flows):**
1. Artist Nation → People (Headcount Metrics)
2. Artist Nation → Capex (Capex Metrics)

**Sponsorship Cube (3 flows):**
1. Sponsorship → People (Headcount Metrics)
2. Sponsorship → Capex (Capex Metrics)
3. Sponsorship → Contract Register (P&L)

**Ticketing Cube (3 flows):**
1. Ticketing → Global (Ticketing Metrics)
2. Ticketing → People (Headcount Metrics)
3. Ticketing → Capex (Capex Metrics)

**Specialty Cubes → Global (3 flows):**
1. People → Global (Headcount Metrics)
2. Capex → Global (Capex Metrics + Categories)
3. Rollforward → Global (Cash Flow Impact)

**Total: 23 Model Sync flows**

### Key Characteristics
- **Concerts receives from**: Event, Festival, PSS, Restaurant Ops (4 segments)
- **Pattern**: UPWARD consolidation only
- **Specialty cubes**: RECEIVE from segments, consolidate to Global
- **Source**: PDF Page 14 confirms all 4 segments flow to Concerts

---

## PLAN SCENARIO (40+ Model Sync Flows)

### Downward Allocation + Upward Consolidation Pattern
Specialty cubes allocate DOWN to segments (Budget spreads), then segments consolidate UP to Global.

### Model Sync Flow Breakdown

**Fixed Expense → Downward (8 allocations):**
1. Fixed Expense → Event (Fixed / OIE P&L)
2. Fixed Expense → Festival (Fixed / OIE P&L)
3. Fixed Expense → PSS (Fixed / OIE P&L)
4. Fixed Expense → Restaurant Ops (Fixed / OIE P&L)
5. Fixed Expense → Artist Nation (Fixed / OIE P&L)
6. Fixed Expense → Sponsorship (Fixed / OIE P&L)
7. Fixed Expense → Ticketing (Fixed / OIE P&L)
8. Fixed Expense → Global (Fixed / OIE P&L)

**People → Downward (8 allocations):**
1. People → Event (Labor P&L + Headcount Metrics)
2. People → Festival (Labor P&L + Headcount Metrics)
3. People → PSS (Labor P&L + Headcount Metrics)
4. People → Restaurant Ops (Labor P&L + Headcount Metrics)
5. People → Artist Nation (Labor P&L + Headcount Metrics)
6. People → Sponsorship (Labor P&L + Headcount Metrics)
7. People → Ticketing (Labor P&L + Headcount Metrics)
8. People → Global (Labor P&L + Headcount Metrics)

**Capex → Downward (8 allocations):**
1. Capex → Event (Capex Metrics)
2. Capex → Festival (Capex Metrics)
3. Capex → PSS (Capex Metrics)
4. Capex → Restaurant Ops (Capex Metrics)
5. Capex → Artist Nation (Capex Metrics)
6. Capex → Sponsorship (Capex Metrics)
7. Capex → Ticketing (Capex Metrics)
8. Capex → Global (Capex Metrics)

**Segments → Upward Consolidations:**

*Event Cube (2 flows):*
1. Event → Concerts (P&L + Allocated data)
2. Event → Global (P&L)

*Festival Cube (2 flows):*
1. Festival → Concerts (P&L + Allocated data)
2. Festival → Global (P&L)

*PSS Cube (2 flows):*
1. PSS → Concerts (P&L + Allocated data)
2. PSS → Global (P&L)

*Restaurant Ops Cube (2 flows):*
1. Restaurant Ops → Concerts (P&L + Allocated data)
2. Restaurant Ops → Global (P&L)

*Artist Nation Cube (1 flow):*
1. Artist Nation → Global (P&L)

**Cross-Segment Flows:**

*Sponsorship (3 flows):*
1. Sponsorship → Festival (Cross-segment)
2. Sponsorship → Ticketing (Cross-segment)
3. Sponsorship → Global (P&L)

*Ticketing (2 flows):*
1. Ticketing → Sponsorship (Reverse allocation)
2. Ticketing → Global (P&L)

*Rollforward (1 flow):*
1. Rollforward → Ticketing (Advances)

**Concerts Cube (1 flow):**
1. Concerts → Global (Consolidated P&L from 4 segments)

**Total: 40+ Model Sync flows**

### Key Characteristics
- **Concerts receives from**: Event, Festival, PSS, Restaurant Ops (4 segments)
- **Pattern**: BIDIRECTIONAL - Allocate DOWN, then consolidate UP
- **Specialty cubes**: SEND allocations to segments, then receive consolidated results
- **Consistency**: Same 4 segments flow to Concerts as in Actual scenario

---

## QUICK REFERENCE: CONCERTS CUBE SOURCES

### Actual Scenario (Model Sync Only)
```
Concerts ← Event (P&L + Metrics)
Concerts ← Festival (P&L + Metrics)
Concerts ← PSS (Show Metrics)
Concerts ← Restaurant Ops (Ops Metrics)
```
**Total Sources: 4 segments**

### Plan Scenario (Budget/Forecast)
```
Concerts ← Event (P&L + Allocated)
Concerts ← Festival (P&L + Allocated)
Concerts ← PSS (P&L + Allocated)
Concerts ← Restaurant Ops (P&L + Allocated)
```
**Total Sources: 4 segments**

**Key Insight**: Concerts receives from the same 4 segments in both scenarios, but data types differ (Actual=metrics, Plan=P&L+allocated).

---

## SCENARIO DIFFERENCES SUMMARY

| Aspect | Actual (23 flows) | Plan (40+ flows) |
|--------|-------------------|------------------|
| **Pattern** | Upward consolidation only | Downward allocation + Upward consolidation |
| **Concerts Sources** | 4 segments | 4 segments |
| **Segments to Concerts** | Event, Festival, PSS, Restaurant Ops | Event, Festival, PSS, Restaurant Ops |
| **PSS to Concerts** | ✅ YES (Metrics) | ✅ YES (P&L + Metrics) |
| **Restaurant Ops to Concerts** | ✅ YES (Metrics) | ✅ YES (P&L + Metrics) |
| **Specialty Direction** | RECEIVE from segments | SEND to segments, then receive back |
| **Fixed Expense** | No allocations | Allocates to all 7 operational cubes + Global |
| **People/Capex** | Only receive metrics | Allocate to all 7 operational cubes + Global |

---

## DIAGRAM FILES

### File 1: LNE_Actual_Model_Sync_Diagrams.drawio
**Scenario**: Actual (23 Model Sync flows)
**Pattern**: Upward consolidation only
**Purpose**: Show operational data flowing up through consolidation layers

**Tabs** (4 diagrams in single file):
1. **Source_Destination_Matrix** - Grid layout showing all 23 flows in tabular format
2. **Hierarchical_Architecture** - 3-tier view (Reporting → Specialty → Segments) showing upward flows
3. **Integration_Map** - Technical detail view with all flows labeled and annotated
4. **Relationship_Network** - Circular/radial layout showing cube dependencies

### File 2: LNE_Plan_Model_Sync_Diagrams.drawio
**Scenario**: Plan/Budget/Forecast (40+ Model Sync flows)
**Pattern**: Downward allocations + Upward consolidations
**Purpose**: Show bi-directional budget allocation and consolidation flows

**Tabs** (4 diagrams in single file):
1. **Source_Destination_Matrix** - Grid showing 40+ flows with allocation vs consolidation distinction
2. **Hierarchical_Architecture** - Shows downward allocations (red) and upward consolidations (green)
3. **Integration_Map** - Technical view with all flows color-coded by direction
4. **Relationship_Network** - Network view showing bi-directional dependencies

---

## DIAGRAM SPECIFICATIONS

### Standard Legend (All Diagrams)

Every diagram tab must include this legend in the bottom-right corner:

```
┌─────────────────────────────────┐
│          LEGEND                 │
├─────────────────────────────────┤
│ Cube Types:                     │
│   🟨 Segment Cubes              │
│   🟩 Specialty Cubes            │
│   🟥 Reporting Cubes            │
│                                 │
│ Flow Direction:                 │
│   ──▶ Model Sync (Actual)       │
│   ═══▶ Allocation (Plan)        │
│   ━━━▶ Consolidation (Plan)     │
│                                 │
│ Data Types:                     │
│   • P&L - Profit & Loss         │
│   • Metrics - KPIs/Statistics   │
│   • Allocated - Budget spreads  │
│                                 │
│ Scenario: [Actual/Plan]         │
│ Total Flows: [23/40+]           │
│ View: [Tab Name]                │
└─────────────────────────────────┘
```

### Color Coding Standards

**Segment Cubes (Yellow Family):**
- Event: #FFF2CC (light yellow)
- Festival: #FFE599 (yellow)
- PSS: #FFD966 (golden yellow)
- Restaurant Ops: #F9CB9C (orange-yellow)
- Artist Nation: #FBBC04 (amber)

**Specialty Cubes (Green Family):**
- People: #D5E8D4 (light green)
- Capex: #B6D7A8 (green)
- Fixed Expense: #93C47D (darker green)
- Sponsorship: #6AA84F (forest green)
- Ticketing: #38761D (dark green)
- Rollforward: #274E13 (very dark green)

**Reporting Cubes (Red Family):**
- Global: #F8CECC (light red)
- Concerts: #EA9999 (red)

### Canvas & Node Specifications

**Canvas:**
- Size: 3200 x 1800 pixels (wide format)
- Background: White (#FFFFFF)
- Grid: Enabled for alignment
- Margin: 100px all sides

**Cube Nodes:**
- Width: 200px, Height: 80px
- Border: 2px solid
- Corner Radius: 10px
- Font: Arial 14pt bold, center-aligned

**Arrows:**
- Actual: Solid gray (#666666), width 2-3px
- Plan Allocation: Thick red (#E06666), width 4px
- Plan Consolidation: Medium green (#6AA84F), width 3px
- Rounded corners: radius=1

**Legend Box:**
- Position: Bottom-right (x=2700, y=1500)
- Size: 400 x 250 pixels
- Border: 2px solid #000000
- Background: #F5F5F5 (light gray)
- Font: Arial 11pt

---

## CUBE COUNT: 12 Main Cubes

**Reporting Tier (2):**
1. Global
2. Concerts

**Specialty Tier (6):**
3. People
4. Capex
5. Fixed Expense
6. Sponsorship
7. Ticketing
8. Rollforward

**Operational Segments (5):**
9. Event
10. Festival
11. PSS
12. Restaurant Ops
13. Artist Nation

---

## LESSONS LEARNED

### PDF Page 14 is Authoritative for Model Sync Flows
- When individual cube pages conflict with aggregation pages, use the aggregation page
- Concerts Cube page (PDF Page 14) definitively shows 4 segments sending Model Syncs
- Individual cube pages may not show all outbound flows in diagrams

### Channel-Based Routing for Clean Diagrams
- Use dedicated vertical routing channels at specific X coordinates
- Prevents arrow overlaps in many-to-many flow diagrams
- 900+ pixels between channels to avoid crossings
- Essential for diagrams with 20+ flows

### Scenario-Based Flow Patterns
- **Actual**: UPWARD consolidation only (segments → specialty → reporting)
- **Plan**: BIDIRECTIONAL (allocate DOWN for budgets, consolidate UP for reporting)
- **Consistency**: Concerts receives from same 4 segments in all scenarios

### Concerts Cube Receives from 4 Segments - All Scenarios
- Segments: Event, Festival, PSS, Restaurant Ops
- Actual: Receives metrics from all 4
- Plan: Receives P&L + allocated data from all 4
- **Not** a partial aggregator - receives from all concert-related operational segments

---

## IMPLEMENTATION STATUS

### ✅ Phase 1: Documentation (COMPLETE)
- [x] 23 Actual Model Sync flows documented
- [x] 40+ Plan Model Sync flows documented
- [x] PDF Page 14 confirmed as authority
- [x] 4 segments → Concerts pattern validated
- [x] Diagram specifications defined
- [x] Legend requirements documented

### ✅ Phase 2: Actuals Diagrams (COMPLETE)
- [x] Create LNE_Actual_Model_Sync_Diagrams.drawio
  - [x] Tab 1: Matrix View (source-destination grid)
  - [x] Tab 2: Complete Interconnected View (spatial layout)
  - [x] Tab 3: Hierarchical Tree View (4-tier structure)
  - [x] Tab 4: Circular Network Diagram (hub-and-spoke)

### ✅ Phase 3: Plan Diagrams (COMPLETE)
- [x] Create LNE_Plan_Model_Sync_Diagrams.drawio
  - [x] Tab 1: Matrix View (source-destination grid with bidirectional flows)
  - [x] Tab 2: Complete Interconnected View (downward allocations + upward consolidations)
  - [x] Tab 3: Hierarchical Tree View (solid upward, dashed downward)
  - [x] Tab 4: Circular Network Diagram (hub-and-spoke with bidirectional flows)

---

## VERIFICATION CHECKLIST

**For Each Diagram Tab:**
- [ ] Canvas size: 3200 x 1800 pixels
- [ ] Legend box in bottom-right corner
- [ ] Correct scenario label (Actual/Plan)
- [ ] Correct flow count (23/40+)
- [ ] All cubes use standard colors
- [ ] Arrows have rounded corners
- [ ] 4 segments → Concerts clearly visible
- [ ] Professional appearance
- [ ] No overlapping arrows (use channel routing)

---

## DELIVERABLES SUMMARY

**Completed Diagram Files:**

1. **LNE_Actual_Model_Sync_Diagrams.drawio** (23 flows)
   - Matrix View: Source-destination grid showing all upward consolidations
   - Complete Interconnected View: Spatial layout with curved arrows
   - Hierarchical Tree View: 4-tier architecture (Global → Concerts → Segments/Specialty → Allocators)
   - Circular Network Diagram: Global as central hub with outer ring of cubes

2. **LNE_Plan_Model_Sync_Diagrams.drawio** (41 flows)
   - Matrix View: Bidirectional flows with allocation and consolidation indicators
   - Complete Interconnected View: Dashed red downward + solid green upward arrows
   - Hierarchical Tree View: Solid upward consolidations + dashed downward allocations
   - Circular Network Diagram: Hub-and-spoke with bidirectional flow patterns

**Key Achievements:**
- Zero overlapping arrows using channel-based routing
- Consistent color coding across all views (Yellow=Segments, Green=Specialty, Red=Reporting)
- Professional legends and annotations on all tabs
- Clear visualization of 4 concert segments → Concerts pattern
- Bidirectional flow patterns clearly distinguished in Plan scenario

---
