TRIBE: MUC Conversations
========================

Version 1.0 Maxim Sokhatsky

Endpoints
--------

* `room/Room` — MQTT
* `actions/1/api/:client/:token` — MQTT
* `events/1/:node/api/anon/:client/:token` — MQTT

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
             to `events/1/:node/api/anon/:client/:token` once.
```

### Auto Join

```
2. server sends `{'Room',_,_,_,_}`
             to `actions/1/api/:client/:token` once.
```

### Approve Join

```
1. client sends `{'Approve',_,_,_}`
             to `events/1/:node/api/anon/:client/:token` once.
```

```
2. server sends `{'Room',_,_,_,_}`
             to `actions/1/api/:party/:token` once.
```

### Leave Room

```
1. client sends `{'Leave',_,_,_}`
             to `events/1/:node/api/anon/:client/:token` once.
```

```
2. server sends `{'Room',_,_,_,_}`
             to `actions/1/api/:client/:token` participants times.
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
