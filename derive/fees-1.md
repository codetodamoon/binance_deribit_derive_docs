# Fees

## Orderbook

Fees on Derive differ if you are are maker (i.e. you put out a resting limit order that is filled) or if you are a taker (i.e. you buy or sell against an existing order). The fees also differ by instrument (options vs perps), and are set out in the table below:

| Instrument | Maker                   | Taker                          |
| :--------- | :---------------------- | :----------------------------- |
| Spot       | No maker fees.          | No taker fees.                 |
| Perp       | 0.01% x notional volume | $0.1 + 0.03% x notional volume |
| Option     | 0.01% x notional volume | $0.5 + 0.03% x notional volume |

Option notional fees are capped at 12.5% of the value of the option.

Note that takers pay an additional `base_fee` of $0.10 (Perps) or $0.5 (Options) per trade regardless of their trade size. This fee is waived for verified market maker accounts.

Examples:

1. Alice buys 2 ETH 2,000 puts using an aggressive order, and the oracle spot price is $2200:\
   `fee = $0.5 + 0.03% * 2 * $2200 = $1.82`
2. Bob opens a 0.1 BTC perp sell limit order and later gets filled by Charlie, with spot at $43,000:\
   `feeBob = 0.01% * 0.1 * $43,000 = $0.43`\
   `feeCharlie = $0.1 + 0.03% * 0.1 * $43,000 = $1.39`

## RFQs

Trades conducted via RFQs get charged the taker notional fee rate to both counterparties (and a base fee to the taker side). Additionally, multi-leg trades **enjoy up to 100% discounts** on the cheaper of the legs based on the below explained set of rules. In summary, for the most common use cases:

* 2-leg option spreads like straddles, verticals, calendars, etc. get charged zero fee on their second leg
* Hedged options (option + perpetual) get charged zero fee on the cheapest between the perp and the option leg

For more complicated trades, the following rules apply. All legs of an RFQ get grouped into `long calls`, `long puts`, `short calls`, `short puts`, `perps`, and total fee is calculated within each group. The full fee is always charged on the most expensive group, no matter how many groups there are. The trades in the remaining groups get a fee discount applied to them in the following order:

1. Cheapest group gets 100% discount
2. Second and third cheapest group gets 50% discount
3. Other groups do not get any further discounts

For example:

1. A call spread has 2 legs, and the two legs belong to different groups (one in `long calls` one in `short calls`). The cheapest leg / group gets a 100% discount.
2. A straddle or a strangle has two legs, and the two legs belong to different groups. The cheapest leg gets a 100% discount.
3. Two long calls bought at different strikes or expiries both fall into the same `long calls` group and therefore have no discount applied to any of them.
4. A risk reversal with a perp hedge has 3 legs each falling in a different group. The cheapest leg gets 100% discount, second cheapest gets 50% discount, and the most expensive leg is paid in full.

### Box Spreads

Additionally, the system recognizes box spreads (a 4-legged trade with a long call and a short put at one strike, and a short call and a long put at another strike, all at the same expiry) as a special strategy with a different fee schedule. A box spread can be thought of as a zero coupon bond paying the notional `(strike_1 - strike_2)`dollars at expiry, and it typically trades at a discount to its notional.

Derive charges a "yield spread" fee for this "bond" equal to `notional x 1% x years_to_expiry`, e.g. for a box with strikes $4000 and $5000 ($1000 notional) and 1 month to expiry, the fee would be `$1000 x 1% x 1/12 = $0.83`. This fee is charged to both maker and taker side, plus a $0.5 base fee is charged to the taker side only.

<br />