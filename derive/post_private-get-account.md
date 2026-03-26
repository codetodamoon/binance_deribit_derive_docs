# Get Account

Account details getter
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
    "/private/get_account": {
      "post": {
        "tags": [
          "Private"
        ],
        "summary": "Get Account",
        "description": "Account details getter\nRequired minimum session key permission level is `read_only`",
        "responses": {
          "200": {
            "description": "successful operation",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/PrivateGetAccountResponseSchema"
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
                "$ref": "#/components/schemas/PrivateGetAccountParamsSchema"
              }
            }
          }
        }
      }
    }
  },
  "components": {
    "schemas": {
      "PrivateGetAccountParamsSchema": {
        "properties": {
          "wallet": {
            "title": "wallet",
            "type": "string",
            "description": "Ethereum wallet address of account"
          }
        },
        "required": [
          "wallet"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PrivateGetAccountResponseSchema": {
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
            "$ref": "#/components/schemas/PrivateGetAccountResultSchema"
          }
        },
        "required": [
          "id",
          "result"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PrivateGetAccountResultSchema": {
        "properties": {
          "cancel_on_disconnect": {
            "title": "cancel_on_disconnect",
            "type": "boolean",
            "description": "Whether cancel on disconnect is enabled for the account"
          },
          "creation_timestamp_sec": {
            "title": "creation_timestamp_sec",
            "type": "integer",
            "default": null,
            "description": "Account creation timestamp in seconds",
            "nullable": true
          },
          "fee_info": {
            "$ref": "#/components/schemas/AccountFeeInfoSchema"
          },
          "is_rfq_maker": {
            "title": "is_rfq_maker",
            "type": "boolean",
            "description": "Whether account allowed to market make RFQs"
          },
          "per_endpoint_tps": {
            "title": "per_endpoint_tps",
            "type": "object",
            "description": "If a particular endpoint has a different max TPS, it will be specified here",
            "additionalProperties": {}
          },
          "referral_code": {
            "title": "referral_code",
            "type": "string",
            "default": null,
            "description": "Referral code for the account (must register with broker program first)",
            "nullable": true
          },
          "subaccount_ids": {
            "title": "subaccount_ids",
            "type": "array",
            "description": "List of subaccount_ids owned by the wallet in `SubAccounts.sol`",
            "items": {
              "title": "subaccount_ids",
              "type": "integer"
            }
          },
          "wallet": {
            "title": "wallet",
            "type": "string",
            "description": "Ethereum wallet address"
          },
          "websocket_matching_tps": {
            "title": "websocket_matching_tps",
            "type": "integer",
            "description": "Max transactions per second for matching requests over websocket (see `Rate Limiting` in docs)"
          },
          "websocket_non_matching_tps": {
            "title": "websocket_non_matching_tps",
            "type": "integer",
            "description": "Max transactions per second for non-matching requests over websocket (see `Rate Limiting` in docs)"
          },
          "websocket_option_tps": {
            "title": "websocket_option_tps",
            "type": "integer",
            "description": "Max transactions per second for EACH option instrument over websocket (see `Rate Limiting` in docs)"
          },
          "websocket_perp_tps": {
            "title": "websocket_perp_tps",
            "type": "integer",
            "description": "Max transactions per second for EACH perp instrument over websocket (see `Rate Limiting` in docs)"
          }
        },
        "required": [
          "cancel_on_disconnect",
          "fee_info",
          "is_rfq_maker",
          "per_endpoint_tps",
          "subaccount_ids",
          "wallet",
          "websocket_matching_tps",
          "websocket_non_matching_tps",
          "websocket_option_tps",
          "websocket_perp_tps"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "AccountFeeInfoSchema": {
        "properties": {
          "base_fee_discount": {
            "title": "base_fee_discount",
            "type": "string",
            "format": "decimal",
            "description": "Base fee discount"
          },
          "option_maker_fee": {
            "title": "option_maker_fee",
            "type": "string",
            "format": "decimal",
            "default": null,
            "description": "Option maker fee - uses default instrument fee rate if None",
            "nullable": true
          },
          "option_taker_fee": {
            "title": "option_taker_fee",
            "type": "string",
            "format": "decimal",
            "default": null,
            "description": "Option taker fee - uses default instrument fee rate if None",
            "nullable": true
          },
          "perp_maker_fee": {
            "title": "perp_maker_fee",
            "type": "string",
            "format": "decimal",
            "default": null,
            "description": "Perp maker fee - uses default instrument fee rate if None",
            "nullable": true
          },
          "perp_taker_fee": {
            "title": "perp_taker_fee",
            "type": "string",
            "format": "decimal",
            "default": null,
            "description": "Perp taker fee - uses default instrument fee rate if None",
            "nullable": true
          },
          "rfq_maker_discount": {
            "title": "rfq_maker_discount",
            "type": "string",
            "format": "decimal",
            "description": "RFQ maker fee discount"
          },
          "rfq_taker_discount": {
            "title": "rfq_taker_discount",
            "type": "string",
            "format": "decimal",
            "description": "RFQ taker fee discount"
          },
          "spot_maker_fee": {
            "title": "spot_maker_fee",
            "type": "string",
            "format": "decimal",
            "default": null,
            "description": "Spot maker fee - uses default instrument fee rate if None",
            "nullable": true
          },
          "spot_taker_fee": {
            "title": "spot_taker_fee",
            "type": "string",
            "format": "decimal",
            "default": null,
            "description": "Spot taker fee - uses default instrument fee rate if None",
            "nullable": true
          }
        },
        "required": [
          "base_fee_discount",
          "option_maker_fee",
          "option_taker_fee",
          "perp_maker_fee",
          "perp_taker_fee",
          "rfq_maker_discount",
          "rfq_taker_discount",
          "spot_maker_fee",
          "spot_taker_fee"
        ],
        "type": "object",
        "additionalProperties": false
      }
    }
  }
}
```