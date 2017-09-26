PRESENCE: Roster Notifications
==============================

Version 1.0 Maxim Sokhatsky

Endpoints
--------

* `actions/1/:node/api/:client` — MQTT

Tuples
------

```erlang
-record('Contact',  {phone_id=[]  :: [] | binary(),
                     avatar=[]    :: [] | binary(),
                     names=[]     :: [] | binary(),
                     surnames=[]  :: [] | binary(),
                     nick= []     :: [] | binary(),
                     email = []   :: [] | binary(),
                     person_id=[] :: [] | binary(),
                     unread=0     :: [] | integer(),
                     last_msg=[]  :: [] | #'Message'{},
                     update=0     :: [] | integer(),
                     presence=[]  :: [] | atom(),
                     status=[]    :: [] | request | authorization
                          | friend | last_msg | atom()}).
```

Overview
--------

PRESENCE protocol is a set of notification messages informing Contacts changing in Rosters.

Protocol
--------

```
1. server issues `{'Contact',Phone,Avatar,_,_,_,_,_,_,_,_,_,_}`
              to `actions/1/api/:client` on avatar changing.
```

```
2. server issues `{'Contact',Phone,_,Names,Surnames,_,_,_,_,_,_,_,_}`
              to `actions/1/api/:client` on name changing.
```

```
3. server issues `{'Contact',Phone,_,_,_,_,_,_,_,_,_,Presence,_}`
              to `actions/1/api/:client` on presence changing.
```

```
4. server issues `{'Contact',Phone,_,_,_,_,_,_,_,_,_,_,Status}`
              to `actions/1/api/:client` on status changing.
```
