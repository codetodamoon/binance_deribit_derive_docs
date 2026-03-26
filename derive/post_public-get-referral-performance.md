# Get Referral Performance

Get the broker program referral performance. Epochs are 28 days long.

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
    "/public/get_referral_performance": {
      "post": {
        "tags": [
          "Public"
        ],
        "summary": "Get Referral Performance",
        "description": "Get the broker program referral performance. Epochs are 28 days long.",
        "responses": {
          "200": {
            "description": "successful operation",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/PublicGetReferralPerformanceResponseSchema"
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
                "$ref": "#/components/schemas/PublicGetReferralPerformanceParamsSchema"
              }
            }
          }
        }
      }
    }
  },
  "components": {
    "schemas": {
      "PublicGetReferralPerformanceParamsSchema": {
        "properties": {
          "end_ms": {
            "title": "end_ms",
            "type": "integer",
            "description": "End timestamp in UTC milliseconds"
          },
          "referral_code": {
            "title": "referral_code",
            "type": "string",
            "default": null,
            "description": "(Optional) referral code",
            "nullable": true
          },
          "start_ms": {
            "title": "start_ms",
            "type": "integer",
            "description": "Start timestamp in UTC milliseconds"
          },
          "wallet": {
            "title": "wallet",
            "type": "string",
            "default": null,
            "description": "(Optional) wallet of the referrer",
            "nullable": true
          }
        },
        "required": [
          "end_ms",
          "start_ms"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PublicGetReferralPerformanceResponseSchema": {
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
            "$ref": "#/components/schemas/PublicGetReferralPerformanceResultSchema"
          }
        },
        "required": [
          "id",
          "result"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PublicGetReferralPerformanceResultSchema": {
        "properties": {
          "fee_share_percentage": {
            "title": "fee_share_percentage",
            "type": "string",
            "format": "decimal",
            "description": "Fee share percentage rewarded to referrer"
          },
          "referral_code": {
            "title": "referral_code",
            "type": "string",
            "description": "Referral code used to get performance"
          },
          "rewards": {
            "title": "rewards",
            "type": "object",
            "description": "Performance by liquidity role / currency / instrument type",
            "additionalProperties": {
              "title": "rewards",
              "type": "object",
              "additionalProperties": {
                "title": "rewards",
                "type": "object",
                "additionalProperties": {
                  "$ref": "#/components/schemas/ReferralPerformanceByInstrumentTypeSchema"
                }
              }
            }
          },
          "stdrv_balance": {
            "title": "stdrv_balance",
            "type": "string",
            "format": "decimal",
            "description": "Staked DRV held used to determine fee share percentage"
          },
          "total_builder_fee_collected": {
            "title": "total_builder_fee_collected",
            "type": "string",
            "format": "decimal",
            "description": "Total builder fee collected (collected using the extra_fee field in orders)"
          },
          "total_fee_rewards": {
            "title": "total_fee_rewards",
            "type": "string",
            "format": "decimal",
            "description": "Total fee rewards to referrers"
          },
          "total_notional_volume": {
            "title": "total_notional_volume",
            "type": "string",
            "format": "decimal",
            "description": "Total referred notional volume"
          },
          "total_referred_fees": {
            "title": "total_referred_fees",
            "type": "string",
            "format": "decimal",
            "description": "Total fees paid by referred traders (double counts if both taker and maker of a trade with rebated fees)"
          }
        },
        "required": [
          "fee_share_percentage",
          "referral_code",
          "rewards",
          "stdrv_balance",
          "total_builder_fee_collected",
          "total_fee_rewards",
          "total_notional_volume",
          "total_referred_fees"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "ReferralPerformanceByInstrumentTypeSchema": {
        "properties": {
          "builder_fee": {
            "title": "builder_fee",
            "type": "string",
            "format": "decimal",
            "description": "Builder fee (a.k.a extra_fee) collected from referred trader"
          },
          "fee_reward": {
            "title": "fee_reward",
            "type": "string",
            "format": "decimal",
            "description": "Fee reward to referrer"
          },
          "notional_volume": {
            "title": "notional_volume",
            "type": "string",
            "format": "decimal",
            "description": "Notional volume"
          },
          "referred_fee": {
            "title": "referred_fee",
            "type": "string",
            "format": "decimal",
            "description": "Fees paid by referred trader"
          }
        },
        "required": [
          "builder_fee",
          "fee_reward",
          "notional_volume",
          "referred_fee"
        ],
        "type": "object",
        "additionalProperties": false
      }
    }
  }
}
```