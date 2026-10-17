You are picking up the Rogue One (R1) work from a separate research session. Below is what was tested and decided. Use it to update the R1 tracker (settings, labels, review rules, expected-performance benchmarks) and to continue analysis when I send new data. Treat the numbers as backtest benchmarks, not guarantees. Ask me before changing anything that isn't listed here.

## Strategy recap
R1 = SPX 0DTE credit spreads triggered by an expected-move (ATM straddle) breach, based on the Trade Busters "Rogue One" signal. High-side breach → sell a put spread; low-side breach → sell a call spread. Template: $3.25 credit, 100 wide, stop measured on the short leg ("Double Bid" trigger). Executed through TAT on IBKR. Backtests come from BYOB using the "R1 Signals" (first signal per side) and "R1 Signals Multi" (every signal) data sets.

## DECISIONS (new R1 configuration)
Old setup (being retired): R1 Break High / R1 Break Low, 10:00–15:00 window, first signal per side, 1x stop.
New setup:
- Entry window: 10:00–13:00 ET (no entries after 1:00pm)
- Signals: ALL signals in the window ("R1 Signals Multi"), both sides
- Stop: 1.5x (raised from 1x)
- Credit $3.25 and width 100: unchanged for now (75-wide is pending, see below)
- Max executions: cap at 3–4 per side per day. This is for buying power only; the cap barely changes the results.
- Size: 1 contract until the live validation below passes.
- In the tracker, tag new-config trades separately from the old "R1 Break High/Low" trades so results never mix.

## Why (BYOB backtests, per 1 spread, after BYOB slippage and fees)
- Stop sweep, full window, first signal, May 2022–Oct 2026: 1x $26.8/trade, 1.25x $27.4, 1.5x $32.4, 2x $28.0, 3x $30.5. Expectancy is flat across stops; none of the gaps were statistically significant. Wider stops raise tail risk sharply: max drawdown 1x −$3.9k, 2x −$9.7k, 3x −$10.9k. Holding stopped trades to expiry looked good on the live data (76% recovered), but over the full history the extra losses from wider stops offset the recoveries.
- Time window: afternoon first-signal trades (after 1pm) faded every year (1x: $108 → $54 → $19 → $13 → −$26 per trade, 2022→2026). Since 2024 they made about $7/trade (95% CI −$26 to +$36), added no diversification, and lowered Sharpe. VIX level did not rescue them. Morning trades had no downward trend.
- First signal only, 10:00–1:00 at 1.5x: $33.2/trade, positive every year (39/34/39/23/34), max drawdown −$4.3k.
- Multi, 10:00–1:00 at 1.5x (May 2022+): 2,202 trades, $31.8/trade, $70k total vs $46k for first-signal only, max drawdown −$7.4k, worst day −$2.27k. 2nd and later signals averaged $28.7/trade and were positive every year (also $48 in 2020–May 2022). 3rd and later signals are noisier. Not re-entering after a same-side stop showed no consistent effect.
- 1.5x was also the best stop level in Multi (1x $25.3, 2x $24.7).
- Live (May–Oct 2026, old config, 177 trades) roughly matched BYOB (live about $10/trade vs BYOB 2026 $6), and stop fills were about 1.03x the target. The backtest is credible.

## Expected performance benchmarks (new config, 1 contract)
- About $13.4k/yr backtest (expect lower live; plan on about $20–25/trade instead of $32)
- 1-yr Monte Carlo: median drawdown about $4k; 95th pct about $7.5k; 99th pct about $10k; P(losing year) about 2–3%
- Historical max drawdown 2020–2026 about −$7.2k; worst day about −$2.3k (4–5 stops). Expect about 8–9 days/yr with 3+ stops.
- Peak buying power per day (cap 3/side): median about $19k, 95th pct about $29k, max about $58k

## Review rules (set in advance — apply them as written)
- After about 150 live trades on the new config (about 3 months): pause and re-evaluate if the average is below $0/trade, OR if drawdown at any point exceeds about $11k per contract (1.5x the backtest's worst).
- Scaling gate: stay at 1 contract until 150+ live trades show (a) average ≥ about $15/trade, (b) drawdown within about $11k, (c) stop fills close to BYOB.
- Afternoon signals (1–3pm): track them as a paper/BYOB series only. Revisit if they average clearly positive over the next 80–100 signals.

## Scaling rule (once the gate passes)
- Contracts = floor(equity ÷ capital per contract), recalculated monthly. Size down by the same rule.
- Capital per contract is set by my drawdown tolerance: 20% → $75k, 25% → $60k, 30% → $50k (based on the 99th-pct 1-yr drawdown of $10k × 1.5 live haircut ÷ tolerance). I haven't chosen a tolerance yet — ask me. Never go below $50k per contract.
- Buying-power check: at least about $30–40k per contract so busy days don't block signals.

## Portfolio context (R1 runs alongside MEIC and an ATM iron fly)
- MEIC: Fridays only (switched in Jul 2026). An OptionOmega rebuild (2022–2026) confirmed Friday was the best weekday in 4 of 5 years (about $513/day vs about $181 for other days; permutation p≈0.001). Decision: narrow MEIC wings from 100/100 to 75/75 (about −1% profit, about −27% margin, same every year). Don't go to 50 (about −9%).
- ATM iron fly (3:56pm, no stop): keep the current wing rule ("35–45 wide or first long at $0.05").
- Full portfolio 2022–2026 backtest (1 lot each): new R1 vs old R1 → annual P/L $54.2k vs $50.0k, Sharpe 3.94 vs 4.25, max drawdown −$6.7k vs −$5.5k. Roughly a fair trade over the full history, but strongly better recently (2026 YTD: R1 new about $17.7k vs old $4.3k). Decision: switch.
- New R1 correlates more with MEIC on Fridays (0.29 vs 0.12 for old R1), so bad Fridays now hit both. Watch combined Friday risk and buying power.
- Note: the MEIC backtest in my odte-portfolio app does NOT match my live MEIC. It trades every day with a wider (≈2x-credit loss) stop; live is Friday-only with a 1x-credit-loss stop. Don't use that backtest for MEIC risk until it's corrected.

## Cost model used for portfolio comparisons
R1: $3.30 commission per spread, $0.05 entry slippage, $0.05 exit slippage on stops (from my odte-portfolio settings). BYOB's own model (fees $1.50/$3.00, stop slippage $0.25) is heavier and was used as a sensitivity check; conclusions held under both.

## Pending / next analysis
1. BYOB export: Multi 10:00–1:00, 1.5x stop at 75-wide and 50-wide. Expectation: 75 is nearly free and saves about $2.5k margin per spread; 50 is noticeably worse. Decide after seeing it.
2. Once about 150 live new-config trades exist: compare backtest vs live (fills, stop-trigger behavior, expectancy) and apply the review rules.
3. Periodically rerun the BYOB 1–3pm window to monitor the afternoon edge.
4. Help me pick a drawdown tolerance, then build the equity thresholds for contracts 2, 3, 4…

## How to analyze new BYOB exports
BYOB CSV columns include EntryTime, OptionType, ShortStrike, Premium, Outcome, ProfitLoss, ProfitLossAfterSlippage, CommissionFees, OpenDate, OpenTime, CloseTime, VIX.
- Per-trade net = ProfitLossAfterSlippage × 100 − CommissionFees.
- Match trades across runs by (EntryTime, OptionType, ShortStrike).
- Signal order: count trades per (OpenDate, OptionType) in time order.
- Judge changes by per-trade expectancy with day-block bootstrap CIs, consistency by year, max drawdown, worst day, and whether neighboring settings agree. Don't judge on total P/L alone.
