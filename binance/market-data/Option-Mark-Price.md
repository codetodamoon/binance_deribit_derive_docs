# Option Mark Price

## API Description

Option mark price and greek info.

## HTTP Request

`GET /eapi/v1/mark`

## Request Weight

**5**

## Request Parameters

| Name   | Type   | Mandatory | Description                              |
|--------|--------|-----------|------------------------------------------|
| symbol | STRING | NO        | Option trading pair, e.g BTC-200730-9000-C |

## Response Example

```json
[
  {
    "symbol": "BTC-200730-9000-C",
    "markPrice": "1343.2883",
    "bidIV": "1.40000077",
    "askIV": "1.50000153",
    "markIV": "1.45000000",
    "delta": "0.55937056",
    "theta": "3739.82509871",
    "gamma": "0.00010969",
    "vega": "978.58874732",
    "highPriceLimit": "1618.241",
    "lowPriceLimit": "1068.3356",
    "riskFreeInterest": "0.1"
  }
]
```

**Field Descriptions:**

- `markPrice`: Mark price
- `bidIV`: Implied volatility Buy
- `askIV`: Implied volatility Sell
- `markIV`: Implied volatility mark
- `delta`: Delta
- `theta`: Theta
- `gamma`: Gamma
- `vega`: Vega
- `highPriceLimit`: Current highest buy price
- `lowPriceLimit`: Current lowest sell price
- `riskFreeInterest`: Risk free rate
