# Swiss Edu-ID Mobile App Integration Framework

## Abstract

The edu-ID Mobile App provides an authorization and authentication interface for securely connecting third party apps with services in the trust domain of Swiss academic services. The edu-ID Mobile App ensures that commercial and non-commercial third party apps can provide added value services based on the existing service infrastructure in Swiss higher education institutions. The edu-ID App's key function is to authorise third party apps on a user's device with academic services within the edu-ID federation. It helps to bridge the user/app store trust domain on the user devices and the trust domain within the Swiss Academic Service Federation.

The edu-ID Mobile App integrates interactions between the edu-ID infrastructure, services in the academic edu-ID federation, and third-party apps installed on the users' devices. It limits service and data exposure to third-parties to authorised sources.

This document describes the overarching system architecture and service interaction protocols.

## Terms of Reference

The key words 'MUST', 'MUST NOT', 'REQUIRED', 'SHALL', 'SHALL NOT', 'SHOULD', 'SHOULD NOT', 'RECOMMENDED', 'MAY', and 'OPTIONAL' in this document are to be interpreted as described in [RFC-2119](https://tools.ietf.org/html/rfc2119).

## Terminology

* __edu-ID Service__: The edu-ID Service is the authorisation endpoint for the federation of academic services. There is one edu-ID Service for the entire federation.

* __edu-ID App__: The edu-ID App provides a trusted environment on the users' mobile device. The edu-ID App is a navtive mobile app that is installed on a user's mobile device.

* __Third Party App__: A thrid party app refers to mobile apps on the users' devices that are not tightly integrated with a service infrastructure and/or is provided by a potentially untrusted party outside of the truested federation domain.

* __Academic Service__: An academic service is system that offers a set of functions to authorised users within the federation of academic institutions.

* __Service Endpoint__: A service endpoint refers to a URI that exposes a specific service function.

* __Protocol Endpoint__: A protocol endpoint combines several service endpoints that are used to achieve the functionality of a certain protocol.

* __Federation__: The federation are all academic services that are connected to the edu-ID Service.

* __User Credentials__: The user credentials are used to identify a user against the edu-ID Service. Typically they include username and password, but can also include other types of information.

* __Token__: A token is a string of alpha-numeric characters that is used to authorise a request to a service endpoint.

  * __request token__: A request token is a single use token that is used during the authorisation process.

  * __access token__: An access token is a token that identifies a series of requests to service endpoints.

* __Trust Domain__: A trust domain defines an environment of shared trust between applications and/or services, in which data can reliably exchanged.

## The Problem of Integrating Mobile Apps into Service Federations


## The Solution as Defined by the edu-ID Mobile App Architecture


## Key References

This document relies on the OAuth 2.0 Authorization Framework as specified in [RFC-6749](https://tools.ietf.org/html/rfc6749) and on the Assertion Framework for OAuth 2.0 Client Authentication and Authorization Grants as specified in [RFC-7521](https://tools.ietf.org/html/rfc7521).

This document does not spefify any Identity Provider (IDP) Functions. IDP functions are already part of the edu-ID system functions as speficied in the [Swiss edu-ID Architecture](https://projects.switch.ch/export/sites/projects/edu-ID/.galleries/documents/SwissEduIDArchitecture_Rev1.pdf).

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

The edu-ID Mobile App architecture is build on top of the [OAuth 2.0 Framework](https://tools.ietf.org/html/rfc6749). The OAuth 2.0 Framework defines a process flow for authorized access to resource services from a wide range of clients. This framework is extended by [OAuth 2.0 Assertions](https://tools.ietf.org/html/rfc7521). OAuth 2.0 Assertions provide means to relay authorization across protocols and services.

The edu-ID Mobile App architecture __enhances__ the OAuth 2.0 Framework by cascading the specified OAuth 2.0 workflow and assertions. By doing so, the architecture does not add any new concepts to the OAuth 2.0 Framework but aligns the specifications' concepts into a coherent business logic for machine-to-machine trust negociation. This logic is not part of the OAuth 2.0 specifications and is therefore left to actual OAuth 2.0 service implementations.

### edu-ID Mobile App Ecosystem Stakeholders

Human stakeholders play a key role in the process of machine-to-machine communication across trust domains. Therefore, the experience of integrating and using the infrastructure must not add complexity to the app development, system maintenance, or device usage. Four key stakeholders are identified for the architecture.

* Federation Management

* Service Provider

* Mobile Device Owner

* Application Developer

### Minimal Security Requirements

* [ ] User names and passwords are NEVER exposed to third party apps.

* [ ] Third party apps NEVER interact directly with the authorization services of the edu-ID infrastructure.

* [ ] Third party apps NEVER receive service endpoints locations without explicit authorization of the users and services.

* [ ] Third party apps gain access ONLY to explicitly requested protocol endpoints or combinations of protocol endpoints.

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

The user authorization process refers to the sequence of steps to tie an user identity to an edu-ID Mobile App instance on a mobile device. This process consists of connecting an edu-ID Mobile App instannce to the edu-ID Federation Service via the device assertion protocol (1), the OAuth user authentication (2) and exchange of user attributes (2a), and the service usage (3) and protocol discovery (3a and 3b). Including service usage resources and protocol discovery into this process is for user convenience in large federations, in which users access only a minor subset of available services. This enables the edu-ID Mobile App to filter the services to which a user has connected one or more identities instead of giving access to all possible services. The protocol discovery (3b) supports the edu-ID Mobile App to identify suitable services that match protocol requests of third party apps.

![edu-ID App Authorization Process](images/eduid_app_architecture_authorization.png)

In the above figure the blue box refers to the steps as specified for OAuth 2.0. The green boxes refer to OAuth Authorization service endpoints and the orange boxes refer to resource service endpoints.

### 2. Service Assertion

The service assertion process refers to the steps that authorise an edu-ID Mobile App instance to an academic service. Only if the edu-ID Mobile App has granted access to an academic service, it will relay access authorization to third-party apps for that service. This process consists of requesting an authorization grant from the edu-ID Federation Service (4), which results in a unique service grant token for the given service. The edu-ID Mobile App requests authorization to the selected service by forwarding the service grant token to it (5). The service grant token authenticates a user to exactly one service. The service grant token can be automatically validated by the academic services. OPTIONALLY, the selected academic service can validate a client token included with the service grant token with the edu-ID Federation Service (6) and/or request profile information from it (6*). The academic services MUST only grant or reject access  and issue a service access token on the grounds of the results of validating the service grant token with the edu-ID Federation Service for completing the authorization of the edu-ID Mobile App. At any point the edu-ID Federation Service or the edu-ID Mobile App MAY invalidate some or all tokens for an edu-ID Mobile App instance with an academic service. In both cases, the academic service MUST invalidate the requested token and all tokens that are issued for that token in the app assertion process (10).

![edu-ID Service Assertion Process](images/eduid_app_service_assertion.png)

In the above figure the blue box refers to the steps as specified for OAuth 2.0. The green boxes refer to OAuth Authorization service endpoints and the orange boxes refer to resource service endpoints.

### 3. App Assertion

The app assertion process refers to granting access to one or more protocol endpoints of an academic service to a third-party app. This process is initiated by the third-party app by requesting access to one or more protocols using the mobile device's operating system mechanics for inter-app communication as a result of a user interaction (7). This request is forwarded by the mobile operating system to the edu-ID Mobile App, which selects possible services on the grounds of the requested protocols. The edu-ID Mobile App informs the mobile device owner that an app requests access to the suitable services and provide the mobile device owner the opportunity to select none, one, some, or all suitable services to be accessed by the requesting app. For each selected service the edu-ID Mobile App asserts the third-party app's access using the edu-ID Mobile App's service access token (8). At this point academic services MAY grant or reject access for the requesting app. Academic services SHOULD only grant an app access token for the requested protocol endpoints. For each academic service that issues an app access token, the edu-ID Mobile App will send the protocol endpoints as well as the related app access token to the third-party app. The third-party app MUST authorize all requests to the protocol endpoints of an academic service using the issued app access token (9). If the third-party app receives an invalid token error from an service endpoint, it SHOULD re-initiate the app assertation process (7). Only if the academic service has issued a refresh token with the app access token, the third-party app MAY request a new app access token from the academic service.

![edu-ID App Assertion Process](images/eduid_app_assertion.png)

## Infrastructure Components and Service Endpoints

### edu-ID Architecture Extensions

#### Device Assertion

The device assertion component provides the service endpoint for the client assertion process as specified in [RFC-7521](https://tools.ietf.org/html/rfc7521). This component allows identification of individual edu-ID Mobile App instances on different clients. Each version of the edu_ID Mobile App that is distributed to a plattform app store has a shared client identifier and client key. The __client identifier__ and __client key__ are unique to the edu-ID Mobile App version for each mobile plattform. The client identifier includes public app identier, platform, and version number. The client key is issued by the Federation Administrator to the edu-ID Mobile App Developers before release of a new edu-ID Mobile App client version for a platform. Any edu-ID Mobile App instance MUST NOT send the client key over an unsecured channel. Any edu-ID Mobile App instance MUST NOT send the client key to any other endpoints than those exposed by the device assertion component of the edu-ID Federation Service.

##### Related Process

* User Authorization

##### Endpoints

###### device-assertion

The device-assertion endpoint issues a permanent device key that identifies a edu-ID Mobile App instance. To authorized requests the endpoint provides a unique client token consisting of a client_id, a client_secret, and a code.

__Accepted methods__:

* POST

__Accepted Content-Types__:

* application/json

* application/x-www-form-urlencoded

__Authorization__:

The client must add a [Basic Authorization](https://tools.ietf.org/html/rfc7617) header including a client identifier and a client key.

__Input Message__:

* client_assertion_type: The client assertion type MUST be the url-encoded string ```urn:ietf:params:oauth:grant-type:authorization_code:device``` (as plain text: ```urn%3Aietf%3Aparams%3Aoauth%3Agrant-type%3Aclient-authorization_code```)

* grant_type: the grant type MUST be ```eduid_client```.

* client_id: user readable name of the device on which the edu-ID Mobile App is running. This name is typically provided by the Device Owner in the system setup. The client_name is used for identifying individual clients.

* client_assertion: must be set to the unique identifier of the device the edu-ID is running. Depending on the device plaform the device id is specific to the edu-ID Mobile App instance and may change between installations and mobile operating system upgrades.

__Response Message__:

The response message is of content-type ```application/json```.

* client_id: unique identifier of the edu-ID Mobile App instance on a User Device. The identifier is used as authorization name during user authentication (2)

* client_secret: challenge key used as authorization secret during user authentication (2)

* code: sign key for authentification challenges during user authentication (2). The code is a shared secret between the edu-ID Federation Service and the the edu-ID Mobile App instance on the User Device. The code MUST NOT be sent by the client to any service endpoint.

__Example__:

client identifier: ```ch.eduid.mobile.ios.20160526```
client key: ```helloWorld```

Request:

```
POST /eduid/service/device-assertion HTTP/1.1
HOST www.eduid.ch
Auhorization: Basic Y2guZWR1aWQubW9iaWxlLmlvcy4yMDE2MDUyNjpoZWxsb1dvcmxk
Content-type: application/json

{"client_assertion_type": "urn%3Aietf%3Aparams%3Aoauth%3Agrant-type%3Aclient-authorization_code","client_id":"example device","client_assertion":"41AFFE15-7666-426B-8DD6-AED1BBAF70CC","grant_type":"eduid_client"}
```

Response:

```
HTTP/1.1 200 OK
Content-type: application/json

{"client_id":"BE74E66B-67DE-429A-B8D5-202BFB6298D4","client_secret":"9DEE73EF5696","code":"A069A6FC.B164BDD9EBE-451CBB22167F5BAD2912C7054609_A6E.7985A3064338D"}
```

#### User Authentication

The user authentication component is part of the edu-ID Core Architecture. Different to web-based environments where the source of the user interface can be enforced by the service, this is not possible for native mobile applications. Therefore, edu-ID Mobile App instances need to authorize themselves for the authentication process.

##### Related Process

* User Authorization

* App Assertion

##### Endpoints

###### authorization

The authorization endpoint has two modes. If called without a password, then it works in challenge mode. It it is called with a password parameter, then it works in authorization mode.

In challenge mode the authorization returns a challenge to be used as salt for password hashing. The endpoint will __always__ return a challenge, regardless of the existance of the requested user identity. As soon the user entered an identity identifier (e.g. an e-mail adderess or a telephone number), edu-ID Mobile App needs to request the authorization challenge for the user.

The password is calculated by creating the salted password hash, as it is available to the edu-ID Authentication Service. This hash is the hashed with the code of the device assertion, and the presented user id. The following javascript code illustrates the algorithm.

```javascript
var pwHash = sha1(pwSaltChallenge+ uiPassword);
var psAuth = sha1(deviceToken.code + "\n" +
                  uiUserId + "\n" +
                  pwHash + "\n");
```

__Accepted methods__:

* POST

__Accepted Content-Types__:

* application/json

* application/x-www-form-urlencoded

__Authorization__:

The client must add a [Basic Authorization](https://tools.ietf.org/html/rfc7617) header including a client_id and a client_secret provided by the device-assertion endpoint.

__Input Message__ (challenge mode):

* challenge: The challenge is a sha1 hash of the direct concatenation of the client_secret and the code.

* user_id: identity identifier, such as an e-mail address or a telephone number.

__Response Message__ (challenge mode):

The response message is of content-type ```application/json```.

* challenge: string that used for password hashing.

__Input Message__ (authentication mode):

* response_type: the response type must have the value ```code``` because no grant redirection is possible.

* challenge: The challenge is a sha1 hash of the direct concatenation of the client_secret and the code.

* user_id: identity identifier, such as an e-mail address or a telephone number.

* password challenge: sha1 encrypted password.

__Example__ (challenge mode):

The example uses the response data from the device-assertion example

Request:

```
POST /eduid/service/authorization HTTP/1.1
HOST www.eduid.ch
Auhorization: Basic QkU3NEU2NkItNjdERS00MjlBLUI4RDUtMjAyQkZCNjI5OEQ0OjlERUU3M0VGNTY5Ng==
Content-type: application/json

{"challenge":"ed3b21c754ba77151f6572ee65dc48374d216bea","username":"hello@example.com"}
```

Response:

```
HTTP/1.1 200 OK
Content-type: application/json

{"challenge":"lGuC5-Iun1GyXI"}
```

__Example__ (authentication mode):

* The example uses the response data from the device-assertion example

* The password is ```helloWorld```

Request:

```
POST /eduid/service/authorization HTTP/1.1
HOST www.eduid.ch
Auhorization: Basic QkU3NEU2NkItNjdERS00MjlBLUI4RDUtMjAyQkZCNjI5OEQ0OjlERUU3M0VGNTY5Ng==
Content-type: application/json

{"challenge":"ed3b21c754ba77151f6572ee65dc48374d216bea",
 "username":"hello@example.com",
 "password":"2006a7cf9722f26e22c426f5f2490d7d093b6c2d"}
```

Response:

```
HTTP/1.1 200 OK
Content-type: application/json

{"token_type":"mac",
 "access_token":"Ia.J663iTFhL0bBAfmAX.qdBJ81k7lNFlcgh80qRExuEzW7C9z",
 "kid":"qzOu1prSzo",
 "mac-key":"3cUee1lc-_TwXc1mZYPYm6m1pcfEUDTNFCRKu2NksxEfzxrol7bx3no",
 "mac_algorithm":"hmac-sha-1"}
```

#### Service Discovery

The service discovery is a convenience component that preselects the services a user has used in the past, because it is more likely that the Device owner wants to use these services with their apps. This component provides a list of services a user has accessed in the past, regardless of the mode used to access an academic service.

This component MUST respond only to authorized requests.

This component MAY exclude services that refused access to the authorised user in the past.

This component SHOULD exclude services that do not expose services.

##### Related Process

* User Authorization

##### Endpoints

###### service-discovery

__Accepted methods__:

* GET

__Accepted Content-Types__:

* Not Applicable

__Authorization__:

The edu-ID Mobile App instance MUST send an Autorization header using the authorization method of its edu-ID access token. Both service endpoints MUST refuse access to unauthorized edu-ID Mobile App instances.

Request:

```
GET /eduid/service/service-discovery HTTP/1.1
HOST www.eduid.ch
Auhorization: MAC kid=DfSc3K2OWH,ts=1463244822,mac=98b4b91feccc97604c5559d94f39c92b9168b178

```

Response:

```
HTTP/1.1 200 OK
Content-type: application/json

["https://moodle.htwchur.ch","https://ilias.unibe.ch","https://www2.icorsi.ch/"]
```

#### Service Assertion

The service assertion component is part of asserting authorization for academic sercies to the edu-ID Mobile App. This component is used by the edu-ID Mobile App to get access an academic service. The academic service can automatically verify the assertation token bassed on shared secrets between the academic service and the edu-ID Federation Server. The academic service may use the the assertion token to request user profile information if necessary.

This component tracks, which academic services have been accessed and provides this information to the service discovery. The component can use accepted authorization grants to inform the accepting academic service that the requesting edu-ID Mobile App access token has been invalidated. If the service assertion revokes an accepted grant, the accepting accademic service MUST invalidate all active tokens that originate to that grant token.

The asserted JWT uses a unique user token that identifies the authorised user to the academic service as subject of the assertion. This user token can be used by the academic service to verify the user identity. This claim is identified by the key ```https://www.eduid.ch/usertoken```.

Additionally, the assertion includes a claim is a unique bearer token that identifies the client to the academic service as a client token. This claim is identified by the key ```https://www.eduid.ch/client```. The edu-ID Federation Service MAY use this client token in order to revoke access to the service.

The assertion is secured by a JWS signature that uses a shared secret between the academic service and the edu-ID Federation Service as key for the signature algorithm. This enables the targeted service to verify the validity of the assertation without accessing the edu-ID Federation Service.

If configured, the assertion may include additional claims requested by the academic service from the edu-ID Federation Server.

##### Related Process

* Service Assertion

##### Endpoints

All endpoints are only available to OAuth authorized requests.

###### service-assertion

__GET method__

The get method expects a client grant token as search string to authorized services.

__Response message__

If the provided client grant token has been issued to the requesting service, then the response will return the user token as provided in the subject of the client assertion. Otherwise the response will be a 403 Forbidden error.

__Example__

Request:

```
GET /eduid/service/service-protocols?sfsoq94kal.d0-94914sF HTTP/1.1
HOST www.eduid.ch
Auhorization: MAC kid=DfS0K2OWH,ts=1463244822,mac=98b4b91... ...78
Content-type: application/json

```

Response:

```
HTTP/1.1 200 OK
Content-type: application/json

{"sub":"asdiqw4-0sadoerip"}
```

__POSTS method__

__Accepted Content-Types__:

* application/json

* application/x-www-form-urlencoded

__Input Message__

* service_uri: URI to the academic service the authorized client seeks to access.

__Response Message__

As response the endpoint returns an client assertion token using a [JWS signed](https://tools.ietf.org/html/rfc7797) [JSON Web-Tokens (JWT)](https://tools.ietf.org/html/rfc7523) as assertion. The edu-ID Mobile App instance MUST present the unaltered assertation to the academic service it whishes to access.

__Example__

Request:

```
POST /eduid/service/service-protocols HTTP/1.1
HOST www.eduid.ch
Auhorization: MAC kid=DfSc3K2OWH,ts=1463244822,mac=98b4b91feccc97604c5559d94f39c92b9168b178
Content-type: application/json

{"service_uri":"https://moodle.htwchur.ch"}
```

Response:

```
HTTP/1.1 200 OK
Content-type: application/json

{"grant_type":"urn%3Aietf%3Aparams%3Aoauth%3Agrant-type%3Ajwt-bearer",
 "assertion":"eyJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwczovL2VkdWlkLmNoIiwiYXVkIjoiaHR0cHM6Ly9tb29kbGUuaHR3Y2h1ci5jaCIsInN1YiI6ImFzZGlxdzQtMHNhZG9lcmlwIiwiZXhwIjoiMTQ2MzI0NDUyMiIsImh0dHBzOi8vZWR1aWQuY2gvY2xpZW50IjogInNmc29xOTRrYWwuZDAtOTQ5MTRzRiJ9.c610b3e69d5c9a833a63da16338c8eeac0c7a2e6709df8dae299b3a3ceb3646e"}
```


#### Protocol Discovery

The protocol discovery is based on the publicly available protocol endpoints that for an academic service. Protocol endpoints combine one or more service endpoints following a well defined **protocol** of accessing the endpoints. Therefore, a protocol endpoint is a root path for the service endpoints of the related protocol. The protocol discovery component's prime function is to support the edu-ID Mobile App during the app assertion process to select suitable services for a thir-party app's protocol request (7).

An instance may directly forward the requested protocol list, upon which the protocol discovery component MUST respond a list of protocol endpoints that contains all academic services that expose the exact protocols as requested. The protocol discovery will return the unprocessed service discovery information as provided by the academic services.

The edu-ID Mobile App MAY prefetch protocol endpoints for a authorized user's services during the user authorization process.

The protocol discovery is tightly coupled to the edu-ID Federation Service's protocol detection. The protocol detection verifies the services that are exposed by the academic services. The protocol detection uses the [RSD2 service discovery](https://github.com/BLC-HTWChur/rsd2-specification/blob/master/rsd2-specification.md).

##### Related Processes

* User Authorization

* App Assertion

##### Endpoints

All endpoints are only available to OAuth authorized requests from edu-ID Mobile Apps.

All service endpoints respond a list (JSON Array) of unprocessed RSD2 data as provided by the academic service.

__Accepted methods__:

* POST

__Accepted Content-Types__:

* application/json

__Authorization__:

The edu-ID Mobile App instance MUST send an Autorization header using the authorization method of its edu-ID access token. Both service endpoints MUST refuse access to unauthorized edu-ID Mobile App instances.

###### service-protocols

This service-protocols endpoint accepts a list of requested protocol names and returns the academic services' discovery information to an edu-ID Mobile App instance. This service endpoint MUST expose all services that expose the exact list of requested protocols.


__Input Message__:

* unnamed list of academic service links exposed by the academic services in their RSD2 data.

__Response Message__:

The response message is of content-type ```application/json```.

* unnamed list of RSD2 definitions of matching academic services.

__Example__:

Request:

```
POST /eduid/service/service-protocols HTTP/1.1
HOST www.eduid.ch
Auhorization: MAC kid=DfSc3K2OWH,ts=1463244822,mac=98b4b91feccc97604c5559d94f39c92b9168b178
Content-type: application/json

["https://moodle.htwchur.ch","https://ilias.unibe.ch","https://www2.icorsi.ch/"]
```

Response:

```
HTTP/1.1 200 OK
Content-type: application/json

[{"engineName": "moodle@ HTW Chur",
  "engineLink": "",
  "homePageLink": "https://moodle.htwchur.ch",
  "homePageIcon": "/favico.png","apis": {...}}, ...]
```

The response has been shortened for readability purposes.

###### protocols

This protocols endpoint accepts a list of academic services and returns the academic services' discovery information to an edu-ID Mobile App instance. If an requested service does not expose any services via RSD2, no information will be included for that service.

__Input Message__:

* unnamed list of protocol names as exposed by the academic services in their RSD2 data.

__Response Message__:

The response message is of content-type ```application/json```.

* unnamed list of RSD2 definitions of matching academic services

__Example__:

Request:

```
POST /eduid/service/service-protocols HTTP/1.1
HOST www.eduid.ch
Auhorization: MAC kid=DfSc3K2OWH,ts=1463244822,mac=98b4b91feccc97604c5559d94f39c92b9168b178
Content-type: application/json

["org.moodle.mobile","gov.adlnet.xapi","org.imsglobal.qti"]
```

Response:

```
HTTP/1.1 200 OK
Content-type: application/json

[{
  "engineName": "moodle@ HTW Chur",
  "engineLink": "",
  "homePageLink": "https://moodle.htwchur.ch",
  "homePageIcon": "/favico.png","apis": {...}}, ...]
```

The response has been shortened for readability purposes.

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

