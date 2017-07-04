MESSAGE: Sending and Receiving Messages
=======================================

Version 1.0 Maxim Sokhatsky

Endpoints
--------

* `actions/api/ClientId` — MQTT
* `events/Node/api/anon/ClientId/Token` — MQTT

Tuples
------

* `{'Typing',Author,Channel}`
* `{'History',Id,Cursor,Data,Status}`
* `{'Message',Id,Container,FeedId,Prev,Next,Feeds,MsgId,From,To,Sync,Created,Access,Starred,Payload,Mime,Seen,Status}`

Overview
--------

MESSAGE API deliver messages.

Protocol
--------

### Sending Message

1. client sends `{'Message',_,_,_,_,_,_,_,_,_,_,_,_,_,_,_,_,_}` to `events/Node/api/anon/ClientId/Token` once.
2. server sends `<<>>` to `actions/api/ClientId` issuer and
          sends `{'Message',_,_,_,_,_,_,_,_,_,_,_,_,_,_,_,_,_}` to `actions/api/ClientId` counterparty.

### Retrieve History

1. client sends `{'History',Id,Cursor,_,_}` to `events/Node/api/anon/ClientId/Token` once.
2. server sends `{'History',Id,_,Messages,_}` to `actions/api/ClientId` one or more times.
