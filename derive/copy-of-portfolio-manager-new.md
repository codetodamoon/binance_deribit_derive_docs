# Portfolio Manager [New] [SOL/HYPE added]

This page documents the up to date values of all market specific parameters for the portfolio margin manager.

# Portfolio Margin Scenarios

These are the scenarios used when computing portfolio margin. Note all regular scenarios have dampening 1.

All combinations in the table below are of the form `(Spot Shock (%), Volatility Shock)`.

| Scenario | ETH/BTC          | SOL               | HYPE              |
| :------- | :--------------- | :---------------- | :---------------- |
| 1        | (+18%, Up)       | (+27%, Up)        | (+33%, Up)        |
| 2        | (+13.5%, Up)     | (+20.25%, Up)     | (+24.75%, Up)     |
| 3        | (+13.5%, Static) | (+20.25%, Static) | (+24.75%, Static) |
| 4        | (+13.5%, Down)   | (+20.25%, Down)   | (+24.75%, Down)   |
| 5        | (+9%, Up)        | (+13.5%, Up)      | (+16.5%, Up)      |
| 6        | (+9%, Static)    | (+13.5%, Static)  | (+16.5%, Static)  |
| 7        | (+9%, Down)      | (+13.5%, Down)    | (+16.5%, Down)    |
| 8        | (+4.5%, Up)      | (+6.75%, Up)      | (+8.25%, Up)      |
| 9        | (+4.5%, Static)  | (+6.75%, Static)  | (+8.25%, Static)  |
| 10       | (+4.5%, Down)    | (+6.75%, Down)    | (+8.25%, Down)    |
| 11       | (+0%, Up)        | (+0%, Up)         | (+0%, Up)         |
| 12       | (+0%, Static)    | (+0%, Static)     | (+0%, Static)     |
| 13       | (+0%, Down)      | (+0%, Down)       | (+0%, Down)       |
| 14       | (-4.5%, Up)      | (-6.75%, Up)      | (-8.25%, Up)      |
| 15       | (-4.5%, Static)  | (-6.75%, Static)  | (-8.25%, Static)  |
| 16       | (-4.5%, down)    | (-6.75%, Down)    | (-8.25%, Down)    |
| 17       | (-9%, Up)        | (-13.5%, Up)      | (-16.5%, Up)      |
| 18       | (-9%, Static)    | (-13.5%, Static)  | (-16.5%, Static)  |
| 19       | (-9%, Down)      | (-13.5%, Down)    | (-16.5%, Down)    |
| 20       | (-13.5%, Up)     | (-20.25%, Up)     | (-24.75%, Up)     |
| 21       | (-13.5%, Static) | (-20.25%, Static) | (-24.75%, Static) |
| 22       | (-13.5%, Down)   | (-20.25%, Down)   | (-24.75%, Down)   |
| 23       | (-18%, Up)       | (-27%, Up)        | (-33%, Up)        |

# Tail Scenarios

These are the tail scenarios which involve large spot shocks and non trivial dampening. All cases involve volatility spot shock up.

Pairs in the table below are of the form `(spot shock (%), dampening factor)`.

| Scenario | ETH/BTC        | SOL            | HYPE                               |
| :------- | :------------- | :------------- | :--------------------------------- |
| 1        | (-66%, 0.21)   | (-66%, 0.36)   | (-66%, 0.49)                       |
| 2        | (-33%, 0.42)   | (-33%, 0.72)   | N/A (captured in vanilla scenario) |
| 3        | (+50%, 0.27)   | (+50%, 0.48)   | (+50%, 0.66)                       |
| 4        | (+100%, 0.13)  | (+100%, 0.24)  | (+100%, 0.33)                      |
| 5        | (+200%, 0.069) | (+200%, 0.12)  | (+200%, 0.165)                     |
| 6        | (+300%, 0.046) | (+300%, 0.08)  | (+300%, 0.11)                      |
| 7        | (+400%, 0.034) | (+400%, 0.06)  | (+400%, 0.0825)                    |
| 8        | (+500%, 0.027) | (+500%, 0.048) | (+500%, 0.066)                     |

