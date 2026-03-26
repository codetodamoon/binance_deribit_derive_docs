# Get Rfqs

Retrieves a list of RFQs matching filter criteria. Takers can use this to get their open RFQs, RFQ history, etc.
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
    "/private/get_rfqs": {
      "post": {
        "tags": [
          "Private"
        ],
        "summary": "Get Rfqs",
        "description": "Retrieves a list of RFQs matching filter criteria. Takers can use this to get their open RFQs, RFQ history, etc.\nRequired minimum session key permission level is `read_only`",
        "responses": {
          "200": {
            "description": "successful operation",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/PrivateGetRfqsResponseSchema"
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
                "$ref": "#/components/schemas/PrivateGetRfqsParamsSchema"
              }
            }
          }
        }
      }
    }
  },
  "components": {
    "schemas": {
      "PaginationInfoSchema": {
        "properties": {
          "count": {
            "title": "count",
            "type": "integer",
            "description": "Total number of items, across all pages"
          },
          "num_pages": {
            "title": "num_pages",
            "type": "integer",
            "description": "Number of pages"
          }
        },
        "required": [
          "count",
          "num_pages"
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
      "PrivateGetRfqsParamsSchema": {
        "properties": {
          "from_timestamp": {
            "title": "from_timestamp",
            "type": "integer",
            "default": 0,
            "description": "Earliest `last_update_timestamp` to filter by (in ms since Unix epoch). If not provied, defaults to 0."
          },
          "page": {
            "title": "page",
            "type": "integer",
            "default": 1,
            "description": "Page number of results to return (default 1, returns last if above `num_pages`)"
          },
          "page_size": {
            "title": "page_size",
            "type": "integer",
            "default": 100,
            "description": "Number of results per page (default 100, max 1000)"
          },
          "rfq_id": {
            "title": "rfq_id",
            "type": "string",
            "format": "uuid",
            "default": null,
            "description": "RFQ ID filter, if applicable",
            "nullable": true
          },
          "status": {
            "title": "status",
            "type": "string",
            "default": null,
            "enum": [
              "open",
              "filled",
              "cancelled",
              "expired"
            ],
            "description": "RFQ status filter, if applicable",
            "nullable": true
          },
          "subaccount_id": {
            "title": "subaccount_id",
            "type": "integer",
            "description": "Subaccount ID for auth purposes, returned data will be scoped to this subaccount."
          },
          "to_timestamp": {
            "title": "to_timestamp",
            "type": "integer",
            "default": 18446744073709552000,
            "description": "Latest `last_update_timestamp` to filter by (in ms since Unix epoch). If not provied, defaults to returning all data up to current time."
          }
        },
        "required": [
          "subaccount_id"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PrivateGetRfqsResponseSchema": {
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
            "$ref": "#/components/schemas/PrivateGetRfqsResultSchema"
          }
        },
        "required": [
          "id",
          "result"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PrivateGetRfqsResultSchema": {
        "properties": {
          "pagination": {
            "$ref": "#/components/schemas/PaginationInfoSchema"
          },
          "rfqs": {
            "title": "rfqs",
            "type": "array",
            "description": "RFQs matching filter criteria",
            "items": {
              "$ref": "#/components/schemas/RFQResultSchema"
            }
          }
        },
        "required": [
          "pagination",
          "rfqs"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "RFQResultSchema": {
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