# MTF branch storage regression and recovery candidate
Date: 28 October 2026
Branch: feature/mtf-integration. Do not modify or merge main.
This record supersedes the NOT RUN status for v0.4.5-dev in MTF_Checkpoints.md: that checkpoint FAILED the user's D1 and H1 regression test on 27 October at22:26IST.

## Observed failure
Both supplied dashboards report zero stored candles, zero pairs, missing confirmed/tentative prices and no TJLs, but also main point3 identification and waiting-for-continuation state. H1 reports no confirmed main level2. This is not a successful no-signal result and is not a reason to move the user's reference.
v0.4.4-dev remains the last example-matched fallback (commit d879efbd499fe111d24bb4b73bf3697a6a0db12e). It is not blanket strategy sign-off.

## Investigation and uncertainty
v0.4.5 introduced CandleStore-owned arrays, var aliases into object fields and method-based appending simultaneously. The append method's returned index drove MSM, while rendering/counter/search consumers read aliases. The user's symptoms are consistent with consumers not seeing the appended history. Exact runtime mechanism/root cause has NOT been reproduced or proved. Do not claim Pine UDT arrays generally fail or that aliasing is conclusively the cause.
A second analogous ownership pattern existed in the TJL records array. Avoid both field-to-var alias declarations in the candidate.

## v0.4.5.1-dev changes
Restore directly constructed persistent tops/bots/wh/wl/ts arrays and chart-level array.push calls from the v0.4.4 storage path. Defer CandleStore abstraction. Preserve original confirmed-candle,selected-start and capacity gates.
Keep TJLState lifecycle and captured scalar origins/bounds; synchronize now receives the five source arrays explicitly. Construct retained TJL records array directly before passing it to TJLState. Calculations and rendering consume the same directly named collection.
Add runtime checks for exactly one timestamp append per processed candle, equal OHLC/timestamp array lengths and matching retained-record array sizes. Stop explicitly if these checks fail instead of continuing with inconsistent storage.
No changes to MSM/CHOCH/ISS detection, D-006 availability, source scopes,default reference inputs,taps or entry rules. Still chart-only, no MTF requests and no Buy/Sell signals. Main remains unchanged.

## Verification status
Pine compilation: NOT RUN here. TradingView runtime/replay/reload: NOT RUN. This is an isolation/recovery candidate, not a verified fix. No automated test result is claimed.
User should preserve saved references and compare candidate versus v0.4.4 over identical loaded historical coverage. Because a new day has elapsed, later bar counts/events may legitimately differ; use same replay endpoint or side-by-side identical coverage.
D1 baseline snapshot: downward Aug25 reference,set2,high4194.645/low4111.330,availableOct14 17:00 America/New_York. H1 snapshot: Aug25 00:30 input,Downward,set32 availableOct22 00:00;32/3pairs,26CHOCH,2ISS,lastbullishOct26 15:00,5rejected/0outside/51cancelled,1052candles at that earlier endpoint.
Retest compilation,nonzero stored candles,confirmed prices,TJL source origins/bounds/availability and the H1 ISS audit; report any storage-invariant error verbatim. Do not resume MTF additions until recovery comparison passes. Failed/equality/tracking-only/history/replay cases from earlier checkpoints remain open.
