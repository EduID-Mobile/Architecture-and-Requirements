# Service Architecture

The edu-ID Mobile App provides an authorization and authentication interface for securely connecting third party apps with services in the trust domain of Swiss academic services. The edu-ID Mobile App ensures that commercial and non-commercial third party apps can provide added value services based on the existing service infrastructure in Swiss higher education institutions. The edu-ID App's key function is to authorise third party apps on a user's device with academic services within the edu-ID federation. It helps to bridge the user/app store trust domain on the user devices and the trust domain within the Swiss Academic Service Federation.

The edu-ID Mobile App integrates interactions between the edu-ID infrastructure, services in the academic edu-ID federation, and third-party apps installed on the users' devices. It limits service and data exposure to third-parties to authorised sources.

This document describes the overarching system architecture and service interaction protocols.

## Introduction

Authorization is a key element for offering personalised experiences. In higher education, authorization is a key for accreditation processes. With the increase of digital information processes in and around education of universities and universities of applied sciences there is also an increased need to connect mobile application and application-based service into administrative and educational processes.

The edu-ID Mobile App provides authorization services to third party apps on the mobile devices of federation users.

The edu-ID mobile app is not a stand alone mobile application but mediates processes and data of services within the academic domain and third-party apps. Therefore, the edu-ID Mobile App is part of an IT ecosystem that spawn across two trust domains:

Firstly, the edu-ID links to the Swiss academic trust domain, which is managed via a central edu-ID service as federation manager that connects distributed services at academic institutions participating in the federation. These distributed services include for example data storage, web-conferencing, online collaboration, or learning management systems. In the context of this document this trust domain is called the Academic Trust Domain.

Secondly, it is part of the user-and-app-store trust domain on the users' devices. This trust domain is curated by the end-users by installing apps on their devices and controlled by the app store host. The app store hosts manage access to application developers by setting and enforcing quality and security guidelines. Within the context of this document this trust domain is called User Device.

Within these trust domains there is exactly one edu-ID service in the Academic Trust Domain and exactly one edu-ID mobile app on the User Device. The edu-ID service and the edu-ID Mobile App are tightly connected. This connection is controlled by the host of the edu-ID service, which creates a separate edu-ID trust domain that bridges the Academic Trust Domain to the User Device.

The Academic Trust Domain connects many distributed "academic services". These services are typically hosted by the institutions that are participating in the federation.

On the User Devices there can be many apps that are related to the users' productivity. These apps are installed and arranged by the owners of the User Devices.

![Illustration of the edu-ID Mobile App ecosystem and trust domains](images/eduid_app_architecture.png)

Some of the services in the Academic Trust Domain services expose service APIs for loosely-coupled machine-to-machine communication. Whereas tightly-coupled machine-to-machine communication requires explicit connections of two services typically initiated system wide by the system administrators, loosely-coupled machine-to-machine communication allows unmanaged ad-hoc communications between services, tools and components. Tightly-coupled machine-to-machine communication can only be performed if all communicators are known at the time of service coupling and controllable. Such connections create a pre-defined trust domain. In highly dynamic environments that spawn trust domains and involve many stakeholders, tight-coupling is resource intensive and does not easily scale. Loosely-coupled machine-to-machine communication overcomes the overhead of tight coupling by distributing coupling responsibility across the different stakeholders in the ecosystem.

Conventional [SAML]() Authentication and Authorization provides reliable identification and single-sign-on solutions within web-based trust domains. SAML-based approaches have been used to connect native mobile apps and distributed services. These approaches vary on the different platforms and require that native mobile apps are part of the same trust domain as the accessed services. SAML-based approaches are known to be prone to identity theft and violation of intellectual property rights (IPR) if trust domains are naively assumed for native applications [REFERENCE MISSING](). This limits the applicability of SAML for loosely-coupled machine-to-machine communications, for which ad-hoc trust domains are established.

The edu-ID Mobile App ecosystem primarily focuses on client authorization and does not include an identity layer. Such layer might be provided by dedicated identity services based on LDIF, OpenID or Web-finger. Security aspects related to the identity profile layer are not addressed by the edu-ID Mobile App architecture.

### Relation to Federation Management

The edu-ID Mobile App is part of the academic trusted federation. The mobile app acts as a frontend for authorizing users on a device.

The mobile app is part of the federation as it is a confidential client to the edu-ID Service. Under normal circumstances only the edu-ID Mobile App connects to the authentication endpoints of the edu-ID service.

