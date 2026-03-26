# Order Book

## API Description

Check orderbook depth on specific symbol

## HTTP Request

```
GET /eapi/v1/depth
```

## Request Weight

| limit | weight |
|-------|--------|
| 5, 10, 20, 50 | 1 |
| 100 | 5 |
| 500 | 10 |
| 1000 | 20 |

## Request Parameters

| Name | Type | Mandatory | Description |
|------|------|-----------|-------------|
| symbol | STRING | YES | Option trading pair, e.g BTC-200730-9000-C |
| limit | INT | NO | Default: 100 Max: 1000. Optional value: [10, 20, 50, 100, 500, 1000] |

## Response Example

```json
{
    "bids": [
        [
            "1000.000",
            "0.1000"
        ]
    ],
    "asks": [
        [
            "1900.000",
            "0.1000"
        ]
    ],
    "T": 1762780909676,
    "lastUpdateId": 361
}
```

### Response Fields

| Field | Description |
|-------|-------------|
| bids | Buy order array |
| asks | Sell order array |
| Price | First element in each order array |
| Quantity | Second element in each order array |
| T | Transaction time (milliseconds) |
| lastUpdateId | Update ID |
