# Index Price Streams

## Stream Description

Underlying (e.g., ETHUSDT) index stream.

## URL PATH

`/market`

**Stream Name:** `!index@arr`

## Update Speed

1000ms

## Response Example

```json
[
    {
        "e": "indexPrice",
        "E": 1763092572229,
        "s": "ETHUSDT",
        "p": "3224.51976744"
    },
    {
        "e": "indexPrice",
        "E": 1763092572229,
        "s": "BTCUSDT",
        "p": "99102.32326087"
    }
]
```

## Parameter Descriptions

| Parameter | Type   | Description              |
|-----------|--------|--------------------------|
| e         | string | Event type               |
| E         | long   | Time                     |
| s         | string | Underlying symbol        |
| p         | string | Index price              |

## Subscribe/Unsubscribe Format

To subscribe to this stream, send:

```json
{
    "method": "SUBSCRIBE",
    "params": ["!index@arr"],
    "id": 1
}
```

To unsubscribe, send:

```json
{
    "method": "UNSUBSCRIBE",
    "params": ["!index@arr"],
    "id": 1
}
```
