# Get Ticker

Get ticker information (best bid / ask, instrument contraints, fees info, etc.) for a single instrument<br /><br />DEPRECATION NOTICE: This RPC is deprecated in favor of `get_tickers` on Dec 1, 2025.

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
    "/public/get_ticker": {
      "post": {
        "tags": [
          "Public"
        ],
        "summary": "Get Ticker",
        "description": "Get ticker information (best bid / ask, instrument contraints, fees info, etc.) for a single instrument<br /><br />DEPRECATION NOTICE: This RPC is deprecated in favor of `get_tickers` on Dec 1, 2025.",
        "responses": {
          "200": {
            "description": "successful operation",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/PublicGetTickerResponseSchema"
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
                "$ref": "#/components/schemas/PublicGetTickerParamsSchema"
              }
            }
          }
        }
      }
    }
  },
  "components": {
    "schemas": {
      "OpenInterestStatsSchema": {
        "properties": {
          "current_open_interest": {
            "title": "current_open_interest",
            "type": "string",
            "format": "decimal",
            "description": "Current open interest for the margin type"
          },
          "interest_cap": {
            "title": "interest_cap",
            "type": "string",
            "format": "decimal",
            "description": "Total open interest cap"
          },
          "manager_currency": {
            "title": "manager_currency",
            "type": "string",
            "default": null,
            "description": "Currency of the manager (only applies to Portfolio Margin)",
            "nullable": true
          }
        },
        "required": [
          "current_open_interest",
          "interest_cap"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "ERC20PublicDetailsSchema": {
        "properties": {
          "borrow_index": {
            "title": "borrow_index",
            "type": "string",
            "format": "decimal",
            "default": "1",
            "description": "Latest borrow index as per `CashAsset.sol` implementation"
          },
          "decimals": {
            "title": "decimals",
            "type": "integer",
            "description": "Number of decimals of the underlying on-chain ERC20 token"
          },
          "supply_index": {
            "title": "supply_index",
            "type": "string",
            "format": "decimal",
            "default": "1",
            "description": "Latest supply index as per `CashAsset.sol` implementation"
          },
          "underlying_erc20_address": {
            "title": "underlying_erc20_address",
            "type": "string",
            "default": "",
            "description": "Address of underlying on-chain ERC20 (not V2 asset)"
          }
        },
        "required": [
          "decimals"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "OptionPublicDetailsSchema": {
        "properties": {
          "expiry": {
            "title": "expiry",
            "type": "integer",
            "description": "Unix timestamp of expiry date (in seconds)"
          },
          "index": {
            "title": "index",
            "type": "string",
            "description": "Underlying settlement price index"
          },
          "option_type": {
            "title": "option_type",
            "type": "string",
            "enum": [
              "C",
              "P"
            ]
          },
          "settlement_price": {
            "title": "settlement_price",
            "type": "string",
            "format": "decimal",
            "default": null,
            "description": "Settlement price of the option",
            "nullable": true
          },
          "strike": {
            "title": "strike",
            "type": "string",
            "format": "decimal"
          }
        },
        "required": [
          "expiry",
          "index",
          "option_type",
          "strike"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PerpPublicDetailsSchema": {
        "properties": {
          "aggregate_funding": {
            "title": "aggregate_funding",
            "type": "string",
            "format": "decimal",
            "description": "Latest aggregated funding as per `PerpAsset.sol`"
          },
          "funding_rate": {
            "title": "funding_rate",
            "type": "string",
            "format": "decimal",
            "description": "Current hourly funding rate as per `PerpAsset.sol`"
          },
          "index": {
            "title": "index",
            "type": "string",
            "description": "Underlying spot price index for funding rate"
          },
          "max_rate_per_hour": {
            "title": "max_rate_per_hour",
            "type": "string",
            "format": "decimal",
            "description": "Max rate per hour as per `PerpAsset.sol`"
          },
          "min_rate_per_hour": {
            "title": "min_rate_per_hour",
            "type": "string",
            "format": "decimal",
            "description": "Min rate per hour as per `PerpAsset.sol`"
          },
          "static_interest_rate": {
            "title": "static_interest_rate",
            "type": "string",
            "format": "decimal",
            "description": "Static interest rate as per `PerpAsset.sol`"
          }
        },
        "required": [
          "aggregate_funding",
          "funding_rate",
          "index",
          "max_rate_per_hour",
          "min_rate_per_hour",
          "static_interest_rate"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PublicGetTickerParamsSchema": {
        "properties": {
          "instrument_name": {
            "title": "instrument_name",
            "type": "string",
            "description": "Instrument name"
          }
        },
        "required": [
          "instrument_name"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PublicGetTickerResponseSchema": {
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
            "$ref": "#/components/schemas/PublicGetTickerResultSchema"
          }
        },
        "required": [
          "id",
          "result"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PublicGetTickerResultSchema": {
        "properties": {
          "amount_step": {
            "title": "amount_step",
            "type": "string",
            "format": "decimal",
            "description": "Minimum valid increment of order amount"
          },
          "base_asset_address": {
            "title": "base_asset_address",
            "type": "string",
            "description": "Blockchain address of the base asset"
          },
          "base_asset_sub_id": {
            "title": "base_asset_sub_id",
            "type": "string",
            "description": "Sub ID of the specific base asset as defined in Asset.sol"
          },
          "base_currency": {
            "title": "base_currency",
            "type": "string",
            "description": "Underlying currency of base asset (`ETH`, `BTC`, etc)"
          },
          "base_fee": {
            "title": "base_fee",
            "type": "string",
            "format": "decimal",
            "description": "$ base fee added to every taker order"
          },
          "best_ask_amount": {
            "title": "best_ask_amount",
            "type": "string",
            "format": "decimal",
            "description": "Amount of contracts / tokens available at best ask price"
          },
          "best_ask_price": {
            "title": "best_ask_price",
            "type": "string",
            "format": "decimal",
            "description": "Best ask price"
          },
          "best_bid_amount": {
            "title": "best_bid_amount",
            "type": "string",
            "format": "decimal",
            "description": "Amount of contracts / tokens available at best bid price"
          },
          "best_bid_price": {
            "title": "best_bid_price",
            "type": "string",
            "format": "decimal",
            "description": "Best bid price"
          },
          "erc20_details": {
            "$ref": "#/components/schemas/ERC20PublicDetailsSchema",
            "nullable": true
          },
          "fifo_min_allocation": {
            "title": "fifo_min_allocation",
            "type": "string",
            "format": "decimal",
            "description": "Minimum number of contracts that get filled using FIFO. Actual number of contracts that gets filled by FIFO will be the max between this value and (1 - pro_rata_fraction) x order_amount, plus any size leftovers due to rounding."
          },
          "five_percent_ask_depth": {
            "title": "five_percent_ask_depth",
            "type": "string",
            "format": "decimal",
            "description": "Total amount of contracts / tokens available at 5 percent above best ask price"
          },
          "five_percent_bid_depth": {
            "title": "five_percent_bid_depth",
            "type": "string",
            "format": "decimal",
            "description": "Total amount of contracts / tokens available at 5 percent below best bid price"
          },
          "index_price": {
            "title": "index_price",
            "type": "string",
            "format": "decimal",
            "description": "Index price"
          },
          "instrument_name": {
            "title": "instrument_name",
            "type": "string",
            "description": "Instrument name"
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
          "is_active": {
            "title": "is_active",
            "type": "boolean",
            "description": "If `True`: instrument is tradeable within `activation` and `deactivation` timestamps"
          },
          "maker_fee_rate": {
            "title": "maker_fee_rate",
            "type": "string",
            "format": "decimal",
            "description": "Percent of spot price fee rate for makers"
          },
          "mark_price": {
            "title": "mark_price",
            "type": "string",
            "format": "decimal",
            "description": "Mark price"
          },
          "mark_price_fee_rate_cap": {
            "title": "mark_price_fee_rate_cap",
            "type": "string",
            "format": "decimal",
            "default": null,
            "description": "Percent of option price fee cap, e.g. 12.5%, null if not applicable",
            "nullable": true
          },
          "max_price": {
            "title": "max_price",
            "type": "string",
            "format": "decimal",
            "description": "Maximum price at which an agressive buyer can be matched. Any portion of a market order that would execute above this price will be cancelled. A limit buy order with limit price above this value is treated as post only (i.e. it will be rejected if it would cross any existing resting order)."
          },
          "maximum_amount": {
            "title": "maximum_amount",
            "type": "string",
            "format": "decimal",
            "description": "Maximum valid amount of contracts / tokens per trade"
          },
          "min_price": {
            "title": "min_price",
            "type": "string",
            "format": "decimal",
            "description": "Minimum price at which an agressive seller can be matched. Any portion of a market order that would execute below this price will be cancelled. A limit sell order with limit price below this value is treated as post only (i.e. it will be rejected if it would cross any existing resting order)."
          },
          "minimum_amount": {
            "title": "minimum_amount",
            "type": "string",
            "format": "decimal",
            "description": "Minimum valid amount of contracts / tokens per trade"
          },
          "open_interest": {
            "title": "open_interest",
            "type": "object",
            "description": "Margin type of subaccount (`PM` (Portfolio Margin), `PM2` (Portfolio Margin 2), or `SM` (Standard Margin)) -> (current open interest, open interest cap, manager currency)",
            "additionalProperties": {
              "title": "open_interest",
              "type": "array",
              "items": {
                "$ref": "#/components/schemas/OpenInterestStatsSchema"
              }
            }
          },
          "option_details": {
            "$ref": "#/components/schemas/OptionPublicDetailsSchema",
            "nullable": true
          },
          "option_pricing": {
            "$ref": "#/components/schemas/OptionPricingSchema",
            "nullable": true
          },
          "perp_details": {
            "$ref": "#/components/schemas/PerpPublicDetailsSchema",
            "nullable": true
          },
          "pro_rata_amount_step": {
            "title": "pro_rata_amount_step",
            "type": "string",
            "format": "decimal",
            "description": "Pro-rata fill share of every order is rounded down to be a multiple of this number. Leftovers of the order due to rounding are filled FIFO."
          },
          "pro_rata_fraction": {
            "title": "pro_rata_fraction",
            "type": "string",
            "format": "decimal",
            "description": "Fraction of order that gets filled using pro-rata matching. If zero, the matching is full FIFO."
          },
          "quote_currency": {
            "title": "quote_currency",
            "type": "string",
            "description": "Quote currency (`USD` for perps, `USDC` for options)"
          },
          "scheduled_activation": {
            "title": "scheduled_activation",
            "type": "integer",
            "description": "Timestamp at which became or will become active (if applicable)"
          },
          "scheduled_deactivation": {
            "title": "scheduled_deactivation",
            "type": "integer",
            "description": "Scheduled deactivation time for instrument (if applicable)"
          },
          "stats": {
            "$ref": "#/components/schemas/AggregateTradingStatsSchema"
          },
          "taker_fee_rate": {
            "title": "taker_fee_rate",
            "type": "string",
            "format": "decimal",
            "description": "Percent of spot price fee rate for takers"
          },
          "tick_size": {
            "title": "tick_size",
            "type": "string",
            "format": "decimal",
            "description": "Tick size of the instrument, i.e. minimum price increment"
          },
          "timestamp": {
            "title": "timestamp",
            "type": "integer",
            "description": "Timestamp of the ticker feed snapshot"
          }
        },
        "required": [
          "amount_step",
          "base_asset_address",
          "base_asset_sub_id",
          "base_currency",
          "base_fee",
          "best_ask_amount",
          "best_ask_price",
          "best_bid_amount",
          "best_bid_price",
          "erc20_details",
          "fifo_min_allocation",
          "five_percent_ask_depth",
          "five_percent_bid_depth",
          "index_price",
          "instrument_name",
          "instrument_type",
          "is_active",
          "maker_fee_rate",
          "mark_price",
          "max_price",
          "maximum_amount",
          "min_price",
          "minimum_amount",
          "open_interest",
          "option_details",
          "option_pricing",
          "perp_details",
          "pro_rata_amount_step",
          "pro_rata_fraction",
          "quote_currency",
          "scheduled_activation",
          "scheduled_deactivation",
          "stats",
          "taker_fee_rate",
          "tick_size",
          "timestamp"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "OptionPricingSchema": {
        "properties": {
          "ask_iv": {
            "title": "ask_iv",
            "type": "string",
            "format": "decimal",
            "description": "Implied volatility of the current best ask"
          },
          "bid_iv": {
            "title": "bid_iv",
            "type": "string",
            "format": "decimal",
            "description": "Implied volatility of the current best bid"
          },
          "delta": {
            "title": "delta",
            "type": "string",
            "format": "decimal",
            "description": "Delta of the option"
          },
          "discount_factor": {
            "title": "discount_factor",
            "type": "string",
            "format": "decimal",
            "description": "Discount factor used to calculate option premium"
          },
          "forward_price": {
            "title": "forward_price",
            "type": "string",
            "format": "decimal",
            "description": "Forward price used to calculate option premium"
          },
          "gamma": {
            "title": "gamma",
            "type": "string",
            "format": "decimal",
            "description": "Gamma of the option"
          },
          "iv": {
            "title": "iv",
            "type": "string",
            "format": "decimal",
            "description": "Implied volatility of the option"
          },
          "mark_price": {
            "title": "mark_price",
            "type": "string",
            "format": "decimal",
            "description": "Mark price of the option"
          },
          "rho": {
            "title": "rho",
            "type": "string",
            "format": "decimal",
            "description": "Rho of the option"
          },
          "theta": {
            "title": "theta",
            "type": "string",
            "format": "decimal",
            "description": "Theta of the option"
          },
          "vega": {
            "title": "vega",
            "type": "string",
            "format": "decimal",
            "description": "Vega of the option"
          }
        },
        "required": [
          "ask_iv",
          "bid_iv",
          "delta",
          "discount_factor",
          "forward_price",
          "gamma",
          "iv",
          "mark_price",
          "rho",
          "theta",
          "vega"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "AggregateTradingStatsSchema": {
        "properties": {
          "contract_volume": {
            "title": "contract_volume",
            "type": "string",
            "format": "decimal",
            "description": "Number of contracts traded during last 24 hours"
          },
          "high": {
            "title": "high",
            "type": "string",
            "format": "decimal",
            "description": "Highest trade price during last 24h"
          },
          "low": {
            "title": "low",
            "type": "string",
            "format": "decimal",
            "description": "Lowest trade price during last 24h"
          },
          "num_trades": {
            "title": "num_trades",
            "type": "string",
            "format": "decimal",
            "description": "Number of trades during last 24h "
          },
          "open_interest": {
            "title": "open_interest",
            "type": "string",
            "format": "decimal",
            "description": "Current total open interest"
          },
          "percent_change": {
            "title": "percent_change",
            "type": "string",
            "format": "decimal",
            "description": "24-hour price change expressed as a percentage. Options: percent change in vol; Perps: percent change in mark price"
          },
          "usd_change": {
            "title": "usd_change",
            "type": "string",
            "format": "decimal",
            "description": "24-hour price change in USD."
          }
        },
        "required": [
          "contract_volume",
          "high",
          "low",
          "num_trades",
          "open_interest",
          "percent_change",
          "usd_change"
        ],
        "type": "object",
        "additionalProperties": false
      }
    }
  }
}
```