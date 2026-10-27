# TopG implementation and MTF checkpoint
Updated: 27 October 2026
Pine source of truth: TopG.pine. No stage-specific Pine files.
Status: chart-timeframe implementation; MTF and Buy/Sell entries NOT implemented.

## Current decision D-006: newest confirmed pair
Approved by the user on 27 October 2026 at 15:02 IST.
This supersedes D-005 and the earlier interpretation of MTF three-point replacement timing.
- Each new confirmed main structural high/low pair immediately supplies the current TJL1 and TJL2/A+ set at that pair's confirmation close.
- Do not wait for the next tentative point 3.
- Archive the previous set at the same close. History can show older groups; Latest shows only the unended current group.
- A source main CHOCH invalidates the old-direction current group immediately; wait for a new-direction confirmed pair before making another group current.
- Tentative points never supply TJL zones. A later point 3 can supply audit metadata only; it cannot replace or revive the set.
- Monday alone does not replace levels or reset usage.
- Current-set availability is not an entry signal; taps, unused eligibility and fresh confirmation remain separate later stages.

## Superseded decision D-005
Approved 26 October 2026 at 04:21 IST. It drew zones at pair confirmation but waited for the next point 3 to promote a replacement trade-finding set. D-006 REPLACES that promotion rule. Do not implement D-005 as a selectable default or continue treating older pair1 as current after pair2 confirms.

## Critical unresolved requirement: start-independent latest TJL
User requires one latest tradable group, not several combined tradable groups. Older groups are history only.
User identified the newer yellow/orange bearish pair in the D1 example as intended latest: low-side zone 4066.535-4111.330 and high-side zone 4194.645-4207.485, pair confirmed 14 October 2026 at 17:00 America/New_York.
D-006 makes that pair current within the existing downward run. It DOES NOT make all initial structure paths discover that same pair.
Manual reference/direction and frozen pending cycles can still produce different confirmed sequences. Do not claim automatic initialization, start-invariance or complete strategy replication has been achieved.
MTF development is paused pending resolution of this structure-selection requirement. Do not merge separate starting paths as simultaneously tradable sets.

## v0.4.2 scope
- Changes TJL replacement timing and associated labels/audit only.
- Preserves shared explicit-input MSM step, CHOCH and ISS detection logic.
- Current initialization remains manual; calculations remain chart-timeframe only.
- First expected D1 check with previous downward inputs: pair2 becomes current at 2026-10-14 17:00, pair1 becomes history. Main high/low and event counts should not change for identical data/inputs.
- TradingView compilation, runtime and visual verification are pending for this revision.

## Baseline evidence, not blanket sign-off
v0.1: reported compilation, scale alignment, Latest/History behaviour.
v0.2.1: one bearish D1 CHOCH numerical/timing audit.
v0.3: one bullish H1 ISS numerical boundary audit and later candidate cancellation. Pullback/search-window completeness remains unverified; zero outside-completion examples in reported run.
v0.4.1: reported compilation and D1 regression matched 2 pairs, high4194.645/low4111.330, 0 CHOCH, 44 candles. This was evidence for the old timing version, not an acceptance target for D-006 current-set selection.

## MTF architecture retained for later
Unique contexts: D1/H4/H1/M5/M1. Source TJLs D1/H4/H1; confirmations D1->H1, H4->M5, H1->M1. H1 has both roles.
Separate calculation from rendering. Isolate state/history per context. Return compact event snapshots with stable identities and actual source close timestamps; deduplicate per context. Record source and delivery times separately.
Current Structure.step has explicit inputs, but outer CHOCH/ISS/TJL lifecycle is still chart-context code. Do not wrap the whole chart script in request.security.
No chart barstate.isconfirmed assumption inside requested context. No developing HTF leakage, hidden delivery delay or backdated live execution claims. Preserve simultaneous events for later winner selection.
M1 is proposed initial operating chart, not verified full-engine support. Do not assume ordinary lower-timeframe security preserves all intrabars.

## Remaining gates
1. Resolve start-independent structural selection without silently substituting generic pivots.
2. Recheck native MSM/CHOCH/ISS/TJL behaviour after refactors.
3. Prove requested-context event equivalence, timestamps, reload/replay and data coverage.
4. Only then implement taps, persistent particular-level usage, cancellation/reactivation and timestamp competition.
5. Final audit: historical TJL -> tap -> fresh directional CHOCH/ISS -> selected Buy/Sell entry.
Still unresolved: source-trend-not-confirmed tradability; genuinely unspecified simultaneous source replacement/CHOCH versus confirmation; persistent identity when a source point appears in multiple sets. History pruning must not reset eligibility/usage.
Stops, targets, sizing and post-entry management remain outside scope. No profitability claims.

## Technical references
https://www.tradingview.com/pine-script-docs/concepts/other-timeframes-and-data/
https://www.tradingview.com/pine-script-docs/concepts/bar-states/
