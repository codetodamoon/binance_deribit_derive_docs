# Cancel By Nonce

Cancel a single order by nonce. Uses up that nonce if the order does not exist, so any future orders with that nonce will fail
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
    "/private/cancel_by_nonce": {
      "post": {
        "tags": [
          "Private"
        ],
        "summary": "Cancel By Nonce",
        "description": "Cancel a single order by nonce. Uses up that nonce if the order does not exist, so any future orders with that nonce will fail\nRequired minimum session key permission level is `admin`",
        "responses": {
          "200": {
            "description": "successful operation",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/PrivateCancelByNonceResponseSchema"
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
                "$ref": "#/components/schemas/PrivateCancelByNonceParamsSchema"
              }
            }
          }
        }
      }
    }
  },
  "components": {
    "schemas": {
      "PrivateCancelByNonceParamsSchema": {
        "properties": {
          "instrument_name": {
            "title": "instrument_name",
            "type": "string",
            "description": "Instrument name"
          },
          "nonce": {
            "title": "nonce",
            "type": "integer",
            "description": "Cancel an order with this nonce"
          },
          "subaccount_id": {
            "title": "subaccount_id",
            "type": "integer",
            "description": "Subaccount ID"
          },
          "wallet": {
            "title": "wallet",
            "type": "string",
            "description": "Wallet address"
          }
        },
        "required": [
          "instrument_name",
          "nonce",
          "subaccount_id",
          "wallet"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PrivateCancelByNonceResponseSchema": {
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
            "$ref": "#/components/schemas/PrivateCancelByNonceResultSchema"
          }
        },
        "required": [
          "id",
          "result"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PrivateCancelByNonceResultSchema": {
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