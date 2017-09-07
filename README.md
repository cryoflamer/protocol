NYNJA PROTOCOL SPECIFICATION
============================

Version 1.0 Maxim Sokhatsky

Overview
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

Features
--------

* Media: Voice, Video, File Transfer, GPS, Text
* Multi-User Conferences (Tribes)
* Multi Accounts (Rosters)

Intro
-----

* Messages: Schema Definition
* Endpoints: Topic Subscriptions (MQTT), WebSockets (WS)

Topics
------

### N2O RPC Topics

* `actions/:vsn/:module/:client_id` — Client Topic
* `events/:vsn/:node/:module/:username/:client_id/:token` — Server Topic

The `actions/:vsn/:module/:client_id` topic is subscribed when connection established. This is done on server so client don't need to subscribe to this topic. All incoming mesages come to this topic.

The `events/:vsn/:node/:module/:username/:client_id/:token` topics are equal number of cores. Client sends API requesus to one of these topics and listen for answers on 'actions' topic.

### ROSTER Topics

* `ses/:phone` — Devices Broadcast
* `ac/:phone_roster` — Friendship Broadcast
* `p2p/:phone_roster/:phone_roster` — Private Chat
* `tribe/:name` — Multiuser Chat

The `ses/:phone` topic is dedicated for accumulating all device sessions under the single topic indexed by the phone. If you send to this topic, all devices of the given phone will receive the message. If you have no right to send to this phone nothing will happens. New devices should be subscribed to this topic on registration.

The `ac/:phone_roster` topic is representing the subscription mesh, base on friendship logic. If you send to this topic, all devices of your friends will recieve this message. For sure server will strict you from sending to other topic than yours. New devices of friends should be subscribed to this topic on registration.

The `p2p/:phone_roster/:phone_roster` topic is representing the private chat between two users. The name of the topic is constructed from two sorted roster identifiers, e.g. `380670001234_12525/380670002234_12334`, left phone is always less or equal then right. If you send to this topic, all devices of two counterparties will recieve the message. If you are not owner of one of these rosters, nothing will happen. New devices of counterparties should be subscribed to this topic on registration.

Sub Protocols
-------------

* [IO](https://github.com/NYNJA-MC/protocol/blob/master/v1/IO.md) — 1
* [INIT](https://github.com/NYNJA-MC/protocol/blob/master/v1/INIT.md) — 1
* [AUTH](https://github.com/NYNJA-MC/protocol/blob/master/v1/AUTH.md) — 5
* [PERSON](https://github.com/NYNJA-MC/protocol/blob/master/v1/PERSON.md) — 2
* [PROFILE](https://github.com/NYNJA-MC/protocol/blob/master/v1/PROFILE.md) — 3
* [PRESENCE](https://github.com/NYNJA-MC/protocol/blob/master/v1/PRESENCE.md) — 3
* [ROSTER](https://github.com/NYNJA-MC/protocol/blob/master/v1/ROSTER.md) — 8
* [MESSAGE](https://github.com/NYNJA-MC/protocol/blob/master/v1/MESSAGE.md) — 3
* [FRIEND](https://github.com/NYNJA-MC/protocol/blob/master/v1/FRIEND.md) — 4
* [TRIBE](https://github.com/NYNJA-MC/protocol/blob/master/v1/TRIBE.md) — 6
* [SEARCH](https://github.com/NYNJA-MC/protocol/blob/master/v1/SEARCH.md) — 1
* [FTP](https://github.com/NYNJA-MC/protocol/blob/master/v1/FTP.md) — 1

Client API RPC Specification
----------------------------

* `Message/client` — Publishing Message to Subscribers
* `Message/edit` — Edit/Remove Message
* `History/get` — Retrieve History from Message stream
* `Cursor/` — Set cursor for Message stream
* `Person/get` — Get Person settings by Phone
* `Person/set` — Set Person settings
* `Profile/get` — Profile retrival by Phone
* `Profile/update` — Profile update
* `Profile/set` — Profile set
* `Profile/remove` — Remove Profile with all Rosters and Auth tokens
* `Roster/get` — Get Roster by Id
* `Roster/update` — Update Roster
* `Roster/set` — Set Roster
* `Roster/remove` — Remove Roster
* `Roster/create` — Create Roster
* `Roster/list` — List Rosters
* `Roster/add` — Add Contacts to Roster
* `Roster/del` — Delete Contacts from Roster
* `Search/contact` — Search Contacts across NYNJA database
* `Auth/reg` — Registration
* `Auth/resend` — Resend SMS
* `Auth/verify` — Verify by SMS
* `Auth/voice` — Voice by Voice
* `Auth/logout` — Logout
* `Auth/push` — Save Apple or Google Push token to Auth record
* `Friend/request` — Friendship Request
* `Friend/confirm` — Confirm friendship
* `Friend/revoke` — Revoke friendship

Payloads
--------

Payloads are MIME-driven, so Video, Audio and Text messages correspond to MIME types.
**Events** come from server and is a subscription topic for client,
**Actions** come from client and is a publishing topic for client.
This is a front channel for raw BERT messages.
Other topics may bind to event or action topics.

Credits
-------

* Marshall Taplits
* Maxim Sokhatsky
* Yuri Maslovsky
* Igor Kharin
* Liubov Mykhailova
