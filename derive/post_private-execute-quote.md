# Execute Quote

Executes a quote.
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
    "/private/execute_quote": {
      "post": {
        "tags": [
          "Private"
        ],
        "summary": "Execute Quote",
        "description": "Executes a quote.\nRequired minimum session key permission level is `admin`",
        "responses": {
          "200": {
            "description": "successful operation",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/PrivateExecuteQuoteResponseSchema"
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
                "$ref": "#/components/schemas/PrivateExecuteQuoteParamsSchema"
              }
            }
          }
        }
      }
    }
  },
  "components": {
    "schemas": {
      "PrivateExecuteQuoteParamsSchema": {
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
      "PrivateExecuteQuoteResponseSchema": {
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
            "$ref": "#/components/schemas/PrivateExecuteQuoteResultSchema"
          }
        },
        "required": [
          "id",
          "result"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PrivateExecuteQuoteResultSchema": {
        "properties": {
          "cancel_reason": {
            "title": "cancel_reason",
            "type": "string",
            "enum": [
              "",
              "user_request",
              "insufficient_margin",
              "signed_max_fee_too_low",
              "mmp_trigger",
              "cancel_on_disconnect",
              "session_key_deregistered",
              "subaccount_withdrawn",
              "rfq_no_longer_open",
              "compliance"
            ],
            "description": "Cancel reason, if any"
          },
          "creation_timestamp": {
            "title": "creation_timestamp",
            "type": "integer",
            "description": "Creation timestamp in ms since Unix epoch"
          },
          "direction": {
            "title": "direction",
            "type": "string",
            "enum": [
              "buy",
              "sell"
            ],
            "description": "Quote direction"
          },
          "fee": {
            "title": "fee",
            "type": "string",
            "format": "decimal",
            "description": "Fee paid for this quote (if executed)"
          },
          "fill_pct": {
            "title": "fill_pct",
            "type": "string",
            "format": "decimal",
            "description": "Percentage of the RFQ that this quote would fill, from 0 to 1."
          },
          "is_transfer": {
            "title": "is_transfer",
            "type": "boolean",
            "description": "Whether the order was generated through `private/transfer_position`"
          },
          "label": {
            "title": "label",
            "type": "string",
            "description": "User-defined label, if any"
          },
          "last_update_timestamp": {
            "title": "last_update_timestamp",
            "type": "integer",
            "description": "Last update timestamp in ms since Unix epoch"
          },
          "legs": {
            "title": "legs",
            "type": "array",
            "description": "Quote legs",
            "items": {
              "$ref": "#/components/schemas/LegPricedSchema"
            }
          },
          "legs_hash": {
            "title": "legs_hash",
            "type": "string",
            "description": "Hash of the legs of the best quote to be signed by the taker."
          },
          "liquidity_role": {
            "title": "liquidity_role",
            "type": "string",
            "enum": [
              "maker",
              "taker"
            ],
            "description": "Liquidity role"
          },
          "max_fee": {
            "title": "max_fee",
            "type": "string",
            "format": "decimal",
            "description": "Signed max fee"
          },
          "mmp": {
            "title": "mmp",
            "type": "boolean",
            "description": "Whether the quote is tagged for market maker protections (default false)"
          },
          "nonce": {
            "title": "nonce",
            "type": "integer",
            "description": "Nonce"
          },
          "quote_id": {
            "title": "quote_id",
            "type": "string",
            "format": "uuid",
            "description": "Quote ID"
          },
          "rfq_filled_pct": {
            "title": "rfq_filled_pct",
            "type": "string",
            "format": "decimal",
            "description": "Total percentage of the RFQ that has already been filled after this execution, from 0 to 1."
          },
          "rfq_id": {
            "title": "rfq_id",
            "type": "string",
            "format": "uuid",
            "description": "RFQ ID"
          },
          "signature": {
            "title": "signature",
            "type": "string",
            "description": "Ethereum signature of the quote"
          },
          "signature_expiry_sec": {
            "title": "signature_expiry_sec",
            "type": "integer",
            "description": "Unix timestamp in seconds"
          },
          "signer": {
            "title": "signer",
            "type": "string",
            "description": "Owner wallet address or registered session key that signed the quote"
          },
          "status": {
            "title": "status",
            "type": "string",
            "enum": [
              "open",
              "filled",
              "cancelled",
              "expired"
            ],
            "description": "Status"
          },
          "subaccount_id": {
            "title": "subaccount_id",
            "type": "integer",
            "description": "Subaccount ID"
          },
          "tx_hash": {
            "title": "tx_hash",
            "type": "string",
            "default": null,
            "description": "Blockchain transaction hash (only for executed quotes)",
            "nullable": true
          },
          "tx_status": {
            "title": "tx_status",
            "type": "string",
            "default": null,
            "enum": [
              "requested",
              "pending",
              "settled",
              "reverted",
              "ignored",
              "timed_out"
            ],
            "description": "Blockchain transaction status (only for executed quotes)",
            "nullable": true
          }
        },
        "required": [
          "cancel_reason",
          "creation_timestamp",
          "direction",
          "fee",
          "fill_pct",
          "is_transfer",
          "label",
          "last_update_timestamp",
          "legs",
          "legs_hash",
          "liquidity_role",
          "max_fee",
          "mmp",
          "nonce",
          "quote_id",
          "rfq_filled_pct",
          "rfq_id",
          "signature",
          "signature_expiry_sec",
          "signer",
          "status",
          "subaccount_id",
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