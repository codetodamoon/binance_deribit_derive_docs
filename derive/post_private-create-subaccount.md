# Create Subaccount

Create a new subaccount under a given wallet, and deposit an asset into that subaccount.<br /><br />See `public/create_subaccount_debug` for debugging invalid signature issues or go to guides in Documentation.
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
    "/private/create_subaccount": {
      "post": {
        "tags": [
          "Private"
        ],
        "summary": "Create Subaccount",
        "description": "Create a new subaccount under a given wallet, and deposit an asset into that subaccount.<br /><br />See `public/create_subaccount_debug` for debugging invalid signature issues or go to guides in Documentation.\nRequired minimum session key permission level is `admin`",
        "responses": {
          "200": {
            "description": "successful operation",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/PrivateCreateSubaccountResponseSchema"
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
                "$ref": "#/components/schemas/PrivateCreateSubaccountParamsSchema"
              }
            }
          }
        }
      }
    }
  },
  "components": {
    "schemas": {
      "PrivateCreateSubaccountParamsSchema": {
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
          "signature",
          "signature_expiry_sec",
          "signer",
          "wallet"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PrivateCreateSubaccountResponseSchema": {
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
            "$ref": "#/components/schemas/PrivateCreateSubaccountResultSchema"
          }
        },
        "required": [
          "id",
          "result"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PrivateCreateSubaccountResultSchema": {
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
            "description": "Transaction id of the request"
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