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
                     presence=[] :: [] | online | offline,
                     status=[] :: [] | get | set | remove | remove }).
```

Overview
--------

ROSTER API manages different accounts with different contact lists.

Protocol
--------

### `Profile/get` — Profile retrival

```
1. client sends `{'Profile',Phone,_,_,_,_,_get}`
             to `events/1//api/anon/:client/:token` once.
```

```
2. server sends `{'Profile',Phone,_,_,_,_,_,_}`
             or `{io,{error,not_authorized},<<>>}`
             to `actions/1/api/:client` once.
```

### `Profile/update` — Profile update

```
1. client sends `{'Profile',Phone,_,_,_,UpdateTime,_,update}`
             to `events/1//api/anon/:client/:token` once.
```

```
2. server sends `{'Profile',Phone,_,_,_,NewUpdateTime,_,update}`
             or `{io,{error,not_authorized},<<>>}`
             to `actions/1/api/:client` once.
```

### `Profile/set` — Profile set

```
1. client sends `{'Profile',Phone,_,_,_,_,_,set}`
             to `events/1//api/anon/:client/:token` once.
```

```
2. server sends `{'Profile',Phone,_,_,_,NewUpdateTime,_,update}`
             or `{io,{error,not_authorized},<<>>}`
             to `actions/1/api/:client` once.
```

### `Profile/remove` — Profile remove

```
1. client sends `{'Profile',Phone,_,_,_,_,_,remove}`
             to `events/1//api/anon/:client/:token` once.
```

```
2. server sends `{'Profile',Phone,_,_,_,_,_,_}`
             or `{io,{error,not_authorized},<<>>}`
             to `actions/1/api/:client` once.
```
