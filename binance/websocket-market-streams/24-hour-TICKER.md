# 24 Hour Ticker

## Stream Description

24hr ticker info for all symbols. Only symbols whose ticker info changed will be sent.

## URL PATH

`/public`

## Stream Name

`<symbol>@optionTicker` or `<underlying>@optionTicker@<expiretionDate>`

Example: `btcusdt@optionTicker@251230`

## Update Speed

**1000ms**

## Response Example

```json
{
    "e": "24hrTicker",
    "E": 1764080707933,
    "s": "ETH-251226-3000-C",
    "p": "0.0000",
    "P": "0.00",
    "w": "200.0000",
    "c": "200.0000",
    "Q": "1.0000",
    "o": "200.0000",
    "h": "200.0000",
    "l": "200.0000",
    "v": "9.0000",
    "q": "1800.0000",
    "O": 1764051060000,
    "C": 1764080707933,
    "F": 1,
    "L": 22,
    "n": 9
}
```

## Field Descriptions

| Field | Description |
|-------|-------------|
| e | Event type |
| E | Event time |
| s | Symbol |
| p | Price change |
| P | Price change percent |
| w | Weighted average price |
| c | Last price |
| Q | Last quantity |
| o | Open price |
| h | High price |
| l | Low price |
| v | Trading volume (in contracts) |
| q | Trade amount (in quote asset) |
| O | Statistics open time |
| C | Statistics close time |
| F | First trade ID |
| L | Last trade ID |
| n | Total number of trades |
