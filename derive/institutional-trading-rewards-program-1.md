# Institutional Trading Rewards Program

The program outlined below and all numbers provided are subject to change

### Method Name

#### `public/get_instrument`

Get single instrument by asset name

### Parameters

| Name             | Type   | Required | Enum | Description     |
| :--------------- | :----- | :------- | :--- | :-------------- |
| instrument\_name | string | True     |      | Instrument name |

### Response

| Name                           | Type              | Required | Enum                                    | Description                                                                    |
| :----------------------------- | :---------------- | :------- | :-------------------------------------- | :----------------------------------------------------------------------------- |
| id                             | string or integer | True     |                                         |                                                                                |
| result                         | *object*          | True     |                                         |                                                                                |
| ➜  activation                  | integer           | True     |                                         | Timestamp at which to activate instrument                                      |
| ➜  amount\_step                | string            | True     |                                         |                                                                                |
| ➜  base\_currency              | string            | True     |                                         | Underlying currency of base asset (`ETH`, `BTC`, etc)                          |
| ➜  expiry                      | integer           | True     |                                         | Instrument expiry timestamp                                                    |
| ➜  instrument\_name            | string            | True     |                                         | Instrument name                                                                |
| ➜  instrument\_type            | string            | True     | `erc20`<br />`option`<br />`perp`<br /> | `erc20`, `option`, or `perp`                                                   |
| ➜  is\_active                  | boolean           | True     |                                         | If `True`: instrument is tradeable within `activation` and `expiry` timestamps |
| ➜  maker\_fee\_rate            | string            | True     |                                         | Percent of spot fee rate                                                       |
| ➜  mark\_price\_fee\_rate\_cap | string or null    | True     |                                         | Percent of option price fee cap, e.g. 12.5%, null if not applicable            |
| ➜  max\_price                  | string            | True     |                                         | Maximum valid limit\_price                                                     |
| ➜  maximum\_amount             | string            | True     |                                         | Maximum valid amount of contracts / tokens per trade                           |
| ➜  min\_price                  | string            | True     |                                         | Minimum valid limit\_price                                                     |
| ➜  minimum\_amount             | string            | True     |                                         | Minimum valid amount of contracts / tokens per trade                           |
| ➜  quote\_currency             | string            | True     |                                         | Underlying currency of quote asset (`USD` for perps, `USDC` for options)       |
| ➜  taker\_fee\_rate            | string            | True     |                                         | Percent of spot fee rate                                                       |
| ➜  tick\_size                  | string            | True     |                                         | Maximum num of decimals in the price                                           |
| ➜  option\_details             | *object*          | True     |                                         | `underlying`, `expiry`, `strike` `option_type`                                 |
| ➜ ➜  expiry                    | integer           | True     |                                         | Unix timestamp of expiry date (in seconds)                                     |
| ➜ ➜  index                     | string            | True     |                                         | Underlying settlement price index                                              |
| ➜ ➜  option\_type              | string            | True     | `C`<br />`P`<br />                      |                                                                                |
| ➜ ➜  strike                    | string            | True     |                                         |                                                                                |
|                                |                   |          |                                         |                                                                                |
| ➜  perp\_details               | *object*          | True     |                                         | TODO                                                                           |
| ➜ ➜  index                     | string            | True     |                                         | Underlying spot price index for funding rate                                   |
| ➜ ➜  max\_rate\_per\_hour      | string            | True     |                                         |                                                                                |
| ➜ ➜  min\_rate\_per\_hour      | string            | True     |                                         |                                                                                |
| ➜ ➜  static\_interest\_rate    | string            | True     |                                         |                                                                                |

### Example

```shell
# Shell example will be inserted here
```

```javascript
// JavaScript example will be inserted here
```

```python
# Python example will be inserted here
```

> The above command returns JSON structured like this:

```json
{
  "id": "example",
  "result": {
    "instrument_name": "ETH-PERP"
  }
}
```