PRIVATE: P2P Conversations
==========================

Version 1.0 Maxim Sokhatsky

Endpoints
---------
* `actions/api/ClientId` — MQTT
* `events/Node/api/anon/ClientId/Token` — MQTT

Tuples
------

* `{'Friend',Roster,User,Status}`
* `{'Confirm',Roster,User,Status}`
* `{'Revoke',Roster,User,Status}`

Overview
--------

PRIVATE API serves the peer-to-peer private conversations.

Protocol
--------

### Friend Request

1. client sends `{'Friend',Id,UserId,FriendId,Status}` to `p2p/RosterId/ParityRosterId` once.

### Confirmation / Authorization

1. client sends `{'Confirm',Id,User,Status}` to `events/Node/api/anon/ClientId/Token` once.
2. server sends `{'Confirm',Id,User,Status}` to `actions/api/PartyId/Token` once.

### Revoke

1. client sends `{'Revoke',Id,User,Status}` to `events/Node/api/anon/ClientId/Token` once.
2. server sends `{'Revoke',Id,User,Status}` to `actions/api/PartyId/Token` once.
