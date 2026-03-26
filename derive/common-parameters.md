# Common Parameters

# Liquidations

These parameters govern the speed, discounting and fees for both the solvent and insolvent auctions.

| Parameter               | Contract Variable                                                   | Value                    | Bounds                   | Description                                                                                                                 |
| :---------------------- | :------------------------------------------------------------------ | :----------------------- | :----------------------- | :-------------------------------------------------------------------------------------------------------------------------- |
| BUFFER\_SCALE           | `bufferMarginPercentage`                                            | 0.15                     | \[0, 5.0]                | A scaling factor used to determine the buffer margin.                                                                       |
| LIQ\_PERCENT            | `SolventAuctionParams.liquidatorFeeRate`                            | 0.10                     | \[0.0, 1.0]              | A percentage of the mark-to-market value of the portfolio used to compute the liquidation fee.                              |
| INITIAL\_DISCOUNT       | `SolventAuctionParams.startingMtMPercentage`                        | 0.05 (0.95 in contracts) | \[0,1.0]                 | A small discount to the mark-to-market value of the portfolio used to compute the initial offer at the liquidation auction. |
| FAST\_DISCOUNT          | `SolventAuctionParam.fastAuctionCutoffPercentage`                   | 0.3 (0.7 in contracts)   | \[0, `INITIAL_DISCOUNT`] | Percentage discount at which the liquidation auction will begin to slow.                                                    |
| FAST\_LIQUIDATION\_TIME | `SolventAuctionParams.fastAuctionLength`                            | 15 minutes               | \[0, 100 hours]          | Duration over which the fast component of the liquidation auction occurs.                                                   |
| LONG\_LIQUIDATION\_TIME | `SolventAuctionParams.slowAuctionLength`                            | 12 hours                 | \[0, 1000 hours]         | Duration over which the slow component of the liquidation auction occurs.                                                   |
| INSOLVENT\_DURATION     | `InsolventAuctionParams.totalSteps InsolventAuctionParams.coolDown` | 60 minutes               | No bounds                | Duration of the insolvent auction                                                                                           |

# Cash (USDC)

These parameter control the interest rate paid on negative balances of the USDC cash asset.

| Parameter           | Contract Variable    | Value                                 | Bounds      | Description                                                                              |
| :------------------ | :------------------- | :------------------------------------ | :---------- | :--------------------------------------------------------------------------------------- |
| OPTIMAL\_UTIL       | `optimalUtil`        | 0.85                                  | \[0.0, 1.0] | Ideal utilization (borrowing) of USDC, beyond which the interest rate increases quickly. |
| MIN\_RATE           | `minRate`            | 0.02                                  | \[0.0, 2.0] | Minimum interest rate                                                                    |
| LOW\_SLOPE          | `rateMultiplier`     | 0.08                                  | \[0.0, 5.0] | Slow change in interest rate per change in utilization                                   |
| HIGH\_SLOPE         | `highRateMultiplier` | 0.90                                  | \[0.0, 5.0] | Fast change in interest rate per change in utilization                                   |
| SM\_INTEREST\_SHARE | `smFeePercentage`    | 0.2 if no insolvencies, 1.0 otherwise | \[0.0, 1.0] | Percentage of the accrued interest given to the Security Module.                         |