Different versions of the app are managed centrally and are separately registered in a way that the edu-ID Service is capable to identify the different app versions when an instance connects to it. Each edu-ID Mobile App instance registers to the federation. This allows to keep track of the individual app instances and versions within the federation.

At the federation level it is possible to exclude certain versions of the edu-ID Mobile App from accessing authorizing users or accessing services.

### Relation to User Management

Registered edu-ID App instances pass user credentials to the edu-ID Service for authenticating users. One edu-ID App instance can authenticate one user. On systems that support multiple user identities, each identity MAY be linked to a separate edu-ID member.

The edu-ID is connected to user management for authentication only.

* Users MAY identify edu-ID App instances that are linked to their accounts.

* Users MAY disconnect edu-ID App instances from any other edu-ID client (either an edu-ID App or via the edu-ID Web-interface).

* The edu-ID Service MAY remove edu-ID App instances for individual users.

* Federation Services MAY exclude access for individual edu-ID App instances.

* Federation Services MAY allow access for users from selected edu-ID App instances.

The edu-ID App has no user management capabilities on its own.

### Relation to User Profiles and Identity Management

After authentication the edu-ID App is only loosely coupled to user management. For an authorized user the app can access profile information for that user by using the edu-ID Profile Filter. This information is used for displaying purposes, only.

## Service Interaction Overview

The edu-ID App depends on the existence of service components. This architecture include primary service components that provide the key functions for authentication and authorization, and secondary service components that for improving the user experience in the diverse and distributed academic federation.

![edu-ID Detailed Architecture](images/eduid_app_architecture-details.png)

### Federation-level Service Endpoints

The edu-ID Mobile relies on the OAuth2 protocol endpoint for authenticating users and clients.