# Account Details

These parameters govern the size and supported expiries of a PMRM subaccount.

| Parameter          | Contract Variable | ETH/BTC/SOL/HYPE | Range     | Description                                                                                                                                                                      |
| :----------------- | :---------------- | :--------------- | :-------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `MAX_ACCOUNT_SIZE` | `maxAccountSize`  | 128              | No bounds | This is the maximum number of assets (options, cash, base, perpetuals) that can be supported by a single portfolio margined subaccount. This is constrained by gas requirements. |
| `MAX_EXPIRIES`     | `maxExpiries`     | 11               | No bounds | This is the maximum number of unique expiries that can be held by a single portfolio margined subaccount.                                                                        |

# Contingency Margin

These parameters govern various pieces of contingency margin which account for possibilities not encoded in the spot and IV shocks.

| Parameter                 | Contract Variable                          | ETH/BTC | SOL   | HYPE   | Range        | Description                                                                                                                            |
| :------------------------ | :----------------------------------------- | :------ | :---- | :----- | :----------- | :------------------------------------------------------------------------------------------------------------------------------------- |
| `PEG_FACTOR`              | `OtherContingencyParameters.pegLossFactor` | 4.0     | 4.0   | 4.0    | \[0.0, 20.0] | Increases `IM_FACTOR `when USDC depegs beyond a threshold value.                                                                       |
| `INITIAL_PERP_FACTOR`     | `OtherContingencyParameters.perpPercent`   | 0.04    | 0.06  | 0.10   | \[0.0, 1.0]  | This is used to compute the perp contingency for initial margin, simply a small percentage (given by `PERP_FACTOR`) of the spot price. |
| `MAINTENANCE_PERP_FACTOR` | `OtherContingencyParameters.perpPercent`   | 0.03    | 0.05  | 0.075  | \[0.0, 1.0]  | As above but for maintenance margin                                                                                                    |
| `MM_OPTION_FACTOR`        | `OtherContingencyParameters.optionPercent` | 0.003   | 0.015 | 0.0175 | \[0.0, 1.0]  | A small percentage (`OPTION_FACTOR`) of the spot price is added per net short contract per strike to the asset contingency.            |
| `IM_OPTION_FACTOR`        | `OtherContingencyParameters.optionPercent` | 0.002   | 0.02  | 0.025  | \[0.0, 1.0]  | As above but for initial margin. I.e. for SOL, an extra 2% of spot is added per naked short option.                                    |

# Volatility Shocks

These parameters govern the shock IVs used when computing portfolio margin.

| Parameter               | Contract Variable                   | ETH/BTC    | SOL        | HYPE       | Range             | Description                                                                                                                       |
| :---------------------- | :---------------------------------- | :--------- | :--------- | :--------- | :---------------- | :-------------------------------------------------------------------------------------------------------------------------------- |
| `VOL_RANGE (up)`        | `VolShockParameters.volRangeUp`     | 0.5        | 0.60       | 0.65       | \[0.01, 2.0]      | Multiplicative scaling of the implied volatility when considering an **increase** in volatility.                                  |
| `VOL_RANGE (down)`      | `VolShockParameters.volRangeDown`   | 0.275      | 0.30       | 0.30       | \[0.01, 1.0]      | Multiplicative scaling of the implied volatility when considering a **decrease** in volatility.                                   |
| `VEGA_POWER (< 30 DTE)` | `VolShockParameters.shortTermPower` | 0.3        | 0.3        | 0.3        | \[0.0, 0.5]       | A power scaling of the multiplicative volatility shock (for short dated expiries).                                                |
| `VEGA_POWER (> 30 DTE)` | `VolShockParameters.longTermPower`  | 0.13       | 0.13       | 0.13       | \[0.0, 0.5]       | A power scaling of the multiplicative volatility shock (for long dated expiries).                                                 |
| `DTE_FLOOR`             | `VolShockParameters.dteFloor`       | 1 day      | 1 day      | 1 day      | \[0.01, 100] days | A floor on the time-to-expiry used when computing the volatility shock. Avoids divergence arising from dividing by near 0 values. |
| `minVolShockUp`         | VolShockParameters.minVolShockUp    | 0.40 (40%) | 0.60 (60%) | 0.60 (60%) | \[0, 20]          | Minimum evaluated (shock) volatility for the vol up scenario                                                                      |

