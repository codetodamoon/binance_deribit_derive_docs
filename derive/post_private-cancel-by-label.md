# Cancel By Label

Cancel all open orders for a given subaccount and a given label.  If instrument_name is provided, only orders for that instrument will be cancelled.
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
    "/private/cancel_by_label": {
      "post": {
        "tags": [
          "Private"
        ],
        "summary": "Cancel By Label",
        "description": "Cancel all open orders for a given subaccount and a given label.  If instrument_name is provided, only orders for that instrument will be cancelled.\nRequired minimum session key permission level is `admin`",
        "responses": {
          "200": {
            "description": "successful operation",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/PrivateCancelByLabelResponseSchema"
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
                "$ref": "#/components/schemas/PrivateCancelByLabelParamsSchema"
              }
            }
          }
        }
      }
    }
  },
  "components": {
    "schemas": {
      "PrivateCancelByLabelParamsSchema": {
        "properties": {
          "instrument_name": {
            "title": "instrument_name",
            "type": "string",
            "default": null,
            "description": "Instrument name. If not provided, all orders for all instruments with the label will be cancelled.  If provided, request counts as a regular matching request for ratelimit purposes.",
            "nullable": true
          },
          "label": {
            "title": "label",
            "type": "string",
            "description": "Cancel all orders for this label"
          },
          "subaccount_id": {
            "title": "subaccount_id",
            "type": "integer",
            "description": "Subaccount ID"
          }
        },
        "required": [
          "label",
          "subaccount_id"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PrivateCancelByLabelResponseSchema": {
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
            "$ref": "#/components/schemas/PrivateCancelByLabelResultSchema"
          }
        },
        "required": [
          "id",
          "result"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PrivateCancelByLabelResultSchema": {
        "properties": {
          "cancelled_orders": {
            "title": "cancelled_orders",
            "type": "integer",
            "description": "Number of cancelled orders"
          }
        },
        "required": [
          "cancelled_orders"
        ],
        "type": "object",
        "additionalProperties": false
      }
    }
  }
}
```