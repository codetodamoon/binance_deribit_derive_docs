# Open Interest

## Stream Description

Option open interest for specific underlying asset on specific expiration date.

Example: `ethusdt@openInterest@221125`

## URL PATH

```
/market
```

## Stream Name

```
underlying@optionOpenInterest@<expirationDate>
```

## Update Speed

```
60s
```

## Response Example

```json
[
    {
        "e": "openInterest",
        "E": 1668759300045,
        "s": "ETH-221125-2700-C",
        "o": "1580.87",
        "h": "1912992.178168204"
    }
]
```

## Response Parameters

| Field | Type | Description |
|-------|------|-------------|
| e | string | Event type |
| E | integer | Event time |
| s | string | Option symbol |
| o | string | Open interest in contracts |
| h | string | Open interest in USDT |

## Subscribe/Unsubscribe Format

```
wss://fstream.binance.com/market/stream?streams=ethusdt@openInterest@221125
```

**Note:** Multiple streams can be combined using the format: `stream1@streamName/stream2@streamName`
