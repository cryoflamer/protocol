AUTH: Contact Registration
==========================

Version 1.0 Maxim Sokhatsky

Endpoints
--------

* `actions/1/api/ClientId` — MQTT
* `events/1//api/anon/ClientId/Token` — MQTT

Tuples
------

Auth tuple represents token storage instances.

```erlang
-record('Auth',     {token=[] :: [] | binary(),
                     dev_key=[] :: [] | binary(),
                     user_id=[] :: [] | binary(),
                     phone=[] :: [] | binary(),
                     client_id=[] :: [] | binary(),
                     type=[] :: [] | atom(),
                     sms_code=[] :: [] | binary(),
                     attempts=[] :: [] | integer(),
                     services=[] :: list()}).
```

Overview
--------

AUTH API is dedicated for process of user registration with SMS confirmation.

Protocol
--------

### Auth Issuing / SMS sending

```
1. client sends `{'Auth',DevKey,_,Phone,Token,_,_,_,_,_}`
             to `events//api/anon/ClientId/Token` once. `DevKey` MUST be unique.
```

```
2. server sends `{io,{ok,    login},              {Table, Token}}`
             or `{io,{ok,    sms_send},           {Table, Token}}`
             or `{io,{error, sms_send},           {Table, Token}}`
             or `{io,{error, not_verified},       {Table, Token}}`
             or `{io,{error, mismatch_user_data}, {Table, Token}}`
             or `{io,{error, session_not_found},  {Table, Token}}`
             or `{io,{error, miltiple_devices},   {Table, Token}}`
             to `actions/api/ClientId` once.
```

### User Confirmation

```
1. client sends `{'Auth',_,_,_,Token,_,verify,SMS,_,_}`
             to `events//api/anon/ClientId/Token` once.
```

```
2. server sends `{io,{ok,    login},              {Table, Token}}`
             or `{io,{error, attempts_expired},   {Table, Token}}`
             or `{io,{error, already_registered}, {Table, Token}}`
             or `{io,{error, roster_not_found},   {Table, Token}}`
             or `{io,{error, session_not_found},  {Table, Token}}`
             or `{io,{error, invalid_sms_code},   {Table, Token}}`
             to `actions/api/ClientId` once.
```

### Resend User Confirmation

```
1. client sends `{'Auth',_,_,_,Token,_,resend_sms,_,_,_}`
             to `events//api/anon/ClientId/Token` once.
```

```
2. server sends `{io,{ok,    sms_send},          {Table, Token}}`
             or `{io,{error, sms_send},          {Table, Token}}`
             or `{io,{error, roster_not_found},  {Table, Token}}`
             or `{io,{error, session_not_found}, {Table, Token}}`
             to `actions/api/ClientId` once.
```
