ROSTER: Managing Contact and Chat Lists
=======================================

Version 1.0 Maxim Sokhatsky

Endpoints
--------

* `actions/api/ClientId` — MQTT
* `events/Node/api/anon/ClientId/Token` — MQTT

Tuples
------

* `{'History',RosterId,UserOrRoomId,[],get}`
* `{'Profile',UserId,Data,Accounts}`
* `{'Roster',Id,UserId,Size,Userlist,RoomList,Status}`
* `{'Person',RosterId, Name, Surname, Username, Phonelist, SoundSettings, ThemeID, VoxImplantID, BlockUsers, Balance, IsParticipants, Status}`

Overview
--------

ROSTER API manages different accounts with different contact lists.

Protocol
--------

### Contact List Retrieval

1. client sends `{'Roster',Id,_,Size,_,_,get}` to `events/Node/api/anon/ClientId/Token` once.
2. server sends `{'Roster',Id,UserId,Size,Userlist,RoomList,Status}` to `actions/api/ClientId` nonzero times.

### Create Account

1. client sends `{'Roster',_,UserId,_,_,_,create}` to `events/Node/api/anon/ClientId/Token` once.
2. server sends `{'Roster',Id,UserId,_,_,_,created}` to `actions/api/ClientId` once.

### Delete Account

1. client sends `{'Roster',Id,_,_,_,_,delete}` to `events/Node/api/anon/ClientId/Token` once.
2. server sends `{'Roster',Id,UserID,0,[],[],deleted}` to `actions/api/ClientId` once.


### Retrieve History

1. client sends `{'History',RosterId,UserOrRoomId,[],get}` to `events/Node/api/anon/ClientId/Token` once.
2. server sends `{'History',RosterId,UserOrRoomId,Messages,{history,1,100}}` to `actions/api/ClientId` once.

### Retrieve/Set Account Profile

1. client sends `{'Profile',UserId, _ ,<<>>}` to `events/Node/api/anon/ClientId/Token` once.
2. server sends `{'Profile',UserId,Data,Accounts}` and
`[{'Person',RosterId, Name, Surname, Username, Phonelist, SoundSettings, ThemeID, VoxImplantID, BlockUsers, Balance, IsParticipants, Status},...]`
to `actions/api/ClientId` once.

### Update Roster

1. client sends `{'Roster',Id,_,Size,Userlist,RoomList,add}` to `events/Node/api/anon/ClientId/Token` once.
2. server sends `{'Roster',Id,UserId,_,Userlist,RoomList,updated}` and
  `[{'Person',RosterId, Name, Surname, Username, Phonelist,[], [], VoxImplantID, [], [], [], []},...]`to `actions/api/ClientId` once.

3. client sends `{'Roster',Id,_,Size,Userlist,RoomList,del}` to `events/Node/api/anon/ClientId/Token` once.
4. server sends `{'Roster',Id,UserId,Size,Userlist,RoomList,updated}` to `actions/api/ClientId` once.


### Retrieve Person

1. client sends `{'Person',RosterId, _, _, _, _,_, _, _, _, _, _, get}` to `events/Node/api/anon/ClientId/Token` once.
2. server sends `{'Person',RosterId, Name, Surname, Username, Phonelist,[], [], VoxImplantID, [], [], [], []}`to `actions/api/ClientId` once.

