# Otako WebSocket Server Layout

### This contains documentation on how the gateway logic will work.

The gateway is a dispatch-only service meaning it will only broadcast data to users (in proper sessions), and not recieve data, other than heartbeats.

To optimize for multi-user connections we will use "pinging" between our websocket systems to confirm if clients are connected or not

users will only recieve data from the server and **will reject any data sent by the user**
one specific connection called the DISPATCH connection will send data from the the HTTP backend to the websocket server, 
it is a dedicated connection that will regularly send "ping" data from the http backend to the websocket server, and vice versa
the dispatch connection will send event data to the websocket server, and the websocket server will compute, parse, and perform action based on the event
and example event situation is described below:

A user client sends an http request to the otako backend, details of the http request are shown below:

POST https://api.otakoapp.com/channels/1/messages
BODY:
{"content":"Hello! This is a message from the Otako client!", nonce:"87654GFGDS7544567FD"}
HEADERS:
Content-Type: application/json
Accept: application/json
Authorization: MzE4MTA5ODIwNzEwNDIwNDgw.DIIv8w.aqcVucnO3WW87J9BX5-a9Bjykx4

The server recieves this request, and proceeds to enter the info into the database, after which doing so, (remember, it returns the message information to the client(the http request)), calls the dispatch connection and tells it to send to the websocket server, with this data:

{"t":"EVENT_MESSAGE_CREATE", "d":{ "id":"3", "channel_id":"1" }, "op":0, "s":"14" "event_id":"1"}

The websocket should then proceed to find full message data from database, broadcast to all users that need to see this, then respond to the HTTP backend with:

{"t":"EVENT_ROUNDTRIP", "event_id":"1", "success":true, "op":0, "s":"14"}

IF the event was not handled successfully, the websocket server should send event and trace details to the logging server, and respond to the http backend with:

{"t":"EVENT_ROUNDTRIP", "event_id":"1", "success":false, "op":0, "s":"14"}

The http server will then continue attempting to send the data to the websocket server until a certain number ( X ) amount of times, after X has been reached and it has still not been successful, it will log the event and drop the request.
The websocket server should only log an event error only **once** per event ID.

the t variable 