# Liquidations

Users who fall beneath their maintenance margin requirements are subject to liquidation. In this section we describe how liquidations take place. The ability to liquidate is open to all; users are encouraged to operate their own liquidation bots. A script is provided [here](https://hackmd.io/5hMWWuLmRIeiCiwwO-xbUQ#Bidding-script).

# Liquidation Auctions

Liquidations are performed via an auction system where a percentage of all assets in the subaccount (quote, base, perpetuals and options) are available to bidders.

When a user falls beneath their maintenance margin requirements (i.e. maintenance margin becomes negative), any user can call the function `liquidate()`on said account. The auction process then begins; the user is prohibited from conducting any transactions using the flagged account.

**A note on portfolio margined accounts**: To avoid excess computation done on chain, when a portfolio margined subaccount is flagged for liquidation, the flagger will also submit what they believe to be the worst case scenario (say, spot up 15%, IV static). This scenario, along with the static vol/spot and basis contingency scenarios, will be used to compute the margin requirements of the portfolio, as well as the buffer margin (explained below). As the auction proceeds (or if the flagger is incorrect), this scenario may no longer be the "worst" scenario. Consequently, liquidators (or any user) are able to submit what they believe to be the "correct" "worst" scenario. This new scenario is accepted if it decreases (makes more negative) the maintenance margin requirement of the subaccount in question.

## Buffer Margin

When liquidating an account, it is important that the resulting account has sufficiently more collateral than its maintenance margin requires. Otherwise, a small move in price against it will result in it being liquidated again. Consequently, we define the buffer margin as:

```Text Formula
Buffer Margin = Maintenance Margin + 
    BUFFER_SCALE * (Maintenance Margin - MtM Value)
```

Where:

* `MtM Value` is the mark-to-market value of the account. This is the sum of all credits (quote and base collateral for all accounts, long options for portfolio margin) and debits (short options for all accounts). Note that the unrealized profit and loss of perpetuals is accounted for in the quote asset due to continuous settlement.
* `Maintenance Margin` is defined for [Standard Margin](https://docs.derive.xyz/reference/standard-margin) and [Portfolio Margin](https://docs.derive.xyz/reference/portfolio-margin) accounts.
* `BUFFER_SCALE = 0.15.`

The `Buffer Margin` represents how much USDC needs to be added to the account in order to terminate the liquidation. For instance, a buffer margin of -1000 means at least $1000 USDC needs to be added in order to stop the liquidation.

> 📘 At the end of a liquidation, we want the liquidating account's buffer margin to be 0 to ensure the a small price move doesn't liquidate the account again.

## Charging the Liquidation Fee

When an account is flagged for liquidation, a small fee is charged. The fee is computed using the following formula:

```python Formula
Liquidation Fee = MtM Value * 0.10 * Buffer Margin
    / (Buffer Margin - MtM Value)
```

## Solvent Auction

After the liquidation fee is charged, the account is put up for a solvent auction. Liquidators can take on a percentage of the entire subaccount at discount to the mark-to-market value.

This discount starts at `INITIAL_DISCOUNT = 5%`, meaning the liquidator buys a percentage of the original subaccount at a 5% discount to the mark-to-market value (mark determined by the manager).

The discount increases linearly from `INITIAL_DISCOUNT` to `FAST_DISCOUNT = 30%` over a period of `FAST_LIQUIDATION_TIME = 15 minutes`.

When a liquidator wishes to liquidate the subaccount, they are able to take on any percentage of the subaccount up to a cap. The cap is computed as follows:

```python Formula
Max Percentage = Buffer Margin / (Buffer Margin - (1 - Discount) * MtM Value
    - Discount x Reserved Funds)
```

Where:

* `Discount` is the current percentage discount to the mark-to-market value.
* `MtM Value` is the mark-to-market value of the entire portfolio defined above.
* `Reserved Funds` is the amount of cash received by the portfolio from all previous liquidations during the auction. It is necessary to ensure liquidators liquidating at the same price/discount pay the same price.
* `Buffer Margin` is defined above of the current subaccount (including reserved funds)

If a liquidator requests more than the cap, they are floored at `Max Percentage` and the liquidation terminates.

Otherwise, the liquidator receives their requested percentage of the current portfolio and pays

```c Formula
Liquidator Cost = Percentage Received by Liquidator x (Portfolio Value - Reserved Funds) x (1-discount)
```

USDC to take on their chunk of the portfolio. The liquidation then continues.

If the discount reaches `FAST_DISCOUNT = 30%`(i.e. the liquidation has been ongoing for `FAST_LIQUIDATION_TIME = 15 minutes`) then the auction continues with the discount decreasing from `FAST_DISCOUNT` up to 100% over `LONG_LIQUIDATION_TIME = 12 hours.`

The above methodology for liquidations still holds, the only difference is that the discount increases at a slower rate.

*Note that all perpetual PNL and funding, along with all interest on the USDC cash asset is settled before any liquidator bid.*

The solvent auction terminates if any of the following conditions are met:

* `Buffer Margin ≥ 0`. This can occur through any or all of the following:
  * sufficient amounts of the portfolio (see below) have been liquidated (i.e. `Max Percentage`)
  * the market has moved sufficiently in the trader's favour.
* The discount hits 100% (i.e. liquidators can take on the portfolio for free). This triggers the start of the insolvent auction (see below).
* `MtM Value ≤ Reserved Funds` (indicating current portfolio is insolvent as per the feeds)
  * if `MtM Value ≤ 0` the insolvent auction (see below) can immediately begin
  * if `MtM Value ≥ 0`  then
    * if `Maintenance Margin < 0`  the solvent auction restarts
    * if `Maintenance Margin ≥ 0` the trader's liquidation is terminated.

### Insolvent Auction

At the insolvent auction, offers start at the mark-to-market value of the portfolio and increase over `INSOLVENT_DURATION = 60 minutes` to the current `Maintenance Margin` of the liquidated portfolio.

Specifically, the current offer at a given time `t` will be given by

```Text Formula
currentOffer = min(0, mtm) + (t/INSOLVENT_DURATION) * (Maintenance Margin - min(0, mtm))
```

where

* `mtm` is the mark-to-market value of the portfolio

From the above, it is clear that all offers at the insolvent auction will also be negative. This indicates that the liquidator will be paid by the security module (SM) (see below) to take on the portfolio.

The percentage that can be taken on by the liquidator during the insolvent auction is always set to 100%.

Offers which reach the maintenance margin stay there indefinitely (note the price liquidators receive will continuously vary with the dynamic maintenance margin of the portfolio).

The insolvent auction ends when either of the following conditions are met:

* All of the portfolio has been liquidated
* `Maintenance Margin ≥ 0`

**Note**: *At the beginning of every insolvent auction, the initial maintenance margin of the portfolio in question is added to a cached sum. If this amount exceeds the total balance of the security module, then all USDC withdrawals are blocked until this is no longer the case. For instance, say Alice became insolvent at time t\_1 and Bob at time t\_2. Let their maintenance margin at these times be MM(Alice, t\_1) and MM(Bob, t\_2) respectively. If Charlie becomes insolvent at t\_3, then USDC withdrawals are blocked if MM(Alice, t\_1) + MM(Bob, t\_2) + MM(Charlie, t\_3) > Security Module Balance. When an insolvent auction terminates, the cached maintenance margin is removed. The Security Module also earns fees and interest over time, so whenever the resulting sum is smaller than the Security module balance, USDC withdrawals can recommence.*

# Insurance Fund

The insurance fund pays liquidators to take on insolvent accounts. In return for backstopping the system, the SM receives a variety of fees. In the event of a large insolvency, the SM could be depleted, triggering socialized losses.

## Socialized Losses

When the security module does not have enough funds to pay out an insolvency, socialized losses are enforced via a temporary withdrawal fee. All users trying to withdraw USDC from the system will be charged a withdrawal fee. This fee applies uniformly to all users and scales with the size of the insolvency.

The temporary withdrawal fee is calculated as follows:

```python Formula
Temporary Withdrawal Fee % = Unpaid Insolvent Debt / (Unpaid Insolvent Debt + Deposited USDC)
```

For example, if the SM is unable to cover $100,000 of insolvent debt and there is $1,000,000 of USDC deposited in the system, then the temporary withdrawal fee is

```python Formula
Temporary Withdrawal Fee % = 100000/(100000 + 1000000) = 9.09%
```

In this example, if a user wishes to withdraw $20,000 USDC, then $20,000 x 9.09% will be charged as part of the temporary withdrawal fee.

When the temporary withdrawal fee is in effect, the security module will receive `SM_FEE = 100%` of all interest payments. This is to ensure the withdrawal fee vanishes as quickly as possible.

# Liquidator Requirements

To liquidate an account, the liquidator must have an account consisting **only** of the cash asset (USDC).

The liquidator is required to possess a minimum amount of cash. During the solvent auction, the liquidator is required to have sufficient cash so that their buffer margin (after taking on the portfolio) is 0. Specifically, the liquidator is required to have the following amount of cash

```Text Formula
cashRequired = Percent Liquidated * (1-discount) * (MtM Value - ResFunds) + f * |Buffer Margin - ResFunds|
```

where:

* `cashRequired` is the cash required to be in the liquidator's account in order to perform the liquidation
* `Percent Liquidated` is the percentage of the current portfolio the liquidator has taken on
* `discount` is the current mark-to-market discount
* `MtM Value` is the mark-to-market value of the portfolio
* `ResFunds` is the total reserved funds for the portfolio undergoing solvent liquidation
* `Buffer Margin` is the buffer margin of the account.

The first term represents the cash the liquidator pays to take on the account (they have to have enough cash to buy it!) while the second term represents the amount of extra USDC that must remain in the liquidator’s cash account to ensure the resulting portfolio has at least zero buffer margin.

For the insolvent auction, we instead require that the liquidator's final account has zero maintenance margin. Similar logic yields

```Text Formula
cashRequired = Percent Liquidated * (Maintenance Margin) - cashReceived
```

where

* `Collateral`  and Maintenance Margin are defined above
* `cashReceived` is the cash received by the liquidator for taking on the portfolio.

**Note**: *A liquidator is required to have a subaccount using the same manager as the liquidated account. I.e. if Bob is liquidating Alice who is using the standard manager, then the subaccount Bob is liquidating Alice with must also subscribe to said manager.*

# Example (Solvent Auction)

Consider Alice’s portfolio consisting of:

* $400,000 of USDC
* 10 short ETH perpetuals
* 30 short ETH calls

Suppose Alice’s portfolio is subject to liquidation.

**Liquidation Fee**: Let’s say the initial market value of her portfolio is $100,000 and her buffer margin is -$60,000. The initial fraction variable in computing her liquidation fee is given by

```python Formula
initial fraction = -60,000/(-60,000-100,000)=37.5%
```

The liquidation fee she is charged is

```python Formula
Liquidation Fee = $100,000 x 10% x 37.5% = $3750
```

**Auction**:

Suppose there are two liquidators in the system: Bob and Charlie.

The auction starts at a discount of 5%. Some time passes and the discount increases to 12%. At this point, Bob decides he wants to liquidate. Alice’s mark-to-market value and buffer margin are recomputed - say these are $98,000 and -$62,000 respectively.

The maximum amount that Bob can liquidate is given by

```python Formula
Max Perecentage = -62000/(-62000 - (1-0.12) x 98,000) = 41.8%
```

Bob only wants to liquidate 20% of Alice’s portfolio. He pays

```python Formula
Bob Cost = 20% x 98000 x (1-0.12) = $17,248
```

To take on 20% of Alice’s portfolio. I.e. he receives 20% of:

* $400,000 of USDC ($320,000 remaining)
* 10 short ETH perpetuals (8 remaining)
* 30 short ETH calls (24 remaining)

Bob is required to have the following cash in his liquidating account

```python Formula
Required Cash (Bob) = 20% x 98000 x (1-0.12) + 20% x |-62,000| = $29,648.
```

The liquidation continues. When the discount reaches 30%, Charlie decides to liquidate the remainder of Alice’s portfolio.

When Charlie decides to step in, Alice’s ***remaining*** portfolio has a mark-to-market value of $82,000 and buffer margin of -$46,000. There are also $17,248 of reserved funds. The maximum amount that Charlie can liquidate is given by

```python Formula
Max Perecentage = -46000/(-46000 - (1-0.3) x 82000 - 0.3 x 17248) = 42.37%
```

I.e. Charlie can liquidate up to 42.37% of Alice’s remaining portfolio. Let’s say Charlie does so - he pays

```tsx Formula
Charlie Cost = 42.37% x 82000 x (1-0.3) = $24320.4
```

and receives 42.37% of

* $320,000 of USDC (note: $17,248 of reserved funds are not included!)
* 8 short ETH perpetuals
* 24 short ETH calls

The liquidation terminates as Alice’s portfolio is now deemed safe (since her buffer margin is zero). Her final portfolio consists of

* $184,416 + (24320.4 + 17248) USDC
* 4.6 short ETH perpetuals
* 13.83 short ETH calls

Charlie is required to have the following amount of cash in his account

```python Formula
Required Cash (Charlie) = 42.37% x (82,000 - 17,248)  x (1-0.3) + 42.37% x |-46,000 - 17,248 | = $46,000
```

Note that this is precisely the current buffer margin of Alice's account!

# Example (Insolvent Auction)

Alice’s portfolio becomes insolvent and the insolvent auction begins.

Suppose that 10 minutes into the insolvent auction process, Bob wants to liquidate Alice. At this point in time, her portfolio has

* `Mark to Market = -$4000`
* `Maintenance Margin = -$15,000`

Using the formula from above, we have

```Text Formula
currentOffer = min(0, mtm) + (t/INSOLVENT_DURATION) * (MM - min(0, mtm))
```

where

* `mtm = -$4000`
* `t = 10`
* `INSOLVENT_DURATION = 60`
* `MM = -$15,000`

This yields

```Text Formula
currentOffer = -$5,833.33
```

In other words, Bob can take on 100% of the portfolio and receive an additional payout of $5833.33 from the Security Module. If Bob wants only 40%, then he receives 40% of the entire portfolio and is paid out

```Text Formula
SM Payout = 0.4 * 5833.33=2333.33
```

from the SM. He is required to have

```Text Formula
cashRequired = f * |MM| - cashReceived = 0.4 * |-15,000| - 2333.33= $3666.67
```

of USDC in his account in order to liquidate the account.