# [Legacy] Portfolio Margin Parameters

This page documents the up to date values of all market specific parameters for the portfolio margin manager.

# Portfolio Margin Scenarios

These are the scenarios used when computing portfolio margin.

| Scenario Number | Spot Shock (%) | Volatility Shock |
| :-------------- | :------------- | :--------------- |
| 1               | +18%           | Up               |
| 2               | +13.5%         | Up               |
| 3               | +13.5%         | Static           |
| 4               | +13.5%         | Down             |
| 5               | +9%            | Up               |
| 6               | +9%            | Static           |
| 7               | +9%            | Down             |
| 8               | +4.5%          | Up               |
| 9               | +4.5%          | Static           |
| 10              | +4.5%          | Down             |
| 11              | +0%            | Up               |
| 12              | +0%            | Static           |
| 13              | +0%            | Down             |
| 14              | -4.5%          | Up               |
| 15              | -4.5%          | Static           |
| 16              | -4.5%          | Down             |
| 17              | -9%            | Up               |
| 18              | -9%            | Static           |
| 19              | -9%            | Down             |
| 20              | -13.5%         | Up               |
| 21              | -13.5%         | Static           |
| 22              | -13.5%         | Down             |
| 23              | -18%           | Up               |

# Account Details

These parameters govern the size and supported expiries of a PMRM subaccount.

| Parameter          | Contract Variable | ETH | BTC | Range     | Description                                                                                                                                                                      |
| :----------------- | :---------------- | :-- | :-- | :-------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `MAX_ACCOUNT_SIZE` | `maxAccountSize`  | 128 | 128 | No bounds | This is the maximum number of assets (options, cash, base, perpetuals) that can be supported by a single portfolio margined subaccount. This is constrained by gas requirements. |
| `MAX_EXPIRIES`     | `maxExpiries`     | 11  | 11  | No bounds | This is the maximum number of unique expiries that can be held by a single portfolio margined subaccount.                                                                        |

# Contingency Margin

These parameters govern various pieces of contingency margin which account for possibilities not encoded in the spot and IV shocks.

| Parameter       | Contract Variable                          | ETH   | BTC   | Range        | Description                                                                                                                 |
| :-------------- | :----------------------------------------- | :---- | :---- | :----------- | :-------------------------------------------------------------------------------------------------------------------------- |
| `PEG_FACTOR`    | `OtherContingencyParameters.pegLossFactor` | 4.0   | 4.0   | \[0.0, 20.0] | Increases `IM_FACTOR `when USDC depegs beyond a threshold value.                                                            |
| `BASE_FACTOR`   | `OtherContingencyParameters.basePercent`   | 0.03  | 0.03  | \[0.0, 1.0]  | This is used to compute the base contingency, simply a small percentage (given by `BASE_FACTOR`) of the spot price.         |
| `PERP_FACTOR`   | `OtherContingencyParameters.perpPercent`   | 0.03  | 0.03  | \[0.0, 1.0]  | This is used to compute the perp contingency, simply a small percentage (given by `PERP_FACTOR`) of the spot price.         |
| `OPTION_FACTOR` | `OtherContingencyParameters.optionPercent` | 0.015 | 0.015 | \[0.0, 1.0]  | A small percentage (`OPTION_FACTOR`) of the spot price is added per net short contract per strike to the asset contingency. |

# Volatility Shocks

These parameters govern the shock IVs used when computing portfolio margin.

| Parameter               | Contract Variable                   | ETH   | BTC   | Range             | Description                                                                                                                       |
| :---------------------- | :---------------------------------- | :---- | :---- | :---------------- | :-------------------------------------------------------------------------------------------------------------------------------- |
| `VOL_RANGE (up)`        | `VolShockParameters.volRangeUp`     | 0.5   | 0.5   | \[0.01, 2.0]      | Multiplicative scaling of the implied volatility when considering an **increase** in volatility.                                  |
| `VOL_RANGE (down)`      | `VolShockParameters.volRangeDown`   | 0.275 | 0.275 | \[0.01, 1.0]      | Multiplicative scaling of the implied volatility when considering a **decrease** in volatility.                                   |
| `VEGA_POWER (< 30 DTE)` | `VolShockParameters.shortTermPower` | 0.3   | 0.3   | \[0.0, 0.5]       | A power scaling of the multiplicative volatility shock (for short dated expiries).                                                |
| `VEGA_POWER (> 30 DTE)` | `VolShockParameters.longTermPower`  | 0.13  | 0.13  | \[0.0, 0.5]       | A power scaling of the multiplicative volatility shock (for long dated expiries).                                                 |
| `DTE_FLOOR`             | `VolShockParameters.dteFloor`       | 1 day | 1 day | \[0.01, 100] days | A floor on the time-to-expiry used when computing the volatility shock. Avoids divergence arising from dividing by near 0 values. |

# Discounting

These parameters govern how long sub-portfolios are discounted.

