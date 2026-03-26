# Supported Products

Derive supports trading of the following three products:

1. Options
2. Perpetual futures
3. Spot (Coming Soon)

Derive also supports multi-asset collateral, allowing traders to collateralize with the quote asset (USDC) as well as supported base assets (ETH, wBTC, etc).

In the sections below we detail relevant properties of these asset classes.

# Products

## Options

Users can mint and trade European options for any expiry and strike price in supported markets, provided a supporting oracle data feed for the expiry exists.

All options are settled to a `SETTLEMENT_TWAP_PERIOD = 30 minute` TWAP of the market's base asset price. See [Settlements](https://docs.lyra.finance/docs/settlements) for more detail.

Ethereum options on Derive are marked to the **ETH/USD** price of the underlying and are settled in **USDC**. The ETH base asset (serving as collateral) is marked to the **ETH/USDC** price.

Bitcoin options are marked to the **BTC/USD** price feed and collateral in the form of wBTC is marked to the **BTC/USDC** rate.

Note the longest dated option supported is capped at `MAX_EXPIRY = 400 days`.

## Perpetual Futures

Users can mint and trade perpetual futures for supported markets.

To ensure perpetuals converge to their respective underlying, funding is exchanged between long and short positions. For example, if the perpetual is trading at a premium to the underlying, then longs will pay shorts funding. This encourages further shorts and decreases the differential between the perpetual and spot prices.

Derive perpetuals are settled continuously. Specifically, whenever a user adjusts their position (say, via a trade or withdrawal) their unrealized funding and profit and loss is settled to the current perpetual price (not the spot price). Further, traders can have the settle function called on their account by other users (including themself!).

### Funding Rate

The instantaneous funding rate is calculated as follows.

First, we calculate the impact notional amount (INA):

```python Formula
INA = 4000
```

Next, we determine the impact ask price (IAP) and impact bid price (IBP). These are defined as the average execution price for a market sell (buy) of the impact notional amount respectively. We use these values to calculate the perpetual premium:

```python Formula
Premium = (max(0, IBP - Spot) - max(0, Spot - IAP)) / Spot
```

Where:

* `IBP` is the impact bid price.
* `Spot` is the spot price of the underlying base asset.
* `IAP` is the impact ask price.

Finally, we calculate the funding rate per hour as:

```python Formula
Funding Rate = Premium / perpConvergence + clamp(UnderlyingRate - Premium/perpConvergence, 0.00625%, -0.00625%) 
```

Where:

* `UnderlyingRate= 0.00125%` (equivalently, 0.01% per 8 hours) and
* `perpConvergence = 8`

The funding paid from longs to shorts is then calculated as follows:

```python Formula
Funding = -Perp Size x Funding Rate x Spot x Time Held
```

Where:

* `Perp Size` is the number of perpetual contracts (this number is negative for shorts).
* `Time Held` is the duration the position is open for in hours.

The perpetual funding rates are capped at

* `PERP_MAX_FUNDING = 0.057% per hour for ETH/BTC and 0.4% for all others`
* `PERP_MIN_FUNDING = -0.057% per hour for ETH/BTC and 0.4% for all others`

In other words, the maximum rate longs can pay shorts will be 0.057% of the spot price per hour. Similarly, the maximum rate shorts can pay longs will be 0.057% per hour (represented as -0.057%).

### Marking Perpetuals

