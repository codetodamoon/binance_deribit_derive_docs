# Get Notifications

Get the notifications related to a subaccount.
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
    "/private/get_notifications": {
      "post": {
        "tags": [
          "Private"
        ],
        "summary": "Get Notifications",
        "description": "Get the notifications related to a subaccount.\nRequired minimum session key permission level is `read_only`",
        "responses": {
          "200": {
            "description": "successful operation",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/PrivateGetNotificationsResponseSchema"
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
                "$ref": "#/components/schemas/PrivateGetNotificationsParamsSchema"
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
      "PrivateGetNotificationsParamsSchema": {
        "properties": {
          "page": {
            "title": "page",
            "type": "integer",
            "default": 1,
            "description": "Page number of results to return",
            "nullable": true
          },
          "page_size": {
            "title": "page_size",
            "type": "integer",
            "default": 50,
            "description": "Number of results per page (must be between 0-50)",
            "nullable": true
          },
          "status": {
            "title": "status",
            "type": "string",
            "default": null,
            "enum": [
              "unseen",
              "seen",
              "hidden"
            ],
            "description": "Status of the notification",
            "nullable": true
          },
          "subaccount_id": {
            "title": "subaccount_id",
            "type": "integer",
            "default": null,
            "description": "Subaccount_id (must be set if wallet param is not set)",
            "nullable": true
          },
          "type": {
            "title": "type",
            "type": "array",
            "default": null,
            "description": "List of notification types to filter by",
            "items": {
              "title": "type",
              "type": "string",
              "enum": [
                "deposit",
                "withdraw",
                "transfer",
                "trade",
                "settlement",
                "liquidation",
                "custom"
              ]
            },
            "nullable": true
          },
          "wallet": {
            "title": "wallet",
            "type": "string",
            "default": null,
            "description": "Wallet address (if set, subaccount_id ignored)",
            "nullable": true
          }
        },
        "type": "object",
        "additionalProperties": false
      },
      "PrivateGetNotificationsResponseSchema": {
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
            "$ref": "#/components/schemas/PrivateGetNotificationsResultSchema"
          }
        },
        "required": [
          "id",
          "result"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PrivateGetNotificationsResultSchema": {
        "properties": {
          "notifications": {
            "title": "notifications",
            "type": "array",
            "description": "Notification response",
            "items": {
              "$ref": "#/components/schemas/NotificationResponseSchema"
            }
          },
          "pagination": {
            "$ref": "#/components/schemas/PaginationInfoSchema"
          }
        },
        "required": [
          "notifications",
          "pagination"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "NotificationResponseSchema": {
        "properties": {
          "event": {
            "title": "event",
            "type": "string",
            "description": "The specific event leading to the notification."
          },
          "event_details": {
            "title": "event_details",
            "type": "object",
            "description": "A JSON-structured dictionary containing detailed data or context about the event.",
            "additionalProperties": {}
          },
          "id": {
            "title": "id",
            "type": "integer",
            "description": "The unique identifier for the notification."
          },
          "status": {
            "title": "status",
            "type": "string",
            "description": "The status of the notification, indicating if it has been read, pending, or processed."
          },
          "subaccount_id": {
            "title": "subaccount_id",
            "type": "integer",
            "description": "The subaccount_id associated with the notification."
          },
          "timestamp": {
            "title": "timestamp",
            "type": "integer",
            "description": "The timestamp indicating when the notification was created or triggered."
          },
          "transaction_id": {
            "title": "transaction_id",
            "type": "integer",
            "default": null,
            "description": "The transaction id associated with the notification.",
            "nullable": true
          },
          "tx_hash": {
            "title": "tx_hash",
            "type": "string",
            "default": null,
            "description": "The transaction hash associated with the notification.",
            "nullable": true
          }
        },
        "required": [
          "event",
          "event_details",
          "id",
          "status",
          "subaccount_id",
          "timestamp"
        ],
        "type": "object",
        "additionalProperties": false
      }
    }
  }
}
```