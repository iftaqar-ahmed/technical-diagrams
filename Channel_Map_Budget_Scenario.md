# Channel Assignment Map - Budget/Forecast Scenario
## Live Nation Cube-to-Cube Data Flows

**Purpose:** Dedicated routing channels to ensure ZERO overlapping arrows
**Complexity:** Higher than Actual (bidirectional + lateral flows)

---

## Cube Positions (5-Tier Layout - Added Registers)

### Tier 1 - Global Reporting (Top)
- **Global Cube:** x=1700, y=200, w=300, h=120

### Tier 2 - Concerts Reporting (Mid-Upper)
- **Concerts Cube:** x=1700, y=550, w=300, h=120

### Tier 3 - Specialty Cubes (Mid, Horizontal Row)
- **People Cube:** x=200, y=950, w=220, h=100
- **Capex Cube:** x=550, y=950, w=220, h=100
- **Fixed Expense Cube:** x=900, y=950, w=220, h=100
- **Sponsorship Cube:** x=2000, y=950, w=220, h=100
- **Ticketing Cube:** x=2400, y=950, w=220, h=100
- **Rollforward Cube:** x=2800, y=950, w=220, h=100

### Tier 4 - Segment Cubes (Lower, Horizontal Row)
- **Event Cube:** x=200, y=1400, w=240, h=100
- **Festival Cube:** x=600, y=1400, w=240, h=100
- **PSS Cube:** x=1000, y=1400, w=240, h=100
- **Restaurant Ops Cube:** x=1400, y=1400, w=240, h=100
- **Artist Nation Cube:** x=1800, y=1400, w=240, h=100

### Tier 5 - Registers (Bottom, Horizontal Row)
- **Named Event Register:** x=200, y=1700, w=280, h=80
- **PSS Register:** x=1000, y=1700, w=220, h=80

---

## Channel Assignments (X Coordinates)

### DOWNWARD ZONE 1: Fixed Expense → Segments
**Channels:** x=920-1320 (6 flows, 80px spacing)

| Flow | Source | Target | Channel X | Color | Label |
|------|--------|--------|-----------|-------|-------|
| 1 | Fixed Expense | Event | 920 | #C00000 | Fixed/OIE P&L |
| 2 | Fixed Expense | Festival | 1000 | #C00000 | Fixed/OIE P&L |
| 3 | Fixed Expense | PSS | 1080 | #C00000 | Fixed/OIE P&L |
| 4 | Fixed Expense | Restaurant Ops | 1160 | #C00000 | Fixed/OIE P&L |
| 5 | Fixed Expense | Artist Nation | 1240 | #C00000 | Fixed/OIE P&L |
| 6 | Fixed Expense | Ticketing (far right) | 1320 | #C00000 | Fixed/OIE P&L |

### DOWNWARD ZONE 2: People → Segments
**Channels:** x=220-620 (6 flows, 80px spacing)

| Flow | Source | Target | Channel X | Color | Label |
|------|--------|--------|-----------|-------|-------|
| 7 | People | Event | 220 | #4472C4 | Labor + Headcount |
| 8 | People | Festival | 300 | #4472C4 | Labor + Headcount |
| 9 | People | PSS | 380 | #4472C4 | Labor + Headcount |
| 10 | People | Restaurant Ops | 460 | #4472C4 | Labor + Headcount |
| 11 | People | Artist Nation | 540 | #4472C4 | Labor + Headcount |
| 12 | People | Ticketing (far right) | 620 | #4472C4 | Labor + Headcount |

### DOWNWARD ZONE 3: Capex → Segments
**Channels:** x=570-970 (6 flows, 80px spacing)

| Flow | Source | Target | Channel X | Color | Label |
|------|--------|--------|-----------|-------|-------|
| 13 | Capex | Event | 570 | #ED7D31 | Capex Metrics |
| 14 | Capex | Festival | 650 | #ED7D31 | Capex Metrics |
| 15 | Capex | PSS | 730 | #ED7D31 | Capex Metrics |
| 16 | Capex | Restaurant Ops | 810 | #ED7D31 | Capex Metrics |
| 17 | Capex | Artist Nation | 890 | #ED7D31 | Capex Metrics |
| 18 | Capex | Ticketing (far right) | 970 | #ED7D31 | Capex Metrics |

### UPWARD ZONE 1: Registers → Segments
**Channels:** x=260, 1040 (2 flows, direct vertical)

| Flow | Source | Target | Channel X | Color | Label |
|------|--------|--------|-----------|-------|-------|
| 19 | Named Event Register | Event | 260 (center) | #FFC000 | Named P&L |
| 20 | PSS Register | PSS | 1040 (center) | #7030A0 | P&L |

### UPWARD ZONE 2: Segments → Concerts
**Channels:** x=1550-1750 (4 flows, 50px spacing)

