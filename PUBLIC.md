PUBLIC: MUC/TRIBE Conversations
===============================

Version 1.0 Maxim Sokhatsky

Endpoints
--------

* `room/Room` — MQTT
* `events/1/Node/api/anon/ClientId/Token` — MQTT

Tuples
------

```erlang
-record('Room',     {room=[] :: [] | binary(),
                     description=[] :: [] | binary(),
                     acl=[] :: list(),
                     settings=[] :: list()}).
```

```erlang
-record('Join',     {id=[] :: [] | binary(),
                     user=[] :: [] | binary(),
                     room=[] :: [] | binary(),
                     answer=[] :: [] | binary()}).
```

```erlang
-record('Leave',    {id=[] :: [] | binary(),
                     user=[] :: [] | binary(),
                     room=[] :: [] | binary(),
                     answer=[] :: [] | binary()}).
```

```erlang
-record('Approve',  {id=[] :: [] | binary(),
                     user=[] :: [] | binary(),
                     room=[] :: [] | binary(),
                     answer=[] :: [] | binary()}).
```

Overview
--------

PUBLIC API serves the MUC groupchat conversations.

Protocol
--------

### Join Room

```
1. client sends `{'Join',_,_,_}`
             to `events/Node/api/anon/ClientId/Token` once.
```

### Auto Join

```
2. server sends `{'Room',_,_,_,_}`
             to `actions/api/ClientId/Token` once.
```

### Approve Join

```
1. client sends `{'Approve',_,_,_}`
             to `events/Node/api/anon/ClientId/Token` once.
```

```
2. server sends `{'Room',_,_,_,_}`
             to `actions/api/PartyId/Token` once.
```

### Leave Room

```
1. client sends `{'Leave',_,_,_}`
             to `events/Node/api/anon/ClientId/Token` once.
```

```
2. server sends `{'Room',_,_,_,_}`
             to `actions/api/{ClientId}/Token` participants times.
```

### Joined

```
1. server sends `{'Message',_,_,_,_,_,_,_,_,_,_,_,_,_,_,_,_,_}`
             to `room/Room` participants times.
```

### Leaved

```
1. server sends `{'Message',_,_,_,_,_,_,_,_,_,_,_,_,_,_,_,_,_}`
             to `room/Room` participants times.
```
