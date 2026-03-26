# Get Subaccount Value History

Get the value history of a subaccount
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
    "/private/get_subaccount_value_history": {
      "post": {
        "tags": [
          "Private"
        ],
        "summary": "Get Subaccount Value History",
        "description": "Get the value history of a subaccount\nRequired minimum session key permission level is `read_only`",
        "responses": {
          "200": {
            "description": "successful operation",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/PrivateGetSubaccountValueHistoryResponseSchema"
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
                "$ref": "#/components/schemas/PrivateGetSubaccountValueHistoryParamsSchema"
              }
            }
          }
        }
      }
    }
  },
  "components": {
    "schemas": {
      "PrivateGetSubaccountValueHistoryParamsSchema": {
        "properties": {
          "end_timestamp": {
            "title": "end_timestamp",
            "type": "integer",
            "description": "End timestamp"
          },
          "period": {
            "title": "period",
            "type": "integer",
            "description": "Period"
          },
          "start_timestamp": {
            "title": "start_timestamp",
            "type": "integer",
            "description": "Start timestamp"
          },
          "subaccount_id": {
            "title": "subaccount_id",
            "type": "integer",
            "description": "Subaccount_id"
          }
        },
        "required": [
          "end_timestamp",
          "period",
          "start_timestamp",
          "subaccount_id"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PrivateGetSubaccountValueHistoryResponseSchema": {
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
            "$ref": "#/components/schemas/PrivateGetSubaccountValueHistoryResultSchema"
          }
        },
        "required": [
          "id",
          "result"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PrivateGetSubaccountValueHistoryResultSchema": {
        "properties": {
          "subaccount_id": {
            "title": "subaccount_id",
            "type": "integer",
            "description": "Subaccount_id"
          },
          "subaccount_value_history": {
            "title": "subaccount_value_history",
            "type": "array",
            "description": "Subaccount value history",
            "items": {
              "$ref": "#/components/schemas/SubAccountValueHistoryResponseSchema"
            }
          }
        },
        "required": [
          "subaccount_id",
          "subaccount_value_history"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "SubAccountValueHistoryResponseSchema": {
        "properties": {
          "subaccount_value": {
            "title": "subaccount_value",
            "type": "string",
            "format": "decimal",
            "description": "Total mark-to-market value of all positions and collaterals"
          },
          "timestamp": {
            "title": "timestamp",
            "type": "integer",
            "description": "Timestamp of when the subaccount value was recorded into the database"
          }
        },
        "required": [
          "subaccount_value",
          "timestamp"
        ],
        "type": "object",
        "additionalProperties": false
      }
    }
  }
}
```