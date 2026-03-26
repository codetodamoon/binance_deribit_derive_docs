# How to Manage a Local Order Book Correctly

## Binance Options Trading - WebSocket Market Streams

This guide explains the correct process for maintaining a local order book using WebSocket streams and REST API snapshots.

### Stream Connection

Connect to the WebSocket stream:

```
wss://fstream.binance.com/public/stream?streams=btc-200630-9000-p@depth@100ms
```

### Depth Snapshot

Retrieve the initial depth snapshot from the REST API:

```
https://eapi.binance.com/eapi/v1/depth?symbol=btc-200630-9000-p&limit=1000
```

### Order Book Management Steps

1. Open a stream to **wss://fstream.binance.com/public/stream?streams=btc-200630-9000-p@depth@100ms**.
2. Buffer the events you receive from the stream. For same price, latest received update covers the previous one.
3. Get a depth snapshot from **https://eapi.binance.com/eapi/v1/depth?symbol=btc-200630-9000-p&limit=1000**.
4. Drop any event where `u` is < `lastUpdateId` in the snapshot.
5. The first processed event should have `U` `<= lastUpdateId` **AND** `u` `>= lastUpdateId`

**Parameter definitions:**
- U = firstUpdateId (the first update ID from the WebSocket stream)
- u = finalUpdateId (the last update ID from the WebSocket stream)
- lastUpdateId = the update ID from the REST depth snapshot

6. While listening to the stream, each new event's `pu` should be equal to the previous event's `u`, otherwise initialize the process from step 3.
7. The data in each event is the **absolute** quantity for a price level.
8. If the quantity is 0, **remove** the price level.
9. Receiving an event that removes a price level that is not in your local order book can happen and is normal.

### Related Documentation

- Previous: Diff Book Depth Streams
- Next: Connect
