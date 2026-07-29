# The Trading Strategy Graveyard

**35 pre-registered backtests of retail trading strategies. 29 kills — including the
strongest raw edge ever measured here, which died of friction alone, and the famous
candlestick patterns, measured across 43,624 signals. Two passes that STILL didn't get
the money. A reader-designed gate that borrowed Japan's lost decades and split the
verdict by five basis points. And a closing 30-hour sweep through the collector's side
of the market — merger arbitrage, prediction-market arbitrage, paid order flow, and two
operator-proposed momentum designs — that ended the hunt with the whole map colored in.
This is the full record — every strategy, every number, every artifact that almost
fooled me.**

**Site: [strategygraveyard.com](https://strategygraveyard.com)** — the same record with
the charts, plus the tools this pipeline is becoming and the vote on whether they ship.

**Want something built or killed?** The Gate 17 extension below exists because a reader
demanded it. That's now a standing offer:
[file a build request](https://github.com/cmatthwilkes-debug/trading-strategy-graveyard/issues/new?template=build-request.yml)
— bring a strategy idea with a stated mechanism and it gets a pre-registered public
gate (credit to you, pass or kill), or request a testing tool for the kit backlog.
Nothing that trades a live account gets built, and requests are public by design.

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
and closed it with CBOE's own 33-year benchmark indices. Then the hunt resumed: gates 21–25,
five more kills in a single day (turn-of-month, factor ETFs, pairs stat-arb carrying the
best gross edge in this entire record, insider clusters — the first kill where friction
wasn't the murderer — and the famous candlestick patterns, dead on 43,624 signals). And
when a reader ran at the one clean pass, the pass got extended back through 2008 and
repriced in public — that story is below the kill table too. All written up there.

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
| Candlestick patterns (bullish engulfing, hammer, piercing line — 43,624 signals) | pooled PF 0.95 @ 0.25%/side; gross ceiling 1.10–1.12; the hammer alone is a literal coin flip (PF 1.00) | the engulfing (0.80) is the single worst construction in this record; "reversal" candles in 2022 = PF 0.75 |
| Cash-merger arbitrage (789 deals hand-built from SEC EDGAR, announcement-first so the broken deals stay in, break gaps paid in full) | 4.03% CAGR vs a cash+3pp bar of 4.32% — Sharpe 0.95, book maxDD −5.5%, and the famous break tail turned out SURVIVABLE (10% broke, mean −2.9%) | priced to the bar: clears cash+3 only at exactly ZERO cost; 0.05%/side already sinks it. The 2019+ holdout was never opened — frozen protocol seals it on a primary fail |
| Prediction-market cross-venue arb (17 identical-resolution Kalshi/Polymarket pairs, hourly) | raw readout "+29%/yr" — every fat entry a bookless listing-week print; mature-market number +2.2%/yr on deployed capital | the gap is real (>1¢ in 50–95% of hours) and smaller than the toll (2.5–4¢ round trip); pays less than T-bills. One venue added a 5% taker fee mid-study |
| Paid order flow / maker rebates (receipts only — no simulated maker fills, queue position is unknowable offline) | net −0.17 to −0.23¢/share at the accessible tier, every venue receipted | the payment is real and starts at ~$10M/month of flow; below that the structure charges YOU to provide |
| Unified everything-rotation (1,365 de-survivorship stocks + 13 cross-asset ETFs, ONE momentum pool — operator's design) | 7.14%/yr vs SPY's 11.53%, maxDD −41% vs −24.5% | the defensive valve never fired: ETFs entered the top-10 in 0 of 55 months — a thousand-stock pool always has ten hotter names than TLT/GLD |
| Trend overlay on the S&P momentum index fund (Faber 10m, drawdown-engine framing pre-registered) | cut maxDD from −31% to −15% — the insurance WORKS | and costs 3.4pp/yr against a 2pp cap frozen in advance; six whipsaw re-entries, one at +17.4% above its exit. Sizing beats switching |
| "Smart money concepts" M15, mechanized with every rule frozen in writing (7,087 forex trades, 5 years) | PF 0.951 BEFORE costs, 0.670 after 1 pip/side | loses gross, and sits inside a 20-seed random-entry band at matched frequency/risk/exits — the zones are decoration |
| Cross-asset trend inside a prop-firm eval (outside spec, co-credit Owen Mantz) | Sharpe 0.82 at honest costs; static-rules eval: 78% funded, EV +$19k/attempt — the record's second pass ◐ | trailing-drawdown + 40% consistency rulesets deny 27–42% of payout cycles: the firm's rule sheet, not the strategy, is the constraint |
| Gate 33's spec on FTMO's actual terms (CFD basket, overnight financing) | basket translation ≈ free (Sharpe 0.82→0.78) | +6%/yr financing on held notional → Sharpe 0.11. The wrapper's own swap charge is the entire kill |
| Gate 33's spec on a swap-free static-drawdown firm (FundedNext class) ◐ | without the consistency rule: 76% funded, EV +$22k | with the published 40% best-day rule: 66–69% of payout cycles denied — one ambiguous payout-policy sentence decides the corner |

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
corrections moved the result UP (I'd been crediting cash a flat 2% instead of the real
monthly T-bill path — bills reached 4%+ by late 2022 and averaged ~5% in 2023 — and I'd
double-charged turnover costs). A pass that gets stronger under attack is what a real
result looks like.

Stated plainly, because the rules require it: 9.5 years of data, no trendless decade in
the sample (that's this family's known killer), T-bills beat it in 3 of those years, and
the max-drawdown figure is month-end-only. It's a compounder for money that can sit
through 20% drawdowns — not income, and no, it did not restart the day bot.

Why this doesn't contradict the kill table: monthly turnover at ETF spreads costs ~7bp a
YEAR. The wall that ate 16 strategies is a per-decision toll — this strategy makes 12
decisions a year instead of 1,400.

### Gate 17, extended — a reader ran at the pass, and the pass got repriced 📉✅

A reader challenged the only clean pass in this record, on two fronts. First, that the
cash-yield correction had over-credited 2022 (2022 bills *averaged* ~2% — the 4–5%
prints came late in the year). Second, and sharper: **2022 is this strategy's easy case
wearing a hard case's clothes.** One inflation shock makes trend rotate to cash exactly
when cash starts paying. The untested regimes were the zero-rate deflationary bear and
the trendless ZIRP decade — the ones where rotation parks you in cash earning nothing
while the filter whipsaws. So I ran both checks the same day. Per-mechanism receipts:

**The cash audit: no over-credit, but the covariance is real.** The fix used the actual
FRED monthly path, not a flat 4–5% (2022: 0.15% in January to 4.25% in December, mean
2.02% — the reader was right about the average). Real path vs flat-2% moved 2022 by
+0.21pp, and legitimately: the book was heaviest in cash in H2, exactly when rates were
highest. The covariance the reader described exists — it's worth ~0.2 points, not the pass.

**The extension: same frozen rules, pushed back to January 2008** (UUP is the youngest
sleeve, live Feb 2007; pre-2016 prices cross-checked against the original source, worst
mean monthly-return disagreement 4.6bps). Over 18.4 years: **CAGR 5.85%, Sharpe 0.69,
max drawdown −12.1%** — vs SPY 11.29%/−46.3% and 60/40 8.39%/−26.2%.

- **2008, the zero-rate bear: −1.5% vs SPY −36.8%.** Handled — but not by cash yield.
  The bond sleeves stayed long and rallied. The mechanism has two legs: inflation bear,
  cash pays; deflation bear, duration pays. 2022 is the only regime that breaks the bond
  leg, which is why it killed everything else here.
- **The reader's regime is where the real damage lives.** Max drawdown troughs in May
  2012 — the middle of the ZIRP grind, not either bear — and doesn't recover until July
  2016. Longest stretch below its own prior peak vs T-bills: **5.1 years** (2011–2016),
  with the cash sleeves paying 1–3bps a year the whole way.
- **Flagged, not hidden:** against the gate's literal CAGR ≥ 6% bar, 5.85% is a thin
  fail. The bar's written rationale was T-bills + 3, which the extension clears
  (extended-sample bills averaged 1.4%, making the bar 4.37%). Both readings stay on
  the record.

The pass holds. The price changed: ~6% through two full regimes, no year worse than
−1.6%, but you can sit 4–5 years underperforming T-bills and 12% below peak. The
realistic failure mode is behavioral — quitting in year three of the grind — not a
crash. Sizing now keys off the extended numbers, not the friendly 2017–2026 ones. A
pass that gets *repriced* under attack, in public, is the other thing a real result
looks like.

**Round two — the reader came back for the flag itself.** His charge: the bar was 6%,
the extension printed 5.85%, and the rationale got recomputed until the strategy
passed — goalpost-moving by the guy who runs a graveyard for it. The sequence receipts
say otherwise: the T-bills+3 derivation sits in the original gate file in the same
line as the number, written before the first run, and the extension's readout bar
("CAGR ≥ mean(T-bills)+3pp on the extended sample") went into the script before that
run executed. But his structural point lands anyway: a bare number with a
parenthetical rationale is **two bars waiting to diverge**, and the file should never
have allowed both readings. That's now a permanent rule in the gate templates — bars
that derive from a rationale get written as the formula, with which form governs
stated up front. And the stricter reading still had teeth: the sizing basis moved to
the extended numbers, and $0 is funded under either reading. An earlier version of
this paragraph also claimed a 90/10 SPY blend "dominates on every tracked metric" —
round three, below, killed that claim.

**Round three — he broke the blend claim too.** He pointed at the 2008 column: pure
finished −1.5% while SPY did −36.8%, so a 90/10 blend triples the loss in the exact
year that proves the mechanism — and the results file had that column in it the whole
time; the "dominates" sentence just didn't mention it. Then he asked what the frontier
looks like either side of 90/10, so it got run finer, with a crash-flavored metric
(worst rolling 12-month return) added alongside the grind-flavored month-end maxDD:

| Faber/SPY | CAGR | Sharpe | maxDD | 2008 | worst 12m | yrs < T-bills |
|---|---|---|---|---|---|---|
| 100/0 | 5.85% | 0.69 | −12.1% | −1.5% | −9.0% | 5.1 |
| 97.5/2.5 | 6.01% | 0.71 | −11.9% | −2.5% | −8.8% | 3.6 |
| 95/5 | 6.16% | 0.72 | −11.6% | −3.5% | −8.5% | 3.5 |
| 92.5/7.5 | 6.32% | 0.73 | −11.3% | −4.5% | −9.2% | 3.0 |
| 90/10 | 6.47% | 0.75 | −11.6% | −5.5% | −10.3% | 2.9 |
| 85/15 | 6.77% | 0.76 | −13.8% | −7.5% | −12.4% | 2.5 |
| 80/20 | 7.07% | 0.77 | −16.1% | −9.4% | −14.5% | 2.4 |

Verdict: "strictly dominates" is dead. 90/10 loses to pure on 2008 (−5.5% vs −1.5%)
and on worst-12-months (−10.3% vs −9.0%), and on the finer grid it isn't even the
drawdown minimum (92.5/7.5 is) — the signature of a parameter artifact, which is what
he called it: blend weight is a parameter, the frontier is an in-sample scan, and
rules 7 and 8 of this very page apply to their author. What survives, as description
rather than recommendation: the Sharpe rise is a smooth broad hump (0.69 → 0.77, no
knife-edge — ordinary diversification math), and a token 2.5% SPY sleeve cuts the
years-below-T-bills from 5.1 to 3.6, which is his own diagnosis — "it's filling the
exact hole you already told me about" — written as a number. The weight axis trades
crash-year pain against grind-year pain; the frontier describes the dial and cannot
pick the point. Also now on the record, from him: the regime where BOTH legs of the
mechanism fail at once — walking into a bear from zero rates, cash paying nothing,
duration with no room to rally — appears nowhere in this 18-year sample. 2020 was
that state for five weeks and the monthly cadence slid through it; Japan sat in it
for twenty years. The record cannot price that regime. Sizing humility has to.

**Round four — he proposed the test, so it became Gate 26: The Japan Test.** His
design, ran verbatim: you can't manufacture the zero-rate-entry regime from US data,
but you can borrow it — JGBs and the Nikkei through the 90s and 2000s are two decades
of exactly that state. His pass/fail spine became the pre-registered gate: *does the
mechanism survive when both legs are dead on entry, or does it just tread water for
20 years?* Frozen before code, bars as formulas (his rule), domestic-only basket
(Nikkei price index / synthetic 10y JGB total-return from monthly yields / gold in
yen / call-rate cash), frozen Faber 10-month SMA, entry January 1990 — the month
after the bubble top. Governing window: 1999–2012, call rate ≈ 0 the whole way,
10-year JGB at 1.8% on entry. Both legs dead.

**Result: survived — by 0.05 percentage points.** Strategy CAGR 3.17% vs a formula
bar of cash + 3pp = 3.12%. A five-basis-point margin is inside data noise and gets
called what it is: a coin landing on its edge (the known biases lean against the
strategy — the Nikkei sleeve is price-only — so the letter of the bar stands). Full
1990–2012 window: 1.76% vs cash 1.27% — **treads water**, exactly his phrase. And
the eyeball pass found the real finding: equal-weight buy-and-hold of the same three
sleeves beat the trend rule by 1.6pp/yr on the governing window. In a zero-rate
world every whipsaw parks you in an asset that pays nothing — so the trend overlay
SUBTRACTED return and what it bought instead was drawdown: −10.0% max versus the
basket's −15.9% and the Nikkei's −62.8%. The surviving margin came almost entirely
from the one sleeve whose engine doesn't run on interest rates: gold in yen went up
4.9x across the governing window while duration earned its coupon and nothing more.
Multi-asset breadth is what survives ZIRP; the trend rule just decides how much of
it you keep. And the grind sentence is now measured: **15.8 years below cash** on
the full window, with the 1990s — the decade cash still paid 3% — the strategy's
worst stretch against it. His kicker, pre-registered in the gate and now on the
record: in this regime the 20% sizing rule stops being humility and becomes the
actual finding. Prediction audit: I predicted treads-water and was wrong by those
same five basis points. His prediction arrived after the numbers were live on this
page and is logged with that timestamp, because he asked to be held to a bar
publicly: "underperforms buy-and-hold of the same basket" — correct, cleanly
(4.76% vs 3.17%); "treads water or worse against cash+3" — correct on the full
window, five basis points from correct on the governing one. And his reading of
the whole result is now the meaning on record, in his words: **this is not a
return engine that happens to survive bears — it's a drawdown engine that gives
up return in flat regimes to buy survival in violent ones.** It survives
everywhere because it isn't trying to win everywhere. His bar-design point is
adopted going forward too: a regime-transplant gate now bars against the basket's
own buy-and-hold as well as cash+3 — "survived the regime" and "the overlay added
anything" are different questions, and under his bar the overlay's ZIRP verdict
is a clean fail (3.17% vs 4.76%), which is the same sentence as the drawdown-engine
finding. Full receipts: `japan_experiment/` GATE.md → results → VERDICT.md
(the gate file stays frozen; name and prediction live in the verdict).
**Co-author: Zestyclose.**

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

## Gates 27–31: the collector's side of the counter (the sweep that ended the hunt)

Twenty-six gates in, the record had a standing thesis: **your slippage is somebody's
revenue.** Every directional family died at the same friction floor, which means someone
is collecting that floor — market makers collecting spread, venues collecting fees,
institutions collecting structural premiums. So the last five gates stopped predicting
price entirely and went at the collecting side directly: can a retail account stand on
the other side of the counter? One 30-hour sweep, five frozen gates, five kills — and
the same sentence measured five ways: **the payment is always real, and it is always
priced to leave nothing above cash-compensation at retail scale.**

**Gate 27 — cash-merger arbitrage (the operator-vs-machine gate).** The operator
registered an unhedged PASS four days before the run; the machine put it at 65% fail.
The deal database was built from SEC filings announcement-first — 4,963 filings parsed,
789 validated cash deals 2016–2026 — so the broken deals stayed in the sample, and every
break paid its gap in full (IRBT −76%, ROG −61% are in there). The result inverted both
predictions' reasoning: the famous break tail was SURVIVABLE (10% of deals broke, mean
−2.9%, book maxDD −5.5% across ~40 concurrent positions — diversification genuinely
insures it), and the death was the quiet kind: net CAGR 4.03% against a cash+3pp bar of
4.32%, clearing it only at exactly zero cost. A structural premium priced almost exactly
at its own compensation floor. The 2019+ holdout stays sealed — the frozen protocol
doesn't open it on a primary fail. And because the first three sims printed 7%, 27%, and
17% before validity audits tore them down (fictional payouts from bond-delisting Form
25s, acquirers mis-cast as targets, a rebalancing rule harvesting bid-ask bounce as
alpha), the full five-round defect ledger ships in `merger_arb_experiment/DEVIATIONS.md`.
A hostile reader should get the knife with the body.

**Gate 28 — prediction-market cross-venue arbitrage.** Seventeen identical-resolution
Kalshi/Polymarket pairs, 25,231 matched hourly rows. Measured raw, the arb "returns"
+29%/yr — and every fat entry sits inside a market's first 72 hours, where one venue's
price is a bookless print that collapses 30 cents in five hours on no news. No
historical order book exists to verify any of it was executable; a price without a book
is not a quote, so that readout ships as an unverifiable upper bound. The mature-market
number — the one you could actually have — is +2.2%/yr on deployed capital, below the
T-bills the cash would otherwise sit in. The residual gap between venues is real,
persistent, and precisely smaller than the round-trip toll: the visible crumb is what
faster capital left behind. Two venue facts worth the price of the gate: Polymarket now
charges a 5% taker fee on the exact market category measured (the toll booth raised its
price mid-study), and Kalshi purges settled-market price history within weeks — the
2024 election fat-gap regime is unmeasurable backward, forever. If that regime ever
returns, only a live depth logger armed IN ADVANCE can price it.

**Gate 29 — paid order flow / liquidity provision.** Pre-registered form: receipts
only, no backtest — a maker strategy's queue position and adverse selection are
unknowable offline, and simulating them is the same fantasy-fill class this repo exists
to kill. The receipts: at the retail-accessible tier, "collecting the rebate" on US
equities nets MINUS 0.17–0.23 cents per share (best route, published schedules); the
buy-side crossover to positive sits near 3 million shares/month. In crypto, the first
genuinely positive maker payment on any US-accessible venue is −0.02% — at $10M+/30-day
volume. Zero-fee maker tiers now exist at zero volume, and zero is not a payment: at 0%
fee the only revenue left is the spread, and the spread exists because resting orders
lose to informed flow. Selling order flow outright requires broker-dealer registration.
The toll booth has a minimum, and the minimum is roughly $10M a month.

**Gates 30–31 — the momentum coda (the operator's designs, gated and priced).** Asked
to beat the index, the operator proposed rotating everything in one pool — 1,365 stocks
plus 13 cross-asset ETFs, one momentum rank, top 10 — so defensive ETFs would take over
in bears. Pre-registered valve check: in 55 months, including all of 2022, not one ETF
ever out-ranked the ten hottest stocks. The pool drowns the valve by arithmetic, and the
book earned 7.14%/yr with a −41% drawdown against SPY's 11.53%. Meanwhile the boring
S&P momentum index fund the exercise was trying to beat compounded 19.5%/yr for a
decade, live — and the professionally-run CONCENTRATED academic momentum fund did 12.4%,
worse than plain SPY. Every step toward "better" — more concentration, fresher signals,
cleverer math — made it worse, monotonically. Gate 31 then priced the one honest
improvement left, a trend overlay on that index fund, under a drawdown-engine framing
frozen in advance: it cut the max drawdown from −31% to −15% (the insurance works) and
paid 3.4pp/yr for it against a 2pp cap (the insurance is overpriced) — six whipsaw
re-entries, one of them 17.4% above its own exit. Verdict on the record: **sizing beats
switching.** The one keeper this entire record ever surfaced is a 13-basis-point index
product you buy, size to your drawdown budget, and leave alone.

Full gate files, frozen predictions, verdicts, and charts: `merger_arb_experiment/`,
`predmarket_arb_experiment/`, `orderflow_experiment/`, `stock_rotation_experiment/`,
`spmo_overlay_experiment/`.

## Gates 32–35 + the prop-firm study: the wrapper arc

An outside collaborator — Owen Mantz, a prop-firm trader who found this repo —
brought two strategies and one thesis: run a bot on a prop firm's capital
through a challenge account. All of it went through the gate, and the arc is
the record in miniature: the playbook died, the spec produced the record's
second pass, and the wrapper turned out to be priced like everything else.

**The rules study (unnumbered — it gates a wrapper, not a strategy).** A
Monte Carlo of FTMO-class two-step challenge rules, every simplification
trader-favorable on purpose. The published rule sheet is beatable: a
ZERO-edge coin flip reaches funded 1-in-3 and is +EV per attempt, because
100% of the value is the funded-stage payout option — 80% of the positive
part of a random walk on the firm's capital, downside capped at the firm's
line. What the sheet doesn't say: ~2.4 years of daily grinding for ~$3k/yr
at the trader-favorable upper bound, median funded life 126 trading days,
payout-denial discretion, automation clauses. This is why a challenge pass
is not a receipt, and the standing bar for any prop-firm claim here is
funded PAYOUT history over months. Addendum: our own retired day bot's real
90-day trade streams through the same rules — EV −$600 at every leverage
multiple at honest costs, hugely +EV at zero cost. The whole wall in one
table: the signal is real; friction eats it; fantasy fills would have
called it brilliant.

**Gate 32 — his playbook.** "Smart money concepts" M15, mechanized with
literature-default parameters and a binding-scope clause (the mechanization
binds our parameterization, not his discretion; his threshold slot stays
open). PF 0.951 before costs across 7,087 trades, all five years
underwater, and inside the random band. **Gate 33 — his trend spec.** The
surprise, and the reason outside specs are welcome: Faber-class cross-asset
trend, vol-targeted, simulated through eval rulesets on the real 2008–2026
path. Sharpe 0.82 at honest costs; against static-drawdown rules it reaches
funded 78% of the time. Against trailing+consistency rules it fails every
cell — trend profits are lumpy and consistency rules deny exactly that
shape. His own spec predicted the constraint would be the firm choice; the
verdict agreed with him verbatim.

**Gates 34–35 — the transfer tests.** Gate 33's passing cell moved onto
FTMO's actual platform: no bond instrument (−0.05 Sharpe), single-name oil
(free), overnight financing on a months-holding CFD book (0.78 → 0.11 —
the whole kill, the cleanest attribution table in the record). Swap-free
static-drawdown EA-allowed firms exist and restore the passing regime on
their published rules — unless one ambiguous consistency clause applies to
standard payouts, in which case two-thirds of cycles get denied. The build
waits on that sentence in writing. Files: `smc_experiment/`,
`trendprop_experiment/`, `propfirm_experiment/`.

## The kill-side audit: turning the gauntlet on our own tombstones

2026-07-29. The operator asked the question this repo exists to invite:
**"could the graveyard be artifacting kills?"** He was right to ask — the
record's skepticism had only ever been spent against passes (merger arb got
five validity rounds because its early readouts looked too GOOD; no result
ever got extra scrutiny for looking too BAD), and nobody had ever audited a
loser tail. Criteria were frozen before looking — what would count as an
overturned kill — then the top-10 losers of three closed gates were
verified against independent price data, and every failed gate's cost
ladder was collated. Pre-committed: publish whichever way it lands.

- **No kill overturned, none weakened past the frozen thresholds.** ORB:
  0/10 loser flags. Mean reversion: 1/10. Trend-follow: 2/10. Corrections
  move profit factors by less than 0.01 against bars of 1.30.
- **Two fabricated losses found** — the loss-side twins of the fake-split
  wins that started the honest-fill era. BHVN's "−91%": Pfizer acquired
  Biohaven at $148.50 and the ticker passed, with no date gap, to a ~$9
  spinco — the cache splices two companies, and the engine booked an
  acquisition-at-a-premium as a wipeout. CCXI's "−80%": a bar-indexed hold
  jumped a 1,209-day delisting gap onto a relisted successor entity.
  Ticker splices fabricate losses exactly like they fabricate wins. New
  standing rules: gap-scan and splice-screen every cached universe;
  eyeball the top-10 LOSERS with the same ritual as the winners.
- **The kills are less cost-fragile than their own auditor predicted** (the
  auditor's frozen guess: 5–7 cost-conditional kills; actual: one). 10 of
  18 failed gates fail even at ZERO cost. Exactly one kill is cleanly
  cost-conditional — pairs, which passes both its frozen bars at
  0.05%/side/leg. If your execution is materially better than retail
  taker, that is the tombstone to re-run at your own numbers.
- **The disclosure this record owed you:** the kills share infrastructure —
  one cost model (calibrated from this pipeline's measured live slippage,
  always taker execution), one fill simulator, largely one data stack.
  Friction-killed verdicts weaken TOGETHER if the cost model is wrong for
  your execution. They are correlated verdicts, not 29 independent
  confirmations, and every results file publishes its full cost ladder so
  the count you trust is the one you recompute at your own costs.
- Near-miss symmetry, also owed: the record's nearest miss either
  direction is a FAIL (the merger-arb ETF proxy, by 0.03pp), and near-miss
  fails now carry the same "inside data noise" flag the Japan gate's
  5-basis-point PASS always did. Audit notes are appended to the affected
  verdict files; the full audit is in `kill_audit/`.


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

That gauge resolved: the pipeline is now packaged and cleaned up as the
**Falsification Kit**, sold in two tiers at
[strategygraveyard.com/kit.html](https://strategygraveyard.com/kit.html) — The Gauntlet
($54, the runnable tools above) and The Field Manual ($139, plus slippage
instrumentation, risk armor, a simulator smoke-test suite, the decision-shadow logger,
and the full 35-gate written record). This repo — the writeup, the artifact-hunting
checklist, the full kill tables — stays free, forever. Nothing in either tier can
place a trade, and nothing in either tier promises performance; the tools test and
measure only.

Stars and issues still steer what gets extracted or built next — that's the signal I
watch. Questions about the methodology are welcome; I'll answer anything in the issues.
