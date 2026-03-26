# Options Trading - Trade

## New Order (TRADE)

## API Description

Send a new order.

## HTTP Request

```
POST /eapi/v1/order
```

## Request Parameters

| Name | Type | Mandatory | Description |
|------|------|-----------|-------------|
| symbol | STRING | YES | Option trading pair, e.g BTC-200730-9000-C |
| side | ENUM | YES | Buy/sell direction: SELL, BUY |
| type | ENUM | YES | Order Type: LIMIT (only support limit) |
| quantity | DECIMAL | YES | Order Quantity |
| price | DECIMAL | NO | Order Price |
| timeInForce | ENUM | NO | Time in force method (Default GTC) |
| reduceOnly | BOOLEAN | NO | Reduce Only (Default false) |
| postOnly | BOOLEAN | NO | Post Only (Default false) |
| newOrderRespType | ENUM | NO | "ACK", "RESULT", Default "ACK" |
| clientOrderId | STRING | NO | User-defined order ID cannot be repeated in pending orders |
| isMmp | BOOLEAN | NO | is market maker protection order, true/false |
| selfTradePreventionMode | ENUM | NO | `EXPIRE_TAKER`: expire taker order when STP triggers / `EXPIRE_MAKER`: expire maker order when STP triggers / `EXPIRE_BOTH`: expire both orders when STP triggers; Default `EXPIRE_MAKER` |
| recvWindow | LONG | NO | The value cannot be greater than 60000 |
| timestamp | LONG | YES | Current timestamp |

**Mandatory parameters depending on order type:**

| Type | Mandatory parameters |
|------|----------------------|
| LIMIT | timeInForce, quantity, price |

## Response Example

```json
{
  "orderId": 4611875134427365377,
  "symbol": "BTC-200730-9000-C",
  "price": "100",
  "quantity": "1",
  "executedQty": "0",
  "side": "BUY",
  "type": "LIMIT",
  "timeInForce": "GTC",
  "reduceOnly": false,
  "createTime": 1592465880683,
  "updateTime": 1566818724722,
  "status": "NEW",
  "avgPrice": "0",
  "source": "API",
  "clientOrderId": "",
  "priceScale": 2,
  "quantityScale": 2,
  "optionSide": "CALL",
  "quoteAsset": "USDT",
  "mmp": false,
  "selfTradePreventionMode": "EXPIRE_MAKER"
}
```
