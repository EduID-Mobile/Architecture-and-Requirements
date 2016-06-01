# Swiss Edu-ID Mobile App Request Tokens

The edu-ID Mobile App provides an authorization and authentication interface for securely connecting third party apps with services in the trust domain of Swiss academic services. The edu-ID Mobile App ensures that commercial and non-commercial third party apps can provide added value services based on the existing service infrastructure in Swiss higher education institutions. The edu-ID App's key function is to authorise third party apps on a user's device with academic services within the edu-ID federation. It helps to bridge the user/app store trust domain on the user devices and the trust domain within the Swiss Academic Service Federation.

The edu-ID Mobile App integrates interactions between the edu-ID infrastructure, services in the academic edu-ID federation, and third-party apps installed on the users' devices. It limits service and data exposure to third-parties to authorised sources.

This document defines the Service Request Token content and handling.

## Introduction

In order to grant access to Third Party Apps the edu-ID Mobile App needs to authorize the active user with the intended Federation Service. The edu-ID Service provides a unique service grant token to the mobile app. This token can be automatically verified by a Federation Service without additional verification with the edu-ID Service. Token verification with the edu-ID Service is OPTIONAL and can be used to validate a service grant token if it deems necessary for the Federation Service.

The service grant token is always issued to one client for the use with one Federation Service. The edu-ID Mobile App MUST NOT alter the service grant token. If a Federation Service accepts a service grant token, the service MUST remember the service grant token's ID passed in the JTI field. If an already used service grant token is used again, then the Federation Serice MUST invalidate all tokens issued on the ground of this token.

## JOSE Header

The JOSE header provides the [JWS](https://tools.ietf.org/html/rfc7515) requirements for token verification. The means of token verification configured bilaterally between the edu-ID Service and the Federation service.

### algorithm (alg)

The edu-ID Service MAY use any algorithm defined for [JWS](https://tools.ietf.org/html/rfc7515) and for [JWA](https://tools.ietf.org/html/rfc7518). There might be a special algorithm specified between the edu-ID Service and a Federation Service. In this case a Federation Service MUST NOT accept a different algorithm.

## Grant Token

The grant token follows the [JSON Web Token specification](https://tools.ietf.org/html/rfc7519).

The grant token MUST contain the following claims.

* issuer (iss)
* subject (sub)
* audience (aud)
* app id (azp)
* the issue time (iat)
* expires after (exp)
* JSON Token ID (jti)

The following claims might not be used.

* not before (nbf)

### Issuer (iss)

The issuer MUST match the edu-ID Service URL (```https://eduid.ch```).

### subject (sub)

The subject MUST contain the unique identifier for the user within the edu-ID Federation.

### audience (aud)

The audience MUST match the Federation Service's homepage link as specified in the RSD discovery.

### app id (azp)

The authorized party MUST refer to the edu-ID Mobile App Identifier (not client specific). A Federation Service MUST NOT accept tokens that are issued for unofficial clients. The edu-ID Service MUST set this claim based on the client authorization and MUST NOT rely on external information.

### issued at time (iat)

The edu-ID Service MUST set the issue at time. The issue time is provided as a timestamp representing the number of seconds from 1970-01-01T0:0:0Z as measured in UTC until the date/time. A Federation Service MUST NOT accept service grant tokens that are provided before the issued at time. 

### expires at (exp)

The edu-ID Service MUST set an expiration time. The expiration time is provided as a timestamp representing the number of seconds from 1970-01-01T0:0:0Z as measured in UTC until the date/time. A Federation Service MUST NOT accept service grant tokens that are provided after the expiration time.

### JSON Token ID (jti)

The token ID is a unique identifier for the token. The edu-ID Service MAY use the token ID to revoke all related tokens for a given device.

**Note** The jti claim is part of the JWT specification but __not__ mentioned in the OpenID Connect specifications

### Additional Claims

The additional claims in the service grant token identify the user who requests access to the service. The additional claims are used as by [OpenID Connect](http://openid.net/specs/openid-connect-core-1_0.html#StandardClaims). The minimum REQUIRED for additional claims are as following:

* name
* given_name
* family_name
* email

## Signature

The signature MUST verify against the edu-ID Serivce's key that has been issued to the service. If the toke has been issued to a different service or client, then the signature should not be validatable by a different Federation Service.

## References

* [JSON Web Algorithms (JWA)](https://tools.ietf.org/html/rfc7518)
* [JSON Web Signature (JWS)](https://tools.ietf.org/html/rfc7515)
* [JSON Web Token (JWT)](https://tools.ietf.org/html/rfc7519)
* [OpenID Connect Core 1.0](http://openid.net/specs/openid-connect-core-1_0.html)
