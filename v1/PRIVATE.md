PRIVATE: P2P Conversations
==========================

Version 1.0 Maxim Sokhatsky

Endpoints
---------

* `actions/1/api/:client` — MQTT
* `events/1/:node/api/anon/:client/:token` — MQTT

Tuples
------

```erlang
-record('Friend',   {id=[] :: [] | binary(),
                     roster_id = [] :: [] | integer(),
                     friend_id = [] :: [] | binary(),
                     status=[] :: [] | simple | atom()}).
```

```erlang
-record('Confirm',  {id=[] :: [] | binary(),
                     roster_id = [] :: [] | integer(),
                     friend_id = [] :: [] | binary(),
                     status=[] :: [] | roster | atom()}).
```

```erlang
-record('Revoke',   {id=[] :: [] | binary(),
                     user=[] :: [] | binary(),
                     status=[] :: [] | atom()}).
```

Overview
--------

PRIVATE API serves the peer-to-peer private conversations.

Protocol
--------

### Friend Request

```
1. client sends `{'Friend',Id,UserId,FriendId,Status}`
             to `events/1/:node/api/anon/:client/:token` once.
```

```
2. server sends `<<>>`
             or `{io,{error,roster_not_found},<<>>}`
             or `{io,{error,not_authorized},<<>>}`
             to `actions/1/api/:party` once.
```

### Confirmation / Authorization

```
1. client sends `{'Confirm',Id,UserId,FriendId,Status}`
             to `events/1/:node/api/anon/:client/:token` once.
```

```
2. server sends `{io,{ok,added},<<>>}`
             or `{io,{ok,{already_present,_}},<<>>}`
             or `{io,{error,roster_not_found},<<>>}`
             or `{io,{error,not_authorized},<<>>}`
             to `actions/1/api/:party` once.
```

```
3. server sends `{Contact,_,_,_,_,_,_}`
             to `actions/1/api/:counterparty` once.
```

### Revoke

```
1. client sends `{'Revoke',Id,User,Status}`
             to `events/1/:node/api/anon/:client/:token` once.
```

```
2. server sends `{'Revoke',Id,User,Status}`
             to `actions/1/api/:party` once.
```
