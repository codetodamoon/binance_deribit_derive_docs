# Statistics

Get statistics for a specific instrument or instrument type

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
    "/public/statistics": {
      "post": {
        "tags": [
          "Public"
        ],
        "summary": "Statistics",
        "description": "Get statistics for a specific instrument or instrument type",
        "responses": {
          "200": {
            "description": "successful operation",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/PublicStatisticsResponseSchema"
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
                "$ref": "#/components/schemas/PublicStatisticsParamsSchema"
              }
            }
          }
        }
      }
    }
  },
  "components": {
    "schemas": {
      "PublicStatisticsParamsSchema": {
        "properties": {
          "currency": {
            "title": "currency",
            "type": "string",
            "default": null,
            "description": "Currency for stats",
            "nullable": true
          },
          "end_time": {
            "title": "end_time",
            "type": "integer",
            "default": null,
            "description": "End time for statistics in ms",
            "nullable": true
          },
          "instrument_name": {
            "title": "instrument_name",
            "type": "string",
            "description": "Instrument name or 'ALL', 'OPTION', 'PERP', 'SPOT'"
          }
        },
        "required": [
          "instrument_name"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PublicStatisticsResponseSchema": {
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
            "$ref": "#/components/schemas/PublicStatisticsResultSchema"
          }
        },
        "required": [
          "id",
          "result"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PublicStatisticsResultSchema": {
        "properties": {
          "daily_fees": {
            "title": "daily_fees",
            "type": "string",
            "format": "decimal",
            "description": "24h Fees"
          },
          "daily_notional_volume": {
            "title": "daily_notional_volume",
            "type": "string",
            "format": "decimal",
            "description": "24h Notional volume"
          },
          "daily_premium_volume": {
            "title": "daily_premium_volume",
            "type": "string",
            "format": "decimal",
            "description": "24h Premium volume"
          },
          "daily_trades": {
            "title": "daily_trades",
            "type": "integer",
            "description": "24h Trades"
          },
          "open_interest": {
            "title": "open_interest",
            "type": "string",
            "format": "decimal",
            "description": "Open interest"
          },
          "total_fees": {
            "title": "total_fees",
            "type": "string",
            "format": "decimal",
            "description": "Total fees"
          },
          "total_notional_volume": {
            "title": "total_notional_volume",
            "type": "string",
            "format": "decimal",
            "description": "Total notional volume"
          },
          "total_premium_volume": {
            "title": "total_premium_volume",
            "type": "string",
            "format": "decimal",
            "description": "Total premium volume"
          },
          "total_trades": {
            "title": "total_trades",
            "type": "integer",
            "description": "Total trades"
          }
        },
        "required": [
          "daily_fees",
          "daily_notional_volume",
          "daily_premium_volume",
          "daily_trades",
          "open_interest",
          "total_fees",
          "total_notional_volume",
          "total_premium_volume",
          "total_trades"
        ],
        "type": "object",
        "additionalProperties": false
      }
    }
  }
}
```