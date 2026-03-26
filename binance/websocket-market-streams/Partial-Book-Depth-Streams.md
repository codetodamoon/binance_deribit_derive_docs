# Partial Book Depth Streams

## Stream Description

Top **<levels>** bids and asks, Valid levels are **<levels>** are 5, 10, 20.

## URL PATH

`/public`

## Stream Name

`<symbol>@depth<level>@100ms` or `<symbol>@depth<level>@500ms`

## Update Speed

**100ms** or **500ms**

## Response Example

```json
{
    "e": "depthUpdate",
    "E": 1762866729459,
    "T": 1762866729358,
    "s": "BTC-251123-126000-C",
    "U": 465,
    "u": 465,
    "pu": 464,
    "b": [
        [
            "1100.000",
            "0.6000"
        ]
    ],
    "a": [
        [
            "1300.000",
            "0.6000"
        ]
    ]
}
```

**Response Parameters:**

| Field | Type | Description |
|-------|------|-------------|
| e | string | Event type |
| E | long | Event time |
| T | long | Transaction time |
| s | string | Option symbol |
| U | long | First update ID in event |
| u | long | Final update ID in event |
| pu | long | Final update ID in last stream |
| b | array | Buy order (price, quantity) |
| a | array | Sell order (price, quantity) |
