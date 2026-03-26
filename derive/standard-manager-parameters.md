# Standard Margin Parameters

This page documents the up to date values of all market specific parameters for the standard manager.

# Account Details

These parameters govern the size of standard margin subaccounts and whether or not they can borrow against supported base assets.

| Parameter        | Contract Variable | Value | Range          | Description                                                                                                                                     |
| :--------------- | :---------------- | :---- | :------------- | :---------------------------------------------------------------------------------------------------------------------------------------------- |
| `BORROW_ENABLED` | `borrowEnabled`   | TRUE  | \[TRUE, FALSE] | If TRUE, standard margined subaccounts can enter into a negative cash balance by borrowing against their base asset.                            |
| `maxAccountSize` | `maxAccountSize`  | 48    | No bounds      | This represents the maximum number of assets (options, base asset, cash, perpetuals) that can be held by a single standard margined subaccount. |

# Delta-1 Margin Parameters

These govern the margin requirements for delta-1 instruments (base assets and perpetual futures).

| Parameter             | Contract Variable                  | ETH    | BTC   | SOL  | DOGE | Range        | Description                                                                                              |
| :-------------------- | :--------------------------------- | :----- | :---- | :--- | :--- | :----------- | :------------------------------------------------------------------------------------------------------- |
| `BASE_DISCOUNT`       | `baseMargin.marginFactor`          | 0.8    | 0.75  | NA   | NA   | \[0.0, 0.99] | Discount to the collateral provided by the base asset.                                                   |
| `BASE_DISCOUNT_SCALE` | `baseMargin.IMFactor`              | 0.9375 | 0.93  | NA   | NA   | \[0.0, .99]  | A scaling of BASE\_DISCOUNT when computing the collateral provided by the base asset for initial margin. |
| `PERP_REQ_MM`         | `PerpMarginRequirements.mmPerpReq` | 0.065  | 0.065 | 0.2  | 0.33 | \[0.0, 1.0]  | The percentage of the spot price per perpetual contract required to be posted for maintenance margin.    |
| `PERP_REQ_IM`         | `PerpMarginRequirements.imPerpReq` | 0.10   | 0.10  | 0.33 | 0.50 | (0.0, 1.0]   | The percentage of the spot price per perpetual contract required to be posted for initial margin.        |

# Isolated Option Margin Parameters

These parameters govern the margin requirement for isolated short option positions.

