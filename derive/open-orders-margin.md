# Orderbook Margin

# Maintenance Margin

The orderbook calculates maintenance margin in accordance with the protocol rules, and makes the values available over various endpoints such as `get_subaccount`. For more details refer to the [standard](https://docs.lyra.finance/docs/standard-margin-1) and [portfolio](https://docs.lyra.finance/docs/portfolio-margin-1) margin sections.

# Initial Margin

Similarly, the orderbook calculates initial margin using protocol rules, and ensures that no trade gets sent for settlement if the margin value after trade would be insufficient. Refer to the aforementioned sections for more detail.

# Open Orders Margin

Limit orders that stay open in the book require that the account has extra margin to cover them if they were to get filled.

The orderbook backend will inspect account's open orders`[order_1, order_2,...]`and find a "worst subset" of these orders, where "worst" is defined as a set of orders that, if filled, leads to the smallest *initial margin* possible. While performing those simulated fills, the backend will take into account the premiums paid or received for option bids and asks, as well as the current positions owned by the account.

For example, suppose the open orders and positions are:

* Orders:`[bid 10 perps @ $1999, ask 100 perps @ $2001, bid 10 1w calls @ $55, bid 5 2w calls @ $75]`
* Positions:`[long 90 perps]`

The backend will try and group the orders by their delta and / or vega sign and arrive at a conclusion that `[bid 10 perps @ $1999, bid 10 calls @ $55, bid 5 2w calls @ $75]` is the worst fill scenario. The open order margin for these orders will then be calculated by finding how much *extra initial margin* would the account require if those orders were to get filled.

For every new open (i.e. non-crossing) order that arrives to the orderbook, the risk engine checks if the sum of current initial margin and the open orders margin is non-negative. In other words, new orders are accepted as long as the account can honour the "worst fill" scenario.

The `private/get_subaccount` endpoint can be used to check which orders have been flagged as "worst subset" and how much open orders margin they require.

## Market Maker Protections (MMPs) and Open Orders Margin

Oftentimes portfolio margin users would be market makers quoting hundreds of assets at the same time. If they have tight MMP limits, it is impossible for them to get filled on all of these quotes simultaneously, so it would be unreasonably capital inefficient to require them to lock margin for very large subsets of orders.

Therefore, for portfolio margin accounts, the process of finding the "worst subset" is constrained by account's market maker protection settings. *If MMP amount limits are enabled*, the "worst subset" of orders would be reduced to an orders subset which can be filled subject to staying within MMP amount limit. Note that the reduced subset cannot be smaller than 2 distinct assets, i.e. the smallest possible open orders margin requirement still enforces that the market maker can honour at least 2 fills on two of the "worst" assets they are quoting.

Using the above example - if MMP amount limit was set to 3, then the worst orders subset would exclude the `bid 5 2w calls @ $75` and would just consist of `[bid 10 perps @ $1999, bid 10 calls @ $55]`, because at least 2 assets have to be fillable by the account.

If the MMP limit was too high (e.g. 30), then the subset would remain unchanged.

Finally note that only MMP *amount* limit supports this capital efficiency improvement, the *delta* limit is ignored. More info on the MMPs can be found in the API reference under `private/set_mmp_config`.