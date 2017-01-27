This document describes the format of the Appclusive REST API

# Introduction

Appclusive exposes its functionality via an [ODATA v3](http://www.odata.org/documentation/odata-version-3-0/) compatible [REST](https://en.wikipedia.org/wiki/Representational_state_transfer) API. So instead of repeating the way how ODATA works in this document we will describe how Appclusive models it entities and advertises its services.

# Authentication

Appclusive support different authentication mechanisms independent of the transport protocol which can be extended by a plugins. Currently the following authentication means are available:

* Windows Integrated, ActiveDirectory  (via `Authorization` header)
* JWT  (via separate user defined HTTP header)
* OAUTH2 (via `Authorization Bearer` header)

[...]
