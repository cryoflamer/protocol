PUBLIC: MUC/TRIBE Conversations
===============================

Version 1.0 Maxim Sokhatsky

Endpoints
--------

* `room/Room` — MQTT
* `events/Node/api/anon/ClientId/Token` — MQTT

Tuples
------

* `{'Join',User,Room,Answer}`
* `{'Room',Room,Description,Acl,Settings}`
* `{'Approve',_,_,_}`
* `{'Leave',_,_,_}`
* `{'Message',_,_,_,_,_,_,MsgId,From,To,Sync,Timings,Access,Likes,Payload,Mime,Status}`

Overview
--------

PUBLIC API serves the MUC groupchat conversations.

Protocol
--------

### Join Room

1. client sends `{'Join',_,_,_}` to `events/Node/api/anon/ClientId/Token` once.

### Auto Join

2. server sends `{'Room',_,_,_,_}` to `actions/api/ClientId/Token` once.

### Approve Join

1. client sends `{'Approve',_,_,_}` to `events/Node/api/anon/ClientId/Token` once.
2. server sends `{'Room',_,_,_,_}` to `actions/api/PartyId/Token` once.

### Leave Room

1. client sends `{'Leave',_,_,_}` to `events/Node/api/anon/ClientId/Token` once.
2. server sends `{'Room',_,_,_,_}` to `actions/api/{ClientId}/Token` participants times.

### Publish Public Message to Room

1. client sends `{'Message',_,_,_,_,_,_,_,_,_,_,_,_,_,_,_,_}` to `events/Node/api/anon/ClientId/Token` once.
2. server sends `{'Message',_,_,_,_,_,_,_,_,_,_,_,_,_,_,_,_}` to `room/Room` participants times.

### Joined

1. server sends `{'Message',_,_,_,_,_,_,_,_,_,_,_,_,_,_,_,_}` to `room/Room` participants times.

### Leaved

1. server sends `{'Message',_,_,_,_,_,_,_,_,_,_,_,_,_,_,_,_}` to `room/Room` participants times.
