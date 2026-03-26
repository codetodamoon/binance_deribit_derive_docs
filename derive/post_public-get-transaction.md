# Get Transaction

Used for getting a transaction by its transaction id

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
    "/public/get_transaction": {
      "post": {
        "tags": [
          "Public"
        ],
        "summary": "Get Transaction",
        "description": "Used for getting a transaction by its transaction id",
        "responses": {
          "200": {
            "description": "successful operation",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/PublicGetTransactionResponseSchema"
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
                "$ref": "#/components/schemas/PublicGetTransactionParamsSchema"
              }
            }
          }
        }
      }
    }
  },
  "components": {
    "schemas": {
      "PublicGetTransactionParamsSchema": {
        "properties": {
          "transaction_id": {
            "title": "transaction_id",
            "type": "string",
            "format": "uuid",
            "description": "transaction_id of the transaction to get"
          }
        },
        "required": [
          "transaction_id"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PublicGetTransactionResponseSchema": {
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
            "$ref": "#/components/schemas/PublicGetTransactionResultSchema"
          }
        },
        "required": [
          "id",
          "result"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PublicGetTransactionResultSchema": {
        "properties": {
          "data": {
            "title": "data",
            "type": "string",
            "description": "Data used to create transaction"
          },
          "error_log": {
            "title": "error_log",
            "type": "string",
            "default": null,
            "description": "Error log if failed tx",
            "nullable": true
          },
          "status": {
            "title": "status",
            "type": "string",
            "enum": [
              "requested",
              "pending",
              "settled",
              "reverted",
              "ignored",
              "timed_out"
            ],
            "description": "Status of the transaction"
          },
          "transaction_hash": {
            "title": "transaction_hash",
            "type": "string",
            "default": null,
            "description": "Transaction hash of a pending tx",
            "nullable": true
          }
        },
        "required": [
          "data",
          "error_log",
          "status",
          "transaction_hash"
        ],
        "type": "object",
        "additionalProperties": false
      }
    }
  }
}
```