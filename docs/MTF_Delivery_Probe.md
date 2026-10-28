# Confirmed-source delivery timing probe v0.1
Date: 28 October 2026
Branch: feature/mtf-integration. main and TopG.pine are unchanged by this checkpoint.
Status: diagnostic code prepared; Pine compilation, chart runtime and live/reload/replay verification NOT RUN here.

## Native checkpoint evidence
User supplied H1 v0.4.6 results at21:53IST matching previously reported structure/TJL/ISS figures and candidate58 audit;1076candles versus1075 previously. Event audit supplied first sourceOPEN2026-08-24 15:00 and last processed sourceCLOSE2026-10-28 12:00 America/New_York.432/0 is calculation-event count/last-batch size, not trade signals. Exact same-endpoint count comparison remains narrower than full regression sign-off.
User corrected D1 reference after an initial Start not found report. At21:57IST D1 showed46candles,2/2pairs,high4194.645/low4111.330,tentative3995.810,currentTJL2 availableOct14 17:00;source lowOct6/highOct8 17:00;point3originOct22 17:00. This matches the supplied recent D1 report. Trailing pasted zero-start prose was not treated as current dashboard evidence.
These reported comparisons allow beginning a platform-timing diagnostic. They do not close replay,history/zone,failed-pattern,equality or tracking-only acceptance gates or authorize merging main.

## Purpose and limits
The standalone tests/mtf_delivery_probe.pine is a test instrument, NOT another TopG implementation. TopG.pine remains the sole strategy source. The probe has no MSM,CHOCH,ISS,TJL,reference initialization,taps,usage or entries; it therefore cannot establish requested-engine equivalence or complete MTF integration.
Run on M1 only. It requests the prior confirmed D1/H4/H1/M5 source candle's open timestamp,close timestamp and closing price. All three tuple items are offset by one SOURCE bar and paired with lookahead_on. Do not remove the offset or substitute unconfirmed HTF values. No barstate.isconfirmed is used inside request.security and no ordinary lower-timeframe requests are used.

## Timing interpretation
Source CLOSE is the original requested candle's close time. First observed M1 OPEN is the operating bar on which the returned source-close identity first changed in this script execution. For historical bars this is historical mapping evidence, not actual wall-clock delivery evidence.
First observed LIVE time is timenow captured on a realtime observation, with primitive varip state retaining the first observation across intrabar executions. It measures when this script observed the update, not when an exchange or broker guaranteed availability; network/feed/scheduling may contribute. Historical values intentionally show N/A. Do not fabricate historical live latency.
M1-open gap is operating-bar OPEN minus source CLOSE in minutes. Zero means mapped to an M1 bar opening at that close; it does NOT prove that TopG confirmation/events or an entry were executable at that moment. Gaps/sessions and loaded-history boundaries may cause later observations. INITIAL sample is the first loaded sample, not demonstrated first delivery, and must not be used as a latency test.
The offset pattern exposes the previous confirmed source value during the following source interval. This probe does not silently claim every strategy event arrives at its required confirmation close, or compensate delays by backdating.

## First TradingView check
1. Create a separate temporary indicator from the probe on OANDA:XAUUSD M1; keep TopG reference settings untouched. Table timezone defaults America/New_York and is explicitly labeled.
2. Report compilation/runtime success or exact error, then send the table. N/A live times on historical observations are expected.
3. While the market is updating, watch at least one M5 boundary. After that source close advances, send the table showing LIVE observed if seen live. Initial/historical rows are not failures merely because they lack live timestamps.
4. Compare a returned source close price/timestamp with its native source candle. Later test H1/H4/D1 boundaries and reload/replay consistency separately; a single M5 example cannot validate every source timeframe.

## Next integration gate
If the probe works, instrument the requested calculation contexts with source-versus-delivery metadata and preserved event batches, then compare requested events against native TopG on matching initialization/history. Each context still requires explicit valid reference/direction and coverage warnings. M1-native confirmation handling must be verified separately; this probe only requests higher timeframes.
Do not attach taps or Buy/Sell selection,claim full-engine M1 support,or merge main based on this probe.

## Platform references
https://www.tradingview.com/pine-script-docs/concepts/other-timeframes-and-data/
https://www.tradingview.com/pine-script-docs/faq/other-data-and-timeframes/
