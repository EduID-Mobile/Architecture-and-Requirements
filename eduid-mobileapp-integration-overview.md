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

* __Third Party App__: A thrid party app refers to mobile apps on the users' devices that are not tightly integrated with a service infrastructure.

* __Academic Service__: An academic service is system that offers a set of functions to authorised users within the federation of academic institutions.

* __Service Endpoint__: A service endpoint refers to a URI that exposes a specific service function.

* __Protocol Endpoint__: A protocol endpoint combines several service endpoints that are used to achieve the functionality of a certain protocol.

* __Federation__: The federation are all academic services that are connected to the edu-ID Service.

* __User Credentials__: The user credentials are used to identify a user against the edu-ID Service. Typically they include username and password, but can also include other types of information.

* __Token__: A token is a string of alpha-numeric characters that is used to authorise a request to a service endpoint.

  * __request token__: A request token is a single use token that is used during the authorisation process.

  * __access token__: An access token is a token that identifies a series of requests to service endpoints.

## Key References

This document relies on the OAuth 2.0 Authorization Framework as specified in [RFC-6749](https://tools.ietf.org/html/rfc6749) and on the Assertion Framework for OAuth 2.0 Client Authentication and Authorization Grants as specified in [RFC-7521](https://tools.ietf.org/html/rfc7521).

This document does not spefify any Identity Provider (IDP) Functions. IDP functions are already part of the edu-ID system functions as speficied in the [Swiss edu-ID Architecture](https://projects.switch.ch/export/sites/projects/edu-ID/.galleries/documents/SwissEduIDArchitecture_Rev1.pdf).

## Academic Service - edu-ID App Architecture Overview

### Minimal Security Requirements

* User names and passwords are NEVER exposed to third party apps.

* Third party apps NEVER interact directly with the authorization services of the edu-ID infrastructure.

* Third party apps NEVER receive or request service endpoints without explicit authoriation of the users and services.

### Advanced Security Requirements

* User credentials are never transmitted in clear text over the Internet.

* Users can revoke all access to academic services from a device by logging out of the edu-ID app.

* Users can remotely revoke device access for all or individual devices.

* Users can remotely revoke access to individual services.

* Service administrators can revoke mobile access for individual users and apps.

* Service administrators can impose access policies for users and apps.

* edu-ID administrators can revoke access for users or their identities.

* Federation services need to authenticate when verifying the identity of edu-ID Mobile App users.

## Infrastructure Service Components


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
