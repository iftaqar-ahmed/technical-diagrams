# Live Nation Model Sync - Actual Scenario (REVISED)

**Date:** 2025-11-10
**Source:** Updated configuration table
**Status:** ⚠️ DIFFERS FROM PREVIOUS DOCUMENTATION

---

## Summary

**Total Syncs:** 30+ (including expanded "All Segment" syncs)
**Pattern:** **BIDIRECTIONAL** (not purely upward as previously documented)
**Key Insight:** Specialty cubes (People, CapEx) send TO segments in Actual scenario

---

## Execution Order by Step

### Step 3: Segment Consolidation to Global
- All Segment Cubes → Global (Corporate Metrics)

### Step 4: Specialty Consolidation to Global
- PLP to Cube → People (Headcount Metrics)
- All Specialty Cubes → Global (Corporate Metrics)

### Step 5: Specialty Distribution to Segments
- People → All Segment Cubes (Headcount Metrics)
- People → Festival (Headcount Metrics)
- People → Ticketing (Headcount Metrics)
- CapEx → All Segment Cubes (Capex Metrics)
- Event → Concert (Show P&L + Show Metrics)
- All Segment Cubes → Global (P&L + Corporate Metrics)

### Step 6: Capex Distribution & Segment Rollup
- Event → Global (Show P&L + Show Metrics)
- CapEx → Festival (Capex Metrics)
- CapEx → Ticketing (Capex Metrics)
- CapEx → Global (Capex Metrics)
- People → Global (Headcount Metrics)

### Step 7: Concert Consolidation & External Feeds
- Festival → Concert (Show P&L + Show Metrics)
- Ticketing → Global (Ticketing Metrics)
- People → Sponsorship (Headcount Metrics)
- Roll Forward → CapEx (Capex Metrics)

### Step 8: Final Consolidation
- Festival → Global (Show P&L + Show Metrics)
- CapEx → Sponsorship (Capex Metrics)
- Roll Forward → Global (Capex Flow Impact)

### Step 9-10: Late Stage Allocations
- People → Event (Headcount Metrics) [Step 9]
- CapEx → Event (Capex Metrics) [Step 10]
- Contract Register → Sponsorship (P&L) [Step 10]

---

## Flow Pattern Analysis

### Phase 1: External Data Ingestion (Step 4)
```
PLP to Cube → People (Headcount actuals from Workday)
```

### Phase 2: Specialty Distribution (Steps 5-6)
```
People → All Segments (Headcount allocation/distribution)
CapEx → All Segments (Capex allocation/distribution)
```

**⚠️ CRITICAL DIFFERENCE:** This is DOWNWARD flow in Actual scenario, which contradicts previous documentation stating "Actual flows upward only."

### Phase 3: Segment Rollup (Steps 5-8)
```
Event → Concert → Global
Festival → Concert → Global
Ticketing → Global (direct)
Sponsorship → Global (via steps 7-8)
```

### Phase 4: External Integration (Steps 7-8)
```
Roll Forward → CapEx (PPM projects)
Roll Forward → Global (Cash flow impact)
Contract Register → Sponsorship (Contract-based P&L)
```

### Phase 5: Late Allocations (Steps 9-10)
```
People → Event (Headcount adjustments)
CapEx → Event (Capex adjustments)
```

---

## Cube-by-Cube Detail

### Event Cube (4 syncs)
| Direction | Source | Target | Data | Step |
|-----------|--------|--------|------|------|
| OUT | Event | Concert | Show P&L + Show Metrics | 5 |
| OUT | Event | Global | Show P&L + Show Metrics | 6 |
| IN | People | Event | Headcount Metrics | 9 |
| IN | CapEx | Event | Capex Metrics | 10 |

**Pattern:** Sends to reporting cubes early (steps 5-6), receives allocations late (steps 9-10)

### Festival Cube (4 syncs)
| Direction | Source | Target | Data | Step |
|-----------|--------|--------|------|------|
| IN | People | Festival | Headcount Metrics | 5 |
| IN | CapEx | Festival | Capex Metrics | 6 |
| OUT | Festival | Concert | Show P&L + Show Metrics | 7 |
| OUT | Festival | Global | Show P&L + Show Metrics | 8 |

**Pattern:** Receives allocations first, then sends to reporting

### Sponsorship Cube (3 syncs)
| Direction | Source | Target | Data | Step |
|-----------|--------|--------|------|------|
| IN | People | Sponsorship | Headcount Metrics | 7 |
| IN | CapEx | Sponsorship | Capex Metrics | 8 |
| IN | Contract Register | Sponsorship | P&L | 10 |

