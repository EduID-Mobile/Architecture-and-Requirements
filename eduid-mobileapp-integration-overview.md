# Swiss Edu-ID Mobile App Integration Framework

## Abstract

The edu-ID Mobile App provides an authorization and authentication interface for securely connecting third party apps with services in the trust domain of Swiss academic services. The edu-ID Mobile App ensures that commercial and non-commercial third party apps can provide added value services based on the existing service infrastructure in Swiss higher education institutions. The edu-ID App's key function is to authorise third party apps on a user's device with academic services within the edu-ID federation. It helps to bridge the user/app store trust domain on the user devices and the trust domain within the Swiss Academic Service Federation.

The edu-ID Mobile App integrates interactions between the edu-ID infrastructure, services in the academic edu-ID federation, and third-party apps installed on the users' devices. It limits service and data exposure to third-parties to authorised sources.

This document describes the overarching system architecture and service interaction protocols.

## Terms of Reference

The key words 'MUST', 'MUST NOT', 'REQUIRED', 'SHALL', 'SHALL NOT', 'SHOULD', 'SHOULD NOT', 'RECOMMENDED', 'MAY', and 'OPTIONAL' in this document are to be interpreted as described in [RFC2119](https://tools.ietf.org/html/rfc2119).

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

## Key References

This document relies on the OAuth 2.0 Authorization Framework as specified in [RFC-6749](https://tools.ietf.org/html/rfc6749) and on the Assertion Framework for OAuth 2.0 Client Authentication and Authorization Grants as specified in [RFC-7521](https://tools.ietf.org/html/rfc7521).

This document does not spefify any Identity Provider (IDP) Functions. IDP functions are already part of the edu-ID system functions as speficied in the [Swiss edu-ID Architecture](https://projects.switch.ch/export/sites/projects/edu-ID/.galleries/documents/SwissEduIDArchitecture_Rev1.pdf).

## Academic Service - edu-ID App Architecture Overview

The edu-ID mobile app is not a stand alone mobile application but mediates processes and data of services within the academic domain and third-party apps. Therefore, the edu-ID Mobile App is part of an IT ecosystem that spawn across two trust domains:

Firstly, the edu-ID links to the Swiss academic trust domain, which is managed via a central edu-ID service as federation manager that connects distributed services at academic institutions participating in the federation. In the context of this document this trust domain is called the Academic Trust Domain.

Secondly, it is part of the user-and-app-store trust domain on the users' devices. This trust domain is courated by the end-users by installing apps on their devices and controlled by the app store host. The app store hosts manage access to application developers by setting and enforcing quality and security guidelines. Within the context of this document this trust domain is called User Device.

Within these trust domains there is exaclty one edu-ID service in the Academic Trust Domain and exactly one edu-ID mobile app on the User Device. The edu-ID service and the edu-ID Mobile App are tightly connected. This connection is controlled by the host of the edu-ID service, which creates a separate edu-ID trust domain that bridges the Academic Trust Domain to the User Device.

The Academic Trust Domain connects many distributed "academic services". These services are typically hosted by the institutions that are participating in the federation.

On the User Devices there can be many apps that are related to the users' productivity. These apps are installed and arranged by the owners of the User Devices.

![Illustration of the edu-ID Mobile App ecosystem and trust domains](images/eduid_app_architecture.png)

Some of the services in the Academic Trust Doamin services expose service APIs for loosely-coupled machine-to-machine communication. Whereas tightly-coupled machine-to-machine communication requires explicit connections of two services typically initiated system wide by the system administrators, losely-coupled machine-to-machine communication allows unmanaged ad-hoc communications between services, tools and components. Tightly-coupled machine-to-machine communication can only be performed if all commuicators are known at the time of service coupling and controlable. Such connections create a pre-defined trust domain. In highly dynamic environments that spawn trust domains and involve many stakeholders, tight-coupling is resource intensive and does not easily scale. Loosely-coupled machine-to-machine communication overcomes the overhead of tight coupling by distributing coupling responsibility across the different stakeholders in the ecosystem.

Conventional [SAML]() Authentication and Authorization provides reliable identification and single-signon solutions within web-based trust domains. SAML-based approaches have been used to connect native mobile apps and distributed services. These approaches vary on the different platforms and require that native mobile apps are part of the same trust domain as the accessed services. SAML-based approaches are known to be prone to identity theft and viotation of intellectual property rights (IPR) if trust domains are naively assumed for native applications [REFERENCE](). This limits the applicability of SAML for loosely-coupled machine-to-machine communications, for which ad-hoc trust domains are established.

The edu-ID Mobile App ecosystem primarily focuses on client authorization and does not include an identity layer. Such layer might be provided by dedicated identity services based on LDIF, OpenID or Web-finger. Security aspects related to the identity profile layer are not addressed by the edu-ID Mobile App architecture.

### Enhanced OAuth 2.0 Framework

The edu-ID Mobile App architecture is build on top of the [OAuth 2.0 Framework](https://tools.ietf.org/html/rfc6749). The OAuth 2.0 Framework defines a process flow for authorized access to resource services from a wide range of clients. This framework is extended by [OAuth 2.0 Assertions](https://tools.ietf.org/html/rfc7521). OAuth 2.0 Assertions provide means to relay authorization across protocols and services.

The edu-ID Mobile App architecture __enhances__ the OAuth 2.0 Framework by cascading the specified OAuth 2.0 workflow and assertions. By doing so, the architecture does not add any new concepts to OAuth 2.0 aligns the specifications' concept into a coherent business logic.

### edu-ID Mobile App Ecosystem Stakeholders

* Federation Management

* Service Provider

* Resource Owner

* Device Owner

* Application Developer

### Minimal Security Requirements

* User names and passwords are NEVER exposed to third party apps.

* Third party apps NEVER interact directly with the authorization services of the edu-ID infrastructure.

* Third party apps NEVER receive service endpoints locations without explicit authorization of the users and services.

* Third party apps gain access ONLY to explicitly requested protocol endpoints or combinations of protocol endpoints.

### Advanced Security Requirements

* User credentials are never transmitted in clear text over the Internet.

* Users can revoke all access to academic services from a device by logging out of the edu-ID app.

* Users can remotely revoke device access for all or individual devices.

* Users can remotely revoke access to individual services.

* Service administrators can revoke mobile access for individual users and apps.

* Service administrators can impose access policies for users and apps.

* edu-ID administrators can revoke access for users or their identities.

* edu-ID administrators can invalidate access for specific edu-ID mobile app versions.

* Federation services need to authenticate when verifying the identity of edu-ID Mobile App users.

## Infrastructure Service Components

The four components of the edu-ID Mobile App ecosystem rely on the interplay of several interdependent components. Each of these components provides service endpoints of the enhanced OAuth 2.0 Framework.

![edu-ID Mobile App Component Overview](images/eduid_app_architecture-details.png)

### edu-ID Architecture Extensions

#### Device Assertion

#### User Authentication

#### Service Discovery

#### Service Assertion

#### Protocol Discovery


### Academic Protocol Endpoints and components

#### Service Exposure

#### edu-ID Authorization

#### App Assertion

#### Access Authorization


### Third-party mobile app components

#### Access Management

#### Authorization Management


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




---


The edu-ID app will authenticate itself via a service grant token that is issued by the edu-ID service for one academic service in the federation. If provided to an academic service, the academic service can validate the token with the edu-ID service. A service grant token is bound to one academic service and can be validated only by that service.

If a service grant token has been used by an academic service, the edu-ID service will immediately invalidate the service grant token and present the user information to the academic service. The edu-ID Service will track, which edu-ID Apps have requested access to which services for allowing users to invalidate their devices within the federation.

On the grounds of the user information an academic service can authorise a user's edu-ID app via an edu-ID access token. The service access token is unique for each edu-ID app instance. Academic services MAY additionally assign a time-to-live (TTL) interval to the edu-ID access token.

The edu-ID access token authorises a user on a given device. The edu-ID access token is a shared secret between the edu-ID App and the Academic Service. The edu-ID access token MUST ONLY be used with the app authorisation protocol endpoint of the service.

The edu-ID App uses the edu-ID access token to authorise a third party app for one or more protocol endpoints of an academic service. In this process the edu-ID will present its access token, the App ID, and the requested protocols to the academic service. The academic service MAY reject to authorise certain App ID-protocol combinations.

If a protocol request by the edu-ID App is granted, the academic service will send an app access token to the edu-ID App. The edu-ID App will provide this app access token to the requesting third party app. This app token is unique for the requested App ID and service protocols. Academic services MAY additionally associate a TTL interval to the app access token that can be different to the TTL for the related edu-ID access token.

App access tokens are used as shared secrets between the academic service and a third party app. The edu-ID App MUST NOT use app access tokens unless it requests to invalidate them.

As long an app access token is valid a third party app may use the app access token to access the authorised protocol endpoints on behalf of the user.

If an academic service invalidates an app access token the thrid party app MUST receive an error message that indicate that the app needs to re-authorise.

If an academic service invalidates an edu-ID access token, it MUST invalidate all app access tokens that were granted using the edu-ID access token.

A user or the edu-ID app MAY request to invalidate the edu-ID access token for a given device. In this case all related app access tokens related to this edu-ID access token MUST be also invalidated.

The edu-ID Service MAY request the academic services to invalidate certain devices. In this case the edu-ID access token and the related app tokens MUST be invalidated.



## References

URI
