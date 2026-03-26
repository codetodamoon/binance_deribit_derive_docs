# Option Margin Account Information

Get current account information.

**HTTP Request:** `GET /eapi/v1/marginAccount`

**Request Weight:** 3

## Request Parameters

| Name | Type | Mandatory | Description |
|------|------|-----------|-------------|
| recvWindow | LONG | NO | The value cannot exceed 60000 |
| timestamp | LONG | YES | Current timestamp in milliseconds |

## Response Example

```json
{
    "asset": [
        {
            "asset": "USDT",
            "marginBalance": "99998.87365244",
            "equity": "99998.87365244",
            "available": "96883.72734374",
            "initialMargin": "3115.14630870",
            "maintMargin": "0.00000000",
            "unrealizedPNL": "0.00000000",
            "adjustedEquity": "99998.87365244"
        }
    ],
    "greek": [
        {
            "underlying": "BTCUSDT",
            "delta": "0",
            "theta": "0",
            "gamma": "0",
            "vega": "0"
        }
    ],
    "time": 1762843368098,
    "canTrade": true,
    "canDeposit": true,
    "canWithdraw": true,
    "reduceOnly": false,
    "tradeGroupId": -1
}
```

## Response Fields Description

| Field | Description |
|-------|-------------|
| asset | Asset type |
| marginBalance | Account balance |
| equity | Account equity |
| available | Available funds |
| initialMargin | Initial margin |
| maintMargin | Maintenance margin |
| unrealizedPNL | Unrealized profit/loss |
| adjustedEquity | margin balance + qualified Long Position Value |
| underlying | Option Underlying |
| delta | Account delta |
| theta | Account theta |
| gamma | Account gamma |
| vega | Account vega |
