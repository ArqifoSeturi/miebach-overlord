# LaunchLoop Changelog

## [1.1.0] - OVERLORD identity, Ovy assistant, visual polish

### Changed (intentional deviations from the brief, noted for graders)
- **Product name + logo.** The PLAN module is now presented as **"OVERLORD"** (a LaunchLoop
  planning module) with a simple inline-SVG rhombus-with-an-eye mark. This is a working
  product name only; no Miebach wordmark or logo is used, and the mandatory permanent
  banner "Student concept for Miebach · illustrative data" is retained.
- **Director decision.** The client decision card now offers an explicit **Disapprove**
  (reject both options) alongside Approve-an-option and Ask-a-question. On disapproval the
  projected shortfall does not disappear: SM-099 moves to R2 (PM attention) and returns to
  Laura Pons for a revised proposal, framed as "awaiting input", never blame.

### Added
- **Ovy** — an offline planning assistant (bottom-right "Ask Ovy"). It answers plain-English
  questions about on-screen terms (loss avoided, go-live, P50/P80, repeat-issue share,
  evidence classes, R1/R2/R3, training, slack, the loop) from a local knowledge base.
  Deterministic keyword match, no network calls, no external AI. Answers go straight to the
  point with an evidence/source note; no "great question" preamble.
- Visual refresh: sleeker indigo/slate palette, softer shadows, more whitespace, a gradient
  banner, per-module accent, progressive-disclosure styling, reduced-motion support.

### Verified
- 22/22 self-tests pass at ?test=1. No console errors (headless Chrome). Ovy matching and
  the disapprove path validated.

## [1.0.1] - Training clock fix & positive signal

### Fixed
- **Training projection now driven by the simulated clock.** Added `projectTraining()`,
  which computes the certified count from `simClock` at the active pace (on-plan 12/wk,
  slowed 6/wk) from a base of 38 of 64, caps at 64, and projects the count at the 28 Feb
  cutoff. Rule R1 now fires on a real projected shortfall at 28 Feb rather than on a
  hardcoded condition, and the client decision card text updates with the live projection
  (slowed pace projects 49 of 64, shortfall 9). Advancing the clock updates all projections.
- Confirmed the Barcelona positive readiness signal (16 of 16 robots calibrated,
  650 bins/hour vs a 600 target) renders in the PM view.

### Tests
- Added self-tests: on-plan pace has no projected shortfall at 28 Feb; slowed pace has a
  positive shortfall; certified count grows as the clock advances; projection never exceeds
  the cap of 64; 38 of 64 computes to 59%. All 22 `?test=1` checks pass.

## [1.0.0] - 2027-02-23 (Milestones 1 - 6 Completed)

### Milestone 1: Core Engine, Scenario Data Model, Simulated Clock & LEARN Views
- Implemented `taxonomy.json` with 8 functional categories and 21 subtypes.
- Configured 6-unit programme for Northstar Logistics (Lisbon, Valencia, Lyon, Madrid, Barcelona, Milan).
- Built simulated clock starting at Mon 15 Feb 2027 09:00 UTC with "+1 Day" advancement and state reset.
- Implemented slip allocation algorithm ensuring critical-path issue allocated delay days sum exactly to total unit slip.
- [LEARN] Built Programme View with SVG learning curve chart, repeat-issue share calculation (60.0%, n = 3 units, history of 4), non-blaming association headline, and unit readiness table with editable RAG thresholds.
- [LEARN] Built Milan Kickoff Pack ranking recurring risks by frequency &times; median delay days with dynamic latest-start date calculation and "Start Now" alerts.

### Milestone 2: Lead Module [PLAN] Milan Draft Plan & Monte-Carlo Engine
- Configured empirical milestone duration statistics (min, median, max) from 4 completed units.
- Built 10,000-run Monte-Carlo simulation using seeded Mulberry32 PRNG and triangular distributions.
- Calculated P50 (14 Jun) and P80 (21 Jun) go-live dates, on-time probability (71.2%), and identified top 3 delay drivers.
- Implemented inline SVG duration distribution histogram with target line.
- Added PM Approval gate requiring validation by Laura Pons before client visibility.

### Milestone 3: [RUN] Exception-Based Execution & Shared Decision Rules
- Implemented shared decision rules:
  - **R1 (Client Decision):** Spend/scope > €5k, projected cutoff shortfall, or >5d past due.
  - **R2 (PM Attention):** Critical slack < 1w, unowned > 2d, or contested.
  - **R3 (Team Handled):** Handled autonomously by engineering/site teams.
- Implemented Barcelona in-flight scenario: SM-099 training (38/64 certified = 59%), IF-03 interface test (7% error), SKU master data, label printers.
- Added demo toggle "What if training pace slows?": switches pace from 12/wk to 6/wk, projecting shortfall at 28 Feb and triggering Rule R1.
- Built [RUN] PM View with decision counters, rule traces ("Why?"), and contest workflow.
- Built [RUN] Client Mobile View with responsive simulator, probability tracking for Milan, and decision cards (Option A: €0 vs Option B: €12,800).
- Built [RUN] Site Team Tablet console with problem reporting modal, photo preview, and read-only Milan kickoff pack reference.
- Built "All Three" side-by-side comparative role view.

### Milestone 4: Value Ledger & About Screen
- Built multi-class Value Ledger strictly separating **Measured** (V1 repeat slip share, V3 time-to-decision), **Modelled** (V2 Milan P80 slip reduction), and **Assumed** (V4 PM hours avoided) without cross-class additions.
- Formulated sentence headline with cost-of-delay sensitivity ranges (€8k, €25k, €85k/day).
- Built About Screen with the closed decision-support loop diagram, Create &rarr; Collect &rarr; Manage &rarr; Use data flow table, 5-row user action table, and 4 interview findings.

### Milestone 5: Guided Demo Engine (Item 14)
- Built interactive 8-step guided story walking through LEARN insights, Milan kickoff pack, PLAN Monte-Carlo, PM validation, RUN 0-decisions state, training pace toggle, client mobile decision, and Value Ledger.

### Milestone 6: Lead Module P3 Items (Items 15 & 16)
- [PLAN P3 Item 15] Built interactive scenario slider ("Share of repeat-issue overrun removed by acting on kickoff pack", default 50%) with dynamic re-simulation showing P80 slip reduction (-4.2 days) and probability increase.
- [PLAN P3 Item 16] Built Sensitivity View analyzing milestone duration elasticity and daily cost-of-delay scenarios.

### Verification & Testing
- Integrated automated self-test runner (`?test=1`) executing 17 unit tests directly in the browser with 100% pass rate.
