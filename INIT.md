INIT: Device Initialization
===========================

Version 1.0 Maxim Sokhatsky

Endpoints
--------

* `actions/api/ClientId` — MQTT
* `events/Node/api/anon/ClientId/Token`

Tuples
------

* `{vnode_max, VNode}`
* `{init,Token}`
* `{'Token',Token}`
* `{io,Actions,TokenRec}`

Overview
--------

INIT API is dedicated for initial device token issuing or initialization.
You may think of it as cookie string that can be used to identify
storage of backend variables per EMQ session. EMQ session can be stored
inside this token.

Protocol
--------

1. client connects to `ns.synrc.com:8083` port.
2. client subscribes to `actions/api/ClientId` automatically.
3. server send `{vnode_max, VNode}` to `actions/api/ClientId` automatically.
4. client sends `{init,Token}` to `events/2/api/anon/ClientId` automatically.
5. server sends `{io,Actions,{'Token',Token}}` to `actions/api/ClientId/VNode` once.
