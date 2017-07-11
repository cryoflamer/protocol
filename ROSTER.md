ROSTER: Managing Contact and Chat Lists
=======================================

Version 1.0 Maxim Sokhatsky

Endpoints
--------

* `actions/api/ClientId` — MQTT
* `events/Node/api/anon/ClientId/Token` — MQTT

Tuples
------

The Roster class represents User Impersonified Account or
Contact Book or Contact List of the User. Each account connected to single
Phone or Profile or Device.

```erlang
-record('Roster',   {id=[] :: [] | integer(),
                     names=[] :: [] | binary(),
                     surnames=[] :: [] | binary(),
                     size=0 :: integer(),
                     userlist=[] :: list(#'Contact'{}),
                     roomlist=[] :: list(#'Room'{}),
                     subscribe=true :: boolean(),
                     phone=[] :: [] | binary(),
                     status=[] :: [] | atom()}).
```

Overview
--------

ROSTER API manages different accounts with different contact lists.

Protocol
--------

### GET ROSTER

1. client sends `{'Roster,Id,_,_,_,_,_,_,_,get}` to `events/Node/api/anon/ClientId/Token` once.
2. server sends `{'Roster,Id,_,_,_,_,_,_,_,_}`
             or `{io,{error,roster_not_found},<<>>}`
             or `{io,{error,not_authorized},<<>>}`
             to `actions/api/ClientId` nonzero times.

### SET ROSTER

1. client sends `{'Roster,Id,_,_,_,_,_,_,_,set}` to `events/Node/api/anon/ClientId/Token` once.
2. server sends `{'Roster,Id,_,_,_,_,_,_,_,_}`
             or `{io,{error,not_authorized},<<>>}`
             to `actions/api/ClientId` nonzero times.

### REMOVE ROSTER

1. client sends `{'Roster,Id,_,_,_,_,_,_,_,remove}` to `events/Node/api/anon/ClientId/Token` once.
2. server sends `{io,{error,not_authorized},<<>>}`
             or `{io,{error,profile_not_found},<<>>}`
             or `{io,{error,roster_not_found},<<>>}`
             or `{io,{ok,removed},<<>>}`
             to `actions/api/ClientId` nonzero times.

### CREATE ROSTER

1. client sends `{'Roster,Id,_,_,_,_,_,_,_,create}` to `events/Node/api/anon/ClientId/Token` once.
2. server sends `{'Roster,Id,_,_,_,_,_,_,_,_}`
             or `{io,{error,not_authorized},<<>>}`
             to `actions/api/ClientId` nonzero times.

### LIST ROSTERS

1. client sends `{'Roster,_,_,_,_,_,_,_,Phone,list}` to `events/Node/api/anon/ClientId/Token` once.
2. server sends `{io,List,<<>>}`
             or `{io,{error,not_authorized},<<>>}`
             to `actions/api/ClientId` nonzero times.

### ADD ROSTER CONTACTS

1. client sends `{'Roster,Id,_,_,_,List,_,_,_,add}` to `events/Node/api/anon/ClientId/Token` once.
2. server sends `{io,{error,roster_not_found},<<>>}`
             or `{io,{error,not_authorized},<<>>}`
             or `{io,{ok,added},<<>>}`
             or `{io,{ok,{already_present,_}},<<>>}`
             to `actions/api/ClientId` nonzero times.

### DELETE ROSTER CONTACTS

1. client sends `{'Roster,Id,_,_,_,List,_,_,_,del}` to `events/Node/api/anon/ClientId/Token` once.
2. server sends `{io,{error,roster_not_found},<<>>}`
             or `{io,{error,not_authorized},<<>>}`
             or `{io,{error,contacts_not_found},<<>>}`
             or `{io,{ok,removed},<<>>}`
             to `actions/api/ClientId` nonzero times.
