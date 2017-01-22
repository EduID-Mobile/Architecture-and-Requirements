# Service Architecture for OpenID Connect

The edu-ID Mobile App provides an authorization and authentication interface for securely connecting third party apps with services in the trust domain of Swiss academic services. The edu-ID Mobile App ensures that commercial and non-commercial third party apps can provide added value services based on the existing service infrastructure in Swiss higher education institutions. The edu-ID App's key function is to authorise third party apps on a user's device with academic services within the edu-ID federation. It helps to bridge the user/app store trust domain on the user devices and the trust domain within the Swiss Academic Service Federation.

The edu-ID Mobile App integrates interactions between the edu-ID infrastructure, services in the academic edu-ID federation, and third-party apps installed on the users' devices. It limits service and data exposure to third-parties to authorised sources.

This document describes the details for connecting with OpenID Connect (OIDC) authorization instances. Due to the specification of OpenID Connect, this document describes a slightly different interactions than the original architecture.

## Introduction

The edu-ID infrastructure will rely on an OpenID Connect authorization services at its core. OpenID Connect builds on top of OAuth2, while focusing only on the ```code``` and ```implicit``` flows of OAuth2. However, OAuth2 also specifies additional flows, such as ```client credentials``` and ```resource-owner password``` flows. These alternative flows allow a tighter integration of trusted components.

Within the edu-ID ecosystem a mobile app may not necessarily be tightly integrated with one specific resource providers, but has to be directed to the appropriate resource providers depending on the user's choice. The edu-ID mobile app acts as a Trust- and Token Agent to mobile apps that want to access resource providers of the edu-ID ecosystem. Moreover, the edu-ID Mobile App allows users to choose from appropriate resource providers, without exposing access URLs to a requesting app on the device.

The objective of this document is to specify trust agent support with minimal change requirements for OpenID Connect capable resource providers. It adds additional requirements for authorization provider.

## Background

