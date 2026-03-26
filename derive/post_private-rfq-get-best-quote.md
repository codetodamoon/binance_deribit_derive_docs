# Rfq Get Best Quote

Performs a "dry run" on an RFQ, returning the estimated fee and whether the trade is expected to pass.<br /><br />Should any exception be raised in the process of evaluating the trade, a standard RPC error will be returned<br />with the error details.
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
    "/private/rfq_get_best_quote": {
      "post": {
        "tags": [
          "Private"
        ],
        "summary": "Rfq Get Best Quote",
        "description": "Performs a \"dry run\" on an RFQ, returning the estimated fee and whether the trade is expected to pass.<br /><br />Should any exception be raised in the process of evaluating the trade, a standard RPC error will be returned<br />with the error details.\nRequired minimum session key permission level is `read_only`",
        "responses": {
          "200": {
            "description": "successful operation",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/PrivateRfqGetBestQuoteResponseSchema"
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
                "$ref": "#/components/schemas/PrivateRfqGetBestQuoteParamsSchema"
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
      "LegUnpricedSchema": {
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
          }
        },
        "required": [
          "amount",
          "direction",
          "instrument_name"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PrivateRfqGetBestQuoteParamsSchema": {
        "properties": {
          "client": {
            "title": "client",
            "type": "string",
            "default": "",
            "description": "Optional client that sent RFQ"
          },
          "counterparties": {
            "title": "counterparties",
            "type": "array",
            "default": null,
            "description": "Optional list of market maker account addresses to request quotes from. If not supplied, all market makers who are approved as RFQ makers will be notified.",
            "items": {
              "title": "counterparties",
              "type": "string"
            },
            "nullable": true
          },
          "direction": {
            "title": "direction",
            "type": "string",
            "default": "buy",
            "enum": [
              "buy",
              "sell"
            ],
            "description": "Planned execution direction (default `buy`)"
          },
          "label": {
            "title": "label",
            "type": "string",
            "default": "",
            "description": "Optional user-defined label for the RFQ"
          },
          "legs": {
            "title": "legs",
            "type": "array",
            "description": "RFQ legs",
            "items": {
              "$ref": "#/components/schemas/LegUnpricedSchema"
            }
          },
          "max_total_cost": {
            "title": "max_total_cost",
            "type": "string",
            "format": "decimal",
            "default": null,
            "description": "An optional max total cost for the RFQ. Only used when the RFQ sender executes as buyer. Polling endpoints and channels will ignore quotes where the total cost across all legs is above this value. Positive values mean the RFQ sender expects to pay $, negative mean the RFQ sender expects to receive $.This field is not disclosed to the market makers.",
            "nullable": true
          },
          "min_total_cost": {
            "title": "min_total_cost",
            "type": "string",
            "format": "decimal",
            "default": null,
            "description": "An optional min total cost for the RFQ. Only used when the RFQ sender executes as seller. Polling endpoints and channels will ignore quotes where the total cost across all legs is below this value. Positive values mean the RFQ sender expects to receive $, negative mean the RFQ sender expects to pay $.This field is not disclosed to the market makers.",
            "nullable": true
          },
          "partial_fill_step": {
            "title": "partial_fill_step",
            "type": "string",
            "format": "decimal",
            "default": "1",
            "description": "Optional step size for partial fills. If not supplied, the RFQ will not support partial fills."
          },
          "rfq_id": {
            "title": "rfq_id",
            "type": "string",
            "format": "uuid",
            "default": null,
            "description": "RFQ ID to get best quote for. If not provided, will return estimates based on mark prices",
            "nullable": true
          },
          "subaccount_id": {
            "title": "subaccount_id",
            "type": "integer",
            "description": "Subaccount ID"
          }
        },
        "required": [
          "legs",
          "subaccount_id"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PrivateRfqGetBestQuoteResponseSchema": {
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
            "$ref": "#/components/schemas/PrivateRfqGetBestQuoteResultSchema"
          }
        },
        "required": [
          "id",
          "result"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PrivateRfqGetBestQuoteResultSchema": {
        "properties": {
          "best_quote": {
            "$ref": "#/components/schemas/QuoteResultPublicSchema",
            "nullable": true
          },
          "direction": {
            "title": "direction",
            "type": "string",
            "enum": [
              "buy",
              "sell"
            ],
            "description": "RFQ direction."
          },
          "down_liquidation_price": {
            "title": "down_liquidation_price",
            "type": "string",
            "format": "decimal",
            "default": null,
            "description": "Liquidation price if the trade were to be filled and the market moves down.",
            "nullable": true
          },
          "estimated_fee": {
            "title": "estimated_fee",
            "type": "string",
            "format": "decimal",
            "description": "An estimate for how much the user will pay in fees ($ for the whole trade)."
          },
          "estimated_realized_pnl": {
            "title": "estimated_realized_pnl",
            "type": "string",
            "format": "decimal",
            "description": "An estimate for the realized PnL of the trade."
          },
          "estimated_realized_pnl_excl_fees": {
            "title": "estimated_realized_pnl_excl_fees",
            "type": "string",
            "format": "decimal",
            "description": "An estimate for the realized PnL of the trade. with cost basis calculated without considering fees."
          },
          "estimated_total_cost": {
            "title": "estimated_total_cost",
            "type": "string",
            "format": "decimal",
            "description": "An estimate for the total $ cost of the trade."
          },
          "filled_pct": {
            "title": "filled_pct",
            "type": "string",
            "format": "decimal",
            "description": "Percentage of the RFQ that has already been filled, from 0 to 1."
          },
          "invalid_reason": {
            "title": "invalid_reason",
            "type": "string",
            "default": null,
            "enum": [
              "Account is currently under maintenance margin requirements, trading is frozen.",
              "This order would cause account to fall under maintenance margin requirements.",
              "Insufficient buying power, only a single risk-reducing open order is allowed.",
              "Insufficient buying power, consider reducing order size.",
              "Insufficient buying power, consider reducing order size or canceling other orders.",
              "Consider canceling other limit orders or using IOC, FOK, or market orders. This order is risk-reducing, but if filled with other open orders, buying power might be insufficient.",
              "Insufficient buying power."
            ],
            "description": "Reason for the RFQ being invalid, if any.",
            "nullable": true
          },
          "is_valid": {
            "title": "is_valid",
            "type": "boolean",
            "description": "`True` if RFQ is expected to pass margin requirements."
          },
          "orderbook_total_cost": {
            "title": "orderbook_total_cost",
            "type": "string",
            "format": "decimal",
            "default": null,
            "description": "Total cost of  the RFQ if it were to be filled at current orderbook prices (same direction as the RFQ). If lower than `estimated_total_cost`, the user may want to use the orderbook instead of RFQs for this order. Will return null if any of the legs do not have orderbook data or enough liquidity for the full fill.",
            "nullable": true
          },
          "post_initial_margin": {
            "title": "post_initial_margin",
            "type": "string",
            "format": "decimal",
            "description": "User's hypothetical margin balance if the trade were to get executed."
          },
          "post_liquidation_price": {
            "title": "post_liquidation_price",
            "type": "string",
            "format": "decimal",
            "default": null,
            "description": "Liquidation price if the trade were to be filled. If both upside and downside liquidation prices exist, returns the closest one to the current index price.",
            "nullable": true
          },
          "pre_initial_margin": {
            "title": "pre_initial_margin",
            "type": "string",
            "format": "decimal",
            "description": "User's initial margin balance before the trade."
          },
          "suggested_max_fee": {
            "title": "suggested_max_fee",
            "type": "string",
            "format": "decimal",
            "description": "Recommended value for `max_fee` of the trade."
          },
          "up_liquidation_price": {
            "title": "up_liquidation_price",
            "type": "string",
            "format": "decimal",
            "default": null,
            "description": "Liquidation price if the trade were to be filled and the market moves up.",
            "nullable": true
          }
        },
        "required": [
          "best_quote",
          "direction",
          "down_liquidation_price",
          "estimated_fee",
          "estimated_realized_pnl",
          "estimated_realized_pnl_excl_fees",
          "estimated_total_cost",
          "filled_pct",
          "invalid_reason",
          "is_valid",
          "orderbook_total_cost",
          "post_initial_margin",
          "post_liquidation_price",
          "pre_initial_margin",
          "suggested_max_fee",
          "up_liquidation_price"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "QuoteResultPublicSchema": {
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
          "fill_pct": {
            "title": "fill_pct",
            "type": "string",
            "format": "decimal",
            "description": "Percentage of the RFQ that this quote would fill, from 0 to 1."
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
          },
          "wallet": {
            "title": "wallet",
            "type": "string",
            "description": "Wallet address of the quote sender"
          }
        },
        "required": [
          "cancel_reason",
          "creation_timestamp",
          "direction",
          "fill_pct",
          "last_update_timestamp",
          "legs",
          "legs_hash",
          "liquidity_role",
          "quote_id",
          "rfq_id",
          "status",
          "subaccount_id",
          "tx_hash",
          "tx_status",
          "wallet"
        ],
        "type": "object",
        "additionalProperties": false
      }
    }
  }
}
```