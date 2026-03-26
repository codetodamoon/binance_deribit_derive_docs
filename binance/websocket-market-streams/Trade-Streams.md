# Trade Streams - Binance Options Trading WebSocket API

## Stream Description

The Trade Streams push raw trade information for a specific symbol or underlying asset.

**URL Path:** `/public`

**Stream Name:** `<symbol>@optionTrade` or `<underlying>@optionTrade`

**Update Speed:** 50ms

**Example URL:**
```
wss://fstream.binance.com/public/stream?streams=btcusdt@optionTrade
```

## Response Parameters

| Field | Type | Description |
|-------|------|-------------|
| `e` | string | Event type |
| `E` | integer | Event time (milliseconds) |
| `T` | integer | Trade completed time (milliseconds) |
| `s` | string | Option trading symbol (e.g., "BTC-251123-126000-C") |
| `t` | integer | Trade ID |
| `p` | string | Price |
| `q` | string | Quantity (always positive) |
| `X` | string | Trade type enum: "MARKET" for Orderbook trading, "BLOCK" for Block trade |
| `S` | string | Direction: "BUY" or "SELL" |
| `m` | boolean | Is the buyer the market maker? |

## Response Example

```json
{
    "e": "trade",
    "E": 1762856064204,
    "T": 1762856064203,
    "s": "BTC-251123-126000-C",
    "t": 4,
    "p": "1300.000",
    "q": "0.1000",
    "X": "MARKET",
    "S": "BUY",
    "m": false
}
```

## Related Streams

- [24 Hour Ticker](/docs/derivatives/options-trading/websocket-market-streams/24-hour-TICKER)
- [Partial Book Depth Streams](/docs/derivatives/options-trading/websocket-market-streams/Partial-Book-Depth-Streams)
- [Connect](/docs/derivatives/options-trading/websocket-market-streams)
- [Live Subscribing/Unsubscribing To Streams](/docs/derivatives/options-trading/websocket-market-streams/Live-Subscribing-Unsubscribing-to-streams)
