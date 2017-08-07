PROFILE: Managing Phone Accounts
================================

Version 1.0 Maxim Sokhatsky

Endpoints
---------

* `actions/1/api/:client` — MQTT
* `events/1//api/anon/:client/:token` — MQTT

Tuples
------

Profile represents Device or Phone for your Accounts visible to network as integer identifiers.
Profile has one-to-one linkage to Person and may hold custom data.

```erlang
-record('Profile',  {phone=[] :: [] | binary(),
                     data=[] :: [] | binary(),
                     person_id=[] :: [] | binary(),
                     accounts=[] :: list(integer() | #'Roster'{}),
                     update=[] :: [] | integer(),
                     presence=[] :: [] | online | offline | atom(),
                     status=[] :: [] | get | set | remove | atom()}).
```

Overview
--------

ROSTER API manages different accounts with different contact lists.

Protocol
--------

### GET PROFILE

```
1. client sends `{'Profile',Phone,_,_,_,_,get}`
             to `events/1//api/anon/:client/:token` once.
```

```
2. server sends `{'Profile',Phone,_,_,_,_,_}`
             or `{io,{error,not_authorized},<<>>}`
             to `actions/1/api/:client` once.
```

### UPDATE PROFILE

```
1. client sends `{'Profile',Phone,_,_,_,UpdateTime,update}`
             to `events/1//api/anon/:client/:token` once.
```

```
2. server sends `{'Profile',Phone,_,_,_,NewUpdateTime,update}`
             or `{io,{error,not_authorized},<<>>}`
             to `actions/1/api/:client` once.
```

### SET PROFILE

```
1. client sends `{'Profile',Phone,_,_,_,_,set}`
             to `events/1//api/anon/:client/:token` once.
```

```
2. server sends `{'Profile',Phone,_,_,_,NewUpdateTime,update}`
             or `{io,{error,not_authorized},<<>>}`
             to `actions/1/api/:client` once.
```

### REMOVE PROFILE

```
1. client sends `{'Profile',Phone,_,_,_,_,remove}`
             to `events/1//api/anon/:client/:token` once.
```

```
2. server sends `{'Profile',Phone,_,_,_,_,_}`
             or `{io,{error,not_authorized},<<>>}`
             to `actions/1/api/:client` once.
```
