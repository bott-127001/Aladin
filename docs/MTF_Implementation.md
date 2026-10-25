# TopG MTF integration checkpoint
Date: 26 October 2026
Status: engineering design checkpoint, NOT an implemented MTF release.
Pine source of truth: TopG.pine. Do not create stage-specific Pine files.
Baseline: v0.4, commit 4cf5ea644e9491bc7fd764560695125294331019.

## Authority
Strategy PDFs and explicit later user approvals govern detection. TradingView documentation governs platform mechanics, not trading rules.

## D-005: approved TJL timing
User approved 26 October 2026 at 04:21 IST:
- Draw confirmed-point TJL1/TJL2 zones when their structural pair confirms.
- Make the replacement trade-finding set available when the next active tentative point 3 is identified.
- A touch before new-set availability cannot activate that new set.
- Retain the previously current set until replacement or source CHOCH.
- Source CHOCH invalidates old-direction sets immediately.
- Monday alone does not replace levels, cancel activation or reset usage.
Structural availability, set availability, price touch and entry confirmation are distinct events.

## Reported baseline evidence, not blanket sign-off
- v0.1: user reported compilation, correct scale alignment and Latest/History switching.
- v0.2.1: one bearish D1 CHOCH audit had two qualifying closes and eligible point-3 timing.
- v0.3: one bullish H1 ISS audit passed numerical boundary checks. Internal P3/P4 share an origin; inclusive source rules do not prohibit it. Full pullback/search-window correctness remains unverified.
- v0.4: D1 current set1 retained while structural pair2 waits for point3. Set1 marked Sep10, ready Oct9. Zone bounds and historical invalidation still need checks.
- No verified live order routing, Buy/Sell entries, profitability backtest or complete strategy sign-off.

## MTF engine requirements
Five unique timeframe contexts: D1, H4, H1, M5 and M1.
- D1/H4/H1: independent source structure and TJL state.
- H1/M5/M1: independent CHOCH/ISS confirmation state.
- H1 can serve source and confirmation roles using one consistent structural context.
Mapping: D1 -> H1; H4 -> M5; H1 -> M1.
First proposed integrated operating chart: M1. Do not claim higher operating-chart support without event-equivalence checks.
Provide independent manual start-time and direction inputs per unique timeframe. Never silently infer initial direction or shift missing starts. Show missing-history and startup status per context.

## Required refactor before requests
Current chart-only logic uses chart OHLC, global candle arrays, drawings and barstate.isconfirmed. It cannot be safely wrapped wholesale in request.security.
1. Separate a calculation-only engine from labels, zones, tables and audit presentation.
2. Give each context isolated candle storage, main/internal structure, captured boundaries and TJL lifecycle state.
3. Parameterize the candle input and original open/close timestamps so no context accidentally uses operating-chart prices.
4. Return compact immutable event snapshots, not entire growing candle collections.
5. Render requested snapshots only in the chart context. Preserve Latest/History equivalence.
6. Avoid introducing source-trend alignment or unapproved tradability filters.

## Event contract
Each event needs context, stable event ID, original source candle, identification close, confirmation close, source level identity and direction. Relevant snapshots include confirmed pair, new point3, main CHOCH, confirmed ISS, TJL set replacement and invalidation.
Deduplicate by context and event ID. Use actual source close timestamps for competition, not evaluation order. Record source event time and chart delivery time separately.

## Timing gate
- barstate.isconfirmed does not work inside request.security (official documentation).
- Unoffset developing higher-timeframe values can change after reload.
- Official offset-plus-lookahead pattern delivers the previous confirmed source bar; assess its actual delivery relative to the required entry close.
- Do not silently backdate a live signal or call delayed delivery immediate execution.
- Choose and verify delivery mechanics before connecting activation to confirmation.
- Preserve simultaneous events as a timestamped batch for later winner selection.
- Preserve M1/M5 events; ordinary lower-timeframe security is not a complete intrabar stream.
- Disclose session gaps, data corrections and available-history boundaries.

## Acceptance gates
A. Native engine after refactor reproduces the supplied D1 CHOCH and H1 ISS baselines for identical inputs/history.
B. Each requested context matches its native-chart event IDs, prices and source timestamps within declared coverage.
C. Confirmed results checked through reload and replay; live delivery timing assessed separately.
D. Multiple events sharing a timestamp are retained without duplicate processing.
E. Missing start/history and resource limits produce explicit warnings, not fabricated continuity.
F. Only after A-E: connect taps, eligibility, one-trade-per-level, cancellation/reactivation and winner selection.
G. Final historical review must trace TJL -> tap -> fresh directional confirmation -> selected entry.

## Open strategy boundaries
Source trend not confirmed: do not silently equate marking direction with tradable trend.
Simultaneous source replacement/CHOCH and confirmation: resolve any genuinely unspecified outcome before entry code.
Particular-level usage identity must persist when the same original point appears in another set. Rendering retention cannot reset it.

## Next code deliverable
Refactor the single TopG.pine into a calculation/presentation boundary and prove native-context regression first. Then add requested timeframe transport. Do not label this checkpoint itself an MTF implementation.

## Technical references
https://www.tradingview.com/pine-script-docs/concepts/other-timeframes-and-data/
https://www.tradingview.com/pine-script-docs/concepts/bar-states/
