# Get Funding Rate History

Get funding rate history. Start timestamp is restricted to at most 30 days ago.<br />End timestamp greater than current time will be truncated to current time.<br />Zero start timestamp is allowed and will default to 30 days from the end timestamp.<br /><br />DB: read replica

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
    "/public/get_funding_rate_history": {
      "post": {
        "tags": [
          "Public"
        ],
        "summary": "Get Funding Rate History",
        "description": "Get funding rate history. Start timestamp is restricted to at most 30 days ago.<br />End timestamp greater than current time will be truncated to current time.<br />Zero start timestamp is allowed and will default to 30 days from the end timestamp.<br /><br />DB: read replica",
        "responses": {
          "200": {
            "description": "successful operation",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/PublicGetFundingRateHistoryResponseSchema"
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
                "$ref": "#/components/schemas/PublicGetFundingRateHistoryParamsSchema"
              }
            }
          }
        }
      }
    }
  },
  "components": {
    "schemas": {
      "PublicGetFundingRateHistoryParamsSchema": {
        "properties": {
          "end_timestamp": {
            "title": "end_timestamp",
            "type": "integer",
            "default": 9223372036854776000,
            "description": "End timestamp of the event history (default current time)"
          },
          "instrument_name": {
            "title": "instrument_name",
            "type": "string",
            "description": "Instrument name to return funding history for"
          },
          "period": {
            "title": "period",
            "type": "string",
            "default": 3600,
            "enum": [
              900,
              3600,
              14400,
              28800,
              86400
            ],
            "description": "Period of the funding rate"
          },
          "start_timestamp": {
            "title": "start_timestamp",
            "type": "integer",
            "default": 0,
            "description": "Start timestamp of the event history (default 0)"
          }
        },
        "required": [
          "instrument_name"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PublicGetFundingRateHistoryResponseSchema": {
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
            "$ref": "#/components/schemas/PublicGetFundingRateHistoryResultSchema"
          }
        },
        "required": [
          "id",
          "result"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PublicGetFundingRateHistoryResultSchema": {
        "properties": {
          "funding_rate_history": {
            "title": "funding_rate_history",
            "type": "array",
            "description": "List of funding rates",
            "items": {
              "$ref": "#/components/schemas/FundingRateSchema"
            }
          }
        },
        "required": [
          "funding_rate_history"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "FundingRateSchema": {
        "properties": {
          "funding_rate": {
            "title": "funding_rate",
            "type": "string",
            "format": "decimal",
            "description": "Hourly funding rate value at the event time"
          },
          "timestamp": {
            "title": "timestamp",
            "type": "integer",
            "description": "Timestamp of the funding rate update (in ms since UNIX epoch)"
          }
        },
        "required": [
          "funding_rate",
          "timestamp"
        ],
        "type": "object",
        "additionalProperties": false
      }
    }
  }
}
```