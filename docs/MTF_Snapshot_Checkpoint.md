# Next checkpoint: native context and event snapshots
Date: 28 October 2026
Development branch: feature/mtf-integration. main must remain unchanged.
Status: implementation specification and frozen reported fixture ONLY. No Pine change or MTF transport is implemented by this checkpoint.

## Recovery evidence and authority
This supersedes the NOT RUN comparison status for v0.4.5.2-dev in MTF_Storage_Regression.md, narrowly for the user's supplied run.
User reported error-free runtime at20:59IST. At21:27IST both v0.4.4-dev and v0.4.5.2-dev tables matched with the same Aug25 00:30 H1 reference and Downward direction:1075candles,32/3pairs,26CHOCH,2ISS,5rejections/0outside/51cancellations,currentTJL32 availableOct22 00:00 and identical candidate58 audit.
Machine-readable capture: tests/fixtures/h1_aug25_native_baseline.json. This is reported output evidence, NOT source OHLC, a full event trace or an automatically executed test. Last processed source close and reference input timezone are not supplied; do not invent them.
Earlier1298candles/40pairs/32CHOCH versus1075/32/26 discrepancy is absent in the final comparison; its cause was not established. Do not retroactively assert a settings mistake or explain it solely by elapsed time.
D1 report21:02IST also restored nonzero46candles,2/2pairs,high4194.645/low4111.330,currentTJL2 availableOct14 17:00;tentative3995.810. This is latest displayed-state evidence, not a same-endpoint v0.4.4 comparison.
D-006 remains authoritative: new confirmed pair immediately current. Fixed manual references remain; start-independence is deferred, not solved. Main stays v0.4.2.

## Implementation deliverable, not yet done
Keep the single TopG.pine source. Extract one calculation-only context advance path with explicit candle OHLC, original open/close timestamps, initialization, capacities and context capabilities. It must orchestrate the existing exact MSM/CHOCH/ISS/TJL calculations without drawings, table calls, display switches or implicit operating-chart inputs.
Each context owns its persistent calculations and directly managed collections. Avoid reintroducing failed field-alias paths. Native chart rendering consumes scalar snapshots rather than mutating calculation state. Do not introduce separate approximate detectors.
Preserve original processing order, source CHOCH restart, cancellation before internal stepping, boundary/strictness/tie rules, and source-confirmed pair timing. Do not silently resolve an unspecified same-close trade ordering.

## Snapshot contract
Emit a new scalar-only record for each event; never put mutable Structure/ISSState/TJLState, growing arrays or drawing IDs into transported event data. Do not mutate emitted records; this is a coding invariant, not a claim that Pine enforces immutability.
Required identity/provenance: symbol/feed, source timeframe, event kind, deterministic identity anchored in source time/origins, context-local sequence, source candle open/close, identification and confirmation close, original point/zone candle, structural direction versus trade side, level/set identities and exact prices/bounds. Keep source event time and later chart delivery time separate.
Event kinds: new main tentative, confirmed pair, main CHOCH, ISS candidate start/rejection/tracking-only/cancellation/confirmation, TJL replacement/invalidation. A single close can produce multiple legitimate event records; preserve the batch rather than overwrite a last-event slot.
CHOCH snapshot must capture old direction/support-or-resistance/activepoint3 and original candles BEFORE restart clears point3. ISS snapshot must capture fixed boundaries, all five prices/origins and P3/P4 zone sources BEFORE later candidate state can change. Pair/TJL snapshot captures original high/low candle zone bounds at confirmation.
History-retention limits are separate from eventual usage identity. Snapshots must not claim a retained historical ISS is a new/current confirmation.

## Native acceptance before requests
Pine refactor must compile and reproduce the frozen H1 report at identical coverage, plus compare v0.4.5.2 side-by-side over a common loaded endpoint. Do not demand that later totals equal an earlier screenshot unless endpoints match.
Verify native events and original candle zones, D1 baseline, actual CHOCH restart, Off/Latest/History equivalence, reload/replay and storage/retention checks. Failed/equality and outside-completion tracking-only cases remain open; zero occurrences is not a tested path.
First native event/snapshot implementation has no MTF requests, taps or entries. Only after native checks: add D1/H4/H1/M5/M1 transport and verify each requested context against its native chart. MappingD1->H1,H4->M5,H1->M1. Explicit per-context start inputs and data-coverage warnings remain required.
Do not merge main or claim MTF/entry completion at this specification checkpoint.
