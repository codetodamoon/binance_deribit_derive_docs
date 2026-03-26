# Common Definition

## Public Endpoints Info

## Terminology

- `symbol` refers to the symbol name of an options contract symbol
- `underlying` refers to the underlying symbol of an options contract symbol
- `quoteAsset` refers to the asset that is the price of a symbol
- `settleAsset` refers to the settlement asset when options are exercised

## ENUM Definitions

**Options contract type**

- CALL
- PUT

**Order side (side)**

- BUY
- SELL

**Position side (positionSide)**

- LONG
- SHORT

**Time in force (timeInForce)**

- GTC - Good Till Cancel
- IOC - Immediate or Cancel
- FOK - Fill or Kill
- GTX - Post only

**Response Type (newOrderRespType)**

- ACK
- RESULT

**Order types (type)**

- LIMIT

**Order status (status)**

- NEW
- REJECTED
- PARTIALLY_FILLED
- FILLED
- CANCELED
- EXPIRED
- EXPIRED_IN_MATCH

**Kline/Candlestick chart intervals:**
m = minutes; h = hours; d = days; w = weeks; M = months

- 1m, 3m, 5m, 15m, 30m, 1h, 2h, 4h, 6h, 8h, 12h, 1d, 3d, 1w, 1M

**STP MODE (selfTradePreventionMode):**

- EXPIRE_TAKER
- EXPIRE_BOTH
- EXPIRE_MAKER

**Rate limiters (rateLimitType)**

- REQUEST_WEIGHT: 2400 per MINUTE
- ORDERS: 1200 per MINUTE

**Rate limit intervals (interval)**

- MINUTE

## Filters

Filters define trading rules on a symbol or an exchange.

### Symbol Filters

#### PRICE_FILTER

```json
{
  "filterType": "PRICE_FILTER",
  "minPrice": "793.112",
  "maxPrice": "1189.668",
  "tickSize": "5.000"
}
```

The PRICE_FILTER defines price rules for a symbol with three components:

- `minPrice` defines the minimum price allowed; disabled when minPrice == 0
- `maxPrice` defines the maximum price allowed; disabled when maxPrice == 0
- `tickSize` defines the intervals that a price can be increased/decreased by; disabled when tickSize == 0

Rules for price validation:

- sell order price >= minPrice
- buy order price <= maxPrice
- (price - minPrice) % tickSize == 0

#### LOT_SIZE

```json
{
  "filterType": "LOT_SIZE",
  "minQty": "0.0001",
  "maxQty": "1000",
  "stepSize": "0.0100"
}
```

The LOT_SIZE filter defines quantity rules with three components:

- `minQty` defines the minimum quantity allowed
- `maxQty` defines the maximum quantity allowed
- `stepSize` defines the intervals that a quantity can be increased/decreased by

Rules for quantity validation:

- quantity >= minQty
- quantity <= maxQty
- (quantity - minQty) % stepSize == 0
