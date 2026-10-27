# MTF integration development checkpoints
Updated: 27 October 2026
Branch: feature/mtf-integration. main remains v0.4.2, commit 803d245f14bba78673c0235e526bac41a3958020.

## Authority and initialization
D-006: newest confirmed pair becomes current immediately, not at next point3.
User deferred start-independence and chose a fixed manual reference for testing. MTF preparation resumed; this supersedes the pause in MTF_Implementation.md, not the unresolved initialization sensitivity.
D1 baseline: OANDA:XAUUSD, downward, selected August25 2026 candle. Input previously displayed02:30; chart later showed UTC+05:30, but preserve saved input rather than silently converting it. Other timeframe contexts still need explicit valid candle-open/direction settings; do not shift missing starts or infer direction.
D1 screenshot baseline: 2/2 pairs, high4194.645/low4111.330, zeroCHOCH, 45 stored candles, currentTJL set2 available2026-10-14 17:00 America/New_York. This is displayed-state evidence, not full strategy verification.

## Checkpoint1: v0.4.3-dev, commit c4ac97d3fe19de373d3999122ba3dffb715f48ee
Extracted explicit-input CHOCH counting and structure restart. User reported compilation and supplied matching D1 screenshot at15:50IST. That run had noCHOCH; confirmation markers were notCHOCH events.
At19:57IST user supplied a separate D1 bullishCHOCH audit: broken4180.345; Aug5 close4247.275; Aug6 close4240.750; confirmedAug6 17:00 America/New_York. Point3 identifiedJul21, origin/restartJul15 17:00, price3976.580. Reported state5/0 pairs,2CHOCH,lastbullish,no currentTJL is internally consistent with no completed new-direction pair.
Evidence status: reported audit numerical/timing consistency checked. Candle/source-point selection, first post-restart pullback, old/new version equivalence, failed/equality cases and replay/reload are not independently verified. The separate audit does not replace the fixed Aug25 test reference.

## Checkpoint2: v0.4.4-dev
Extracted ISSState with owned internal Structure and internal point arrays. beginSearch captures fixed boundaries and gate; cancelSearch terminates unfinished search/tracking; advance uses explicit candle/history inputs and returns internal/rejection/confirmation/tracking-only outcomes. Calculation methods do not read chart OHLC/time, barstate, drawings, view inputs or global histories.
Preserve first internal pair after the main-pullback identification candle; original point3 reference; inclusive marked-price boundaries; reject/stop on out-of-range marked points; fifth tentative identification timing; completion-close test; permanent eligibility loss but continued tracking on outside completion; oneISS per candidate; main-continuation/CHOCH cancellation before internal stepping; zones from internalP3/P4; retained confirmed history.
Main MSM/CHOCH, D-006 TJL rules, chart scopes and default start/direction inputs are unchanged. Existing saved test settings must be retained.
Still chart-only: storage, TJL lifecycle, immutable transport snapshots and orchestration remain to extract before MTF requests. No taps, usage or Buy/Sell signals. Main is untouched.

## Verification for v0.4.4-dev: NOT RUN
1. Compile in TradingView. Repeat D1 fixed-reference baseline and separate CHOCH audit; compare same historical coverage.
2. H1 with identical previously tested start/direction/history: compare ISS count/side/confirmation time, all five prices/origins, fixed boundaries, completion close and internalP3/P4 zones.
3. Check out-of-bounds rejection, outside-completion tracking-only, cancellation, fresh search and no revival under same candidate. Do not treat zero occurrences as a tested success path.
4. Compare Off/Latest/History underlying results; reload/replay and check drawings do not alter calculation state.
A compile or one valid example is not full module sign-off.

## Next
Complete context-state/event extraction, prove native equivalence, then D1/H4/H1/M5/M1 transport with source and delivery timestamps kept distinct. MappingD1->H1,H4->M5,H1->M1. Verify integrated/native equivalence before taps/entry selection. Merge only after final testing and explicit user approval.
