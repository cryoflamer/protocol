PRIVATE: Private Conversations
==============================

Version 1.0 Maxim Sokhatsky

Endpoints
---------

* `actions/1/api/phone/:phone` — MQTT
* `events/1//api/anon/:client/:token` — MQTT

Tuples
------

### `Friend/simple` — Send friend request

```erlang
-record('Friend',   {id=[] :: [] | binary(),
                     roster_id = [] :: [] | integer(),
                     friend_id = [] :: [] | binary(),
                     status=[] :: [] | simple | list }).
```

### `Confirm/friend` — Confirm friend

```erlang
-record('Confirm',  {id=[] :: [] | binary(),
                     roster_id = [] :: [] | integer(),
                     friend_id = [] :: [] | binary(),
                     status=[] :: [] | friend}).
```

### `Revoke/friend` — Revoke friend

```erlang
-record('Revoke',   {id=[] :: [] | binary(),
                     user=[] :: [] | binary(),
                     status=[] :: [] | revoke}).
```

Overview
--------

PRIVATE API serves the peer-to-peer private conversations.

Protocol
--------

### Friend Request

```
1. client sends `{'Friend',Id,UserId,FriendId,simple}`
             or `{'Friend',Id,UserId,FriendIds,list}`
             to `events/1//api/anon/:client/:token` once.
```

```
2. server sends `<<>>`
             or `{io,{error,roster_not_found},<<>>}`
             or `{io,{error,not_authorized},<<>>}`
             to `actions/1/api/phone/:party_phone` once.
```

### Confirmation / Authorization

```
1. client sends `{'Confirm',Id,UserId,FriendId,friend}`
             to `events/1//api/anon/:client/:token` once.
```

```
2. server sends `<<>>`
             or `{io,{ok,{already_present,_}},<<>>}`
             or `{io,{error,roster_not_found},<<>>}`
             or `{io,{error,not_authorized},<<>>}`
             to `actions/1/api/phone/:party_phone` once.
```

```
3.  server sends `{Contact,Party_phone,_,_,_,_,NewUpdateTime,_}`
              to `actions/1/api/phone/:counterparty_phone`
             and `{Contact,ConterParty_phone,_,_,_,_,NewUpdateTime,_}`
              to `actions/1/api/phone/:party_phone` once.
```

### Revoke

```
1. client sends `{'Revoke',Id,User,Status}`
             to `events/1//api/anon/:client/:token` once.
```

```
2. server sends `{'Revoke',Id,User,Status}`
             to `actions/1/api/phone/:party` once.
```

