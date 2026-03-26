# Execute Quote Debug

Sends a quote in response to an RFQ request.<br />The legs supplied in the parameters must exactly match those in the RFQ.

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
    "/public/execute_quote_debug": {
      "post": {
        "tags": [
          "Public"
        ],
        "summary": "Execute Quote Debug",
        "description": "Sends a quote in response to an RFQ request.<br />The legs supplied in the parameters must exactly match those in the RFQ.",
        "responses": {
          "200": {
            "description": "successful operation",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/PublicExecuteQuoteDebugResponseSchema"
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
                "$ref": "#/components/schemas/PublicExecuteQuoteDebugParamsSchema"
              }
            }
          }
        }
      }
    }
  },
  "components": {
    "schemas": {
      "LegPricedSchema": {
        "properties": {
          "amount": {
            "title": "amount",
            "type": "string",
            "format": "decimal",
            "description": "Amount in units of the base"
          },
          "direction": {
            "title": "direction",
            "type": "string",
            "enum": [
              "buy",
              "sell"
            ],
            "description": "Leg direction"
          },
          "instrument_name": {
            "title": "instrument_name",
            "type": "string",
            "description": "Instrument name"
          },
          "price": {
            "title": "price",
            "type": "string",
            "format": "decimal",
            "description": "Leg price"
          }
        },
        "required": [
          "amount",
          "direction",
          "instrument_name",
          "price"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PublicExecuteQuoteDebugParamsSchema": {
        "properties": {
          "client": {
            "title": "client",
            "type": "string",
            "default": "",
            "description": "Optional client that sent the execute quote"
          },
          "direction": {
            "title": "direction",
            "type": "string",
            "enum": [
              "buy",
              "sell"
            ],
            "description": "Quote direction, `buy` means trading each leg at its direction, `sell` means trading each leg in the opposite direction."
          },
          "enable_taker_protection": {
            "title": "enable_taker_protection",
            "type": "boolean",
            "default": false,
            "description": "Whether taker protection is enabled for the quote"
          },
          "label": {
            "title": "label",
            "type": "string",
            "default": "",
            "description": "Optional user-defined label for the quote"
          },
          "legs": {
            "title": "legs",
            "type": "array",
            "description": "Quote legs",
            "items": {
              "$ref": "#/components/schemas/LegPricedSchema"
            }
          },
          "max_fee": {
            "title": "max_fee",
            "type": "string",
            "format": "decimal",
            "description": "Max fee ($ for the full trade). Request will be rejected if the supplied max fee is below the estimated fee for this trade."
          },
          "nonce": {
            "title": "nonce",
            "type": "integer",
            "description": "Unique nonce defined as a concatenated `UTC timestamp in ms` and `random number up to 6 digits` (e.g. 1695836058725001, where 001 is the random number)"
          },
          "quote_id": {
            "title": "quote_id",
            "type": "string",
            "format": "uuid",
            "description": "Quote ID to execute against"
          },
          "rfq_id": {
            "title": "rfq_id",
            "type": "string",
            "format": "uuid",
            "description": "RFQ ID to execute (must be sent by `subaccount_id`)"
          },
          "signature": {
            "title": "signature",
            "type": "string",
            "description": "Ethereum signature of the quote"
          },
          "signature_expiry_sec": {
            "title": "signature_expiry_sec",
            "type": "integer",
            "description": "Unix timestamp in seconds. Expiry MUST be at least 310 seconds from now. Once time till signature expiry reaches 300 seconds, the quote will be considered expired. This buffer is meant to ensure the trade can settle on chain in case of a blockchain congestion."
          },
          "signer": {
            "title": "signer",
            "type": "string",
            "description": "Owner wallet address or registered session key that signed the quote"
          },
          "subaccount_id": {
            "title": "subaccount_id",
            "type": "integer",
            "description": "Subaccount ID"
          }
        },
        "required": [
          "direction",
          "legs",
          "max_fee",
          "nonce",
          "quote_id",
          "rfq_id",
          "signature",
          "signature_expiry_sec",
          "signer",
          "subaccount_id"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PublicExecuteQuoteDebugResponseSchema": {
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
            "$ref": "#/components/schemas/PublicExecuteQuoteDebugResultSchema"
          }
        },
        "required": [
          "id",
          "result"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PublicExecuteQuoteDebugResultSchema": {
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
          "encoded_legs": {
            "title": "encoded_legs",
            "type": "string",
            "description": "ABI encoded legs data"
          },
          "legs_hash": {
            "title": "legs_hash",
            "type": "string",
            "description": "Keccak hashed legs data"
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
          "encoded_legs",
          "legs_hash",
          "typed_data_hash"
        ],
        "type": "object",
        "additionalProperties": false
      }
    }
  }
}
```