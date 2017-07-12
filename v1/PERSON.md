PERSON: External Settings
=========================

Version 1.0 Maxim Sokhatsky

Endpoints
--------

* `actions/1/api/:client` — MQTT
* `events/1/:node/api/anon/:client/:token` — MQTT

Tuples
------

```erlang
-record('Person',  {id=[] :: [] | binary(),
                    name=[] :: list(),
                    surname=[] :: list(),
                    username=[] :: list(),
                    phonelist=[] :: list(),
                    alias=[] :: list(),
                    avatar=[] :: list(),
                    localize=[] :: list(),
                    'NotificationSettings'=[] :: list(),
                    'SoundSettings'=[] :: list(),
                    'ThemeID'=[] :: list(),
                    'voxImplantID'=[] :: list(),
                    'BlockUsers'=[] :: list(),
                    'balance'=0 :: integer(),
                    'isParticipants'=[] :: list(),
                    status=[] :: atom() | []}).
```

Overview
--------

PERSON API manages different setting across phone numbers, rosters and roster contacts.
It is also a linking table for external ids, etc.

Protocol
--------

### GET PERSON

```
1. client sends `{'Person',Id,_,_,_,_,_,_,_,_,_,_,_,_,_,_,get}`
             to `events/1/:node/api/anon/:client/:token` once.
```

```
2. server sends `{'Person',Id,_,_,_,_,_,_,_,_,_,_,_,_,_,_,_}`
             or `{io,{error,not_authorized},<<>>}`
             to `actions/1/api/:client` once.
```
### SET PERSON

```
1. client sends `{'Person',Id,_,_,_,_,_,_,_,_,_,_,_,_,_,_,set}`
             to `events/1/:node/api/anon/:client/:token` once.
```

```
2. server sends `{'Person',Id,_,_,_,_,_,_,_,_,_,_,_,_,_,_,_}`
             or `{io,{error,not_authorized},<<>>}`
             to `actions/1/api/:client` once.
```
