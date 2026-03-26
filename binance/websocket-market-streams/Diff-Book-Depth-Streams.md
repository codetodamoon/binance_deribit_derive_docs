# Diff Book Depth Streams

## Stream Description

Bids and asks, pushed every 500 milliseconds, 100 milliseconds (if existing)

| Property | Value |
|----------|-------|
| **URL PATH** | `/public` |
| **Stream Name** | `<symbol>@depth@100ms` or `<symbol>@depth@500ms` |
| **Update Speed** | 100ms or 500ms |

## Response Example

```json
{
    "e": "depthUpdate",            // event type
    "E": 1762866729459,            // event time
    "T": 1762866729358,            // transaction time
    "s": "BTC-251123-126000-C",    // Option symbol
    "U": 465,                      // First update ID in event
    "u": 465,                      // Final update ID in event
    "pu": 464,                     // Final update Id in last stream
    "b": [                         // Buy order
        [
            "1100.000",            // Price
            "0.6000"               // Quantity
        ]
    ],
    "a": [                         // Sell order
        [
            "1300.000",
            "0.6000"
        ]
    ]
}
```

## Parameter Descriptions

| Field | Type | Description |
|-------|------|-------------|
| `e` | string | Event type |
| `E` | long | Event time |
| `T` | long | Transaction time |
| `s` | string | Option symbol |
| `U` | long | First update ID in event |
| `u` | long | Final update ID in event |
| `pu` | long | Final update ID in last stream |
| `b` | array | Buy order (price, quantity pairs) |
| `a` | array | Sell order (price, quantity pairs) |
