# MTF integration development checkpoints
Date: 27 October 2026
Branch: feature/mtf-integration. main remains v0.4.2, commit 803d245f14bba78673c0235e526bac41a3958020.

## Current authority and initialization
D-006 remains in force: newest confirmed pair becomes current immediately, not at the next point 3.
User agreed to defer start-independence and use a fixed manual reference for testing; MTF preparation is therefore resumed, superseding the pause in MTF_Implementation.md. This does not resolve initialization sensitivity.
D1 baseline: OANDA:XAUUSD, downward, selected August 25 2026 candle. Previous input displayed 02:30; its display timezone is not established, so preserve the saved selection rather than converting that time. Other contexts still require their own explicit valid candle-open/direction inputs; do not silently copy the D1 timestamp or silently shift missing starts.
User's D1 screenshot: 2 total/directional pairs, high4194.645/low4111.330, no CHOCH, 45 stored candles, current TJL set2 available 2026-10-14 17:00 America/New_York. Screenshot verifies displayed state only, not complete strategy correctness.

## Checkpoint 1: v0.4.3-dev
Scope: extract CHOCH consecutive-close counting, event count/time/side and structure restart into explicit-input methods in the single TopG.pine. These methods do not read chart OHLC/time, drawings, global histories, view inputs or barstate. The chart wrapper still supplies eligible closed candles.
Preserve strict support/resistance comparison, active-point-3 requirement, identification-close eligibility, reset on new tentative/pair, two adjacent closes, old-direction zone rendering before restart, point3 reference, and pullback gate after confirmation. Preserve original simultaneous continuation/CHOCH halt.
This is a partial boundary extraction, NOT the completed per-context engine and NOT an MTF release. ISS, TJL lifecycle, storage, event snapshots and orchestration remain to extract. No request.security calls, taps, usage tracking or Buy/Sell entries are added. Default initialization is unchanged; retain the user's saved reference settings.

## Verification pending
TradingView compilation/runtime: NOT RUN. Native regression: NOT RUN for this revision.
1. Load branch TopG.pine, preserve D1 reference/direction; compare set2, prices and available close against the baseline. Candle count is comparable only over identical coverage; later completed bars naturally increase it.
2. Check a known actual CHOCH event, not just the no-CHOCH D1 example: same second-close timestamp, side, source zones, point3 reference and first subsequent pullback.
3. Check a failed consecutive-close pair and equality; they must not confirm CHOCH. Check eligible point3 identification candle.
4. Compare Latest/History/Off underlying results and repeat after reload/replay.
5. Recheck the previously reviewed H1 ISS example because it shares main CHOCH state.
Do not call checkpoint passed on compilation alone.

## Next checkpoints
Extract remaining calculation state and immutable events, prove native parity, then add D1/H4/H1/M5/M1 transport with explicit source/delivery timing. Compare integrated events to native contexts before activation/entry work.
Mapping remains D1->H1, H4->M5, H1->M1. Entry selection uses actual confirmation close timestamps and approved tie-breakers. Main may be merged only after final testing and explicit user approval.
