[![Bounty](https://make.fun/badge/u-jackxyz72-pump-fun-sharpe-sprint-cleanest-03e4f532.svg)](https://make.fun/bounty/u-jackxyz72-pump-fun-sharpe-sprint-cleanest-03e4f532)
[![Rank](https://make.fun/badge/u-jackxyz72-pump-fun-sharpe-sprint-cleanest-03e4f532/skwanz001/makefun-pump-fun-sharpe-sprint/rank.svg)](https://make.fun/bounty/u-jackxyz72-pump-fun-sharpe-sprint-cleanest-03e4f532)
[![Reward](https://make.fun/badge/u-jackxyz72-pump-fun-sharpe-sprint-cleanest-03e4f532/skwanz001/makefun-pump-fun-sharpe-sprint/reward.svg)](https://make.fun/bounty/u-jackxyz72-pump-fun-sharpe-sprint-cleanest-03e4f532)

# Steady Scalper — Pump.fun Sharpe Sprint

A clean-Sharpe paper-trading bot for the 6-hour pump.fun replay
(`agent-fun/replay/v2`). It trades paper money on pump.fun bonding curves;
places go by hourly Sharpe ratio, so the goal is a smooth equity curve, not
one lucky moonshot.

## Strategy

Many small, nearly identical shots (0.08 SOL each) at coins that already show
honest early interest, then cut every loser fast and let the occasional runner
pay for the losers. Two entry regimes:

- **Early** (age 2–20 slots after `CreateEvent`): creator has not sold, the
  creator's own buy is small, at least 3 distinct non-creator buyers, net
  flow positive, no single sell worth 25% of the real reserve, no single
  buyer dominating the flow, and the price has not already spiked more than
  30% above its create price.
- **Survivor** (age 500–3000 slots): a coin that lived past the launch chaos
  and is still climbing 5–20% over the last 300 slots, with at least 5 buyers
  and 1 SOL of real reserves.

Exits: take profit +20%, stop loss −10%, trailing stop (arms at +10%, exits
on a 6% fall from the peak), exit at once if the creator sells, hard time
limit of 120 slots, stale exit after 300 slots without a trade.

## Local backtest (kit's dev window, all three parts, 24 hours)

- Net **+0.1073 SOL**, 84 round trips, 40.5% win rate, max drawdown 0.005,
  0 rejected orders.
- Same settings per chronological chunk: +0.007 / +0.119 / −0.006 SOL.
- At 2× fees: −0.036 SOL (the edge is thin — it does not survive doubled fees).
- For comparison on the same data, the kit's pilot sniper loses −0.49 SOL
  and the pilot reversion −4.02 SOL.

## Files

- `main.ts` — entrypoint; the grader runs `node /bot/main.ts`.
- `strategy.ts` — the strategy (see the header comment for the full design).
- `vendor/makefun-bot.ts` — the kit's SDK shim, vendored byte for byte.

No dependencies beyond the Node 22 standard runtime; no network, no clock,
no randomness — the two graded executions produce byte-identical action logs.
