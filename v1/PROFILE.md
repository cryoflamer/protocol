PROFILE: Managing Phone Accounts
================================

Version 1.0 Maxim Sokhatsky

Endpoints
---------

* `actions/1/api/:client` — MQTT
* `events/1/:node/api/anon/:client/:token` — MQTT

Tuples
------

Profile represents Device or Phone for your Accounts visible to network as integer identifiers.
Profile has one-to-one linkage to Person and may hold custom data.

```erlang
-record('Profile',  {phone=[] :: [] | binary(),
                     data=[] :: [] | binary(),
                     person_id=[] :: [] | binary(),
                     accounts=[] :: list(integer()),
                     status=[] :: [] | atom()}).
```

Overview
--------

ROSTER API manages different accounts with different contact lists.

Protocol
--------

### GET PROFILE

```
1. client sends `{'Profile',Phone,_,_,_,get}`
             to `events/1/:node/api/anon/:client/:token` once.
```

```
2. server sends `{'Profile',Phone,_,_,_,_}`
             or `{io,{error,not_authorized},<<>>}`
             to `actions/1/api/:client` nonzero times.
```

### SET PROFILE

```
1. client sends `{'Profile',Phone,_,_,_,set}`
             to `events/1/:node/api/anon/:client/:token` once.
```

```
2. server sends `{'Profile',Phone,_,_,_,_}`
             or `{io,{error,not_authorized},<<>>}`
             to `actions/1/api/:client` nonzero times.
```

### REMOVE PROFILE

```
1. client sends `{'Profile',Phone,_,_,_,remove}`
             to `events/1/:node/api/anon/:client/:token` once.
```

```
2. server sends `{'Profile',Phone,_,_,_,_}`
             or `{io,{error,not_authorized},<<>>}`
             to `actions/1/api/:client` once.
```
