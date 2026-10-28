# MTF branch storage regression and recovery candidates
Updated: 28 October 2026
Branch: feature/mtf-integration. Do not modify or merge main.
This record supersedes obsolete NOT RUN statuses for failed candidates in MTF_Checkpoints.md.

## v0.4.5-dev: FAILED user regression
27 October22:26IST: both D1/H1 dashboards report zero stored candles/pairs, missing prices and noTJLs despite main point3 identification/waiting state. H1 reports no confirmed mainlevel2. Not a valid no-signal result; user must not change reference to work around it.
v0.4.4-dev (d879efbd499fe111d24bb4b73bf3697a6a0db12e) remains last example-matched fallback, not blanket strategy sign-off.
CandleStore-owned arrays,field aliases and method append were introduced together; mismatch symptoms point to that wiring but precise Pine runtime mechanism has not been proved. Do not claim UDT arrays generally fail.

## v0.4.5.1-dev: FAILED user runtime test
Commit61153ec7212f55b76482038669942574c89bc0e3 restored direct persistent candle arrays/chart pushes,passed source arrays explicitly to TJL synchronization and added candle-length and TJL array-size checks.
User28October20:51IST: Error on bar14820: TopG TJL record-storage invariant failed; stop regression test; #main():585. Changing direction also left no levels/tables. This confirms failure of the compared TJL collection sizes in that run; it does not independently establish complete candle-storage recovery. Missing final tables are compatible with execution halting.
Do not hide the problem by merely deleting the size check. No further reference hunting required.

## v0.4.5.2-dev: proposed single-collection candidate
Remove records field entirely from TJLState. Construct only the directly named tjlRecords collection; explicitly pass that same array to synchronize on every eligible completed candle. All push/shift/iteration operations inside lifecycle use the passed collection, and chart rendering uses that caller-owned collection. Scalar lifecycle state,current/newest records and captured zone bounds remain.
Keep direct OHLC/time arrays and per-candle append/length invariant from5.1. Replace invalid dual-reference comparison with concrete retention accounting: retained=min(sets,cap),omitted+retained=sets. Check that a current set has a retained last record with matching serial and no ended timestamp. Checks remain fail-fast and print retention figures when applicable.
No MSM/CHOCH/ISS detection,reference defaults,D-006,chart scopes or entry rules changed. No MTF requests,taps,usage or entries. Main untouched. This is still a recovery candidate,not a verified fix; the exact mechanism behind the old discrepancy is unproven.

## Verification NOT RUN for5.2
Pine compile/runtime/replay/reload have not been run here; no automated test result is claimed.
Preserve saved references. Compare against4.4 over identical loaded history/replay endpoint. A new day has elapsed: raw counts need not equal an earlier screenshot unless coverage matches.
D1 earlier baseline: Aug25 Downward,set2,high4194.645/low4111.330,availableOct14 17:00 America/New_York.
H1 earlier baseline: Aug25 00:30input,Downward,set32 availableOct22 00:00;32/3pairs,26CHOCH,2ISS,lastbullishOct26 15:00,5rejected/0outside/51cancelled,1052candles at previous endpoint. Candidate58 points/origins remain recorded in MTF_Checkpoints.md.
Test compilation,no invariant errors,nonzero candles,confirmed prices,TJL zones/source origins/available times and H1 ISS audit; compare Latest/History/Off and later reload/replay. Report any invariant error verbatim. Remain paused on MTF additions until recovery comparison passes. Earlier failed/equality/tracking-only/history tests are still open.
