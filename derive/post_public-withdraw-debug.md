# Withdraw Debug

Used for debugging only, do not use in production. Will return the incremental encoded and hashed data.<br /><br />See guides in Documentation for more.

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
    "/public/withdraw_debug": {
      "post": {
        "tags": [
          "Public"
        ],
        "summary": "Withdraw Debug",
        "description": "Used for debugging only, do not use in production. Will return the incremental encoded and hashed data.<br /><br />See guides in Documentation for more.",
        "responses": {
          "200": {
            "description": "successful operation",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/PublicWithdrawDebugResponseSchema"
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
                "$ref": "#/components/schemas/PublicWithdrawDebugParamsSchema"
              }
            }
          }
        }
      }
    }
  },
  "components": {
    "schemas": {
      "PublicWithdrawDebugParamsSchema": {
        "properties": {
          "amount": {
            "title": "amount",
            "type": "string",
            "format": "decimal",
            "description": "Amount of the asset to withdraw"
          },
          "asset_name": {
            "title": "asset_name",
            "type": "string",
            "description": "Name of asset to withdraw"
          },
          "is_atomic_signing": {
            "title": "is_atomic_signing",
            "type": "boolean",
            "default": false,
            "description": "Used by vaults to determine whether the signature is an EIP-1271 signature"
          },
          "nonce": {
            "title": "nonce",
            "type": "integer",
            "description": "Unique nonce defined as (UTC_timestamp in ms)(random_number_up_to_6_digits) (e.g. 1695836058725001, where 001 is the random number)"
          },
          "signature_expiry_sec": {
            "title": "signature_expiry_sec",
            "type": "integer",
            "description": "Unix timestamp in seconds. Expiry MUST be >5min from now"
          },
          "signer": {
            "title": "signer",
            "type": "string",
            "description": "Ethereum wallet address that is signing the withdraw"
          },
          "subaccount_id": {
            "title": "subaccount_id",
            "type": "integer",
            "description": "Subaccount_id"
          }
        },
        "required": [
          "amount",
          "asset_name",
          "nonce",
          "signature_expiry_sec",
          "signer",
          "subaccount_id"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PublicWithdrawDebugResponseSchema": {
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
            "$ref": "#/components/schemas/PublicWithdrawDebugResultSchema"
          }
        },
        "required": [
          "id",
          "result"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PublicWithdrawDebugResultSchema": {
        "properties": {
          "action_hash": {
            "title": "action_hash",
            "type": "string",
            "description": "Keccak hashed action data"
          },
          "encoded_data": {
            "title": "encoded_data",
            "type": "string",
            "description": "ABI encoded deposit data"
          },
          "encoded_data_hashed": {
            "title": "encoded_data_hashed",
            "type": "string",
            "description": "Keccak hashed encoded_data"
          },
          "typed_data_hash": {
            "title": "typed_data_hash",
            "type": "string",
            "description": "EIP 712 typed data hash"
          }
        },
        "required": [
          "action_hash",
          "encoded_data",
          "encoded_data_hashed",
          "typed_data_hash"
        ],
        "type": "object",
        "additionalProperties": false
      }
    }
  }
}
```