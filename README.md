# The Trading Strategy Graveyard

**16 pre-registered backtests of retail trading strategies. Zero passed. This is the full
record — every strategy, every number, every artifact that almost fooled me.**

Two years of YouTube and fintwit will tell you momentum scalping works, gap trading works,
opening-range breakouts work, mean reversion works. I built a testing pipeline to find out
which one I should automate, and committed to one rule that changed everything: **write the
pass/fail criteria down BEFORE running the test, and never move the goalposts after.**

16 pre-registered gates later, the answer is: none of them. Not one strategy family
survived honest testing. I lost $0 learning this — every strategy died in the pipeline
before touching money.

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
   printed PF 3.96 by accidentally cherry-picking under 10% of trades. It did it twice, on
   two unrelated strategies.

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

The pattern across the whole table: **every price-derived signal converges to roughly
PF 1.05–1.10 gross** — a real, detectable asymmetry — **and retail friction (0.10–0.25%
per side) eats it whole, every time.** Buy-strength and buy-weakness. Long and short.
Intraday and multi-day. Doesn't matter.

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

I'm gauging whether it's worth cleaning these up for release. If you'd use them, **star
the repo or open an issue** saying what you'd want — that's the signal I'm watching.

Questions about the methodology are welcome; I'll answer anything in the issues.
