# Oracles

For each market, there is a set of oracle inputs used for marking assets when computing margin and liquidations. This data is posted onchain for transparency and available via the public REST API. This data is provided by [Block Scholes](https://www.blockscholes.com/).

| Name               | Description                              |
| ------------------ | ---------------------------------------- |
| Spot Price         | Spot price of the market                 |
| Forward Price      | Forward price for given expiry           |
| Perp Price         | Market Price perp contract is trading at |
| Implied Volatility | Implied volatility for expiry, strike    |
| Risk Free Rate     | Annualised interest rate for USD         |

Associated with each feed is a confidence score which ranges between 0 (low confidence) and 1 (high confidence). Confidence scores are used to calculate oracle contingencies for both [Standard Margin](https://docs.derive.xyz/reference/standard-margin-1) and [Portfolio Margin](https://docs.derive.xyz/reference/portfolio-margin-1) accounts.

# Implied Volatility

The implied volatility for each strike in a given expiry is obtained from a (raw) Stochastic Volatility Inspired (SVI) curve. Such a surface is characterised by 5 parameters. The implied volatility `(IV)` for a given strike is given by

```python Formula
IV = sqrt((a + b * (rho * (k - m) + sqrt((k - m)**2 + e**2))) / tau)
```

Where:

* `a`, `b`, `rho`, `m`, `e` are parameters supplied by the data feed.
* `k = log(K / forward)` is the (natural) log moneyness of the given strike `K`.
* `tau` is time to expiry (in years).
* `Spot` is the spot price of the underlying base asset.

For large and near 0 strikes `K`, the minimum and maximum values of the log moneyness`k` is bounded between `k_min` and `k_max` where:

* `k_min = - MIN_SCALE * sqrt(a + b * sigma)`
* `k_max = MAX_SCALE * sqrt(a + b * sigma)`

and `(MIN_SCALE, MAX_SCALE) = (4,4)`.

In other words:

* if `k > k_max`, set `k = k_max` and
* if `k < k_min`, set `k = k_min`.

Essentially, the SVI curve flattens out past strikes 4 standard deviations (using ATM total implied volatility) away from the current spot.

Further, note that the maximum total implied volatility and total implied variance are capped to, respectively:

* `MAX_TOTAL_VOL = 24.0`
* `MAX_TOTAL_VAR = 144.0`