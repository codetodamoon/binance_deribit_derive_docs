# Get Spot Feed History Candles

Get spot feed history candles by currency<br /><br />DB: read replica

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
    "/public/get_spot_feed_history_candles": {
      "post": {
        "tags": [
          "Public"
        ],
        "summary": "Get Spot Feed History Candles",
        "description": "Get spot feed history candles by currency<br /><br />DB: read replica",
        "responses": {
          "200": {
            "description": "successful operation",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/PublicGetSpotFeedHistoryCandlesResponseSchema"
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
                "$ref": "#/components/schemas/PublicGetSpotFeedHistoryCandlesParamsSchema"
              }
            }
          }
        }
      }
    }
  },
  "components": {
    "schemas": {
      "PublicGetSpotFeedHistoryCandlesParamsSchema": {
        "properties": {
          "currency": {
            "title": "currency",
            "type": "string",
            "description": "Currency"
          },
          "end_timestamp": {
            "title": "end_timestamp",
            "type": "integer",
            "description": "End timestamp"
          },
          "period": {
            "title": "period",
            "type": "string",
            "enum": [
              60,
              300,
              900,
              1800,
              3600,
              14400,
              28800,
              86400,
              604800
            ],
            "description": "Period"
          },
          "start_timestamp": {
            "title": "start_timestamp",
            "type": "integer",
            "description": "Start timestamp"
          }
        },
        "required": [
          "currency",
          "end_timestamp",
          "period",
          "start_timestamp"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PublicGetSpotFeedHistoryCandlesResponseSchema": {
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
            "$ref": "#/components/schemas/PublicGetSpotFeedHistoryCandlesResultSchema"
          }
        },
        "required": [
          "id",
          "result"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PublicGetSpotFeedHistoryCandlesResultSchema": {
        "properties": {
          "currency": {
            "title": "currency",
            "type": "string",
            "description": "Currency"
          },
          "spot_feed_history": {
            "title": "spot_feed_history",
            "type": "array",
            "description": "Spot feed history candles",
            "items": {
              "$ref": "#/components/schemas/SpotFeedHistoryCandlesResponseSchema"
            }
          }
        },
        "required": [
          "currency",
          "spot_feed_history"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "SpotFeedHistoryCandlesResponseSchema": {
        "properties": {
          "close_price": {
            "title": "close_price",
            "type": "string",
            "format": "decimal",
            "description": "Close price"
          },
          "high_price": {
            "title": "high_price",
            "type": "string",
            "format": "decimal",
            "description": "High price"
          },
          "low_price": {
            "title": "low_price",
            "type": "string",
            "format": "decimal",
            "description": "Low price"
          },
          "open_price": {
            "title": "open_price",
            "type": "string",
            "format": "decimal",
            "description": "Open price"
          },
          "price": {
            "title": "price",
            "type": "string",
            "format": "decimal",
            "description": "Spot price"
          },
          "timestamp": {
            "title": "timestamp",
            "type": "integer",
            "description": "Timestamp of when the spot price was recored into the database"
          },
          "timestamp_bucket": {
            "title": "timestamp_bucket",
            "type": "integer",
            "description": "Timestamp bucket; this value is regularly spaced out with `period` seconds between data points, missing values are forward-filled from earlier data where possible, if no earlier data is available, values are back-filled from the first observed data point"
          }
        },
        "required": [
          "close_price",
          "high_price",
          "low_price",
          "open_price",
          "price",
          "timestamp",
          "timestamp_bucket"
        ],
        "type": "object",
        "additionalProperties": false
      }
    }
  }
}
```