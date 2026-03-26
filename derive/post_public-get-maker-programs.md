# Get Maker Programs

Get all maker programs, including past / historical ones.

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
    "/public/get_maker_programs": {
      "post": {
        "tags": [
          "Public"
        ],
        "summary": "Get Maker Programs",
        "description": "Get all maker programs, including past / historical ones.",
        "responses": {
          "200": {
            "description": "successful operation",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/PublicGetMakerProgramsResponseSchema"
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
                "$ref": "#/components/schemas/PublicGetMakerProgramsParamsSchema"
              }
            }
          }
        }
      }
    }
  },
  "components": {
    "schemas": {
      "PublicGetMakerProgramsParamsSchema": {
        "properties": {},
        "type": "object",
        "additionalProperties": false
      },
      "PublicGetMakerProgramsResponseSchema": {
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
              "$ref": "#/components/schemas/ProgramResponseSchema"
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
      "ProgramResponseSchema": {
        "properties": {
          "asset_types": {
            "title": "asset_types",
            "type": "array",
            "description": "List of asset types covered by the program",
            "items": {
              "title": "asset_types",
              "type": "string"
            }
          },
          "currencies": {
            "title": "currencies",
            "type": "array",
            "description": "List of currencies covered by the program",
            "items": {
              "title": "currencies",
              "type": "string"
            }
          },
          "end_timestamp": {
            "title": "end_timestamp",
            "type": "integer",
            "description": "End timestamp of the epoch"
          },
          "min_notional": {
            "title": "min_notional",
            "type": "string",
            "format": "decimal",
            "description": "Minimum dollar notional to quote for eligibility"
          },
          "name": {
            "title": "name",
            "type": "string",
            "description": "Name of the program"
          },
          "rewards": {
            "title": "rewards",
            "type": "object",
            "description": "Rewards for the program as a token -> total reward amount mapping",
            "additionalProperties": {
              "title": "rewards",
              "type": "string",
              "format": "decimal"
            }
          },
          "start_timestamp": {
            "title": "start_timestamp",
            "type": "integer",
            "description": "Start timestamp of the epoch"
          }
        },
        "required": [
          "asset_types",
          "currencies",
          "end_timestamp",
          "min_notional",
          "name",
          "rewards",
          "start_timestamp"
        ],
        "type": "object",
        "additionalProperties": false
      }
    }
  }
}
```