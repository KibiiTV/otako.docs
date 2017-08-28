# Gateway Events

# GATEWAY_HELLO
### Sent from the gateway on connection, contains auth data and debug information.

| Field | Type | Description |
|-------|------|-------------|
| ping_inv | integer | the interval (in milliseconds) the client should heartbeat with |
| _trace | array of strings | used for debugging, array of servers connected to |

```json
{
    "e":"GATEWAY_HELLO",
    "d":{
        "ping_inv":15432,
        "nonce":"NONCE_INFO_HERE",
        
        "_trace":["otako-prd-main-01", "otako-prd-main-02"]
    },
    "op":12,
    "s":1
}
```

# GATEWAY_ERROR
### Sent from the gateway on connection, contains error information
