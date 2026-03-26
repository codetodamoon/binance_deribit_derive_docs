# Get Mmp Config

Get the current mmp config for a subaccount (optionally filtered by currency)
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
    "/private/get_mmp_config": {
      "post": {
        "tags": [
          "Private"
        ],
        "summary": "Get Mmp Config",
        "description": "Get the current mmp config for a subaccount (optionally filtered by currency)\nRequired minimum session key permission level is `read_only`",
        "responses": {
          "200": {
            "description": "successful operation",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/PrivateGetMmpConfigResponseSchema"
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
                "$ref": "#/components/schemas/PrivateGetMmpConfigParamsSchema"
              }
            }
          }
        }
      }
    }
  },
  "components": {
    "schemas": {
      "PrivateGetMmpConfigParamsSchema": {
        "properties": {
          "currency": {
            "title": "currency",
            "type": "string",
            "default": null,
            "description": "Currency to get the config for. If not provided, returns all configs for the subaccount",
            "nullable": true
          },
          "subaccount_id": {
            "title": "subaccount_id",
            "type": "integer",
            "description": "Subaccount_id for which to get the config"
          }
        },
        "required": [
          "subaccount_id"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PrivateGetMmpConfigResponseSchema": {
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
            "title": "result",
            "type": "array",
            "description": "",
            "items": {
              "$ref": "#/components/schemas/MMPConfigResultSchema"
            }
          }
        },
        "required": [
          "id",
          "result"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "MMPConfigResultSchema": {
        "properties": {
          "currency": {
            "title": "currency",
            "type": "string",
            "description": "Currency of this mmp config"
          },
          "is_frozen": {
            "title": "is_frozen",
            "type": "boolean",
            "description": "Whether the subaccount is currently frozen"
          },
          "mmp_amount_limit": {
            "title": "mmp_amount_limit",
            "type": "string",
            "format": "decimal",
            "default": "0",
            "description": "Maximum total order amount that can be traded within the mmp_interval across all instruments of the provided currency. The amounts are not netted, so a filled bid of 1 and a filled ask of 2 would count as 3.<br />Default: 0 (no limit)"
          },
          "mmp_delta_limit": {
            "title": "mmp_delta_limit",
            "type": "string",
            "format": "decimal",
            "default": "0",
            "description": "Maximum total delta that can be traded within the mmp_interval across all instruments of the provided currency. This quantity is netted, so a filled order with +1 delta and a filled order with -2 delta would count as -1<br />Default: 0 (no limit)"
          },
          "mmp_frozen_time": {
            "title": "mmp_frozen_time",
            "type": "integer",
            "description": "Time interval in ms setting how long the subaccount is frozen after an mmp trigger, if 0 then a manual reset would be required via private/reset_mmp"
          },
          "mmp_interval": {
            "title": "mmp_interval",
            "type": "integer",
            "description": "Time interval in ms over which the limits are monotored, if 0 then mmp is disabled"
          },
          "mmp_unfreeze_time": {
            "title": "mmp_unfreeze_time",
            "type": "integer",
            "description": "Timestamp in ms after which the subaccount will be unfrozen"
          },
          "subaccount_id": {
            "title": "subaccount_id",
            "type": "integer",
            "description": "Subaccount_id for which to set the config"
          }
        },
        "required": [
          "currency",
          "is_frozen",
          "mmp_frozen_time",
          "mmp_interval",
          "mmp_unfreeze_time",
          "subaccount_id"
        ],
        "type": "object",
        "additionalProperties": false
      }
    }
  }
}
```