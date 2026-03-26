# Build Register Session Key Tx

Build a signable transaction params dictionary.

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
    "/public/build_register_session_key_tx": {
      "post": {
        "tags": [
          "Public"
        ],
        "summary": "Build Register Session Key Tx",
        "description": "Build a signable transaction params dictionary.",
        "responses": {
          "200": {
            "description": "successful operation",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/PublicBuildRegisterSessionKeyTxResponseSchema"
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
                "$ref": "#/components/schemas/PublicBuildRegisterSessionKeyTxParamsSchema"
              }
            }
          }
        }
      }
    }
  },
  "components": {
    "schemas": {
      "PublicBuildRegisterSessionKeyTxParamsSchema": {
        "properties": {
          "expiry_sec": {
            "title": "expiry_sec",
            "type": "integer",
            "description": "Expiry of the session key"
          },
          "gas": {
            "title": "gas",
            "type": "integer",
            "default": null,
            "description": "Gas allowance for transaction. If none, will use estimateGas * 150%",
            "nullable": true
          },
          "nonce": {
            "title": "nonce",
            "type": "integer",
            "default": null,
            "description": "Wallet's transaction count, If none, will use eth.getTransactionCount()",
            "nullable": true
          },
          "public_session_key": {
            "title": "public_session_key",
            "type": "string",
            "description": "Session key in the form of an Ethereum EOA"
          },
          "wallet": {
            "title": "wallet",
            "type": "string",
            "description": "Ethereum wallet address of account"
          }
        },
        "required": [
          "expiry_sec",
          "gas",
          "nonce",
          "public_session_key",
          "wallet"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PublicBuildRegisterSessionKeyTxResponseSchema": {
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
            "$ref": "#/components/schemas/PublicBuildRegisterSessionKeyTxResultSchema"
          }
        },
        "required": [
          "id",
          "result"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PublicBuildRegisterSessionKeyTxResultSchema": {
        "properties": {
          "tx_params": {
            "title": "tx_params",
            "type": "object",
            "description": "Transaction params in dictionary form, same as `TxParams` in `web3.py`",
            "additionalProperties": {}
          }
        },
        "required": [
          "tx_params"
        ],
        "type": "object",
        "additionalProperties": false
      }
    }
  }
}
```