MESSAGE: Sending and Receiving Messages
=======================================

Version 1.0 Maxim Sokhatsky

Endpoints
--------

* `actions/api/ClientId` — MQTT
* `events/Node/api/anon/ClientId/Token` — MQTT

Tuples
------

```erlang
-record('Typing',   {author=[] :: list(),
                     room=[] :: list()}).
```

```erlang
-record('History',  {roster_id= [] :: [] | binary(),
                     contact_id=[] :: list(),
                     data=[] :: list(#'Message'{}),
                     status=[] :: atom() | []}).
```

```erlang
-record('Message',  {id=[] :: [] | integer(),
                     container=Container :: atom(),
                     feed_id=[] :: term(),
                     prev=[] :: [] | integer(),
                     next=[] :: [] | integer(),
                     feeds=[] :: list()
                     msg_id = [] :: [] | binary(),
                     from = [] :: [] | binary(),
                     to = [] :: [] | binary(),
                     sync = [] :: [],
                     created = [] :: [],
                     access = [] :: [],
                     starred = [] :: [],
                     payload = <<>> :: [] | binary(),
                     mime = [],
                     seen_by = [],
                     status = [] :: [] | atom()}).
```
Overview
--------

MESSAGE API deliver messages.

Protocol
--------

### Sending Message

1. client sends `{'Message',_,_,_,_,_,_,_,_,_,_,_,_,_,_,_,_,_}` to `events/Node/api/anon/ClientId/Token` once.
2. server sends `<<>>` to `actions/api/ClientId` issuer and
          sends `{'Message',_,_,_,_,_,_,_,_,_,_,_,_,_,_,_,_,_}` to `actions/api/ClientId` counterparty.

### Retrieve History

1. client sends `{'History',Id,Cursor,_,_}` to `events/Node/api/anon/ClientId/Token` once.
2. server sends `{'History',Id,_,Messages,_}` to `actions/api/ClientId` one or more times.