| Parameter      | Contract Variable                     | ETH  | BTC  | Range       | Description                                                                                                |
| :------------- | :------------------------------------ | :--- | :--- | :---------- | :--------------------------------------------------------------------------------------------------------- |
| `RATE_PARAM_1` | `MarginParameters.rateMultScale`      | 1.0  | 1.0  | \[0.0, 5.0] | Multiplicative scaling of the risk free rate used when computing the discounting for a long sub-portfolio. |
| `RATE_PARAM_2` | `MarginParameters.rateAddScale`       | 0.12 | 0.12 | \[0.0, 5.0] | Additive scaling of the risk free rate used when computing the discounting for a long sub-portfolio.       |
| `STATIC_SCALE` | `MarginParameters.baseStaticDiscount` | 0.95 | 0.95 | \[0.0, 1.0] | A flat scaling of the sub-portfolio's shocked marked value (only applies if positive).                     |

# Forward Contingency

These are parameters that govern the forward contingency. This accounts for forward basis movements against the trader.

| Parameter            | Contract Variable                                | ETH                            | BTC                            | Range    | Description                                                            |
| :------------------- | :----------------------------------------------- | :----------------------------- | :----------------------------- | :------- | :--------------------------------------------------------------------- |
| `ADD_FACTOR`         | `BasisContingencyParameters.baseContAddFactor`   | 0.5                            | 0.5                            | \[0,5.0] | Additive scaling factor when computing the basis contingency           |
| `MULT_FACTOR`        | `BasisContingencyParameters.basisContMultFactor` | 2.0                            | 2.0                            | \[0,5.0] | Multiplicative scaling factor when computing the basis contingency.    |
| `UP_SCENARIO_MOVE`   | `BasisContingencyParameters.scenarioSpotUp`      | Spot up 1.045 (IV static)      | Spot up 1.045 (IV static)      | N/A      | One of the spot shock scenarios used to compute the basis contingency. |
| `DOWN_SCENARIO_MOVE` | `BasisContingencyParameters.scenarioSpotDown`    | Spot down to 0.955 (IV static) | Spot down to 0.955 (IV static) | N/A      | The other scenario used to compute the basis contingency.              |

# Initial Margin and Oracle Contingency

These govern how initial margin is typically defined, as well as circumstances where it may increase due to stable coin depeggings and/or low data confidence.

| Parameter              | Contract Variable                             | ETH  | BTC  | Range        | Description                                                                                                                          |
| :--------------------- | :-------------------------------------------- | :--- | :--- | :----------- | :----------------------------------------------------------------------------------------------------------------------------------- |
| `IM_FACTOR`            | `MarginParameters.imFactor`                   | 1.25 | 1.25 | \[1.01, 4.0] | This scales the `maxLoss` and `contingencies` when computing the initial margin for a portfolio margined subaccount.                 |
| `USDC_THRESHOLD`       | `OtherContingencyParameters.pegLossThreshold` | 0.99 | 0.99 | \[0.0, 1.05] | Value of USDC beneath which the depeg contingency comes into effect.                                                                 |
| `CONFIDENCE_THRESHOLD` | `OtherContingencyParameters.confThreshold`    | 0.55 | 0.55 | \[0.0, 1.0]  | Value of the confidence beneath which the relevant data is considered low confidence (thereby attracting additional initial margin). |
| `CONFIDENCE_SCALE`     | `OtherContingencyParameters.confMargin`       | 1.0  | 1.0  | \[0, 2.0]    | Percentage of the spot price added when considering the oracle contingency.                                                          |

# Open Interest Caps

Open interest caps on options, base and perpetual instruments.

| Parameter           | Contract Variable            | ETH      | BTC     | Range                | Description                                    |
| :------------------ | :--------------------------- | :------- | :------ | :------------------- | :--------------------------------------------- |
| `UNDERLYING_OI_CAP` | `baseAsset.totalPositionCap` | 750      | 15      | \[0, no upper bound] | Maximum open interest of the underlying asset. |
| `OPTION_OI_CAP`     | `option.totalPositionCap`    | 2000,000 | 100,000 | \[0, no upper bound] | Maximum open interest of options.              |
| `PERP_OI_CAP`       | `perp.totalPositionCap`      | 250,000  | 12,000  | \[0, no upper bound] | Maximum open interest of perpetuals.           |

# Fees

These are fees charged by the managers on the protocol layer.

| Parameter     | Contract Variable      | ETH        | BTC        | Range     | Description                                                                       |
| :------------ | :--------------------- | :--------- | :--------- | :-------- | :-------------------------------------------------------------------------------- |
| `SPOT_FACTOR` | `manager.OIFeeRateBPS` | 0.70 (70%) | 0.70 (70%) | \[0, 5.0] | Percentage of the spot price charged  when the trade increases the open interest. |
| `MIN_OI_FEE`  | `minOIFee`             | $800 USDC  | $800 USDC  | \[0, 800] | Minimum fee charged when open interest is increased.                              |