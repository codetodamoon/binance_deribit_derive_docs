# Get Option Settlement Prices

Get settlement prices by expiry for each currency

# OpenAPI definition

```json
{
  "openapi": "3.0.0",
  "info": {
    "version": "1.0.0",
    "title": "REST API"
  },
  "servers": [
    {
      "url": "https://api.lyra.finance",
      "description": "Prod"
    },
    {
      "url": "https://api-demo.lyra.finance",
      "description": "Testnet"
    }
  ],
  "paths": {
    "/public/get_option_settlement_prices": {
      "post": {
        "tags": [
          "Public"
        ],
        "summary": "Get Option Settlement Prices",
        "description": "Get settlement prices by expiry for each currency",
        "responses": {
          "200": {
            "description": "successful operation",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/PublicGetOptionSettlementPricesResponseSchema"
                }
              }
            }
          }
        },
        "parameters": [],
        "requestBody": {
          "required": true,
          "content": {
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/PublicGetOptionSettlementPricesParamsSchema"
              }
            }
          }
        }
      }
    }
  },
  "components": {
    "schemas": {
      "PublicGetOptionSettlementPricesParamsSchema": {
        "properties": {
          "currency": {
            "title": "currency",
            "type": "string",
            "description": "Currency for which to show expiries"
          }
        },
        "required": [
          "currency"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PublicGetOptionSettlementPricesResponseSchema": {
        "properties": {
          "id": {
            "oneOf": [
              {
                "title": "",
                "type": "string"
              },
              {
                "title": "",
                "type": "integer"
              }
            ]
          },
          "result": {
            "$ref": "#/components/schemas/PublicGetOptionSettlementPricesResultSchema"
          }
        },
        "required": [
          "id",
          "result"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PublicGetOptionSettlementPricesResultSchema": {
        "properties": {
          "expiries": {
            "title": "expiries",
            "type": "array",
            "description": "List of expiry details",
            "items": {
              "$ref": "#/components/schemas/ExpiryResponseSchema"
            }
          }
        },
        "required": [
          "expiries"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "ExpiryResponseSchema": {
        "properties": {
          "expiry_date": {
            "title": "expiry_date",
            "type": "string",
            "description": "Expiry date in `YYYYMMDD` format"
          },
          "price": {
            "title": "price",
            "type": "string",
            "format": "decimal",
            "default": null,
            "description": "Settlement price will show None if not yet settled onchain",
            "nullable": true
          },
          "utc_expiry_sec": {
            "title": "utc_expiry_sec",
            "type": "integer",
            "description": "UTC timestamp of expiry"
          }
        },
        "required": [
          "expiry_date",
          "price",
          "utc_expiry_sec"
        ],
        "type": "object",
        "additionalProperties": false
      }
    }
  }
}
```