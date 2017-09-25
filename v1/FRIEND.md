FRIEND: Manage Private Subscriptions
====================================

Version 1.0 Maxim Sokhatsky

Endpoints
---------

* `actions/1/api/phone/:phone` — MQTT
* `events/1//api/anon/:client/:token` — MQTT

Tuples
------

```erlang
-record('Friend', {phone_id = [] :: [] | binary(),
                   friend_id = [] :: [] | binary(),
                   status=[] :: [] | request | confirm | revoke }).
```

Overview
--------

FRIEND API serves the management of peer-to-peer private subscriptions.
At the time users establish a friendship the MQTT subscriptions are being created.

Protocol
--------

### `Friend/request` — Friendship Request

```
1. client sends `{'Friend',Id,UserId,FriendId,request}`
             to `events/1//api/anon/:client/:token` once.
```

```
2. server sends `{io,{error,roster_not_found},<<>>}`
             or `{io,{error,not_authorized},<<>>}`
             to `actions/1/api/phone/:party_phone` once.
```

### `Friend/confirm` — Confirm friendship

This is the moment when subscriptions to private
conversation MQTT topic are being created for both counterparties.

```
1. client sends `{'Friend',Id,UserId,FriendId,confirm}`
             to `events/1//api/anon/:client/:token` once.
```

```
2. server sends `{io,{ok,{already_present,_}},<<>>}`
             or `{io,{error,roster_not_found},<<>>}`
             or `{io,{error,not_authorized},<<>>}`
             to `actions/1/api/phone/:party` once.
```

```
3.  server sends `{'Contact',Party,_,_,_,_,Time,_}`
              to `actions/1/api/phone/:counterparty`
             and `{'Contact',ConterParty,_,_,_,_,Time,_}`
              to `actions/1/api/phone/:party` once.
```

### `Friend/revoke` — Revoke friendship

This is the moment when subscriptions to private
conversation MQTT topic are being removed for both counterparties.

```
1. client sends `{'Friend',Id,User,revoke}`
             to `events/1//api/anon/:client/:token` once.
```

```
2. server sends `{'Contact',Party,_,_,_,_,Time,_}`
             to `actions/1/api/phone/:party` once.
```

### `Friend/ban` — Ban friend

This is the moment when subscriptions to private
conversation MQTT topic are being removed for both counterparties.

```
1. client sends `{'Friend',Id,User,revoke}`
             to `events/1//api/anon/:client/:token` once.
```

```
2. server sends `{'Contact',Party,_,_,_,_,Time,_}`
             to `actions/1/api/phone/:party` once.
```

### `Friend/mute` — Mute friend

This is the moment when subscriptions to private
conversation MQTT topic are being removed for both counterparties.

```
1. client sends `{'Friend',Id,User,mute}`
             to `events/1//api/anon/:client/:token` once.
```

```
2. server sends `{'Contact',Party,_,_,_,_,Time,_}`
             to `actions/1/api/phone/:party` once.
```
