# Mark Price - WebSocket Market Streams

## Stream Description

The mark price for all option symbols on specific underlying asset.

**Example:** `btcusdt@optionMarkPrice`

**WebSocket URL:** `wss://fstream.binance.com/market/stream?streams=btcusdt@optionMarkPrice`

## Technical Details

| Parameter | Value |
|-----------|-------|
| URL PATH | `/market` |
| Stream Name | `<underlying>@optionMarkPrice` |
| Update Speed | 1000ms |

## Response Fields

| Field | Description |
|-------|-------------|
| `s` | Symbol |
| `mp` | Mark price |
| `E` | Event time |
| `e` | Event type |
| `i` | Index price |
| `P` | Estimated Settle Price (useful in 0.5 hour before settlement) |
| `bo` | Best buy price |
| `ao` | Best sell price |
| `bq` | Best buy quantity |
| `aq` | Best sell quantity |
| `b` | Buy implied volatility |
| `a` | Sell implied volatility |
| `hl` | Buy maximum price |
| `ll` | Sell minimum price |
| `vo` | Volatility |
| `rf` | Risk free rate |
| `d` | Delta |
| `t` | Theta |
| `g` | Gamma |
| `v` | Vega |

## Related Documentation

- Previous: [Open Interest](/docs/derivatives/options-trading/websocket-market-streams/Open-Interest)
- Next: [Kline Candlestick Streams](/docs/derivatives/options-trading/websocket-market-streams/Kline-Candlestick-Streams)