# Discounting

These parameters govern how long sub-portfolios are discounted.

| Parameter            | Contract Variable                        | ETH/BTC/SOL/HYPE | Range        | Description                                                                                                 |
| :------------------- | :--------------------------------------- | :--------------- | :----------- | :---------------------------------------------------------------------------------------------------------- |
| `shortRateMultScale` | `MarginParameters.shortRateMultScale`    | 0.0              | \[0.0, 10.0] | Multiplicative scaling of the risk free rate used when computing the discounting for a short sub-portfolio. |
| `shortRateAddScale`  | `MarginParameters.shortRateAddScale`     | 0.10             | \[0.0, 10.0] | Additive scaling of the risk free rate used when computing the discounting for a short sub-portfolio.       |
| `longRateMultScale`  | `MarginParameters.longRateMultScale`     | 0.0              | \[0.0, 10.0] | Multiplicative scaling of the risk free rate used when computing the discounting for a long sub-portfolio.  |
| `longRateAddScale`   | `MarginParameters.longRateAddScale`      | 0.10             | \[0.0, 10.0] | Additive scaling of the risk free rate used when computing the discounting for a long sub-portfolio.        |
| `STATIC_SCALE_POS`   | `MarginParameters.baseStaticDiscountPos` | 0.98             | \[0.0, 1.1]  | A flat scaling of the sub-portfolio's shocked marked value (only applies if positive).                      |
| `STATIC_SCALE_NEG`   | `MarginParameters.baseStaticDiscountNeg` | 1.02             | \[0.9, 10.0] | A flat scaling of the sub-portfolio's shocked marked value (only applies if negative).                      |

# Forward Contingency

These are parameters that govern the forward contingency. This accounts for forward basis movements against the trader.

| Parameter            | Contract Variable                                | ETH/BTC/SOL/HYPE                                                             | Range    | Description                                                            |
| :------------------- | :----------------------------------------------- | :--------------------------------------------------------------------------- | :------- | :--------------------------------------------------------------------- |
| `ADD_FACTOR`         | `BasisContingencyParameters.baseContAddFactor`   | 0.5                                                                          | \[0,5.0] | Additive scaling factor when computing the basis contingency           |
| `MULT_FACTOR`        | `BasisContingencyParameters.basisContMultFactor` | 2.0                                                                          | \[0,5.0] | Multiplicative scaling factor when computing the basis contingency.    |
| `UP_SCENARIO_MOVE`   | `BasisContingencyParameters.scenarioSpotUp`      | Smallest magnitude spot up scenario with IV static (i.e. 1.045 for BTC/ETH)  | N/A      | One of the spot shock scenarios used to compute the basis contingency. |
| `DOWN_SCENARIO_MOVE` | `BasisContingencyParameters.scenarioSpotDown`    | Largest magnitude spot down scenario with IV static (i.e. 0.955 for BTC/ETH) | N/A      | The other scenario used to compute the basis contingency.              |

# Initial Margin and Oracle Contingency

These govern how initial margin is typically defined, as well as circumstances where it may increase due to stable coin depeggings and/or low data confidence.

