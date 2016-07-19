# Swiss Edu-ID Mobile App Terminology

The edu-ID Mobile App provides an authorization and authentication interface for securely connecting third party apps with services in the trust domain of Swiss academic services. The edu-ID Mobile App ensures that commercial and non-commercial third party apps can provide added value services based on the existing service infrastructure in Swiss higher education institutions. The edu-ID App's key function is to authorise third party apps on a user's device with academic services within the edu-ID federation. It helps to bridge the user/app store trust domain on the user devices and the trust domain within the Swiss Academic Service Federation.

The edu-ID Mobile App integrates interactions between the edu-ID infrastructure, services in the academic edu-ID federation, and third-party apps installed on the users' devices. It limits service and data exposure to third-parties to authorised sources.

This document defines the core terminology used throughout the architecture documentation.

## Terms of Reference

The key words 'MUST', 'MUST NOT', 'REQUIRED', 'SHALL', 'SHALL NOT', 'SHOULD', 'SHOULD NOT', 'RECOMMENDED', 'MAY', and 'OPTIONAL' in this document are to be interpreted as described in [RFC-2119](https://tools.ietf.org/html/rfc2119).

## Terms used

* __Federation__: The federation are all academic services that are connected to the edu-ID Service.

* __edu-ID Service__: The edu-ID Service is the authorisation endpoint for the federation of academic services. There is one edu-ID Service for the entire federation.

* __edu-ID Mobile App__: The edu-ID Mobile App provides a trusted environment on the users' mobile device. The edu-ID App is a navtive mobile app that is installed on a user's mobile device.

* __Third Party App__: A thrid party app refers to mobile apps on the users' devices that are not tightly integrated with a service infrastructure and/or is provided by a potentially untrusted party outside of the truested federation domain.

* __Federation Service__: A federation service is system that offers a set of functions to authorised users within the federation of academic institutions.

* __Service Endpoint__: A service endpoint refers to a URI that exposes a specific service function.

* __Protocol Endpoint__: A protocol endpoint combines several service endpoints that are used to achieve the functionality of a certain protocol.

* __User Credentials__: The user credentials are used to identify a user against the edu-ID Service. Typically they include username and password, but can also include other types of information.

* __Token__: A token is a string of alpha-numeric characters that is used to authorise a request to a service endpoint.

  * __Request token__: A request token is a single use token that is used during the authorisation process. Request tokens are always JWT tokens.

  * __Access token__: An access token is a token that identifies a series of requests to service endpoints. Access tokens are either MAC tokens or JWT tokens, depending on the protocol endpoint requirements

* __Trust Domain__: A trust domain defines an environment of shared trust between applications and/or services, in which data can reliably exchanged.

## References

* [RFC-6749: The OAuth 2.0 Autorization Framework](https://tools.ietf.org/html/rfc6749)
* [RFC-7521: Assertion Framework for OAuth 2.0 Client Authentication and Authorization Grants](https://tools.ietf.org/html/rfc7521)
* [RFC-2617: The Basic HTTP Authorization Scheme](https://tools.ietf.org/html/rfc7617)
* [RFC-7523: JSON Web-Tokens (JWT)](https://tools.ietf.org/html/rfc7523)
* [RFC-7797: JSON Web Signature](https://tools.ietf.org/html/rfc7797)
* [RFC-7800: Proof-of-Possession Key for JWT](https://tools.ietf.org/html/rfc7800)
* [OAuth 2.0 Message Authentication Code (MAC) Tokens (IETF RFC Draft)](https://tools.ietf.org/html/draft-ietf-oauth-v2-http-mac-05)
* [Swiss edu-ID Architecture](https://projects.switch.ch/export/sites/projects/edu-ID/.galleries/documents/SwissEduIDArchitecture_Rev1.pdf)
* [RSD2 service discovery](https://github.com/BLC-HTWChur/rsd2-specification/blob/master/rsd2-specification.md)


