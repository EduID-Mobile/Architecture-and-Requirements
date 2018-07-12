# Core App Architecture

The edu-ID Mobile App provides an authorization and authentication interface for securely connecting third party apps with services in the trust domain of Swiss academic services. The edu-ID Mobile App ensures that commercial and non-commercial third party apps can provide added value services based on the existing service infrastructure in Swiss higher education institutions. The edu-ID App's key function is to authorise third party apps on a user's device with academic services within the edu-ID federation. It helps to bridge the user/app store trust domain on the user devices and the trust domain within the Swiss Academic Service Federation.

The edu-ID Mobile App integrates interactions between the edu-ID infrastructure, services in the academic edu-ID federation, and third-party apps installed on the users' devices. It limits service and data exposure to third-parties to authorised sources.

This document describes the architecture of the edu-ID Mobile App.

## Introduction

The edu-ID Mobile App is the center piece for authorizing native apps on mobile devices, including smart phones and tablets. The edu-ID Mobile app is a stand alone component that is installed and maintained independently from other apps on the federation users' mobile devices.

This architecture document describes the components and their relations for facilitating the [edu-ID Mobile App user experience](20-user-experience.md).

This document heavily relies on the [service architecture](30-app-architecture.md) and the [user experience](02-user-experience.md) parts of the edu-ID Mobile App architecture.

The app can be launched through two processes.

* stand-alone launch through the device's app launcher.
* protocol-authorization requests from third party apps.

## Components

The edu-ID Mobile APP has 6 functional components.

* Profile manager
* Protocol discovery
* Service authorization
* App authorization manager
* App access provider

### Profile Manager

The profile manager is responsible handling device assertion, user authentication and profile caching. The profile manager also provides authorization data for authorized requests to the edu-ID Service.

All components that perform authorized requests to the edu-ID Service rely on data handled by the profile manager.

The profile manager also provides the data for the institution filter in the app authorization user interface.

#### Device Assertion Interface

The device assertion interface is used for registering an edu-ID Mobile App instance at the edu-ID service. (1)

This interface is also used for upgrading the edu-ID Mobile App Version if updates are applied on a device.

#### User Authentication Interface

The user authentication interface exchanges data during user authentication (2).

The obtained access token is used for authorizing requests to the edu-ID Service.

The obtained access token MAY contain user profile information following the OpenID Connect identity claims.

#### User Profile Filter Interface

The user provile filter interface can access the edu-ID Service's User Profile Attribute Filter (2a). These attributes can include further information about a users affiliations to institutions within the academic federation.

The use of the edu-ID Service's User Profile Attribute Filter is specified by the edu-ID project and the protocol will be implement on availability.

This interface is used only used for informative purposes.

### Protocol Discovery

The protocol discovery component performs protocol lookups in the federation's protocol discovery (3b).

This component is typically triggered by the app access provider (7a).

The protocol discovery MAY implement result caching for reducing data transmissions.

The protocol discovery filters RSD2 objects according to the requested protocols. Other components of the edu-ID Mobile App typically have only access to information about OAuth2 protocol endpoints and requested protocols.

#### RSD2 Discovery Interface

The RSD2 discovery interface allows the protocol discovery to query the federation's protocol discovery.

### Service Manager

The service manager keeps track of the Federation Services the edu-ID Mobile it is authenticated for. Additionally, it keeps an internal record of granted app assertions on the device.

This data helps to personalise the user experience in the app authorization user interface.

The service manager privides the filter criteria for the personal filter for the app authorization user interface.

If a [Service Usage Component](20-service-architecture.md) is available in the federation, then this information will be included into the service management (3).

#### Token Revocation Interface

