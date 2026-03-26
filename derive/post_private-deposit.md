# Deposit

Deposit an asset to a subaccount.<br /><br />See `public/deposit_debug' for debugging invalid signature issues or go to guides in Documentation.
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
    "/private/deposit": {
      "post": {
        "tags": [
          "Private"
        ],
        "summary": "Deposit",
        "description": "Deposit an asset to a subaccount.<br /><br />See `public/deposit_debug' for debugging invalid signature issues or go to guides in Documentation.\nRequired minimum session key permission level is `admin`",
        "responses": {
          "200": {
            "description": "successful operation",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/PrivateDepositResponseSchema"
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
                "$ref": "#/components/schemas/PrivateDepositParamsSchema"
              }
            }
          }
        }
      }
    }
  },
  "components": {
    "schemas": {
      "PrivateDepositParamsSchema": {
        "properties": {
          "amount": {
            "title": "amount",
            "type": "string",
            "format": "decimal",
            "description": "Amount of the asset to deposit"
          },
          "asset_name": {
            "title": "asset_name",
            "type": "string",
            "description": "Name of asset to deposit"
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
          "signature": {
            "title": "signature",
            "type": "string",
            "description": "Ethereum signature of the deposit"
          },
          "signature_expiry_sec": {
            "title": "signature_expiry_sec",
            "type": "integer",
            "description": "Unix timestamp in seconds. Expiry MUST be >5min from now"
          },
          "signer": {
            "title": "signer",
            "type": "string",
            "description": "Ethereum wallet address that is signing the deposit"
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
          "signature",
          "signature_expiry_sec",
          "signer",
          "subaccount_id"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PrivateDepositResponseSchema": {
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
            "$ref": "#/components/schemas/PrivateDepositResultSchema"
          }
        },
        "required": [
          "id",
          "result"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PrivateDepositResultSchema": {
        "properties": {
          "status": {
            "title": "status",
            "type": "string",
            "description": "`requested`"
          },
          "transaction_id": {
            "title": "transaction_id",
            "type": "string",
            "format": "uuid",
            "description": "Transaction id of the deposit"
          }
        },
        "required": [
          "status",
          "transaction_id"
        ],
        "type": "object",
        "additionalProperties": false
      }
    }
  }
}
```