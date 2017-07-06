AUTH: Contact Registration
==========================

Version 1.0 Maxim Sokhatsky

Endpoints
--------

* `actions/api/ClientId` — MQTT
* `events/Node/api/anon/ClientId/Token` — MQTT

Tuples
------

* `{'Auth',Token,UserId,Phone,DevKey,ClientId,Type,SmsCode,Attempts,Services}`

Overview
--------

AUTH API is dedicated for process of user registration with SMS confirmation.

Protocol
--------

### Auth Issuing / SMS sending

1. client sends `{'Auth',_,_,Phone,_,_,_,_,_,_}` to `events/Node/api/anon/ClientId/Token` once.
2. server sends `{io,{ok, login},{'Token',Token}}`
             or `{io,{ok, sms_send},{'Token',Token}}`
             or `{io,{error, not_verified},<<>>}`
             or `{io,{error, mismatch_user_data},<<>>}`
             or `{io,{error, Error},{'Token',Token}}}`
             to `actions/api/ClientId` once.

### User Confirmation

1. client sends `{'Auth',Token,_,Phone,_,_,verify,SMS,_,_}` once.
2. server sends `{io,{ok, login},{'Token',Token}}`
             or `{io,{error, attempts_expired},{'Token',Token}}`
             or `{io,{error, roster_not_found},<<>>}`
             or `{io,{error,invalid_sms_code}, {'Token',Token}}`
             to `actions/api/ClientId` once.

### Resend User Confirmation

1. client sends `{'Auth',_,_,_,_,_,resend_sms,SMS,_,_}` once.
2. server sends `{io,{ok, sms_send},{'Token',Token}}`
             or `{io,{error, roster_not_found},<<>>}`
             or `{io,{error, session_not_found}, <<>>}`
             to `actions/api/ClientId` once.
