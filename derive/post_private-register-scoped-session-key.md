# Register Scoped Session Key

Registers a new session key bounded to a scope without a transaction attached.<br />If you want to register an admin key, you must provide a signed raw transaction.
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
    "/private/register_scoped_session_key": {
      "post": {
        "tags": [
          "Private"
        ],
        "summary": "Register Scoped Session Key",
        "description": "Registers a new session key bounded to a scope without a transaction attached.<br />If you want to register an admin key, you must provide a signed raw transaction.\nRequired minimum session key permission level is `account`",
        "responses": {
          "200": {
            "description": "successful operation",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/PrivateRegisterScopedSessionKeyResponseSchema"
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
                "$ref": "#/components/schemas/PrivateRegisterScopedSessionKeyParamsSchema"
              }
            }
          }
        }
      }
    }
  },
  "components": {
    "schemas": {
      "PrivateRegisterScopedSessionKeyParamsSchema": {
        "properties": {
          "expiry_sec": {
            "title": "expiry_sec",
            "type": "integer",
            "description": "Expiry of the session key"
          },
          "ip_whitelist": {
            "title": "ip_whitelist",
            "type": "array",
            "default": null,
            "description": "List of whitelisted IPs, if empty then any IP is allowed.",
            "items": {
              "title": "ip_whitelist",
              "type": "string"
            },
            "nullable": true
          },
          "label": {
            "title": "label",
            "type": "string",
            "default": null,
            "description": "User-defined session key label",
            "nullable": true
          },
          "public_session_key": {
            "title": "public_session_key",
            "type": "string",
            "description": "Session key in the form of an Ethereum EOA"
          },
          "scope": {
            "title": "scope",
            "type": "string",
            "default": "read_only",
            "enum": [
              "admin",
              "account",
              "read_only"
            ],
            "description": "Scope of the session key. Defaults to READ_ONLY level permissions. "
          },
          "signed_raw_tx": {
            "title": "signed_raw_tx",
            "type": "string",
            "default": null,
            "description": "A signed RLP encoded ETH transaction in form of a hex string (same as `w3.eth.account.sign_transaction(unsigned_tx, private_key).rawTransaction.hex()`) Must be included if the scope is ADMIN.",
            "nullable": true
          },
          "wallet": {
            "title": "wallet",
            "type": "string",
            "description": "Ethereum wallet address of account"
          }
        },
        "required": [
          "expiry_sec",
          "public_session_key",
          "wallet"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PrivateRegisterScopedSessionKeyResponseSchema": {
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
            "$ref": "#/components/schemas/PrivateRegisterScopedSessionKeyResultSchema"
          }
        },
        "required": [
          "id",
          "result"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PrivateRegisterScopedSessionKeyResultSchema": {
        "properties": {
          "expiry_sec": {
            "title": "expiry_sec",
            "type": "integer",
            "description": "Session key expiry timestamp in sec"
          },
          "ip_whitelist": {
            "title": "ip_whitelist",
            "type": "array",
            "default": null,
            "description": "List of whitelisted IPs, if empty then any IP is allowed.",
            "items": {
              "title": "ip_whitelist",
              "type": "string"
            },
            "nullable": true
          },
          "label": {
            "title": "label",
            "type": "string",
            "default": null,
            "description": "User-defined session key label",
            "nullable": true
          },
          "public_session_key": {
            "title": "public_session_key",
            "type": "string",
            "description": "Session key in the form of an Ethereum EOA"
          },
          "scope": {
            "title": "scope",
            "type": "string",
            "enum": [
              "admin",
              "account",
              "read_only"
            ],
            "description": "Session key permission level scope"
          },
          "transaction_id": {
            "title": "transaction_id",
            "type": "string",
            "format": "uuid",
            "default": null,
            "description": "ID to lookup status of transaction if signed_raw_tx is provided",
            "nullable": true
          }
        },
        "required": [
          "expiry_sec",
          "ip_whitelist",
          "label",
          "public_session_key",
          "scope",
          "transaction_id"
        ],
        "type": "object",
        "additionalProperties": false
      }
    }
  }
}
```