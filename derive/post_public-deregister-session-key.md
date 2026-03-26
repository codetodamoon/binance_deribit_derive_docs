# Deregister Session Key

Used for de-registering admin scoped keys. For other scopes, use `/edit_session_key`.

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
    "/public/deregister_session_key": {
      "post": {
        "tags": [
          "Public"
        ],
        "summary": "Deregister Session Key",
        "description": "Used for de-registering admin scoped keys. For other scopes, use `/edit_session_key`.",
        "responses": {
          "200": {
            "description": "successful operation",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/PublicDeregisterSessionKeyResponseSchema"
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
                "$ref": "#/components/schemas/PublicDeregisterSessionKeyParamsSchema"
              }
            }
          }
        }
      }
    }
  },
  "components": {
    "schemas": {
      "PublicDeregisterSessionKeyParamsSchema": {
        "properties": {
          "public_session_key": {
            "title": "public_session_key",
            "type": "string",
            "description": "Session key in the form of an Ethereum EOA"
          },
          "signed_raw_tx": {
            "title": "signed_raw_tx",
            "type": "string",
            "description": "A signed RLP encoded ETH transaction in form of a hex string (same as `w3.eth.account.sign_transaction(unsigned_tx, private_key).rawTransaction.hex()`)"
          },
          "wallet": {
            "title": "wallet",
            "type": "string",
            "description": "Ethereum wallet address of account"
          }
        },
        "required": [
          "public_session_key",
          "signed_raw_tx",
          "wallet"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PublicDeregisterSessionKeyResponseSchema": {
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
            "$ref": "#/components/schemas/PublicDeregisterSessionKeyResultSchema"
          }
        },
        "required": [
          "id",
          "result"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PublicDeregisterSessionKeyResultSchema": {
        "properties": {
          "public_session_key": {
            "title": "public_session_key",
            "type": "string",
            "description": "Session key in the form of an Ethereum EOA"
          },
          "transaction_id": {
            "title": "transaction_id",
            "type": "string",
            "format": "uuid",
            "description": "ID to lookup status of transaction"
          }
        },
        "required": [
          "public_session_key",
          "transaction_id"
        ],
        "type": "object",
        "additionalProperties": false
      }
    }
  }
}
```