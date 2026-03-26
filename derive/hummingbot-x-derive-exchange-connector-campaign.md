# Hummingbot x Derive Exchange Connector Campaign

This campaign is open-ended and designed to reward algo traders who help grow the liquidity and trading volume on Derive. The program includes incentives for both simple market-making setups and more advanced funding rate strategies.

## Description

Integrate the Derive exchange connector with your Hummingbot client and earn rewards by contributing to market liquidity or capturing arbitrage opportunities on Derive’s spot or perpetual markets.

***

## How to Participate

1. Connect to Derive using the Hummingbot connector. Integration guides are available [here](https://hummingbot.org/blog/running-a-trading-bot-with-hummingbot-on-derive/).
2. Submit your registration using [the official signup form](https://forms.gle/8od3ZzrxPHfjk9haA). Each user must register a unique Derive wallet address and Hummingbot instance ID.
3. Run your strategy on Derive’s perpetual [markets](https://www.derive.xyz/perps/eth) (spot or perps).
4. Once your bot generates at least $150 in total trading fees (long or short), you’ll become eligible for the reward.

***

## Incentives

<Table align={["left","left","left"]}>
  <thead>
    <tr>
      <th>
        Action
      </th>

      <th>
        Reward
      </th>

      <th>
        Details
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        Run a bot using the Derive connector and:

        * Tier 1: Generate $25 in fees or
        * Tier 2: Generate $150 in fees
      </td>

      <td>
        Tier 1: 100 OP or\
        Tier 2: 400 OP
      </td>

      <td>
        Limited to first come first serve
      </td>
    </tr>

    <tr>
      <td>
        Rank in the top tier of the trading volume leaderboard
      </td>

      <td>
        Eligibility for Derive's market maker rebate program
      </td>

      <td>
        Optional opt-in
      </td>
    </tr>
  </tbody>
</Table>

***

## Strategy Options

**Simple Quoting Bot**\
Deploy a basic quoting bot that provides passive liquidity on Derive. A sample script is available that quotes around the mark price. Ideal for traders looking to get started quickly.

**Arbitrage & Directional Strategies**\
Run delta-neutral or directional strategies that profit from spread capture, price inefficiencies, or funding rate mispricings. Strategies can be deployed on spot or perps. All fee-generating trades qualify toward the reward.

***

## Eligibility Criteria

* Campaign start: Ongoing
* End date: To be announced with at least 5 days’ notice
* Rewards are limited to the first 20 verified participants who reach the $150 fee threshold
* One reward per person, enforced via unique wallet address and Hummingbot instance ID
* Only trading activity verified to be executed via Hummingbot will be eligible
* Participants must register via the signup form prior to trading

***

## How We Verify Hummingbot Usage

To qualify for the reward:

* You must connect via the official Hummingbot connector (`derive` or `derive_perpetual`)
* Include your Hummingbot instance ID in the signup form
* Our team will validate trades against internal logs to confirm they originated from Hummingbot
* Additional checks may include strategy patterns, trade frequency, and order metadata

***

## Resources

* [Derive Exchange Connector on Hummingbot](https://hummingbot.org/exchanges/derive/)
* [Connector Setup Guide](https://hummingbot.org/blog/running-a-trading-bot-with-hummingbot-on-derive/)
* [Derive Developer API Docs](https://docs.derive.xyz/reference/overview)
* [Perpetual Trading Fees](https://docs.derive.xyz/reference/fees-1)

***

## Terms

All participants must comply with Derive’s [Terms of Use](https://www.derive.xyz/terms-of-use). Derive reserves the right to verify and validate all trading activity. Submissions that appear to be manipulative or in violation of campaign rules will be disqualified.