# Service Architecture for OpenID Connect

The edu-ID Mobile App provides an authorization and authentication interface for securely connecting third party apps with services in the trust domain of Swiss academic services. The edu-ID Mobile App ensures that commercial and non-commercial third party apps can provide added value services based on the existing service infrastructure in Swiss higher education institutions. The edu-ID App's key function is to authorise third party apps on a user's device with academic services within the edu-ID federation. It helps to bridge the user/app store trust domain on the user devices and the trust domain within the Swiss Academic Service Federation.

The edu-ID Mobile App integrates interactions between the edu-ID infrastructure, services in the academic edu-ID federation, and third-party apps installed on the users' devices. It limits service and data exposure to third-parties to authorised sources.

This document describes the details for connecting with OpenID Connect (OIDC) authorization instances. Due to the specification of OpenID Connect, this document describes a slightly different interactions than the original architecture.

## Introduction

The edu-ID infrastructure will rely on an OpenID Connect authorization services at its core. OpenID Connect builds on top of OAuth2, while focusing only on the ```code``` and ```implicit``` grants of OAuth2. However, OAuth2 also specifies additional flows, such as [client credentials](https://tools.ietf.org/html/rfc6749#section-4.4) and [resource-owner password](https://tools.ietf.org/html/rfc6749#section-4.3) grants. These alternative grant types allow tighter integration of trusted components.

Within the edu-ID ecosystem a mobile app may not necessarily be tightly integrated with one specific resource provider, but has to be directed to the appropriate resource providers depending on the resource owner's choice. The edu-ID mobile app acts as a Trust- and Token Agent to mobile apps that want to access resource providers of the edu-ID ecosystem. Moreover, the edu-ID Mobile App allows resource owners to choose from appropriate resource providers, without exposing access URLs to a requesting app on the device.

The objective of this document is to specify trust agent support with minimal change requirements for OpenID Connect capable resource providers. It adds additional requirements for authorization provider.

## Background

<<<<<<< HEAD
The academic service MUST use an [OIDC Authorization Flow](http://openid.net/specs/openid-connect-core-1_0.html) to obtain the client authorization from the authorization service.
=======
Within the OIDC ecosystem, the [native application agent specification](http://openid.bitbucket.org/draft-native-application-agent-core-01.htm) has been drafted. This draft never reached completion and appears unmaintained. This draft specification assumes that any native app on a device knows about potential resource provider. This assumption is not necessarily valid for a service domain, such as national academia. In such a service domain a service MAY exist more than once for different user audiences.
>>>>>>> eb70175c6d7ee245068d0fc567b1b2c4ef6c542c

Other standardization attempts to connect native mobile apps assume that a native app connects to an authorization service through a browser proxy.  This has been identified as a [security risk](https://tools.ietf.org/html/rfc7636). This risk can only be mitigated by adding new cryptographic means to the OAuth2 protocol. However, this cannot fully solve the problem of using web-browsers and custom URLs as proxy services.

## Objective

The edu-ID Mobile App aims for a tighter integration with the authrization service by removing the need of a mediating web-browser. In this setting the edu-ID Mobile App is a Token and Trust Agent that acts as a confidential client to the authorization service. This removes the security risk of  authorization code interceptions and the risk of exposing institutional affiliations to third parties before authorization has been granted.

<<<<<<< HEAD
The EduID Mobile App initiates the Authorization through a self-assigned client assertion. The client assertion is targeted for the academic service using a code and an id_token for the academic service. The id_token holds a JWS. The JWS is signed and the provided ```kid``` header is set to ```urn:oidc:CLAIM```, where claim can be of the value ```email```, ```profile```, ```address```, or ```phone```. In this case the academic service generates the shared key through concatenation of the string values provided the scope claims in the order listed in [OIDC Core 1.0, Section 5.4](http://openid.net/specs/openid-connect-core-1_0.html#ScopeClaims).
=======
## OIDC Architecture Overview
>>>>>>> eb70175c6d7ee245068d0fc567b1b2c4ef6c542c

The edu-ID Mobile App architecture builds on top of the [Assertion Framework for OAuth2](https://tools.ietf.org/html/rfc7521) and [Proof-of-Possession Key Semantics for JSON Web Tokens (JWTs)](https://tools.ietf.org/html/rfc7800). Within this framework the authorization service can assume that the requests issued by the token-agent are confirmed by the user and no further interaction is required.

The app authentication has 9 steps.

1. The token-agent authorization for a resource owner with the authorization service
2. The issuing of a primary token for the token agent
3. The access request of an app on the device
4. An assertion code request to the resource provider using its client url.
5. An assertion grant request to the authrozation service's token endpoint.
6. An OpenID Connect id_token and token response from the token endpoint.
7. Access confirmation by the resource provider to the token-agent.
8. The token-agent passes the access tokens to the requesting app.
9. The app makes API calls using the access token.


```
+----------------+
|                |
|     Device     |
|                |
| +------------+ |                                        +-------------+
| |            | |                                        |             |
| |            |>----(1) Login + Client-Authorization---->|             |
| |            | |                  rfc7521               |     AP      |
| |            | |                                        |             |
| |            |<--------------(2) Agent Token-----------<|             |
| |            | |                                        |             |
| |    Token   | |                                        +---+-----+---+
| |    Agent   | |                                            ^     V
| |            | |                                   rfc7521  |     |
| |            | |                                   rfc7523 (5)   (6) id_token
| |            | |                                            |     |  token
| |            | |                                            ^     V
| |            | |                                        +---+---------+
| |            | |                                        |  /          |
| |            |>------------(4) Assertion Code---------->|-+           |
| |            | |            rfc7521 + rfc7523           |             |
| |            | |                                        |             |
| |            |<--------------(7) App Token-------------<|             |
| |            | |                                        |             |
| +------------+ |                                        |             |
|     ^    V     |                                        |             |
|     |    |     |                                        |             |
|    (3)  (8)    |                                        |     RP      |
|     |    |     |                                        |             |
|     ^    V     |                                        |             |
| +------------+ |                                        |             |
| |            | |                                        |             |
| |            |>---------(9) API call with token-------->|             |
| |    App     | |                                        |             |
| |            | |                                        |             |
| |            |<----------(9) Operation Response--------<|             |
| |            | |                                        |             |
| +------------+ |                                        +-------------+
|                |
+----------------+
```
Figure 1: App authorization using a token agent authorization

## Terminology

* Token Agent (TA) - native app on a device that can request authorizations for other apps on the device or within the application context in a device network.

* Authorization service (AP) - OpenID Connect authorization service/ authorization provider.

* Resource provider (RP) - OpenID Connect party that uses one or more AP for authorization.

* Resource owner (RO) - Agent, who uses the AP for authorization, typically indicating the human end-user.

<<<<<<< HEAD
The primary access_token is issued to the EduID Mobile App through the EduID Authorization Service. Therefore, the academic service MUST pass the access_token and the refresh_token to the EduID Mobile App.
=======
* App - Native app on the same device or within the same application context as the TA.
>>>>>>> eb70175c6d7ee245068d0fc567b1b2c4ef6c542c

## Token-agent authorization for a resource owner

The TA authorizes with the AP using the [OAuth2 resource owner password flow](https://tools.ietf.org/html/rfc6749#section-4.3) to the authorization service's token endpoint.

The TA authorizes itself using a [client-authentication assertion](https://tools.ietf.org/html/rfc7521#section-4.2).

1. The client assertion MUST signed using a shared secret between the TA and the AP. The shared secret identifies a group of TAs (e.g., platform and version). The exchange of this shared secret between the AP and the TA is out of scope for this document.

<<<<<<< HEAD
The academic service MAY initiate a normal authorization request as with the primary key from the EduID Authorization service or the authorization service associated with the primary key. In this case the academic service MUST verify that the ```sub```-claim of the Authorization response matches the primary token.
=======
2. The client assertion MUST use the ```iss``` claim to identify the device TA group. The MUST match the signature.
>>>>>>> eb70175c6d7ee245068d0fc567b1b2c4ef6c542c

3. The client assertion MUST include a ```sub``` claim, that identifies the TA's instance. The ```sub``` claim MUST include a globaly unique identifier for the TA instance. This is typically the device id as provided by the device's operating system.

4. The client assertion MUST include a ```cnf``` claim that includes a key for signing the token requests for the RO. The format of the ```cnf``` claim as specified by the [Proof-of-Possession Key Semantics](https://tools.ietf.org/html/rfc7800).

5. A TA MUST NOT reuse keys for different RO using the same TA.

6. The AP MUST not prompt the user for further authentication if a client authorization is presented.

7. The AP MUST report authorization errors to the TA if the client authorization failed.

8. An AP MAY require password encryption as defined in [Section Password Encryption](#password-encryption), below.

### Successful response

A successful response includes an ```access_token``` and a ```refresh_token``` for the TA (2) as specified for [OAuth2 Section 4.3.3](https://tools.ietf.org/html/rfc6749#section-4.3.3).

<<<<<<< HEAD
The academic service MUST keep the initial secondary access_token and refresh_token that were sent to the EduID Mobile App.
=======
1. The AP MAY include an ```id_token```.

2. The ```access_token``` MUST be valid for the AP's endpoints that require user-level authorization.

### Error response

The error response is specified in [OAuth2 Section 5.2](https://tools.ietf.org/html/rfc6749#section-5.2).

## Access request of an app

An App forwards an access request to the TA through the device's operating system's means. These requests are defined by the [edu-ID NAIL API](40-nail-api.md). Any access request must include scope information that can be used to identify suitable services that can respond to the requirements of the requesting app.

1. An App MUST identify itself in the request.

2. The TA MUST NOT respond to a request before it completed all [authorization request(s) (8)](#authorization-request).

### Successful response

An successful response includes the authorization token as provided by the RP (7).

### Error response

1. The TA MUST NOT include reasons for authorization failures to the requesting App. An error is indicated by not providing any token information to the requesting App.

2. The requesting App MUST NOT make any assumptions why the authorization has failed.

3. The requesting App MUST remain in an unauthorized state if no token information has been provided.

## Assertion code request to the resource provider

For requesting Apps, the TA SHOULD select one or more suitable RP for authorization. This can be either automatic or through user interaction. For each RP, the TA generates an authorization assertion on behalf of the requesting App.

### Assertion format

1. The assertion MUST be provided as [JWT](https://tools.ietf.org/html/rfc7519).

2. The assertion MUST include an ```iss``` claim that points to the TA's instance as presented in (1) to the AP.

3. The assertion MUST include an ```aud``` claim that points to the AP's token endpoint.

4. The assertion MUST include an OIDC ```azp``` claim that includes the client_id of the RP.

5. The assertion MUST include a ```sub``` claim that identifies the requesting App.

6. The assertion MUST include an ```iat``` claim as defined for JWT.

7. The assertion MUST include an ```exp``` claim as defined for JWT.

8. The assertion MUST be signed using the key presented in the ```cnf``` claim during the [TA authorization(1)](#token-agent-authorization-for-a-resource owner).

9. The TA MAY include additional claims for the AP into the assertion.

### Authorization request

The TA sends an authorization request to the RP's ```redirect_uri``` (4). The ```redirect_uri``` is the same as it registered for the RP at the AP.

1. All parameters MUST get encoded as form parameters into the request url.

2. The request MUST include a ```aud``` parameter to points to the AP's token endpoint.

3. A RP SHOULD verify that it is a registered client for the AP indicated in the ```aud``` parameter.

4. The request MUST include an ```assertion``` parameter containing the assertion token.

5. The request MUST include an ```grant_type``` prameter containing the grant_type for the assertion parameter as defined in [JSON Web Token (JWT) Profile for OAuth 2, Section 2.1](https://tools.ietf.org/html/rfc7523#section-2.1).

6. The RP MUST forward all request parameters to the AP in the ```POST```request.

7. The RP MUST NOT alter the parameters when forwarding them to the AP.

8. The RP MUST add a ```scope``` parameter into the request to the AP.

9. The ```scope``` parameter MUST follow [OpenID Connect Core, Section 5.2](http://openid.net/specs/openid-connect-core-1_0.html#ScopeClaims).

#### Successful response

Successful responses are handled as the [OpenID Connect Core, Section 3.1.3.3](http://openid.net/specs/openid-connect-core-1_0.html#TokenResponse) (7).

#### Error response

The RP MAY use HTTP Status codes in case of errors before forwarding the assertion to the AP. Otherwise, error responses are handled as the [OpenID Connect Core, Section 3.1.3.4](http://openid.net/specs/openid-connect-core-1_0.html#TokenErrorResponse).

## Assertion grant request to the authrozation service

The RP POSTs the authorization assertion to the AP's token endpoint (5). For the resource provider this is an alternate call to the regular token endpoint call in the [OpenID Connect Core, Section 3.1.3.1](http://openid.net/specs/openid-connect-core-1_0.html#TokenRequest).

1. The token response is identical to [OpenID Connect Core, Section 3.1.3.3]()

2. The AP MUST verify the assertion's signature.

3. The AP MUST select the authorized user based on the signature used for signing the assertion as provided in (1).

4. The AP MUST validate the assertion's ```azp``` and the provided ```redirect_uri``` of the provided client_id.

5. The RP MUST validate the ```id_token``` and ```access_token``` as required as of [OpenID Connect Core, Section 3.1.3.7](http://openid.net/specs/openid-connect-core-1_0.html#IDTokenValidation).

6. The RP MUST forward all but the ```id_token``` to the TA as in the code flow (7).

7. A RP MUST indicate an error to the TA, if it rejects the assertion based on its internal usage policies.

8. The TA MUST forward the token information to the requesting App (8) as a response for the access request (3).

9. The TA MUST include ALL obtained tokens into the response to the requesting App (8), if it obtained for more than one RP in the process of one access request (3).

10. The TA MUST include access URLs for the requesting App's API calls (9) in the response (8) for an access request (3).

## Security considerations

### Client assertion encryption

As the client assertion contains a shared key for communication between the authorization service and the TA, this key might be intercepted though a man-in-the-middle attack.

It is RECOMMENDED to encrypt the client assertion for the AP and provide it as JWE.

### App assertion encryption

Although the app assertion does not contain security relevant information, a TA MAY use it to pass implementation specific parameters to the AP.

It is RECOMMENDED to encrypt the app assertion and provide it as JWE.

### Password Encryption

APs MAY require TAs to use password encryption for authenticating ROs. If an AP requires password encryption, the TA MUST format the token request as following:

1. The password MUST contain a [JWE](https://tools.ietf.org/html/rfc7516) as password token.

2. The password token MUST be encrypted for the authorization service following the encryption requirements of the authorization service.

3. The password token's payload MUST contain a JWS as user token.

4. The user token MUST be signed by the token agent using the same key as the client assertion.

4. The user token's ```sub``` claim MUST match the username of the request.

5. The user token MUST contain a ```password``` claim.

6. The user token's ```password``` claim MUST contain the password as provided by the resource owner.
>>>>>>> eb70175c6d7ee245068d0fc567b1b2c4ef6c542c
