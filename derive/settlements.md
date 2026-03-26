# Settlements

All options are settled to a time-weighted-average-price (TWAP) of the spot (index) price over `SETTLEMENT_TWAP_PERIOD = 30` minutes. In the lead up to settlement, the option is marked to a linear combination of the TWAP and forward.

Specifically:

* If `Time To Expiry > SETTLEMENT_TWAP_PERIOD`, then the option is marked using the forward price.
* If `0 < Time To Expiry < SETTLEMENT_TWAP_PERIOD` then the options are marked to a weighted sum of the forward price and average spot price. Spot data is read in once per minute and is used to compute:

  ```python Formula
  S_avg(n) = (S_1 + ... S_n)/n
  ```

  where `S_i` is the spot price recorded at the `i`th minute. If there are `n` minutes until expiry and `SETTLEMENT_TWAP_PERIOD - n ≥ 0`, then the price used to mark all options of this expiry is given by:

  ```python Formula
  S_mark = (SETTLEMENT_TWAP_LENGTH - n) / SETTLEMENT_TWAP_LENGTH * Forward Price
  		+ n / SETTLEMENT_TWAP_LENGTH * S_avg(n)
  ```
* At expiration, the option is settled to `S_avg(30)`.

# Example

Consider an option expiring on December 1 2024 at 20:00.

* Up until, the price used to mark all options expiring at this time will be the forward price `F`.
* At 19:40 (20 minutes to expiration), then we compute
  ```
  S_avg(10) = (S_1 + S_2 + ... + S_10)/10
  ```
* Here, `S_1` is the spot price recorded at 19:31, `S_2` is the spot price recorded at 19:32 and so forth.
* The option is marked to
  ```
  S_mark = (20/30) * F + (10/30) * S_avg(10)
  ```
* Anytime after 20:00, the option will be settled and marked to `S_avg(30)`

# Delta Decay

A consequence of options settling to a TWAP of spot is so called "delta decay". In the 30 minutes leading up to settlement, the underlying price used to mark options will be a linear combination of the average spot price and current forward. This essentially fixes some component of the underlying price. As a consequence, the option's rate of change with respect to underlying movements (its delta) will decrease. We call this **delta decay**.

A good example of this will be a very in-the-money put option. Suppose you have a $10,000 strike ETH put when IV = 50% and the ETH forward and spot are both trading at $1650. The delta of this option is practically 1.

Come 15 minutes to expiration, the option will be marked to an equally weighted combination of the average spot price over the last 15 minutes (say, $1646) and the current forward ($1652), i.e. the option will be marked to an underlying of (1/2) x $1646 + (1/2) x $1652 = $1649.\
At this stage, half of the settlement price has already been printed, meaning each $1 change in spot only has half the effect it normally would have. In other words, the delta of this put has decayed from 1.0 to 0.5.

This is an important phenomena for users of portfolio margin to understand, since net delta exposure is used to compute their margin requirements.

For standard margin subaccounts delta decay also has an effect on the margin requirement for shorts (since the mark-to-market value of the short becomes more "fixed" as settlement approaches).

**Note**:  Because options settle to a TWAP of the spot price, it is possible for a trader selling a covered call (with a near zero strike price) using portfolio margin to be liquidated. This is because as the delta of the option approaches 0 (as time to expiry approaches 0), a position that was previously delta neutral will no longer be so. For the vast majority of users, this is an incredibly unlikely outcome. Again, we re-iterate that it is only really possible for covered calls whose strike is near 0.