# Keepalive User Data Stream (USER_STREAM)

## API Description

Keepalive a user data stream to prevent disconnection. The listenKey must be sent every 30 minutes to maintain the connection. This is valid for both options and futures/vanilla options user data streams.

## HTTP Request

```
PUT /eapi/v1/listenKey
```

## Request Weight

**1**

## Request Parameters

None

## Response Example

```json
{}
```

## Notes

- Sending a PUT request on a listenKey will extend its validity for another 60 minutes
- This mechanism is valid for all user data streams (Options and Futures/Vanilla Options)
- A single connection is only valid for 24 hours; expect to be disconnected at the 24 hour mark
- It is not necessary to call keepalive to extend the validity of the stream after 60 minutes; the connection will automatically close at that time

## Related Documentation

- [Start User Data Stream](/docs/derivatives/options-trading/user-data-streams/Start-User-Data-Stream)
- [Close User Data Stream](/docs/derivatives/options-trading/user-data-streams/Close-User-Data-Stream)
