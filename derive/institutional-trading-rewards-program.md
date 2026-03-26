# Institutional Trading Rewards Program

*\*The program outlined below and all numbers provided are subject to change.*

# Program Purpose

The purpose of the Program is to support the development of the products listed below by increasing liquidity and volume in the Exchange’s central limit order book and RFQ platform, therefore, benefiting all Participants in the market.

# Program Scope

Options, Spot, and Perpetual Futures trading on the Derive Exchange.

# Eligibility

The Program is open to any firm or individual completing the application and meeting program requirements. All applicants are subject to review and approval by the Exchange. The program is opt-in so all firms must provide all relevant information to be considered for rewards.

If a participant meets the conditions of different fee tiers through market making, and total trading volume, they will enjoy their highest eligible fee tier. Wash trading is strictly prohibited and will result in disqualification from all reward programs.

# Partner Incentives

## Market Maker Rewards:

**60K $OP rewards pool per 28-day epoch** distributed to qualifying Market Makers split pro-rata based on Market Making Score.

* 40K allocated to options (80% orderbook, 20% RFQ)
* 20K allocated to perpetual futures (50% to majors, 50% to Alt markets)

Up to **$500,000 USDC per 28-day epoch exchange rebate program**

## OP Perp Taker Rewards

10k OP rewards **pool per 28-day epoch** distributed to qualifying Takers split pro-rata based on taker volume

* Requirements: > $250M notional OR > 7.5% in TAKER VOLUME

## DRV and OP Public Trading Incentives

* Traders and Market Makers earn points on net taker fees paid
* Currently, 10K OP allocated weekly in public rewards

## DRV Market Maker Incentives

1M DRV rewards pool distributed to qualifying Market Makers split pro-rata based on Market Making Score.

* 500K allocated to options
* 500K allocated to perpetual futures (50% to majors, 50% to Alt markets)
* DRV will be distributed after the airdrop period when the token is launched. 50% of the earned Market Maker DRV rewards are to be paid out immediately when DRV goes live, while the other half is subject to a 6-epoch vesting period. If the address becomes inactive (**earns rewards \< 1 % of MM score**) for any epoch during the vesting period, the vested rewards will be subject to forfeiture. All rewards are subject to governance approval.

# Trading Fees

Derive's fee structure consists of:

* A maker/taker fee model
* Fee-based rebate program for market makers and volume program participants

| Fees | Perpetual Futures Maker | Perpetual Futures Taker | Spot Maker | Spot Taker | Options Maker | Options Taker |
| :--- | :---------------------- | :---------------------- | :--------- | :--------- | :------------ | :------------ |
| ETH  | 1bps                    | 3bps                    | 15bps      | 15bps      | 1bps          | 3bps          |
| BTC  | 1bps                    | 3bps                    | 15bps      | 15bps      | 1bps          | 3bps          |
| ALT  | 1bps                    | 3bps                    | 15bps      | 15bps      | 1bps          | 3bps          |

*\*Derive's matching fees are subject to change.*

# USDC Rewards Pool Overview:

Market Makers add value to the protocol by lowering the cost to trade. The Derive Exchange pays program participants on their Maker Volume, and offers discounts to high-volume Takers, through a rebate program. Trading rebates are distributed in 28-day epochs and outlined in the table below.

**$500,000 USDC rewards pool per 28-day epoch**

* Rebate program
* $250K (50%) Options
* $250K (50%) Perpetual Futures (50% to majors, 50% to Alt markets)
* Participants receive discounted fees atomically
* Negative fees are processed at a rebate at the end of each epoch
* New Market Makers eligible to receive introductory top fee tier for first full epoch of quoting
* Fee tier and rebate determined by Market Maker Ranking and Volume Trading rebates are distributed in 28-day epochs and outlined in the table below:

| MM % Score Share |    | 28-day Volume |    | 28-day Volume Share |     | stDRV Holdings  | Perp Maker Fee | Perp Taker Fee | Option Maker Fee | Option Taker Fee | Spot Maker Fee | Spot Taker Fee |
| :--------------- | :- | :------------ | :- | :------------------ | :-- | :-------------- | :------------- | :------------- | :--------------- | :--------------- | :------------- | :------------- |
| 10%              | OR | ≥ 250M        | OR | ≥ 10%               | AND | ≥ 500,000 stDRV | -0.010%        | 0.015%         | -0.0050%         | 0.0075%          | 0%             | 0%             |
| 10%              | OR | ≥ 250M        | OR | ≥ 10%               | OR  | ≥ 500,000 stDRV | -0.005%        | 0.020%         | -0.0025%         | 0.0100%          | 0%             | 0%             |
| 5%               | OR | ≥ 100M        | OR | ≥ 5%                | OR  | ≥ 250,000 stDRV | -0.0025%       | 0.025%         | -0.0013%         | 0.0125%          | 0%             | 0%             |
| 1%               | OR | ≥ 25M         | OR | ≥ 2%                | OR  | ≥ 100,000 stDRV | 0.000%         | 0.030%         | 0.000%           | 0.0150%          | 0%             | 0%             |
| All traders      |    | \< 25M        |    |                     |     |                 | 0.01%          | 0.030%         | 0.01%            | 0.030%           | 0%             | 0%             |

