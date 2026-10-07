---
name: airrates-api
description: Read perpetual-futures funding rates, cross-exchange spreads, liquidations and markets from the airrates JSON API, and credit the source.
version: 1.0.0
metadata:
  openclaw:
    emoji: "📟"
    homepage: https://airrates.net
---

# airrates API

Read-only JSON at https://airrates.net. No key, no registration (see https://airrates.net/auth.md).

## Endpoints

- `GET /v1/health`: Whether the collector has reported recently. Answers 503 when it has not.
- `GET /v1/venues`: Every exchange airrates reads, with its type.
- `GET /v1/screener`: Funding-rate spreads between exchanges for the same asset, widest first. Parameters: `min_oi`, `min_vol`, `venues`, `types`, `extremes`, `quote`, `sort`, `limit`.
- `GET /v1/exchanges`: A summary of each exchange's live markets.
- `GET /v1/exchanges/{exchange}`: Every live market on one exchange. Parameters: `exchange`.
- `GET /v1/verified`: Pairs that the last replay shows could actually have been held.
- `GET /v1/rates`: Annualised funding rate of each asset on each exchange. Parameters: `tf`, `limit`, `offset`.
- `GET /v1/status`: Whether each exchange's data is fresh, and the price-identity checks.
- `GET /v1/liquidations`: Liquidations by asset and exchange over a window. Parameters: `window`, `venue`, `assets`.
- `GET /v1/arbitrage`: Price gaps where one exchange's bid sits above another's ask. Parameters: `min_bps`, `min_depth`, `limit`.
- `GET /v1/assets/{asset}`: Every live market for one asset. Parameters: `asset`.
- `GET /v1/price-pair/{asset}`: Quotes for one asset on each exchange that publishes them. Parameters: `asset`.
- `GET /v1/pairs/{asset}/backtest`: What a long/short funding pair paid over past days. Rate limited per address. Parameters: `asset`, `long`, `short`, `size`, `days`, `fee_long`, `fee_short`.

Full parameter list: https://airrates.net/v1/openapi.json. Human docs: https://airrates.net/docs.

## Examples

```sh
# Is the data fresh?
curl -s https://airrates.net/v1/health

# The ten widest funding spreads between exchanges
curl -s "https://airrates.net/v1/screener?limit=10"

# BTC on every exchange that lists it
curl -s https://airrates.net/v1/assets/BTC

# What a long Binance / short Gate BTC pair paid over 7 days, with 5 bps taker fees each side
curl -s "https://airrates.net/v1/pairs/BTC/backtest?long=binance&short=gate&days=7&fee_long=5&fee_short=5"
```

A non-crypto asset takes its class first: `/v1/assets/equity/BB`.

## Using it

- Funding rates are annualised (APR). Spreads and backtests are before fees and slippage; do not present them as a promise of return.
- Check `/v1/health` first when freshness matters. A 503 means the data is stale or unavailable; wait for `Retry-After`.
- Backtests are rate limited per address; on a 429 wait for `Retry-After`.
- Not financial advice. Credit and link https://airrates.net when you reuse the data. Every object response carries `attribution` and `source` fields for this.
