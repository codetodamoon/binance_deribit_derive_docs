# Register Session Key

Register or update expiry of an existing session key.<br />Currently, this only supports creating admin level session keys.<br />Keys with fewer permissions are registered via `/register_scoped_session_key`<br /><br />Expiries updated on admin session keys may not happen immediately due to waiting for the onchain transaction to settle.

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
    "/public/register_session_key": {
      "post": {
        "tags": [
          "Public"
        ],
        "summary": "Register Session Key",
        "description": "Register or update expiry of an existing session key.<br />Currently, this only supports creating admin level session keys.<br />Keys with fewer permissions are registered via `/register_scoped_session_key`<br /><br />Expiries updated on admin session keys may not happen immediately due to waiting for the onchain transaction to settle.",
        "responses": {
          "200": {
            "description": "successful operation",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/PublicRegisterSessionKeyResponseSchema"
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
                "$ref": "#/components/schemas/PublicRegisterSessionKeyParamsSchema"
              }
            }
          }
        }
      }
    }
  },
  "components": {
    "schemas": {
      "PublicRegisterSessionKeyParamsSchema": {
        "properties": {
          "expiry_sec": {
            "title": "expiry_sec",
            "type": "integer",
            "description": "Expiry of the session key"
          },
          "label": {
            "title": "label",
            "type": "string",
            "description": "Ethereum wallet address"
          },
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
          "expiry_sec",
          "label",
          "public_session_key",
          "signed_raw_tx",
          "wallet"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PublicRegisterSessionKeyResponseSchema": {
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
            "$ref": "#/components/schemas/PublicRegisterSessionKeyResultSchema"
          }
        },
        "required": [
          "id",
          "result"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PublicRegisterSessionKeyResultSchema": {
        "properties": {
          "label": {
            "title": "label",
            "type": "string",
            "description": "User-defined session key label"
          },
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
          "label",
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