# Symbol Price Ticker

## Index Price

### API Description

Get spot index price for option underlying.

### HTTP Request

```
GET /eapi/v1/index
```

### Request Weight

**1**

### Request Parameters

| Name | Type | Mandatory | Description |
|------|------|-----------|-------------|
| underlying | STRING | YES | Spot pair (Option contract underlying asset, e.g BTCUSDT) |

### Response Example

```json
{
  "time": 1656647305000,
  "indexPrice": "105917.75"
}
```

*Note: "Current index price" is the value returned in the `indexPrice` field.*
