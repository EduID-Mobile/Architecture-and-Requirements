# Swiss Edu-ID Mobile App Integration Framework

## Abstract

The edu-ID Mobile App provides an authorization and authentication interface for securely connecting third party apps with services in the trust domain of Swiss academic services. The edu-ID Mobile App ensures that commercial and non-commercial third party apps can provide added value services based on the existing service infrastructure in Swiss higher education institutions. The edu-ID App's key function is to authorise third party apps on a user's device with academic services within the edu-ID federation. It helps to bridge the user/app store trust domain on the user devices and the trust domain within the Swiss Academic Service Federation.

The edu-ID Mobile App integrates interactions between the edu-ID infrastructure, services in the academic edu-ID federation, and third-party apps installed on the users' devices. It limits service and data exposure to third-parties to authorised sources.

This document describes the overarching system architecture and service interaction protocols.

## Introduction

## The Problem of Integrating Mobile Apps into Service Federations


## The Solution as Defined by the edu-ID Mobile App Architecture


## Academic Service - edu-ID App Architecture Overview

The edu-ID mobile app is not a stand alone mobile application but mediates processes and data of services within the academic domain and third-party apps. Therefore, the edu-ID Mobile App is part of an IT ecosystem that spawn across two trust domains:

Firstly, the edu-ID links to the Swiss academic trust domain, which is managed via a central edu-ID service as federation manager that connects distributed services at academic institutions participating in the federation. These distributed services include for example data storage, web-conferencing, online collaboration, or learning management systems. In the context of this document this trust domain is called the Academic Trust Domain.

Secondly, it is part of the user-and-app-store trust domain on the users' devices. This trust domain is courated by the end-users by installing apps on their devices and controlled by the app store host. The app store hosts manage access to application developers by setting and enforcing quality and security guidelines. Within the context of this document this trust domain is called User Device.

Within these trust domains there is exaclty one edu-ID service in the Academic Trust Domain and exactly one edu-ID mobile app on the User Device. The edu-ID service and the edu-ID Mobile App are tightly connected. This connection is controlled by the host of the edu-ID service, which creates a separate edu-ID trust domain that bridges the Academic Trust Domain to the User Device.

The Academic Trust Domain connects many distributed "academic services". These services are typically hosted by the institutions that are participating in the federation.

On the User Devices there can be many apps that are related to the users' productivity. These apps are installed and arranged by the owners of the User Devices.

![Illustration of the edu-ID Mobile App ecosystem and trust domains](images/eduid_app_architecture.png)

Some of the services in the Academic Trust Doamin services expose service APIs for loosely-coupled machine-to-machine communication. Whereas tightly-coupled machine-to-machine communication requires explicit connections of two services typically initiated system wide by the system administrators, losely-coupled machine-to-machine communication allows unmanaged ad-hoc communications between services, tools and components. Tightly-coupled machine-to-machine communication can only be performed if all commuicators are known at the time of service coupling and controlable. Such connections create a pre-defined trust domain. In highly dynamic environments that spawn trust domains and involve many stakeholders, tight-coupling is resource intensive and does not easily scale. Loosely-coupled machine-to-machine communication overcomes the overhead of tight coupling by distributing coupling responsibility across the different stakeholders in the ecosystem.

Conventional [SAML]() Authentication and Authorization provides reliable identification and single-signon solutions within web-based trust domains. SAML-based approaches have been used to connect native mobile apps and distributed services. These approaches vary on the different platforms and require that native mobile apps are part of the same trust domain as the accessed services. SAML-based approaches are known to be prone to identity theft and viotation of intellectual property rights (IPR) if trust domains are naively assumed for native applications [REFERENCE MISSING](). This limits the applicability of SAML for loosely-coupled machine-to-machine communications, for which ad-hoc trust domains are established.

The edu-ID Mobile App ecosystem primarily focuses on client authorization and does not include an identity layer. Such layer might be provided by dedicated identity services based on LDIF, OpenID or Web-finger. Security aspects related to the identity profile layer are not addressed by the edu-ID Mobile App architecture.

### Enhanced OAuth 2.0 Framework

