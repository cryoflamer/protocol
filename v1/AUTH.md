AUTH: Contact Registration
==========================

Version 1.0 Maxim Sokhatsky

Endpoints
--------

* `actions/1/api/:client` — MQTT
* `events/1//api/anon/:client/:token` — MQTT

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
1. client sends `{'Auth',Token,_,ClientId,Phone,_,Type,_,_,Services}`
             to `events/1//api/anon/:client/:token` once.
```

```
2. server sends `{io,{ok,    login},              {Table, Token}}` the session is verified.
             or `{io,{ok,    sms_send},           {Table, Token}}` the verified sms is sent successfully.
             or `{io,{ok2,   jwt, JwtCode}},      {Table, Token}}` the verified jwt code is generated if Services = [jwt].
             or `{io,{error, sms_send},           {Table, Token}}` verified sms is not sent successfully.
             or `{io,{error, not_verified},       {Table, Token}}` the session is not verified.
             or `{io,{error, mismatch_user_data}, {Table, Token}}` the devkey and the phone are not matched for the session.
             or `{io,{error, session_not_found},  {Table, Token}}` the session with the token is not found.
             or `{io,{error, miltiple_devices},   {Table, Token}}` more then one sessions with the same device key is found.
             to `actions/1/api/:client` once.
```

### User Confirmation

```
1. client sends `{'Auth',Token,_,ClientId,Phone,_,verify,SMS,_,_}`
             to `events/1//api/anon/:client/:token` once.
```

```
2. server sends `{io,{ok,    login},              {Table, Token}}`
             or `{io,{error, attempts_expired},   {Table, Token}}` the number of attempts is expired
             or `{io,{error, roster_not_found},   {Table, Token}}` nothing rosters found in the user prfile
             or `{io,{error, session_not_found},  {Table, Token}}`
             or `{io,{error, invalid_sms_code},   {Table, Token}}`
             to `actions/1/api/:client` once.
```

### Resend User Confirmation

```
1. client sends `{'Auth',Token,_,ClientId,Phone,_,resend_sms,_,_,_}`
             to `events/1//api/anon/:client/:token` once.
```

```
2. server sends `{io,{ok,    sms_send},          {Table, Token}}`
             or `{io,{error, sms_send},          {Table, Token}}`
             or `{io,{error, roster_not_found},  {Table, Token}}`
             or `{io,{error, session_not_found}, {Table, Token}}`
             to `actions/1/api/:client` once.
```

### Request Voice Call

```
1. client sends `{'Auth',Token,_,_,_,_,voice_call,_,_,language_code}`
             to `events/1//api/anon/:client/:token` once.
```

NOTE! Language code is atom (for example, en, ru).

```
2. server sends `{io,{ok,    login},             {Table, Token}}`
             or `{io,{ok,    call_in_progress},  {Table, Token}}`
             or `{io,{error, session_not_found}, {Table, Token}}`
             to `actions/1/api/:client` once.
```