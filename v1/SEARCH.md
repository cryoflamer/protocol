SEARCH: Directory Search
========================

Version 1.0 Maxim Sokhatsky

Endpoints
--------

* `actions/1/api/phone/:phone` — MQTT
* `actions/1/api/:client` — MQTT
* `events/1//api/anon/:client/:token` — MQTT

Tuples
------


```erlang
-record('Search',   {id = [] :: [] | integer(),
                     phone = [] :: [] | list(binary()),
                     names = [] :: [] | binary(),
                     surnames = [] :: [] | binary(),
                     query = [] :: [] | binary(),
                     status = [] :: [] | contact | atom()}).
```

Overview
--------

User emits Search request and gets Contacts (which represents
the found rosters) packed in Roster message (the search result envelop).

Protocol
--------

### `Search/contact` — Search Contacts

```
1. client sends `{'Search,RosterId,Phone,_,_,[],contact}`
             to `events/1//api/anon/:client/:token` once.
```

```
2. server sends `{'Roster,Id,_,_,_,UserList,_,_,_,_,_,_}`
             or `{io,{error,profile_not_found},<<>>}`
             or `{io,{error,roster_not_found},<<>>}`
             or `{io,{error,not_authorized},<<>>}`
             to `actions/1/api/:client` once.
```
