# Native Application Integration Layer

## Introduction

The Native Application Integration Layer abstracts OAuth2-specific, authentication protocol specific and operating system specific interactions for third party applications that run natively on the users' devices.

Apps are expected to rely on public protocols

The NAIL is designed to facilitate simultaneous connections to several service endpoints. Apps that can handle only one simultaneous authorization MAY limit this support. Otherwise it is always safe to assume that average users in the Swiss academic domain have access to more than one Federation Service that meets an app's protocol requirements.

While NAIL supports the protocol of the Swiss edu-ID Mobile App, it is agnostic to the authorizing app. The only requirement is that the authorizing app uses the same data formats.

## NAIL API Overview

The NAIL API supports two interactions with the edu-ID trust domains. From the business logic perspective of a third party app these two interactions relate to:

1. Protocol authorization (7), and
2. Protocol handling (9)

The NAIL API __does not__ facilitate or validate protocol requests. It relies on the third party app's capabilities to handle protocol specifications appropriately.

The NAIL API separated into two components.

* The NAIL Interface API is defines OS-specific interactions and data structures that are required to exchange data with the edu-ID Mobile App or Federation Services. The Interface API handles three tasks:

    1. The OS-level data exchange with the edu-ID Mobile App (7).
    2. Management of authorization data including protocol endpoints (9).
    3. Token renewal (9)

* The NAIL Framework API is the primary interface for third party app developers and abstracts the NAIL Interface API so developers can focus on their business logic. The NAIL Framework API abstract the NAIL Interface API into common function calls that are shared across platforms. At this level the NAIL Framework API supports six tasks:

     1. Protocol authorization and endpoint discovery (7)
     2. Service discovery (7*)
     3. Endpoint URL generation during request preparation (9)
     4. Authorization token generation during request preparation (9)
     5. Token renewal (9)
     6. Data serialization for data persistency

![edu-ID NAIL Architecture Overview](images/eduid_app_nail_architecture-color.png)

Developers of third party apps will typically only use the NAIL Framework API.

## NAIL Framework API

The NAIL Framework API abstracts all protocol independent interface functions that are required for authorized protocol requests. It does not provide any logic for performing protocol requests. This allows developers to choose appropriate channels for authorized interactions.

The NAIL Framework API provides a singleton class interface.

The protocol authorization and endpoint discovery is supported by the API call.

```authorizeProtocols(protocolList) -> successPromise```

This call abstract the OS-level data exchange with an authorizing mobile app, such as the edu-ID Mobile App. As the data exchange

## NAIL Interface API

### Protocol Authorization

The NAIL protocol requires an app to present a set of information, including but not limited to its bundle id and name.



### Protocol handling.

### Access Restrictions

Scoped requests

- Apps may not access endpoints outside their scope

Simple Bearer Tokens Handling

Refreshing Access Tokens


## Javascript Integration for Apache Cordova

