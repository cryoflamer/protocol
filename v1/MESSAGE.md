MESSAGE: Sending and Receiving Messages
=======================================

Version 1.0 Maxim Sokhatsky

Endpoints
--------

* `actions/1/api/phone/:phone` — MQTT
* `events/1//api/anon/:client/:token` — MQTT

Tuples
------

```erlang
-record('Typing',   {author=[] :: list(),
                     room=[] :: list()}).
```

```erlang
-record('History',  {roster_id= [] :: [] | binary(),
                     contact_id=[] :: [] | binary(),
                     cursor :: [] | integer() | binary(),
                     data=[] :: list(#'Message'{}),
                     status=[] :: atom() | updated | last_loaded | last_msg}).
```

```erlang
-record('Message',  {id=[] :: [] | integer(),
                     container = chain,
                     feed_id=[] :: term() | {p2p, binary(), binary()},
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
                     files = [] :: [] | list(#'Desc'{}), %% will be added after alpha version
                     seen_by = [],
                     edit_msg = [] :: [] | integer(),
                     status = [] :: [] | atom() | client | sent | offline
                            | online | internal | last_read | edit }).
```

```erlang
-record('Desc',     {mime = [] :: [] | binary(),
                     payload = [] :: [] | binary() }).

```

```erlang
-record('Cursor',   {phone = [] :: [] | binary(),
                     roster = [] :: [] | integer(),
                     position = [] :: [] | integer() }).

```

Overview
--------

MESSAGE API deliver messages.

Protocol
--------

### Sending Message to Subscribers

```
1. client sends `{'Message',_,_,_,_,_,_,_,FromPhoneId,ToPhoneId,_,_,_,_,Payload,_,_,client}`
             to `events/1//api/anon/:client/:token` once.
```

```
2. server sends `{'Message',Id,_,_,_,_,_,_,FromPhoneId,ToPhoneId,_,Created,_,_,Payload,_,_,sent}`
                to `p2p/:from_phone_id/:to_phone_id` once.
```

```
3. server sends `{'Message',Id,_,_,_,_,_,_,ToPhoneId,FromPhoneId,_,Created,_,_,Payload,_,_,sent}`
                to `p2p/:FromPhoneId/:ToPhoneId` once.
```

### Edit/Remove Message

```
1. client sends `{'Message',Id,_,_,_,_,_,_,FromPhoneId,
                  ToPhoneId,_,_,_,_,Payload,SeenBy,EditMsgId,edit}`
             to `events/1//api/anon/:client/:token` once.
```

```
2. server sends `{'Message',Id,_,_,_,_,_,_,FromPhoneId,ToPhoneId,_,
                  Created,_,_,Payload,SeenBy,EditMsgId,edit}`
             to `p2p/:FromPhoneId/:ToPhoneId` counterparty.
```

```
3. server sends `{'Message',Id,_,_,_,_,_,_,FromPhoneId,ToPhoneId,_,
                  Created,_,_,Payload,SeenBy,EditMsgId,edit}`
             to `actions/1/api/phone/:from_phone` to issuer.
```

### Retrieve History

```
1. client sends `{'History',Id,Contact,MsgId,_,get}`
             to `events/1//api/anon/:client/:token` once.
```

```
2. server sends `{'History',Id,Contact,MsgId,Messages,get}`
             to `actions/1/api/:client` once or more.
```

