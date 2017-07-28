MESSAGE: Sending and Receiving Messages
=======================================

Version 1.0 Maxim Sokhatsky

Endpoints
--------

* `actions/1/api/:phone` — MQTT
* `events/1/:node/api/anon/:client/:token` — MQTT

Tuples
------

```erlang
-record('Typing',   {author=[] :: list(),
                     room=[] :: list()}).
```

```erlang
-record('History',  {roster_id= [] :: [] | binary(),
                     contact_id=[] :: list(),
                     unread_counter = 0 :: integer(),
                     last_loaded_msg_id :: [] | integer() | binary(),
                     data=[] :: list(#'Message'{}),
                     status=[] :: atom() | []}).
```

```erlang
-record('Message',  {id=[] :: [] | integer(),
                     container=Container :: atom(),
                     feed_id=[] :: term(),
                     prev=[] :: [] | integer(),
                     next=[] :: [] | integer(),
                     feeds=[] :: list(),
                     msg_id = [] :: [] | binary(),
                     from = [] :: [] | binary(),
                     to = [] :: [] | binary(),
                     sync = [] :: [],
                     created = [] :: [] | integer() | binary(),
                     access = [] :: [],
                     starred = [] :: [],
                     payload = <<>> :: [] | binary(),
                     mime = [],
                     seen_by = [],
                     edit_msg = [] :: [] | integer(),
                     status = [] :: [] | atom()}).
```
Overview
--------

MESSAGE API deliver messages.

Protocol
--------

### Sending Message

```
1. client sends `{'Message',_,_,_,_,_,_,_,_,_,_,_,_,_,_,_,_,_}`
             to `events/1/:node/api/anon/:client/:token` once.
```

```
2. server sends `<<>>` to `actions/api/:client` issuer and
          sends `{'Message',_,_,_,_,_,_,_,_,_,_,_,_,_,_,_,_,_}`
             to `actions/1/api/:client` counterparty.
```

### Retrieve History

```
1. client sends `{'History',Id,Contact,_,_,_,_}` where FromMsgId is the last retreived message id. If FromMsgId is empty then the all history must be pulled for the session
             to `events/1/:node/api/anon/:client/:token` once.
```

```
2. server sends `{'History',Id,Contact,FromMsgId,Messages,_}` where FromMsgId is new last retreived message id for the session
             to `actions/1/api/:client` once or more.
          sends `{'History',Id,_,LastLoadedMsgId,[],_}` where
             to `p2p/:from_phone_id/to_phone_id/:client` as retain.
```

### P2P

* `actions/1/api/:phone` — MQTT
* `p2p/:from_phone_id/:to_phone_id` — MQTT
* `events/1/:node/api/anon/:client/:token` — MQTT

```
1. client sends `{'Message',_,_,_,_,_,_,_,FromPhoneId,ToPhoneId,_,_,_,_,Payload,_,_,_}`
             to `events/1/:node/api/anon/:client/:token` once.
```

```
2. server sends `{'Message',Id,_,_,_,_,_,_,FromPhoneId,ToPhoneId,_,Created,_,_,Payload,_,_,_}`
                to `p2p/:from_phone_id/:to_phone_id` counterparty.
             sends `{'Message',Id,_,_,_,_,_,_,FromPhoneId,ToPhoneId,_,Created,_,_,Payload,_,_,_}`
                to `actions/1/api/:from_phone` issuer to all "from" sessions.
```

### P2P Read
```
1. client sends `{'Message',LastReadId,_,_,_,_,_,_,FromPhoneId,ToPhoneId,_,_,_,_,Payload,_,_,last_read}`
             to `events/1/:node/api/anon/:client/:token` once and marks message as read.

2. server sends `{'History',FromId,ToId,0,_,[LastMsg],_}`
                to `p2p/:from_phone_id/:to_phone_id` as retain.
   server sends `{'History',ToId,FromId,Unread+1,_,[LastMsg],_}`
                to `p2p/:to_phone_id/:from_phone_id` as retain.
```

### P2P Edit/Remove Message

```
1. client sends `{'Message',Id,_,_,_,_,_,_,FromPhoneId,ToPhoneId,_,_,_,_,Payload,SeenBy,EditMsgId,EditStatus}`
             to `events/1/:node/api/anon/:client/:token` once and marks message as read.

2. server sends `{'Message',Id,_,_,_,_,_,_,FromPhoneId,ToPhoneId,_,Created,_,_,Payload,SeenBy,EditMsgId,EditStatus}`
                to `p2p/:from_phone_id/:to_phone_id` counterparty.
             sends `{'Message',Id,_,_,_,_,_,_,FromPhoneId,ToPhoneId,_,Created,_,_,Payload,SeenBy,EditMsgId,EditStatus}`
                to `actions/1/api/:from_phone` issuer to all "from" sessions.
```

