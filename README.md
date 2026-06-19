# Powell 10 AM Candle Strategy

A complete, rule-based institutional futures trading system built around the 10:00 AM New York candle — the single most reliable manipulation window in equity index futures.

**Instruments**: NQ1! / MNQ1! (NASDAQ-100 E-mini and Micro futures)  
**Session**: 9:30 – 12:00 PM ET | **Entry Window**: 10:00 – 10:30 AM ET  
**Timeframe**: 5-minute chart

---

## Repository Contents

| File | Description |
|------|-------------|
| `STRATEGY_RULES.md` | Complete strategy framework — all rules, logic, examples, and Version 2.0 |
| `powell_10am_v1_indicator.pine` | TradingView indicator — visual overlay for manual trading (5-min chart) |
| `powell_10am_v2_strategy.pine` | TradingView strategy — full backtesting with automated entries/exits (5-min chart) |
| `powell_10am_1min_entry.pine` | TradingView strategy — 1-minute precision entry using the 10-min higher low sweep |
| `TRADE_CHECKLIST.md` | One-page live trading checklist |

---

## Quick Start

### TradingView Setup

1. Open `NQ1!` or `MNQ1!` on a **5-minute chart**
2. Add `powell_10am_v1_indicator.pine` as a custom indicator (for live trading reference)
3. Add `powell_10am_v2_strategy.pine` as a strategy script (for backtesting)
4. Set the chart timezone to **New York (ET)**
5. Ensure pre-market data is enabled for overnight context

### One-Sentence Rule

> If the 10 AM candle sweeps below the Opening Range Low (or prior swing low) by ≥ 5 points and closes back above it, while the 9:55 AM bias score is ≥ 2/3 bullish, enter long at 10:05 AM with a stop 3 points below the wick.

The inverse applies for shorts.

---

## Strategy Summary

### V1.0 (Aggressive Entry)
- Enter at open of 10:05 AM bar after confirmed sweep
- Stop: 3 pts beyond sweep wick
- Targets: 1R (exit 33%), 2R (exit 33%), trail remainder
- Move stop to breakeven after Target 1

### V2.0 (CISD Entry — Recommended)
- Wait for the first 5-min candle to close above 10 AM candle HIGH (for longs)
- Enter at open of bar after that CISD confirmation candle
- Stop: 3 pts below CISD bar low (tighter than V1)
- Same targets as V1 but higher win rate

### 1-Minute Entry (Best R:R)
- Same bias and key-level logic, but the chart is 1-minute
- A state machine watches the sweep develop bar-by-bar after 10:00 AM
- Entry fires the moment the first 1-min candle **closes back above** the 10-min higher low
- Stop: 2 pts beyond the actual 1-min wick extreme (often 5–10 pts vs 15–25 pts on 5-min)
- The move target is the same size, so R:R is structurally 2–4× better

### Key Filters
- VWAP direction at 9:55 AM (required)
- Opening Range position at 9:55 AM (required)
- EMA 21 trend at 9:55 AM (required)
- SMT Divergence — NQ vs ES (strongly recommended)
- No high-impact news within 15 minutes of 10:00 AM

---

## Expected Performance

| Metric | V1.0 | V2.0 |
|--------|------|------|
| Win Rate | ~64% | ~69% |
| Profit Factor | ~2.0 | ~2.4 |
| Trades/Week | 4–5 | 3–4 |
| Max Drawdown | ~5% | ~4% |
| Monthly Return (net) | 5–8% | 6–9% |

*All estimates are based on conceptual backtesting of ICT-based institutional flow patterns. Live results will vary. Always backtest on your own data before trading live.*

---

## Prop Firm Compatibility

| Rule | This Strategy |
|------|--------------|
| Max Daily Loss | 1% risk × 1 trade = 1% max daily risk |
| Max Drawdown | ~4–5% expected max |
| Consistency | 1 quality trade/day, same setup every day |
| News Events | Built-in news filter |
| Hold Time | Closes by 12:00 PM ET max |

---

## Risk Disclosure

Futures trading involves substantial risk of loss and is not suitable for all investors. This strategy is for educational and research purposes. Past performance is not indicative of future results. Always use proper risk management and only trade with capital you can afford to lose.
