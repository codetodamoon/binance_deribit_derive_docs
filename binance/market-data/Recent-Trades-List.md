# Recent Trades List

## API Description

Get recent market trades

## HTTP Request

```
GET /eapi/v1/trades
```

## Request Weight

**5**

## Request Parameters

| Name | Type | Mandatory | Description |
|------|------|-----------|-------------|
| symbol | STRING | YES | Option trading pair, e.g BTC-200730-9000-C |
| limit | INT | NO | Number of records Default:100 Max:500 |

## Response Example

```json
[
    {
        "id": 2323857420768529130,
        "tradeId": 1,
        "symbol": "BTC-251123-126000-C",
        "price": "1300",
        "qty": "0.1",
        "quoteQty": "130",
        "side": -1,
        "time": 1762780453623
    }
]
```

**Field descriptions:**

- `id`: Trade ID
- `tradeId`: Trade ID
- `symbol`: Completed trade symbol
- `price`: Completed trade price
- `qty`: Completed trade quantity
- `quoteQty`: Completed trade amount
- `side`: Completed trade direction (-1 Sell, 1 Buy)
- `time`: Time in milliseconds
