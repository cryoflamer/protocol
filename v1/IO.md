IO: Exceptions and Effects Protocol
===================================

Version 1.0 Maxim Sokhatsky

Endpoints
--------

* `actions/1/:node/api/:client` — MQTT

Tuples
------

```erlang
-record(error, { code=[] :: [] | binary() }).
```

```erlang
-record(ok, { code=[] :: [] | binary() }).
```

```erlang
-record(io, { code=[] :: [] | #'Ok'{} | #'Error'{},
              data=[] :: [] | <<>> | { atom(), binary() | integer() } }).
```

Overview
--------

IO PROTOCOL is intented to transport return codes, JavaScript acctions, Exceptions and other server effects.

Protocol
--------

```
1. server issues `{io,_,_}`
              to `actions/1/api/:client` randomly.
```
