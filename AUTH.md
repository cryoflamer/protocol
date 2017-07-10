AUTH: Contact Registration
==========================

Version 1.0 Maxim Sokhatsky

Endpoints
--------

* `actions/api/ClientId` — MQTT
* `events//api/anon/ClientId/Token` — MQTT

Tuples
------

* `{'Auth',DevKey,UserId,Phone,Token,ClientId,Type,SmsCode,Attempts,Services}`

Overview
--------

AUTH API is dedicated for process of user registration with SMS confirmation.

Protocol
--------

### Auth Issuing / SMS sending

1. client sends `{'Auth',DevKey,_,Phone,Token,_,_,_,_,_}` to `events//api/anon/ClientId/Token` once. `DevKey` MUST be unique.
2. server sends `{io,{ok, login},{'Token',Token}}`
             or `{io,{ok, sms_send},{'Token',Token}}`
             or `{io,{error, sms_send},{'Token',Token}}`
             or `{io,{error, not_verified},{'Token',Token}}`
             or `{io,{error, mismatch_user_data},<<>>}`
             or `{io,{error, session_not_found},<<>>}`
             or `{io,{error, miltiple_devices},<<>>}}`
             to `actions/api/ClientId` once.

### User Confirmation

1. client sends `{'Auth',_,_,_,Token,_,verify,SMS,_,_}` to `events//api/anon/ClientId/Token` once.
2. server sends `{io,{ok, login},{'Token',Token}}`
             or `{io,{error, attempts_expired},{'Token',Token}}`
             or `{io,{error, roster_not_found},<<>>}`
             or `{io,{error, session_not_found},<<>>}`
             or `{io,{error, invalid_sms_code}, {'Token',Token}}`
             to `actions/api/ClientId` once.

### Resend User Confirmation

1. client sends `{'Auth',_,_,_,Token,_,resend_sms,_,_,_}` to `events//api/anon/ClientId/Token` once.
2. server sends `{io,{ok, sms_send},{'Token',Token}}`
             or `{io,{error, sms_send},{'Token',Token}}`
             or `{io,{error, roster_not_found},<<>>}`
             or `{io,{error, session_not_found}, <<>>}`
             to `actions/api/ClientId` once.
