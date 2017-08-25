AUTH: Contact Registration
==========================

Version 2.0 Maxim Sokhatsky

Endpoints
--------

* `actions/2/api/:client` — MQTT
* `events/2//api/anon/:client/:token` — MQTT

Tuples
------

Auth tuple represents token storage instances.

```erlang
-record('Auth', {token=[] :: [] | binary(),
                 dev_key=[] :: [] | binary(),
                 user_id=[] :: [] | binary(),
                 phone=[] :: [] | binary(),
                 client_id=[] :: [] | binary(),
                 type=[] :: reg | resend | voice | verify | login | logout,
                 sms_code=[] :: [] | binary(),
                 attempts=[] :: [] | integer(),
                 services=[] :: list(atom()),
                 created = [] :: [] | integer() | binary(),
                 last_online = [] :: [] | integer() | binary()}).
```

AUTH model represents distinct non-duplicating 10-tuples with
single unique key=token which represent session identifier.

Client Id
---------

`<<"emqttd_",Symbols/binary>>`

ClientId resembles all tuple fields to represent a unique
row indexed naturally by token. ClientId is an unique MQTT
session identifier that respawned with clear_session to `false`.
Also ClientId should resemble all session columns:

```
TOKEN PHONE      DEVKEY CLIENTID OS      SERVICES
12131 3800001122 APPLE1 emqttd_1 iOS     [ua]
10293 3800000000 NEXUS1 emqttd_2 Android [jwt]
10294 3800000001 NEXUS2 emqttd_3 Android [jwt]
12323 3800000000 SONY02 emqttd_4 Android [jwt]
```

Overview
--------

AUTH API is dedicated for process of user registration with SMS confirmation.

Protocol
--------

### Registration

```
1. client sends `{'Auth',[],[],[],Phone,[],reg,[],[],[],[],[]}`
             or `{'Auth',[],[],[],Phone,[],reg,[],[],[jwt],[],[]}`
             to `events/2//api/anon/:client/:token` once.
```

```
2. server sends `{io, Result, {'Auth', Token}}`
             to `actions/2/api/:client` once.
```

Result:

* `{ok,sms_sent}` — the genrated SMS code is sent successfully.

### Voice Call

```
1. client sends `{'Auth',Token,[],[],[],[],voice,[],[],[Lang],[],[]}`
             to `events/2//api/anon/:client/:token` once.
```

Lang:

* ua, en, it — ANSI country codes as atoms

```
2. server sends `{io, Result, {'Auth', Token}}`
             to `actions/1/api/:client` once.
```

Result:

* `{ok,call_in_progress}` — Call started
* `{error,session_not_found}` — Auth record not found

### Verify

In case of error client might want to send resend SMS for alredy registered token.

```
1. client sends `{'Auth',Token,[],[],[],[],verify,SMS,[],[],[],[]}`
             or `{'Auth',Token,[],[],[],[],verify,SMS,[],[jwt],[],[]}`
             to `events/2//api/anon/:client/:token` once.
```

```
2. server sends `{io, Result, {'Auth', Token}}`
             to `actions/2/api/:client` once.
```

Result:

* `{ok,login}` — Logged in
* `{error,session_not_found}` — Auth record absent
* `{error,mismatch_user_data}` — Record is found but wrong
* `{error,invalid_sms_code}` — Wrong SMS
* `{error,invalid_jwt_code}` — Wrong SMS if `jwt` in services
* `{error,attempts_expired}` — Hacker?

### Login

```
1. client sends `{'Auth',Token,DevKey,ClientId,Phone,[],login,[],[],[],[],[]}`
             to `events/2//api/anon/:client/:token` once.
```

```
2. server sends `{io, Result, {'Auth', Token}}`
             to `actions/2/api/:client` once.
```

Result:

* `{ok,login}` — Logged in
* `{error,session_not_found}` — Auth record is not found
* `{error,mismatch_user_data}` — Record is found but wrong


### Logout

```
1. client sends `{'Auth',[],[],[],[],[],logout,[],[],[],[],[]}`
             to `events/2//api/anon/:client/:token` once.
```

```
2. server sends `{io, Result, {'Auth', Token}}`
             to `actions/2/api/:client` once.
```

Result:

`{ok,logout}` — Logged out

### Invalid Messages

```
1. client sends `{'Auth',_,_,_,_,_,_,_,_,_,_,_}`
             to `events/2//api/anon/:client/:token` once.
```

```
2. server sends `{io, {error,invalid_data}, {'Auth', Token}}`
             to `actions/2/api/:client` once.
```

