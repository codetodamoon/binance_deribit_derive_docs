# Naming

Derive products use the following naming system:

<Table align={["left","left","left"]}>
  <thead>
    <tr>
      <th style={{ textAlign: "left" }}>
        Product
      </th>

      <th style={{ textAlign: "left" }}>
        Template
      </th>

      <th style={{ textAlign: "left" }}>
        Examples
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td style={{ textAlign: "left" }}>
        Quote / Base
      </td>

      <td style={{ textAlign: "left" }}>
        $TICKER
      </td>

      <td style={{ textAlign: "left" }}>
        ETH, BTC, USDC
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Perpetual
      </td>

      <td style={{ textAlign: "left" }}>
        $TICKER-PERP
      </td>

      <td style={{ textAlign: "left" }}>
        ETH-PERP
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Option
      </td>

      <td style={{ textAlign: "left" }}>
        $TICKER-$DAY$MONTH$YEAR-$STRIKE-C|P
      </td>

      <td style={{ textAlign: "left" }}>
        BTC-20250316-420-C,\
        BTC-20230916-580-P
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Spot
      </td>

      <td style={{ textAlign: "left" }}>
        $BASE-$QUOTE
      </td>

      <td style={{ textAlign: "left" }}>
        ETH-USDC, BTC-USDC
      </td>
    </tr>
  </tbody>
</Table>