Within the OIDC ecosystem, the [native application agent specification](http://openid.bitbucket.org/draft-native-application-agent-core-01.htm) has been drafted. This draft never reached completion and appears unmaintained. This draft specification assumes that any native app on a device knows about potential resource provider. This assumption is not necessarily valid for a service domain, such as national academia. In such a service domain a service MAY exist more than once for different user-audiences.

Other standardization attempts to connect native mobile apps assume that a native app connects to an authorization service through a browser proxy.  This has been identified as a [security risk](https://tools.ietf.org/html/rfc7636). This risk can only be mitigated by adding new cryptographic means to the OAuth2 protocol. However, this cannot fully solve the problem of using web-browsers as proxy services.

The edu-ID Mobile App aims for a tighter integration with the authrization service by removing the need of a mediating web-browser. In this setting the edu-ID Mobile App is a Token and Trust Agent that acts as a confidential client to the authorization service. This removes the security risk of  authorization code interceptions and the risk of exposing institutional affiliations to third parties before authorization has been granted.

## OIDC Architecture Overview

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
| |            |<-----------(7) App Access Token---------<|             |
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

## Token-agent authorization for a resource owner

The token agent authorizes with the authorization service using the [OAuth2 resource owner password flow](https://tools.ietf.org/html/rfc6749#section-4.3) to the authorization service's token endpoint.

The token agent authorizes itself using a [client-authentication assertion](https://tools.ietf.org/html/rfc7521#section-4.2).

The client assertion MUST signed using a shared secret between the token-agent and the authorization service. The shared secret identifies a group of token agents (e.g., platform and version).

The client assertion MUST use the ```iss``` claim to identify the device token group. The MUST match the signature.

The client assertion MUST include a ```sub``` claim, that identifies the token agent's instance. The ```sub``` claim MUST include a globaly unique identifier for the token-agent instance. This is typically the device id as provided by the device's operating system.

The client assertion MUST include a ```cnf``` claim that includes a key for signing the token requests for the authorized user. A token agent MUST NOT reuse keys for different users using the same token agent. The format of the ```cnf``` claim is defined by the [Proof-of-Possession Key Semantics](https://tools.ietf.org/html/rfc7800).

The authorization service MUST not prompt the user for further authentication if the client authorization assertion is accepted.

### Successful response

A successful response includes an ```access_token``` and a ```refresh_token``` for the token agent (2) as specified for [OAuth2 Section 4.3.3](https://tools.ietf.org/html/rfc6749#section-4.3.3).

The authorization service MAY include an ```id_token```.

The ```access_token``` MUST be valid for the authorization service's endpoints that require user-level authorization.

### Error response

The error response is specified in [OAuth2 Section 5.2](https://tools.ietf.org/html/rfc6749#section-5.2).

## Access request of an app

An app forwards an access request to the token agent through the device's operating system's means. These requests are defined by the [edu-ID NAIL API](40-nail-api.md). Any access request must include scope information that can be used to identify suitable services that can respond to the requirements of the requesting app.

An requesting app MUST identify itself in the request.

The token agent MUST NOT respond to a request before it completed the authorization request.

### Successful response

An successful response includes the authorization token as provided by the resource provider (7).

### Error response

The token agent MUST NOT include reasons for authorization failures to the requesting app.

An error is indicated by not providing any token information to the requesting app.

The requesting app MUST NOT make any assumptions why the authorization has failed.

The requesting app MUST remain in an unauthorized state if no token information has been provided.

## Assertion code request to the resource provider

For requesting apps the token agent will select one or more suitable services for authorization. This can be either automatic or through user interaction. For each service, the token agent generates an authorization assertion on behalf of the requesting app.

### Assertion format

The assertion is a [JWT](https://tools.ietf.org/html/rfc7519).

The assertion MUST include an ```iss``` claim that points to the token agent's instance as presented in (1) to the authorization service.

The assertion MUST include an ```aud``` claim that points to the authorization service token endpoint.

The assertion MUST include an OIDC ```azp``` claim that includes the client_id of the resource provider.

The assertion MUST include a ```sub``` claim that identifies the requesting app.

The assertion MUST include an ```iat``` claim as defined for JWT.

The assertion MUST include an ```exp``` claim as defined for JWT.

The assertion MUST be signed using the key presented in the ```cnf``` claim in (1).

The assertion MAY be ```JWA``` encrypted for the authorization service.

### Authorization request

The token agent sends an authorization request to the resource provider's redirect_uri as registered for the resouce provider at the authorization service (4).

All parameters MUST get encoded as form parameters into the request url.

The request MUST include a ```aud``` parameter to points to the authorization service's endpoint root.

A resource provider MUST verify that it is a registered client at the requested aud value.

The request MUST include an ```assertion``` parameter containing the assertion token.

The request MUST include an ```grant_type``` prameter containing the grant_type for the assertion parameter as defined in [JSON Web Token (JWT) Profile for OAuth 2, Section 2.1](https://tools.ietf.org/html/rfc7523#section-2.1).

The resource provider MUST forward all request parameters but the ```aud``` parameter to the authorization service. It MUST NOT alter these parameters.

The resource provider MUST include a ```scope``` parameter for tailoring the id_token's contents. The ```scope``` parameter MUST follow [OpenID Connect Core, Section 5.2](http://openid.net/specs/openid-connect-core-1_0.html#ScopeClaims).

#### Successful response

Successful responses are handled as the OpenID Connect code flow (7).

#### Error response

The resource provider MAY use HTTP Status codes in case of errors before forwarding the assertion to the authorization service. Otherwise, error responses are handled as the OpenID Connect code flow.

## Assertion grant request to the authrozation service

The resource provider POSTs the authorization assertion to the authorization service's token endpoint (5). For the resource provider this is an alternate call to the regular token endpoint call in the OpenID Connect code flow.

The autorization service MUST verify the assertion's signature.

The authorization service MUST select the authorized user based on the signature used for signing the assertion.

The authorization service MUST generate the ```id_token```-based on the azp claim of the assertion.

The resource provider MUST validate the ```id_token``` and ```access_token``` as required as of [OpenID Connect Core, Section 3.1.3.7](http://openid.net/specs/openid-connect-core-1_0.html#IDTokenValidation).

The resource provider MUST forward all but the ```id_token``` to the token agent as in the code flow (7).

The token agent MUST forward the token information to the requesting app (8) as a response for the access request (3).

If the token agent obtained more than one token as a result to one access request (3), it MUST include ALL obtained tokens into the response.

The token agent MUST include access URLs for the requesting app's API calls (9) in the response (8) for an access request (3).

## Security considerations

All assertions SHOULD be encrypted for the authorization services.
