# Event: User Data Stream Expired

## Event Type

`@@dataStream#expired`

## Description

This event is sent when the user data stream has expired. This occurs when the listenKey validity period has ended (after 60 minutes from the last keepalive or creation without a subsequent keepalive), or when the stream has been connected for 24 hours.

## WebSocket Event Message Format

This is a system event sent from the server to the client.

```json
{
  "e": "@dataStream#expired",
  "expiration": 1762855900452
}
```

## Event Parameters

| Field | Type | Description |
|-------|------|-------------|
| e | string | Event type identifier |
| expiration | integer | Timestamp when the stream expired (in milliseconds) |

## Action Required

Upon receiving this event, the client should:

1. Create a new listenKey by calling the [Start User Data Stream](/docs/derivatives/options-trading/user-data-streams/Start-User-Data-Stream) endpoint
2. Establish a new WebSocket connection using the new listenKey

## Notes

- This event signals the end of the current user data stream session
- A new listenKey must be obtained to continue receiving user data updates
- The previous listenKey is no longer valid after receiving this event
