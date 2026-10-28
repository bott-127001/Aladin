# v0.4.6-dev: native context and event-driven presentation
Date: 28 October 2026
Branch: feature/mtf-integration. main unchanged. Snapshot contract remains docs/MTF_Snapshot_Checkpoint.md.
Status: implemented candidate; Pine compilation and TradingView regression NOT RUN. No runtime or full correctness claim.

## Boundary
ContextState.advanceContext receives explicit candle OHLC,previous candle values/source close,source open/close,symbol/timeframe,manual start,capacity,retention,capabilities and caller-owned histories/TJL collection/event batch. It orchestrates MSM,CHOCH,ISS,TJL with no chart OHLC/time globals,barstate,display switches,drawing or table calls. The native adapter supplies only completed chart candles. A future requested context must create its own state AND every collection; this revision does not yet create or transport five contexts.
Direct authoritative candle arrays and the explicit single calculation-TJL collection are retained. No CandleStore field aliases. Context contains calculation objects/counters,not chart rendering objects.

## Snapshots
CalcEvent contains scalar strings,numbers,bools only. Snapshots are fully built before publish and never mutated afterwards by this implementation. Identity uses symbol,timeframe,event kind,source confirmation timestamp and captured origins; sequence is context-local. This is convention,not language-enforced immutability or proved cross-start equivalence.
Events preserve batches per source close: main tentative/pair,CHOCH,ISS start/rejection/tracking-only/cancellation/confirmation/internal diagnostic points,and TJL new/end/point3 metadata. CHOCH captures old support/resistance,point3 and previous/second closes before restart. ISS completion captures five origins/body prices,original candle wick zones,boundaries and completion close. TJL events copy scalar source zones/timestamps rather than returning mutable calculation records.
Snapshots carry source time only. Requested-chart delivery timestamps/deduplication and event-equivalence transport remain future work; no backdating or live-delivery guarantee is claimed.

## Presentation
Native chart consumes events after calculation. Point/zone/marker drawing helpers receive captured scalar origins/prices/bounds; they never read calculation candle arrays. Audit text is produced from snapshots. TJL displayRecords are explicitly constructed presentation copies,not aliases to engineRecords or snapshots; end and metadata events update those display copies only. The engine never reads displayRecords. A retention-size check detects missed projection events; individual event/zone parity still needs inspection.
Existing Off/Latest/History controls,scope and audit priority retained. Optional calculation-event audit adds event count,batch size,first source open,last source close and latest identity. It defaults off. No additional trade filters or entries. Formatting of diagnostic/audit text may differ; calculation figures and source-zone identity must not.

## Native regression gate
Use v0.4.5.2-dev commit a801c10cea564e8534e232378c3c09431001d8dd as side-by-side baseline with identical start,direction,feed,history and replay endpoint. Frozen H1 fixture is reported-output evidence,not raw OHLC or an executable price-series test.
Expected earlier H1 endpoint:1075candles,32/3pairs,26CHOCH,2ISS,bullishOct26 15:00,5rejected/0outside/51cancelled,TJL32 availableOct22 00:00. Compare all candidate58 prices/origins/bounds and both P3/P4 zones. Newer endpoints may have additional bars/events; never force old counts on different coverage.
Also check fixed-reference D1 current pair2,original zone bounds,sourceCHOCH snapshots/restart,history endpoints,zero-width zones,Off/Latest/History equality,and reload/replay. Confirmed ISS audit retention is historical evidence,not a newly actionable signal. Outside-completion and strict equality/failed patterns remain open tests.
Inspect multi-event batches without overwriting events. Check calculation-event first/last timestamps when comparing coverage. Do not proceed to requests or merge main until native regression passes; further MTF source delivery/timing and entry rules remain unimplemented.
