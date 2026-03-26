# Create Subaccount Debug

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
    "/public/create_subaccount_debug": {
      "post": {
        "tags": [
          "Public"
        ],
        "summary": "Create Subaccount Debug",
        "description": "Used for debugging only, do not use in production. Will return the incremental encoded and hashed data.<br /><br />See guides in Documentation for more.",
        "responses": {
          "200": {
            "description": "successful operation",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/PublicCreateSubaccountDebugResponseSchema"
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
                "$ref": "#/components/schemas/PublicCreateSubaccountDebugParamsSchema"
              }
            }
          }
        }
      }
    }
  },
  "components": {
    "schemas": {
      "PublicCreateSubaccountDebugParamsSchema": {
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
          "currency": {
            "title": "currency",
            "type": "string",
            "default": null,
            "description": "Base currency of the subaccount (only for `PM`)",
            "nullable": true
          },
          "margin_type": {
            "title": "margin_type",
            "type": "string",
            "enum": [
              "PM",
              "SM",
              "PM2"
            ],
            "description": "`PM` (Portfolio Margin), `PM2 (Portfolio Margin 2), or `SM` (Standard Margin))"
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
            "description": "Ethereum wallet address that is signing the deposit"
          },
          "wallet": {
            "title": "wallet",
            "type": "string",
            "description": "Ethereum wallet address"
          }
        },
        "required": [
          "amount",
          "asset_name",
          "margin_type",
          "nonce",
          "signature_expiry_sec",
          "signer",
          "wallet"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PublicCreateSubaccountDebugResponseSchema": {
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
            "$ref": "#/components/schemas/PublicCreateSubaccountDebugResultSchema"
          }
        },
        "required": [
          "id",
          "result"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PublicCreateSubaccountDebugResultSchema": {
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