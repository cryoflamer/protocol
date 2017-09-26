PROFILE: Managing Phone Accounts
================================

Version 1.0 Maxim Sokhatsky

Endpoints
---------

* `actions/1/api/:client` — MQTT
* `events/1//api/anon/:client/:token` — MQTT

Records
-------

Profile represents Device or Phone for your Accounts visible to network as integer identifiers.
Profile has one-to-one linkage to Person and may hold custom data.

```erlang
-record('Profile',  {phone=[] :: [] | binary(),
                     data=[] :: [] | binary(),
                     person_id=[] :: [] | binary(),
                     email = [] :: [] | list(#'Email'{}),
                     accounts=[] :: list(integer() | #'Roster'{}),
                     update=0 :: [] | integer(),
                     presence=[] :: [] | atom(),
                     status=[] :: [] | update | get | set | remove
                            | email | confirm | phone }).
```

Overview
--------

ROSTER API manages different accounts with different contact lists.

Protocol
--------

### `Profile/get` — Profile retrival

```
1. client sends `{'Profile',Phone,_,_,_,_,_,_,get}`
             to `events/1//api/anon/:client/:token` once.
```

```
2. server sends `{'Profile',Phone,_,_,_,_,_,_,get}`
             or `{io,{error,not_authorized},<<>>}`
             to `actions/1/api/:client` once.
```

### `Profile/update` — Profile update in friend's rosters

```
1. client sends `{'Profile',Phone,_,_,_,_,UpdateTime,_,update}`
             to `events/1//api/anon/:client/:token` once.
```

```
2. server sends `{'Profile',Phone,_,_,_,_,UpdateTime,_,update}`
             or `{io,{error,not_authorized},<<>>}`
             to `actions/1/api/:client` once.
```

### `Profile/set` — Profile raw set

```
1. client sends `{'Profile',Phone,_,_,_,_,_,_,set}`
             to `events/1//api/anon/:client/:token` once.
```

```
2. server sends `{'Profile',Phone,_,_,_,_,UpdateTime,_,set}`
             or `{io,{error,not_authorized},<<>>}`
             to `actions/1/api/:client` once.
```

### `Profile/link` — Link email to Profile

```
1. client sends `{'Profile',_,_,_,_,_,_,_,mx}`
             to `events/1//api/anon/:client/:token` once.
```

### `Profile/email` — Confirm email

```
1. client sends `{'Profile',_,_,_,Email,_,Time,_,email}`
             to `events/1//api/anon/:client/:token` once.
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

### `Profile/migrate` — Profile migration

```
1. client sends `{'Profile',NewPhone,_,_,_,_,_,Time,_,migrate}`
             to `events/1//api/anon/:client/:token` once.
```

```
2. server sends `{io, Result, <<>>}`
             to `actions/1/api/:client` once.
```

Result:

* `{ok,migration}` — proceed the migration to a new device.

### `Profile/phone` — Confirm Device Migration

```
1. client sends `{'Profile',NewPhone,_,_,_,_,Time,_,phone}`
             or `{io,{error,not_authorized},<<>>}`
             to `actions/1/api/:client` once.
```
