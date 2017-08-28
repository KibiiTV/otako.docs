# Otako WebSocket Server Layout

### This contains documentation on how the gateway logic will work.
### This document does not contain API to Gateway connections, only Gateway to User connections.

# Gateway OPCodes

| Code | Name | Client Action | Description |
|------|------|------|-------------|
| 0 | Dispatch | Receive | dispatches an event |
| 2 | Heartbeat | Send/Receive | used for ping checking |
| 4 | Voice State Update | Send | used to join/move/leave voice channels **NOTE: NOT AN IMPLEMENTED FEATURE** |
| 6 | Voice Server Ping | Send | used for voice ping checking **NOTE: NOT AN IMPLEMENTED FEATURE** |
| 8 | Reconnect | Receive | used to tell clients to reconnect to the gateway |
| 10 | Invalid Session | Receive | used to notify client they have an invalid session id |
| 12 | Hello | Receive | sent immediately after connecting, contains heartbeat and server debug information |
| 14 | Heartbeat ACK | Receive | sent immediately following a client heartbeat that was received |

# Gateway Connections

Gateway authentication will be done via WebSocket Request Headers. 
Since a websocket connection is just a socket connection upgraded over the web (Look into how an HTTP Request works to learn about HTTP Requests). A WebSocket is an upgraded web request and instead of closing the socket connection (where the HTTP request is sent), it continues using that socket to send and recieve data from client to server, over the web. Thus, allowing us to have query strings, headers, and cookies.



Following connection, the gateway will authenticate a user and proceed to reply with either the **HELLO** or **AUTH_ERROR** event.
```json
{
    "e":"GATEWAY_ERROR",
    "d":{
        "message":"ERR_TOKEN_INVALID",
        "code":0
    },
    "op":10,
    "s":0
}
```
or with
```json
{
    "e":"GATEWAY_HELLO",
    "d":{
        "ping_inv":15432,
        "_trace":["otako-prd-main-01", "otako-prd-main-02"]
    },
    "op":12,
    "s":1
}
```

