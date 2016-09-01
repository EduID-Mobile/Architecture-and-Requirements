# Terminology

The edu-ID Mobile App provides an authorization and authentication interface for securely connecting third party apps with services in the trust domain of Swiss academic services. The edu-ID Mobile App ensures that commercial and non-commercial third party apps can provide added value services based on the existing service infrastructure in Swiss higher education institutions. The edu-ID App's key function is to authorise third party apps on a user's device with academic services within the edu-ID federation. It helps to bridge the user/app store trust domain on the user devices and the trust domain within the Swiss Academic Service Federation.

The edu-ID Mobile App integrates interactions between the edu-ID infrastructure, services in the academic edu-ID federation, and third-party apps installed on the users' devices. It limits service and data exposure to third-parties to authorised sources.

This document defines the core terminology that is used throughout the architecture documentation.

## Terms of Reference

The key words 'MUST', 'MUST NOT', 'REQUIRED', 'SHALL', 'SHALL NOT', 'SHOULD', 'SHOULD NOT', 'RECOMMENDED', 'MAY', and 'OPTIONAL' in the entire Swiss edu-ID Mobile App architecture, including this document are to be interpreted as described in [RFC-2119](https://tools.ietf.org/html/rfc2119).

## Introduction

The Swiss edu-ID Mobile App interfaces with the Swiss Academic Cloud Services. Academic services are organised in a service federation. This federation forms a trust domain in which academic user are managed by their host institution but can access institutional and shared services using their institutional user credentials.

The Swiss academic federation presently includes over 800 service instances. Therefore, mobile authorization services have depend on open standards that can be easily integrated into the underlying products.

The present Swiss academic cloud services form a federation on top of the SAML specification. This specifications has several known limitations. Recent activities in the context of the Internet Engineering Task Force (IETF) have tackled these limitations for greater flexibility and security for identification and authorization services. The result are two specifications that are driven and supported by major industrial actors including, but not limited to Google, Microsoft and Facebook:

* OAuth2
* OpenID Connect.

OAuth2 provides a framework for authorized access to resources and services on the Internet. OAuth2 handles only use cases related to authentication and authorization. Similarly to SAML, OAuth2 is designed to support authorized access to services over the Internet. While the SAML has been designed on top of HTTP and requires an HTTP user agent, OAuth2 allows authorization over HTTP, it is not limited to HTTP so it can be used across different protocols and types of user agents. This allows OAuth2 to be used in scenarios, in which user interactions with a web-based resource are not possible or desirable.

The main difference of OAuth2 over SAML is that OAuth2 specification only specifies a protocol framework. Many aspects for implementing OAuth2 build on top of other specifications that are partly independent from OAuth2. This includes identity management as OAuth2 is agnostic to any form of identity management.

OpenID Connect builds on top of OAuth2 and extends the framework towards identity management. OpenID Connect is designed to pass identity information between authorization services or identify providers (IDP) and resource services transparently.

The Swiss edu-ID Mobile App builds entirely on top of OAuth2. Like OAuth2 the app's core functions are agnostic to identity information. Therefore, this architecture documentation does not consider identity information as specified for OpenID Connect. Furthermore, SWITCH proposed a number of non-standard identity information and handling procedures.

## DISCLAIMER

All documents that are part of the present architecture silently assume that identity information conforms to OpenID Connect and/or SWITCH edu-ID Identity requirements whenever parties depend on such information.

## Underpinning Specifications

The Swiss edu-ID Mobile App architecture builds on top of the following specifications. Implementation-related requirements defined in the following specifications are not repeated in this document.

* [RFC-2617: The Basic HTTP Authorization Scheme](https://tools.ietf.org/html/rfc2617)
* [RFC-6749: The OAuth 2.0 Authorization Framework](https://tools.ietf.org/html/rfc6749)
* [RFC-7009: OAuth2 Token Revocation](https://tools.ietf.org/html/rfc7009)
* [RFC-7516: JSON Web Encryption (JWE)](https://tools.ietf.org/html/rfc7516)
* [RFC-7517: JSON Web Key (JWK)] (https://tools.ietf.org/html/rfc7517)
* [RFC-7518: JSON Web Algorithms (JWA)](https://tools.ietf.org/html/rfc7518)
* [RFC-7519: JSON Web Tokens (JWT)](https://tools.ietf.org/html/rfc7519)
* [RFC-7521: Assertion Framework for OAuth 2.0 Client Authentication and Authorization Grants](https://tools.ietf.org/html/rfc7521)
* [RFC-7523: JSON Web-Tokens (JWT) for OAuth 2.0 Client Authentication and Authorization Grants](https://tools.ietf.org/html/rfc7523)
* [RFC-7797: JSON Web Signature](https://tools.ietf.org/html/rfc7797)
* [RFC-7800: Proof-of-Possession Key for JWT](https://tools.ietf.org/html/rfc7800)

* [Swiss edu-ID Architecture](https://projects.switch.ch/export/sites/projects/eduid/.galleries/documents/SwissEduIDArchitecture_Rev1.pdf)

* [RSD2 service discovery](https://github.com/BLC-HTWChur/rsd2-specification/blob/master/rsd2-specification.md)

## Informative Sources

* Swiss edu-ID Identity Entries

* [OpenId Connect 1.0](https://openid.net/specs/openid-connect-basic-1_0.html)

* [RFC-7520: Examples of protecting content using JSON Object Signing and Encryption](https://tools.ietf.org/html/rfc7520)

This architecture does not specify any data formats or service endpoints that are already covered by these specifications. For all protocol flows documented throughout the Swiss edu-ID Mobile App architecture the underpinning specification is the point of reference.

Conflicts between the present architecture document and the related specifications MUST be considered as mistakes in the present architecture. In case of conflicts, identifying parties SHOULD report this conflict in order to correct it.

## Terms used

* __Federation__: The federation are all academic services that are connected to the edu-ID Service.

* __edu-ID Service__: The edu-ID Service is the authorisation endpoint for the federation of academic services. There is one edu-ID Service for the entire federation.

* __edu-ID Mobile App__: The edu-ID Mobile App provides a trusted environment on the users' mobile device. The edu-ID App is a native mobile app that is installed on a user's mobile device.

* __Third Party App__: A third party app refers to mobile apps on the users' devices that are not tightly integrated with a service infrastructure and/or is provided by a potentially untrusted party outside of the trusted federation domain.

* __Federation Service__: A federation service is system that offers a set of functions to authorised users within the federation of academic institutions.

* __Service Endpoint__: A service endpoint refers to a URI that exposes a specific service function.

* __Protocol Endpoint__: A protocol endpoint combines several service endpoints that are used to achieve the functionality of a certain protocol.

* __User Credentials__: The user credentials are used to identify a user against the edu-ID Service. Typically they include username and password, but can also include other types of identifying information such as the ID of the student card.

* __Token__: A token is a string of alpha-numeric characters that is used to authorise a request to a service endpoint.

  * __Request token__: A request token is a single use token that is used during the authorisation process. Request tokens are always JWT tokens.

  * __Access token__: An access token is a token that identifies a series of requests to service endpoints. Access tokens are either MAC tokens or JWT tokens, depending on the protocol endpoint requirements

* __Trust Domain__: A trust domain defines an environment of shared trust between applications and/or services, in which data can reliably exchanged.

## Security related notes

This architecture refers to keys as cryptographic tools. This architecture is agnostic with respect to the cryptographic method. Whenever, this architecture document refers to cryptographic keys it assumes conformance to the [JWA specification](https://tools.ietf.org/html/rfc7518).

| [Previous: User Experience](03-artwork.md) | [Return Architecture Overview](00-overview.md) | [Next: Service Architecture](20-service-architecture.md) |
| :---- | :----: | ----: |