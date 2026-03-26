# Transfer Erc20

Transfer ERC20 assets from one subaccount to another (e.g. USDC or ETH).<br /><br />For transfering positions (e.g. options or perps), use `private/transfer_position` instead.
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
    "/private/transfer_erc20": {
      "post": {
        "tags": [
          "Private"
        ],
        "summary": "Transfer Erc20",
        "description": "Transfer ERC20 assets from one subaccount to another (e.g. USDC or ETH).<br /><br />For transfering positions (e.g. options or perps), use `private/transfer_position` instead.\nRequired minimum session key permission level is `admin`",
        "responses": {
          "200": {
            "description": "successful operation",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/PrivateTransferErc20ResponseSchema"
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
                "$ref": "#/components/schemas/PrivateTransferErc20ParamsSchema"
              }
            }
          }
        }
      }
    }
  },
  "components": {
    "schemas": {
      "PrivateTransferErc20ParamsSchema": {
        "properties": {
          "recipient_details": {
            "$ref": "#/components/schemas/SignatureDetailsSchema"
          },
          "recipient_subaccount_id": {
            "title": "recipient_subaccount_id",
            "type": "integer",
            "description": "Subaccount_id of the recipient"
          },
          "sender_details": {
            "$ref": "#/components/schemas/SignatureDetailsSchema"
          },
          "subaccount_id": {
            "title": "subaccount_id",
            "type": "integer",
            "description": "Subaccount_id"
          },
          "transfer": {
            "$ref": "#/components/schemas/TransferDetailsSchema"
          }
        },
        "required": [
          "recipient_details",
          "recipient_subaccount_id",
          "sender_details",
          "subaccount_id",
          "transfer"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "SignatureDetailsSchema": {
        "properties": {
          "nonce": {
            "title": "nonce",
            "type": "integer",
            "description": "Unique nonce defined as (UTC_timestamp in ms)(random_number_up_to_6_digits) (e.g. 1695836058725001, where 001 is the random number)"
          },
          "signature": {
            "title": "signature",
            "type": "string",
            "description": "Ethereum signature of the transfer"
          },
          "signature_expiry_sec": {
            "title": "signature_expiry_sec",
            "type": "integer",
            "description": "Unix timestamp in seconds. Expiry MUST be >5min from now"
          },
          "signer": {
            "title": "signer",
            "type": "string",
            "description": "Ethereum wallet address that is signing the transfer"
          }
        },
        "required": [
          "nonce",
          "signature",
          "signature_expiry_sec",
          "signer"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "TransferDetailsSchema": {
        "properties": {
          "address": {
            "title": "address",
            "type": "string",
            "description": "Ethereum address of the asset being transferred"
          },
          "amount": {
            "title": "amount",
            "type": "string",
            "format": "decimal",
            "description": "Amount to transfer"
          },
          "sub_id": {
            "title": "sub_id",
            "type": "integer",
            "description": "Sub ID of the asset being transferred"
          }
        },
        "required": [
          "address",
          "amount",
          "sub_id"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PrivateTransferErc20ResponseSchema": {
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
            "$ref": "#/components/schemas/PrivateTransferErc20ResultSchema"
          }
        },
        "required": [
          "id",
          "result"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PrivateTransferErc20ResultSchema": {
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
            "description": "Transaction id of the transfer"
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