# Close User Data Stream (USER_STREAM)

## API Description

Close out a user data stream.

## HTTP Request

```
DELETE /eapi/v1/listenKey
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

- Doing a DELETE on a listenKey will close the stream and invalidate the listenKey
- This is valid for both options and futures/vanilla options user data streams
- After closing, you will no longer receive any user data updates on that stream

## Related Documentation

- [Start User Data Stream](/docs/derivatives/options-trading/user-data-streams/Start-User-Data-Stream)
- [Keepalive User Data Stream](/docs/derivatives/options-trading/user-data-streams/Keepalive-User-Data-Stream)