**Pattern:** Only receives (no outbound to Concert or Global shown in steps 7-10)

### Ticketing Cube (3 syncs)
| Direction | Source | Target | Data | Step |
|-----------|--------|--------|------|------|
| IN | People | Ticketing | Headcount Metrics | 5 |
| IN | CapEx | Ticketing | Capex Metrics | 6 |
| OUT | Ticketing | Global | Ticketing Metrics | 7 |

**Pattern:** Receives allocations, sends metrics to Global (no Concert sync)

### People Cube (3 syncs + distribution)
| Direction | Source | Target | Data | Step |
|-----------|--------|--------|------|------|
| IN | PLP to Cube | People | Headcount Metrics | 4 |
| OUT | People | All Segment Cubes | Headcount Metrics | 5 |
| OUT | People | Global | Headcount Metrics | 6 |

**Distribution targets (step 5):**
- Event (also step 9)
- Festival
- Sponsorship
- Ticketing
- PSS (implied by "All Segment Cubes")
- ArtistNation (implied)
- RestaurantOps (implied)

### CapEx Cube (2 syncs + distribution)
| Direction | Source | Target | Data | Step |
|-----------|--------|--------|------|------|
| OUT | CapEx | All Segment Cubes | Capex Metrics | 5 |
| OUT | CapEx | Global | Capex Metrics | 6 |

**Distribution targets (step 5-6):**
- Event (also step 10)
- Festival (step 6)
- Sponsorship (step 8)
- Ticketing (step 6)
- PSS (implied)
- ArtistNation (implied)
- RestaurantOps (implied)

### Roll Forward Cube (2 syncs)
| Direction | Source | Target | Data | Step |
|-----------|--------|--------|------|------|
| OUT | Roll Forward | CapEx | Capex Metrics | 7 |
| OUT | Roll Forward | Global | Capex Flow Impact | 8 |

**Pattern:** External data source, sends to specialty and reporting

### Global Cube (3 consolidation groups)
| Source | Data | Step |
|--------|------|------|
| All Segment Cubes | Corporate Metrics | 3 |
| All Specialty Cubes | Corporate Metrics | 4 |
| All Segment Cubes | P&L + Corporate Metrics | 5 |

**Consolidation sources:**
- Event, Festival, Sponsorship, Ticketing, PSS, ArtistNation, RestaurantOps
- People, CapEx, Fixed Expense (specialty)
- Roll Forward (external)

---

## Key Differences from Previous Documentation

### Previous Understanding (from README_Model_Syncs.md)
```
Actual Scenario: UPWARD ONLY
Segments → Specialty → Reporting → Global
```

**Example:**
- Segments send headcount UP to People cube
- People aggregates and sends UP to Global

### Current Configuration (from your table)
```
Actual Scenario: BIDIRECTIONAL
External → Specialty → Segments (allocation)
Segments → Reporting → Global (rollup)
```

**Example:**
- PLP sends TO People (external ingestion)
- People sends DOWN to segments (allocation)
- Segments send UP to Global (consolidation)

### Possible Explanations

1. **Actual scenario includes allocations** - Perhaps Live Nation allocates actual headcount and capex costs TO segments for full absorption costing
2. **Configuration changed** - The original diagrams were based on earlier documentation; current configuration reflects latest requirements
3. **Timing differences** - Steps 5-8 are allocations, steps 9-10 are adjustments/corrections
4. **Documentation mismatch** - Original PDF may have shown Budget logic, not Actual

---

## Questions for Clarification

1. **Is this the current production configuration?**
2. **Why do People and CapEx allocate TO segments in Actual?**
   - Is this absorption costing (allocating actual costs to business units)?
   - Or is this budgeted headcount/capex rates applied to actuals?
3. **What's the difference between step 5 and step 9 People → Event syncs?**
   - Step 5: People → All Segment Cubes
   - Step 9: People → Event (explicit)
   - Are these different data types or timing?
4. **Missing segments in explicit syncs:**
   - PSS, ArtistNation, RestaurantOps not shown in steps 5-10
   - Are they included in "All Segment Cubes" only?

---

## Next Steps

- [ ] Confirm this configuration with Live Nation team
- [ ] Update Model Sync diagram to reflect bidirectional flows
- [ ] Implement sync logic in `86358_Sync_Cubes.cs`
- [ ] Document dependency order for execution
- [ ] Create unit tests for each sync

---

**Last Updated:** 2025-11-10
**Document Version:** 2.0 (REVISED)
