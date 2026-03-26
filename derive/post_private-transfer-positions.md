# Transfer Positions

Transfers multiple positions from one subaccount to another, owned by the same wallet.<br /><br />The transfer is executed as a an RFQ. A mock RFQ is first created from the taker parameters, followed by a maker quote and a taker execute.<br />The leg amounts, prices and instrument name must be the same in both param payloads.<br />Fee is not charged and a zero `max_fee` must be signed.<br />Every leg in the transfer must be a position reduction for either maker or taker (or both).<br /><br />History: for position transfer history, use the `private/get_trade_history` RPC (not `private/get_erc20_transfer_history`).
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
    "/private/transfer_positions": {
      "post": {
        "tags": [
          "Private"
        ],
        "summary": "Transfer Positions",
        "description": "Transfers multiple positions from one subaccount to another, owned by the same wallet.<br /><br />The transfer is executed as a an RFQ. A mock RFQ is first created from the taker parameters, followed by a maker quote and a taker execute.<br />The leg amounts, prices and instrument name must be the same in both param payloads.<br />Fee is not charged and a zero `max_fee` must be signed.<br />Every leg in the transfer must be a position reduction for either maker or taker (or both).<br /><br />History: for position transfer history, use the `private/get_trade_history` RPC (not `private/get_erc20_transfer_history`).\nRequired minimum session key permission level is `admin`",
        "responses": {
          "200": {
            "description": "successful operation",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/PrivateTransferPositionsResponseSchema"
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
                "$ref": "#/components/schemas/PrivateTransferPositionsParamsSchema"
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
      "QuoteResultSchema": {
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
      },
      "PrivateTransferPositionsParamsSchema": {
        "properties": {
          "maker_params": {
            "$ref": "#/components/schemas/SignedQuoteParamsSchema"
          },
          "taker_params": {
            "$ref": "#/components/schemas/SignedQuoteParamsSchema"
          },
          "wallet": {
            "title": "wallet",
            "type": "string",
            "description": "Public key (wallet) of the account"
          }
        },
        "required": [
          "maker_params",
          "taker_params",
          "wallet"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "SignedQuoteParamsSchema": {
        "properties": {
          "direction": {
            "title": "direction",
            "type": "string",
            "enum": [
              "buy",
              "sell"
            ],
            "description": "Quote direction, `buy` means trading each leg at its direction, `sell` means trading each leg in the opposite direction."
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
          "signature",
          "signature_expiry_sec",
          "signer",
          "subaccount_id"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PrivateTransferPositionsResponseSchema": {
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
            "$ref": "#/components/schemas/PrivateTransferPositionsResultSchema"
          }
        },
        "required": [
          "id",
          "result"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PrivateTransferPositionsResultSchema": {
        "properties": {
          "maker_quote": {
            "$ref": "#/components/schemas/QuoteResultSchema"
          },
          "taker_quote": {
            "$ref": "#/components/schemas/QuoteResultSchema"
          }
        },
        "required": [
          "maker_quote",
          "taker_quote"
        ],
        "type": "object",
        "additionalProperties": false
      }
    }
  }
}
```