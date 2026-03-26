# Get Withdrawal History

Get subaccount withdrawal history.
Required minimum session key permission level is `read_only`

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
    "/private/get_withdrawal_history": {
      "post": {
        "tags": [
          "Private"
        ],
        "summary": "Get Withdrawal History",
        "description": "Get subaccount withdrawal history.\nRequired minimum session key permission level is `read_only`",
        "responses": {
          "200": {
            "description": "successful operation",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/PrivateGetWithdrawalHistoryResponseSchema"
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
                "$ref": "#/components/schemas/PrivateGetWithdrawalHistoryParamsSchema"
              }
            }
          }
        }
      }
    }
  },
  "components": {
    "schemas": {
      "PrivateGetWithdrawalHistoryParamsSchema": {
        "properties": {
          "end_timestamp": {
            "title": "end_timestamp",
            "type": "integer",
            "default": 9223372036854776000,
            "description": "End timestamp of the event history (default current time)"
          },
          "start_timestamp": {
            "title": "start_timestamp",
            "type": "integer",
            "default": 0,
            "description": "Start timestamp of the event history (default 0)"
          },
          "subaccount_id": {
            "title": "subaccount_id",
            "type": "integer",
            "description": "Subaccount id"
          }
        },
        "required": [
          "subaccount_id"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PrivateGetWithdrawalHistoryResponseSchema": {
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
            "$ref": "#/components/schemas/PrivateGetWithdrawalHistoryResultSchema"
          }
        },
        "required": [
          "id",
          "result"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PrivateGetWithdrawalHistoryResultSchema": {
        "properties": {
          "events": {
            "title": "events",
            "type": "array",
            "description": "List of withdrawals",
            "items": {
              "$ref": "#/components/schemas/WithdrawalSchema"
            }
          }
        },
        "required": [
          "events"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "WithdrawalSchema": {
        "properties": {
          "amount": {
            "title": "amount",
            "type": "string",
            "format": "decimal",
            "description": "Amount withdrawn by the subaccount"
          },
          "asset": {
            "title": "asset",
            "type": "string",
            "description": "Asset withdrawn"
          },
          "error_log": {
            "title": "error_log",
            "type": "object",
            "default": null,
            "description": "If failed, error log for reason",
            "additionalProperties": {},
            "nullable": true
          },
          "timestamp": {
            "title": "timestamp",
            "type": "integer",
            "description": "Timestamp of the withdrawal (in ms since UNIX epoch)"
          },
          "tx_hash": {
            "title": "tx_hash",
            "type": "string",
            "description": "Hash of the transaction that withdrew the funds"
          },
          "tx_status": {
            "title": "tx_status",
            "type": "string",
            "enum": [
              "requested",
              "pending",
              "settled",
              "reverted",
              "ignored",
              "timed_out"
            ],
            "description": "Status of the transaction that deposited the funds"
          }
        },
        "required": [
          "amount",
          "asset",
          "error_log",
          "timestamp",
          "tx_hash",
          "tx_status"
        ],
        "type": "object",
        "additionalProperties": false
      }
    }
  }
}
```