<Table align={["left","left","left","left","left","left"]}>
  <thead>
    <tr>
      <th>
        Parameter
      </th>

      <th>
        Contract Variable
      </th>

      <th>
        ETH
      </th>

      <th>
        BTC
      </th>

      <th>
        Range
      </th>

      <th>
        Description
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        `SHORT_OPTION_IM_MAX`
      </td>

      <td>
        `OptionMarginParams.maxSpotReq`
      </td>

      <td>
        0.15
      </td>

      <td>
        0.15
      </td>

      <td>
        This page d
      </td>

      <td>
        The maximum percentage of the spot price required as additional initial margin for a short call/put.

        Examples provided at the end of [Standard Margin](https://docs.derive.xyz/docs/standard-margin).
      </td>
    </tr>

    <tr>
      <td>
        `SHORT_OPTION_IM_MIN`
      </td>

      <td>
        `OptionMarginParmas.minSpotReq`
      </td>

      <td>
        0.13
      </td>

      <td>
        0.13
      </td>

      <td>
        This page d
      </td>

      <td>
        The minimum percentage of the spot price required as additional initial margin for a short call/put.
      </td>
    </tr>

    <tr>
      <td>
        `CALL_MM`
      </td>

      <td>
        `OptionMarginParmas.mmCallSpotReq`
      </td>

      <td>
        0.09
      </td>

      <td>
        0.09
      </td>

      <td>
        This page
      </td>

      <td>
        A constant percentage of the spot price required as additional maintenance margin for a short call.
      </td>
    </tr>

    <tr>
      <td>
        `PUT_MM`
      </td>

      <td>
        `OptionMarginParmas.mmPutSpotReq`
      </td>

      <td>
        0.09
      </td>

      <td>
        0.09
      </td>

      <td>
        This page
      </td>

      <td>
        A constant percentage of the spot price required as additional maintenance margin for a short put.
      </td>
    </tr>

    <tr>
      <td>
        `PUT_MTM`
      </td>

      <td>
        `OptionMarginParmas.mmPutMTMReq`
      </td>

      <td>
        0.09
      </td>

      <td>
        0.09
      </td>

      <td>
        This page
      </td>

      <td>
        A constant percentage of the put's mark-to-market value required as additional maintenance margin for a short put.
      </td>
    </tr>

    <tr>
      <td>
        `MTM_OFFSET`
      </td>

      <td>
        `OptionMarginParams.mmOffsetScale`
      </td>

      <td>
        1.05
      </td>

      <td>
        1.05
      </td>

      <td>
        This page
      </td>

      <td>
        A scaling of the mark-to-market value of the put used to compute initial margin.
      </td>
    </tr>
  </tbody>
</Table>

# Spread Margin Parameters

These parameters govern the margin requirements for sub-portfolios with naked short calls and spreads.

| Parameter           | Contract Variable                    | ETH | BTC | Range        | Description                                                                                                       |
| :------------------ | :----------------------------------- | :-- | :-- | :----------- | :---------------------------------------------------------------------------------------------------------------- |
| `UNPAIRED_SCALE_IM` | `OptionMarginParams.unpairedIMScale` | 1.2 | 1.2 | \[1.01, 3.0] | A large percentage of the forward price per "naked" short call  per expiry is added to the offset initial margin. |
| `UNPAIRED_SCALE_MM` | `OptionMarginParams.unpairedMMScale` | 1.1 | 1.1 | \[1.01, 3.0] | As above but for maintenance margin.                                                                              |

# Stablecoin Margin Parameters

These control the extra initial margin requirements when USDC (the designated cash asset) depegs.

| Parameter        | Contract Variable         | ETH  | BTC  | SOL  | DOGE | Range        | Description                                                                                                                                                                           |
| :--------------- | :------------------------ | :--- | :--- | :--- | :--- | :----------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `DEPEG_FACTOR`   | `DepegParams.depegFactor` | 2.0  | 2.0  | 2.0  | 2.0  | \[0, 10]     | Each short option and perpetual held by the subaccount attracts extra initial margin that scales with `DEPEG_FACTOR` and the difference between the USDC price and a given threshold. |
| `USDC_THRESHOLD` | `DepegParams.threshold`   | 0.99 | 0.99 | 0.99 | 0.99 | \[0.0, 1.05] | Value of USDC beneath which extra depeg contingency will begin to be added.                                                                                                           |

# Confidence Margin Parameters

These control the extra initial margin requirements when any of the data feeds have low confidence.

| Parameter              | Contract Variable                                                                                                     | ETH  | BTC  | SOL  | DOGE | Range     | Description                                                                                                         |
| :--------------------- | :-------------------------------------------------------------------------------------------------------------------- | :--- | :--- | :--- | :--- | :-------- | :------------------------------------------------------------------------------------------------------------------ |
| `CONFIDENCE_SCALE`     | `OracleContingencyParams.OCFactor`                                                                                    | 1.0  | 1.0  | 1.0  | 1.0  | \[0, 2.0] | Percentage of the spot price per short option, base or perpetual contract added to the initial margin requirements. |
| `THRESHOLD_CONFIDENCE` | `OracleContingencyParams.perpThreshold OracleContingencyParams.optionThreshold OracleContingencyParams.baseThreshold` | 0.55 | 0.55 | 0.55 | 0.55 | \[0, 1.0] | Value of the confidence beneath which extra IM is added.                                                            |

# Open Interest Caps

Open interest caps on options, base and perpetual instruments.

| Parameter           | Contract Variable            | ETH       | BTC     | SOL  | DOGE      | Range                | Description                                    |
| :------------------ | :--------------------------- | :-------- | :------ | :--- | :-------- | :------------------- | :--------------------------------------------- |
| `UNDERLYING_OI_CAP` | `baseAsset.totalPositionCap` | 250       | 5       | NA   | NA        | \[0, no upper bound] | Maximum open interest of the underlying asset. |
| `OPTION_OI_CAP`     | `option.totalPositionCap`    | 2,000,000 | 100,000 | NA   | NA        | \[0, no upper bound] | Maximum open interest of options.              |
| `PERP_OI_CAP`       | `perp.totalPositionCap`      | 250,000   | 12,000  | 8000 | 5,000,000 | \[0, no upper bound] | Maximum open interest of perpetuals.           |

# Fees

These are fees charged by the managers on the protocol layer.

| Parameter     | Contract Variable      | ETH       | BTC       | SOL       | DOGE      | Range        | Description                                                                       |
| :------------ | :--------------------- | :-------- | :-------- | :-------- | :-------- | :----------- | :-------------------------------------------------------------------------------- |
| `SPOT_FACTOR` | `manager.OIFeeRateBPS` | 0.7 (70%) | 0.7 (70%) | 0.7 (70%) | 0.7 (70%) | \[0, 5.0]    | Percentage of the spot price charged  when the trade increases the open interest. |
| `MIN_OI_FEE`  | `minOIFee`             | $800 USDC | $800 USDC | $800 USDC | $800 USDC | \[0, 10,000] | Minimum fee charged when open interest is increased.                              |