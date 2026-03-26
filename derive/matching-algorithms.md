# Matching Algorithms

Derive supports both FIFO (a.k.a. price/time) and pro-rata matching algorithms, as well as "blends" thereof (e.g. a % of the order being matched FIFO and a % pro-rata).

### FIFO

In FIFO matching, resting orders are ranked by price first, then by the order creation time. Order with the smallest creation timestamp gets top priority amongst all orders of the same price.

### Pro-rata

In pro-rata matching, resting orders are ranked by price first. If several orders have the same price, then the share of the incoming taker order they get is determined pro-rata by the orders' sizes.

For example, suppose Alice and Bob are quoting 10 and 30 contracts respectively at a price of $150, and Charlie sends a taker order of size 20. Alice's share of the totals size is `10 / (10 + 30) = 0.25`, therefore her order gets filled `20 x 0.25 = 5` contracts. Bob's share is `30 / (10 + 30) = 0.75` so he gets filled the remaining `20 x 0.75 = 15`.

### FIFO & pro-rata blend

The above 2 algorithms are building blocks of Derive's FIFO & pro-rata blend. At a high level, the algorithm performs the following 3 steps at every price level:

1. FIFO pass: a certain % or size of an order gets routed through regular FIFO.
2. Pro-rata pass: the remainder gets filled pro-rata, some rounding is applied to the fill share of every participating order.
3. FIFO cleanup pass: the part of the order that still remains unfilled due to rounding performed in (2) gets routed FIFO.

There are 3 parameters governing the degree of this blend (available in `get_instrument` and `get_ticker` payloads and channels)

1. `pro_rata_fraction` determines the maximum % of the order's size that can get filled pro-rata. If this number is zero, the algorithm is equivalent to full FIFO.
2. `fifo_min_allocation` determines the minimum order size threshold that will get routed through FIFO no matter what the parameter in (1) is set to. This adds an incentive for market makers to better the current market, since they are the first to improve the price, small flow will go more towards them. For example, if this value is 5 and `pro_rata_fraction` is 80%, an order of size 10 will have a size of `max(5, 10 x 20%) = 5` routed through FIFO and the remaining 5 through pro-rata.
3. `pro_rata_amount_step` determines the rounding of the fill shares under pro-rata, for example suppose this value is 1, and if Alice's pro-rata share of an order is 25%, and the order is of size 5. Then Alice's unrounded share of the order is 1.25, which gets rounded down to 1. This rounding would happen for every pro-rata fill participant, and the unfilled portions (the "rounding errors") are added up and routed FIFO.

## Algorithms for Products

1. Perpetuals are matched through regular FIFO
2. Options are matched through the blend algorithm with the following parameters:

| Parameter              | ETH | BTC |
| :--------------------- | :-- | :-- |
| `pro_rata_fraction`    | 0.8 | 0.8 |
| `fifo_min_allocation`  | 10  | 1   |
| `pro_rata_amount_step` | 1   | 0.1 |