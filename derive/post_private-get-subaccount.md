# Get Subaccount

Get open orders, active positions, and collaterals of a subaccount
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
    "/private/get_subaccount": {
      "post": {
        "tags": [
          "Private"
        ],
        "summary": "Get Subaccount",
        "description": "Get open orders, active positions, and collaterals of a subaccount\nRequired minimum session key permission level is `read_only`",
        "responses": {
          "200": {
            "description": "successful operation",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/PrivateGetSubaccountResponseSchema"
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
                "$ref": "#/components/schemas/PrivateGetSubaccountParamsSchema"
              }
            }
          }
        }
      }
    }
  },
  "components": {
    "schemas": {
      "CollateralResponseSchema": {
        "properties": {
          "amount": {
            "title": "amount",
            "type": "string",
            "format": "decimal",
            "description": "Asset amount of given collateral"
          },
          "amount_step": {
            "title": "amount_step",
            "type": "string",
            "format": "decimal",
            "description": "Minimum amount step for the collateral"
          },
          "asset_name": {
            "title": "asset_name",
            "type": "string",
            "description": "Asset name"
          },
          "asset_type": {
            "title": "asset_type",
            "type": "string",
            "enum": [
              "erc20",
              "option",
              "perp"
            ],
            "description": "Type of asset collateral (currently always `erc20`)"
          },
          "average_price": {
            "title": "average_price",
            "type": "string",
            "format": "decimal",
            "description": "Average price of the collateral, 0 for USDC."
          },
          "average_price_excl_fees": {
            "title": "average_price_excl_fees",
            "type": "string",
            "format": "decimal",
            "description": "Average price of whole position excluding fees"
          },
          "creation_timestamp": {
            "title": "creation_timestamp",
            "type": "integer",
            "description": "Timestamp of when the position was opened (in ms since Unix epoch)"
          },
          "cumulative_interest": {
            "title": "cumulative_interest",
            "type": "string",
            "format": "decimal",
            "description": "Cumulative interest earned on supplying collateral or paid for borrowing"
          },
          "currency": {
            "title": "currency",
            "type": "string",
            "description": "Underlying currency of asset (`ETH`, `BTC`, etc)"
          },
          "delta": {
            "title": "delta",
            "type": "string",
            "format": "decimal",
            "description": "Asset delta w.r.t. the delta currency"
          },
          "delta_currency": {
            "title": "delta_currency",
            "type": "string",
            "description": "Currency with respect to which delta is reported.For example, LRTs like WEETH have their delta reported in ETH"
          },
          "initial_margin": {
            "title": "initial_margin",
            "type": "string",
            "format": "decimal",
            "description": "USD value of collateral that contributes to initial margin"
          },
          "maintenance_margin": {
            "title": "maintenance_margin",
            "type": "string",
            "format": "decimal",
            "description": "USD value of collateral that contributes to maintenance margin"
          },
          "mark_price": {
            "title": "mark_price",
            "type": "string",
            "format": "decimal",
            "description": "Current mark price of the asset"
          },
          "mark_value": {
            "title": "mark_value",
            "type": "string",
            "format": "decimal",
            "description": "USD value of the collateral (amount * mark price)"
          },
          "open_orders_margin": {
            "title": "open_orders_margin",
            "type": "string",
            "format": "decimal",
            "description": "USD margin requirement for all open orders for this asset / instrument"
          },
          "pending_interest": {
            "title": "pending_interest",
            "type": "string",
            "format": "decimal",
            "description": "Portion of interest that has not yet been settled on-chain. This number is added to the portfolio value for margin calculations purposes."
          },
          "realized_pnl": {
            "title": "realized_pnl",
            "type": "string",
            "format": "decimal",
            "description": "Realized trading profit or loss of the collateral, 0 for USDC."
          },
          "realized_pnl_excl_fees": {
            "title": "realized_pnl_excl_fees",
            "type": "string",
            "format": "decimal",
            "description": "Realized trading profit or loss of the position excluding fees"
          },
          "total_fees": {
            "title": "total_fees",
            "type": "string",
            "format": "decimal",
            "description": "Total fees paid for opening and changing the position"
          },
          "unrealized_pnl": {
            "title": "unrealized_pnl",
            "type": "string",
            "format": "decimal",
            "description": "Unrealized trading profit or loss of the collateral, 0 for USDC."
          },
          "unrealized_pnl_excl_fees": {
            "title": "unrealized_pnl_excl_fees",
            "type": "string",
            "format": "decimal",
            "description": "Unrealized trading profit or loss of the position excluding fees"
          }
        },
        "required": [
          "amount",
          "amount_step",
          "asset_name",
          "asset_type",
          "average_price",
          "average_price_excl_fees",
          "creation_timestamp",
          "cumulative_interest",
          "currency",
          "delta",
          "delta_currency",
          "initial_margin",
          "maintenance_margin",
          "mark_price",
          "mark_value",
          "open_orders_margin",
          "pending_interest",
          "realized_pnl",
          "realized_pnl_excl_fees",
          "total_fees",
          "unrealized_pnl",
          "unrealized_pnl_excl_fees"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PrivateGetSubaccountResultSchema": {
        "properties": {
          "collaterals": {
            "title": "collaterals",
            "type": "array",
            "description": "All collaterals that count towards margin of subaccount",
            "items": {
              "$ref": "#/components/schemas/CollateralResponseSchema"
            }
          },
          "collaterals_initial_margin": {
            "title": "collaterals_initial_margin",
            "type": "string",
            "format": "decimal",
            "description": "Total initial margin credit contributed by collaterals"
          },
          "collaterals_maintenance_margin": {
            "title": "collaterals_maintenance_margin",
            "type": "string",
            "format": "decimal",
            "description": "Total maintenance margin credit contributed by collaterals"
          },
          "collaterals_value": {
            "title": "collaterals_value",
            "type": "string",
            "format": "decimal",
            "description": "Total mark-to-market value of all collaterals"
          },
          "currency": {
            "title": "currency",
            "type": "string",
            "description": "Currency of subaccount"
          },
          "initial_margin": {
            "title": "initial_margin",
            "type": "string",
            "format": "decimal",
            "description": "Total initial margin requirement of all positions and collaterals.Trades will be rejected if this value falls below zero after the trade."
          },
          "is_under_liquidation": {
            "title": "is_under_liquidation",
            "type": "boolean",
            "description": "Whether the subaccount is undergoing a liquidation auction"
          },
          "label": {
            "title": "label",
            "type": "string",
            "description": "User defined label"
          },
          "maintenance_margin": {
            "title": "maintenance_margin",
            "type": "string",
            "format": "decimal",
            "description": "Total maintenance margin requirement of all positions and collaterals.If this value falls below zero, the subaccount will be flagged for liquidation."
          },
          "margin_type": {
            "title": "margin_type",
            "type": "string",
            "enum": [
              "PM",
              "SM",
              "PM2"
            ],
            "description": "Margin type of subaccount (`PM` (Portfolio Margin), `PM2` (Portfolio Margin 2), or `SM` (Standard Margin))"
          },
          "open_orders": {
            "title": "open_orders",
            "type": "array",
            "description": "All open orders of subaccount",
            "items": {
              "$ref": "#/components/schemas/OrderResponseSchema"
            }
          },
          "open_orders_margin": {
            "title": "open_orders_margin",
            "type": "string",
            "format": "decimal",
            "description": "Total margin requirement of all open orders.Orders will be rejected if this value plus initial margin are below zero after the order."
          },
          "positions": {
            "title": "positions",
            "type": "array",
            "description": "All active positions of subaccount",
            "items": {
              "$ref": "#/components/schemas/PositionResponseSchema"
            }
          },
          "positions_initial_margin": {
            "title": "positions_initial_margin",
            "type": "string",
            "format": "decimal",
            "description": "Total initial margin requirement of all positions"
          },
          "positions_maintenance_margin": {
            "title": "positions_maintenance_margin",
            "type": "string",
            "format": "decimal",
            "description": "Total maintenance margin requirement of all positions"
          },
          "positions_value": {
            "title": "positions_value",
            "type": "string",
            "format": "decimal",
            "description": "Total mark-to-market value of all positions"
          },
          "projected_margin_change": {
            "title": "projected_margin_change",
            "type": "string",
            "format": "decimal",
            "description": "Projected change in maintenance margin requirement between now and projected margin at 8:01 UTC. If this value plus current maintenance margin ise below zero, the account is at risk of being flagged for liquidation right after the upcoming expiry."
          },
          "subaccount_id": {
            "title": "subaccount_id",
            "type": "integer",
            "description": "Subaccount_id"
          },
          "subaccount_value": {
            "title": "subaccount_value",
            "type": "string",
            "format": "decimal",
            "description": "Total mark-to-market value of all positions and collaterals"
          }
        },
        "required": [
          "collaterals",
          "collaterals_initial_margin",
          "collaterals_maintenance_margin",
          "collaterals_value",
          "currency",
          "initial_margin",
          "is_under_liquidation",
          "label",
          "maintenance_margin",
          "margin_type",
          "open_orders",
          "open_orders_margin",
          "positions",
          "positions_initial_margin",
          "positions_maintenance_margin",
          "positions_value",
          "projected_margin_change",
          "subaccount_id",
          "subaccount_value"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "OrderResponseSchema": {
        "properties": {
          "amount": {
            "title": "amount",
            "type": "string",
            "format": "decimal",
            "description": "Order amount in units of the base"
          },
          "average_price": {
            "title": "average_price",
            "type": "string",
            "format": "decimal",
            "description": "Average fill price"
          },
          "cancel_reason": {
            "title": "cancel_reason",
            "type": "string",
            "enum": [
              "",
              "user_request",
              "mmp_trigger",
              "insufficient_margin",
              "signed_max_fee_too_low",
              "cancel_on_disconnect",
              "ioc_or_market_partial_fill",
              "session_key_deregistered",
              "subaccount_withdrawn",
              "compliance",
              "trigger_failed",
              "validation_failed"
            ],
            "description": "If cancelled, reason behind order cancellation"
          },
          "creation_timestamp": {
            "title": "creation_timestamp",
            "type": "integer",
            "description": "Creation timestamp (in ms since Unix epoch)"
          },
          "direction": {
            "title": "direction",
            "type": "string",
            "enum": [
              "buy",
              "sell"
            ],
            "description": "Order direction"
          },
          "extra_fee": {
            "title": "extra_fee",
            "type": "string",
            "format": "decimal",
            "default": "0",
            "description": "(Optional) Extra fee in USDC added to the total final fee paid by user (must be between 0.000001 and 1,000 USDC).",
            "nullable": true
          },
          "filled_amount": {
            "title": "filled_amount",
            "type": "string",
            "format": "decimal",
            "description": "Total filled amount for the order"
          },
          "instrument_name": {
            "title": "instrument_name",
            "type": "string",
            "description": "Instrument name"
          },
          "is_transfer": {
            "title": "is_transfer",
            "type": "boolean",
            "description": "Whether the order was generated through `private/transfer_position`"
          },
          "label": {
            "title": "label",
            "type": "string",
            "description": "Optional user-defined label for the order"
          },
          "last_update_timestamp": {
            "title": "last_update_timestamp",
            "type": "integer",
            "description": "Last update timestamp (in ms since Unix epoch)"
          },
          "limit_price": {
            "title": "limit_price",
            "type": "string",
            "format": "decimal",
            "description": "Limit price in quote currency"
          },
          "max_fee": {
            "title": "max_fee",
            "type": "string",
            "format": "decimal",
            "description": "Max fee in units of the quote currency"
          },
          "mmp": {
            "title": "mmp",
            "type": "boolean",
            "description": "Whether the order is tagged for market maker protections"
          },
          "nonce": {
            "title": "nonce",
            "type": "integer",
            "description": "Unique nonce defined as (UTC_timestamp in ms)(random_number_up_to_3_digits) (e.g. 1695836058725001, where 001 is the random number)"
          },
          "order_fee": {
            "title": "order_fee",
            "type": "string",
            "format": "decimal",
            "description": "Order fee paid so far"
          },
          "order_id": {
            "title": "order_id",
            "type": "string",
            "description": "Order ID"
          },
          "order_status": {
            "title": "order_status",
            "type": "string",
            "enum": [
              "open",
              "filled",
              "cancelled",
              "expired",
              "untriggered"
            ],
            "description": "Order status"
          },
          "order_type": {
            "title": "order_type",
            "type": "string",
            "enum": [
              "limit",
              "market"
            ],
            "description": "Order type"
          },
          "quote_id": {
            "title": "quote_id",
            "type": "string",
            "format": "uuid",
            "default": null,
            "description": "Quote ID if the trade was executed via RFQ",
            "nullable": true
          },
          "replaced_order_id": {
            "title": "replaced_order_id",
            "type": "string",
            "format": "uuid",
            "default": null,
            "description": "If replaced, ID of the order that was replaced",
            "nullable": true
          },
          "signature": {
            "title": "signature",
            "type": "string",
            "description": "Ethereum signature of the order"
          },
          "signature_expiry_sec": {
            "title": "signature_expiry_sec",
            "type": "integer",
            "description": "Signature expiry timestamp"
          },
          "signed_limit_price": {
            "title": "signed_limit_price",
            "type": "string",
            "format": "decimal",
            "default": null,
            "description": "The original limit price that the user signed. Only set when the order was adjusted (i.e., for post-only orders with reject_post_only=false that would have crossed). Used for on-chain submission.",
            "nullable": true
          },
          "signer": {
            "title": "signer",
            "type": "string",
            "description": "Owner wallet address or registered session key that signed order"
          },
          "subaccount_id": {
            "title": "subaccount_id",
            "type": "integer",
            "description": "Subaccount ID"
          },
          "time_in_force": {
            "title": "time_in_force",
            "type": "string",
            "enum": [
              "gtc",
              "post_only",
              "fok",
              "ioc"
            ],
            "description": "Time in force"
          },
          "trigger_price": {
            "title": "trigger_price",
            "type": "string",
            "format": "decimal",
            "default": null,
            "description": "(Required for trigger orders) Index or Market price to trigger order at",
            "nullable": true
          },
          "trigger_price_type": {
            "title": "trigger_price_type",
            "type": "string",
            "default": null,
            "enum": [
              "mark",
              "index"
            ],
            "description": "(Required for trigger orders) Trigger with Index or Mark Price",
            "nullable": true
          },
          "trigger_reject_message": {
            "title": "trigger_reject_message",
            "type": "string",
            "default": null,
            "description": "(Required for trigger orders) Error message if error occured during trigger",
            "nullable": true
          },
          "trigger_type": {
            "title": "trigger_type",
            "type": "string",
            "default": null,
            "enum": [
              "stoploss",
              "takeprofit"
            ],
            "description": "(Required for trigger orders) Stop-loss or Take-profit.",
            "nullable": true
          }
        },
        "required": [
          "amount",
          "average_price",
          "cancel_reason",
          "creation_timestamp",
          "direction",
          "filled_amount",
          "instrument_name",
          "is_transfer",
          "label",
          "last_update_timestamp",
          "limit_price",
          "max_fee",
          "mmp",
          "nonce",
          "order_fee",
          "order_id",
          "order_status",
          "order_type",
          "quote_id",
          "signature",
          "signature_expiry_sec",
          "signer",
          "subaccount_id",
          "time_in_force"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PositionResponseSchema": {
        "properties": {
          "amount": {
            "title": "amount",
            "type": "string",
            "format": "decimal",
            "description": "Position amount held by subaccount"
          },
          "amount_step": {
            "title": "amount_step",
            "type": "string",
            "format": "decimal",
            "description": "Minimum amount step for the position"
          },
          "average_price": {
            "title": "average_price",
            "type": "string",
            "format": "decimal",
            "description": "Average price of whole position"
          },
          "average_price_excl_fees": {
            "title": "average_price_excl_fees",
            "type": "string",
            "format": "decimal",
            "description": "Average price of whole position excluding fees"
          },
          "creation_timestamp": {
            "title": "creation_timestamp",
            "type": "integer",
            "description": "Timestamp of when the position was opened (in ms since Unix epoch)"
          },
          "cumulative_funding": {
            "title": "cumulative_funding",
            "type": "string",
            "format": "decimal",
            "description": "Cumulative funding for the position (only for perpetuals)."
          },
          "delta": {
            "title": "delta",
            "type": "string",
            "format": "decimal",
            "description": "Asset delta (w.r.t. forward price for options, `1.0` for perps)"
          },
          "gamma": {
            "title": "gamma",
            "type": "string",
            "format": "decimal",
            "description": "Asset gamma (zero for non-options)"
          },
          "index_price": {
            "title": "index_price",
            "type": "string",
            "format": "decimal",
            "description": "Current index (oracle) price for position's currency"
          },
          "initial_margin": {
            "title": "initial_margin",
            "type": "string",
            "format": "decimal",
            "description": "USD initial margin requirement for this position"
          },
          "instrument_name": {
            "title": "instrument_name",
            "type": "string",
            "description": "Instrument name (same as the base Asset name)"
          },
          "instrument_type": {
            "title": "instrument_type",
            "type": "string",
            "enum": [
              "erc20",
              "option",
              "perp"
            ],
            "description": "`erc20`, `option`, or `perp`"
          },
          "leverage": {
            "title": "leverage",
            "type": "string",
            "format": "decimal",
            "default": null,
            "description": "Only for perps. Leverage of the position, defined as `abs(notional) / collateral net of options margin`",
            "nullable": true
          },
          "liquidation_price": {
            "title": "liquidation_price",
            "type": "string",
            "format": "decimal",
            "default": null,
            "description": "Index price at which position will be liquidated",
            "nullable": true
          },
          "maintenance_margin": {
            "title": "maintenance_margin",
            "type": "string",
            "format": "decimal",
            "description": "USD maintenance margin requirement for this position"
          },
          "mark_price": {
            "title": "mark_price",
            "type": "string",
            "format": "decimal",
            "description": "Current mark price for position's instrument"
          },
          "mark_value": {
            "title": "mark_value",
            "type": "string",
            "format": "decimal",
            "description": "USD value of the position; this represents how much USD can be recieved by fully closing the position at the current oracle price"
          },
          "net_settlements": {
            "title": "net_settlements",
            "type": "string",
            "format": "decimal",
            "description": "Net amount of USD from position settlements that has been paid to the user's subaccount. This number is subtracted from the portfolio value for margin calculations purposes.<br />Positive values mean the user has recieved USD from settlements, or is awaiting settlement of USD losses. Negative values mean the user has paid USD for settlements, or is awaiting settlement of USD gains."
          },
          "open_orders_margin": {
            "title": "open_orders_margin",
            "type": "string",
            "format": "decimal",
            "description": "USD margin requirement for all open orders for this asset / instrument"
          },
          "pending_funding": {
            "title": "pending_funding",
            "type": "string",
            "format": "decimal",
            "description": "A portion of funding payments that has not yet been settled into cash balance (only for perpetuals). This number is added to the portfolio value for margin calculations purposes."
          },
          "realized_pnl": {
            "title": "realized_pnl",
            "type": "string",
            "format": "decimal",
            "description": "Realized trading profit or loss of the position."
          },
          "realized_pnl_excl_fees": {
            "title": "realized_pnl_excl_fees",
            "type": "string",
            "format": "decimal",
            "description": "Realized trading profit or loss of the position excluding fees"
          },
          "theta": {
            "title": "theta",
            "type": "string",
            "format": "decimal",
            "description": "Asset theta (zero for non-options)"
          },
          "total_fees": {
            "title": "total_fees",
            "type": "string",
            "format": "decimal",
            "description": "Total fees paid for opening and changing the position"
          },
          "unrealized_pnl": {
            "title": "unrealized_pnl",
            "type": "string",
            "format": "decimal",
            "description": "Unrealized trading profit or loss of the position."
          },
          "unrealized_pnl_excl_fees": {
            "title": "unrealized_pnl_excl_fees",
            "type": "string",
            "format": "decimal",
            "description": "Unrealized trading profit or loss of the position excluding fees"
          },
          "vega": {
            "title": "vega",
            "type": "string",
            "format": "decimal",
            "description": "Asset vega (zero for non-options)"
          }
        },
        "required": [
          "amount",
          "amount_step",
          "average_price",
          "average_price_excl_fees",
          "creation_timestamp",
          "cumulative_funding",
          "delta",
          "gamma",
          "index_price",
          "initial_margin",
          "instrument_name",
          "instrument_type",
          "leverage",
          "liquidation_price",
          "maintenance_margin",
          "mark_price",
          "mark_value",
          "net_settlements",
          "open_orders_margin",
          "pending_funding",
          "realized_pnl",
          "realized_pnl_excl_fees",
          "theta",
          "total_fees",
          "unrealized_pnl",
          "unrealized_pnl_excl_fees",
          "vega"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PrivateGetSubaccountParamsSchema": {
        "properties": {
          "subaccount_id": {
            "title": "subaccount_id",
            "type": "integer",
            "description": "Subaccount_id"
          }
        },
        "required": [
          "subaccount_id"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PrivateGetSubaccountResponseSchema": {
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
            "$ref": "#/components/schemas/PrivateGetSubaccountResultSchema"
          }
        },
        "required": [
          "id",
          "result"
        ],
        "type": "object",
        "additionalProperties": false
      }
    }
  }
}
```