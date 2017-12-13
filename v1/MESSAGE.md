MESSAGE: Sending and Receiving Messages
=======================================

Version 1.0 Maxim Sokhatsky

Endpoints
--------

* `actions/1/api/:client` — MQTT
* `events/1//api//` — MQTT
* `ses/:phone` - MQTT
* `p2p/:phone_id/:phone_id` — MQTT
* `room/:room` — MQTT


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
-record('Message',      {id        = [] :: [] | integer(),
                         container = chain :: atom() | chain | cur,
                         feed_id   = [] :: #muc{} | #p2p{},
                         prev      = [] :: [] | integer(),
                         next      = [] :: [] | integer(),
                         msg_id    = [] :: [] | binary(),
                         from      = [] :: [] | binary(),
                         to        = [] :: [] | binary(),
                         created   = [] :: [] | integer() | binary(),
                         files     = [] :: [] | list(#'Desc'{}),
                         type      = [] :: [] | atom() | reply | forward | sched
                                              | online | offline | join | leave,
                         link      = [] :: [] | integer(),
                         seenby    = [] :: [] | integer() | list(integer()),
                         repliedby = [] :: [] | integer() | list(integer()),
                         status    = [] :: [] | client | async | deleted
                                              | sent | internal | event | edit | muc }).
```

```erlang
-record('Desc',         {id       = [] :: [] | binary(),
                         mime     = <<"text">> :: [] | binary(),
                         payload  = [] :: [] | binary(),
                         size     = 0  :: integer(),
                         filename = [] :: [] | binary(),
                         info     = [] :: [] | binary()}).
```

Overview
--------

MESSAGE API deliver messages.

Protocol
--------

### `Message/client` — General Sending Message to Subscribers

```
1. client sends `{'Message',[],_,Feed,_,_,_,From,To,_,Files,Type,_,SeenBy,RepliedBy,client}`
             to `events/1//api//` once.
```
Explanations:
* SeenBy - Якщо повідомлення всередині рума, то це список `id` мемберів, що не мають бачити
це повідомлення. В подальшому може бути звортній фільтр - тобто список тільки тих мемберів, що можуть
бачити повідомлення. Відповідно `id` мають бути від'ємні.

Examples:

* From — `380676631870_1`
* To — `380676631870_1` or `lobby`
* Feed — `{p2p,_,_}` or `{muc,_}`
* SeenBy - `[11, 24]` or `[]`

```
2. server sends `{'Message',Id,_,Feed,_,_,_,From,To,_,Files,Type,_,_,_,sent}`
             to `p2p/:address` twice
             or `room/:room` members times.
```

### `Message/reply` — Sending Reply Message to Subscribers

```
1. client sends `{'Message',[],_,Feed,_,_,_,From,To,_,Files,reply,Link,_,_,client}`
             to `events/1//api//` once.
```
Explanations:
* Link - вказує на `id` повідомлення, що цитується.

Examples:

* From — `380676631870_1`
* To — `380676631870_1` or `lobby`
* Feed — `{p2p,_,_}` or `{muc,_}`
* SeenBy - `[11, 24]` or `[]`

```
2. server sends `{'Message',Id,_,Feed,_,_,_,From,To,_,Files,Type,LinkId,_,_,sent}`
             to `p2p/:address` twice
             or `room/:room` members times.
   (?) server sends `{'Message',LinkId,_,Feed,_,_,_,From,To,_,Files,Type,_,_,RepliedBy,sent}`
             to `p2p/:address` twice
             or `room/:room` members times.

```
Explanations:
* RepliedBy - список `id` повідомлень, що цитують LinkId повідомлення.


### `Message/upload` — Sending Async Upload Message

```
1. client sends `{'Message',[],_,_,_,_,Id,From,To,_,Files,Type,_,upload}`
             to `events/1//api//` once.
```

```
2. server sends `{'Message',Id,_,_,_,_,_,To,From,_,Files,Type,_,link}`
             to `p2p/:address' twice
             or `room/:room` members times.
```

```
3. client sends `{'Message',Id,_,_,_,_,_,From,To,_,Files,Type,_,complete}`
             to `events/1//api//` once.
```

```
4. server sends `{'Message',Id,_,_,_,_,_,To,From,_,Files,Type,_,sent}`
             to `p2p/:address' twice
             or `room/:room` members times.
```

### `Message/edit` — Edit/Remove Message

```
1. client sends `{'Message',Id,_,_,_,_,_,_,_,_,Files,Type,_,edit}`
             to `events/1//api//` once.
```

```
2. server sends `{'Message',Id,_,_,_,_,_,_,_,_,Files,Type,_,edit}`
             to `p2p/:address' twice
             or `room/:room` member times.
```

### `History/get` — Retrieve History

```
1. client sends `{'History',PhoneId,Feed,Size,EntityId,_,get}`
             to `events/1//api//` once.
```

* Feed     — `#p2p{}` or `#muc{}` key of the conversation
* EntityId — id of the message(or other entity) beginning from which you want to get list of next or prev messages.

```
2. server sends `{'History',PhoneId,Feed,NewPos,EntityId,Data,get}`	     
             or `{io,{error,invalid_data},<<>>}`	    
             to `actions/1/api/:client` once or more.
```

* Data    — list of messages.

### `History/update` — Set cursor

Sets marker to feed for counterparty.

```
1. client sends `{'History',PhoneId,Feed,_,Position,[],update}`
             to `events/1//api//` once.
```


* Position — id of the message you want cursor to point

```

2. server sends `{Contact,PhoneId,_,_,_,_,_,_,_,_,Unread,_,_,_,_,last_msg}` 
             or `{Room,Name,_,_,_,_,_,_,_,_,_,Unread,_,_,_,last_msg}` 
             to `ses/:phone` once 
             or `{io,{error,invalid_data},<<>>}`
             to `actions/1/api/:client` once.
```
