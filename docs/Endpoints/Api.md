This document describes the format of the Appclusive REST API

# Introduction

Appclusive exposes its functionality via an [ODATA v3](http://www.odata.org/documentation/odata-version-3-0/) compatible [REST](https://en.wikipedia.org/wiki/Representational_state_transfer) API. So instead of repeating the way how ODATA works in this document we will describe how Appclusive models it entities and advertises its services.

# Authentication

Appclusive supports different authentication mechanisms independent of the underlying transport protocol which can be extended by a plugins. Currently the following authentication means are available:

* Windows Integrated, ActiveDirectory  (via `Authorization` header)
* [JWT](https://jwt.io/)  (via separate user defined HTTP header)
* [OAUTH2](https://oauth.net/2/) (via `Authorization` header with `Bearer` token)

Appclusive does not act as an Identity Provider but relies on external systems to provide that functionality.

For more information about Authentication see the chapter about [Security](../Security).

# Uri Format

Appclusive distributes its functionality across several separate *endpoints*:

* Diagnostics

	`httpS://appclusive.example.com/api/Diagnostics`

* Core

	`httpS://appclusive.example.com/api/Core`

* Model

	`httpS://appclusive.example.com/api/Model`

These endpoints can be extended by means of plugins. Depending on the endpoint several entity sets are accessible. To determine the exact functionality each endpoints exposes its *schema* via `httpS://appclusive.example.com/api/endpoint/$metadata`.

For a specific entity set consult the respective documentation.

**DFTODO** list entity sets per endpoint

# API Operations

In general each entity set supports basic [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) operations:

## Read

### List

Retrieving entities is performed by an `HTTP GET` operation. ODATA filter operators and paging semantics apply.

Example: `GET httpS://appclusive.example.com/api/Core/KeyNameValues`

`HTTP 200` is the usual response when entities are returned. The response body will then contain a list of entities that match the request criteria.

Note: if no entities are found that match the request criteria an *empty* list is returned. The response will still be `HTTP 200`.

### Single

To retrieve a single entity the entity *id* is placed in brackets after the entity set:

Example: `GET httpS://appclusive.example.com/api/Core/KeyNameValues(42L)`

`HTTP 200` is the response and the response body will contain the entity specified by its `id`. If no entity exists the response will be `HTTP 404`.

Note: all *ids* are of type `long` (or `int64`), so they should be suffixed by a capital `L`.

Note: if a client requests an entity from a different tenant where the client does not have permissions to retrieve that entity, the HTTP response will also be `HTTP 404` (to prevent information disclosure).

## Create

Creating an entity is normally performed by an `HTTP POST` operation. 

Example: `POST httpS://appclusive.example.com/api/Core/KeyNameValues`

The body of the HTTP requests depends on the actual entity set (which can be determined by the aforementioned `$metadata` schema.

`HTTP 201` indicates that the entity has been created. The response body will then contain the new entity.

`HTTP 202` indicates that the operation has been accepted, but has not been completed yet. The `Location` header of the HTTP response will contain an `Uri` to a `Job` entity which can be tracked by the client.

Note: when requesting the instantiation of [Blueprints](./endpoints/Core/Catalogue/Blueprint.md) the actual creation is not performed *directly* via `HTTP POST`, but performed via an [Order](./endpoints/Core/Catalogue/Order.md).

## Update

Updating existing entities can be performed in two different ways:

### Full Update

An `HTTP PUT` operation expects the complete entity in its HTTP body (regardless of whether an attribute has changed or not).

Example: `PUT httpS://appclusive.example.com/api/Core/KeyNameValues(42L)`

### Partial Update

With `HTTP PATCH` the HTTP body may only contain the actual attributes that have changed and should be updated, eg. the `Name` of a `KeyNameValue`.

Example: `PATCH httpS://appclusive.example.com/api/Core/KeyNameValues(42L)`

``` json
{
  "Name" : "Updated Name"
}
```

`HTTP 204` indicates a successful update operation where no content is returned in the response body. To get the updated entity (especially useful with `PATCH` operations, a new `GET` request must be performed.

Note1: do *NOT* use `HTTP MERGE`, but always use `HTTP PATCH`.
Note2: Appclusive uses [*upper* camel case](https://en.wikipedia.org/wiki/Camel_case).

## Delete

To delete an entity an `HTTP DELETE` must be performed:

Example: `DELETE httpS://appclusive.example.com/api/Core/KeyNameValues(42L)`

`HTTP 204` indicates a successful delete operation where no content is returned in the response body.
