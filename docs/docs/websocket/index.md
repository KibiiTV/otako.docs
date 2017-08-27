# Otako WebSocket Server Layout

### This contains documentation on how the gateway logic will work.

The gateway is a dispatch-only service meaning it will only broadcast data to users (in proper sessions), and not recieve data, other than heartbeats.

To optimize for multi-user connections we will use "pinging" between our websocket systems to confirm if clients are connected or not, and to automatically disconnect users which are not responding, or dummy units.

## [API to Gateway documentation](api-to-gateway)

# Gateway Connections

Authentication to the gateway will be done using request headers, and inputs as well.
Following connection, the gateway will authenticate a user and procceed to reply with either
```json
{
    "e":"AUTHENTICATION_ERROR",
    "d":{
        "message":"Invalid Token",
        "code":0
    },
    "op":9,
    "s":0
}
```
or with
```json
{
    "e":"HELLO",
    "d":{
        "ping_inv":15432,
        "_trace":["otako-prd-main-01", "otako-prd-main-02"]
    },
    "op":10,
    "s":1,
}
```
