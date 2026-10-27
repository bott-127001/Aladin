# MTF integration development checkpoints
Updated: 27 October 2026
Branch: feature/mtf-integration. main remains v0.4.2, commit 803d245f14bba78673c0235e526bac41a3958020.

## Authority and initialization
D-006: newest confirmed pair becomes current immediately, not at next point3.
User deferred start-independence and chose a fixed manual reference for testing. MTF preparation resumed; this supersedes the pause in MTF_Implementation.md, not unresolved initialization sensitivity.
D1 baseline: OANDA:XAUUSD, downward, selected August25 2026 candle. Preserve saved selection. Chart later displayed UTC+05:30. Other contexts require explicit valid candle-open/direction settings; do not shift missing starts or infer direction.
D1 screenshot: 2/2 pairs, high4194.645/low4111.330, zeroCHOCH,45 stored candles,currentTJL set2 available2026-10-14 17:00 America/New_York. Evidence of displayed state, not full strategy verification.

## Checkpoint1: v0.4.3-dev, c4ac97d3fe19de373d3999122ba3dffb715f48ee
Extracted explicit-input CHOCH counting/restart. User reported compilation and matching D1 screenshot15:50IST. That run had noCHOCH; structure-confirmation markers were notCHOCH events.
Separate user audit19:57IST: bullishD1 CHOCH, broken4180.345; Aug5 close4247.275; Aug6 close4240.750; confirmedAug6 17:00 America/New_York. Point3 identifiedJul21,origin/restartJul15 17:00,price3976.580. State5/0 pairs,2CHOCH,lastbullish,no currentTJL internally consistent with no new-direction completed pair.
Only reported audit numerical/timing consistency checked. Actual candle/source selection, post-restart pullback, failed/equality cases and replay/reload not independently verified. Separate audit does not replace fixed Aug25 reference.

## Checkpoint2: v0.4.4-dev, d879efbd499fe111d24bb4b73bf3697a6a0db12e
Extracted ISSState: owned internal engine/point arrays; beginSearch captures boundaries/gate; cancelSearch ends unfinished search/tracking; advance takes explicit candles/histories and returns tentative/pair/rejection/ISS/tracking-only outcomes. No chart OHLC/time, barstate, drawings, view inputs or global-history reads inside methods.
Preserved inclusive marked-price boundaries, fifth-tentative identification timing, completion close, rejection versus tracking-only distinction, permanent eligibility loss, oneISS/candidate, main continuation/CHOCH cancellation before internal step, internalP3/P4 zone sources and confirmed history.
User H1 comparison20:41-20:54IST: Aug25 00:30 reference,Downward,OANDA:XAUUSD. v0.4.3 and v0.4.4 supplied tables match32/3 pairs,high4141.015/low4112.350,tentative3985.485,26CHOCH,lastbearishOct19 09:00;2ISS,lastbullishOct26 15:00;5rejected/0outside/51cancelled;1052candles.
Candidate58: bornOct26 00:00,fixed3985.485-4141.015,P1 4015.925@Oct26 00:00,P2 4006.400@03:00,P3 4017.790@06:00,P4 3993.170@10:00,P5tentative4023.240@10:00,completion4012.685@15:00. All reported origins/prices match across versions. Audit times America/New_York; reference input display time is retained as supplied, not silently converted.
User resolved date discrepancy20:54IST: TJL32 confirmed/availableOct22 00:00,notOct27; high sourceOct21 16:00,low sourceOct21 03:00; laterpoint3originOct25 20:00. This is consistent with Oct26 ISS. Evidence supports example parity, not complete regression acceptance. Outside-completion count0 is not a tested tracking-only case.

## Checkpoint3: v0.4.5-dev
Extract CandleStore with explicit OHLC/open-time append and existing chart-level capacity/start/confirmed-candle guards. Chart drawing helpers retain aliases into that store; no assumption of MTF isolation is made yet.
Extract TJLState.synchronize with explicit Structure,CandleStore,event counters,index,source-close timestamp and history cap. Own current/newest pointers, seen counters, set count and retained records. Preserve sourceCHOCH invalidation, pair-confirmation replacement, optional point3 metadata only and Monday no-op. No rendering/view switches inside lifecycle method.
Capture original high/low candle timestamps and body-to-wick zone bounds at pair confirmation. TJL drawing/audit now use those captured scalar fields rather than looking up candle indices. Lifecycle status/timestamps remain mutable; this is NOT an immutable transport-event implementation.
Keep default start/direction, detection logic, chart scopes, zone colours and Latest/History/Off behaviour unchanged. keepTJL remains bounded record history, not a used-level registry; future usage identity must survive drawing/history pruning.
Still chart-only. Full per-timeframe context orchestration,event snapshots,source/delivery timing and requests remain. No taps,used-level tracking or entries. main untouched.

## Verification v0.4.5-dev: NOT RUN
Compile in TradingView. Preserve saved settings and compare against v0.4.4 for identical historical coverage:
- D1 reference: set2,high4194.645/low4111.330,availableOct14 17:00.
- H1 reference Aug25 00:30 Downward: set32 availableOct22 00:00;source originsOct21 03:00/16:00; captured zone bounds must match old drawings/tooltips; ISS2/bullishOct26 15:00 and same counters/points for same coverage.
- Actual sourceCHOCH: no old-direction current set until a replacement confirmed pair forms; old historical records end at sourceCHOCH.
- Latest versus History versus Off: underlying state equal. Historical endpoints,zero-width zones,omitted count and reload/replay must be checked.
- Remaining ISS failed/boundary/tracking-only and CHOCH equality/post-restart tests remain open. No full sign-off from compilation or one example.

## Next
Finish context-state orchestration and immutable event snapshots; native equivalence before D1/H4/H1/M5/M1 transport. MappingD1->H1,H4->M5,H1->M1. Preserve actual source versus delivery timestamps and compare requested/native events before activation/entries. Merge only after final testing and explicit approval.
