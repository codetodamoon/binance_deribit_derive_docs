# Get All Currencies

Get all active currencies with their spot price, spot price 24hrs ago.<br /><br />For real-time updates, recommend using channels -> ticker or orderbook.

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
    "/public/get_all_currencies": {
      "post": {
        "tags": [
          "Public"
        ],
        "summary": "Get All Currencies",
        "description": "Get all active currencies with their spot price, spot price 24hrs ago.<br /><br />For real-time updates, recommend using channels -> ticker or orderbook.",
        "responses": {
          "200": {
            "description": "successful operation",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/PublicGetAllCurrenciesResponseSchema"
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
                "$ref": "#/components/schemas/PublicGetAllCurrenciesParamsSchema"
              }
            }
          }
        }
      }
    }
  },
  "components": {
    "schemas": {
      "PublicGetAllCurrenciesParamsSchema": {
        "properties": {},
        "type": "object",
        "additionalProperties": false
      },
      "PublicGetAllCurrenciesResponseSchema": {
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
              "$ref": "#/components/schemas/CurrencyDetailedResponseSchema"
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
      "CurrencyDetailedResponseSchema": {
        "properties": {
          "asset_cap_and_supply_per_manager": {
            "title": "asset_cap_and_supply_per_manager",
            "type": "object",
            "description": "Current open interest and open interest cap by manager and asset type",
            "additionalProperties": {
              "title": "asset_cap_and_supply_per_manager",
              "type": "object",
              "additionalProperties": {
                "title": "asset_cap_and_supply_per_manager",
                "type": "array",
                "items": {
                  "$ref": "#/components/schemas/OpenInterestStatsSchema"
                }
              }
            }
          },
          "borrow_apy": {
            "title": "borrow_apy",
            "type": "string",
            "format": "decimal",
            "description": "Borrow APY (only for USDC)"
          },
          "currency": {
            "title": "currency",
            "type": "string",
            "description": "Underlying currency of asset (`ETH`, `BTC`, etc)"
          },
          "erc20_details": {
            "title": "erc20_details",
            "type": "object",
            "default": null,
            "description": "Details of the erc20 asset (if applicable)",
            "additionalProperties": {
              "title": "erc20_details",
              "type": "string",
              "nullable": true
            },
            "nullable": true
          },
          "instrument_types": {
            "title": "instrument_types",
            "type": "array",
            "description": "Instrument types supported for the currency",
            "items": {
              "title": "instrument_types",
              "type": "string",
              "enum": [
                "erc20",
                "option",
                "perp"
              ]
            }
          },
          "managers": {
            "title": "managers",
            "type": "array",
            "description": "Managers supported for the currency",
            "items": {
              "$ref": "#/components/schemas/ManagerContractResponseSchema"
            }
          },
          "market_type": {
            "title": "market_type",
            "type": "string",
            "enum": [
              "ALL",
              "SRM_BASE_ONLY",
              "SRM_OPTION_ONLY",
              "SRM_PERP_ONLY",
              "CASH"
            ],
            "description": "Market type of the currency"
          },
          "pm2_collateral_discounts": {
            "title": "pm2_collateral_discounts",
            "type": "array",
            "description": "Initial and Maintenance Margin discounts for given collateral in PM2",
            "items": {
              "$ref": "#/components/schemas/PM2CollateralDiscountsSchema"
            }
          },
          "protocol_asset_addresses": {
            "$ref": "#/components/schemas/ProtocolAssetAddressesSchema"
          },
          "spot_price": {
            "title": "spot_price",
            "type": "string",
            "format": "decimal",
            "description": "Spot price of the currency"
          },
          "spot_price_24h": {
            "title": "spot_price_24h",
            "type": "string",
            "format": "decimal",
            "default": null,
            "description": "Spot price of the currency 24 hours ago",
            "nullable": true
          },
          "srm_im_discount": {
            "title": "srm_im_discount",
            "type": "string",
            "format": "decimal",
            "description": "Initial Margin discount for given collateral in Standard Manager (e.g. LTV). Only the Standard Manager supports non-USDC collateral"
          },
          "srm_mm_discount": {
            "title": "srm_mm_discount",
            "type": "string",
            "format": "decimal",
            "description": "Maintenance Margin discount for given collateral in Standard Manager (e.g. liquidation threshold). Only the Standard Manager supports non-USDC collateral"
          },
          "supply_apy": {
            "title": "supply_apy",
            "type": "string",
            "format": "decimal",
            "description": "Supply APY (only for USDC)"
          },
          "total_borrow": {
            "title": "total_borrow",
            "type": "string",
            "format": "decimal",
            "description": "Total collateral borrowed in the protocol (only USDC is borrowable)"
          },
          "total_supply": {
            "title": "total_supply",
            "type": "string",
            "format": "decimal",
            "description": "Total collateral supplied in the protocol"
          }
        },
        "required": [
          "asset_cap_and_supply_per_manager",
          "borrow_apy",
          "currency",
          "instrument_types",
          "managers",
          "market_type",
          "pm2_collateral_discounts",
          "protocol_asset_addresses",
          "spot_price",
          "srm_im_discount",
          "srm_mm_discount",
          "supply_apy",
          "total_borrow",
          "total_supply"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "OpenInterestStatsSchema": {
        "properties": {
          "current_open_interest": {
            "title": "current_open_interest",
            "type": "string",
            "format": "decimal",
            "description": "Current open interest for the margin type"
          },
          "interest_cap": {
            "title": "interest_cap",
            "type": "string",
            "format": "decimal",
            "description": "Total open interest cap"
          },
          "manager_currency": {
            "title": "manager_currency",
            "type": "string",
            "default": null,
            "description": "Currency of the manager (only applies to Portfolio Margin)",
            "nullable": true
          }
        },
        "required": [
          "current_open_interest",
          "interest_cap"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "ManagerContractResponseSchema": {
        "properties": {
          "address": {
            "title": "address",
            "type": "string",
            "description": "Address of the manager"
          },
          "currency": {
            "title": "currency",
            "type": "string",
            "default": null,
            "description": "Currency of the manager (only applies to portfolio managers)",
            "nullable": true
          },
          "margin_type": {
            "title": "margin_type",
            "type": "string",
            "enum": [
              "PM",
              "SM",
              "PM2"
            ],
            "description": "Margin type of the manager"
          }
        },
        "required": [
          "address",
          "margin_type"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "PM2CollateralDiscountsSchema": {
        "properties": {
          "im_discount": {
            "title": "im_discount",
            "type": "string",
            "format": "decimal",
            "description": "Initial Margin discount for given collateral in PM2"
          },
          "manager_currency": {
            "title": "manager_currency",
            "type": "string",
            "description": "Currency of the manager"
          },
          "mm_discount": {
            "title": "mm_discount",
            "type": "string",
            "format": "decimal",
            "description": "Maintenance Margin discount for given collateral in PM2"
          }
        },
        "required": [
          "im_discount",
          "manager_currency",
          "mm_discount"
        ],
        "type": "object",
        "additionalProperties": false
      },
      "ProtocolAssetAddressesSchema": {
        "properties": {
          "option": {
            "title": "option",
            "type": "string",
            "default": null,
            "description": "Address of the Derive protocol option contract (none if not supported)",
            "nullable": true
          },
          "perp": {
            "title": "perp",
            "type": "string",
            "default": null,
            "description": "Address of the Derive protocol perp contract (none if not supported)",
            "nullable": true
          },
          "spot": {
            "title": "spot",
            "type": "string",
            "default": null,
            "description": "Address of the Derive protocol spot contract (none if not supported)",
            "nullable": true
          },
          "underlying_erc20": {
            "title": "underlying_erc20",
            "type": "string",
            "default": null,
            "description": "Address of the erc20 asset on Derive chain. This is the asset that is deposited into the spot asset",
            "nullable": true
          }
        },
        "type": "object",
        "additionalProperties": false
      }
    }
  }
}
```