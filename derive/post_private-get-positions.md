# Get Positions

Get active positions of a subaccount
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
    "/private/get_positions": {
      "post": {
        "tags": [
          "Private"
        ],
        "summary": "Get Positions",
        "description": "Get active positions of a subaccount\nRequired minimum session key permission level is `read_only`",
        "responses": {
          "200": {
            "description": "successful operation",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/PrivateGetPositionsResponseSchema"
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
                "$ref": "#/components/schemas/PrivateGetPositionsParamsSchema"
              }
            }
          }
        }
      }
    }
  },
  "components": {
    "schemas": {
      "PositionResponseSchema": {
        "properties": {
          "amount": {
            "title": "amount",
            "type": "string",
            "format": "decimal",
            "description": "Position amount held by subaccount"
          },
          "amount_step": {
            "title": "amount_step",
            "type": "string",
            "format": "decimal",
            "description": "Minimum amount step for the position"
          },
          "average_price": {
            "title": "average_price",
            "type": "string",
            "format": "decimal",
            "description": "Average price of whole position"
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
          "cumulative_funding": {
            "title": "cumulative_funding",
            "type": "string",
            "format": "decimal",
            "description": "Cumulative funding for the position (only for perpetuals)."
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
          "leverage": {
            "title": "leverage",
            "type": "string",
            "format": "decimal",
            "default": null,
            "description": "Only for perps. Leverage of the position, defined as `abs(notional) / collateral net of options margin`",
            "nullable": true
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
          "net_settlements": {
            "title": "net_settlements",
            "type": "string",
            "format": "decimal",
            "description": "Net amount of USD from position settlements that has been paid to the user's subaccount. This number is subtracted from the portfolio value for margin calculations purposes.<br />Positive values mean the user has recieved USD from settlements, or is awaiting settlement of USD losses. Negative values mean the user has paid USD for settlements, or is awaiting settlement of USD gains."
          },
          "open_orders_margin": {
            "title": "open_orders_margin",
            "type": "string",
            "format": "decimal",
            "description": "USD margin requirement for all open orders for this asset / instrument"
          },
          "pending_funding": {
            "title": "pending_funding",
            "type": "string",
            "format": "decimal",
            "description": "A portion of funding payments that has not yet been settled into cash balance (only for perpetuals). This number is added to the portfolio value for margin calculations purposes."
          },
          "realized_pnl": {
            "title": "realized_pnl",
            "type": "string",
            "format": "decimal",
            "description": "Realized trading profit or loss of the position."
          },
          "realized_pnl_excl_fees": {
            "title": "realized_pnl_excl_fees",
            "type": "string",
            "format": "decimal",
            "description": "Realized trading profit or loss of the position excluding fees"
          },
          "theta": {
            "title": "theta",
            "type": "string",
            "format": "decimal",
            "description": "Asset theta (zero for non-options)"
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
            "description": "Unrealized trading profit or loss of the position."
          },
          "unrealized_pnl_excl_fees": {
            "title": "unrealized_pnl_excl_fees",
            "type": "string",
            "format": "decimal",
            "description": "Unrealized trading profit or loss of the position excluding fees"
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
          "amount_step",
          "average_price",
          "average_price_excl_fees",
          "creation_timestamp",
          "cumulative_funding",
          "delta",
          "gamma",
          "index_price",
          "initial_margin",
          "instrument_name",
          "instrument_type",
          "leverage",
          "liquidation_price",
          "maintenance_margin",
          "mark_price",
          "mark_value",
          "net_settlements",
          "open_orders_margin",
          "pending_funding",
          "realized_pnl",
          "realized_pnl_excl_fees",
          "theta",
          "total_fees",
          "unrealized_pnl",
          "unrealized_pnl_excl_fees",
          "vega"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PrivateGetPositionsParamsSchema": {
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
      "PrivateGetPositionsResponseSchema": {
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
            "$ref": "#/components/schemas/PrivateGetPositionsResultSchema"
          }
        },
        "required": [
          "id",
          "result"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PrivateGetPositionsResultSchema": {
        "properties": {
          "positions": {
            "title": "positions",
            "type": "array",
            "description": "All active positions of subaccount",
            "items": {
              "$ref": "#/components/schemas/PositionResponseSchema"
            }
          },
          "subaccount_id": {
            "title": "subaccount_id",
            "type": "integer",
            "description": "Subaccount_id"
          }
        },
        "required": [
          "positions",
          "subaccount_id"
        ],
        "type": "object",
        "additionalProperties": false
      }
    }
  }
}
```