* The token endpoint.
* The authorization endpoint.
* The revoke endpoint as specified in [RFC 7009](https://tools.ietf.org/html/rfc7009)

The token and the authorization endpoints are specified in [OAuth2 section 3](https://tools.ietf.org/html/rfc6749#section-3).

### Institution-level Service Endpoints

Federation Services need to provide at least an token and a revoke service endpoint as a target the edu-ID Mobile App.

The token endpoint MUST support at least the following grant-types:

* urn:ietf:param:oauth:grant-type:jwt-bearer
* authorization_code

In addition the institution-level service endpoints MAY support scoped request. For services supporting scoped requests the scoping MUST ensure that:

* The edu-ID Mobile App instances' token MUST be scoped to access the token endpoint.
* Third party app instance tokens MUST get scoped to the authorized protocols.
* Third party app instances MAY have several active authorization tokens for different protocol.

The revoke endpoint MUST support [OAuth2 token revocation](https://tools.ietf.org/html/rfc7009). Token revocation MAY be performed by the edu-ID Service, the edu-ID Mobile App, or the authorized third party app.

For Federation Services that use token extension MUST scope the third party app for accessing the token endpoint.

## Process Flows

The edu-ID Mobile App relies on the [Client Crendentials Grant flow](https://tools.ietf.org/html/rfc6749#section-4.4) and the [Authorization Code Grant flow](https://tools.ietf.org/html/rfc6749#section-4.1). The client credentials grant flow is only used for client registration. All flows rely on the [Proof-of-Possession Key Semantics for JSON Web Tokens (JWTs) specification](https://tools.ietf.org/html/rfc7800).

Federation services MAY additionally implement the [Extension Grant flow](https://tools.ietf.org/html/rfc6749#section-4.5). The extension grant flow is only relevant to the [native application integration layer](40-nail-api.md).

The process flows are related to endpoint interfaces. The detailed architecture

### edu-ID Mobile App Client Assertion

The edu-ID Mobile App comes in variations for Android and iOS. Each variation has its independent versioning. Each version of the edu-ID Mobile App requires an unique version key.

Before an edu-ID Mobile App instance (client) can access the edu-ID Service endpoints, it MUST "register" itself using the client credentials Grant flow (1). Using a [self-assigned client assertion](https://tools.ietf.org/html/rfc7521#section-4.2) as credentials. The client assertion uses the version key for verifying its origin.

Each client has its independent authorization. The client assertion is necessary, so users can revoke access for specific clients in case of

* technical breakdown,
* loss,
* theft, or
* handover.

A client assertion MUST include the instance's device information including.

* unique device ID
* device name
* device type
* OS Version.

The edu-ID Service MUST provide an unique instance token to the edu-ID Mobile App instance.

The edu-ID Service MAY reject individual version keys from authorizing with the service.

The edu-ID Service MUST allow already confirmed edu-ID Mobile App instances to upgrade to a higher version if the instance can present the appropriate key.

The edu-ID Service MUST allow already confirmed edu-ID Mobile App instances to alter the device information if the correct instance token has been presented.

The edu-ID Service MAY revoke instance tokens in order to exclude specific clients from accessing the infrastructure. The edu-ID Service MUST refuse rejected clients from re-obtaining a client token.

### User Authentication

The edu-ID Mobile App's primary purpose is to authenticate mobile users with the edu-ID Service. The user authentication connects directly to the edu-ID user authentication component by sending the app user's credentials to its service-endpoint (2).

The edu-ID User Authentication MUST reject authentication from unregistered clients in Authorization Code Grant mode.

As a result the client receives a __personal access token__ so it can access edu-ID components that provide access to personal information. This access token MUST be persistent so the client does not need to store the user credentials on the device. If the edu-ID Authentication components uses unsecured Bearer tokens the access token MUST be renewable.

The personal access token is used to access the following edu-ID endpoint.

* User Authentication for logout
* User Profile
* Service Assertion
* Service Usage Manager (if present)
* Protocol discovery (if [integrated into the edu-ID Service](50-operational-model.md))

The personal access token SHOULD be a [JWT](https://tools.ietf.org/html/rfc7523) that contains a [JWK](https://tools.ietf.org/html/rfc7517) to be used for all further authorization with edu-ID Services.

Immediately after authentication the client will access the edu-ID User Profile Attribute Filter using the personal access token for retrieving basic user information for a user (2a). This information is only used for presentation purposes in [stand alone mode](30-app-architecture.md).

### Service Usage Manager

The service usage manager provides a list of services that have been used previously by a user via edu-ID authentication. This information is already collected by the edu-ID infrastructure (e.g. for facilitating logout), but within the architecture there is no identifiable service end-point or component that allows users to access this information.

The service usage manager is a personalised web-service that MUST provide only information for an authenticated user.

The service usage manager provides only an interface for obtaining a list of service URLs. This interface is optional and relies on cross-matching the information in the edu-ID User Directory, edu-ID Federation Index and (possibly) the edu-ID Affiliation Index.

For example, a user that uses only the Moodle instances at HTW Chur and USI will receive the following list.

```json
[
"https://moodle.htwchur.ch",
"https://www2.icorsi.ch/"
]
```
(Line-breaks are for illustration purposes)

The Service Usage Manager provides a convenience function for preselecting and prioritising services during the authorizing third party app access. If the service is present then the edu-ID Mobile App will use this service for improving the user experience.

The edu-ID Mobile App requests usage information from the Service Usage Manager in two occasions (3):

1. after successful login and
2. whenever the app is launched.

IF no Service Usage Manager is available in the edu-ID Infrastructure, then the users MUST select their services from the full list of matching Federation Services.

The edu-ID Mobile App MAY cache successful service authorizations to mimic the function of the Service Usage Manager.

### Protocol Discovery

The protocol discovery is a process by whose the Federation Service can provide the protocol endpoints requested by an app. The protocol discovery has two components.

* A service level component for exposing the available protocols and
* A Federation-wide protocol discovery for querying suitable federation services.

The federation-wide protocol discovery provides an query interface to authorized clients to determine, which services provide a requested set of protocols.

The protocol discovery uses the RSD2 discovery mechanism to finding, which federation services expose protocols to external clients.

Federation Services that do not expose any protocols for such purpose MAY NOT expose RSD2 information. In this case the protocol discovery will automatically exclude these services from the discovery. Alternatively, services MAY provide RSD2 documents without any protocol information to indicate that no protocols are externally exposed.

The query interface accepts a list of protocol names conforming to the following JSON data-structure.

```json
[
"protocol-name1",
"protocol-name2",
...
"protocol-nameN"
]
```
(Line-breaks are for illustration purposes)

The protocol discovery returns only those services that fully match the requested protocol list. For example, the following statement will return only services that expose Moodle Mobile web-services.

```json
[
"org.moodle.mobile"
]
```
(Line-breaks are for illustration purposes)

If an app requires access to the mobile protocol __and__ the XAPI protocol, then the request SHOULD include both protocols such as:

```json
[
"org.moodle.mobile",
"gov.adlnet.xapi"
]
```
(Line-breaks are for illustration purposes)

Which protocols are available for a service are completely under control of the service providers. All services in the federation that wish to support mobile access through the edu-ID Mobile App MUST support the __org.ietf.oauth2__ protocol.

As a result the protocol discovery returns a list of RSD2 documents that matches the requested protocols. This list MAY include services that the authenticated user has not accessed previously.

If the requested protocols are not matched within the federation the protocol discovery MUST return an empty list.

The protocol discovery also allows to query for a list of specific services according to their base URLs exposed by the federation management using the following format:

```json
[
"service-url1",
"service-url2",
...
"service-urlN"
]
```
(Line-breaks are for illustration purposes)

For example, if a user has accessed the Moodle Systems at HTW Chur or at USI then the request would be as following:

```json
[
"https://moodle.htwchur.ch",
"https://www2.icorsi.ch/"
]
```
(Line-breaks are for illustration purposes)

The protocol discovery will query RSD2 information for any for the requested services. The result will include a list of RSD2 documents that matches the available services.

The list of returned RSD2 documents MAY be less or equal the number of requested services.

If none of the requested services are supported, then the protocol discovery will return an empty list.

The returned list MAY NOT be in the same order as the requested protocols. Clients MUST NOT assume that the response is in the same order as the request.

If the client is not requesting for an authenticated user, then the protocol discovery MUST reject the query.

The protocol discovery MAY be implemented as an independent federation service. In this case the client MUST authorize access using a service assertion.

#### RSD Discovery

The RSD Discovery processes the RSD exposure components of Federation services. If a Federation Service exposes their protocols using RSD2, then the RSD discovery will include the provided information into the protocol discovery.

The RSD Discovery relies on service URLs provided by the edu-ID Federation Index.

The RSD Discovery runs as a web-crawler with the protocol discovery. It periodically updates information from the federation index and tests if the provided RSD information at the federation services has changed (0).

#### edu-ID Mobile App Access To The Protocol Discovery

The edu-ID Mobile App access to the protocol discovery during two processes (3b).

1. If a Service Usage Manager is present, after successful authentication and whenever the edu-ID Mobile App is launched.

2. During a protocol request by a third party app.

If a Service Usage Manager is present, then the edu-ID Mobile App MAY pre-fetch a user's services for improving the user experience. In this case the edu-ID Mobile App can preselect relevant services that match a third-party app's protocol request without accessing the edu-ID Infrastructure.

During protocol requests by third party apps, the edu-ID Mobile App uses the protocol discovery to find all matching services for the request. These services MAY include services that a user might not be able to access.

Users can select to authorize with one, some or all services presented by the service discovery. The latter case MAY increase the number of user accounts and data traffic on Federation Services if no Service Usage Manager is present in the edu-ID Infrastructure.

### Institution-level User Authorization

The edu-ID Mobile App authorizes a user with Federation Services. Therefore, Federation Services MUST provide an edu-ID Authorization API that accepts the edu-ID Assertion (user authorization) of an edu-ID Mobile App client following the [RFC 7521](https://tools.ietf.org/html/rfc7521) with the [RFC 7523 extension](https://tools.ietf.org/html/rfc7523). The identity specific claims presented by the assertion token are subject to the [OpenID Connect Attribute specification](https://openid.net/specs/openid-connect-basic-1_0.html#rfc.section.2.5). The identity claims are beyond the scope of the edu-ID Mobile App functions.

An authenticated edu-ID Mobile App client will request a service assertion for one specific target Federation Service from the [authorization endpoint](https://tools.ietf.org/html/rfc6749#section-3.1) of the edu-ID Infrastructure. The client will receive the authorization endpoint of the target service together with an assertion token that it uses as an service grant code with the target service (4).

The Federation Service has to provide an OAuth2 token endpoint, which is used as the target endpoint during the client authorization process (5). During authorization the Federation Service validates the incoming assertion and verifies the service access for the asserted user.

If the Federation Service accepts an assertion presented by a edu-ID Mobile App client, it MUST provide an service access token for the edu-ID Mobile App.

The service access token MUST be a [JWT](https://tools.ietf.org/html/rfc7523) that contains a [JWK](https://tools.ietf.org/html/rfc7517) to be used with all further communication.

If the user or the assertion is rejected by the Federation Service, the Federation Service MUST reject the request appropriately. If a user is rejected by a Federation Service the edu-ID Mobile App MAY remove the service from being presented for authorization for that user.

If the edu-ID Authorization supports token scoping, then the access token for the edu-ID Mobile App MUST ONLY be scoped to the edu-ID Authorization Service.

A Federation Service MAY verify an assertion token with the edu-ID Service (6). This verification process is not part of the OAuth2 specifications.

An edu-ID Mobile App client may [revoke its authorization](https://tools.ietf.org/html/rfc7009).

The edu-ID Service MAY revoke access for an edu-ID Mobile App client via its assertion token.

![edu-ID Service Assertion Process](images/eduid_app_service_assertion.png)
(Letters in the blue box "Client Assertion" refer to the flow defined in [RFC6749](https://tools.ietf.org/html/rfc6749))

### Institution-level API Authorization

When a third party app requests access to protocols, the edu-ID Mobile App allows the users to decide which Federation Services should be granted for access (7).

If a user refuses access to any service, then that third will receive no authorization and no information about the user or potential services.

App authorization is only possible if the user's edu-ID Mobile App client has been authorized by the Federation Service (5).

If a user selects to grant access to a Federation Service the  edu-ID Mobile App client will connect to the edu-ID Authorization Component of the selected Federation Service and indicate that the user likes to grant access for the requested protocols to an identified third party app (8). This request will use the OAuth2 token endpoint using the ```authorization_code``` grant scheme.

The authorization code is a JWT with the following claims.

* iss: edu-ID Mobile App client id
* aud: Federation Service authorization endpoint
* sub: third party app's bundle id.

Additionally, the authorization request MUST include a scope referring to the service endpoints of the requested protocols.

This allows the federation service to determine whether to accept or reject certain third party apps.

The JWT presented by the edu-ID Mobile App client MUST be signed by the edu-ID Mobile App client using the key presented in the service access token.

During this process the edu-ID Mobile App MUST authenticate itself using the previously provided service access token.

The crypographic signatures presented authorization code and the client authentication MUST match.

If the federation service permits the requested app access it MUST provide an app access token to the edu-ID Mobile App client. This access token is passed by the edu-ID Mobile App to the third party app.

The app access token MUST grant the bearer access to the Federation Service's endpoints (9).

The format of the app access token is specific to the Federation Service implementation. It is recommended to provide cryptographically protected JWT-Bearer tokens.

If cryptographically protected JWT-Bearer tokens are used, then the issuer of the JWT token MUST match the subject during the app authorization (typically a bundle id).

If the Federation Service supports scoping, then the app access token MUST be scoped to the requested protocol endpoints. In this case the Federation Service MUST allow multiple authorizations for the same app instance for different scopes.

The Federation Service MUST link app access tokens to the service access token that has been used by the edu-ID Mobile App.

## Session Termination and Token Revocation

OAuth2 Tokens MAY have limited time to live.

If a token has a pre-determinated lifespan, the token MUST be accompanied by a [refresh token](https://tools.ietf.org/html/rfc6749#section-6). Both, edu-ID Mobile App clients and third party apps MUST be able to recover a new valid access token from the issuing authority after the old access token has expired.

Besides expired tokens, users, the edu-ID Mobile App, the edu-ID Service, and Federation Services may [revoke access tokens](https://tools.ietf.org/html/rfc7009). This is typically referred to as session termination or logout.

The tokens related to the edu-ID Mobile App are organised as a token tree. This means that all tokens that were issued on the grounds of another token are only valid as long the superior token remains valid. Consequently, if the superior token is revoked, then all connected tokens will be automatically revoked.

![edu-ID Token Tree](images/eduid_app_token_revokation_tree.png)

System administrators and users can revoke tokens via the edu-ID Service as well as through an edu-ID Mobile App client (10).

If an edu-ID Mobile App client or the edu-ID Service request to [revoke a client's service access token](https://tools.ietf.org/html/rfc7009), then the Federation Service MUST revoke all related app access tokens.

For revoked tokens it MUST NOT be possible to recover via refresh tokens.

### The special case of revoking assertion tokens

The edu-ID Service can only revoke previous assertions, because the service access tokens are private to the edu-ID Mobile App client. However, an action can be used only once and is then invalidated for further authorization.

In order to allow the edu-ID Service to revoke previous assertions at the federation services, a Federation Service MUST keep the originating assertion with the granted service access token. If the edu-ID Service revokes the assertion this is equivalent if a edu-ID Mobile App client revokes its service access token.

[Previous: Artwork](03-artwork.md) [Return Architecture Overview](00-overview.md) [Next: App Architecture](30-app-architecture.md)
