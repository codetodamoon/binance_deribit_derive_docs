# Account Funding Flow (USER_DATA)

## API Description

Query account funding flows.

## HTTP Request

`GET /eapi/v1/bill`

## Request Weight

**1**

## Request Parameters

| Name | Type | Mandatory | Description |
|------|------|-----------|-------------|
| currency | STRING | YES | Asset type, only support USDT as of now |
| recordId | LONG | NO | Return the recordId and subsequent data, the latest data is returned by default, e.g 100000 |
| startTime | LONG | NO | Start Time, e.g 1593511200000 |
| endTime | LONG | NO | End Time, e.g 1593512200000 |
| limit | INT | NO | Number of result sets returned Default:100 Max:1000 |
| recvWindow | LONG | NO | The value cannot be greater than 60000 |
| timestamp | LONG | YES | Current timestamp in milliseconds |

> **Note:** Only support querying data in the past 3 months

## Response Example

```json
[
  {
    "id": 1125899906842624000,
    "asset": "USDT",
    "amount": "-0.552",
    "type": "FEE",
    "createDate": 1592449456000
  },
  {
    "id": 1125899906842624000,
    "asset": "USDT",
    "amount": "100",
    "type": "CONTRACT",
    "createDate": 1592449456000
  },
  {
    "id": 1125899906842624000,
    "asset": "USDT",
    "amount": "10000",
    "type": "TRANSFER",
    "createDate": 1592448410000
  }
]
```

### Response Fields Description

| Field | Type | Description |
|-------|------|-------------|
| id | LONG | Record ID |
| asset | STRING | Asset type |
| amount | STRING | Amount (positive numbers represent inflow, negative numbers represent outflow) |
| type | STRING | Type (FEE, CONTRACT, TRANSFER) |
| createDate | LONG | Time (timestamp in milliseconds) |
