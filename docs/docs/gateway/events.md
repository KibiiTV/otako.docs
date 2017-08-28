# Gateway Events



































## The layout for an event is like so:

### Event Name
Event description
###### Event Name Structure
| Field | Type | Description |
|-------|------|-------------|
| message | string | the example data for this example event |

###### Example Event Name
```json
{
    "e":"EVENT_PROTOCOL_NAME",
    "d":{
        "message":"EVENT_DATA_HERE"
    },
    "op":0,
    "s":0
}
```

In markdown:
```md

### Event Name
Event description
###### Event Name Structure
| Field | Type | Description |
|-------|------|-------------|
| message | string | the example data for this example event |

###### Example Event Name
```json
{
    "e":"EVENT_PROTOCOL_NAME",
    "d":{
        "message":"EVENT_DATA_HERE"
    },
    "op":0,
    "s":0
}
\```
```