# Get Maker Program Scores

Get scores breakdown by maker program.

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
    "/public/get_maker_program_scores": {
      "post": {
        "tags": [
          "Public"
        ],
        "summary": "Get Maker Program Scores",
        "description": "Get scores breakdown by maker program.",
        "responses": {
          "200": {
            "description": "successful operation",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/PublicGetMakerProgramScoresResponseSchema"
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
                "$ref": "#/components/schemas/PublicGetMakerProgramScoresParamsSchema"
              }
            }
          }
        }
      }
    }
  },
  "components": {
    "schemas": {
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
      },
      "PublicGetMakerProgramScoresParamsSchema": {
        "properties": {
          "epoch_start_timestamp": {
            "title": "epoch_start_timestamp",
            "type": "integer",
            "description": "Start timestamp of the program epoch"
          },
          "program_name": {
            "title": "program_name",
            "type": "string",
            "description": "Program name"
          }
        },
        "required": [
          "epoch_start_timestamp",
          "program_name"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PublicGetMakerProgramScoresResponseSchema": {
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
            "$ref": "#/components/schemas/PublicGetMakerProgramScoresResultSchema"
          }
        },
        "required": [
          "id",
          "result"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PublicGetMakerProgramScoresResultSchema": {
        "properties": {
          "program": {
            "$ref": "#/components/schemas/ProgramResponseSchema"
          },
          "scores": {
            "title": "scores",
            "type": "array",
            "description": "Scores breakdown of the program by market maker",
            "items": {
              "$ref": "#/components/schemas/ScoreBreakdownSchema"
            }
          },
          "total_score": {
            "title": "total_score",
            "type": "string",
            "format": "decimal",
            "description": "Total score across all market makers for the epoch"
          },
          "total_volume": {
            "title": "total_volume",
            "type": "string",
            "format": "decimal",
            "description": "Total volume across all market makers for the epoch"
          }
        },
        "required": [
          "program",
          "scores",
          "total_score",
          "total_volume"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "ScoreBreakdownSchema": {
        "properties": {
          "coverage_score": {
            "title": "coverage_score",
            "type": "string",
            "format": "decimal",
            "description": "Coverag component of the score of the account for this program"
          },
          "holder_boost": {
            "title": "holder_boost",
            "type": "string",
            "format": "decimal",
            "description": "A custom account multiplier for the score due to holding tokens"
          },
          "quality_score": {
            "title": "quality_score",
            "type": "string",
            "format": "decimal",
            "description": "Quality component of the score of the account for this program"
          },
          "total_score": {
            "title": "total_score",
            "type": "string",
            "format": "decimal",
            "description": "Total score of the account for this program"
          },
          "volume": {
            "title": "volume",
            "type": "string",
            "format": "decimal",
            "description": "Volume traded by the account for this epoch"
          },
          "volume_multiplier": {
            "title": "volume_multiplier",
            "type": "string",
            "format": "decimal",
            "description": "Multiplier for the volume traded by the account"
          },
          "wallet": {
            "title": "wallet",
            "type": "string",
            "description": "Wallet address of the account"
          }
        },
        "required": [
          "coverage_score",
          "holder_boost",
          "quality_score",
          "total_score",
          "volume",
          "volume_multiplier",
          "wallet"
        ],
        "type": "object",
        "additionalProperties": false
      }
    }
  }
}
```