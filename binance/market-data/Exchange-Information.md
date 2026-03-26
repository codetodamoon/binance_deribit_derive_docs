# Exchange Information

## API Description

Current exchange trading rules and symbol information

## HTTP Request

```
GET /eapi/v1/exchangeInfo
```

## Request Weight

**1**

## Request Parameters

None

## Response Example

```json
{
  "timezone": "UTC",
  "serverTime": 1592387337630,
  "optionContracts": [
    {
      "baseAsset": "BTC",
      "quoteAsset": "USDT",
      "underlying": "BTCUSDT",
      "settleAsset": "USDT"
    }
  ],
  "optionAssets": [
    {
      "name": "USDT"
    }
  ],
  "optionSymbols": [
    {
      "expiryDate": 1660521600000,
      "filters": [
        {
          "filterType": "PRICE_FILTER",
          "minPrice": "0.02",
          "maxPrice": "80000.01",
          "tickSize": "0.01"
        },
        {
          "filterType": "LOT_SIZE",
          "minQty": "0.01",
          "maxQty": "100",
          "stepSize": "0.01"
        }
      ],
      "symbol": "BTC-220815-50000-C",
      "side": "CALL",
      "strikePrice": "50000",
      "underlying": "BTCUSDT",
      "unit": 1,
      "liquidationFeeRate": "0.0019000",
      "minQty": "0.01",
      "maxQty": "100",
      "initialMargin": "0.15",
      "maintenanceMargin": "0.075",
      "minInitialMargin": "0.1",
      "minMaintenanceMargin": "0.05",
      "priceScale": 2,
      "quantityScale": 2,
      "quoteAsset": "USDT",
      "status": "TRADING"
    }
  ],
  "rateLimits": [
    {
      "rateLimitType": "REQUEST_WEIGHT",
      "interval": "MINUTE",
      "intervalNum": 1,
      "limit": 2400
    },
    {
      "rateLimitType": "ORDERS",
      "interval": "MINUTE",
      "intervalNum": 1,
      "limit": 1200
    },
    {
      "rateLimitType": "ORDERS",
      "interval": "SECOND",
      "intervalNum": 10,
      "limit": 300
    }
  ]
}
```

### Response Fields Description

| Field | Description |
|-------|-------------|
| timezone | Time zone used by the server |
| serverTime | Current system time |
| optionContracts | Option contract underlying asset info |
| baseAsset | Base currency |
| quoteAsset | Quotation asset |
| underlying | Name of the underlying asset of the option contract |
| settleAsset | Settlement currency |
| optionAssets | Option asset info |
| optionSymbols | Option trading pair info |
| expiryDate | Expiry time |
| filters | Trading rules |
| filterType | Filter type (PRICE_FILTER, LOT_SIZE) |
| minPrice | Minimum price |
| maxPrice | Maximum price |
| tickSize | Price tick size |
| minQty | Minimum order quantity |
| maxQty | Maximum order quantity |
| stepSize | Quantity step size |
| symbol | Trading pair name |
| side | Direction: CALL long, PUT short |
| strikePrice | Strike price |
| unit | Contract unit |
| liquidationFeeRate | Liquidation fee rate |
| initialMargin | Initial Margin Ratio |
| maintenanceMargin | Maintenance Margin Ratio |
| minInitialMargin | Min Initial Margin Ratio |
| minMaintenanceMargin | Min Maintenance Margin Ratio |
| priceScale | Price precision |
| quantityScale | Quantity precision |
| status | Trading Status |
| rateLimits | Rate limit information |
