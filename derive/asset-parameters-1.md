# Asset Parameters

# Option Settlement

This parameter control the duration of the time-weighted-average-price (TWAP) used to settle options.

| Parameter        | Contract Variable          | Value      | Bounds    | Description                                                            |
| :--------------- | :------------------------- | :--------- | :-------- | :--------------------------------------------------------------------- |
| SETTLEMENT\_TWAP | `SETTLEMENT_TWAP_DURATION` | 30 minutes | No bounds | Length of the TWAP of the spot price to which all options will settle. |

# Perpetual Asset

These control the pricing of perpetual assets, as well as how the associated funding rate is computed.

<Table align={["left","left","left","left","left"]}>
  <thead>
    <tr>
      <th>
        Parameter
      </th>

      <th>
        Contract Variable
      </th>

      <th>
        Value
      </th>

      <th>
        Bounds
      </th>

      <th>
        Description
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        IMPACT\_NOTIONAL
      </td>

      <td>
        N/A (a relevant parameter, but not on the contract layer)
      </td>

      <td>
        4000
      </td>

      <td>
        No bounds
      </td>

      <td>
        This controls the perpetual price slippage when computing the funding rate.
      </td>
    </tr>

    <tr>
      <td>
        PERP\_TWAP\_LENGTH
      </td>

      <td>
        N/A (a relevant parameter, but not on the contract layer)
      </td>

      <td>
        30 minutes
      </td>

      <td>
        No bounds
      </td>

      <td>
        This is the length of the TWAP of the difference between spot and perpetual price. The perpetual itself is marked to the spot price plus this TWAPed difference.
      </td>
    </tr>

    <tr>
      <td>
        PERP\_MAX\_PERCENT\_DIFF
      </td>

      <td>
        `feed.spotDiffCap`
      </td>

      <td>
        6%
      </td>

      <td>
        No bounds
      </td>

      <td>
        This is the maximum percentage of the spot price the perpetual can be marked to. I.e. the perpetual cannot be marked any higher than 5% higher/lower than the current spot price.
      </td>
    </tr>

    <tr>
      <td>
        PERP\_STATIC\_RATE
      </td>

      <td>
        `staticInterestRate`
      </td>

      <td>
        0.0000125 (i.e. 0.00125%) per hour
      </td>

      <td>
        No bounds
      </td>

      <td>
        This is a static amount of funding paid from longs to shorts per hour.
      </td>
    </tr>

    <tr>
      <td>
        PERP\_MAX\_FUNDING
      </td>

      <td>
        `maxRatePerHour`
      </td>

      <td>
        400% annualized (ETH/BTC/HYPE/SOL) and 600% for others.
      </td>

      <td>
        No bounds
      </td>

      <td>
        This is the absolute value of the maximum funding rate per hour. I.e. the maximum funding paid from longs to shorts per hour is 400% annualized.
      </td>
    </tr>

    <tr>
      <td>
        PERP\_MIN\_FUNDING
      </td>

      <td>
        `minRatePerHour`
      </td>

      <td>
        -400% annualized (ETH/BTC/HYPE/SOL) and -600% for others.
      </td>

      <td>
        No bounds
      </td>

      <td>
        This is the absolute value of the minimum funding rate per hour. I.e. the minimum funding paid from longs to shorts per hour is -400% annualized.

        Must be the negative of `PERP_MAX_FUNDING`.
      </td>
    </tr>

    <tr>
      <td>
        CONVERGENCE\_PERIOD
      </td>

      <td>
        `fundingConvergencePeriod`
      </td>

      <td>
        8
      </td>

      <td>
        No bound
      </td>

      <td>
        This scales the difference between the spot (index) price and relevant impact prices (bid/ask) when computing the funding rate.
      </td>
    </tr>
  </tbody>
</Table>

# Volatility Feed/Option Asset

These parameters control the pricing and marking of the option asset.

<Table align={["left","left","left","left","left"]}>
  <thead>
    <tr>
      <th>
        Parameter
      </th>

      <th>
        Contract Variable
      </th>

      <th>
        Value
      </th>

      <th>
        Bounds
      </th>

      <th>
        Description
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        SVI\_MAX\_SCALE
      </td>

      <td>
        `SVI_MAX_SCALE`
      </td>

      <td>
        4
      </td>

      <td>
        No Bounds
      </td>

      <td>
        This is used to set the strikes after which volatility from the SVI curve is flattened out.

        Not configurable post launch
      </td>
    </tr>

    <tr>
      <td>
        SVI\_MIN\_SCALE
      </td>

      <td>
        `SVI_MIN_SCALE`
      </td>

      <td>
        -4
      </td>

      <td>
        No Bounds
      </td>

      <td>
        As above
      </td>
    </tr>

    <tr>
      <td>
        MAX\_TOTAL\_VOL
      </td>

      <td>
        `MAX_TOTAL_VOL`
      </td>

      <td>
        24.0
      </td>

      <td>
        No Bounds
      </td>

      <td>
        This is the maximum value of the total variance that can be used in the Black76 formula

        Not configurable post launch
      </td>
    </tr>

    <tr>
      <td>
        MAX\_TOTAL\_VAR
      </td>

      <td>
        `MAX_TOTAL_VAR`
      </td>

      <td>
        144.0
      </td>

      <td>
        No Bounds
      </td>

      <td>
        This is the maximum value of the total implied variance that can be used in the Black76 formula.

        Not configurable post launch
      </td>
    </tr>

    <tr>
      <td>
        MAX\_EXPIRY
      </td>

      <td>
        `MAX_EXPIRY`
      </td>

      <td>
        400 days
      </td>

      <td>
        No bounds
      </td>

      <td>
        This is the longest dated expiry that can be supported.

        Not configurable post launch
      </td>
    </tr>

    <tr>
      <td>
        INTEREST\_RATE
      </td>

      <td>
        `staticRateFeed.rate`
      </td>

      <td>
        0.00
      </td>

      <td>
        No Bounds
      </td>

      <td>
        Interest rate used for discounting.
      </td>
    </tr>
  </tbody>
</Table>

# HeartBeats

Each feed has an associated heartbeat. If data is not received for longer than the associated heartbeat, all trades will revert until said data is received.

| Feed                                              | Value      | Justification                                                                                                       |
| :------------------------------------------------ | :--------- | :------------------------------------------------------------------------------------------------------------------ |
| Forward (difference from spot)                    | 60 minutes | Forward difference to spot shouldn’t be too large, so a long heartbeat exposes minimal risk                         |
| Forward (settlement period)                       | 10 minutes | Settlement period is 30 minutes, so not receiving data for a third of this time should warrant a revert.            |
| Spot                                              | 20 minutes | Prices are sensitive to spot.                                                                                       |
| Volatility                                        | 30 minutes | Volatility doesn’t move too quickly and prices are not as sensitive (compared to spot)                              |
| Perpetual (difference from spot)                  | 30 minutes | This is the difference between spot and the perpetual mark price, so should be small in the vast majority of times. |
| Perpetual (used to compute impact ask/bid prices) | 30 minutes | This only determines funding which is implicitly TWAPed, so a longer heartbeat is ok.                               |
| Stable feed (USDC)                                | 60 minutes | Feed will most of the time not move, but needs to be responsive when it does                                        |