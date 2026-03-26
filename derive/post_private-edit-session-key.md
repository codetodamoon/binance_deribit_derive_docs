# Edit Session Key

Edits session key parameters such as label and IP whitelist.<br />For non-admin keys you can also toggle whether to disable a particular key.<br />Disabling non-admin keys must be done through /deregister_session_key
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
    "/private/edit_session_key": {
      "post": {
        "tags": [
          "Private"
        ],
        "summary": "Edit Session Key",
        "description": "Edits session key parameters such as label and IP whitelist.<br />For non-admin keys you can also toggle whether to disable a particular key.<br />Disabling non-admin keys must be done through /deregister_session_key\nRequired minimum session key permission level is `account`",
        "responses": {
          "200": {
            "description": "successful operation",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/PrivateEditSessionKeyResponseSchema"
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
                "$ref": "#/components/schemas/PrivateEditSessionKeyParamsSchema"
              }
            }
          }
        }
      }
    }
  },
  "components": {
    "schemas": {
      "PrivateEditSessionKeyParamsSchema": {
        "properties": {
          "disable": {
            "title": "disable",
            "type": "boolean",
            "default": false,
            "description": "Flag whether or not to disable to session key. Defaulted to false. Only allowed for non-admin keys. Admin keys must go through `/deregister_session_key` for now."
          },
          "ip_whitelist": {
            "title": "ip_whitelist",
            "type": "array",
            "default": null,
            "description": "Optional list of whitelisted IPs, an empty list can be supplied to whitelist all IPs",
            "items": {
              "title": "ip_whitelist",
              "type": "string"
            },
            "nullable": true
          },
          "label": {
            "title": "label",
            "type": "string",
            "default": null,
            "description": "Optional new label for the session key",
            "nullable": true
          },
          "public_session_key": {
            "title": "public_session_key",
            "type": "string",
            "description": "Session key in the form of an Ethereum EOA"
          },
          "wallet": {
            "title": "wallet",
            "type": "string",
            "description": "Ethereum wallet address of account"
          }
        },
        "required": [
          "public_session_key",
          "wallet"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PrivateEditSessionKeyResponseSchema": {
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
            "$ref": "#/components/schemas/PrivateEditSessionKeyResultSchema"
          }
        },
        "required": [
          "id",
          "result"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PrivateEditSessionKeyResultSchema": {
        "properties": {
          "expiry_sec": {
            "title": "expiry_sec",
            "type": "integer",
            "description": "Session key expiry timestamp in sec"
          },
          "ip_whitelist": {
            "title": "ip_whitelist",
            "type": "array",
            "description": "List of whitelisted IPs, if empty then any IP is allowed.",
            "items": {
              "title": "ip_whitelist",
              "type": "string"
            }
          },
          "label": {
            "title": "label",
            "type": "string",
            "description": "User-defined session key label"
          },
          "public_session_key": {
            "title": "public_session_key",
            "type": "string",
            "description": "Public session key address (Ethereum EOA)"
          },
          "registered_sec": {
            "title": "registered_sec",
            "type": "integer",
            "description": "Session key registration time"
          },
          "scope": {
            "title": "scope",
            "type": "string",
            "description": "Session key permission level scope"
          }
        },
        "required": [
          "expiry_sec",
          "ip_whitelist",
          "label",
          "public_session_key",
          "registered_sec",
          "scope"
        ],
        "type": "object",
        "additionalProperties": false
      }
    }
  }
}
```