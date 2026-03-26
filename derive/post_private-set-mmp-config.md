# Set Mmp Config

Set the mmp config for the subaccount and currency
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
    "/private/set_mmp_config": {
      "post": {
        "tags": [
          "Private"
        ],
        "summary": "Set Mmp Config",
        "description": "Set the mmp config for the subaccount and currency\nRequired minimum session key permission level is `account`",
        "responses": {
          "200": {
            "description": "successful operation",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/PrivateSetMmpConfigResponseSchema"
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
                "$ref": "#/components/schemas/PrivateSetMmpConfigParamsSchema"
              }
            }
          }
        }
      }
    }
  },
  "components": {
    "schemas": {
      "PrivateSetMmpConfigParamsSchema": {
        "properties": {
          "currency": {
            "title": "currency",
            "type": "string",
            "description": "Currency of this mmp config"
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
          "subaccount_id": {
            "title": "subaccount_id",
            "type": "integer",
            "description": "Subaccount_id for which to set the config"
          }
        },
        "required": [
          "currency",
          "mmp_frozen_time",
          "mmp_interval",
          "subaccount_id"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PrivateSetMmpConfigResponseSchema": {
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
            "$ref": "#/components/schemas/PrivateSetMmpConfigResultSchema"
          }
        },
        "required": [
          "id",
          "result"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PrivateSetMmpConfigResultSchema": {
        "properties": {
          "currency": {
            "title": "currency",
            "type": "string",
            "description": "Currency of this mmp config"
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
          "subaccount_id": {
            "title": "subaccount_id",
            "type": "integer",
            "description": "Subaccount_id for which to set the config"
          }
        },
        "required": [
          "currency",
          "mmp_frozen_time",
          "mmp_interval",
          "subaccount_id"
        ],
        "type": "object",
        "additionalProperties": false
      }
    }
  }
}
```