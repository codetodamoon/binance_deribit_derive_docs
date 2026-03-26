# Options Trading - Account

## Option Margin Account Information

## API Description

Get current account information.

## HTTP Request

```
GET /eapi/v1/marginAccount
```

## Request Weight

**3**

## Request Parameters

| Name | Type | Mandatory | Description |
|------|------|-----------|-------------|
| recvWindow | LONG | NO | |
| timestamp | LONG | YES | |

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

## Response Parameters Description

| Field | Type | Description |
|-------|------|-------------|
| asset | array | Asset information array |
| asset.asset | string | Asset type |
| asset.marginBalance | string | Account balance |
| asset.equity | string | Account equity |
| asset.available | string | Available funds |
| asset.initialMargin | string | Initial margin |
| asset.maintMargin | string | Maintenance margin |
| asset.unrealizedPNL | string | Unrealized profit/loss |
| asset.adjustedEquity | string | Margin balance + qualified Long Position Value |
| greek | array | Greek data array |
| greek.underlying | string | Option Underlying |
| greek.delta | string | Account delta |
| greek.theta | string | Account theta |
| greek.gamma | string | Account gamma |
| greek.vega | string | Account vega |
| time | long | Timestamp |
| canTrade | boolean | Whether trading is enabled |
| canDeposit | boolean | Whether deposit is enabled |
| canWithdraw | boolean | Whether withdrawal is enabled |
| reduceOnly | boolean | Reduce only status |
| tradeGroupId | integer | Trade group ID |
