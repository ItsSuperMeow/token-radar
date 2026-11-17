# Token Radar

[![make.fun · token-radar-01](https://make.fun/badge/token-radar-01.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![rank](https://make.fun/badge/token-radar-01/ItsSuperMeow/token-radar/rank.svg)](https://make.fun/bounty/token-radar-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![reward](https://make.fun/badge/token-radar-01/ItsSuperMeow/token-radar/reward.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)

A token launch radar: a dashboard that ranks pre-token DeFi protocols by an
implied value and shows how to farm each one.

Built for the [make.fun token-radar-01 bounty](https://make.fun/bounty/token-radar-01)
($1,325). The entry is `main.ts` at the root: it reads the sealed snapshot at
`/input/snapshot.json`, writes the ranked board to `/output/board.json`, and
copies `dashboard/` and `screenshots/` to `/output` with a `dossier.json` the
judge reads.

## The model (radar-2)

Implied value follows only from the snapshot's own figures:

```
implied = round( tvl_usd x category multiple
                 x (1 + clamp(change_7d, -60, 120) / 100 x 0.5)
                 x (0.6 + min(age_days, 1095) / 1095 x 0.4) )
```

The **category multiple** is the heart of it: for every category, the median
market-cap-to-TVL ratio of the protocols in that category whose token already
trades, computed from this snapshot. A pre-token protocol is worth its TVL
times what the market already pays for a dollar of TVL in its own kind of
protocol. A category with fewer than 3 trading peers falls back to the
snapshot-wide median multiple. No protocol name is special-cased; the same
function scores every row.

The board covers all 40 watchlist protocols and adds 186 pre-token protocols
below the $50M floor, for 226 rows. Protocols whose token already trades are
kept apart in the dashboard's Live view at their market cap — a fact, not a
model — and their ratios are what price the board.

## Run it

```sh
mkdir -p out && MAKEFUN_OUTPUT_DIR=out node main.ts --input <snapshot.json|directory>
```

Node 22.18+ (standard library only, no install). The dashboard renders
offline: `dashboard/index.html` reads `data.js` beside it.

## License

MIT. See LICENSE.
