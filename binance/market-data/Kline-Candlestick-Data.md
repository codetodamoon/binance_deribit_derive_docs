# Kline Candlestick Data | Binance Open Platform

## Kline/Candlestick Data

### API Description

Kline/candlestick bars for an option symbol. Klines are uniquely identified by their open time.

### HTTP Request

```
GET /eapi/v1/klines
```

### Request Weight

**1**

### Request Parameters

| Name | Type | Mandatory | Description |
|------|------|-----------|-------------|
| symbol | STRING | YES | Option trading pair, e.g BTC-200730-9000-C |
| interval | STRING | YES | Time interval |
| startTime | LONG | NO | Start Time 1592317127349 |
| endTime | LONG | NO | End Time |
| limit | INT | NO | Number of records Default:500 Max:1500 |

> * If startTime and endTime are not sent, the most recent klines are returned.

### Response Example

```json
[
    [
        1762779600000,      // Open time
        "1300.000",         // Open
        "1300.000",         // High
        "1300.000",         // Low
        "1300.000",         // Close
        "0.1000",           // Volume
        1762780499999,      // Close time
        "130.0000000",      // Quote asset volume
        1,                  // Number of trades
        "0.1000",           // Taker buy base asset volume
        "130.0000000",      // Taker buy quote asset volume
        "0"                 // Ignore.
    ]
]
```
