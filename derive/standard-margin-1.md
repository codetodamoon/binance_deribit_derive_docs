# Standard Margin

Standard margin evaluates each position’s margin in isolation, with the exception of spreads whose margin requirements can be offset for the same expiry. It is the default margin option for traders.

If a standard margin sub-acccount falls below its maintenance margin requirement, the account will be liquidated. See [Liquidations](https://docs.derive.xyz/reference/liquidations) for more details.

# Margin Calculation

The standard margin requirement is calculated by summing an account’s margin requirement for its perpetual and option positions and then adding the value of its USDC balance and base assets (with haircut). For each market, the margin is calculated as follows:

```python Formula
Initial Margin = USDC Balance + Base Collateral + Perp Margin + Option Margin + Depeg Contingency + Oracle Contingency

Maintenance Margin = USDC Balance + Base Collateral + Perp Margin + Option Margin
```

Where:

* `Perp Margin` is the margin requirement for all perpetuals in the account, calculated as a simple percentage of the underlying’s spot price. The profit and loss of said perpetuals, as well as owe/owing funding is included in this value.
* `Option Margin` is the margin requirement for all options in the account, typically calculated as the sum of isolated margin for each option, with the possibility for margin offsets for spreads and other multi-legged strategies within the same expiry.
* `Depeg Contingency` is extra initial margin conditionally required to protect against USDC depegging.
* `Oracle Contingency` is extra initial margin conditionally required to protect against inaccurate oracle data feeds.
* `USDC Balance` is the subaccount's balance of USDC (which can be positive or negative)
* `Base Collateral` is the value of the base asset held in the subaccount (with a risk based haircut).

Each margin component has slightly greater initial margin requirements compared to maintenance margin requirements. This is applied via parameterization. Read the sections below for more details.

Both initial and maintenance margin are centred around zero. I.e. a sub-account is subject to liquidation if its `Maintenance Margin` is negative (see [Liquidations](../liquidations.md) for more details) and a sub-account is only able to open a new position if its final `Initial Margin` is positive. See the end of this page for more detail on this and risk reducing trades.

> 📘 Depeg and Oracle Contingencies are typically zero and only add to initial margin requirements, i.e. they do not change margin requirements for already open positions, but may block opening new positions.

## Base Collateral

Base Collateral which is simply the sum of the value of each base asset, multiplied by a risk based haircut.

```python Formula
Collateral(MM) = Σ (Baseₘ * BASE_DISCOUNTₘ * Spotₘ)
Collateral(IM) = Σ (Baseₘ * BASE_DISCOUNTₘ * BASE_SCALEₘ * Spotₘ)
```

Where:

* `Baseₘ` is the account’s balance of base asset `m`.
* `BASE_DISCOUNTₘ` is the discount factor for base asset `m` (0.8 for ETH and 0.75 for BTC).
* `BASE_DISCOUNT_SCALEₘ` is a scale factor less than 1 used when computing the collateral value of the base asset in initial margin calculations (0.9375 for ETH, 0.93 for BTC).
* `Spotₘ` is the spot price of base asset `m`.

## Perpetuals Margin

All perpetuals have margin requirements proportional to the perpetual price. This percentage is larger when computing initial margin:

```python Formula
Initial Perpetual Margin = -abs(Size * 0.10 * Perpetual Price)
Maintenance Perpetual Margin = -abs(Size * 0.065 * Perpetual Price)
```

Where:

* `Size` is the number of perpetual contracts (this number is negative for shorts).
* `Perpetual Price` is the mark price of the perpetual.

## Option Margin

Options are typically margined in isolation, with the exception of spreads and multi-legged strategies whose margin requirements can be offset for the same expiry.

The option margin for an account is the sum of each expiry’s margin, which is calculated by grouping positions per expiry, summing their isolated margin and offsetting spreads and multi-legged strategies where possible.

Options with different underlyings (ETH, BTC) are margined separately and then added together. I.e. the option margin of all ETH options is found, then added to that for all BTC options to get the total option margin for the subaccount. In the following, we focus on computing the option margin for a single underlying; the option margin for a subaccount with multiple underlyings is easily found given this.

### Isolated Margin

The isolated margin of an option for strike price `j` is calculated as follows:

For long calls and puts:

```python Formula
Initial Isolated Option Marginⱼ = None 
Maintenance Isolated Option Marginⱼ = None
```

For short calls:

```python Formula
Initial Isolated Option Marginⱼ = n * (-max(0.15 - OTMⱼ/Spot, 0.13) * Spot + Mark Priceⱼ)
Maintenance Isolated Option Marginⱼ = n * (-0.09 * Spot + Mark Priceⱼ)
```

For short puts:

```python Formula
Initial Marginⱼ = n * (-max(max(0.15 - OTMⱼ/Spot, 0.13) * Spot
    - Mark Priceⱼ, -1.05 x Maintenance Marginⱼ))

Maintenance Marginⱼ = n * (-max(0.09 * -Mark Priceⱼ, 0.09 * Spot) + Mark Priceⱼ)
```

Where:

* `n` is the number of short options held
* `OTM` is the out-the-money amount. For calls, `OTM = max(0, Strike - Spot)` and for puts, `OTM = max(0, Spot - Strike).`
* `Spot` is the spot price of the underlying base asset.
* `Mark Price` is the mark-to-market value of the option calculated using [Black76](https://en.wikipedia.org/wiki/Black_model) with no discounting. Since the option is short, this is a negative quantity.

**Note**: Traders shorting out of the money put should be aware that it is possible, with a large enough spot move, for their puts to become liquidatable. This is because as the price of the spot/forward rise, so too do the maintenance margin requirements (a percentage of spot for OTMs). That is, while the put itself becomes less valuable, the margin requirements can still rise. Traders should be encouraged to either add more collateral during such periods or close out such positions.

### Expiry Margin

The default margin of expiry `i` is calculated by summing the isolated margin of each option in the account for that expiry:

```python Formula
Default Initial Marginᵢ = Σ Initial Marginⱼ
Default Maintenance Marginᵢ = Σ Maintenance Marginⱼ
```

We also compute an offset margin for expiry `i` to offset spreads and other multi-legged strategies. This is made up of 2 components:

1. The minimum intrinsic value of the expiry’s options evaluated at all strikes in with this expiry (including the zero strike).
2. A percentage of the expiry's forward price multiplied by the number of "naked" short calls in the expiry.

**Note**: Naked short calls in an expiry are found by summing the net positions over all calls in the expiry and capping the result at 0, i.e.

```python Formula
Naked Short Call Size = -max(Number of Short Calls - Number of Long Calls, 0)
```

For example, if an expiry has 3 short calls on the $1600 strike and 2 long calls on the $1800 strike, then Naked Short Call Size is simply -1. Combining all of this, we have

```python Formula
Offset Initial Marginᵢ = min(Intrinsic Valueᵢ, 0)
    + UNPAIRED_SCALE_IM * Naked Short Call Sizeᵢ * Forward Priceᵢ

Offset Maintenance Marginᵢ = min(Intrinsic Valueᵢ, 0)
    + UNPAIRED_SCALE_MM * Naked Short Call Sizeᵢ * Forward Priceᵢ
```

Where:

* `Intrinsic Valueᵢ` is the intrinsic value of all options in the expiry evaluated at each strike `k` for expiry `i` (including the zero strike).
* `Short Call Sizeᵢ` is the number of naked short call contracts open in the expiry `i`.
* `Forward Priceᵢ` is the forward price for the expiry `i`.
* `UNPAIRED_SCALE_IM = 1.2` scales naked short calls for initial margin requirements.
* `UNPAIRED_SCALE_MM = 1.1` scales naked short calls for maintenance margin requirements.

The final margin for expiry `i` is then the better (larger) of the default expiry margin and offset expiry margin:

```python Formula
Initial Marginᵢ = max(Default Initial Marginᵢ, Offset Initial Marginᵢ)
Maintenance Marginᵢ = max(Default Maintenance Marginᵢ, Offset Initial Marginᵢ)
```

Note that both `Default Initial Margin` and `Offset Initial Margin` are negative, so the above takes the more lenient margin requirements for the trader.

### Total Margin

Finally, the total option margin for an account is the sum of each expiry’s margin:

```python Formula
Option Initial Margin = Σ Initial Marginᵢ
Option Maintenance Margin = Σ Maintenance Marginᵢ
```

## Depeg Contingency

When USDC depegs from $1, additional initial margin requirements are added:

```python Formula
Depeg Contingency = -max(0, USDC_THRESHOLD - USDC Value) * Spot * DEPEG_FACTOR
    * (Σ Short Option Sizeⱼ + abs(Perp Size))
```

Where:

* `USDC Value` is the market value of USDC.
* `Spot` is the spot price of the underlying base asset.
* `Short Option Sizeⱼ` is the absolute number of contracts for a short option with strike `j` in the account.
* `Perp Size` is the number of perpetual contracts (this number is negative for shorts).
* `USDC_THRESHOLD = 0.99` is the threshold value of USDC that triggers a depegging event.
* `DEPEG_FACTOR = 2.0` scales the depeg contingency.

Note that this is the depeg contingency for a given underlying (say, ETH). For multiple underlying assets, the relevant asset's spot and corresponding subaccount positions are used.

> 📘 When the USDC value (reported by oracle data feeds) is greater than the depegging threshold, the depeg margin requirement is $0.

## Oracle Contingency

Associated to each oracle data feed is a confidence score; a metric for the feed's reliability and accuracy. When confidence scores are below a given threshold, this indicates the data feeds could be feeding inaccurate price data into the system, and the protocol automatically introduces additional initial margin requirements. Learn more about oracle data feeds [here](../oracles.md).

The oracle contingency is the sum of base, perpetual and option oracle contingencies:

```python Formula
Base Oracle Contingency = - CONFIDENCE_SCALE_SM * Base Balance * Spot
    * (1 - Spot Confidence) if Spot Confidence < BASE_CONF_THRESHOLD, otherwise 0
```

```python Formula
Perp Oracle Contingency = - CONFIDENCE_SCALE_SM * abs(Perp Size) * Spot
		* (1 - min(Spot Confidence, Perp Confidence))
  if min(Spot Confidence, Perp Confidence) < PERP_CONF_THRESHOLD, otherwise 0
```

```python Formula
Option Oracle Contingency = - CONFIDENCE_SCALE_SM * abs(Short Options Size) * Spot
    * (1 - min(Spot Confidence, Forward Confidence, Vol Confidence))
  if min(Spot Confidence, Forward Confidence, Vol Confidence) < OPTION_CONF_THRESHOLD, otherwise 0 
```

I.e.

```python Formula
Oracle Contingency = Base + Perp + Option Oracle Contingencies
```

Where:

* `CONFIDENCE_SCALE_SM = 1.0` is a constant scaling factor.
* `Short Options Size` is the number of short options contracts for the relevant feeds.
* `Perp Size` is the number of perpetual contracts open in the account.
* `Spot Confidence` is confidence score of the spot price data.
* `Forward Confidence` is confidence score of the forward data.
* `Vol Confidence` is confidence of the implied volatility data.
* `Perp Confidence` is confidence of the perpetual price data.
* `BASE_CONF_THRESHOLD = PERP_CONF_THRESHOLD = OPTION_CONF_THRESHOLD = 0.55` represent the thresholds after which extra initial margin is added due to low confidence.

## Open Interest Caps

The standard manager has a cap on the open interest of all instruments it supports. Namely:

* (ETH, BTC) = (250, 5) base asset
* (ETH, BTC) = (2,000,000, 100,000) options
* (ETH, BTC) = (250,000, 12,000) perpetual contracts

These bounds can be adjusted as necessary over time. A low amount of base asset is set due to limited functionality of this instrument at launch. This will be raised when a spot market is available.

# Risk Reducing Trades and Risk Assessors

On the smart contract level, the standard risk manager will allow any trade to be conducted so long as it satisfies either of the following conditions:

* the initial margin of the portfolio after the transaction is conducted is positive (`IM(post) > 0`) OR
* the trade is *risk reducing*

To clarify the last point, we say a trade is *risk reducing* if it consists of one of the following transactions:

* Adds a long option
* Adds a positive amount of the cash asset (USDC)
* Adds base collateral
* Closes a perpetual

It is highly desirable to always allow users to be able to close risky positions. For example, suppose a trader has a short ETH $1700 call with $300 of USDC as collateral. Perhaps the trader wishes to de-risk and buy back 0.5 of this call with some of their USDC.

On the smart contract layer, this transaction does not satisfy the second condition (since cash will be taken from the account to buy back the option). Further, if the option is sufficiently risky, there is no guarantee that the first condition will be satisfied either. This is problematic, since the above conditions would otherwise prohibit users from de-risking their portfolios.

This motivates the existence of **risk assessors (RAs)**; entities permitted to skip certain risk checks (see [portfolio margin](https://v2-docs.lyra.finance/docs/portfolio-margin)) and/or allow certain trades not normally allowed by the managers.

Specifically for the standard manager, the risk assessor will have special logic to always allow users to close risky (i.e. short) positions. On-chain, the managers will always verify that all accounts have positive maintenance margin.

**This means that a malicious risk assessor can never open liquidatable (nor insolvent) positions**.

The worst that such a nefarious entity can do is refuse to permit users from closing risky positions (short options, etc) (but this can still be done permissionlessly on-chain).

# Examples

## Example 1: A simple short call

ETH is trading at $1900.

Account:

* $2000 of USDC
* 3.0 short ETH $1800 calls expiring in 3 weeks, mark price $120 per call.

We have:

```python Formula
USDC Balance = 2000
```

Since ETH is at $1900, we have `OTM = max(0,1800-1900)=0`.

```python Formula
Initial Margin(short calls) = 3 * (-max(0.15 - 0/1900, 0.13) * 1900 - 120) = -$1215
Maintenance Margin(short calls) = 3 * (-0.09 * 1900 - 120) = -$873
```

Thus:

```python Formula
Initial Margin = -1215 + 2000 = $785
Maintenance Margin(short calls) = $1127
```

Since both the maintenance and initial margin are positive, the subaccount is not liquidatable and new positions may be added.

## Example 2: Spread Logic

Account:

* $2000 of USDC
* 8 x SHORT $1700 ETH calls expiring in 2 weeks
* 8 x LONG $1900 ETH calls expiring in 2 weeks

Assume that ETH is trading at $2100 and the 2 weekly forward at $2105.

Let’s begin by computing the default margin.

```python Formula
Default Margin = margin(SHORT 1700 call) + margin(LONG 1900 call)
```

Since long options contribute nothing to the default margin, we only have to compute the margin of the short $1700 call.

Assuming a 0% interest rate and an implied volatility of 92.5%. Then the mark price of the $1700 call is $425. We have:

```python Formula
Default Initial Margin = 8 * (-max(0.15 - 0/2100, 0.13) * 2100 - 425) = -5920
Default Maintenance Margin = 8 * (-0.09 * 2100 - 425) = -4912
```

Next, we compute the offset margin. In the 2 week expiry there are 8 long calls and 8 short calls; thus, there are no naked calls. We compute the intrinsic value at all relevant strikes (0, $1700, $1900) in the table below:

| Strike | Intrinsic Value |
| ------ | --------------- |
| $0     | 0               |
| $1700  | 0               |
| $1900  | -$1600          |

Thus, we have

```python Formula
Offset Margin = -1600
```

Finally,

```python Formula
Initial Option Marginᵢ = max(-5920, -1600) = -1600
Maintenance Option Marginᵢ = max(-4912, -1600) = -1600
```

and so both the initial and maintenance option margin are -$1600. We thus have

```python Formula
Initial Margin = -1600 + 2000 = +400 
Maintenance Margin = -1600 + 2000 = +400
```

## Example 3: Multi Asset Account

Account:

* $25000 USDC
* 8 x SHORT $1700 ETH calls expiring in 2 weeks
* 8 x LONG $1900 ETH calls expiring in 2 weeks
* 7 x LONG BTC perpetuals (with no unrealized PNL or funding)

Assume that ETH is trading at $2100 (as in the above example with 2 weekly forward at $2105) and BTC (and its perpetual) are trading at $28,000.

Using the above example, we know

```python Formula
Initial Option Margin(ETH) = -1600
Maintenance Option Margin(ETH) = -1600
```

To find the same quantities for BTC, we know

```python Formula
Initial Margin(BTC Perp)= -7 * 0.10 * 28000 = -19600
Maintenance Margin(BTC Perp) = -7 * 0.065 * 28000 = -12740
```

Thus, the total margin requirements for the account are:

```python Formula
Initial Margin = 25000 + -1600 + -19600 = 3800
Maintenance Margin = 25000 + -1600 + -12740 = 10660
```

## Example 4: General Case

Consider the same subaccount as in example 3, but now assume:

* The confidence of the BTC perp feed decreases to 0.50 (below the threshold of 0.55) and
* The market price of USDC depegs to $0.7.

Let’s now compute the confidence and depeg margins. We have

```python Formula
Perp Confidence Margin(BTC) = -1.0 * 7 * 28000 * (1 - 0.5) = -98000
```

For depeg margin, we have

```python Formula
Depeg Margin(ETH) = -Max(0, 0.99 - 0.7) * 2100 * 2.0 * (8) = -9744
Depeg Margin(BTC) = -Max(0, 0.99 - 0.7) * 28000 * 2.0 * (7) = -113680
```

So the margin requirements of the subaccount become

```python Formula
Initial Margin= 25000 + -1600 + -19600 + -98000 + -9744 + -113680 = -217624
Maintenance Margin = 25000 + -1600 + -12740 = 10660
```