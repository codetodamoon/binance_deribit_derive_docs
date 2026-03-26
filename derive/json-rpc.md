# JSON-RPC

[JSON-RPC](https://www.jsonrpc.org/specification) is a standard RPC (remote procedure call) protocol widely used both by the exchanges and the native Ethereum ecosystem. Derive API is built on top of this specification with minor adjustments.

This API protocol is transport agnostic: it can be used both over Websockets and HTTP. The request parameters and method names are equivalent over both protocols, however HTTP does not support subscriptions.

## WebSocket

In WebSockets, clients must send messages as JSON objects in the with the following fields:

```json
{
  "id": string,
  "method": string,
  "params": object
}
```

**NOTE** the WebSocket URL is `wss://api-demo.lyra.finance/ws`

## HTTP Post

When used with HTTP POST requests, client must send messages to the endpoint's specified url with the appropriate path, and attach a POST payload that matches the `params` schema. See REST API for examples.

When used with HTTP GET requests, client must send messages over the same base url, and provide the `params` object via url-encoded query parameters. See REST API for examples.

Regardless of the chosen transport protocol, the server will always send back messages in the form:

```json JSON
{
  "id": string,
  "result": object
}
```

if the RPC call was successful, or:

```json JSON
{
  "id": string,
  "error": {
    "code": number,
    "message": string, 
    "data": string
  }
}
```

if the RPC call resulted in an error. [RPC Error Codes](https://docs.derive.xyz/reference/error-codes) are shared across HTTP and Websocket APIs.