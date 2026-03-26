# Send Rfq

Requests two-sided quotes from participating market makers.
Required minimum session key permission level is `account`

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
    "/private/send_rfq": {
      "post": {
        "tags": [
          "Private"
        ],
        "summary": "Send Rfq",
        "description": "Requests two-sided quotes from participating market makers.\nRequired minimum session key permission level is `account`",
        "responses": {
          "200": {
            "description": "successful operation",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/PrivateSendRfqResponseSchema"
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
                "$ref": "#/components/schemas/PrivateSendRfqParamsSchema"
              }
            }
          }
        }
      }
    }
  },
  "components": {
    "schemas": {
      "PrivateSendRfqParamsSchema": {
        "properties": {
          "client": {
            "title": "client",
            "type": "string",
            "default": "",
            "description": "Optional client that sent RFQ"
          },
          "counterparties": {
            "title": "counterparties",
            "type": "array",
            "default": null,
            "description": "Optional list of market maker account addresses to request quotes from. If not supplied, all market makers who are approved as RFQ makers will be notified.",
            "items": {
              "title": "counterparties",
              "type": "string"
            },
            "nullable": true
          },
          "label": {
            "title": "label",
            "type": "string",
            "default": "",
            "description": "Optional user-defined label for the RFQ"
          },
          "legs": {
            "title": "legs",
            "type": "array",
            "description": "RFQ legs",
            "items": {
              "$ref": "#/components/schemas/LegUnpricedSchema"
            }
          },
          "max_total_cost": {
            "title": "max_total_cost",
            "type": "string",
            "format": "decimal",
            "default": null,
            "description": "An optional max total cost for the RFQ. Only used when the RFQ sender executes as buyer. Polling endpoints and channels will ignore quotes where the total cost across all legs is above this value. Positive values mean the RFQ sender expects to pay $, negative mean the RFQ sender expects to receive $.This field is not disclosed to the market makers.",
            "nullable": true
          },
          "min_total_cost": {
            "title": "min_total_cost",
            "type": "string",
            "format": "decimal",
            "default": null,
            "description": "An optional min total cost for the RFQ. Only used when the RFQ sender executes as seller. Polling endpoints and channels will ignore quotes where the total cost across all legs is below this value. Positive values mean the RFQ sender expects to receive $, negative mean the RFQ sender expects to pay $.This field is not disclosed to the market makers.",
            "nullable": true
          },
          "partial_fill_step": {
            "title": "partial_fill_step",
            "type": "string",
            "format": "decimal",
            "default": "1",
            "description": "Optional step size for partial fills. If not supplied, the RFQ will not support partial fills."
          },
          "subaccount_id": {
            "title": "subaccount_id",
            "type": "integer",
            "description": "Subaccount ID"
          }
        },
        "required": [
          "legs",
          "subaccount_id"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "LegUnpricedSchema": {
        "properties": {
          "amount": {
            "title": "amount",
            "type": "string",
            "format": "decimal",
            "description": "Amount in units of the base"
          },
          "direction": {
            "title": "direction",
            "type": "string",
            "enum": [
              "buy",
              "sell"
            ],
            "description": "Leg direction"
          },
          "instrument_name": {
            "title": "instrument_name",
            "type": "string",
            "description": "Instrument name"
          }
        },
        "required": [
          "amount",
          "direction",
          "instrument_name"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PrivateSendRfqResponseSchema": {
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
            "$ref": "#/components/schemas/PrivateSendRfqResultSchema"
          }
        },
        "required": [
          "id",
          "result"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PrivateSendRfqResultSchema": {
        "properties": {
          "ask_total_cost": {
            "title": "ask_total_cost",
            "type": "string",
            "format": "decimal",
            "default": null,
            "description": "Ask total cost for the RFQ implied from orderbook (as `sell`)",
            "nullable": true
          },
          "bid_total_cost": {
            "title": "bid_total_cost",
            "type": "string",
            "format": "decimal",
            "default": null,
            "description": "Bid total cost for the RFQ implied from orderbook (as `buy`)",
            "nullable": true
          },
          "cancel_reason": {
            "title": "cancel_reason",
            "type": "string",
            "enum": [
              "",
              "user_request",
              "insufficient_margin",
              "signed_max_fee_too_low",
              "mmp_trigger",
              "cancel_on_disconnect",
              "session_key_deregistered",
              "subaccount_withdrawn",
              "rfq_no_longer_open",
              "compliance"
            ],
            "description": "Cancel reason, if any"
          },
          "counterparties": {
            "title": "counterparties",
            "type": "array",
            "default": null,
            "description": "List of requested counterparties, if applicable",
            "items": {
              "title": "counterparties",
              "type": "string"
            },
            "nullable": true
          },
          "creation_timestamp": {
            "title": "creation_timestamp",
            "type": "integer",
            "description": "Creation timestamp in ms since Unix epoch"
          },
          "filled_direction": {
            "title": "filled_direction",
            "type": "string",
            "default": null,
            "enum": [
              "buy",
              "sell"
            ],
            "description": "Direction at which the RFQ was filled (only if filled)",
            "nullable": true
          },
          "filled_pct": {
            "title": "filled_pct",
            "type": "string",
            "format": "decimal",
            "description": "Percentage of the RFQ that has been filled, from 0 to 1."
          },
          "label": {
            "title": "label",
            "type": "string",
            "description": "User-defined label, if any"
          },
          "last_update_timestamp": {
            "title": "last_update_timestamp",
            "type": "integer",
            "description": "Last update timestamp in ms since Unix epoch"
          },
          "legs": {
            "title": "legs",
            "type": "array",
            "description": "RFQ legs",
            "items": {
              "$ref": "#/components/schemas/LegUnpricedSchema"
            }
          },
          "mark_total_cost": {
            "title": "mark_total_cost",
            "type": "string",
            "format": "decimal",
            "default": null,
            "description": "Mark total cost for the RFQ (assuming `buy` direction)",
            "nullable": true
          },
          "max_total_cost": {
            "title": "max_total_cost",
            "type": "string",
            "format": "decimal",
            "default": null,
            "description": "Max total cost for the RFQ",
            "nullable": true
          },
          "min_total_cost": {
            "title": "min_total_cost",
            "type": "string",
            "format": "decimal",
            "default": null,
            "description": "Min total cost for the RFQ",
            "nullable": true
          },
          "partial_fill_step": {
            "title": "partial_fill_step",
            "type": "string",
            "format": "decimal",
            "description": "Step size for partial fills (default: 1)"
          },
          "rfq_id": {
            "title": "rfq_id",
            "type": "string",
            "format": "uuid",
            "description": "RFQ ID"
          },
          "status": {
            "title": "status",
            "type": "string",
            "enum": [
              "open",
              "filled",
              "cancelled",
              "expired"
            ],
            "description": "Status"
          },
          "subaccount_id": {
            "title": "subaccount_id",
            "type": "integer",
            "description": "Subaccount ID"
          },
          "total_cost": {
            "title": "total_cost",
            "type": "string",
            "format": "decimal",
            "default": null,
            "description": "Total cost for the RFQ (only if filled)",
            "nullable": true
          },
          "valid_until": {
            "title": "valid_until",
            "type": "integer",
            "description": "RFQ expiry timestamp in ms since Unix epoch"
          },
          "wallet": {
            "title": "wallet",
            "type": "string",
            "description": "Wallet address of the RFQ sender"
          }
        },
        "required": [
          "ask_total_cost",
          "bid_total_cost",
          "cancel_reason",
          "counterparties",
          "creation_timestamp",
          "filled_direction",
          "filled_pct",
          "label",
          "last_update_timestamp",
          "legs",
          "mark_total_cost",
          "max_total_cost",
          "min_total_cost",
          "partial_fill_step",
          "rfq_id",
          "status",
          "subaccount_id",
          "total_cost",
          "valid_until",
          "wallet"
        ],
        "type": "object",
        "additionalProperties": false
      }
    }
  }
}
```