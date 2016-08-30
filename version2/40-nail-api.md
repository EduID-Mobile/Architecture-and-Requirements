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

```authorizeProtocols(protocolList) -> completionPromise```
```authorizeProtocols(protocolList, singleton) -> completionPromise```

This method implements the protocol authorization. It abstracts the OS-level data exchange with an authorizing mobile app, such as the edu-ID Mobile App. The completion of the request is asynchroneous. Therefore, this method requires a completion handler to return the result to the app's business logic.

The method expects a list of protocol names as the first parameter.

The two parameter variant expects a boolean value to indicate if the business logic can handle only one (1) service authorization.

The completion handler has no parameter.

```serviceNames() -> listOfServiceNames```

This method returns the list of authorized service names obtained by all requests to ```authorizeProtocols()```.

If the NAIL did not receive any service authorizations, it will return an empty list.

```serialize() -> String```

This method serializes the internal data structures. The NAIL API does not ensure persistency. Therefore, the business logic MUST ensure that any persistent data is stored approrpiately. The ```serialize()``` method returns a UTF8 character string that can get used for initialising the NAIL API.

``` deserialize(nailSerialization) ```

This method parses the serialised data. This function is typically used during initialization.

```getDisplayName(serviceName) -> string```

This method returns the display name of an authorized Federation Service.

```getEndpointUrl(serviceName,protocolName) -> uristring```
```getEndpointUrl(serviceName,protocolName,endpointPath) -> uristring```

```getServiceToken(serviceName, protocolName) -> string```
```getServiceToken(serviceName, protocolName, claims) -> token```

The claims parameter holds a dictionary object with additional claims to be added to a JWT token. Additional claims can be used to increase the security of the token. Protocols MAY require clients to set specific claims. If the service uses basic bearer tokens, this parameter will be ignored.

If the requested service return an empty string, the token has been expired. In this case the app's business logig MUST refresh the token for this service using the ```refreshToken()``` method.

```refreshToken(serviceName) -> completionPromise```

This method refreshes a service's access token.

The completion handler receives a boolean value that indicates if the NAIL API was able to refresh the token. On failure the app's business logic MAY remove the service from the NAIL API.

``` removeService(serviceName) ```

This method removes an authorized service from the NAIL API.

## NAIL Interface API

### Protocol Authorization

The NAIL protocol requires an app to present a set of information, including but not limited to its bundle id and name.

This parameter will always be a list of authorized protocol endpoints (array). An authorized protocol endpoint contains a parsed access token and the Federation Service RSD. The business logic MUST NOT assume a complete RSD document but only to find the requested  protocol endpoints.

### Protocol handling.

### Access Restrictions

Scoped requests

- Apps may not access endpoints outside their scope

Simple Bearer Tokens Handling

Refreshing Access Tokens


## Javascript Integration for Apache Cordova

