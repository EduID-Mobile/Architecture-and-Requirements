# Swiss Edu-ID Mobile App Request Tokens

The edu-ID Mobile App provides an authorization and authentication interface for securely connecting third party apps with services in the trust domain of Swiss academic services. The edu-ID Mobile App ensures that commercial and non-commercial third party apps can provide added value services based on the existing service infrastructure in Swiss higher education institutions. The edu-ID App's key function is to authorise third party apps on a user's device with academic services within the edu-ID federation. It helps to bridge the user/app store trust domain on the user devices and the trust domain within the Swiss Academic Service Federation.

The edu-ID Mobile App integrates interactions between the edu-ID infrastructure, services in the academic edu-ID federation, and third-party apps installed on the users' devices. It limits service and data exposure to third-parties to authorised sources.

This document defines the App Request Token structure for authorizing a edu-ID Mobile App Instance in order to obtain a client grant.

## Introduction

The edu-ID Mobile App is installed on the mobile devices on different plattforms. Each installation is called an edu-ID Mobile App Instance. The edu-ID Mobile App Instances act as clients to the edu-ID Service. Before a user authorization has been obtained, each client needs to authorize itself with the edu-ID Service in order to be able to authenticate users. The edu-ID Service MAY reject edu-ID Mobile App versions or plattforms that are no longer supported, compromized or otherwise restricted.

The client authorization uses a signed JWT-Bearer token. This token helps to verify the client's identity. The signature is based on a key and a key id (kid) pair. The key id is used by the client for identifying itself against the service and verify the JWT token as well as its signature. The signature algorithm MAY be predefined and if it is, the edu-ID Service MUST reject all other algorithms for that key.

The edu-ID Mobile App Request Token MUST be presented as Authorization Header during the user authentication request.

The key id and key pair are assigned to an edu-ID Mobile App version prior to release in a way that each version has its version-specific key-id-pair. This key-id-pair is associated to a version identifier.

## JOSE Header

The JOSE header provides the [JWS](https://tools.ietf.org/html/rfc7515) requirements for client identification. The client request token's payload MUST NOT be detached from the signature. All client request tokens MUST be secured.

### key ID (kid)

A client MUST identify itself via a key id (kid). The related key is used to verify the signature. The key ID must be registered with the edu-ID Service, otherwise the JWT MUST get rejected.

### algorithm (alg)

A client MAY use any algorithm defined for [JWS](https://tools.ietf.org/html/rfc7515) and for [JWA](https://tools.ietf.org/html/rfc7518). The edu-ID Service MAY enforce specific algorithms for specific clients. In this case a client MUST NOT use a different algorithm.

## Grant Token

The grant token follows the [JSON Web Token specificaiton](https://tools.ietf.org/html/rfc7519).

The grant token MUST contain the following claims.

* issuer (iss)
* subject (sub)
* audience (aud)
* device name (name)

The grant token MAY contain an issued at (iat) claim.

The grant token SHOULD NOT contain the following claims.

* expires after (exp)
* not before (nbf)
* JSON Token ID (jti)

### Issuer (iss)

The issuer MUST match the edu-ID Mobile App name that maps to the key-id-pair matching the provided version identifier.

### subject (sub)

The subject MUST contain the device/app UUID of the edu-ID Mobile app instance. If the same client requests a new access token if an old token exists for the same UUID, the previous token will be automatically invalidated.

### audience (aud)

The audience MUST match the requested based URL.

### Additional Claims

#### name

The name refers to the client name. This name refers to the device name chosen by the device owner.

## Signature

The signature MUST verify against the stored key for the provided key id.

## References

* [JSON Web Algorithms (JWA)](https://tools.ietf.org/html/rfc7518)
* [JSON Web Signature (JWS)](https://tools.ietf.org/html/rfc7515)
* [JSON Web Token (JWT)](https://tools.ietf.org/html/rfc7519)
