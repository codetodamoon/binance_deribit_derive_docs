# Get Margin

Calculates margin for a given portfolio and (optionally) a simulated state change.<br />Does not take into account open orders margin requirements.public/withdraw_debug

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
    "/public/get_margin": {
      "post": {
        "tags": [
          "Public"
        ],
        "summary": "Get Margin",
        "description": "Calculates margin for a given portfolio and (optionally) a simulated state change.<br />Does not take into account open orders margin requirements.public/withdraw_debug",
        "responses": {
          "200": {
            "description": "successful operation",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/PublicGetMarginResponseSchema"
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
                "$ref": "#/components/schemas/PublicGetMarginParamsSchema"
              }
            }
          }
        }
      }
    }
  },
  "components": {
    "schemas": {
      "PublicGetMarginParamsSchema": {
        "properties": {
          "margin_type": {
            "title": "margin_type",
            "type": "string",
            "enum": [
              "PM",
              "SM",
              "PM2"
            ],
            "description": "`PM` (Portfolio Margin), `PM2 (Portfolio Margin 2), or `SM` (Standard Margin))"
          },
          "market": {
            "title": "market",
            "type": "string",
            "default": null,
            "description": "Must be defined for Portfolio Margin",
            "nullable": true
          },
          "simulated_collateral_changes": {
            "title": "simulated_collateral_changes",
            "type": "array",
            "default": null,
            "description": "Optional, add collaterals to simulate deposits / withdrawals / spot trades",
            "items": {
              "$ref": "#/components/schemas/SimulatedCollateralSchema"
            },
            "nullable": true
          },
          "simulated_collaterals": {
            "title": "simulated_collaterals",
            "type": "array",
            "description": "List of collaterals in a simulated portfolio",
            "items": {
              "$ref": "#/components/schemas/SimulatedCollateralSchema"
            }
          },
          "simulated_position_changes": {
            "title": "simulated_position_changes",
            "type": "array",
            "default": null,
            "description": "Optional, add positions to simulate perp / option trades",
            "items": {
              "$ref": "#/components/schemas/SimulatedPositionSchema"
            },
            "nullable": true
          },
          "simulated_positions": {
            "title": "simulated_positions",
            "type": "array",
            "description": "List of positions in a simulated portfolio",
            "items": {
              "$ref": "#/components/schemas/SimulatedPositionSchema"
            }
          }
        },
        "required": [
          "margin_type",
          "simulated_collaterals",
          "simulated_positions"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "SimulatedCollateralSchema": {
        "properties": {
          "amount": {
            "title": "amount",
            "type": "string",
            "format": "decimal",
            "description": "Collateral amount to simulate"
          },
          "asset_name": {
            "title": "asset_name",
            "type": "string",
            "description": "Collateral ERC20 asset name (e.g. ETH, USDC, WSTETH)"
          }
        },
        "required": [
          "amount",
          "asset_name"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "SimulatedPositionSchema": {
        "properties": {
          "amount": {
            "title": "amount",
            "type": "string",
            "format": "decimal",
            "description": "Position amount to simulate"
          },
          "entry_price": {
            "title": "entry_price",
            "type": "string",
            "format": "decimal",
            "default": null,
            "description": "Only for perps. Entry price to use in the simulation. Mark price is used if not provided.",
            "nullable": true
          },
          "instrument_name": {
            "title": "instrument_name",
            "type": "string",
            "description": "Instrument name"
          }
        },
        "required": [
          "amount",
          "instrument_name"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PublicGetMarginResponseSchema": {
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
            "$ref": "#/components/schemas/PublicGetMarginResultSchema"
          }
        },
        "required": [
          "id",
          "result"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PublicGetMarginResultSchema": {
        "properties": {
          "is_valid_trade": {
            "title": "is_valid_trade",
            "type": "boolean",
            "description": "True if trade passes margin requirement"
          },
          "post_initial_margin": {
            "title": "post_initial_margin",
            "type": "string",
            "format": "decimal",
            "description": "Initial margin requirement post trade"
          },
          "post_maintenance_margin": {
            "title": "post_maintenance_margin",
            "type": "string",
            "format": "decimal",
            "description": "Maintenance margin requirement post trade"
          },
          "pre_initial_margin": {
            "title": "pre_initial_margin",
            "type": "string",
            "format": "decimal",
            "description": "Initial margin requirement before trade"
          },
          "pre_maintenance_margin": {
            "title": "pre_maintenance_margin",
            "type": "string",
            "format": "decimal",
            "description": "Maintenance margin requirement before trade"
          },
          "subaccount_id": {
            "title": "subaccount_id",
            "type": "integer",
            "description": "Subaccount_id"
          }
        },
        "required": [
          "is_valid_trade",
          "post_initial_margin",
          "post_maintenance_margin",
          "pre_initial_margin",
          "pre_maintenance_margin",
          "subaccount_id"
        ],
        "type": "object",
        "additionalProperties": false
      }
    }
  }
}
```