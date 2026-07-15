# The Trading Strategy Graveyard

**24 pre-registered backtests of retail trading strategies. 21 kills — including the
strongest raw edge ever measured here, which died of friction alone. Two passes that
STILL didn't get the money. This is the full record — every strategy, every number,
every artifact that almost fooled me.**

**Site: [strategygraveyard.com](https://strategygraveyard.com)** — the same record with
the charts, plus the tools this pipeline is becoming and the vote on whether they ship.

Two years of YouTube and fintwit will tell you momentum scalping works, gap trading works,
opening-range breakouts work, mean reversion works. I built a testing pipeline to find out
which one I should automate, and committed to one rule that changed everything: **write the
pass/fail criteria down BEFORE running the test, and never move the goalposts after.**

The first 16 pre-registered gates all failed. Not one fast-trading strategy family
survived honest testing. I lost $0 learning this — every strategy died in the pipeline
before touching money. Then, in a 48-hour stretch, three more gates ran on the opposite
corner of the map — slow, diversified, cross-asset — and the record got more interesting
than a pure graveyard: one clean pass, one pass that got benched by its own pre-registered
tiebreaker, and one gate where the machine caught ME making a wrong prediction in public.
Then Gate 20 took options premium selling — the last mainstream family still untested —
and closed it with CBOE's own 33-year benchmark indices. Then the hunt resumed: gates 21–24,
four more kills in a single day (turn-of-month, factor ETFs, pairs stat-arb carrying the
best gross edge in this entire record, and insider clusters — the first kill where
friction wasn't the murderer). All written up below the kill table.

## The rules (each one added after a specific disaster)

1. **Pre-register the gate.** Pass/fail thresholds (profit factor at a stated cost,
   monthly consistency, holdout confirmation) committed in a file before any code runs.
   Fail = family closed. No "what if we tweak the stop" salvage.
2. **Split-adjusted bars only.** Raw bars + a reverse split inside your hold window = a
   fabricated +4,890% winner. That's a real trade my pipeline "found" — a $0.17 stock,
   1:25 reverse split, pure artifact. A strategy "passed validation" for six days on this
   before the adjusted rerun killed it.
3. **Honest fills.** A backtest that fills your stop AT the stop price is fantasy. Names
   that gap through a −5% stop close at −10 to −15%. Model the fill you actually get:
   checked at the close, filled at the close; gap-throughs fill at the open.
4. **De-survivorship universe.** My trend-following test scored PF 1.47 on a curated
   megacap list and 1.05 on a broad random universe. Same code, same period. The curated
   number was survivorship, not edge — you're trend-following the winners you already
   know won.
5. **Disjoint holdout, run once, only if the primary passes.** My best candidate hit
   PF 1.23 on the primary sample and 0.93 (negative expectancy) on the holdout. Sample
   luck is real.
6. **Cost ladder on every result.** Report at 0 / 0.05% / 0.10% / 0.25% per side. This
   column is where every single strategy died.
7. **Eyeball the top-10 winners raw.** If a metric improves monotonically as you extend a
   parameter (hold length, lookback), you have an artifact, not an edge.
8. **Slot-capped portfolio stats are a false-positive generator.** A slot-capped variant
   printed PF 3.96 by accidentally cherry-picking under 10% of trades — then printed
   PF 6.31 doing the same thing on an unrelated strategy. Two miracles, same bug shape.

## The graveyard (best HONEST number per family, after the rules above)

| Strategy family | Best honest result | Died of |
|---|---|---|
| Intraday momentum scalp | PF 0.97 @ zero cost, best time window | 49% win, symmetric ±1.18% = coin flip |
| Same, full 190-name mover universe | PF 0.91 @ 0 / 0.75 @ 0.05%/side | breadth ≠ edge; movers are coin flips too |
| Time-of-day gating (10:00–12:30) | PF 0.97 @ 0 | cuts bleed toward breakeven, creates nothing |
| "Let winners run" exit | made it WORSE (0.90 → 0.86) | continuation at +2% is still a coin flip |
| Overnight holds | −208% over one held month | pure gap risk |
| Mean reversion (Connors RSI-2) | PF 0.77 @ 0.25%/side, holdout 0.78 | real ~1.1 gross signal, friction eats it whole |
| Trend following (Donchian, broad universe) | PF 1.05 @ 0.25%/side | US equities are one correlated bet (2022: PF 0.40) |
| Catalyst gaps (gap ≥5% + volume spike) | PF 1.23 @ 0.25%/side, holdout 0.93 | split artifacts + fill fantasy + sample luck |
| Earnings drift (real 8-K dates from SEC EDGAR) | PF 1.11 — WORSE than non-earnings gaps (1.29) | scheduled reactions get faded; PEAD is dead at retail cost |
| Penny stocks (same catalyst logic) | PF 1.00 at a FANTASY 0.1% cost | real penny spreads are 1.5–3% per side |
| News sentiment (LLM-scored, 19.5k full articles, point-in-time replay) | made every configuration worse | news arrives WITH or AFTER the move |
| Opening-range breakout long+short on "stocks in play" (the published Sharpe-2.4 paper) | PF 0.82–0.85 @ 0.10%/side, all 3 pre-committed range variants | −0.009R per trade GROSS; the short arm loses everywhere |
| Delta-neutral funding-rate carry (crypto perps) | real mechanism, no losing year in 6 — but 2026 nets ~0.4% on capital | institutionalized away: 2021 ~22% → 2026 ~1% gross |
| Daily-sampled trend rule (200d SMA, same logic as the monthly rule that PASSED) | 7.31% CAGR vs the monthly version's 8.10%, worse drawdown | whipsaw: 8.8 signal flips/sleeve/yr vs 2.2 monthly — frequency is a cost, not a feature |
| Options premium selling (5 pro covered-call/put-write ETFs + CBOE's own BXM/PUT/BXMD/CLL indices) | every fund's Sharpe BELOW its own underlying; best index 9.92%/yr vs SPY 10.81% over 33 years | selling the upside costs more than the premium collects; short vol = short the gap |
| Turn-of-month effect (T-4..T+3, the classic window) | Sharpe 0.44 vs SPY's 0.73; captured 42% of returns in 29% of days — barely above proportional | decayed to the noise floor post-publication; third documented edge-death life-cycle in this repo |
| Factor ETFs (MTUM/VLUE/QUAL/USMV, static + 12-1 rotation) | EW Sharpe 0.87, rotation 0.84 vs SPY 0.90 | a decade of "smart beta" = SPY minus 0.8%/yr; the value regime finally arrived in 2026, after the test |
| Equity pairs / stat-arb (GGR distance method, liquid names, long-short) | **gross PF 2.23 — the strongest raw edge in this entire record** → 1.06 at 0.10%/side/leg → 0.41 at 0.25% | market-neutral crosses the spread FOUR times per round trip on a 0.22%-per-trade move; the edge is real and you can't afford to harvest it |
| Insider-cluster buying (2+ insiders, $200k+, SEC Form 4 structured data) | PF 1.17 @ 0.25%/side, 60d hold — and only 1.23 gross: the flattest cost decay in the record | the first kill where friction wasn't the murderer: real drift, too small for the bar, and 2022 PF 0.86 — insiders buy early, all the way down |

The pattern across the whole table: **every DIRECTIONAL price-derived signal converges
to roughly PF 1.05–1.10 gross** — a real, detectable asymmetry — **and retail friction
(0.10–0.25% per side) eats it whole, every time.** Buy-strength and buy-weakness. Long
and short. Intraday and multi-day. Doesn't matter. The one exception proves the rule
harder: the market-neutral pairs signal measured PF 2.23 gross — much more real than
anything directional — and died anyway, because hedged trades pay friction TWICE on a
smaller harvested move. The better the structure, the higher its toll.

## Gates 17-20: the passes, the benchings, and the last family

After the 16th kill I wrote down the honest scope of the finding: *directional fast
trading on liquid US equities doesn't clear retail cost.* That sentence points somewhere
specific — the opposite corner: slow decisions, months-long holds, across asset classes,
where friction structurally can't reach. Three pre-registered gates later (and then a
fourth, for the one family nothing had ever tested):

### Gate 17 — Cross-asset ETF trend following: the first PASS ✅

Faber's 10-month SMA rule, verbatim from literature that predates my sample by decades —
zero tuned parameters, so there's nothing to overfit and no holdout needed. 13 ETFs across
equities, bonds, gold, commodities, currencies, REITs. Long or cash, monthly.

**CAGR 8.10%, Sharpe 0.90, max drawdown −4.5%, and 2022 — the year that killed everything
else in this repo — finished at −1.1%** while SPY did −18% and 60/40 did −23%. The
pre-registered fresh-eyes review the next morning found two modeling errors and BOTH
corrections moved the result UP (I'd been crediting cash a flat 2% when 2022–23 T-bills
paid 4–5%, and I'd double-charged turnover costs). A pass that gets stronger under attack
is what a real result looks like.

Stated plainly, because the rules require it: 9.5 years of data, no trendless decade in
the sample (that's this family's known killer), T-bills beat it in 3 of those years, and
the max-drawdown figure is month-end-only. It's a compounder for money that can sit
through 20% drawdowns — not income, and no, it did not restart the day bot.

Why this doesn't contradict the kill table: monthly turnover at ETF spreads costs ~7bp a
YEAR. The wall that ate 16 strategies is a per-decision toll — this strategy makes 12
decisions a year instead of 1,400.

### Gate 18 — ETF rotation ("there's always something going up"): PASSED its headline,
### BENCHED by its own tiebreaker ⚖️

Cross-sectional relative momentum: rank the same 13 ETFs by 12-month return, hold the top
4 if they beat T-bills, else cash. The primary gate passed — 10.99% CAGR, and in 2022 it
rotated into commodities, oil, and the dollar and finished UP 3.1%. The rotation intuition
is real.

Then the pre-registered secondary criterion — written BEFORE the run: a 50/50 blend with
the Gate-17 strategy must beat it on Sharpe AND max drawdown, or deployment doesn't change
— failed on both counts (Sharpe 0.72 vs 0.90; maxDD −12.5% vs −4.5%). The eyeball pass
found the whole CAGR edge lived in one year (ex-2025: 7.09% vs 6.88% — the silver/gold
mania, +145%/+64%, real prints but one regime). And the top-3 variants' worst drawdown
(−26%) was February–June 2026, i.e. the leadership reversal happening RIGHT NOW.

**A strategy that passes its headline numbers and still doesn't get the money is not the
system failing — it's the only moment the system actually matters.** Anyone can hold the
line on a failing backtest. The tiebreaker you wrote down in advance is for the day the
backtest flatters you.

### Gate 19 — Daily vs monthly: the machine referees its operators 🔀

I said daily sampling would lose to monthly (whipsaw multiplication). The counterargument
in the room: daily catches rotations faster. Both predictions went into GATE.md before the
run. Split decision:

- **Daily trend: FAILED, as I predicted** (7.31% vs 8.10% — that's the new kill-table row).
- **Daily rotation: PASSED, against my prediction** — 13.97% CAGR at next-close fills, and
  it survived every check I threw at it expecting it to die: filling a day LATE improved
  it (daily rankings buy spikes; the delay dodges the one-day snap-back), it holds at
  triple the assumed costs, the edge over monthly survives with 2025 deleted entirely
  (10.70% vs 6.71%), and it wins exactly where monthly rotation is weakest — reversal
  years (2018: −4.3% vs −12.5%).

Then the same frozen tiebreaker from Gate 18 benched it anyway: blend Sharpe 0.92 vs 0.96,
maxDD −10.3% vs −4.5%. Close — and this is exactly the moment loosening the standard is
most tempting and most fatal. The certified-but-benched strategy keeps its certification;
the deployment decision keeps its integrity; and the repo gains its best exhibit: **a
falsification machine that catches its operator's wrong predictions, not just his bad
strategies.** (Also on its record: 21.8x annual turnover — every gain short-term for
taxes — and its worst month in 9.5 years was LAST month.)

### Gate 20 — Options premium selling: the last of the mainstream families ❌

Covered calls, put-writes, "volatility income" — the last family from the standard
retail playbook that none of the gates above had touched, and the internet's favorite
answer to "how do I get paid while I wait." I tested it twice over, so no
retail-implementation excuse survives:

**Round 1 — five professional wrappers** (covered-call and put-write ETFs on QQQ, SPY,
and vol futures: QYLD, XYLD, JEPI, PUTW, SVOL), total-return basis so distributions
count. Pre-registered prediction: they'd fail the 2022-cushion test. Wrong — they all
PASSED it (premium sellers genuinely soften grinding bears; JEPI −3.5% in 2022). What
actually killed them, unanimously, was risk-adjusted return vs their own underlying:
**every single fund had a worse Sharpe than the index it writes options on.** QQQ made
21.6%/yr over QYLD's decade; QYLD kept 10.0% of that with 70% of the drawdown. And their
worst months ARE the crash months (XYLD −16.3% in Mar-2020): the premium never covers
the tail. One fund in the test (PUTW) appears to have quietly closed mid-sample — a
survivorship lesson embedded inside the options test.

**Round 2 — the mechanism itself,** on CBOE's own benchmark indices built from real
traded SPX option prices back to 1990 (BXM, PUT, BXMD, CLL). This is where it got
interesting: **pre-2012, every premium-selling index BEAT SPY risk-adjusted. Post-2012,
every one lags it.** The volatility risk premium was real — the academic literature that
made these strategies famous was written on the pre-2012 sample. Then systematic
vol-selling funds and 0DTE flows industrialized it, and the edge migrated to whoever
collects the flow. It's the same life-cycle the funding-carry gate documented in crypto
(2021: ~22% → 2026: ~0.4%), playing out two decades slower. In the 2020 crash the
indices fell HARDER than SPY (BXM −54.6% annualized vs −32.4%): short vol is short the
gap. Insurance that pays out in drizzle and fails in the flood.

Prediction audit, again: directionally right (family closed), wrong in the details (the
failure mode I predicted wasn't the one that fired). Both audits are in the experiment
folder — the machine referees its operator, not just the strategies.

## Gates 21–24: the hunt resumes (2026-07-15, three kills and counting)

The stand-down on edge-hunting ended deliberately: every kill is now also a public
writeup, so the testing pays either way. Same rules — gate frozen before code,
predictions on record, no salvage sweeps. Three gates reported the same day:

**Gate 22 — turn-of-month (kill #18).** The classic T-4..T+3 window, one fixed
literature definition. Sharpe 0.44 vs SPY's 0.73; it captured 42% of returns in 29% of
days — barely above its proportional share, versus the ~100% concentration the
1988–2005 literature documented. Third full edge-death life-cycle in this repo (after
the vol premium and funding carry): real premium, published, arbitraged to the noise
floor. One residual gleam, stated with its own warning label: 2022 came in at +12.7%
while SPY did −18.2% — a single regime, exactly the kind of one-year edge the rotation
gate already taught us not to buy.

**Gate 23 — factor ETFs (kill #19).** MTUM/VLUE/QUAL/USMV, equal-weight and 12-1
rotation, ten years. Equal-weight: Sharpe 0.87 vs SPY's 0.90 — "smart beta" priced out
to SPY minus 0.8%/yr. Rotation: 0.84, better 2022, same verdict. The eyeball pass
verified the sting in the tail: value finally woke up violently in 2026 (VLUE +48%
YTD) — *after* a decade of drag that would have shaken out any live allocator. The
premium shows up on its schedule, not yours.

**Gate 24 — pairs / stat-arb (kill #20, and the headline).** GGR distance method,
liquid names, long-short. **Gross PF 2.23 — the strongest raw edge in these 24
gates.** Then the cost ladder: 1.52 at 0.05%/side/leg, 1.06 at 0.10%, 0.41 at 0.25%.
A market-neutral round trip crosses the spread four times to harvest a +0.22%
average move; the strategy's entire gross edge approximately equals its own friction
bill. The pre-registered prediction got audited both ways: the cost model was
confirmed exactly, and my estimate of the raw signal was ~5x too low. Institutional
stat-arb desks aren't seeing something retail can't see — they're paying something
retail can't pay.

**Gate 21 — insider-cluster buying (kill #21, and a different kind of death).** Form 4
open-market purchase clusters (2+ distinct insiders, $200k+ aggregate, 10 days) via
SEC's own structured data, filing-date entries, no lookahead. The drift is REAL: +1.9%
average per event over 60 days, PF 1.23 gross — and because multi-week holds barely pay
friction, it lands at 1.17 net. **That's the flattest gross-to-net decay in the entire
record: the first kill where friction wasn't the murderer.** What killed it instead:
magnitude (1.17 < the 1.30 bar) and 2022 (PF 0.86 — insiders buy early, all the way
down through a bear; a 60-day window pays their earliness, not their eventual
vindication). Two prediction audits on record: my co-pilot predicted PASS (first
recorded human prediction — the machine referees him too now); I said 50/50 with the
drift real, and the coin landed tails with a failure mode I didn't predict. Also
caught mid-gate by the eyeball rule: pre-window filings collapsing into fake day-one
mega-clusters that "entered" at meme-season June 2021 — the artifact flattered the
strategy and got corrected before publication. Sixth artifact the checklist has caught.

## The three lessons that cost the most

**1. The market's fill is not your backtest's fill.** Half my "edges" were fill
fantasies. The highest-ROI line of code I wrote all year models a stop as "checked at the
close, filled at the close."

**2. A passing backtest is a hypothesis, not a result.** My catalyst strategy passed a
bias-controlled validation — de-survivorship universe, liquidity floors, positive in the
2022 bear — and was still fake. Split artifacts, fill fantasy, and a failed holdout took
it apart layer by layer, six days after it "passed." If I'd deployed on the pass date,
I'd have found out with money.

**3. Your slippage is somebody's revenue.** The bots that actually make money — market
makers, arbitrageurs, carry desks — don't predict direction. They collect the friction
directional traders pay. Once I read my cost-ladder column as their income statement, the
whole table made sense: I'd spent months reverse-engineering their customers.

None of this says edges don't exist. It says that at retail cost structures, on liquid US
equities, price patterns aren't where they live — and an honest pipeline tells you that
for $0, while a dishonest one tells you whatever you want to hear until you fund it.

## About this repo

Right now this repo is the writeup. The pipeline behind it consists of:

- an **honest-fill simulator** (close-fill / intraday-GTC / gap-through fill models, with
  a per-side cost ladder)
- **pre-registration gate templates** (the discipline in the rules above, as files)
- a **de-survivorship universe builder** (broad random sampling + split-adjusted bar
  caching)
- a **SEC EDGAR 8-K fetcher** (free, survivorship-free earnings dates — the data vendors
  charge for)
- a **funding-rate fetcher + carry simulator** (public endpoints, no keys)
- the **ETF gate harness** from Gates 17–19 (monthly + daily engines, real FRED T-bill
  cash path, turnover accounting, pre-registered GATE/VERDICT templates with the
  blend-tiebreaker pattern)
- the **Gate 20 options harness** (total-return wrapper testing with NAV-erosion
  accounting, plus the CBOE benchmark-index fetcher for the 33-year mechanism test)

I'm gauging whether it's worth cleaning these up for release. If you'd use them, **star
the repo or open an issue** saying what you'd want — that's the signal I'm watching.

Questions about the methodology are welcome; I'll answer anything in the issues.
