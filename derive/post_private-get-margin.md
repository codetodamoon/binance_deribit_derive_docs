# Get Margin

Calculates margin for a given subaccount and (optionally) a simulated state change. Does not take into account<br />open orders margin requirements.
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
    "/private/get_margin": {
      "post": {
        "tags": [
          "Private"
        ],
        "summary": "Get Margin",
        "description": "Calculates margin for a given subaccount and (optionally) a simulated state change. Does not take into account<br />open orders margin requirements.\nRequired minimum session key permission level is `read_only`",
        "responses": {
          "200": {
            "description": "successful operation",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/PrivateGetMarginResponseSchema"
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
                "$ref": "#/components/schemas/PrivateGetMarginParamsSchema"
              }
            }
          }
        }
      }
    }
  },
  "components": {
    "schemas": {
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
      "PrivateGetMarginParamsSchema": {
        "properties": {
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
      "PrivateGetMarginResponseSchema": {
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
            "$ref": "#/components/schemas/PrivateGetMarginResultSchema"
          }
        },
        "required": [
          "id",
          "result"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PrivateGetMarginResultSchema": {
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