# Get Vault Statistics

Gets all the latest vault shareRate, totalSupply and TVL values for all vaults.<br /><br />For data on shares across chains, use public/get_vault_assets.

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
    "/public/get_vault_statistics": {
      "post": {
        "tags": [
          "Public"
        ],
        "summary": "Get Vault Statistics",
        "description": "Gets all the latest vault shareRate, totalSupply and TVL values for all vaults.<br /><br />For data on shares across chains, use public/get_vault_assets.",
        "responses": {
          "200": {
            "description": "successful operation",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/PublicGetVaultStatisticsResponseSchema"
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
                "$ref": "#/components/schemas/PublicGetVaultStatisticsParamsSchema"
              }
            }
          }
        }
      }
    }
  },
  "components": {
    "schemas": {
      "PublicGetVaultStatisticsParamsSchema": {
        "properties": {},
        "type": "object",
        "additionalProperties": false
      },
      "PublicGetVaultStatisticsResponseSchema": {
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
              "$ref": "#/components/schemas/VaultStatisticsResponseSchema"
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
      "VaultStatisticsResponseSchema": {
        "properties": {
          "base_value": {
            "title": "base_value",
            "type": "string",
            "format": "decimal",
            "description": "The value of the vault's token against the base currency. Ex: rswETHC vs rswETH"
          },
          "block_number": {
            "title": "block_number",
            "type": "integer",
            "description": "The Derive chain block number"
          },
          "block_timestamp": {
            "title": "block_timestamp",
            "type": "integer",
            "description": "Timestamp of the Derive chain block number"
          },
          "subaccount_value_at_last_trade": {
            "title": "subaccount_value_at_last_trade",
            "type": "string",
            "format": "decimal",
            "default": null,
            "description": "Will return None before vault creates subaccount or if no trades found.",
            "nullable": true
          },
          "total_supply": {
            "title": "total_supply",
            "type": "string",
            "format": "decimal",
            "description": "Total supply of the vault's token on Derive chain"
          },
          "underlying_value": {
            "title": "underlying_value",
            "type": "string",
            "format": "decimal",
            "default": null,
            "description": "The value of the vault's token against the underlying currency. Ex: rswETHC vs ETH",
            "nullable": true
          },
          "usd_tvl": {
            "title": "usd_tvl",
            "type": "string",
            "format": "decimal",
            "description": "Total USD TVL of the vault"
          },
          "usd_value": {
            "title": "usd_value",
            "type": "string",
            "format": "decimal",
            "description": "The value of the vault's token against USD"
          },
          "vault_name": {
            "title": "vault_name",
            "type": "string",
            "description": "Name of the vault"
          }
        },
        "required": [
          "base_value",
          "block_number",
          "block_timestamp",
          "subaccount_value_at_last_trade",
          "total_supply",
          "underlying_value",
          "usd_tvl",
          "usd_value",
          "vault_name"
        ],
        "type": "object",
        "additionalProperties": false
      }
    }
  }
}
```