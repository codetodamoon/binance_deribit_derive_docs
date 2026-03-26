# BookTicker

## Stream Description

Pushes any update to the best bid or ask's price or quantity in real-time for a specified symbol.

## URL Path

`/public`

## Stream Name

`<symbol>@bookTicker`

## Update Speed

Real-Time

## Response Example

```json
{
    "e": "bookTicker",             // event type
    "u": 2472,                     // order book updateId
    "s": "BTC-251226-110000-C",    // symbol
    "b": "5000.000",               // best bid price
    "B": "0.2000",                 // bid bid quantity
    "a": "5100.000",               // best ask price
    "A": "0.1000",                 // best ask quantity
    "T": 1763041762942,            // transaction time
    "E": 1763041762942             // event time
}
```

### Parameter Descriptions

| Field | Type | Description |
|-------|------|-------------|
| e | string | Event type |
| u | integer | Order book updateId |
| s | string | Symbol |
| b | string | Best bid price |
| B | string | Best bid quantity |
| a | string | Best ask price |
| A | string | Best ask quantity |
| T | integer | Transaction time |
| E | integer | Event time |
