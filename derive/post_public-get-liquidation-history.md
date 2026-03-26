# Get Liquidation History

Returns a paginated liquidation history for all subaccounts. Note that the pagination is based on the number of<br />raw events that include bids, auction start, and auction end events. This means that the count returned in the<br />pagination info will be larger than the total number of auction events. This also means the number of returned<br />auctions per page will be smaller than the supplied `page_size`.

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
    "/public/get_liquidation_history": {
      "post": {
        "tags": [
          "Public"
        ],
        "summary": "Get Liquidation History",
        "description": "Returns a paginated liquidation history for all subaccounts. Note that the pagination is based on the number of<br />raw events that include bids, auction start, and auction end events. This means that the count returned in the<br />pagination info will be larger than the total number of auction events. This also means the number of returned<br />auctions per page will be smaller than the supplied `page_size`.",
        "responses": {
          "200": {
            "description": "successful operation",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/PublicGetLiquidationHistoryResponseSchema"
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
                "$ref": "#/components/schemas/PublicGetLiquidationHistoryParamsSchema"
              }
            }
          }
        }
      }
    }
  },
  "components": {
    "schemas": {
      "AuctionHistoryResultSchema": {
        "properties": {
          "auction_id": {
            "title": "auction_id",
            "type": "string",
            "description": "Unique ID of the auction"
          },
          "auction_type": {
            "title": "auction_type",
            "type": "string",
            "enum": [
              "solvent",
              "insolvent"
            ],
            "description": "Type of auction"
          },
          "bids": {
            "title": "bids",
            "type": "array",
            "description": "List of auction bid events",
            "items": {
              "$ref": "#/components/schemas/AuctionBidEventSchema"
            }
          },
          "end_timestamp": {
            "title": "end_timestamp",
            "type": "integer",
            "default": null,
            "description": "Timestamp of the auction end (in ms since UNIX epoch), or `null` if not ended",
            "nullable": true
          },
          "fee": {
            "title": "fee",
            "type": "string",
            "format": "decimal",
            "description": "Fee paid by the subaccount"
          },
          "start_timestamp": {
            "title": "start_timestamp",
            "type": "integer",
            "description": "Timestamp of the auction start (in ms since UNIX epoch)"
          },
          "subaccount_id": {
            "title": "subaccount_id",
            "type": "integer",
            "description": "Liquidated subaccount ID"
          },
          "tx_hash": {
            "title": "tx_hash",
            "type": "string",
            "description": "Hash of the transaction that started the auction"
          }
        },
        "required": [
          "auction_id",
          "auction_type",
          "bids",
          "end_timestamp",
          "fee",
          "start_timestamp",
          "subaccount_id",
          "tx_hash"
        ],
        "type": "object",
        "additionalProperties": false
      },
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
      "PublicGetLiquidationHistoryParamsSchema": {
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
            "default": null,
            "description": "(Optional) Subaccount ID",
            "nullable": true
          }
        },
        "type": "object",
        "additionalProperties": false
      },
      "PublicGetLiquidationHistoryResponseSchema": {
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
            "$ref": "#/components/schemas/PublicGetLiquidationHistoryResultSchema"
          }
        },
        "required": [
          "id",
          "result"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PublicGetLiquidationHistoryResultSchema": {
        "properties": {
          "auctions": {
            "title": "auctions",
            "type": "array",
            "description": "List of auction results",
            "items": {
              "$ref": "#/components/schemas/AuctionHistoryResultSchema"
            }
          },
          "pagination": {
            "$ref": "#/components/schemas/PaginationInfoSchema"
          }
        },
        "required": [
          "auctions",
          "pagination"
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
      }
    }
  }
}
```