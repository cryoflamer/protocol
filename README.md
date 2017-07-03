NYNJA PROTOCOL SPECIFICATION
============================

Version 1.0 Maxim Sokhatsky

OVERVIEW
--------

To support the development of messaging apps and IOT infrastructure,
NYNJA Protocol is designed from the ground up to be the most efficient,
stable and scalable protocol available.  NYNJA itself is an open-source
specification for Messaging and IOT applications and is a part of an
open source family of N2O protocols. While it can be used with any
transport we promote MQTT as an efficient binary protocol dedicated
for applications over unreliable networks such as GSM, CDMA and other
wireless networks.

NYNJA Protocol enables all companies, regardless of size, to build
systems that can compete with the world’s most sophisticated
communication platforms. NYNJA Protocol has been made open source
for the benefit of the global community of developers. NYNJA Protocol
powers the NYNJA Mobile Communicator.

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
-record('error',    {data=[]}).
-record('ok',       {data=[]}).
-record('io',       {code=[],data=[]}).

-record('Auth',     {token=[], user_id=[], phone=[], dev_key=[],
                     type=[], sms_code=[], attempts=[], services=[]}).

-record('Message',  {id=[], container=Container, feed_id=[], prev=[], next=[], feeds=[],
                     msg_id = [], from = [], to = [],
                     sync = [], % timestamp, last sync
                     created = [], access = [], starred = [],
                     payload = [], mime = [], seen_by = [], status = []}).

-record('History', {roster_id=[],contact_id=[],data=[],status=[]}).

-record('Person',   {id=[], name=[], surname=[], username=[], phonelist=[],
                     alias=[], avatar=[], localize=[], 'NotificationSettings'=[],
                     'SoundSettings'=[], 'ThemeID'=[], 'voxImplantID'=[],
                     'BlockUsers'=[], 'balance'=[], 'isParticipants'=[], status=[]}).

-record('Roster',   {id=[], names=[], surnames=[], user_id=[],
                     size=[], userlist=[], roomlist=[], status=[]})

-record('Profile',  {id=[], phone=[],data=[],accounts=[]}).
-record('Friend',   {id=[], roster_id = [], friend_id = [], status=[]}).
-record('Confirm',  {id=[], roster_id=[], friend_id=[], status=[]}).
-record('Revoke',   {id=[], user=[], status=[]}).
-record('Typing',   {room=[], author=[]}).
-record('Room',     {room=[], description=[], acl=[], settings=[]}).
-record('Join',     {id=[], user=[], room=[], answer=[]}).
-record('Leave',    {id=[], user=[], room=[], answer=[]}).
-record('Approve',  {id=[], user=[], room=[], answer=[]}).

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
