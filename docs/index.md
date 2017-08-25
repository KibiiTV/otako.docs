## OTAKOAPP DOCUMENTATION OVERLAY

#### Websocket Generalisation:

A websocket connection will be considered a `"session"`.
Users will be limited to a certain amount of `"sessions"` (undefined as of yet).

Users will ONLY recieve data from the server and **WILL REJECT ANY DATA SENT BY THE USER**.

One connection will be called the `"DISPATCH" connection`. It will send data from the `HTTP Backend` to the `Websocket Server`, and vice-versa.

The `"DISPATCH" connection` is a dedicated connection that will regularly send `PING` data from the `HTTP Backend` to the `Websocket Server`. The `Websocket Server` will compute, parse and perform actions based on the event.

#### Example Situation:

```
POST https://api.otakoapp.com/channels/1/messages
BODY:
{"content":"Hello! This is a message from the Otako client!", nonce:"87654GFGDS7544567FD"}
HEADERS:
Content-Type: application/json
Accept: application/json
Authorization: MzE4MTA5ODIwNzEwNDIwNDgw.DIIv8w.aqcVucnO3WW87J9BX5-a9Bjykx4
```
The server recieves this `request` and enters the info into the `database`. Remember that it returns the `Message Information` from the `client`(`HTTP Request`), calls the `Dispatch Connection` and tells it to send to the `Websocket Server`, with the following `data`:
```
{"t":"EVENT_MESSAGE_CREATE", "d":{ "id":"3", "channel_id":"1" }, "op":0, "s":"14" "event_id":"1"}
```
The `Websocket` should then find the full `Message Data` from the `database`, broadcast to all users that need to see the message, then respond to the `HTTP Backend` with:
```
{"t":"EVENT_ROUNDTRIP", "event_id":"1", "success":true, "op":0, "s":"14"}
```
**IF** the event was not handled successfully, the websocket server should send `Event` and `Trace Details` to the `Logging Server`, and respond to the `HTTP Backend` with:
```
{"t":"EVENT_ROUNDTRIP", "event_id":"1", "success":false, "op":0, "s":"14"}
```
The `HTTP Server` will continue attempting to send the `data` to the  websocket server `X` number of times. After `X` has been reached, and is still unsuccessful, it will log the `Event` and drop the `Request`.

The `Websocket Server` should only log an event error only **once** per event ID.


`Last Updated: 00:48AM Coordinated Universal Time(UTC), 26, August, 2017.`