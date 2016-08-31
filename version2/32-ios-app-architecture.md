# iOS App Architecture

## Introduction

This document reports on the OS-specific implementation details of the [edu-ID app architecture](30-app-architecture.md) for the iOS platform.

## App organisation

The edu-ID Mobile App is organized into a application logic components as well as three data stores.

The business logic components comprise the following organisation priciples.

* Shared framework components
* Core application components
* App extension components

The data stores include the following structure.

* The edu-ID App Data Store
* The edu-ID Extension Data Store
* The edu-ID Group Data Store

The edu-ID Group Data Store is accessible by the edu-ID Mobile App as well as the edu-ID Extension Data. All data stores are cryptographically protected and all app components use the operating system-level run-time protection.

## App Components

### Shared framework components

The following components are shared between the edu-ID app and the edu-ID app extension.

* Profile Manager
* Protocol discovery
* Service Manager (Personal Filter)

### Core application components

* Service Manager (Token Revocation)

### App extension components

The following components are used only by the app extension.

* Service Authorization
* App Assertion Manager
* App Access Provider

## App Group

The edu-ID app extension can exchange data with the edu-ID Mobile App through a group protected data store.

The iOS application group name is named as following.

``` group.ch.eduid.mobile ```

## Data Stores

Presently, the edu-ID App Data Store and the edu-ID Extension Data Store are not used for storing app related data.

### edu-ID Group Data Store

All data that is used during the app authorization (7) is stored in the edu-ID Group Data Store. The data is stored in a structured data model on top of the CoreData framework's database engine.

## Data Model

The iOS Data Model consists of four relations.

* ``` EduidConfiguration ``` for storing user configurations
* ``` Protocols ``` for maintaining protocol requests.
* ``` Tokens ``` for tracking the different tokens of the app
* ``` UserServices ``` for caching the RSD2 objects for the discovered services.

### EduidConfiguration

The relation consists of key-value tuples.

| Name | Type | Key |
| :---- | :---- | :---- |
| cfg_name | ```String``` | Yes |
| cfg_value | ```String``` | No |

### Protocols

The relation consists of key-value tuples, where the key provides a reference to a UserService RSD2 object.

| Name | Type | Key |
| :---- | :---- | :---- |
| baseurl | ```String``` | Yes |
| protocol_id | ```String``` | No |

### Tokens

A token contains a tuple of four elements that describe the type, target, and authorization token.

| Name | Type | Key |
| :---- | :---- | :---- |
| subject | ```String``` | Yes |
| target | ```String``` | Yes |
| type | ```String``` | Yes |
| token | ```String``` | No |

### UserService

The UserService relation provides processed information of the RSD2 object for a Federation Service.

| Name | Type | Key |
| :---- | :---- | :---- |
| baseurl | ```String``` | Yes |
| name | ```String``` | No |
| rsd | ```String``` | No |
| token | ```String``` | No |
| token_target | ```String``` | No |

The ```name``` field contains the display name for the Federation Service. This information is used by the Service Manager component.

The ```token``` field contains the service authorization token for a UserService.

The ```token_target``` field contains the OAuth2 endpoint.

The ```rsd``` field contains the RSD2 object as it has been provided by the protocol discovery.
