# Start User Data Stream (USER_STREAM)

## API Description

Start a new user data stream. The stream will close after 60 minutes unless a keepalive is sent. If the account has an active `listenKey`, that `listenKey` will be returned and its validity will be extended for 60 minutes.

## HTTP Request

```
POST `/eapi/v1/listenKey`
```

## Request Weight

**1**

## Request Parameters

None

## Response Example

```json
{
  "listenKey": "pqia91ma19a5s61cv6a81va65sdf19v8a65a1a5s61cv6a81va65sdf19v8a65a1",
  "expiration": 1762855900452
}
```

## Response Parameters

| Field | Type | Description |
|-------|------|-------------|
| listenKey | string | User data stream identifier |
| expiration | integer | ListenKey expiration timestamp in milliseconds |

## Notes

- The listenKey must be included in all subsequent WebSocket connections to receive user data updates
- Send a keepalive request every 60 minutes to maintain the stream
- The stream automatically closes after 60 minutes without keepalive
- If an existing listenKey is already active, calling this endpoint extends its validity by 60 minutes

## Related Endpoints

- **Keepalive**: `PUT /eapi/v1/listenKey` - Extend listenKey validity
- **Close**: `DELETE /eapi/v1/listenKey` - Close the user data stream
- **Connect**: WebSocket connection at `wss://estream.binance.com/ws/<listenKey>`
