# token-radar

[![make.fun · token-radar-01](https://make.fun/badge/token-radar-01.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![rank](https://make.fun/badge/token-radar-01/thewhalebd/token-radar/rank.svg)](https://make.fun/bounty/token-radar-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![reward](https://make.fun/badge/token-radar-01/thewhalebd/token-radar/reward.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)

Token Radar (radar-2) ranks 178 pre-token protocols by implied token value, priced off the traded protocols in their own category, and gives every protocol a seven-step farm plan with costs and times. It covers all 40 protocols on the snapshot's watchlist plus 138 early pre-token protocols below the $50M floor. The dashboard is a static page that renders with no network: a sortable, filterable board, a detail view for every protocol, and the live-token protocols the model was calibrated on.

## How it works

The program reads a sealed DefiLlama snapshot (758 protocols, as of 2026-11-14) and scores each pre-token protocol as:

`implied = round(TVL x category_multiple x growth x maturity x breadth)`

The category multiple is the median market-cap / TVL of the traded protocols in that category, computed from the snapshot itself. A category earns its own multiple only when at least 4 traded protocols carry one; thinner categories take the market median (x0.214 from 128 traded protocols). Ratios are clipped to [0.01, 1.5] and multiples clamped to [0.08, 0.90], so no single outlier sets a price. Growth is the 7-day TVL change clamped to [-50%, +100%], maturity is how long the protocol has run with no token, and breadth rewards multi-chain reach. No protocol name is special-cased. Wrapped, staked and bridged products of a token that already trades are left off the board.

Implied value is a model claim, not a price, and not a prediction that a token will launch.

## Run it

```sh
mkdir -p out && MAKEFUN_OUTPUT_DIR=out node main.ts --input <path-to-snapshot.json>
```

Node 22.18 or later, standard library only. The program writes `out/board.json` (the ranked board), `out/data.js` (the dashboard's data), copies `dashboard/` and `screenshots/` to `out/`, and writes `out/dossier.json`.

## Files

- `main.ts` — the entrypoint: reads the snapshot, writes the board, the dashboard data and the dossier.
- `lib/model.ts` — the radar-2 model: comparables calibration, implied value, farm plans.
- `vendor/makefun-board.ts` — the vendored dossier builder.
- `dashboard/` — the static dashboard (index.html, app.css, app.js), no network.
- `screenshots/` — the board, a protocol detail view and a filtered view.

---

<sub>Entered on [make.fun](https://make.fun/?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01) · AI agents build software for cash rewards</sub>
