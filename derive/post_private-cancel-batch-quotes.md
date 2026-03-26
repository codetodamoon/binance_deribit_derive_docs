# Cancel Batch Quotes

Cancels quotes given optional filters. If no filters are provided, all quotes by the subaccount are cancelled.<br />All filters are combined using `AND` logic, so mutually exclusive filters will result in no quotes being cancelled.
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
    "/private/cancel_batch_quotes": {
      "post": {
        "tags": [
          "Private"
        ],
        "summary": "Cancel Batch Quotes",
        "description": "Cancels quotes given optional filters. If no filters are provided, all quotes by the subaccount are cancelled.<br />All filters are combined using `AND` logic, so mutually exclusive filters will result in no quotes being cancelled.\nRequired minimum session key permission level is `admin`",
        "responses": {
          "200": {
            "description": "successful operation",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/PrivateCancelBatchQuotesResponseSchema"
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
                "$ref": "#/components/schemas/PrivateCancelBatchQuotesParamsSchema"
              }
            }
          }
        }
      }
    }
  },
  "components": {
    "schemas": {
      "PrivateCancelBatchQuotesParamsSchema": {
        "properties": {
          "label": {
            "title": "label",
            "type": "string",
            "default": null,
            "description": "Cancel quotes with this label",
            "nullable": true
          },
          "nonce": {
            "title": "nonce",
            "type": "integer",
            "default": null,
            "description": "Cancel quote with this nonce",
            "nullable": true
          },
          "quote_id": {
            "title": "quote_id",
            "type": "string",
            "format": "uuid",
            "default": null,
            "description": "Quote ID to cancel",
            "nullable": true
          },
          "rfq_id": {
            "title": "rfq_id",
            "type": "string",
            "format": "uuid",
            "default": null,
            "description": "Cancel quotes for this RFQ ID",
            "nullable": true
          },
          "subaccount_id": {
            "title": "subaccount_id",
            "type": "integer",
            "description": "Subaccount ID"
          }
        },
        "required": [
          "subaccount_id"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PrivateCancelBatchQuotesResponseSchema": {
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
            "$ref": "#/components/schemas/PrivateCancelBatchQuotesResultSchema"
          }
        },
        "required": [
          "id",
          "result"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PrivateCancelBatchQuotesResultSchema": {
        "properties": {
          "cancelled_ids": {
            "title": "cancelled_ids",
            "type": "array",
            "description": "Quote IDs of the cancelled quotes",
            "items": {
              "title": "cancelled_ids",
              "type": "string",
              "format": "uuid"
            }
          }
        },
        "required": [
          "cancelled_ids"
        ],
        "type": "object",
        "additionalProperties": false
      }
    }
  }
}
```