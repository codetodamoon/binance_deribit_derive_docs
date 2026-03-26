# Get Latest Signed Feeds

Get latest signed data feeds

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
    "/public/get_latest_signed_feeds": {
      "post": {
        "tags": [
          "Public"
        ],
        "summary": "Get Latest Signed Feeds",
        "description": "Get latest signed data feeds",
        "responses": {
          "200": {
            "description": "successful operation",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/PublicGetLatestSignedFeedsResponseSchema"
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
                "$ref": "#/components/schemas/PublicGetLatestSignedFeedsParamsSchema"
              }
            }
          }
        }
      }
    }
  },
  "components": {
    "schemas": {
      "PublicGetLatestSignedFeedsParamsSchema": {
        "properties": {
          "currency": {
            "title": "currency",
            "type": "string",
            "default": null,
            "description": "Currency filter, (defaults to all currencies)",
            "nullable": true
          },
          "expiry": {
            "title": "expiry",
            "type": "integer",
            "default": null,
            "description": "Expiry filter for options and forward data (defaults to all expiries). Use `0` to get data only for spot and perpetuals",
            "nullable": true
          }
        },
        "type": "object",
        "additionalProperties": false
      },
      "PublicGetLatestSignedFeedsResponseSchema": {
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
            "$ref": "#/components/schemas/PublicGetLatestSignedFeedsResultSchema"
          }
        },
        "required": [
          "id",
          "result"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PublicGetLatestSignedFeedsResultSchema": {
        "properties": {
          "fwd_data": {
            "title": "fwd_data",
            "type": "object",
            "description": "currency -> expiry -> latest forward feed data",
            "additionalProperties": {
              "title": "fwd_data",
              "type": "object",
              "additionalProperties": {
                "$ref": "#/components/schemas/ForwardFeedDataSchema"
              }
            }
          },
          "perp_data": {
            "title": "perp_data",
            "type": "object",
            "description": "currency -> feed type -> latest perp feed data",
            "additionalProperties": {
              "title": "perp_data",
              "type": "object",
              "additionalProperties": {
                "$ref": "#/components/schemas/PerpFeedDataSchema"
              }
            }
          },
          "rate_data": {
            "title": "rate_data",
            "type": "object",
            "description": "currency -> expiry -> latest rate feed data",
            "additionalProperties": {
              "title": "rate_data",
              "type": "object",
              "additionalProperties": {
                "$ref": "#/components/schemas/RateFeedDataSchema"
              }
            }
          },
          "spot_data": {
            "title": "spot_data",
            "type": "object",
            "description": "currency -> latest spot feed data",
            "additionalProperties": {
              "$ref": "#/components/schemas/SpotFeedDataSchema"
            }
          },
          "vol_data": {
            "title": "vol_data",
            "type": "object",
            "description": "currency -> expiry -> latest vol feed data",
            "additionalProperties": {
              "title": "vol_data",
              "type": "object",
              "additionalProperties": {
                "$ref": "#/components/schemas/VolFeedDataSchema"
              }
            }
          }
        },
        "required": [
          "fwd_data",
          "perp_data",
          "rate_data",
          "spot_data",
          "vol_data"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "ForwardFeedDataSchema": {
        "properties": {
          "confidence": {
            "title": "confidence",
            "type": "string",
            "format": "decimal",
            "description": "The confidence score of the price"
          },
          "currency": {
            "title": "currency",
            "type": "string",
            "description": "The currency for which the spot feed represents"
          },
          "deadline": {
            "title": "deadline",
            "type": "integer",
            "description": "The latest time the data can be submitted on chain"
          },
          "expiry": {
            "title": "expiry",
            "type": "integer",
            "description": "The expiry for the forward feed"
          },
          "fwd_diff": {
            "title": "fwd_diff",
            "type": "string",
            "format": "decimal",
            "description": "difference of forward price from current spot price"
          },
          "signatures": {
            "$ref": "#/components/schemas/OracleSignatureDataSchema"
          },
          "spot_aggregate_latest": {
            "title": "spot_aggregate_latest",
            "type": "string",
            "format": "decimal",
            "description": "expiry -> spot * time value at the latest timestamp"
          },
          "spot_aggregate_start": {
            "title": "spot_aggregate_start",
            "type": "string",
            "format": "decimal",
            "description": "spot * time value at the start of the settlement period"
          },
          "timestamp": {
            "title": "timestamp",
            "type": "integer",
            "description": "The timestamp for which the data was created"
          }
        },
        "required": [
          "confidence",
          "currency",
          "deadline",
          "expiry",
          "fwd_diff",
          "signatures",
          "spot_aggregate_latest",
          "spot_aggregate_start",
          "timestamp"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "OracleSignatureDataSchema": {
        "properties": {
          "signatures": {
            "title": "signatures",
            "type": "array",
            "description": "The signatures of the given signers",
            "items": {
              "title": "signatures",
              "type": "string"
            }
          },
          "signers": {
            "title": "signers",
            "type": "array",
            "description": "The signers who verify the data integrity",
            "items": {
              "title": "signers",
              "type": "string"
            }
          }
        },
        "type": "object",
        "additionalProperties": false
      },
      "PerpFeedDataSchema": {
        "properties": {
          "confidence": {
            "title": "confidence",
            "type": "string",
            "format": "decimal",
            "description": "The confidence score of the price"
          },
          "currency": {
            "title": "currency",
            "type": "string",
            "description": "The currency for which the spot feed represents"
          },
          "deadline": {
            "title": "deadline",
            "type": "integer",
            "description": "The latest time the data can be submitted on chain"
          },
          "signatures": {
            "$ref": "#/components/schemas/OracleSignatureDataSchema"
          },
          "spot_diff_value": {
            "title": "spot_diff_value",
            "type": "string",
            "format": "decimal",
            "description": "The difference between the spot price and the perp price"
          },
          "timestamp": {
            "title": "timestamp",
            "type": "integer",
            "description": "The timestamp for which the data was created"
          },
          "type": {
            "title": "type",
            "type": "string",
            "enum": [
              "P",
              "A",
              "B"
            ],
            "description": "The type of perp feed; mid price, ask impact or bid impact"
          }
        },
        "required": [
          "confidence",
          "currency",
          "deadline",
          "signatures",
          "spot_diff_value",
          "timestamp",
          "type"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "RateFeedDataSchema": {
        "properties": {
          "confidence": {
            "title": "confidence",
            "type": "string",
            "format": "decimal",
            "description": "The confidence score of the rate"
          },
          "currency": {
            "title": "currency",
            "type": "string",
            "description": "The currency for which the spot feed represents"
          },
          "deadline": {
            "title": "deadline",
            "type": "integer",
            "description": "The latest time the data can be submitted on chain"
          },
          "expiry": {
            "title": "expiry",
            "type": "integer",
            "description": "The expiry for the rate feed"
          },
          "rate": {
            "title": "rate",
            "type": "string",
            "format": "decimal",
            "description": "The implied rate for the currency/expiry"
          },
          "signatures": {
            "$ref": "#/components/schemas/OracleSignatureDataSchema"
          },
          "timestamp": {
            "title": "timestamp",
            "type": "integer",
            "description": "The timestamp for which the data was created"
          }
        },
        "required": [
          "confidence",
          "currency",
          "deadline",
          "expiry",
          "rate",
          "signatures",
          "timestamp"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "SpotFeedDataSchema": {
        "properties": {
          "confidence": {
            "title": "confidence",
            "type": "string",
            "format": "decimal",
            "description": "The confidence score of the price"
          },
          "currency": {
            "title": "currency",
            "type": "string",
            "description": "The currency for which the spot feed represents"
          },
          "deadline": {
            "title": "deadline",
            "type": "integer",
            "description": "The latest time the data can be submitted on chain"
          },
          "feed_source_type": {
            "title": "feed_source_type",
            "type": "string",
            "default": "S",
            "enum": [
              "S",
              "O"
            ],
            "description": "The source of the feed"
          },
          "price": {
            "title": "price",
            "type": "string",
            "format": "decimal",
            "description": "The price of the currency in USD"
          },
          "signatures": {
            "$ref": "#/components/schemas/OracleSignatureDataSchema"
          },
          "timestamp": {
            "title": "timestamp",
            "type": "integer",
            "description": "The timestamp for which the data was created"
          }
        },
        "required": [
          "confidence",
          "currency",
          "deadline",
          "price",
          "signatures",
          "timestamp"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "VolFeedDataSchema": {
        "properties": {
          "confidence": {
            "title": "confidence",
            "type": "string",
            "format": "decimal",
            "description": "The confidence score of the price"
          },
          "currency": {
            "title": "currency",
            "type": "string",
            "description": "The currency for which the spot feed represents"
          },
          "deadline": {
            "title": "deadline",
            "type": "integer",
            "description": "The latest time the data can be submitted on chain"
          },
          "expiry": {
            "title": "expiry",
            "type": "integer",
            "description": "The expiry for the options for the vol feed"
          },
          "signatures": {
            "$ref": "#/components/schemas/OracleSignatureDataSchema"
          },
          "timestamp": {
            "title": "timestamp",
            "type": "integer",
            "description": "The timestamp for which the data was created"
          },
          "vol_data": {
            "$ref": "#/components/schemas/VolSVIParamDataSchema"
          }
        },
        "required": [
          "confidence",
          "currency",
          "deadline",
          "expiry",
          "signatures",
          "timestamp",
          "vol_data"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "VolSVIParamDataSchema": {
        "properties": {
          "SVI_a": {
            "title": "SVI_a",
            "type": "string",
            "format": "decimal"
          },
          "SVI_b": {
            "title": "SVI_b",
            "type": "string",
            "format": "decimal"
          },
          "SVI_fwd": {
            "title": "SVI_fwd",
            "type": "string",
            "format": "decimal"
          },
          "SVI_m": {
            "title": "SVI_m",
            "type": "string",
            "format": "decimal"
          },
          "SVI_refTau": {
            "title": "SVI_refTau",
            "type": "string",
            "format": "decimal"
          },
          "SVI_rho": {
            "title": "SVI_rho",
            "type": "string",
            "format": "decimal"
          },
          "SVI_sigma": {
            "title": "SVI_sigma",
            "type": "string",
            "format": "decimal"
          }
        },
        "required": [
          "SVI_a",
          "SVI_b",
          "SVI_fwd",
          "SVI_m",
          "SVI_refTau",
          "SVI_rho",
          "SVI_sigma"
        ],
        "type": "object",
        "additionalProperties": false
      }
    }
  }
}
```