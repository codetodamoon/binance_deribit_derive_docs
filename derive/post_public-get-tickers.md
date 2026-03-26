# Get Tickers

Get tickers information (best bid / ask, stats, etc.) for a multiple instruments.<br /><br />For options: currency is required and expiry_date is required.<br />For perps: currency is optional, expiry_date will throw an error.<br />For erc20s: currency is optional, expiry_date will throw an error.<br /><br />For most up to date stream of tickers, use the `ticker.<instrument_name>.<interval>` channels.

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
    "/public/get_tickers": {
      "post": {
        "tags": [
          "Public"
        ],
        "summary": "Get Tickers",
        "description": "Get tickers information (best bid / ask, stats, etc.) for a multiple instruments.<br /><br />For options: currency is required and expiry_date is required.<br />For perps: currency is optional, expiry_date will throw an error.<br />For erc20s: currency is optional, expiry_date will throw an error.<br /><br />For most up to date stream of tickers, use the `ticker.<instrument_name>.<interval>` channels.",
        "responses": {
          "200": {
            "description": "successful operation",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/PublicGetTickersResponseSchema"
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
                "$ref": "#/components/schemas/PublicGetTickersParamsSchema"
              }
            }
          }
        }
      }
    }
  },
  "components": {
    "schemas": {
      "PublicGetTickersParamsSchema": {
        "properties": {
          "currency": {
            "title": "currency",
            "type": "string",
            "default": null,
            "description": "Underlying currency of asset (`ETH`, `BTC`, etc). Required for options.",
            "nullable": true
          },
          "expiry_date": {
            "oneOf": [
              {
                "title": "",
                "type": "string"
              },
              {
                "title": "",
                "type": "integer"
              }
            ],
            "description": "Expiry date in the form YYYYMMDD. Required for options."
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
          }
        },
        "required": [
          "instrument_type"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PublicGetTickersResponseSchema": {
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
            "$ref": "#/components/schemas/PublicGetTickersResultSchema"
          }
        },
        "required": [
          "id",
          "result"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PublicGetTickersResultSchema": {
        "properties": {
          "tickers": {
            "title": "tickers",
            "type": "object",
            "description": "Dictionary of tickers by channel",
            "additionalProperties": {
              "$ref": "#/components/schemas/TickerSlimSchema"
            }
          }
        },
        "required": [
          "tickers"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "TickerSlimSchema": {
        "properties": {
          "A": {
            "title": "A",
            "type": "string",
            "format": "decimal",
            "description": "Amount of contracts / tokens available at best ask price"
          },
          "B": {
            "title": "B",
            "type": "string",
            "format": "decimal",
            "description": "Amount of contracts / tokens available at best bid price"
          },
          "I": {
            "title": "I",
            "type": "string",
            "format": "decimal",
            "description": "Index price"
          },
          "M": {
            "title": "M",
            "type": "string",
            "format": "decimal",
            "description": "Mark price"
          },
          "a": {
            "title": "a",
            "type": "string",
            "format": "decimal",
            "description": "Best ask price"
          },
          "b": {
            "title": "b",
            "type": "string",
            "format": "decimal",
            "description": "Best bid price"
          },
          "f": {
            "title": "f",
            "type": "string",
            "format": "decimal",
            "default": null,
            "description": "Current hourly funding rate",
            "nullable": true
          },
          "maxp": {
            "title": "maxp",
            "type": "string",
            "format": "decimal",
            "description": "Maximum price at which an agressive buyer can be matched. Any portion of a market order that would execute above this price will be cancelled. A limit buy order with limit price above this value is treated as post only (i.e. it will be rejected if it would cross any existing resting order)."
          },
          "minp": {
            "title": "minp",
            "type": "string",
            "format": "decimal",
            "description": "Minimum price at which an agressive seller can be matched. Any portion of a market order that would execute below this price will be cancelled. A limit sell order with limit price below this value is treated as post only (i.e. it will be rejected if it would cross any existing resting order)."
          },
          "option_pricing": {
            "$ref": "#/components/schemas/OptionPricingSlimSchema",
            "nullable": true
          },
          "stats": {
            "$ref": "#/components/schemas/AggregateTradingStatsSlimSchema"
          },
          "t": {
            "title": "t",
            "type": "integer",
            "description": "Creation timestamp of the snapshot in milliseconds"
          }
        },
        "required": [
          "A",
          "B",
          "I",
          "M",
          "a",
          "b",
          "f",
          "maxp",
          "minp",
          "option_pricing",
          "stats",
          "t"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "OptionPricingSlimSchema": {
        "properties": {
          "ai": {
            "title": "ai",
            "type": "string",
            "format": "decimal",
            "description": "Implied volatility of the current best ask"
          },
          "bi": {
            "title": "bi",
            "type": "string",
            "format": "decimal",
            "description": "Implied volatility of the current best bid"
          },
          "d": {
            "title": "d",
            "type": "string",
            "format": "decimal",
            "description": "Delta of the option"
          },
          "df": {
            "title": "df",
            "type": "string",
            "format": "decimal",
            "description": "Discount factor used to calculate option premium"
          },
          "f": {
            "title": "f",
            "type": "string",
            "format": "decimal",
            "description": "Forward price used to calculate option premium"
          },
          "g": {
            "title": "g",
            "type": "string",
            "format": "decimal",
            "description": "Gamma of the option"
          },
          "i": {
            "title": "i",
            "type": "string",
            "format": "decimal",
            "description": "Implied volatility of the option"
          },
          "m": {
            "title": "m",
            "type": "string",
            "format": "decimal",
            "description": "Mark price of the option"
          },
          "r": {
            "title": "r",
            "type": "string",
            "format": "decimal",
            "description": "Rho of the option"
          },
          "t": {
            "title": "t",
            "type": "string",
            "format": "decimal",
            "description": "Theta of the option"
          },
          "v": {
            "title": "v",
            "type": "string",
            "format": "decimal",
            "description": "Vega of the option"
          }
        },
        "required": [
          "ai",
          "bi",
          "d",
          "df",
          "f",
          "g",
          "i",
          "m",
          "r",
          "t",
          "v"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "AggregateTradingStatsSlimSchema": {
        "properties": {
          "c": {
            "title": "c",
            "type": "string",
            "format": "decimal",
            "description": "Number of contracts traded during last 24 hours"
          },
          "h": {
            "title": "h",
            "type": "string",
            "format": "decimal",
            "description": "Highest trade price during last 24h"
          },
          "l": {
            "title": "l",
            "type": "string",
            "format": "decimal",
            "description": "Lowest trade price during last 24h"
          },
          "n": {
            "title": "n",
            "type": "integer",
            "description": "Number of trades during last 24h "
          },
          "oi": {
            "title": "oi",
            "type": "string",
            "format": "decimal",
            "description": "Current total open interest"
          },
          "p": {
            "title": "p",
            "type": "string",
            "format": "decimal",
            "description": "Options: 24hr percent change in premium; Perps: 24hr percent change in mark price"
          },
          "pr": {
            "title": "pr",
            "type": "string",
            "format": "decimal",
            "description": "Premium volume traded during last 24 hours"
          },
          "v": {
            "title": "v",
            "type": "string",
            "format": "decimal",
            "description": "Notional volume traded during last 24 hours"
          }
        },
        "required": [
          "c",
          "h",
          "l",
          "n",
          "oi",
          "p",
          "pr",
          "v"
        ],
        "type": "object",
        "additionalProperties": false
      }
    }
  }
}
```