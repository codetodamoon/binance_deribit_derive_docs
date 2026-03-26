# Get Interest History

Get subaccount interest payment history.
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
    "/private/get_interest_history": {
      "post": {
        "tags": [
          "Private"
        ],
        "summary": "Get Interest History",
        "description": "Get subaccount interest payment history.\nRequired minimum session key permission level is `read_only`",
        "responses": {
          "200": {
            "description": "successful operation",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/PrivateGetInterestHistoryResponseSchema"
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
                "$ref": "#/components/schemas/PrivateGetInterestHistoryParamsSchema"
              }
            }
          }
        }
      }
    }
  },
  "components": {
    "schemas": {
      "PrivateGetInterestHistoryParamsSchema": {
        "properties": {
          "end_timestamp": {
            "title": "end_timestamp",
            "type": "integer",
            "default": 9223372036854776000,
            "description": "End timestamp of the event history (default current time)"
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
      "PrivateGetInterestHistoryResponseSchema": {
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
            "$ref": "#/components/schemas/PrivateGetInterestHistoryResultSchema"
          }
        },
        "required": [
          "id",
          "result"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PrivateGetInterestHistoryResultSchema": {
        "properties": {
          "events": {
            "title": "events",
            "type": "array",
            "description": "List of interest payments",
            "items": {
              "$ref": "#/components/schemas/InterestPaymentSchema"
            }
          }
        },
        "required": [
          "events"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "InterestPaymentSchema": {
        "properties": {
          "interest": {
            "title": "interest",
            "type": "string",
            "format": "decimal",
            "description": "Dollar interest paid (if negative) or received (if positive) by the subaccount"
          },
          "timestamp": {
            "title": "timestamp",
            "type": "integer",
            "description": "Timestamp of the interest payment (in ms since UNIX epoch)"
          }
        },
        "required": [
          "interest",
          "timestamp"
        ],
        "type": "object",
        "additionalProperties": false
      }
    }
  }
}
```