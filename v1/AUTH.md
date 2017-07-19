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
2. server sends `{io,{ok,    login},              {Table, Token}}` 
             or `{io,{ok,    sms_send},           {Table, Token}}` 
             or `{io,{ok2,   jwt, JwtCode},       {Table, Token}}` 
             or `{io,{error, sms_send},           {Table, Token}}` 
             or `{io,{error, not_verified},       {Table, Token}}`
             or `{io,{error, mismatch_user_data}, {Table, Token}}` 
             or `{io,{error, session_not_found},  {Table, Token}}` 
             or `{io,{error, miltiple_devices},   {Table, Token}}` 
             to `actions/1/api/:client` once.
```

* `{ok,login}` — the session is verified.
* `{ok,sms_send}` — the verified sms is sent successfully.
* `{ok2,jwt,Code}` — the verified jwt code is generated if Services = [jwt].
* `{error,sms_send}` — verified sms is not sent successfully.
* `{error,not_verified}` —  the session is not verified.
* `{error,mismatch_user_data}` — the devkey and the phone are not matched for the session.
* `{error,session_not_found}` — the session with the token is not found.
* `{error,miltiple_devices}` — more then one sessions with the same device key is found.

### User Confirmation

```
1. client sends `{'Auth',Token,_,ClientId,Phone,_,verify,SMS,_,_}`
             to `events/1//api/anon/:client/:token` once.
```

```
2. server sends `{io,{ok,    login},              {Table, Token}}`
             or `{io,{error, attempts_expired},   {Table, Token}}`
             or `{io,{error, roster_not_found},   {Table, Token}}` 
             or `{io,{error, session_not_found},  {Table, Token}}`
             or `{io,{error, invalid_sms_code},   {Table, Token}}`
             to `actions/1/api/:client` once.
```

* `{error,attempts_expired}` — the number of attempts is expired
* `{error,roster_not_found}` — nothing rosters found in the user profile

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
