# Get Interest Rate History

Get latest USDC interest rate history

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
    "/public/get_interest_rate_history": {
      "post": {
        "tags": [
          "Public"
        ],
        "summary": "Get Interest Rate History",
        "description": "Get latest USDC interest rate history",
        "responses": {
          "200": {
            "description": "successful operation",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/PublicGetInterestRateHistoryResponseSchema"
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
                "$ref": "#/components/schemas/PublicGetInterestRateHistoryParamsSchema"
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
      "PublicGetInterestRateHistoryParamsSchema": {
        "properties": {
          "from_timestamp_sec": {
            "title": "from_timestamp_sec",
            "type": "integer",
            "description": "From timestamp in seconds"
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
          "to_timestamp_sec": {
            "title": "to_timestamp_sec",
            "type": "integer",
            "description": "To timestamp in seconds"
          }
        },
        "required": [
          "from_timestamp_sec",
          "to_timestamp_sec"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PublicGetInterestRateHistoryResponseSchema": {
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
            "$ref": "#/components/schemas/PublicGetInterestRateHistoryResultSchema"
          }
        },
        "required": [
          "id",
          "result"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PublicGetInterestRateHistoryResultSchema": {
        "properties": {
          "interest_rates": {
            "title": "interest_rates",
            "type": "array",
            "description": "List of interest rates, recent first",
            "items": {
              "$ref": "#/components/schemas/InterestRateHistoryResponseSchema"
            }
          },
          "pagination": {
            "$ref": "#/components/schemas/PaginationInfoSchema"
          }
        },
        "required": [
          "interest_rates",
          "pagination"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "InterestRateHistoryResponseSchema": {
        "properties": {
          "block": {
            "title": "block",
            "type": "integer",
            "description": "Derive Chain block number"
          },
          "borrow_apy": {
            "title": "borrow_apy",
            "type": "string",
            "format": "decimal",
            "description": "Borrow APY"
          },
          "supply_apy": {
            "title": "supply_apy",
            "type": "string",
            "format": "decimal",
            "description": "Supply APY"
          },
          "timestamp_sec": {
            "title": "timestamp_sec",
            "type": "integer",
            "description": "Timestamp in seconds"
          },
          "total_borrow": {
            "title": "total_borrow",
            "type": "string",
            "format": "decimal",
            "description": "Total USDC borrowed"
          },
          "total_supply": {
            "title": "total_supply",
            "type": "string",
            "format": "decimal",
            "description": "Total USDC supplied"
          }
        },
        "required": [
          "block",
          "borrow_apy",
          "supply_apy",
          "timestamp_sec",
          "total_borrow",
          "total_supply"
        ],
        "type": "object",
        "additionalProperties": false
      }
    }
  }
}
```