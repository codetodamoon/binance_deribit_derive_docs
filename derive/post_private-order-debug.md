# Order Debug

Debug a new order
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
    "/private/order_debug": {
      "post": {
        "tags": [
          "Private"
        ],
        "summary": "Order Debug",
        "description": "Debug a new order\nRequired minimum session key permission level is `read_only`",
        "responses": {
          "200": {
            "description": "successful operation",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/PrivateOrderDebugResponseSchema"
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
                "$ref": "#/components/schemas/PrivateOrderDebugParamsSchema"
              }
            }
          }
        }
      }
    }
  },
  "components": {
    "schemas": {
      "PrivateOrderDebugParamsSchema": {
        "properties": {
          "amount": {
            "title": "amount",
            "type": "string",
            "format": "decimal",
            "description": "Order amount in units of the base"
          },
          "client": {
            "title": "client",
            "type": "string",
            "default": "",
            "description": "Client that submitted the order",
            "nullable": true
          },
          "direction": {
            "title": "direction",
            "type": "string",
            "enum": [
              "buy",
              "sell"
            ],
            "description": "Order direction"
          },
          "extra_fee": {
            "title": "extra_fee",
            "type": "string",
            "format": "decimal",
            "default": "0",
            "description": "Extra fee in USDC added to the total final fee paid by user and directly sent to client / builder (must be between 0.000001 and 1000 USDC). The `referral_code` field must also be filled out. See Builder Fee page in docs for more info."
          },
          "instrument_name": {
            "title": "instrument_name",
            "type": "string",
            "description": "Instrument name"
          },
          "is_atomic_signing": {
            "title": "is_atomic_signing",
            "type": "boolean",
            "default": false,
            "description": "Used by vaults to determine whether the signature is an EIP-1271 signature.",
            "nullable": true
          },
          "label": {
            "title": "label",
            "type": "string",
            "default": "",
            "description": "Optional user-defined label for the order"
          },
          "limit_price": {
            "title": "limit_price",
            "type": "string",
            "format": "decimal",
            "description": "Limit price in quote currency.<br />This field is still required for market orders because it is a component of the signature. However, market orders will not leave a resting order in the book in case of a partial fill."
          },
          "max_fee": {
            "title": "max_fee",
            "type": "string",
            "format": "decimal",
            "description": "Max fee per unit of volume, denominated in units of the quote currency (usually USDC).Order will be rejected if the supplied max fee is below the estimated fee for this order."
          },
          "mmp": {
            "title": "mmp",
            "type": "boolean",
            "default": false,
            "description": "Whether the order is tagged for market maker protections (default false)"
          },
          "nonce": {
            "title": "nonce",
            "type": "integer",
            "description": "Unique nonce defined as (UTC_timestamp in ms)(random_number_up_to_3_digits) (e.g. 1695836058725001, where 001 is the random number).Note, using a random number beyond 3 digits will cause JSON serialization to fail."
          },
          "order_type": {
            "title": "order_type",
            "type": "string",
            "default": "limit",
            "enum": [
              "limit",
              "market"
            ],
            "description": "Order type:<br />- `limit`: limit order (default)<br />- `market`: market order, note that limit_price is still required for market orders, but unfilled order portion will be marked as cancelled"
          },
          "reduce_only": {
            "title": "reduce_only",
            "type": "boolean",
            "default": false,
            "description": "If true, the order will not be able to increase position's size (default false). If the order amount exceeds available position size, the order will be filled up to the position size and the remainder will be cancelled. This flag is only supported for market orders or non-resting limit orders (IOC or FOK)"
          },
          "referral_code": {
            "title": "referral_code",
            "type": "string",
            "default": "",
            "description": "Optional referral code for the order"
          },
          "reject_post_only": {
            "title": "reject_post_only",
            "type": "boolean",
            "default": true,
            "description": "If true (default), post-only orders that would cross the book are rejected. If false, the limit price is adjusted to be 1 tick away from the BBO instead of rejecting."
          },
          "reject_timestamp": {
            "title": "reject_timestamp",
            "type": "integer",
            "default": 9223372036854776000,
            "description": "UTC timestamp in ms, if provided the matching engine will reject the order with an error if `reject_timestamp` < `server_time`. Note that the timestamp must be consistent with the server time: use `public/get_time` method to obtain current server time."
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
            "description": "Subaccount ID"
          },
          "time_in_force": {
            "title": "time_in_force",
            "type": "string",
            "default": "gtc",
            "enum": [
              "gtc",
              "post_only",
              "fok",
              "ioc"
            ],
            "description": "Time in force behaviour:<br />- `gtc`: good til cancelled (default)<br />- `post_only`: a limit order that will be rejected if it crosses any order in the book, i.e. acts as a taker order<br />- `fok`: fill or kill, will be rejected if it is not fully filled<br />- `ioc`: immediate or cancel, fill at best bid/ask (market) or at limit price (limit), the unfilled portion is cancelled<br />Note that the order will still expire on the `signature_expiry_sec` timestamp."
          },
          "trigger_price": {
            "title": "trigger_price",
            "type": "string",
            "format": "decimal",
            "default": null,
            "description": "(Required for trigger orders) \"index\" or \"mark\" price to trigger order at",
            "nullable": true
          },
          "trigger_price_type": {
            "title": "trigger_price_type",
            "type": "string",
            "default": null,
            "enum": [
              "mark",
              "index"
            ],
            "description": "(Required for trigger orders) Trigger with \"mark\" price as \"index\" price type not supported yet.",
            "nullable": true
          },
          "trigger_type": {
            "title": "trigger_type",
            "type": "string",
            "default": null,
            "enum": [
              "stoploss",
              "takeprofit"
            ],
            "description": "(Required for trigger orders) \"stoploss\" or \"takeprofit\"",
            "nullable": true
          }
        },
        "required": [
          "amount",
          "direction",
          "instrument_name",
          "limit_price",
          "max_fee",
          "nonce",
          "signature",
          "signature_expiry_sec",
          "signer",
          "subaccount_id"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PrivateOrderDebugResponseSchema": {
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
            "$ref": "#/components/schemas/PrivateOrderDebugResultSchema"
          }
        },
        "required": [
          "id",
          "result"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PrivateOrderDebugResultSchema": {
        "properties": {
          "action_hash": {
            "title": "action_hash",
            "type": "string",
            "description": "Keccak hashed action data"
          },
          "encoded_data": {
            "title": "encoded_data",
            "type": "string",
            "description": "ABI encoded order data"
          },
          "encoded_data_hashed": {
            "title": "encoded_data_hashed",
            "type": "string",
            "description": "Keccak hashed encoded_data"
          },
          "raw_data": {
            "$ref": "#/components/schemas/SignedTradeOrderSchema"
          },
          "typed_data_hash": {
            "title": "typed_data_hash",
            "type": "string",
            "description": "EIP 712 typed data hash"
          }
        },
        "required": [
          "action_hash",
          "encoded_data",
          "encoded_data_hashed",
          "raw_data",
          "typed_data_hash"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "SignedTradeOrderSchema": {
        "properties": {
          "data": {
            "$ref": "#/components/schemas/TradeModuleDataSchema"
          },
          "expiry": {
            "title": "expiry",
            "type": "integer"
          },
          "is_atomic_signing": {
            "title": "is_atomic_signing",
            "type": "boolean"
          },
          "module": {
            "title": "module",
            "type": "string"
          },
          "nonce": {
            "title": "nonce",
            "type": "integer"
          },
          "owner": {
            "title": "owner",
            "type": "string"
          },
          "signature": {
            "title": "signature",
            "type": "string"
          },
          "signer": {
            "title": "signer",
            "type": "string"
          },
          "subaccount_id": {
            "title": "subaccount_id",
            "type": "integer"
          }
        },
        "required": [
          "data",
          "expiry",
          "is_atomic_signing",
          "module",
          "nonce",
          "owner",
          "signature",
          "signer",
          "subaccount_id"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "TradeModuleDataSchema": {
        "properties": {
          "asset": {
            "title": "asset",
            "type": "string"
          },
          "desired_amount": {
            "title": "desired_amount",
            "type": "string",
            "format": "decimal"
          },
          "is_bid": {
            "title": "is_bid",
            "type": "boolean"
          },
          "limit_price": {
            "title": "limit_price",
            "type": "string",
            "format": "decimal"
          },
          "recipient_id": {
            "title": "recipient_id",
            "type": "integer"
          },
          "sub_id": {
            "title": "sub_id",
            "type": "integer"
          },
          "trade_id": {
            "title": "trade_id",
            "type": "string"
          },
          "worst_fee": {
            "title": "worst_fee",
            "type": "string",
            "format": "decimal"
          }
        },
        "required": [
          "asset",
          "desired_amount",
          "is_bid",
          "limit_price",
          "recipient_id",
          "sub_id",
          "trade_id",
          "worst_fee"
        ],
        "type": "object",
        "additionalProperties": false
      }
    }
  }
}
```