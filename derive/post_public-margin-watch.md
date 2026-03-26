# Margin Watch

Calculates MtM and maintenance margin for a given subaccount.

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
    "/public/margin_watch": {
      "post": {
        "tags": [
          "Public"
        ],
        "summary": "Margin Watch",
        "description": "Calculates MtM and maintenance margin for a given subaccount.",
        "responses": {
          "200": {
            "description": "successful operation",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/PublicMarginWatchResponseSchema"
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
                "$ref": "#/components/schemas/PublicMarginWatchParamsSchema"
              }
            }
          }
        }
      }
    }
  },
  "components": {
    "schemas": {
      "PublicMarginWatchParamsSchema": {
        "properties": {
          "force_onchain": {
            "title": "force_onchain",
            "type": "boolean",
            "default": false,
            "description": "Force the fetching of on-chain balances, default False."
          },
          "subaccount_id": {
            "title": "subaccount_id",
            "type": "integer",
            "description": "Subaccount ID to get margin for."
          }
        },
        "required": [
          "subaccount_id"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PublicMarginWatchResponseSchema": {
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
            "$ref": "#/components/schemas/PublicMarginWatchResultSchema"
          }
        },
        "required": [
          "id",
          "result"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PublicMarginWatchResultSchema": {
        "properties": {
          "collaterals": {
            "title": "collaterals",
            "type": "array",
            "description": "All collaterals that count towards margin of subaccount",
            "items": {
              "$ref": "#/components/schemas/CollateralPublicResponseSchema"
            }
          },
          "currency": {
            "title": "currency",
            "type": "string",
            "description": "Currency of subaccount"
          },
          "initial_margin": {
            "title": "initial_margin",
            "type": "string",
            "format": "decimal",
            "description": "Total initial margin requirement of all positions and collaterals."
          },
          "maintenance_margin": {
            "title": "maintenance_margin",
            "type": "string",
            "format": "decimal",
            "description": "Total maintenance margin requirement of all positions and collaterals.If this value falls below zero, the subaccount will be flagged for liquidation."
          },
          "margin_type": {
            "title": "margin_type",
            "type": "string",
            "enum": [
              "PM",
              "SM",
              "PM2"
            ],
            "description": "Margin type of subaccount (`PM` (Portfolio Margin), `PM2` (Portfolio Margin 2), or `SM` (Standard Margin))"
          },
          "positions": {
            "title": "positions",
            "type": "array",
            "description": "All active positions of subaccount",
            "items": {
              "$ref": "#/components/schemas/PositionPublicResponseSchema"
            }
          },
          "subaccount_id": {
            "title": "subaccount_id",
            "type": "integer",
            "description": "Subaccount_id"
          },
          "subaccount_value": {
            "title": "subaccount_value",
            "type": "string",
            "format": "decimal",
            "description": "Total mark-to-market value of all positions and collaterals"
          },
          "valuation_timestamp": {
            "title": "valuation_timestamp",
            "type": "integer",
            "description": "Timestamp (in seconds since epoch) of when margin and MtM were computed."
          }
        },
        "required": [
          "collaterals",
          "currency",
          "initial_margin",
          "maintenance_margin",
          "margin_type",
          "positions",
          "subaccount_id",
          "subaccount_value",
          "valuation_timestamp"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "CollateralPublicResponseSchema": {
        "properties": {
          "amount": {
            "title": "amount",
            "type": "string",
            "format": "decimal",
            "description": "Asset amount of given collateral"
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
          }
        },
        "required": [
          "amount",
          "asset_name",
          "asset_type",
          "initial_margin",
          "maintenance_margin",
          "mark_price",
          "mark_value"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PositionPublicResponseSchema": {
        "properties": {
          "amount": {
            "title": "amount",
            "type": "string",
            "format": "decimal",
            "description": "Position amount held by subaccount"
          },
          "delta": {
            "title": "delta",
            "type": "string",
            "format": "decimal",
            "description": "Asset delta (w.r.t. forward price for options, `1.0` for perps)"
          },
          "gamma": {
            "title": "gamma",
            "type": "string",
            "format": "decimal",
            "description": "Asset gamma (zero for non-options)"
          },
          "index_price": {
            "title": "index_price",
            "type": "string",
            "format": "decimal",
            "description": "Current index (oracle) price for position's currency"
          },
          "initial_margin": {
            "title": "initial_margin",
            "type": "string",
            "format": "decimal",
            "description": "USD initial margin requirement for this position"
          },
          "instrument_name": {
            "title": "instrument_name",
            "type": "string",
            "description": "Instrument name (same as the base Asset name)"
          },
          "instrument_type": {
            "title": "instrument_type",
            "type": "string",
            "enum": [
              "erc20",
              "option",
              "perp"
            ],
            "description": "`erc20`, `option`, or `perp`"
          },
          "liquidation_price": {
            "title": "liquidation_price",
            "type": "string",
            "format": "decimal",
            "default": null,
            "description": "Index price at which position will be liquidated",
            "nullable": true
          },
          "maintenance_margin": {
            "title": "maintenance_margin",
            "type": "string",
            "format": "decimal",
            "description": "USD maintenance margin requirement for this position"
          },
          "mark_price": {
            "title": "mark_price",
            "type": "string",
            "format": "decimal",
            "description": "Current mark price for position's instrument"
          },
          "mark_value": {
            "title": "mark_value",
            "type": "string",
            "format": "decimal",
            "description": "USD value of the position; this represents how much USD can be recieved by fully closing the position at the current oracle price"
          },
          "theta": {
            "title": "theta",
            "type": "string",
            "format": "decimal",
            "description": "Asset theta (zero for non-options)"
          },
          "vega": {
            "title": "vega",
            "type": "string",
            "format": "decimal",
            "description": "Asset vega (zero for non-options)"
          }
        },
        "required": [
          "amount",
          "delta",
          "gamma",
          "index_price",
          "initial_margin",
          "instrument_name",
          "instrument_type",
          "liquidation_price",
          "maintenance_margin",
          "mark_price",
          "mark_value",
          "theta",
          "vega"
        ],
        "type": "object",
        "additionalProperties": false
      }
    }
  }
}
```