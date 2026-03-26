# Get Option Settlement History

Get expired option settlement history for a subaccount
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
    "/private/get_option_settlement_history": {
      "post": {
        "tags": [
          "Private"
        ],
        "summary": "Get Option Settlement History",
        "description": "Get expired option settlement history for a subaccount\nRequired minimum session key permission level is `read_only`",
        "responses": {
          "200": {
            "description": "successful operation",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/PrivateGetOptionSettlementHistoryResponseSchema"
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
                "$ref": "#/components/schemas/PrivateGetOptionSettlementHistoryParamsSchema"
              }
            }
          }
        }
      }
    }
  },
  "components": {
    "schemas": {
      "PrivateGetOptionSettlementHistoryParamsSchema": {
        "properties": {
          "subaccount_id": {
            "title": "subaccount_id",
            "type": "integer",
            "description": "Subaccount ID for which to get expired option settlement history"
          }
        },
        "required": [
          "subaccount_id"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PrivateGetOptionSettlementHistoryResponseSchema": {
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
            "$ref": "#/components/schemas/PrivateGetOptionSettlementHistoryResultSchema"
          }
        },
        "required": [
          "id",
          "result"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PrivateGetOptionSettlementHistoryResultSchema": {
        "properties": {
          "settlements": {
            "title": "settlements",
            "type": "array",
            "description": "List of expired option settlements",
            "items": {
              "$ref": "#/components/schemas/OptionSettlementResponseSchema"
            }
          },
          "subaccount_id": {
            "title": "subaccount_id",
            "type": "integer",
            "description": "Subaccount_id for which to get expired option settlement history"
          }
        },
        "required": [
          "settlements",
          "subaccount_id"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "OptionSettlementResponseSchema": {
        "properties": {
          "amount": {
            "title": "amount",
            "type": "string",
            "format": "decimal",
            "description": "Amount that was settled"
          },
          "expiry": {
            "title": "expiry",
            "type": "integer",
            "description": "Expiry timestamp of the option"
          },
          "instrument_name": {
            "title": "instrument_name",
            "type": "string",
            "description": "Instrument name"
          },
          "option_settlement_pnl": {
            "title": "option_settlement_pnl",
            "type": "string",
            "format": "decimal",
            "description": "USD profit or loss from option settlements calculated as: settlement value - (average cost including fees x amount)"
          },
          "option_settlement_pnl_excl_fees": {
            "title": "option_settlement_pnl_excl_fees",
            "type": "string",
            "format": "decimal",
            "description": "USD profit or loss from option settlements calculated as: settlement value - (average price excluding fees x amount)"
          },
          "settlement_price": {
            "title": "settlement_price",
            "type": "string",
            "format": "decimal",
            "description": "Price of option settlement"
          },
          "subaccount_id": {
            "title": "subaccount_id",
            "type": "integer",
            "description": "Subaccount ID of the settlement event"
          }
        },
        "required": [
          "amount",
          "expiry",
          "instrument_name",
          "option_settlement_pnl",
          "option_settlement_pnl_excl_fees",
          "settlement_price",
          "subaccount_id"
        ],
        "type": "object",
        "additionalProperties": false
      }
    }
  }
}
```