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
                     settings=[] :: list(),
                     members=[] :: list(#'Member'{}),
                     admins=[]  :: list(#'Member'{}),
                     type=[] :: [] | group | channel,
                     tos=[] :: [] | binary(),
                     status=[] :: [] | create | join | leave
                            | ban | uban | add_admins | rem_admins
                            | add_members | remove_members
                            | patch | get | delete | settings
                            | voice | video }).
```

Overview
--------

TRIBE API serves the MUC groupchat conversations.

Protocol
--------

### Room/create

```
1. client sends `{'Room',_,_,_,_,_,_,_,_}`
             to `events/1//api/anon/:client/:token` once.
```

```
2. server sends `{'Room',_,_,_,_,_,_,_,_}`
             to `actions/1/api/:client/:token` once.
```