| Flow | Source | Target | Channel X | Color | Label |
|------|--------|--------|-----------|-------|-------|
| 21 | Event | Concerts | 1550 | #FFC000 | Show P&L |
| 22 | Festival | Concerts | 1620 | #70AD47 | Show P&L |
| 23 | PSS | Concerts | 1690 | #7030A0 | P&L + Metrics |
| 24 | Restaurant Ops | Concerts | 1760 | #A5A5A5 | P&L + Metrics |

### UPWARD ZONE 3: Segments → Global
**Channels:** x=1800-2000 (5 flows, 50px spacing)

| Flow | Source | Target | Channel X | Color | Label |
|------|--------|--------|-----------|-------|-------|
| 25 | Event | Global | 1800 | #FFC000 | Show P&L |
| 26 | Festival | Global | 1850 | #70AD47 | Show P&L |
| 27 | PSS | Global | 1900 | #7030A0 | P&L + Metrics |
| 28 | Restaurant Ops | Global | 1950 | #A5A5A5 | P&L + Metrics |
| 29 | Artist Nation | Global | 2000 | #C00000 | P&L |

### UPWARD ZONE 4: Concerts & Specialty → Global
**Channels:** x=1850 (center), 2200-2400 (3 flows)

| Flow | Source | Target | Channel X | Color | Label |
|------|--------|--------|-----------|-------|-------|
| 30 | Concerts | Global | 1850 (center) | #E26B0A | P&L + Corporate |
| 31 | Capex | Global | 2200 | #ED7D31 | Capex Metrics |
| 32 | Sponsorship | Global | 2300 | #9673A6 | P&L |
| 33 | Ticketing | Global | 2400 | #44546A | P&L + Ticketing |

### LATERAL ZONE: Specialty ↔ Specialty (Exterior Edges)
**Channels:** x=100-150 (left edge), x=3100-3200 (right edge), y=950-1050

| Flow | Source | Target | Route | Color | Label |
|------|--------|--------|-------|-------|-------|
| 34 | Festival | Sponsorship | Left edge x=100, y=1000 | #70AD47 | P&L (Festival ID) |
| 35 | Sponsorship | Festival | Left edge x=150, y=1020 | #9673A6 | P&L |
| 36 | Sponsorship | Ticketing | Horizontal y=950 | #9673A6 | P&L |
| 37 | Ticketing | Sponsorship | Horizontal y=1000 | #44546A | P&L |
| 38 | Capex | Rollforward | Horizontal y=950 | #ED7D31 | Capex Metrics |
| 39 | Rollforward | Ticketing | Right edge x=3100, y=1000 | #5B9BD5 | Advance Metrics |

---

## Flow Summary

**Total Flows:** 39 cube-to-cube connections
**Downward:** 18 flows (Fixed Expense/People/Capex → Segments)
**Upward (Registers → Segments):** 2 flows
**Upward (Segments → Concerts):** 4 flows
**Upward (Segments → Global):** 5 flows
**Upward (Concerts/Specialty → Global):** 4 flows
**Lateral:** 6 flows (Specialty ↔ Specialty)

**Channel Spacing:** 50-80px ensures NO OVERLAPS
**Canvas Size:** 3400x1900 (wider for lateral flows)

---

## Color Legend

| Cube | Color Code | Hex |
|------|------------|-----|
| Fixed Expense | Red | #C00000 |
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

## Special Routing Strategy

### Bidirectional Flows
**Sponsorship ↔ Festival:** Use canvas left edge (x=100-150)
**Sponsorship ↔ Ticketing:** Use horizontal mid-tier routing (y=950, y=1000)
**Capex → Rollforward → Ticketing:** Chain routing using right side

### Complex Convergence
**Global Cube receives from 9 sources:**
- Use RIGHT channels (x=2200-2400) for specialty cubes
- Use CENTER channels (x=1800-2000) for segment cubes
- Stagger arrival heights to prevent congestion

### Wide Canvas Strategy
- **Extra 200px width** (3400 vs 3200) accommodates exterior routing
- **Lateral flows** stay on canvas edges (x=100, x=3100)
- **Vertical flows** use interior channels (x=200-2400)

---

## Waypoint Strategy

**All flows use 3-7 waypoints (more complex than Actual):**

**Downward flows:**
1. Exit source cube (horizontal 50-100px out)
2. Enter dedicated channel (turn vertical)
3. Travel down channel (vertical to target tier)
4. Exit channel (turn horizontal)
5. Enter target cube (horizontal final approach)

**Upward flows:**
1. Exit source cube (horizontal 50-100px out)
2. Enter dedicated channel (turn vertical)
3. Travel up channel (vertical to target tier)
4. Exit channel (turn horizontal)
5. Enter target cube (horizontal final approach)

**Lateral flows:**
1. Exit source cube (horizontal to edge)
2. Travel on edge (vertical or horizontal)
3. Turn toward target (change direction)
4. Horizontal approach to target
5. Enter target cube

**Result:** Clear, traceable paths with ZERO overlaps even with bidirectional flows
