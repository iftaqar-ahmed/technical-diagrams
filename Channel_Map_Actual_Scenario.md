# Channel Assignment Map - Actual Scenario
## Live Nation Cube-to-Cube Data Flows

**Purpose:** Dedicated routing channels to ensure ZERO overlapping arrows

---

## Cube Positions (4-Tier Layout)

### Tier 1 - Global Reporting (Top)
- **Global Cube:** x=1600, y=200, w=300, h=120

### Tier 2 - Concerts Reporting (Mid-Upper)
- **Concerts Cube:** x=1600, y=500, w=300, h=120

### Tier 3 - Specialty Cubes (Mid, Horizontal Row)
- **People Cube:** x=200, y=900, w=220, h=100
- **Capex Cube:** x=550, y=900, w=220, h=100
- **Fixed Expense Cube:** x=900, y=900, w=220, h=100
- **Sponsorship Cube:** x=1900, y=900, w=220, h=100
- **Ticketing Cube:** x=2250, y=900, w=220, h=100
- **Rollforward Cube:** x=2600, y=900, w=220, h=100

### Tier 4 - Segment Cubes (Bottom, Horizontal Row)
- **Event Cube:** x=200, y=1400, w=240, h=100
- **Festival Cube:** x=550, y=1400, w=240, h=100
- **PSS Cube:** x=900, y=1400, w=240, h=100
- **Restaurant Ops Cube:** x=1250, y=1400, w=240, h=100
- **Artist Nation Cube:** x=1600, y=1400, w=240, h=100

---

## Channel Assignments (X Coordinates)

### LEFT ZONE: People → Segments (Downward, 7 flows)
| Flow | Source | Target | Channel X | Color | Label |
|------|--------|--------|-----------|-------|-------|
| 1 | People | Event | 220 | #4472C4 | Headcount Metrics |
| 2 | People | Festival | 260 | #4472C4 | Headcount Metrics |
| 3 | People | PSS | 300 | #4472C4 | Headcount Metrics |
| 4 | People | Restaurant Ops | 340 | #4472C4 | Headcount Metrics |
| 5 | People | Artist Nation | 380 | #4472C4 | Headcount Metrics |
| 6 | People | Sponsorship | 420 (up to Tier 3) | #4472C4 | Headcount Metrics |
| 7 | People | Ticketing | 460 (up to Tier 3) | #4472C4 | Headcount Metrics |

**Spacing:** 40px between channels (prevents overlap)

### LEFT-CENTER ZONE: Capex → Segments (Downward, 7 flows)
| Flow | Source | Target | Channel X | Color | Label |
|------|--------|--------|-----------|-------|-------|
| 8 | Capex | Event | 570 | #ED7D31 | Capex Metrics |
| 9 | Capex | Festival | 610 | #ED7D31 | Capex Metrics |
| 10 | Capex | PSS | 650 | #ED7D31 | Capex Metrics |
| 11 | Capex | Restaurant Ops | 690 | #ED7D31 | Capex Metrics |
| 12 | Capex | Artist Nation | 730 | #ED7D31 | Capex Metrics |
| 13 | Capex | Sponsorship | 770 (up to Tier 3) | #ED7D31 | Capex Metrics |
| 14 | Capex | Ticketing | 810 (up to Tier 3) | #ED7D31 | Capex Metrics |

**Spacing:** 40px between channels

### CENTER ZONE: Segments → Concerts (Upward, 4 flows)
| Flow | Source | Target | Channel X | Color | Label |
|------|--------|--------|-----------|-------|-------|
| 15 | Event | Concerts | 1500 | #FFC000 | Show P&L + Metrics |
| 16 | Festival | Concerts | 1550 | #70AD47 | Show P&L + Metrics |
| 17 | PSS | Concerts | 1600 | #7030A0 | P&L + Metrics |
| 18 | Restaurant Ops | Concerts | 1650 | #A5A5A5 | Ops Metrics |

**Spacing:** 50px between channels (wider for visibility)

### RIGHT-CENTER ZONE: Segments → Global (Upward, 5 flows)
| Flow | Source | Target | Channel X | Color | Label |
|------|--------|--------|-----------|-------|-------|
| 19 | Event | Global | 1700 | #FFC000 | Show P&L + Metrics |
| 20 | Festival | Global | 1750 | #70AD47 | Show P&L + Metrics |
| 21 | PSS | Global | 1800 | #7030A0 | P&L + Metrics |
| 22 | Artist Nation | Global | 1850 | #C00000 | P&L |

### RIGHT ZONE: Concerts & Specialty → Global (Upward, 5 flows)
| Flow | Source | Target | Channel X | Color | Label |
|------|--------|--------|-----------|-------|-------|
| 23 | Concerts | Global | 1750 (center) | #E26B0A | Corporate Metrics |
| 24 | Capex | Global | 2100 | #ED7D31 | Capex Metrics |
| 25 | Sponsorship | Global | 2150 | #9673A6 | P&L |
| 26 | Ticketing | Global | 2200 | #44546A | Ticketing Metrics |
| 27 | Rollforward | Global | 2250 | #5B9BD5 | Cash Flow Impact |

### LATERAL ZONE: Specialty ↔ Specialty (Horizontal, 1 flow)
| Flow | Source | Target | Channel Y | Color | Label |
|------|--------|--------|-----------|-------|-------|
| 28 | Capex | Rollforward | 950 (mid-tier 3) | #ED7D31 | Capex Metrics |

---

## Flow Summary

**Total Flows:** 28 cube-to-cube connections
**Downward:** 14 flows (People/Capex → Segments)
**Upward:** 13 flows (Segments/Concerts/Specialty → Global)
**Lateral:** 1 flow (Capex → Rollforward)

**Channel Width:** 40-50px spacing ensures NO OVERLAPS
**Canvas Size:** 3200x1800 (adequate for all channels)

---

## Color Legend

| Cube | Color Code | Hex |
|------|------------|-----|
| People | Blue | #4472C4 |
| Capex | Orange | #ED7D31 |
| Event | Yellow | #FFC000 |
| Festival | Green | #70AD47 |
| PSS | Purple | #7030A0 |
| Restaurant Ops | Gray | #A5A5A5 |
| Artist Nation | Red | #C00000 |
| Concerts | Dark Orange | #E26B0A |
| Sponsorship | Violet | #9673A6 |
| Ticketing | Dark Gray | #44546A |
| Rollforward | Light Blue | #5B9BD5 |

---

## Waypoint Strategy

**All flows use 3-5 waypoints:**
1. **Exit source cube** (horizontal 50-100px out)
2. **Enter dedicated channel** (turn vertical)
3. **Travel on channel** (vertical path to target tier)
4. **Exit channel** (turn horizontal)
5. **Enter target cube** (horizontal final approach)

**Result:** Clear, traceable paths with ZERO overlaps
