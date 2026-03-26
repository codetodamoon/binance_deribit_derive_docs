# Get Vault Balances

Get all vault assets held by user. Can query by smart contract address or smart contract owner.<br /><br />Includes VaultERC20Pool balances

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
    "/public/get_vault_balances": {
      "post": {
        "tags": [
          "Public"
        ],
        "summary": "Get Vault Balances",
        "description": "Get all vault assets held by user. Can query by smart contract address or smart contract owner.<br /><br />Includes VaultERC20Pool balances",
        "responses": {
          "200": {
            "description": "successful operation",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/PublicGetVaultBalancesResponseSchema"
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
                "$ref": "#/components/schemas/PublicGetVaultBalancesParamsSchema"
              }
            }
          }
        }
      }
    }
  },
  "components": {
    "schemas": {
      "PublicGetVaultBalancesParamsSchema": {
        "properties": {
          "smart_contract_owner": {
            "title": "smart_contract_owner",
            "type": "string",
            "default": null,
            "description": "If wallet not provided, can query balances by EOA that owns smart contract wallet",
            "nullable": true
          },
          "wallet": {
            "title": "wallet",
            "type": "string",
            "default": null,
            "description": "Ethereum wallet address of smart contract",
            "nullable": true
          }
        },
        "type": "object",
        "additionalProperties": false
      },
      "PublicGetVaultBalancesResponseSchema": {
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
              "$ref": "#/components/schemas/VaultBalanceResponseSchema"
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
      "VaultBalanceResponseSchema": {
        "properties": {
          "address": {
            "title": "address",
            "type": "string"
          },
          "amount": {
            "title": "amount",
            "type": "string",
            "format": "decimal"
          },
          "chain_id": {
            "title": "chain_id",
            "type": "integer"
          },
          "name": {
            "title": "name",
            "type": "string"
          },
          "vault_asset_type": {
            "title": "vault_asset_type",
            "type": "string"
          }
        },
        "required": [
          "address",
          "amount",
          "chain_id",
          "name",
          "vault_asset_type"
        ],
        "type": "object",
        "additionalProperties": false
      }
    }
  }
}
```