# Get Funding History

Get subaccount funding history.<br /><br />DB: read replica
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
    "/private/get_funding_history": {
      "post": {
        "tags": [
          "Private"
        ],
        "summary": "Get Funding History",
        "description": "Get subaccount funding history.<br /><br />DB: read replica\nRequired minimum session key permission level is `read_only`",
        "responses": {
          "200": {
            "description": "successful operation",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/PrivateGetFundingHistoryResponseSchema"
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
                "$ref": "#/components/schemas/PrivateGetFundingHistoryParamsSchema"
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
      "PrivateGetFundingHistoryParamsSchema": {
        "properties": {
          "end_timestamp": {
            "title": "end_timestamp",
            "type": "integer",
            "default": 9223372036854776000,
            "description": "End timestamp of the event history (default current time)"
          },
          "instrument_name": {
            "title": "instrument_name",
            "type": "string",
            "default": null,
            "description": "Instrument name (returns history for all perpetuals if not provided)",
            "nullable": true
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
          "start_timestamp": {
            "title": "start_timestamp",
            "type": "integer",
            "default": 0,
            "description": "Start timestamp of the event history (default 0)"
          },
          "subaccount_id": {
            "title": "subaccount_id",
            "type": "integer",
            "description": "Subaccount id"
          }
        },
        "required": [
          "subaccount_id"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PrivateGetFundingHistoryResponseSchema": {
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
            "$ref": "#/components/schemas/PrivateGetFundingHistoryResultSchema"
          }
        },
        "required": [
          "id",
          "result"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PrivateGetFundingHistoryResultSchema": {
        "properties": {
          "events": {
            "title": "events",
            "type": "array",
            "description": "List of funding payments",
            "items": {
              "$ref": "#/components/schemas/FundingPaymentSchema"
            }
          },
          "pagination": {
            "$ref": "#/components/schemas/PaginationInfoSchema"
          }
        },
        "required": [
          "events",
          "pagination"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "FundingPaymentSchema": {
        "properties": {
          "funding": {
            "title": "funding",
            "type": "string",
            "format": "decimal",
            "description": "Dollar funding paid (if negative) or received (if positive) by the subaccount"
          },
          "instrument_name": {
            "title": "instrument_name",
            "type": "string",
            "description": "Instrument name"
          },
          "pnl": {
            "title": "pnl",
            "type": "string",
            "format": "decimal",
            "description": "Cashflow from the perp PnL settlement"
          },
          "timestamp": {
            "title": "timestamp",
            "type": "integer",
            "description": "Timestamp of the funding payment (in ms since UNIX epoch)"
          }
        },
        "required": [
          "funding",
          "instrument_name",
          "pnl",
          "timestamp"
        ],
        "type": "object",
        "additionalProperties": false
      }
    }
  }
}
```