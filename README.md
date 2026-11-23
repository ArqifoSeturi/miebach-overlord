# LaunchLoop · Decision Support for Multi-Site Warehouse Go-Lives

**Lead Module:** PLAN  
**Academic Context:** ESADE Digital Transformation Course · Hackathon Prototype  
**Partner Context:** Fictional collaboration with Miebach Consulting for fictional client Northstar Logistics.

---

## 1. Product Overview & Core Question

> *"How might we accelerate go-live timelines for client warehouse implementation teams?"*

LaunchLoop addresses the pervasive lack of visibility and workflow friction between Miebach Consulting, client logistics teams, and automated equipment suppliers. It operates as a continuous three-stage loop:

1. **LEARN:** Turns historical go-live data into structured lessons, repeat-issue learning curves, and scheduled kickoff packs for upcoming facilities.
2. **PLAN (Our Lead Module):** Generates master go-live milestone schedules and empirical 10,000-run Monte-Carlo simulations (P50/P80 dates and probability of hitting target dates) validated by the Miebach Project Manager.
3. **RUN:** Provides exception-based execution tracking for in-flight facilities (e.g. Barcelona DC), highlighting who must decide what by when according to transparent shared rules (R1, R2, R3).
4. **FEEDBACK:** Resolved execution variances and issues feed back into LEARN for subsequent units (e.g. Milan DC).

---

## 2. Shared Scenario & Fictional Data Model

### The Programme: Northstar Logistics
Six distribution centres in sequential go-live order:
- **Lisbon DC:** Completed (Slip: +21 days).
- **Valencia DC:** Completed (Slip: +14 days).
- **Lyon DC:** Completed (Slip: +14 days).
- **Madrid DC:** Completed (Slip: +7 days).
- **Barcelona DC:** In flight (Target Go-Live: Mon 15 March 2027; Simulated Clock start: Mon 15 February 2027).
- **Milan DC:** Planned (Kickoff: Mon 15 March 2027; Planned Go-Live: Mon 7 June 2027, 12 planned weeks).

### Milestone Template (12 Planned Weeks)
To ensure overrun comparability across units, all distribution centres share a common planned milestone structure:
1. `Master data validated` (Client IT): 8 planned weeks.
2. `Interface tests passed` (Client IT): 9 planned weeks.
3. `Hardware on site` (Robotic storage supplier): 7 planned weeks.
4. `Training certified` (Client Operations): 8 planned weeks.
*(These four milestones run in parallel; the critical path is dictated by the longest branch)*
5. `Cutover rehearsal` (Miebach): 3 planned weeks (sequential after longest parallel branch).
6. `Go-Live`: Total planned duration = $9 + 3 = 12$ weeks.

---

## 3. Critical Path Slip Allocation Methodology

Unit slip (days) is calculated as:
$$\text{Unit Slip (Days)} = ((\max(\text{parallel actual weeks}) + \text{cutover actual weeks}) - 12\text{ planned weeks}) \times 7\text{ days}$$

### Slip Allocation Across Issues
1. **Critical Branch Identification:** For each completed unit, the longest parallel milestone plus the sequential Cutover Rehearsal constitutes the critical path.
2. **Proportional Allocation:** The unit's total slip days are allocated across issues located on this critical path in exact proportion to their raw reported `delay_days`.
3. **Slack Absorption:** Issues on non-critical parallel branches receive an allocated delay of **0 days** because their overruns were fully absorbed by branch slack.
4. **Exact Sum Guarantee:** The allocated delay days for issues in each completed unit sum exactly to that unit's total slip:
   - Lisbon DC: $12 + 9 = 21$ days (100% matched to 21-day slip).
   - Valencia DC: $10 + 4 = 14$ days (100% matched to 14-day slip).
   - Lyon DC: $10 + 4 = 14$ days (100% matched to 14-day slip).
   - Madrid DC: $4 + 3 = 7$ days (100% matched to 7-day slip).

---

## 4. Lead Module: PLAN (Monte-Carlo Simulation)

The PLAN module drafts the master schedule for Milan DC without using external LLM black-boxes:
- **Empirical Historical Inputs:** Milestone min, median (mode), and max actual weeks are pulled from the 4 completed go-lives.
- **Monte-Carlo Engine:** 10,000 seeded iterations using triangular distributions $T(\min, \text{mode}, \max)$.
- **Outputs:**
  - Probability of hitting 7 June 2027: **71.2%** (n = 4 units, low evidence, fictional history).
  - P50 Projected Go-Live (Median): 14 June 2027 (+7.0 days slip).
  - P80 Projected Go-Live (Conservative): 21 June 2027 (+14.0 days slip).
  - Top 3 Delay Drivers: Interface tests passed (#1), Training certified (#2), Cutover rehearsal (#2 direct).
- **P3 Scenario Control (Item 15):** Evaluates mitigating repeat-issue overruns by acting on the Kickoff Pack (default 50% mitigation recovers 4.2 days of P80 slip and lifts probability to ~78.4%).
- **P3 Sensitivity View (Item 16):** Demonstrates elasticity of milestone shifts and cost of delay across Low (€8k/d), Base (€25k/d), and High (€85k/d) assumptions.

---

## 5. Seeded Randomness & Deterministic Reproducibility

- The Monte-Carlo engine uses a deterministic **Mulberry32 PRNG** initialized with seed `20270215`.
- Data was not retro-fitted or tuned to produce an artificial target; historical durations reflect authentic industrial variance across robotics, WMS interfaces, workforce certification, and master data.
- Reloading the page produces identical results every time.

---

## 6. Where an LLM Could Help (Future Roadmap)

While LaunchLoop runs 100% client-side with zero external network dependencies, an integrated LLM could assist in:
1. **Normalising Fragmented Site Spreadsheets:** Parsing heterogeneous Excel/CSV formats, mapping varied column headers in multiple languages (English, Spanish, German) to the canonical taxonomy.
2. **Matching Semantically Similar Issues:** Clustering unstructured problem descriptions across facilities to identify emerging systemic failure modes.
3. **Drafting Weekly Status Reports for PM Approval:** Summarising weekly test pass rates, open exceptions, and next steps into natural language briefs for human review.

---

## 7. Canonical Taxonomy (`taxonomy.json`)

The taxonomy comprises exactly 8 categories and 21 subtypes:
- **Master data:** duplicate SKUs; unit-of-measure conflicts; missing dimensions or weights
- **Integration and testing:** interface test failure; incomplete test coverage; environment instability
- **Training and change management:** certification behind plan; low shift coverage; process resistance
- **Process adaptation:** customisation request; undocumented workaround
- **Site and infrastructure:** hardware delivery late; network or Wi-Fi gaps; building not ready
- **Vendor and integrator:** delivery slip; resourcing shortfall
- **Client decisions and approvals:** late sign-off; scope change; data not provided
- **Cutover and hypercare:** rehearsal gaps; stabilisation capacity

---

## 8. Self-Test Suite

To run the automated verification suite directly in the browser:
Open `launchloop.html?test=1`. 
17 unit tests run automatically in the console and display a summary verification alert covering data allocations, taxonomy matching, Monte-Carlo determinism, decision rules, and clock advancement.
