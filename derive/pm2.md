# PM2

These notes describe the new portfolio margin engine (referred to herein as PM2). For the legacy portfolio margin engine, see [PM1](https://dash.readme.com/project/lyra-api/v2.1-Draft/docs/portfolio-margin-1). Note the legacy portfolio margin engine (PM1) will continue to be supported on both the exchange and contract layers.

# Introduction

A user's margin requirement under PM2 is computed as follows:

```python Formula
Margin = MtM + im_factor * maxLoss + sum(Contingencies)
```

where:

* `MtM` is the mark value of all assets and positions (including unrealized funding and associated profit & loss (PNL)).
* `im_factor` is a constant that is `0.8` (`1.0`) when computing maintenance (initial) margin. As we explain below, this increases (only for initial margin purposes) if the value of the cash asset depegs from $1.
* `maxLoss` is the maximum realistic loss the portfolio can sustain under a variety of expected and "Black Swan" scenarios consisting of a variety of spot, volatility and skew shocks. These are discussed in full detail in the sections below.
* `Contingencies` consist of various extra margin additions for base collateral, perp positions and naked short options that are necessary to ensure sufficient collateral is present in an account. These are discussed in detail below.

Compared to standard margin, this methodology can substantially increase a portfolio’s capital efficiency.

We note that PM2 accounts do not support cross-market margin, meaning each portfolio margin account can only support derivates corresponding to one base asset which we call the **denominated base asset**.

For example, an ETH portfolio margin account can only open ETH options and perpetuals. BTC options and perpetuals cannot be supported in an ETH PMRM account. However, an ETH portfolio margin account can support all other forms of (non risk cancelling) base assets. I.e. BTC can be collateral in an ETH portfolio margined account.

If a portfolio margin account's collateral falls below its maintenance margin requirement, the account will be liquidated. See [Liquidations](https://docs.derive.xyz/reference/liquidations) for more details.

> 📘 PM2 does not support cross-market margin, meaning unlike standard margin, only ETH derivatives can be held in an ETH PM2 account.

# \[New!] Collateral and Risk Cancellation

PM2 managers have two big upgrades over the PM1 managers in regards to collateral:

1. All forms of collateral will be supported in the PM2 risk manager (as in the standard manager). This means traders can use, e.g. USDT as collateral in the PMRM or ETH in a BTC PMRM.
2. For each PMRM, some assets will be “risk cancelling” and be included in the portfolio margin scenario calculations. This means short ETH call deltas can be offset by, say, weETH in the ETH PMRM. BTC collateral can also be used as collateral, though it will not cancel risk.

# \[New!] Margin Calculation

A user's initial and maintenance margin requirements are calculated as follows:

```python Formula
Initial Margin = Portfolio MtM +  
    + (im_factor + depeg_factor) 
    * maxLoss + Contingencies + Oracle Contingency

Maintenance Margin = Portfolio MtM +
    + maxLoss + Contingencies
```

Where:

```python Formula
Contingencies = Collateral Contingencies + Perp Contingency + Option Contingency

maxLoss = min(Regular Scenario PNL, Tail Scenario PNL, Skew Scenario PNL, Forward PNL)
```

and

* `Portfolio MtM` is the mark-to-market value of the portfolio (includes all perpetual funding/PNL). This includes the account's USDC balance and all base assets (marked to their current value). Note options are marked with a discounting of 1.0.
* `depeg_factor` is extra initial margin conditionally required to protect against USDC depegging.
* `maxLoss` is the greatest PNL loss that can occur under a variety of scenarios, namely "regular scenarios", "tail scenarios", "skew scenarios" and "forward scenarios" (referred to as the "forward contingency").
* `Regular Loss` is the maximum PNL loss the portfolio will endure under 23 scenarios comprised of various forward and volatility shocks.
* `Tail Loss` is similarly the worst PNL the portfolio will endure under a number of "tail scenario" moves involving substantial price shocks (and volatility increase). These PNLs will be dampened to ensure users are not over-margined.
* `Skew Loss` is the maximum loss resulting from a change in the skew of the volatility surface. This can involve the skew rotating about the at-the-money or contracting/flattening.
* `Forward Contingency` accounts for the forward basis for each expiry moving unfavourably against the trader.
* `Collateral Contingency` is extra margin applied to each collateral asset held in the account (i.e. a risk based haircut).
* `Perp Contingency` is extra margin added for each perp asset held by the account
* `Option Contingency` is extra margin added for each naked short option held by the account.
* `Oracle Contingency` is extra initial margin conditionally required to protect against inaccurate oracle data feeds.

> 📘 Oracle Contingencies are typically zero and only add to initial margin requirements, i.e. they do not change margin requirements for already open positions, but may block opening new positions.

As described in [Standard Margin](https://docs.derive.xyz/docs/standard-margin), an account is subject to liquidation if `Maintenance Margin` falls beneath 0 and can only open new positions if the final state has `Initial Margin` above 0.

# \[New!] Option Mark Values

To ensure that long dated options are accurately marked, all options with in the PM2 manager will be marked using a dynamic discounting rate. In PM1, options in both managers were marked to 0 discounting, leading to larger margin requirements for long dated options.

In particular, the price of a call will be given by

```python Formula
Call Price = exp(-r_T * T) * [F * N(d1) -  K * N(d2)]
d1 = (ln(F/K) + (sigma **2)/2 * T) / (sigma * sqrt(T))
d2 = d1 - sigma * sqrt(T)
```

where

* `r_T` is the discounting rate for the expiry `T`. Note that in PM1 (and the current standard manager) this was always set to 0, now in PM2 this will be a dynamic rate (typically marked to US T-bills).
* `T` is the time to expiry in years
* `F` is the forward price
* `K` is the strike
* `N()` is the cumulative normal distribution function

# Regular Loss

To compute `Regular Loss`, the portfolio’s options, perpetual and base assets are evaluated under 23 forward and volatility shocks. The largest loss is set as `Regular Loss` and potentially used to define the account's margin requirement.

The scenarios are comprised of all spot shocks from -18% to +18%, increasing in steps of 4.5%. Up/down/static vol shocks for all scenarios between (and inclusive of) -13.5% and +13.5%. Only volatility increasing scenarios are used for +18% and -18%. See [PM2 Parameters](https://dash.readme.com/project/lyra-api/v2.1-Draft/docs/portfolio-margin-parameters-copy) for an explicit table.

The magnitude of these shocks is explained below.

## \[New!] Shocked Risk Cancelling Asset Value

The account’s base collateral, as well as all risk cancelling collateral is shocked by a constant factor under scenario `k`:

```python Formula
Shocked Risk Cancelling Valueₖ = Σ Collateral * (1 + Spot Shockₖ) * collateral_spot
```

Where:

* `Collateral` is the balance of the risk cancelling asset (e.g. wBTC, LBTC in the BTC manager)
* `Spot Shockₖ` is the shock to the spot price for scenario `k`.
* `collateral_spot` is the spot price of the underlying risk cancelling asset.
* The sum is taken over all risk cancelling assets.

## Shocked Perpetual Value

The account’s perpetual position value is shocked by a constant factor under scenario `k`:

```python Formula
Shocked Perp Valueₖ = Perp Position * (1 + Spot Shockₖ) * Perp Price
```

Where:

* `Perp Position` is the number of perpetual contracts (this number is negative for shorts).
* `Spot Shockₖ` is the shock to the spot price for scenario `k`.
* `Perp Price` is the mark-to-market value of the perpetual.

## Shocked Option Value

The account’s option positions are grouped and shocked per expiry. Each expiry `i` is evaluated for a given shock scenario `k`. The shocked value for an expiry `i` is the value of each option position with strike price `j` calculated with shocked forward and volatility values.

The forward is shocked to:

```python Formula
Shocked Forwardᵢₖ = Forwardᵢ * (1 + Spot Shockₖ)
```

Where:

* `Forwardᵢ` is the forward price of expiry `i`.
* `Spot Shockₖ` is the shock multiplier for scenario `k`.

The IV shock is multiplicative. The magnitude of the shock depends on time to expiry and whether or not the shock is positive or negative. We have:

```python Formula
Shocked IVⱼₖ = IV Shockₖ * IVⱼ

IV Shockₖ = 1 + VOL_RANGE * ((30 / 365) / max(1 / 365, Time To Expiryᵢ))**VEGA_POWER
```

Where:

* `IV` is the implied volatility of strike `j`.
* `Time To Expiry` is the number of years until expiry.
* `VOL_RANGE = -0.275` if IV is being shocked down.
* `VOL_RANGE = 0.5` if IV is being shocked up.
* `VEGA_POWER = 0.3` if time to expiry \< 30 days.
* `VEGA_POWER = 0.13` if time to expiry > 30 days.

## \[New!] Discounting

As marks were not discounted in 2.0, only sub-portfolios with a net positive value were discounted. Now, to ensure accurate margining and marking, net positive sub-portfolios will continue to be discounted but net short portfolios will be anti discounted (marked up).

For an expiry `i`, suppose the rate is given by `r_i` and time to expiry (in years) given by `tau_i`. The mark discounting will be given by

```python Formula
mark_discounting = exp(-r_i * tau_i)
```

The discounting for the subportfolio of expiry `i` will then be given by

```python Formula
Discountᵢ =

Expiry Value > 0: STATIC_SCALE * exp(-(r_i * RATE_PARAM_POS_1 + RATE_PARAM_POS_2) * tau_i)
                       

Expiry Value < 0: min(1/mark_discounting, STATIC_SCALE_NEG / exp(-(r_i * RATE_PARAM_NEG_1 + RATE_PARAM_NEG_2) * tau_i))
```

Where

* `RATE_PARAM_POS_1 = 0.0`.
* `RATE_PARAM_POS_2 = 0.10`.
* `STATIC_SCALE = 0.98`
* `STATIC_SCALE_NEG = 1.02`
* `RATE_PARAM_NEG_1 = 0.0`.
* `RATE_PARAM_NEG_2 = 0.10`.

Finally, an expiry group’s shocked value is calculated by summing the shocked value of each option for an expiry using [Black76](https://en.wikipedia.org/wiki/Black_model), with the applied discount for the given expiry:

```python Formula
Shocked Expiry Value_i_k = Discount_i x Σ Shocked Option Value_i_j_k
```

where the sum is taken over all strikes/options `j` for the given expiry `i` and scenario `k`.

## Scenario Analysis

The portfolio’s loss under scenario `k` is then the sum of perpetual, (risk cancelling) collateral and each option expiry’s value minus its shocked value:

```python Formula
Portfolio Value =  Risk Cancelling Collateral Value + Perp Value + Σ Expiry Valueᵢ

Portfolio Shocked Valueₖ = Shocked Risk Cancelling Valueₖ + Shocked Perp Valueₖ + Σ Shocked Expiry Valueᵢₖ

Portfolio Lossₖ = Portfolio Shocked Valueₖ - Portfolio Value
```

Where:

* `Risk Cancelling Value` is the mark value of the account's risk cancelling collateral.
* `Perp Value` is the mark value of the account's perpetual position.
* `Expiry Valueᵢ` is the mark-to-market value of the account's option positions with expiry `i`.
* `Shocked Base Valueₖ` is the shocked value of the account's base balance for scenario `k`
* `Shocked Perp Valueₖ` is the shocked value of the account's perpetual position for scenario `k`
* `Shocked Expiry Valueᵢₖ` is the shocked mark-to-market value of the account's option positions with expiry `i` under shock scenario `k`

`Regular Loss` is then the smallest (i.e. most negative) portfolio loss under all 23 scenarios:

```tsx python
Regular Loss = min(Portfolio Loss(1), Portfolio Loss(2), ..., Portfolio Loss(23))
```

# \[New!] Tail Loss

To ensure sufficient collateral is posted for very out-of-the-money options, we also consider various "tail scenarios". These involve substantial spot shocks up and down and always occur with a volatility increase as described in the previous section.

To prevent over margining of positions, a `dampening_factor` (specific to each tail scenario) is applied to the PNL for each tail scenario. Note that a `dampening_factor` also applies to each regular scenario (shown above), though these will always be set to 1, as well as to each skew scenario, described below.

The methodology for computing the tail loss is the same as computing `Regular Loss` described above, so we have

```tsx python
Tail Loss = min(dampening_1 * Tail Loss(1), dampening_2 * Tail Loss(2), ..., dampening_n * Tail Loss(n))
```

where there are `n` tail loss scenarios. See the parameters section for more detail on the specific scenarios.

# \[New!] Skew Loss

In addition to the tail scenarios described above, we need to add ***two*** extra “skew scenarios” to account for vega neutral portfolios in the event of a surface's skew being mispriced.

This was originally addressed by the PM1 `optionContingency` but - to ensure adequate margining - led to poor capital efficiency for bounded loss structures like spreads and flies. The `optionContingency` will remain in the upgraded PMRM but with substantially lowered parameters.

## Motivation

Suppose we have an SVI surface which, when plotted in log moneyness space (`k=log[K/F]`), has a minimum at `k=m`. Note we also enforce a flattening of the SVI surface beyond certain values of `k`. These are given by `kmin` and `kmax` (see the Oracles section for more detail).

The volatility surface has a skew - for instance, vols with `k > m` are higher than those with `k < m`. Visually, the skew is a measure of how “tilted” or asymmetric the SVI curve is about `k=m.`

Skew can change - the surface may become more tilted and can “rotate” - the OTM vols might increase while the ITMs might decrease (or vice versa). The surface can also become “tighter” or "flatter" - both the OTM and ITM vols might increase (or decrease).

Usually, skew will not be a significant risk for a portfolio as we have very large volatility shocks to address this. However, it is still a case to consider to ensure adequate margin being posted by traders.

## Implementation

There are two skew scenarios to consider:

* `linearScenario`: This is the first surface described above (one side becomes steeper, the other side less so). This translates to multiplying the volatility in this scenario by a linear function of `k`.
* `absScenario`: This is the other scenario where the surface is “tightened”. This equates to multiplying by an absolute value function of `k`.

To compute the `skewScenarioPNL` (for either scenario above), we consider a given expiry `T_j`.

For each strike `K_i` in the portfolio with expiry `T_j`, evaluate shock skew vol

`ivSkewShock_(i,j) = IV(i,j) x (1+skewShock(i,j))`

where `IV(i,j)` is the IV of the option in question (computed using the usual SVI) and `skewShock(i,j)` is a multiplicative factor that we now describe how to find.

### Linear Scenario

We have

```python Formula
linear_mult_cap_j = linear_base_cap + linear_base_scale * sqrt(tau_j)
```

where `tau_j` is the time to expiry in years for expiry `j` and

* `linear_base_cap = 0.25` and
* `linear_base_scale = -0.1`.

In other words, `linear_mult_cap` caps how much the implied volatility will be scaled under this scenario.

We also define `k_star` which sets the bounds after which the skew is capped. Here we expect to have `min_k_star=0.01` and `width_scale=4`.

```python Formula
k_star = max(min_k_star, width_scale * sqrt(tau_j) x sig_est
```

where `sig_est` is given by

```python Formula
sig_est = vol_param_add + vol_param_scale * sqrt(tau_j)
```

and

* `vol_param_add = 0.6`
* `vol_param_scale = 0.0`

The `skew_multiplier` is then defined as

```python Formula
skew_multiplier = min(mult_cap * k/k_star, mult_cap) if k > 0
skew_multiplier = max(mult_cap * k/k_star, -mult_cap) if k < 0 
```

### Abs Scenario

As above, we have

```python Formula
abs_mult_cap_j = abs_base_cap + abs_base_scale * sqrt(tau_j)
```

with `k_star` and `sig_est` defined as above (and same parameters).

The abs scenario skew multiplier is then defined as

```python Formula
skew_multiplier = min(mult_cap * k/k_star, mult_cap) if k > 0
skew_multiplier = max(mult_cap * k/k_star, -mult_cap) if k < 0 
```

### Computing the Loss

For each of the `linear` and `abs` scenarios, we then find the shocked PNL of the subportfolio under this shock.

`expiryPNL(j) = [Discounting(j) * Σ BS(ivSkewShock(i,j)] -  Σ BS(IV(i,j))`

where

* `Discounting(j)` is the discounting logic (see above) for a given subexpiry.

We thus have

`skewPNL(scenario) = dampening(scenario) * Σ -|expiryPNL(j)|` where `dampening` is a unique factor (similar to the tail scenarios) for each scenario.

We then have `Skew Loss = min(linearSkewPNL, absSkewPNL)`

# Forward Loss (Contingency)

The forward contingency accounts for the forward basis for an account’s options moving unfavourably against the trader. This is unchanged from PM1 (though we refer to it as the Forward Loss for consistency).

We consider an up and down scenario where spot goes up or down 4.5%. For each expiry `i`, we calculate the basis loss, or the max loss of the up and down scenario for each expiry’s option positions:

```python Formula
Basis Lossᵢ = min(
    min(0, Up Expiry Valueᵢ - Expiry Valueᵢ),
    min(0, Down Expiry Valueᵢ - Expiry Valueᵢ)
)
```

Where:

* `Expiry Valueᵢ` is the mark-to-market value of positions in expiry `i`.
* `Up Expiry Valueᵢ` is the mark-to-market value of positions in expiry `i` under the up scenario (4.5% increase in spot).
* `Down Expiry Valueᵢ` is the mark-to-market value of positions in expiry `i` under the down scenario (4.5% decrease in spot).

The forward contingency is then the sum of each expiry’s basis loss:

```python Formula
Forward Contingency = Σ (ADD_FACTOR + MULT_FACTOR * Time To Expiryᵢ) * Basis Lossᵢ
```

Where:

* `ADD_FACTOR = 0.5`
* `MULT_FACTOR = 2.0`.
* `Time To Expiryᵢ` is the number of years until expiry.

# Contingencies

The contingencies are typically small extra margin requirements to ensure adequate collateral is posted under all conditions for a given account.

## \[New!] Collateral Contingency

All collateral assets - both risk cancelling and otherwise - have small risk based haircuts that lead to small increases in the user's margin requirement. In particular, we have

```python Formula
Collateral Contingency = - Σ n(collateral) * HAIRCUT * Spot
```

Where:

* `n(collateral)` is the number of a given collateral asset in the account
* `Spot` is the spot price of the collateral asset
* `HAIRCUT` is a small percentage (say, 3%). This is smaller when computing maintenance margin over initial margin
* The sum is taken over all collateral assets held in the account (stables, LRTs, ETH, BTC, etc).

## Perp Contingency

The account's perpetual position has a small amount of margin associated with it, based on the number of perpetual contracts and spot price:

```python Formula
Perp Contingency = -abs(Perp Size) * PERP_FACTOR * Spot
```

Where:

* `Perp Size` is the number of perpetual contracts (this number is negative for shorts).
* `PERP_FACTOR = 0.03` for maintenance margin and `0.04` for initial margin.
* `Spot` is the spot price of the underlying base asset.

## Option Contingency

For each short option `j` in the portfolio, a small amount of margin is associated with it based on the number of short contracts and a small percentage of the spot price:

```python Formula
Option Contingency = Σ min(0, Option Sizeⱼ) x OPTION_FACTOR x Spot 
```

Where:

* `Option Sizeⱼ` is the number of option contracts held for expiry `i` and strike `j` (this number is negative for shorts).
* `OPTION_FACTOR = 0.005`.
* `Spot` is the spot price of the underlying base asset.

Note, due to the inclusion of the skew scenarios, option contingency in 2.1 is considerably smaller than in 2.0.

## Oracle Contingency

This is the same as [standard margin oracle contingency](/reference/standard-margin/#oracle-contingency), with the exception that Option Oracle Contingency counts the total (absolute value of the) number of long and short options contracts on a given strike, not just the number of short options contracts:

```python Formula
Option Oracle Contingency = - CONFIDENCE_SCALE * Options Size * Spot
    * (1 - min(Spot Confidence, Forward Confidence, Vol Confidence))
```

Where `Options Size` is the total number of long and short options contracts for a given strike/expiry. For example, if there are 4 long calls and 3 short puts on the $2000 3 weekly strike, then `Options Size = 4 + 3 = 7`.

## Depeg Contingency

When USDC depegs from $1, the value of `mFactor` will increase, proportional to the magnitude of the depeg. Specifically, we have

```python Formula
mFactor = mFactor + max(0, 0.99 - USDC Price) * PEG_FACTOR 
```

where

* `PEG_FACTOR = 4.0`
* `USDC_PRICE` is the current price of USDC (measured in USD).

# Open Interest Caps

The portfolio margin manager has a cap on the open interest of all instruments it supports. Namely:

* (ETH, BTC) = (750, 15) base asset
* (ETH, BTC) = (2,000,000, 100,000) options
* (ETH, BTC) = (250,000, 12,000) perpetual contracts

These bounds can be adjusted as necessary over time. A low amount of base asset is set due to limited functionality of this instrument at launch. This will be raised when a spot market is available.

# Risk Reducing Trades and Risk Assessors

As is the case with the standard manager, on the smart contract level, the portfolio margin risk manager will allow any trade to be conducted so long as it satisfies either of the following conditions:

* the initial margin of the portfolio after the transaction is conducted is positive (`IM(post) > 0`) OR
* the trade is risk reducing (see the description in [Standard Margin](https://docs.derive.xyz/docs/standard-margin)).

A subtle but key difference is that closing perpetual positions for portfolio margined accounts is not considered a risk reducing transaction. This is because perpetuals could act as a delta hedge and so removing these positions might increase the risk of the portfolio.

As with the standard manager, the portfolio margin manager will also support risk assessors. In addition to the aforementioned benefits of allowing general risk reducing trades, risk assessors are necessary because computing all scenarios for portfolio margin on chain is expensive.

In order to minimise such gas costs and offer traders large portfolio margined accounts, risk assessors only have to compute 3 scenarios on chain: the "mark-to-market" (mtm) scenario (where spot and vol are static) and the two scenarios used to compute the basis contingency (see below).

**This means that the risk assessors cannot allow insolvent positions to be opened.**

They can, however, allow liquidatable positions to be opened.

# Example

Consider an account comprised of the following:

* 1 x SHORT ETH $2500 CALL (IV = 76%)
* 1 x LONG ETH $2700 PUT (IV = 76%)
* 700 USDC
* 2.1 weETH

where both options have the same expiry (37.2 days) and the corresponding forward is marked at $2,695.78. Assume the risk free rate for the expiry is `r = 21%` and the spot price of ETH is $2,681.60.

## Computing Regular Loss

There are 23 scenarios under which the portfolio is evaluated to compute `Regular Loss`, these are shown in the table below. Let’s explain how to compute the `Regular Loss`.

Consider scenario 1, where the forward increases by 18% and the volatility increases. For the former, this results in the portfolio being marked to

```python Formula
Forward = 2695.78 x 1.18 = 3181.02
```

To find the shocked volatilities, we know

```python Formula
shocked IV = IV shock x IV 
```

where

```python Formula
IV shock = 1 + VOL_RANGE x ((30 / 365) / Time To Expiry))**VEGA_POWER
```

Since `VOL_RANGE = 0.50`, `Time To Expiry = 37.2/365` and `VEGA_POWER = 0.30`, we have:

```python Formula
IV shock = 1 +  0.50 x ((30 / 365) / (37.2 / 365))**0.3 = 1.9549
```

I.e. the volatility is increased by 95.5% (this is multiplicative).

Hence, the shocked volatilities are:

```python Formula
shocked IV(2700 strike) = 0.76 * 1.9549 = 1.486
shocked IV(2600 strike) = 0.76 * 1.9549 = 1.486
```

Using these values, we have:

```python Formula
Mark Price(2700 Put)  = Put(0.76, 2700, 2695.78, 0.21, 37.2/365) = 257.055
Mark Price(2700 Put, shocked) = Put(1.486, 2700, 3181.02, 0.21, 37.2/365) = 335.477
Mark Price(2500 SHORT Call)  = -Call(0.76, 2500, 2695.78, 0.21, 37.2/365) = -353.047
Mark Price(2500 SHORT Call, shocked)  = -Call(1.486, 2500, 3181.02, 0.21, 37.2/365) = -918.811
```

Thus:

```python Formula
PNL(2700 shocked put) = 335.477 - 257.055 = +78.42
PNL(2500 shocked call) = -918.811 - (-353.047) = -565.764
Total undiscounted PNL (2 week expiry) = +78.42 + (-565.764) = -487.344
```

Next, we compute the (anti)discount to apply, i.e.

```python Formula
(Anti)Discount =1/ exp(-(0.5 * 0.21 + 0.08) * 37.2 / 365 ) = 1.01903
```

Note the anti-discounting increases the total negative value of the sub-portfolio. This would be capped at `1 / (exp(-0.21 * 37.2/365)) = 1.02163`

So the final discounted PNL is:

```python Formula
Discounted Total PNL = 1.01903 * (-487.344) = -496.618
```

In the table below, we repeat this methodology for all other scenarios. From the table, we obtain:

```python Formula
Regular Loss = min(PortfolioLoss(shock k)) = -1576.9
```

Now, we compute the forward contingency. Using the scenarios in **bold**, we have:

```python Formula
Up Losses = [min(0, -220.0)] = -220.0
Down Losses = [min(0, -581.3)] = -581.3
Forward Contingency = (0.5 + 2.0 * 37.2/365) * (-581.3) = -409.14
```

As for the collateral contingency, we have a 10% additional haircut for maintenance margin (15% for IM) on weETH (spot price $2800). So we have

```python Formula
Collateral Contingency (MM) = 0.10 * 2.1 * 2800 = $588 
Collateral Contingency (IM) = 0.15 * 2.1 * 2800 = $882 
```

We now compute the option contingency. There is a single short option on the $2500 strike, so we have

```python Formula
Option Contingency = -0.005 x 2,681.60 x 1 = -13.408
```

Finally, we compute the skew losses as

`['LINEAR', -7.187935096715933], ['ABS', -13.325272744348467]`

And so we have `MM = 4554.314` and `IM = 4168.54`

| Scenario | Forward Shock | Vol Shock | Total (Discounted) PNL |
| -------- | ------------- | --------- | ---------------------- |
| 1        | +18%          | Up        | 161.08                 |
| 2        | +13.5%        | Up        | 37.84                  |
| 3        | +13.5%        | Unchanged | 16.63                  |
| 4        | +13.5%        | Down      | -0.73                  |
| 5        | +9%           | Up        | -84.53                 |
| 6        | +9%           | Unchanged | -102.6                 |
| 7        | +9%           | Down      | -118.5                 |
| 8        | +4.5%         | Up        | -206.14                |
| 9        | +4.5%         | Unchanged | **-220.0**             |
| 10       | +4.5%         | Down      | -232.476               |
| 11       | 0             | Up        | -547.77                |
| 12       | 0             | Unchanged | **-344.0243**          |
| 13       | 0             | Down      | -233.9                 |
| 14       | -4.5%         | Up        | -782.30                |
| 15       | -4.5%         | Unchanged | **-581.301**           |
| 16       | -4.5%         | Down      | -471.706               |
| 17       | -9%           | Up        | -1031.5                |
| 18       | -9%           | Unchanged | -840.26                |
| 19       | -9%           | Down      | -738.56                |
| 20       | -13.5%        | Up        | -1296.23               |
| 21       | -13.5%        | Unchanged | -1121.65               |
| 22       | -13.5%        | Down      | -1034.60               |
| 23       | -18%          | Up        | **-1576.937**          |