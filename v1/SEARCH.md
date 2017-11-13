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
-record('Search',   {id       =[] :: [] | integer(),
                     phone    =[] :: [] | list(binary()),
                     names    =[] :: [] | binary(),
                     surnames =[] :: [] | binary(),
                     query    =[] :: [] | binary(),
                     status   =[] :: [] | contact | qrcode | nick
                                        | names | phone | phonebook }).
```

Overview
--------

User emits Search request and gets Contacts (which represents
the found rosters) packed in Roster message (the search result envelop).

Protocol
--------

### `Search/contact` — Search Contacts

```
1. client sends `{'Search,RosterId,Ref,Field,'==',Value,Status}`
             to `events/1//api/anon//` once.
```

```

2. server sends `{io, {ok, Ref},{'Roster,Id,_,_,_,UserList,_,_,_,_,_,_,Status}}`
             to `actions/1/api/:client` once.
```