**RFQ Discounts**

| 28-day RFQ Volume |    | 28-day RFQ Volume Share | RFQ Maker Fee Discount | RFQ Taker Fee Discount |
| :---------------- | :- | :---------------------- | :--------------------- | :--------------------- |
| ≥ 100M            | OR | ≥ 5%                    | 100%                   | 50%                    |
| ≥ 25M             | OR | ≥ 2%                    | 25%                    | 10%                    |
| \< 25M            | OR | \< 2%                   | 0%                     | 0%                     |

* MM ranking, Volume share, and stDRV holding requirements are by market, i.e. spot, options, perps separately.
* To achieve best fee tiers it requires 500k stDRV per program i.e. 1M required for both options and perps
* Discounts will apply on a percentage basis to fees on spreads and other complex fee logic and calculated pro-rata if $500K rewards is exceeded
* Fees are subject to change
* See [fee documentation](https://docs.lyra.finance/reference/fees-1) for more details

# Obligations:

The central limit order book will be snapshotted randomly, in 15 minutes windows to evaluate Market Maker performance. Optimizations to be added for supporting desired strike ranges and expiries if deemed necessary after launch. For simplification, the only requirement for MMs is to meet the minimum quote size in order for their quotes to be counted for a given snapshot:

To qualify for rewards, Traders must meet the following requirements:

* Market Maker Program:
  * Min quote size: $500
  * Delta Range: only options with a Delta > 1 and \< 98 will be included in the scoring snapshots
  * 28-day Notional Trading Volume > $5M
  * Trading Volume Share > 2%

# Additional Incentives:

## Increased Rate Limits

Rate limits on matching engine requests are in place as a safeguard for the exchange's order processing capacity. Participants in the Taker Incentive Program are eligible for the highest level of Matching Engine Requests. Initially, there will be two tiers for rate limiting based on Market Maker and Taker status. The exchange reserves the right to add additional rate limit tiers to be assigned based on a combination of Volume Share and Market Maker Rankings.

![Rate Limits](https://files.readme.io/3b8ed9789693cda59222a4bdcb7e1ead4c08c17e8159f70acda60846c3385580-image.png)

## Advanced Market Maker Protections

Participants in the program may choose to enable the following Market Maker Protections:

* Cancel\_On\_Disconnect
* Trade\_limit - max # of trades per time interval
* Quantity\_limit - max # of instruments per time interval
* Delta\_limit - max # delta per time interval
* Post\_only - order rejects if it would execute on post
* Frozen\_time - auto reset of MMPs
* Manual Reset (if desired)

### How to participate?

* **No commitments required**
* Please provide contact information [here](https://forms.gle/ivDyhdSUGGWPJsJh7)

### Monitoring and Termination of Status

The Exchange reserves the right to remove any Participant from this Program if the Exchange has determined, in good faith, that the Participant consistently and egregiously underperforms the obligations, as determined by the Exchange in its sole discretion. Moreover, the Exchange reserves the right to prohibit the participant and any affiliated entities or individuals from trading, accessing, or participating in any Exchange products and programs for an indefinite period of time, including a prohibition that extends for several years, if the Exchange determines the Participant is found to have engaged in willful misconduct of Exchange Rules.

# Appendix - Market Making Scoring Calculations

**Market Makers are scored on:**

* Market Coverage (40% for Options, 50% for perps)
* Market Quality (40% for Options, 50% for perps)
* RFQ (20% for Options, 0% for perps).

**Market Coverage, Market Quality scores are boosted by:**

* Distance from Best Market Multiplier
* Market Scaling Factor

RFQ Scores are boosted by:

* Distance from Max Cost.
* Time Score
* Market Scaling Factors

**MM Scores are then weighted by:**

* Volume Share

## Distance from Best Market Multipliers

* MM’s score for **Market Coverage and Market Quality** include a series of multipliers based on an order's price distance from best market price
* The further quotes are away from the BBO, the lower the multiplier
* The closer orders are to the BBO, the higher the multiplier
* Low-quality markets have a multiplier of 0

| Distance from BBO Options     | Multiplier |
| :---------------------------- | :--------- |
| \< 0.10%                      | 5          |
| 0.10% \< Order Price \< 0.50% | 1.5        |
| 0.50% \< Order Price \< 1.0%  | 1          |
| 1.0% \< Order Price \< 2.0%   | 0.5        |
| > 2%                          | 0          |

| Distance from BBO Perps        | Multiplier |
| :----------------------------- | :--------- |
| \< 0.0050%                     | 5          |
| 0.005% \< Order Price \< 0.01% | 1.5        |
| 0.01% \< Order Price \< 0.05%  | 1          |
| 0.05% \< Order Price \< 0.10%  | 0.5        |
| > 0.1%                         | 0          |

*\*Weightings and categories are subject to change, see the documentation for the most up to date.*

## Distance from Max Cost

* MM’s RFQ Score includes a series of multipliers based on an RFQ responses price distance from the maximum cost of the order
* The closer quotes are to the Max Cost, the lower the multiplier
* The more competitive the RFQ responses are, the higher the multiplier
* Low-quality markets have a multiplier of 0

| Distance from Max Cost | Multiplier |
| :--------------------- | :--------- |
| \< 0%                  | 0          |
| 0 - 1%                 | 1          |
| 1 - 3%                 | 2          |
| 3%+                    | 4          |

*\*Weightings and categories are subject to change, see the documentation for the most up to date.*

## Market Scaling Factor

Each market, set of expiries, or group of strikes can have a unique Market Scaling Factor to encourage liquidity. As markets mature, MSF can be set to 0 and new markets will be incentivized. Initially all market scaling factors will be set to 1.

| Market                         | Market Scaling Factor |
| :----------------------------- | :-------------------- |
| ETH Perps                      | 1                     |
| ETH Weekly Options \< 7 DTE    | 3                     |
| ETH Long-Dated Options > 7 DTE | 1                     |
| BTC Perps                      | 1                     |
| BTC Weekly Options \< 7 DTE    | 3                     |
| BTC Long-Dated Options > 7 DTE | 1                     |
| SOL Perps                      | 1                     |
| DOGE Perps                     | 1                     |

| Alt Markets | Market Scaling Factor |
| :---------- | :-------------------- |
| BNB         | 1                     |
| XRP         | 1                     |
| LINK        | 1                     |
| AVAX        | 1                     |
| UNI         | 1                     |
| TAO         | 1                     |
| WIF         | 1                     |
| OP          | 1                     |
| NEAR        | 1                     |
| ARB         | 1                     |
| Aave        | 1                     |
| INJ         | 1                     |
| BONK        | 1                     |
| TIA         | 1                     |
| SUI         | 1                     |
| ENA         | 1                     |
| PEPE        | 1                     |
| Worldcoin   | 1                     |
| SEI         | 1                     |
| EIG         | 1                     |

*\*Market scaling factors are subject to change, see the documentation for the most up to date.*

## Option & Perpetual Futures Scoring

### Market Coverage 50% (40% post RFQ implementation)

* Time in Market
  * % of the time MMs quotes are on for specified strikes, and expiries. A MM is considered "on" if they are meeting min quoting obligations when the snapshot is taken.
  * √(∑ (# snapshots MM is on for *Distance from Best Market Multiplier) / # of snapshots taken)* Market Scaling Factor.
  * Let:
    * Db = Distance from Best Market Multiplier on the bid side
    * Da = Distance from Best Market Multiplier on the ask side
    * N = # of snapshots taken
    * n = # of snapshots MM is on for
    * F = Market Scaling Factor

![Market Coverage Formula](https://files.readme.io/739ac9b27ff42abce3058148f14cb5f6b8e781afd332c5db448f93c609889d12-Schermafbeelding_2024-11-27_om_00.30.52.png)

### Market Quality 50% (40% post RFQ implementation)

* Book Size
  * Let:
    * Vb = MM quantity bid volume, scaled by its multiplier
    * Va = MM scaled quantity ask volume
    * Ta =Total scaled quantity bid Volume
    * Tb = Total scaled quantity ask Volume
    * Dmax = Maximum Distance from BBO Multiplier (currently 5x)
  * Total MM bid/ask volume relative to exchanges total bid-ask volume taken at each snapshot
  * Sqrt taken to smooth results
  * Scaled volumes = volume scaled by distance from BBO multipliers (I.e. $1000 notional at top of book = $5K)

![Market Quality Formula](https://files.readme.io/ebe599e1f0b6764d0236526782da862b739dddc4cc5253bf22ca6d38187a1c69-Schermafbeelding_2024-11-27_om_00.34.06.png)

### RFQ Score 20% Options (0% Perps) - Scored Separately

Note: upon launch of RFQ, Market Coverage and Market Quality Scores weighting will each be reduced by 15% and 5% respectively to account for a 20% RFQ allocation.

RFQ is scored as follows:

* For each MM, we consider all quotes for a given RFQ
* For each quote, we compute the `DistanceFromMaxCost` and the `TimeScore`
* E.g. if MM ABC posts 3 different responses to Alice’s RFQ, we compute `DistanceFromMaxCost/TimeScores` for each quote and select the maximum
* If a RFQ is filled, it is scaled by `filledScale = 1.0`, otherwise `0`

<br />

* *maxj(⋅)* indicates that we're taking the max combined score for a given order (i.e. the order).
  * Let:
    * RFQV = RFQ volume for the given order
    * RFQT =Total RFQ Volume is the total volume (i.e. all other quotes from unique MMs) on this order. Note that each MM’s volume is only counted once.
    * *Fi* = Market Scaling Factor
    * TS = Time Score
    * RFQD = RFQ response’s Distance From Max Cost
    * *fi* =  `filledScale`  which is 1.0 if the order is filled and 0.1 otherwise

![RFQ Score Formula](https://files.readme.io/5920d002f784994727c4dd584f7af927b93c3e58023d6888a518bc251f711b53-Schermafbeelding_2024-11-27_om_19.55.00.png)

**RFQ Example**

Alice posts an RFQ for 100 x LONG ETH $2900/$3200 call spreads at `t=0` with

* Best bid on $2900 = $24
* Best ask on $3200 = $10
* Order book BBO = $14

At

* t = 0.4s, ABC posts a quote at $13.5 per spread - 100 ETH
* t = 1.5s, XYZ posts a quote at $13 per spread - 100 ETH
* t = 2.5s XYZ posts a fill at $12.5 per spread - 100 ETH

For ABC’s Quote:

* RFQD= ($1400-$1350)/$1400 = 3.5% = 4
* RFQV = $1350
* RFQT = ??
* TimeScore = 2
* f = 0.1

For XYZ’s Quote:

* RFQD= ($1400-$1300)/$1400 = 7.1% = 4
* RFQV = $1300
* RFQT = ??
* TimeScore = 1
* f = 0.1

For XYZ’s Fill:

* RFQD= ($1400-$1250)/$1400 = 10.7% = 4
* RFQV = $1250
* RFQT = ??
* TimeScore = 0.5
* f = 1

ABC’s `RFQScore` = 1/???√∑(1250*1*.1\*Max((4+2),(4+1))

XYZ’s `RFQScore` = √∑(($30,000*5\_.5)/($90,000))* 1\_  1.2 = 1

ABC gets a 5 DMM multiplier for being filled on their second quote

XYZ Gets a 5 DMM for being filled

## Trading Volume Multiplier

MMs Score is boosted by % volume traded

Volume Weight = 2.0

![Trading Volume Multiplier](https://files.readme.io/9d3d061a3e8ee639b09cf328992a0b06ef4494c7033309c25b92ecee75a0eacc-Schermafbeelding_2024-11-27_om_01.19.26.png)

## Final Score

![Final Score Formula](https://files.readme.io/325f9dfd5b7d64149f62b2395efa5a6b4b43c4cc8c1260c830f41c7bcdac272a-Schermafbeelding_2025-02-25_om_00.07.11.png)

## Example (Options):

|         | Market Coverage | Market Quality | Quote Efficiency | Initial Score | Initial Rank | Volume Boost | Final Score | Final Rank |
| :------ | :-------------- | :------------- | :--------------- | :------------ | :----------- | :----------- | :---------- | :--------- |
| Metrics | Time in Market  | Book Size      | Msg-Volume Ratio | Score         |              |              |             |            |
| Weight  | 50%             | 40%            | 10%              | 100%          |              |              |             |            |
| Score   | 0.8             | 0.9            | 0.001            |               |              |              |             |            |
| Results | 0.40            | 0.36           | 0.0001           | 0.761         | 5            | 1.2          | 1.09        | 3          |

<br />

*\*addresses designated as market maker are not eligible for the referral program.*