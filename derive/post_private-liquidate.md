# Liquidate

Liquidates a given subaccount using funds from another subaccount. This endpoint has a few limitations:<br />1. If succesful, the RPC will freeze the caller's subaccount until the bid is settled or is reverted on chain.<br />2. The caller's subaccount must not have any open orders.<br />3. The caller's subaccount must have enough withdrawable cash to cover the bid and the buffer margin requirements.
Required minimum session key permission level is `admin`

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
    "/private/liquidate": {
      "post": {
        "tags": [
          "Private"
        ],
        "summary": "Liquidate",
        "description": "Liquidates a given subaccount using funds from another subaccount. This endpoint has a few limitations:<br />1. If succesful, the RPC will freeze the caller's subaccount until the bid is settled or is reverted on chain.<br />2. The caller's subaccount must not have any open orders.<br />3. The caller's subaccount must have enough withdrawable cash to cover the bid and the buffer margin requirements.\nRequired minimum session key permission level is `admin`",
        "responses": {
          "200": {
            "description": "successful operation",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/PrivateLiquidateResponseSchema"
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
                "$ref": "#/components/schemas/PrivateLiquidateParamsSchema"
              }
            }
          }
        }
      }
    }
  },
  "components": {
    "schemas": {
      "PrivateLiquidateParamsSchema": {
        "properties": {
          "cash_transfer": {
            "title": "cash_transfer",
            "type": "string",
            "format": "decimal",
            "description": "Amount of cash to transfer to a newly created subaccount for bidding. Must be non-negative."
          },
          "last_seen_trade_id": {
            "title": "last_seen_trade_id",
            "type": "integer",
            "description": "Last seen trade ID for account being liquidated. Not checked if set to 0."
          },
          "liquidated_subaccount_id": {
            "title": "liquidated_subaccount_id",
            "type": "integer",
            "description": "Subaccount ID of the account to be liquidated."
          },
          "nonce": {
            "title": "nonce",
            "type": "integer",
            "description": "Unique nonce defined as (UTC_timestamp in ms)(random_number_up_to_6_digits) (e.g. 1695836058725001, where 001 is the random number)"
          },
          "percent_bid": {
            "title": "percent_bid",
            "type": "string",
            "format": "decimal",
            "description": "Percent of the liquidated position to bid for. Will bid for the maximum possible percent of the position if set to 1"
          },
          "price_limit": {
            "title": "price_limit",
            "type": "string",
            "format": "decimal",
            "description": "Maximum amount of cash to be paid from bidder to liquidated account (supports negative amounts for insolvent auctions). Not checked if set to 0."
          },
          "signature": {
            "title": "signature",
            "type": "string",
            "description": "Ethereum signature of the order"
          },
          "signature_expiry_sec": {
            "title": "signature_expiry_sec",
            "type": "integer",
            "description": "Unix timestamp in seconds. Order signature becomes invalid after this time, and the system will cancel the order.Expiry MUST be at least 5 min from now."
          },
          "signer": {
            "title": "signer",
            "type": "string",
            "description": "Owner wallet address or registered session key that signed order"
          },
          "subaccount_id": {
            "title": "subaccount_id",
            "type": "integer",
            "description": "Subaccount ID owned by wallet, that will be doing the bidding."
          }
        },
        "required": [
          "cash_transfer",
          "last_seen_trade_id",
          "liquidated_subaccount_id",
          "nonce",
          "percent_bid",
          "price_limit",
          "signature",
          "signature_expiry_sec",
          "signer",
          "subaccount_id"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PrivateLiquidateResponseSchema": {
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
            "$ref": "#/components/schemas/PrivateLiquidateResultSchema"
          }
        },
        "required": [
          "id",
          "result"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PrivateLiquidateResultSchema": {
        "properties": {
          "estimated_bid_price": {
            "title": "estimated_bid_price",
            "type": "string",
            "format": "decimal",
            "description": "Estimated bid price for this liquidation"
          },
          "estimated_discount_pnl": {
            "title": "estimated_discount_pnl",
            "type": "string",
            "format": "decimal",
            "description": "Estimated profit (increase in the subaccount mark value) if the liquidation is successful."
          },
          "estimated_percent_bid": {
            "title": "estimated_percent_bid",
            "type": "string",
            "format": "decimal",
            "description": "Estimated percent of account the bid will aquire"
          },
          "transaction_id": {
            "title": "transaction_id",
            "type": "string",
            "format": "uuid",
            "description": "The transaction id of the related settlement transaction"
          }
        },
        "required": [
          "estimated_bid_price",
          "estimated_discount_pnl",
          "estimated_percent_bid",
          "transaction_id"
        ],
        "type": "object",
        "additionalProperties": false
      }
    }
  }
}
```