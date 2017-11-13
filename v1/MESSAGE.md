MESSAGE: Sending and Receiving Messages
=======================================

Version 1.0 Maxim Sokhatsky

Endpoints
--------

* `p2p/:phone_id/:phone_id` — MQTT
* `actions/1/api/:client` — MQTT
* `events/1//api/anon//` — MQTT

Tuples
------

```erlang
-record('Typing',   {author=[] :: list(),
                     room=[] :: list()}).
```

```erlang
-record(muc,        {name = [] :: [] | binary() }).
-record(group,      {name = [] :: [] | binary() }).
-record(p2p,        {from = [] :: [] | binary(),
                     to   = [] :: [] | binary() }).

```

```erlang
-record('History',  {roster_id  =[] :: [] | binary(),
                     feed       =[] :: [] | #p2p{} | #muc{},
                     size       = 0 :: [] | integer(),
                     entity_id  = 0 :: [] | integer(),
                     data       =[] :: list(#'Message'{}),
                     status     =[] :: [] | updated | get | update
                                          | last_loaded | last_msg }).
```

```erlang
-record('Message',  {id       =[] :: [] | integer(),
                     container = chain :: atom() | chain | cur,
                     feed_id  =[] :: #muc{} | #p2p{},
                     prev     =[] :: [] | integer(),
                     next     =[] :: [] | integer(),
                     msg_id   =[] :: [] | binary(),
                     from     =[] :: [] | binary(),
                     to       =[] :: [] | binary(),
                     created  =[] :: [] | integer() | binary(),
                     files    =[] :: [] | list(#'Desc'{}),
                     type     =[] :: [] | atom() | reply | forward | sched
                                        | online | offline | join | leave,
                     edit_msg =[] :: [] | integer(),
                     status   =[] :: [] | client | async
                                        | sent | internal | last_read | edit | muc }).
```

```erlang
-record('Desc',     {mime     = <<"text">> :: [] | binary(),
                     payload  =[] :: [] | binary(),
                     size     = 0 :: integer(),
                     filename =[] :: [] | binary(),
                     info     =[] :: [] | binary()}).
```

Overview
--------

MESSAGE API deliver messages.

Protocol
--------

### `Message/client` — General Sending Message to Subscribers

```
1. client sends `{'Message',[],_,Feed,_,_,_,From,To,_,Files,Type,_,_,client}`
             to `events/1//api/anon//` once.
```

Examples:

* From — `380676631870_1`
* To — `380676631870_1` or `lobby`
* Feed — `{p2p,_,_}` or `{muc,_}`

```
2. server sends `{'Message',Id,_,Feed,_,_,_,From,To,_,Files,Type,_,_,sent}`
             to `p2p/:address` twice
             or `room/:room` members times.
```

### `Message/upload` — Sending Async Upload Message

```
1. client sends `{'Message',[],_,_,_,_,Id,From,To,Files,Time,Type,_,upload}`
             to `events/1//api/anon//` once.
```

```
2. server sends `{'Message',Id,_,_,_,_,_,To,From,Files,Time,Type,_,link}`
             to `p2p/:address' twice
             or `room/:to` members times.
```

```
3. client sends `{'Message',Id,_,_,_,_,_,From,To,Files,Time,Type,_,complete}`
             to `events/1//api/anon//` once.
```

```
4. server sends `{'Message',Id,_,_,_,_,_,To,From,Files,Time,Type,_,sent}`
             to `p2p/:address' twice
             or `room/:to` members times.
```

### `Message/edit` — Edit/Remove Message

```
1. client sends `{'Message',Id,_,_,_,_,_,_,_,Files,Time,Type,_,edit}`
             to `events/1//api/anon//` once.
```

```
2. server sends `{'Message',Id,_,_,_,_,_,_,_,Files,Time,Type,_,edit}`
             to `p2p/:address' twice
             or `room/:to` member times.
``

### `History/get` — Retrieve History

```
1. client sends `{'History',Id,Feed,Size,EntityId,_,get}`
             to `events/1//api/anon//` once.
```
* Feed — `#p2p{}` key of the conversation

* EntityId — id of the message beginning from which you want to get list of next or prev messages.

```
2. server sends `{'History',Id,Feed,NewPos,EntityId,Data,get}`
             to `actions/1/api/:client` once or more.
```

### `History/update` — Set cursor

Sets marker to feed for counterparty.

```
1. client sends `{'History',Id,Feed,_,Position,[],update}`
             to `events/1//api/anon//` once.
```


* Position — id of the message you want cursor to point

```

2. server sends `{ok2,Feed,Count}`
             to `actions/1/api/:client` once or more.
```
