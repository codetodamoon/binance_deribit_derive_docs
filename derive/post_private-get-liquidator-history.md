# Get Liquidator History

Returns a paginated history of auctions that the subaccount has participated in as a liquidator.
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
    "/private/get_liquidator_history": {
      "post": {
        "tags": [
          "Private"
        ],
        "summary": "Get Liquidator History",
        "description": "Returns a paginated history of auctions that the subaccount has participated in as a liquidator.\nRequired minimum session key permission level is `read_only`",
        "responses": {
          "200": {
            "description": "successful operation",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/PrivateGetLiquidatorHistoryResponseSchema"
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
                "$ref": "#/components/schemas/PrivateGetLiquidatorHistoryParamsSchema"
              }
            }
          }
        }
      }
    }
  },
  "components": {
    "schemas": {
      "AuctionBidEventSchema": {
        "properties": {
          "amounts_liquidated": {
            "title": "amounts_liquidated",
            "type": "object",
            "description": "Amounts of each asset that were closed",
            "additionalProperties": {
              "title": "amounts_liquidated",
              "type": "string",
              "format": "decimal"
            }
          },
          "cash_received": {
            "title": "cash_received",
            "type": "string",
            "format": "decimal",
            "description": "Cash received by the subaccount for the liquidation. For the liquidated accounts this is the amount the liquidator paid to buy out the percentage of the portfolio. For the liquidator account, this is the amount they received from the security module (if positive) or the amount they paid for the bid (if negative)"
          },
          "discount_pnl": {
            "title": "discount_pnl",
            "type": "string",
            "format": "decimal",
            "description": "Realized PnL due to liquidating or being liquidated at a discount to mark portfolio value"
          },
          "percent_liquidated": {
            "title": "percent_liquidated",
            "type": "string",
            "format": "decimal",
            "description": "Percent of the subaccount that was liquidated"
          },
          "positions_realized_pnl": {
            "title": "positions_realized_pnl",
            "type": "object",
            "description": "Realized PnL of each position that was closed",
            "additionalProperties": {
              "title": "positions_realized_pnl",
              "type": "string",
              "format": "decimal"
            }
          },
          "positions_realized_pnl_excl_fees": {
            "title": "positions_realized_pnl_excl_fees",
            "type": "object",
            "description": "Realized PnL of each position that was closed, excluding fees from total cost basis",
            "additionalProperties": {
              "title": "positions_realized_pnl_excl_fees",
              "type": "string",
              "format": "decimal"
            }
          },
          "realized_pnl": {
            "title": "realized_pnl",
            "type": "string",
            "format": "decimal",
            "description": "Realized PnL of the auction bid, assuming positions are closed at mark price at the time of the liquidation"
          },
          "realized_pnl_excl_fees": {
            "title": "realized_pnl_excl_fees",
            "type": "string",
            "format": "decimal",
            "description": "Realized PnL of the auction bid, excluding fees from total cost basis, assuming positions are closed at mark price at the time of the liquidation"
          },
          "timestamp": {
            "title": "timestamp",
            "type": "integer",
            "description": "Timestamp of the bid (in ms since UNIX epoch)"
          },
          "tx_hash": {
            "title": "tx_hash",
            "type": "string",
            "description": "Hash of the bid transaction"
          }
        },
        "required": [
          "amounts_liquidated",
          "cash_received",
          "discount_pnl",
          "percent_liquidated",
          "positions_realized_pnl",
          "positions_realized_pnl_excl_fees",
          "realized_pnl",
          "realized_pnl_excl_fees",
          "timestamp",
          "tx_hash"
        ],
        "type": "object",
        "additionalProperties": false
      },
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
      "PrivateGetLiquidatorHistoryParamsSchema": {
        "properties": {
          "end_timestamp": {
            "title": "end_timestamp",
            "type": "integer",
            "default": 9223372036854776000,
            "description": "End timestamp of the event history (default current time)"
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
          "start_timestamp": {
            "title": "start_timestamp",
            "type": "integer",
            "default": 0,
            "description": "Start timestamp of the event history (default 0)"
          },
          "subaccount_id": {
            "title": "subaccount_id",
            "type": "integer",
            "description": "Subaccount ID"
          }
        },
        "required": [
          "subaccount_id"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PrivateGetLiquidatorHistoryResponseSchema": {
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
            "$ref": "#/components/schemas/PrivateGetLiquidatorHistoryResultSchema"
          }
        },
        "required": [
          "id",
          "result"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PrivateGetLiquidatorHistoryResultSchema": {
        "properties": {
          "bids": {
            "title": "bids",
            "type": "array",
            "description": "List of auction bid events",
            "items": {
              "$ref": "#/components/schemas/AuctionBidEventSchema"
            }
          },
          "pagination": {
            "$ref": "#/components/schemas/PaginationInfoSchema"
          }
        },
        "required": [
          "bids",
          "pagination"
        ],
        "type": "object",
        "additionalProperties": false
      }
    }
  }
}
```