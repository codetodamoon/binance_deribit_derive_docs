# Cancel All Trigger Orders

Cancel all trigger orders for this subaccount.<br /><br />Also used by cancel_all in WS.
Required minimum session key permission level is `admin`

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
    "/private/cancel_all_trigger_orders": {
      "post": {
        "tags": [
          "Private"
        ],
        "summary": "Cancel All Trigger Orders",
        "description": "Cancel all trigger orders for this subaccount.<br /><br />Also used by cancel_all in WS.\nRequired minimum session key permission level is `admin`",
        "responses": {
          "200": {
            "description": "successful operation",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/PrivateCancelAllTriggerOrdersResponseSchema"
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
                "$ref": "#/components/schemas/PrivateCancelAllTriggerOrdersParamsSchema"
              }
            }
          }
        }
      }
    }
  },
  "components": {
    "schemas": {
      "PrivateCancelAllTriggerOrdersParamsSchema": {
        "properties": {
          "subaccount_id": {
            "title": "subaccount_id",
            "type": "integer"
          }
        },
        "required": [
          "subaccount_id"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PrivateCancelAllTriggerOrdersResponseSchema": {
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
            "title": "result",
            "type": "string",
            "enum": [
              "ok"
            ],
            "description": ""
          }
        },
        "required": [
          "id",
          "result"
        ],
        "type": "object",
        "additionalProperties": false
      }
    }
  }
}
```