| Parameter              | Contract Variable                             | ETH/BTC | SOL  | HYPE | Range        | Description                                                                                                                          |
| :--------------------- | :-------------------------------------------- | :------ | :--- | :--- | :----------- | :----------------------------------------------------------------------------------------------------------------------------------- |
| `IM_FACTOR`            | `MarginParameters.imFactor`                   | 1.0     | 1.1  | 1.15 | \[0.5, 10.0] | This scales the `maxLoss` and `contingencies` when computing the initial margin for a portfolio margined subaccount.                 |
| `MM_FACTOR`            | `MarginParameters.mmFactor`                   | 0.80    | 0.90 | 0.95 | \[0.5, 10.0] | As above, but for maintenance margin.                                                                                                |
| `USDC_THRESHOLD`       | `OtherContingencyParameters.pegLossThreshold` | 0.99    | 0.99 | 0.99 | \[0.0, 1.05] | Value of USDC beneath which the depeg contingency comes into effect.                                                                 |
| `CONFIDENCE_THRESHOLD` | `OtherContingencyParameters.confThreshold`    | 0.55    | 0.55 | 0.55 | \[0.0, 1.0]  | Value of the confidence beneath which the relevant data is considered low confidence (thereby attracting additional initial margin). |
| `CONFIDENCE_SCALE`     | `OtherContingencyParameters.confMargin`       | 1.0     | 1.0  | 1.0  | \[0, 2.0]    | Percentage of the spot price added when considering the oracle contingency.                                                          |

# Skew Shock Parameters

| Parameter            | ETH/BTC/SOL/HYPE | Range      | Description                                                                    |
| :------------------- | :--------------- | :--------- | :----------------------------------------------------------------------------- |
| `linearBaseCap`      | 0.25             | \<= 10     | Sets the maximum multiple of the volatility for the linear scenario            |
| `absBaseCap`         | 0.25             | \<= 10     | As above for the abs scenario                                                  |
| `linearCBase`        | -0.1             | > = -10    | Sets how much to lower the maximum multiple for longer dated expiries          |
| `absCBase`           | -0.1             | > = -10    | As above for the abs scenario                                                  |
| `minKStar`           | 0.01             | > = 0      | Minimum width before flattening the vol increase for the skew scenarios        |
| `widthScale`         | 4.0              | \<= 10     | How many std deviations based on ATM vol after which we cap the increase in IV |
| `volParameterStatic` | 0.60             | \[0, 10]   | Estimate of IV used to approximate Kstar                                       |
| `volParameterScale`  | 0.0              | \[-20, 20] | Corrects vol for longer timescales                                             |

# Risk Cancellation

**Risk Cancelling Collateral for the ETH PM:**

* (w)ETH
* wstETH
* weETH
* rswETH
* rsETH

**Risk Cancelling Collateral for BTC PM:**

* (w)BTC
* LBTC
* cbBTC
* eBTC
* solvBTC
* xSolvBTC

**Risk Cancelling Collateral for SOL PM:**

* wSOL (currently not supported)

**Risk Cancelling Collateral for HYPE PM:**

* HYPE

| **Asset**    | **ETH MM Haircut**  | **ETH IM Haircut**  | **BTC MM Haircut**  | **BTC IM Haircut**  |
| ------------ | ------------------- | ------------------- | ------------------- | ------------------- |
| **ETH**      | 5.6%                | 7%                  | Same as std manager | Same as std manager |
| **wstETH**   | 5.6%                | 10%                 | Same as std manager | Same as std manager |
| **weETH**    | 15.6%               | 17%                 | Same as std manager | Same as std manager |
| **rswETH**   | 30.6%               | 32%                 | Same as std manager | Same as std manager |
| **rsETH**    | 20.6%               | 22%                 | Same as std manager | Same as std manager |
| **wBTC**     | Same as std manager | Same as std manager | 10.6%               | 12%                 |
| **LBTC**     | Same as std manager | Same as std manager | 15.6%               | 17%                 |
| **cbBTC**    | Same as std manager | Same as std manager | 15.6%               | 17%                 |
| **eBTC**     | Same as std manager | Same as std manager | 15.6%               | 17%                 |
| **solvBTC**  | Same as std manager | Same as std manager | 20.6%               | 22%                 |
| **xsolvBTC** | Same as std manager | Same as std manager | 20.6%               | 22%                 |

All other assets (e.g. sUSDe, OP) have the same haircuts as in the standard manager.

For the SOL and HYPE managers, all haircuts are the same as the standard manager. For HYPE collateral in the HYPE PM, this attracts a 10% haircut for MM and 20% haircut for IM.

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