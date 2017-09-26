ROOM: MUC Conversations
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
-record('Member',   {id=[] :: [] | integer(),
                     container = chain | cur,
                     feed_id=[] :: #room{},
                     prev=[] :: [] | integer(),
                     next=[] :: [] | integer(),
                     phone_id=[] :: [] | binary(),
                     avatar=[] :: [] | binary(),
                     names=[] :: [] | binary(),
                     surnames=[] :: [] | binary(),
                     nick= [] :: [] | binary(),
                     email=[] :: [] | binary(),
                     person_id=[] :: [] | binary(),
                     unread=0 :: [] | integer(),
                     update=0 :: [] | integer(),
                     options=[] :: [] | list(),
                     fetch=[] :: [] | integer(),
                     presence=[] :: [] | online | offline | join | leave,
                     status=[] :: [] | admin | member | patch}).

-record('Room',     {room=[] :: [] | binary(),
                     description=[] :: [] | binary(),
                     settings=[] :: list(),
                     members=[] :: list(#'Member'{}),
                     admins=[]  :: list(#'Member'{}),
                     type=[] :: [] | group | channel,
                     tos=[] :: [] | binary(),
                     status=[] :: [] | create | join | leave
                            | ban | uban 
                            | patch | get | delete | settings
                            | voice | video }).
```

Overview
--------

TRIBE API serves the MUC groupchat conversations.

Protocol
--------

### `Room/create` — Create MUC

```
1. client sends `{'Room',Name,Desc,_,_,_,_,_,create}`
             to `events/1//api/anon/:client/:token` once.
```

```
2. server sends `{'Room',Name,Desc,_,_,_,_,_,create}`
             to `actions/1/api/:client/:token` once.
```

### `Room/patch` — Modify MUC Settings

```
1. client sends `{'Room',Name,Desc,Settings,Members,Admins,Type,Tos,patch}`
             to `events/1//api/anon/:client/:token` once.
```

```
2. server sends `{'Room',Name,Desc,Settings,Members,Admins,Type,Tos,patch}`
             to `room/:room` members times.
```

### `Room/join` — Join Members by Admin

```
1. client sends `{'Room',Name,_,_,Members,Admins,_,_,join}`
             to `events/1//api/anon/:client/:token` once.
```

```
2. server sends `{'Room',_,_,_,_,_,_,_,join}`
             to `room/:room` members times.
```

### `Room/leave` — Leave Members by Admin

```
1. client sends `{'Room',Name,_,_,Members,Admins,_,_,leave}`
             to `events/1//api/anon/:client/:token` once.
```

```
2. server sends `{'Room',_,_,_,_,_,_,_,leave}`
             to `room/:room` members times.
```

### `Room/ban` — Ban Members by Admin

```
1. client sends `{'Room',Name,_,_,Members,_,_,_,ban}`
             to `events/1//api/anon/:client/:token` once.
```

```
2. server sends `{'Room',_,_,_,_,_,_,_,ban}`
             to `room/:room` members times.
```

### `Room/unban` — Unban Members by Admin

```
1. client sends `{'Room',Name,_,_,Members,_,_,_,unban}`
             to `events/1//api/anon/:client/:token` once.
```

```
2. server sends `{'Room',_,_,_,_,_,_,_,unban}`
             to `room/:room` members times.
```

