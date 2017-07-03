NYNJA PROTOCOL SPECIFICATION
============================

Version 1.0 Maxim Sokhatsky

OVERVIEW
--------

The NYNJA Protocol is designed from the ground up to be the most efficient, stable and scalable protocol available to support the development of messaging apps and IOT infrastructure. NYNJA itself is an open-source specification for Messaging applications and is a part of an open source family of N2O protocols. While it can be used with any transport we promote MQTT as an most efficiend binary protocol dedicated for applications over unreliable networks such as GSM, CDMA an other wireless networks.

NYNJA Protocol enables all companies regardless of size to build systems which can compete with the most sophisticated communication platforms on the market. The NYNJA Protocol powers the NYNJA Mobile Communicator and the protocol is open source for the benefit of the global community of developers to use and contribute to.

FEATURES
--------

* Presence
* Voice
* Video
* File Transfer
* GPS
* Text
* Multi-User Conferences
* Multi Accounts

INTRO
-----

* Messages: Schema Definition
* Endpoints: Topic Subscriptions (MQTT), WebSockets (WS)

URI Components
--------------

### Client

* `actions/index/emqttd_198234215548221`

### Server:

* `events/3/index/maxim@synrc.com/emqttd_198234215548221`
* `events/2/login/anon/emqttd_198234215548221`

### Review Application:

* `room/n2o`

Metawords
---------

* `randomly` — may be received by client randomly
* `once` — should be received once according to sub-protocol section
* `issues` — may be send randomly.
* `sends` — ususal sending
* `participants` — depending on number of mates

Sub Protocols
-------------

* [INIT](https://github.com/NYNJA-MC/protocol/blob/master/INIT.md) — 1
* [FTP](https://github.com/NYNJA-MC/protocol/blob/master/FTP.md) — 1
* [AUTH](https://github.com/NYNJA-MC/protocol/blob/master/AUTH.md) — 2
* [PUBLIC](https://github.com/NYNJA-MC/protocol/blob/master/PUBLIC.md) — 7
* [PRIVATE](https://github.com/NYNJA-MC/protocol/blob/master/PRIVATE.md) — 4
* [PRESENCE](https://github.com/NYNJA-MC/protocol/blob/master/PRESENCE.md) — 2
* [ROSTER](https://github.com/NYNJA-MC/protocol/blob/master/ROSTER.md) — 5

![SCHEMA](https://github.com/NYNJA-MC/protocol/blob/master/roster.png)

Messages
--------

```erlang
-record('Token',    {data=[]}).
-record('Auth',     {username=[], token=[], services=[]}).
-record('Person',   {id=[], name=[], surname=[], username=[], status=[]}).
-record('Roster',   {size=[], userlist=[]}). % server to client message
-record('Friend',   {id=[], user=[], status=[]}).
-record('Confirm',  {user=[], type=[]}).
-record('Private',  {id=[], recipient=[], body=[], author=[], status=[]}).
-record('Typing',   {room=[], author=[]}).
-record('Room',     {room=[], description=[], acl=[], settings=[]}).
-record('Join',     {user=[], room=[], answer=[]}).
-record('Public',   {id=[], room=[], message=[], author=[], status=[]}).
```

Payloads
--------

Payloads are MIME-driven, so Video, Audio and Text messages correspond to MIME types. **Events** come from server and is a subscription topic for client, **Actions** come from client and is a publishing topic for client. This is a front channel for raw BERT messages. Other topics may bind to event or action topics.

Credits
-------

* Marshall Taplits
* Maxim Sokhatsky
* Yuri Maslovsky
* Igor Kharin
* Liubov Mykhailova
