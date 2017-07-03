PRESENCE: Contact Tracking
==========================

Version 1.0 Maxim Sokhatsky

Endpoints
--------

* `actions/api/ClientId` — MQTT
* `events/Node/api/anon/ClientId/Token` — MQTT

Tuples
------
* `{'Person',RosterId, Name, Surname, Username, Phonelist, SoundSettings, ThemeID, VoxImplantID, BlockUsers, Balance, IsParticipants, online/offline}`
* `{'Typing',From,Author}`

Overview
--------

PRESENCE API is specify the mechanism of notification the client with online status of user.
If the user is in the roster of subsribed rooms or subscribed users, then its
presence (online and offline) is being sent to API topic.

The second case is to send `Typing` message on Keyboard

Protocol
--------

### Roster

1. server sends `{'Person',RosterId, Name, Surname, Username, Phonelist, _, _, VoxImplantID, _, _, _, Status}`
    to `p2p/AccId` eventually.

### Typing

1. client sends `{'Typing',_,_}` to `p2p/AccId/ParityAccId` eventually.
