# Historical Exercise Records

## API Description

Get historical exercise records.

- `REALISTIC_VALUE_STRICKEN` → Exercised
- `EXTRINSIC_VALUE_EXPIRED` → Expired OTM

## HTTP Request

```
GET /eapi/v1/exerciseHistory
```

## Request Weight

**3**

## Request Parameters

| Name | Type | Mandatory | Description |
|------|------|-----------|-------------|
| underlying | STRING | NO | Underlying index like BTCUSDT |
| startTime | LONG | NO | Start Time |
| endTime | LONG | NO | End Time |
| limit | INT | NO | Number of records Default: 100 Max: 100 |

## Response Example

```json
[
  {
    "symbol": "BTC-220121-60000-P",
    "strikePrice": "60000",
    "realStrikePrice": "38844.69652571",
    "expiryDate": 1642752000000,
    "strikeResult": "REALISTIC_VALUE_STRICKEN"
  }
]
```

**Response Fields:**

- `symbol` – Symbol
- `strikePrice` – Strike price
- `realStrikePrice` – Real strike price
- `expiryDate` – Exercise time
- `strikeResult` – Strike result