Perpetual contracts are marked to the the sum of the current spot price and a `PERP_TWAP_LENGTH = 30 minute` time weighted average price (TWAP) of the difference between the spot and perpetual prices. Specifically, the mark price of the perpetual used in [Standard Margin](https://docs.derive.xyz/docs/standard-margin) and [Portfolio Margin](https://docs.derive.xyz/docs/portfolio-margin) is given by

```
Perpetual Mark Price = Spot Price + 30 minute TWAP(Perpetual Mark Price - Spot Price)
```

where

* `Spot Price` is the current mark price of the spot asset
* `Perpetual Mark Price` is the current mark price of the perpetual asset.

For more detail on said feeds, see [Oracles](https://docs.lyra.finance/docs/oracles-1).

Note that the maximum (minimum) price that a perpetual can be marked to is `1+PERP_MAX_PERCENT_DIFF =1.06` (`1-PERP_MAX_PERCENT_DIFF = 0.94`) of the spot price.

# Collateral

## Quote (USDC)

The protocol's quote asset is USDC. This is the main way by which users collateralize their option and perpetual positions.

The quote asset plays a special role in the protocol since it has an intrinsic lending market built into it: users holding long option positions and/or the underlying assets will be able to borrow USDC, entering into a debit (negative) cash balance.

Users with debited cash pay interest on their balance. Interest accrued from accounts in debit is then distributed to credited (positive cash) accounts. The Security Module (see [Liquidations](https://docs.derive.xyz/docs/liquidations) for more detail) typically takes a share of `SM_FEE = 20%` of all interest paid to positive cash holders. This increases to 100% if the Security Module is depleted.

The more negative cash (borrowing) taking place, the higher the interest rate paid by borrowers. The utilization of USDC is defined as the following ratio

```python Formula
Utilization = totalBorrow/(totalSupply - min(0,netPrint))
```

where

* `totalSupply` is the sum of all positive cash balances
* `totalBorrow` is the sum of all negative cash balances
* `netPrint` is a extra factor used to deal with asymmetric USDC balances around settlement. The vast majority of the time this should be near 0 and will not play a meaningful role in the value of `Utilization`.

The interest rate on debit balances is calculated with the following piece-wise function:

```python Formula
Interest Rate =

if Utilization < OPTIMAL_UTIL:
		MIN_RATE + Utilization / OPTIMAL_UTIL x LOW_SLOPE

if Utilization > OPTIMAL_UTIL:
		MIN_RATE + LOW_SLOPE + (Utilization - OPTIMAL_UTIL) / (1 - OPTIMAL_UTIL) x HIGH_SLOPE
```

Note that interest is continuously settled, meaning that a user's interest can be settled at any point in time.

*It is important to realize that if there is substantial borrowing, traders might not be able to immediately withdraw their USDC. In this very unlikely scenario, interest rates will be extremely high to disincentivize borrowing and encourage loan repayments. Users unable to withdraw in these circumstances will in turn receive a higher interest rate on their USDC balances.*

**Detail on`netPrint`:** Whenever a position is settled, the other side of the trade is not necessarily automatically settled. The `netPrint` variable accounts for this temporary difference. For example, if Alice is long an in-the-money call and Bob is short said call, at settlement Alice might be settled 1 minute  before Bob and be paid out $100 USDC. In this window, `netPrint` will increase by $100 as Alice's cash balance increases by $100. When Bob is settled soon after, his cash balance will decrease by $100 and `netPrint` will decrease by $100. In the contracts, the following relationship always holds:

```
balanceOf + netPrint = totalSupply + totalBorrow
```

where

* `balanceOf` is the total amount of USDC deposited into the system and
* `totalSupply`, `totalBorrow` and `netPrint` are defined above.

It is important to stress again that the `netPrint` variable only accounts for such temporary asymmetric printings. The vast majority of the time it will be small and not contribute meaningfully to the utilization and interest rate calculations.

## Base (Multi-Asset)

Users can also hold the underlying base asset token as a collateral or hedge. The protocol can support any ERC20 token as collateral, with new base assets (and markets) being approved via an onchain governance process. For example, wETH, wstETH or wBTC tokens could be used as collateral.

Unlike options, perpetuals and the quote asset, users can only hold positive balances of the base asset in their accounts.

# Open Interest Fees

On the base layer, trades that increase the total open interest of a given instrument are charged a fee proportional to the spot price and the total number of contracts in the trade. This fee is capped from below. Specifically, if a trade increases the open interest of a given instrument by `n` contracts, then the fee charged to the trader is given by

```
OIFee = max(MIN_OI_FEE, n x SPOT_FACTOR x Spot)
```

where

* `Spot` is the spot price of the underlying base asset being traded (say, the spot price of ETH for an ETH call option)
* `MIN_OI_FEE = $800`
* `SPOT_FACTOR = 70%`

Note: These fee parameters are transitionary and will be lowered substantially post launch.