The edu-ID Mobile App architecture builds on top of the [OAuth 2.0 Framework](https://tools.ietf.org/html/rfc6749). The OAuth 2.0 Framework defines a process flow for authorized access to resource services from a wide range of clients. This framework is extended by [OAuth 2.0 Assertions](https://tools.ietf.org/html/rfc7521). OAuth 2.0 Assertions provide means to relay authorization across protocols and services.

The edu-ID Mobile App architecture __enhances__ the OAuth 2.0 Framework by cascading the specified OAuth 2.0 workflow and assertions. By doing so, the architecture does not add  new concepts to the OAuth 2.0 Framework but aligns the specifications' concepts into a coherent business logic for machine-to-machine trust negociation. This logic is not part of the OAuth 2.0 specifications and is therefore left to actual OAuth 2.0 service implementations.

Recent implementation of OAuth2 build on JSON Web-Tokens (JWT). JWT provides a more flexible and more secure authentication and authorization approach than the original (plain) Bearer scheme for OAuth2 [https://tools.ietf.org/html/rfc6750]. Different to OAuth1 MAC authorization, JWT is fully compatible to the Bearer scheme.

#### Implementation Issues

The OAuth2 specification defines a framework for authentication and authorization. It does not specify identity provisioning. Identity provisioning is fully encapsulated in the authorization code of a JWT. The organisation of provided identity information is partially covered by the OpenID Connect specification [OPENID CONNECT 1.0]. Actual implementations of OAuth2/OpenID Connect Identity Providers (IDP) may allow additional/federation-specific identity information.

The EduID Mobile App is agnostic to the IDP implementation as it uses and passes JWT tokens without manipulating identity related information.

#### Dymanic JWT

Different to some commercial implementations that uses JWT as static codes, the EduID Mobile App uses __dynamic__ JWT that are regenerated for each request, based on a shared secret between the authorizing service and the authorized client. Static JWT can be verified by the authorizing service as self orginated. Clients can verify the authorizing instance of a static JWT based on a public or shared keys. The dynmaic JWT replaces the OAuth1 MAC Authorization by having request-specific JWT. This means that clients MUST regenerate a JWT for every request to the authorizing service or to the resource service. This JWT is secured by a shared key between the client and the authorizing service/resource service.

Compared to static JWT the authorizing service can verify not only the origin of the authorization, but also the client. While with static JWT potential attackers may hijack the JWT for making requests, with dynamic JWT they need to possess a shared secret in order to generate auhorizing JWT. Depending on the used cryprographic mechanism, the shared secret might never cross the network. With dynmaic JWTs it is possible to reduce potential security risks of static Bearer tokens and allows longer time-to-live (TTL) intervals.

Implementing IDPs need to support dynamic JWTs in order to work with the EduID Mobile App.

### edu-ID Mobile App Ecosystem Stakeholders

Human stakeholders play a key role in the process of machine-to-machine communication across trust domains. Therefore, the experience of integrating and using the infrastructure must not add complexity to the app development, system maintenance, or device usage. Four key stakeholders are identified for the architecture.

* Federation Management

* Service Provider

* Mobile Device Owner

* Application Developer

### Minimal Security Requirements

* [ ] User names and passwords are NEVER exposed to third party apps.

* [x] Third party apps NEVER interact directly with the authorization services of the edu-ID infrastructure.

* [ ] Third party apps NEVER receive service endpoints locations without explicit authorization of the users and services.

* [x] Third party apps gain access ONLY to explicitly requested protocol endpoints or combinations of protocol endpoints.

### Advanced Security Requirements

* [ ] User credentials are never transmitted in clear text over the Internet.

* [ ] Users can revoke all access to academic services from a device by logging out of the edu-ID app.

* [ ] Users can remotely revoke device access for all or individual devices.

* [ ] Users can remotely revoke access to individual services.

* [ ] Service administrators can revoke mobile access for individual users and apps.

* [ ] Service administrators can impose access policies for users and apps.

* [ ] edu-ID administrators can revoke access for users or their identities.

* [ ] edu-ID administrators can invalidate access for specific edu-ID mobile app versions.

* [ ] Federation services need to authenticate when verifying the identity of edu-ID Mobile App users.

* [ ] Federation services may revoke access to certain versions of the edu-ID Mobile App on certain or all supported plattforms.

## Infrastructure Service Interfaces and Core Process

The four components of the edu-ID Mobile App ecosystem rely on the interplay of several interdependent components. Each of these components provides service endpoints of the enhanced OAuth 2.0 Framework. The components are related to three main processes.

![edu-ID Mobile App Component Overview](images/eduid_app_architecture-details.png)

### 1. User Authorization

The user authorization process refers to the sequence of steps to tie an user identity to an edu-ID Mobile App instance on a mobile device. This process consists of connecting an edu-ID Mobile App instannce to the edu-ID Federation Service via the device assertion protocol (1), the OAuth user authentication (2) and exchange of user attributes (2a), and the service usage (3) and protocol discovery (3a and 3b).

Including service usage resources and protocol discovery into this process is for user convenience in large federations, in which users access only a minor subset of available services. This enables the edu-ID Mobile App to filter the services to which a user has connected one or more identities instead of giving access to all possible services. The protocol discovery (3b) supports the edu-ID Mobile App to identify suitable services that match protocol requests of third party apps.

![edu-ID App Authorization Process](images/eduid_app_architecture_authorization.png)

In the above figure the blue box refers to the steps as specified for OAuth 2.0. The green boxes refer to OAuth Authorization service endpoints and the orange boxes refer to resource service endpoints.

### 2. Service Assertion

The service assertion process refers to the steps that authorise an edu-ID Mobile App instance to an academic service. Only if the edu-ID Mobile App has granted access to an academic service, it will relay access authorization to third-party apps for that service. This process consists of requesting an authorization grant from the edu-ID Federation Service (4), which results in a unique service grant token for the given service.

The edu-ID Mobile App requests authorization to the selected service by forwarding the service grant token to it (5). The service grant token authenticates a user to exactly one service.

The service grant token can be automatically validated by the academic services. OPTIONALLY, the selected academic service can validate a client token included with the service grant token with the edu-ID Federation Service (6) and/or request profile information from it (6*).

The academic services MUST only grant or reject access and issue a service access token on the grounds of the results of validating the service grant token with the edu-ID Federation Service for completing the authorization of the edu-ID Mobile App. At any point the edu-ID Federation Service or the edu-ID Mobile App MAY invalidate some or all tokens for an edu-ID Mobile App instance with an academic service. In both cases, the academic service MUST invalidate the requested token and all tokens that are issued for that token in the app assertion process (10).

![edu-ID Service Assertion Process](images/eduid_app_service_assertion.png)

In the above figure the blue box refers to the steps as specified for OAuth 2.0. The green boxes refer to OAuth Authorization service endpoints and the orange boxes refer to resource service endpoints.

### 3. App Assertion

The app assertion process refers to granting access to one or more protocol endpoints of an academic service to a third-party app. This process is initiated by the third-party app by requesting access to one or more protocols using the mobile device's operating system mechanics for inter-app communication as a result of a user interaction (7). This request is forwarded by the mobile operating system to the edu-ID Mobile App, which selects possible services on the grounds of the requested protocols.

The edu-ID Mobile App informs the mobile device owner that an app requests access to the suitable services and provide the mobile device owner the opportunity to select none, one, some, or all suitable services to be accessed by the requesting app. For each selected service the edu-ID Mobile App asserts the third-party app's access using the edu-ID Mobile App's service access token (8).

At this point academic services MAY grant or reject access for the requesting app. Academic services SHOULD only grant an app access token for the requested protocol endpoints. For each academic service that issues an app access token, the edu-ID Mobile App will send the protocol endpoints as well as the related app access token to the third-party app. The third-party app MUST authorize all requests to the protocol endpoints of an academic service using the issued app access token (9).

If the third-party app receives an invalid token error from an service endpoint, it SHOULD re-initiate the app assertation process (7). Only if the academic service has issued a refresh token with the app access token, the third-party app MAY request a new app access token from the academic service.

![edu-ID App Assertion Process](images/eduid_app_assertion.png)

## Infrastructure Components and Service Endpoints

### edu-ID Architecture Extensions

The edu-ID architecture does not specify the integration of the system beyond the level of access authorization. This architecture is extended in

### Academic Protocol Endpoints and components

#### Service Exposure

The service exposure is an [RSD2](https://github.com/BLC-HTWChur/rsd2-specification/blob/master/rsd2-specification.md) interface that exposes web-service protocols offered by an academic service. In order to be recognised by this architecture it requires at least 2 endpoints:

* The /service.txt that provides the pointes to the actual rsd2 endpoints.

* One of the defined RSD2 endpoints for XML, JSON, or YAML-formatted RSD2 information.

#### edu-ID Authorization

The edu-ID Authorization component's key function is to handle OAuth2 authorization assertions. Additionally, the component has an token revocation endpoint that can be used by the edu-ID Federation Service or an edu-ID Mobile App instance for invalidating edu-ID client and app tokens for a user. Finally, the edu-ID Authorization includes token validation that refer back to the edu-ID Federation service.

The edu-ID Authorization component token validation accepts grant assertions issued by the edu-ID Federation Service. These assertions are issued by the service-assertion endpoint of the edu-ID Federation Service to authorized edu-ID Mobile App instances. The assertions are issued specifically for the academic service and the service can validate the assertion based on a shared secret between the edu-ID Federation Service and the academic service. For known subjects the assertion handling MAY accept the assertion immediately. For unknown subjects it is RECOMMENDED to validate the client token and the user token with the edu-ID Fefertion Service. For client tokens it is RECOMMENDED that the academic service does not enforce automatic timing out of the client. If the academic service automatically invalidates client tokens, then it must inform the edu-ID Federation Service that the related client grant token is no longer valid. In this case the edu-ID MObile App MUST request a new assertion for the service.

The token revocation accepts an app token, a client token, a user token or a client grant token. Depending on the token presented to the revocation, the revocation will invalidate all related tokens. An authorized edu-ID Mobile App can only revoke the current access token or app tokens. Revoking app tokens allows edu-ID Mobile App instances to remove access for selected mobile apps  to the academic service. This is typically done on user request. An edu-ID Mobile App instance may revoke its own client token. This happens if the user logs out or the edu-ID Mobile Apps user token to the edu-ID Federation Service has been otherwise invalidated by the edu-ID Mobile App. The edu-ID Federation service can revoke access for a specific edu-ID Mobile App instance by revoking its client grant token. This is typically done if the user wishes to disconnect a device remotely or if the edu-ID Federation Service restricts access to specific versions of the App. Finally, the edu-ID Federation Service can revoke access to all edu-ID Mobile App instances and the related apps via the service's user token.

The token validation component can validate a client grant token with the edu-ID Federation Service. It can also validate the user identity of the user token and get access to more details of the user profile.

#### App Assertion

The app assertion allows authorized edu-ID Mobile Apps to request app access tokens for third party apps. The academic service SHOULD enforce scoping of the third party app's access to the requested services. If an authorized edu-ID Mobile App instance requests tokens for third-party apps, then the app assertion of the Academic Service MUST link the client tokens with its requested app tokens.

#### Access Authorization

All authorized apps should have access to all services within the scope of their access token. A third party apps need to authorize their access by adding an MAC Authorization header to all service reuqests.

### Third-party mobile app components

Third party app can request access to selected protocols. These apps need to be able to handle multiple protocol endpoints per protocol.

#### Access Management

The access management provides the interfaces to the app's business logic for requesting access to the requiered protocols. This component issued the inter-app communication on the device.

#### Authorization Management

The authorization management chooses the

### edu-ID Mobile App components

#### Profile Manager

#### Service Manager

#### Service Authorization

#### App Assertion Manager

#### App Access Provider


## Process and Protocol Overview

### User authentication and Service authorization

### Third Party app assertion

### Protocol Discovery


## References

* [RFC-6749: The OAuth 2.0 Autorization Framework](https://tools.ietf.org/html/rfc6749)
* [RFC-7521: Assertion Framework for OAuth 2.0 Client Authentication and Authorization Grants](https://tools.ietf.org/html/rfc7521)
* [RFC-2617: The Basic HTTP Authorization Scheme](https://tools.ietf.org/html/rfc7617)
* [RFC-7523: JSON Web-Tokens (JWT)](https://tools.ietf.org/html/rfc7523)
* [OAuth 2.0 Message Authentication Code (MAC) Tokens (IETF RFC Draft)](https://tools.ietf.org/html/draft-ietf-oauth-v2-http-mac-05)
* [Swiss edu-ID Architecture](https://projects.switch.ch/export/sites/projects/edu-ID/.galleries/documents/SwissEduIDArchitecture_Rev1.pdf)
* [RSD2 service discovery](https://github.com/BLC-HTWChur/rsd2-specification/blob/master/rsd2-specification.md)

