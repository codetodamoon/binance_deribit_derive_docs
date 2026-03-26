# Get Collaterals

Get collaterals of a subaccount
Required minimum session key permission level is `read_only`

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
    "/private/get_collaterals": {
      "post": {
        "tags": [
          "Private"
        ],
        "summary": "Get Collaterals",
        "description": "Get collaterals of a subaccount\nRequired minimum session key permission level is `read_only`",
        "responses": {
          "200": {
            "description": "successful operation",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/PrivateGetCollateralsResponseSchema"
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
                "$ref": "#/components/schemas/PrivateGetCollateralsParamsSchema"
              }
            }
          }
        }
      }
    }
  },
  "components": {
    "schemas": {
      "PrivateGetCollateralsParamsSchema": {
        "properties": {
          "subaccount_id": {
            "title": "subaccount_id",
            "type": "integer",
            "description": "Subaccount_id"
          }
        },
        "required": [
          "subaccount_id"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PrivateGetCollateralsResponseSchema": {
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
            "$ref": "#/components/schemas/PrivateGetCollateralsResultSchema"
          }
        },
        "required": [
          "id",
          "result"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PrivateGetCollateralsResultSchema": {
        "properties": {
          "collaterals": {
            "title": "collaterals",
            "type": "array",
            "description": "All collaterals that count towards margin of subaccount",
            "items": {
              "$ref": "#/components/schemas/CollateralResponseSchema"
            }
          },
          "subaccount_id": {
            "title": "subaccount_id",
            "type": "integer",
            "description": "Subaccount_id"
          }
        },
        "required": [
          "collaterals",
          "subaccount_id"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "CollateralResponseSchema": {
        "properties": {
          "amount": {
            "title": "amount",
            "type": "string",
            "format": "decimal",
            "description": "Asset amount of given collateral"
          },
          "amount_step": {
            "title": "amount_step",
            "type": "string",
            "format": "decimal",
            "description": "Minimum amount step for the collateral"
          },
          "asset_name": {
            "title": "asset_name",
            "type": "string",
            "description": "Asset name"
          },
          "asset_type": {
            "title": "asset_type",
            "type": "string",
            "enum": [
              "erc20",
              "option",
              "perp"
            ],
            "description": "Type of asset collateral (currently always `erc20`)"
          },
          "average_price": {
            "title": "average_price",
            "type": "string",
            "format": "decimal",
            "description": "Average price of the collateral, 0 for USDC."
          },
          "average_price_excl_fees": {
            "title": "average_price_excl_fees",
            "type": "string",
            "format": "decimal",
            "description": "Average price of whole position excluding fees"
          },
          "creation_timestamp": {
            "title": "creation_timestamp",
            "type": "integer",
            "description": "Timestamp of when the position was opened (in ms since Unix epoch)"
          },
          "cumulative_interest": {
            "title": "cumulative_interest",
            "type": "string",
            "format": "decimal",
            "description": "Cumulative interest earned on supplying collateral or paid for borrowing"
          },
          "currency": {
            "title": "currency",
            "type": "string",
            "description": "Underlying currency of asset (`ETH`, `BTC`, etc)"
          },
          "delta": {
            "title": "delta",
            "type": "string",
            "format": "decimal",
            "description": "Asset delta w.r.t. the delta currency"
          },
          "delta_currency": {
            "title": "delta_currency",
            "type": "string",
            "description": "Currency with respect to which delta is reported.For example, LRTs like WEETH have their delta reported in ETH"
          },
          "initial_margin": {
            "title": "initial_margin",
            "type": "string",
            "format": "decimal",
            "description": "USD value of collateral that contributes to initial margin"
          },
          "maintenance_margin": {
            "title": "maintenance_margin",
            "type": "string",
            "format": "decimal",
            "description": "USD value of collateral that contributes to maintenance margin"
          },
          "mark_price": {
            "title": "mark_price",
            "type": "string",
            "format": "decimal",
            "description": "Current mark price of the asset"
          },
          "mark_value": {
            "title": "mark_value",
            "type": "string",
            "format": "decimal",
            "description": "USD value of the collateral (amount * mark price)"
          },
          "open_orders_margin": {
            "title": "open_orders_margin",
            "type": "string",
            "format": "decimal",
            "description": "USD margin requirement for all open orders for this asset / instrument"
          },
          "pending_interest": {
            "title": "pending_interest",
            "type": "string",
            "format": "decimal",
            "description": "Portion of interest that has not yet been settled on-chain. This number is added to the portfolio value for margin calculations purposes."
          },
          "realized_pnl": {
            "title": "realized_pnl",
            "type": "string",
            "format": "decimal",
            "description": "Realized trading profit or loss of the collateral, 0 for USDC."
          },
          "realized_pnl_excl_fees": {
            "title": "realized_pnl_excl_fees",
            "type": "string",
            "format": "decimal",
            "description": "Realized trading profit or loss of the position excluding fees"
          },
          "total_fees": {
            "title": "total_fees",
            "type": "string",
            "format": "decimal",
            "description": "Total fees paid for opening and changing the position"
          },
          "unrealized_pnl": {
            "title": "unrealized_pnl",
            "type": "string",
            "format": "decimal",
            "description": "Unrealized trading profit or loss of the collateral, 0 for USDC."
          },
          "unrealized_pnl_excl_fees": {
            "title": "unrealized_pnl_excl_fees",
            "type": "string",
            "format": "decimal",
            "description": "Unrealized trading profit or loss of the position excluding fees"
          }
        },
        "required": [
          "amount",
          "amount_step",
          "asset_name",
          "asset_type",
          "average_price",
          "average_price_excl_fees",
          "creation_timestamp",
          "cumulative_interest",
          "currency",
          "delta",
          "delta_currency",
          "initial_margin",
          "maintenance_margin",
          "mark_price",
          "mark_value",
          "open_orders_margin",
          "pending_interest",
          "realized_pnl",
          "realized_pnl_excl_fees",
          "total_fees",
          "unrealized_pnl",
          "unrealized_pnl_excl_fees"
        ],
        "type": "object",
        "additionalProperties": false
      }
    }
  }
}
```