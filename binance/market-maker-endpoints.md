# Options Trading - Market Maker Endpoints

## Get Market Maker Protection Config (TRADE)

## API Description

Get config for MMP.

## HTTP Request

```
GET /eapi/v1/mmp (HMAC SHA256)
```

## Request Weight

**1**

## Request Parameters

| Name | Type | Mandatory | Description |
|------|------|-----------|-------------|
| underlying | STRING | TRUE | underlying, e.g BTCUSDT |
| recvWindow | LONG | NO | timestamp |
| timestamp | LONG | YES | |

## Response Example

```json
{
    "underlyingId": 2,
    "underlying": "BTCUSDT",
    "windowTimeInMilliseconds": 3000,
    "frozenTimeInMilliseconds": 300000,
    "qtyLimit": "2",
    "deltaLimit": "2.3",
    "lastTriggerTime": 0
}
```
