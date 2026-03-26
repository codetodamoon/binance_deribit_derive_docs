# Cancel Batch Rfqs

Cancels RFQs given optional filters.<br />If no filters are provided, all RFQs for the subaccount are cancelled.<br />All filters are combined using `AND` logic, so mutually exclusive filters will result in no RFQs being cancelled.
Required minimum session key permission level is `account`

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
    "/private/cancel_batch_rfqs": {
      "post": {
        "tags": [
          "Private"
        ],
        "summary": "Cancel Batch Rfqs",
        "description": "Cancels RFQs given optional filters.<br />If no filters are provided, all RFQs for the subaccount are cancelled.<br />All filters are combined using `AND` logic, so mutually exclusive filters will result in no RFQs being cancelled.\nRequired minimum session key permission level is `account`",
        "responses": {
          "200": {
            "description": "successful operation",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/PrivateCancelBatchRfqsResponseSchema"
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
                "$ref": "#/components/schemas/PrivateCancelBatchRfqsParamsSchema"
              }
            }
          }
        }
      }
    }
  },
  "components": {
    "schemas": {
      "PrivateCancelBatchRfqsParamsSchema": {
        "properties": {
          "label": {
            "title": "label",
            "type": "string",
            "default": null,
            "description": "Cancel RFQs with this label",
            "nullable": true
          },
          "nonce": {
            "title": "nonce",
            "type": "integer",
            "default": null,
            "description": "Cancel RFQ with this nonce",
            "nullable": true
          },
          "rfq_id": {
            "title": "rfq_id",
            "type": "string",
            "format": "uuid",
            "default": null,
            "description": "RFQ ID to cancel",
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
      "PrivateCancelBatchRfqsResponseSchema": {
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
            "$ref": "#/components/schemas/PrivateCancelBatchRfqsResultSchema"
          }
        },
        "required": [
          "id",
          "result"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PrivateCancelBatchRfqsResultSchema": {
        "properties": {
          "cancelled_ids": {
            "title": "cancelled_ids",
            "type": "array",
            "description": "RFQ IDs of the cancelled RFQs",
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