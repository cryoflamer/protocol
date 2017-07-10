PROFILE: Managing Phone Accounts
================================

Version 1.0 Maxim Sokhatsky

Endpoints
--------

* `actions/api/ClientId` — MQTT
* `events/Node/api/anon/ClientId/Token` — MQTT

Tuples
------

* `{'Profile',Phone,Data,PersonId,Accounts,Status}`

Overview
--------

ROSTER API manages different accounts with different contact lists.

Protocol
--------

### GET PROFILE

1. client sends `{'Profile',Phone,_,_,_,get}` to `events/Node/api/anon/ClientId/Token` once.
2. server sends `{'Profile',Phone,_,_,_,_}`
             or `{io,{error,not_authorized},<<>>}`
             to `actions/api/ClientId` nonzero times.

### SET PROFILE

1. client sends `{'Roster,Phone,_,_,_,set}` to `events/Node/api/anon/ClientId/Token` once.
2. server sends `{'Roster,Phone,_,_,_,_}`
             or `{io,{error,not_authorized},<<>>}`
             to `actions/api/ClientId` nonzero times.

### REMOVE PROFILE

1. client sends `{'Roster,Phone,_,_,_,remove}` to `events/Node/api/anon/ClientId/Token` once.
2. server sends `{'Roster,Phone,_,_,_,_}`
             or `{io,{error,not_authorized},<<>>}`
             to `actions/api/ClientId` nonzero times.