The token revocation interface handles the [token revocation requests](https://tools.ietf.org/html/rfc7009). By default, the service manager can revoke only tokens for the edu-ID Mobile App instance or tokens that were obtained through it.

The token revocation interface MUST be triggered only on user request.

#### Service Usage Discovery Interface

The service usage discovery sends an authorized GET request to the edu-ID Service's Service Usage Manager in order to obtain a personalised list of Federation Services for a authenticated user.

Only if a [Service Usage Component](20-service-architecture.md) is available in the federation, then the service usage discovery interface is active. This interface is activated at build time of the edu-ID Mobile App.

### Service Authorization

The service authorization component handles the process of obtaining an assertion token from the edu-ID Service and pass it as an authorization code to a Federation Service.

The service authorization extends the service manager to handle the data exchange between two service endpoints.

The service authorization is only triggered if a user likes to authorize app access to a Federation Service that has not been accessed from the device, yet.

If the edu-ID Mobile App fails to authorize with the requested Federation Service.

#### Service Assertion Interface

This interface provides a connector to the authorization endpoint for obtaining an authorization code for a Federation service. The authorization code holds and encrypted authorization assertion for the Federation Service.

The edu-ID Mobile App MUST pass this assertion directly to the service code authorization interface.

#### Service Code Authorization Interface

This interface sends the authorization code obtained via the service assertion interface to the token endpoint of a Federation Service.

The edu-ID Mobile App MUST use the code format as provided by the edu-ID service. This could be either of the following grant types.

* ```urn:ietf:param:oauth:grant-type:jwt-bearer```
* ```urn:ietf:param:oauth:grant-type:saml2-bearer```

As the edu-ID Mobile App does not touch the bearer token, the format is defined by agreement between the edu-ID service and the Federation Service.

Federation Services MAY reject an assertion. The edu-ID Mobile App instance MUST remove the rejecting Federation Service from the list of authorized services.

### App Access Provider

The app access provider manages the device-level communication between the edu-ID Mobile App and third party apps. The app access provider ensures that only [valid protocol authorization requests](40-nail-api.md) are handled by the edu-ID app.

The app access provider integrates the authorization tokens provided by the app authorization manager with the RSD2 object provided by the app's protocol discovery.

On iOS the app access provider is part of the edu-ID Authorization App Extension.

#### OS Data Exchange Interface

This interface filters the incoming protocol requests and ensures the related responses.

### App Authorization Manager

The app authorization manager handles app assertions for third party apps for authorized Fedeation Services.

The app authorization manager is triggered during app authorization after a user has selected the Federation Services for authorization.

On iOS the app authorization manager is part of an edu-ID Authorization Extension.

#### App Authorization Interface

The app authorization interface connects to a Federation Service's token endpoint for the OAuth2 protocol.

The app authorization interface MUST expect and respond using the following protocol name.

``` urn:ietf:params:oauth:assertion ```

The app authorization interface MUST add the requested protocols as scope for the Federation Service.

The result is an access token and an optional refresh token.

## User Interfaces

The first version of the edu-ID Mobile App has five user interfaces.

* Login user interface
* Logout user interface
* Profile information user interface
* App authorization user interface
* Service authorization user interface

### Login User Interface

The login user interface is displayed whenever the app is launched and no active authorization for an edu-ID user is present. This can be during stand-alone launches or during protocol authorization requests

The login user interface consists of the same user interface elements for entering user credentials as the web-based edu-ID Service interfaces.

### Logout User Interface

The logout user interface provides an intermediate step before users can revoke their edu-ID authorization for the device.

The logout user interface informs the users about the consequences and displays affected services and apps.

Users can decide via the logout user interface if they want to continue logging out or if they want to keep the device authorization.

### Profile Information User Interface

The profile information user interface allows users to see the edu-ID identity that is connected to the present device. If affiliation information is available in the user profile, then this information is displayed, too.

### App Authorization User Interface

The app authorization user interface is launched before app authorization during protocol authorization requests of third party apps. The service authorization interface allows users to select services they want to grant a requesting app access.

Users can select all or some services for authorization. Alternatively, users can choose to completely reject an authorization request.

The information displayed in the app authorization interface is the result of the protocol discovery for a third party app's protocol authorization request. This result includes all Federation Services that support the requested protocols.

Users can narrow down the provided information via two filters.

* A personal filter, and
* An institution filter

The personal filter relies on the service manager to filter only services that were accessed previously.

The institution filter relies on profile information and includes only services, for which a user has active affiliations. If no affiliation information is present in a user profile, then the institution filter is not available.

In addition users can use a __search__ to narrow down the service list. The search targets the service's display names and host address.

### Service Authorization User Interface

The service authorization user interface allows users to introspect  granted authorizations. Through this interface users can see which apps have active authorizations and which services they use.

Users can actively revoke service and app authorizations through the service authorization user interface.

The service authorization user interface does not allow to request service authorizations.

## Security Considerations

Whenever possible the edu-ID Mobile App MUST be configured for data encryption and memory randomization. This ensures device-level data protection.

The edu-ID Mobile App uses only token authorization for accessing the edu-ID Service as well as Federation Services. User credentials MUST NOT get stored on the device. On Android and iOS this is defined at built time.

Additional authentication (e.g. biometric, beacon, network level, geo-location) using device sensors MAY be requested at the level of the edu-ID authentication, service authorizations, and app authorization if enhanced security requirements become necessary. Such extensions will be tied to the respective components and are not considered independently at the level of the app architecture.


| [Previous: Service Implementation Checklist](22-jwt-implementer-checklist.md) | [Return to Architecture Overview](00-overview.md) | [Next: iOS Architecture](32-ios-app-architecture.md) |
| :---- | :----: | ----: |
