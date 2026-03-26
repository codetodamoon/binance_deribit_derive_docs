# Get Trade History

Get trade history for a subaccount, with filter parameters.
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
    "/private/get_trade_history": {
      "post": {
        "tags": [
          "Private"
        ],
        "summary": "Get Trade History",
        "description": "Get trade history for a subaccount, with filter parameters.\nRequired minimum session key permission level is `read_only`",
        "responses": {
          "200": {
            "description": "successful operation",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/PrivateGetTradeHistoryResponseSchema"
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
                "$ref": "#/components/schemas/PrivateGetTradeHistoryParamsSchema"
              }
            }
          }
        }
      }
    }
  },
  "components": {
    "schemas": {
      "PaginationInfoSchema": {
        "properties": {
          "count": {
            "title": "count",
            "type": "integer",
            "description": "Total number of items, across all pages"
          },
          "num_pages": {
            "title": "num_pages",
            "type": "integer",
            "description": "Number of pages"
          }
        },
        "required": [
          "count",
          "num_pages"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "TradeResponseSchema": {
        "properties": {
          "direction": {
            "title": "direction",
            "type": "string",
            "enum": [
              "buy",
              "sell"
            ],
            "description": "Order direction"
          },
          "expected_rebate": {
            "title": "expected_rebate",
            "type": "string",
            "format": "decimal",
            "description": "Expected rebate for this trade"
          },
          "extra_fee": {
            "title": "extra_fee",
            "type": "string",
            "format": "decimal",
            "description": "Extra fee in USDC added by the reffering client (included in trade fee)"
          },
          "index_price": {
            "title": "index_price",
            "type": "string",
            "format": "decimal",
            "description": "Index price of the underlying at the time of the trade"
          },
          "instrument_name": {
            "title": "instrument_name",
            "type": "string",
            "description": "Instrument name"
          },
          "is_transfer": {
            "title": "is_transfer",
            "type": "boolean",
            "description": "Whether the trade was generated through `private/transfer_position`"
          },
          "label": {
            "title": "label",
            "type": "string",
            "description": "Optional user-defined label for the order"
          },
          "liquidity_role": {
            "title": "liquidity_role",
            "type": "string",
            "enum": [
              "maker",
              "taker"
            ],
            "description": "Role of the user in the trade"
          },
          "mark_price": {
            "title": "mark_price",
            "type": "string",
            "format": "decimal",
            "description": "Mark price of the instrument at the time of the trade"
          },
          "order_id": {
            "title": "order_id",
            "type": "string",
            "description": "Order ID"
          },
          "quote_id": {
            "title": "quote_id",
            "type": "string",
            "format": "uuid",
            "default": null,
            "description": "Quote ID if the trade was executed via RFQ",
            "nullable": true
          },
          "realized_pnl": {
            "title": "realized_pnl",
            "type": "string",
            "format": "decimal",
            "description": "Realized PnL for this trade"
          },
          "realized_pnl_excl_fees": {
            "title": "realized_pnl_excl_fees",
            "type": "string",
            "format": "decimal",
            "description": "Realized PnL for this trade using cost accounting that excludes fees"
          },
          "rfq_id": {
            "title": "rfq_id",
            "type": "string",
            "format": "uuid",
            "default": null,
            "description": "RFQ ID if the trade was executed via RFQ",
            "nullable": true
          },
          "subaccount_id": {
            "title": "subaccount_id",
            "type": "integer",
            "description": "Subaccount ID"
          },
          "timestamp": {
            "title": "timestamp",
            "type": "integer",
            "description": "Trade timestamp (in ms since Unix epoch)"
          },
          "trade_amount": {
            "title": "trade_amount",
            "type": "string",
            "format": "decimal",
            "description": "Amount filled in this trade"
          },
          "trade_fee": {
            "title": "trade_fee",
            "type": "string",
            "format": "decimal",
            "description": "Base_fee (only takers) + unit_fee (adjusted via rebates / discounts) + extra_fee (set by referrring client))"
          },
          "trade_id": {
            "title": "trade_id",
            "type": "string",
            "description": "Trade ID"
          },
          "trade_price": {
            "title": "trade_price",
            "type": "string",
            "format": "decimal",
            "description": "Price at which the trade was filled"
          },
          "transaction_id": {
            "title": "transaction_id",
            "type": "string",
            "description": "The transaction id of the related settlement transaction"
          },
          "tx_hash": {
            "title": "tx_hash",
            "type": "string",
            "default": null,
            "description": "Blockchain transaction hash",
            "nullable": true
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
            "description": "Blockchain transaction status"
          }
        },
        "required": [
          "direction",
          "expected_rebate",
          "extra_fee",
          "index_price",
          "instrument_name",
          "is_transfer",
          "label",
          "liquidity_role",
          "mark_price",
          "order_id",
          "quote_id",
          "realized_pnl",
          "realized_pnl_excl_fees",
          "rfq_id",
          "subaccount_id",
          "timestamp",
          "trade_amount",
          "trade_fee",
          "trade_id",
          "trade_price",
          "transaction_id",
          "tx_hash",
          "tx_status"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PrivateGetTradeHistoryParamsSchema": {
        "properties": {
          "from_timestamp": {
            "title": "from_timestamp",
            "type": "integer",
            "default": 0,
            "description": "Earliest timestamp to filter by (in ms since Unix epoch). If not provied, defaults to 0."
          },
          "instrument_name": {
            "title": "instrument_name",
            "type": "string",
            "default": null,
            "description": "Instrument name to filter by",
            "nullable": true
          },
          "order_id": {
            "title": "order_id",
            "type": "string",
            "default": null,
            "description": "Order id to filter by",
            "nullable": true
          },
          "page": {
            "title": "page",
            "type": "integer",
            "default": 1,
            "description": "Page number of results to return (default 1, returns last if above `num_pages`)"
          },
          "page_size": {
            "title": "page_size",
            "type": "integer",
            "default": 100,
            "description": "Number of results per page (default 100, max 1000)"
          },
          "quote_id": {
            "title": "quote_id",
            "type": "string",
            "default": null,
            "description": "If supplied, quote id to filter by. Supports either a concrete UUID, or `is_quote` and `is_not_quote` enum",
            "nullable": true
          },
          "subaccount_id": {
            "title": "subaccount_id",
            "type": "integer",
            "default": null,
            "description": "Subaccount_id (must be set if wallet is blank)",
            "nullable": true
          },
          "to_timestamp": {
            "title": "to_timestamp",
            "type": "integer",
            "default": 18446744073709552000,
            "description": "Latest timestamp to filter by (in ms since Unix epoch). If not provied, defaults to returning all data up to current time."
          },
          "wallet": {
            "title": "wallet",
            "type": "string",
            "default": null,
            "description": "Wallet address (if set, subaccount_id ignored)",
            "nullable": true
          }
        },
        "type": "object",
        "additionalProperties": false
      },
      "PrivateGetTradeHistoryResponseSchema": {
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
            "$ref": "#/components/schemas/PrivateGetTradeHistoryResultSchema"
          }
        },
        "required": [
          "id",
          "result"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PrivateGetTradeHistoryResultSchema": {
        "properties": {
          "pagination": {
            "$ref": "#/components/schemas/PaginationInfoSchema"
          },
          "subaccount_id": {
            "title": "subaccount_id",
            "type": "integer",
            "description": "Subaccount ID requested, or 0 if not provided"
          },
          "trades": {
            "title": "trades",
            "type": "array",
            "description": "List of trades",
            "items": {
              "$ref": "#/components/schemas/TradeResponseSchema"
            }
          }
        },
        "required": [
          "pagination",
          "subaccount_id",
          "trades"
        ],
        "type": "object",
        "additionalProperties": false
      }
    }
  }
}
```