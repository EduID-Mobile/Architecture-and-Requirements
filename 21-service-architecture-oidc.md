# Service Architecture for OpenID Connect

The edu-ID Mobile App provides an authorization and authentication interface for securely connecting third party apps with services in the trust domain of Swiss academic services. The edu-ID Mobile App ensures that commercial and non-commercial third party apps can provide added value services based on the existing service infrastructure in Swiss higher education institutions. The edu-ID App's key function is to authorise third party apps on a user's device with academic services within the edu-ID federation. It helps to bridge the user/app store trust domain on the user devices and the trust domain within the Swiss Academic Service Federation.

The edu-ID Mobile App integrates interactions between the edu-ID infrastructure, services in the academic edu-ID federation, and third-party apps installed on the users' devices. It limits service and data exposure to third-parties to authorised sources.

This document describes the details for connecting with OpenID Connect (OIDC) authorization instances. Due to the specification of OpenID Connect, this document describes a slightly different interactions than the original architecture.

## Interplay of the components.

The EduID Mobile App Architecture implements a cascaded authorization process connecting Academic Services to the OIDC Authorization service of the EduID System.

The academic service acts as a Resource Provider from the perspective of the EduID Authorization Service as defined by [OIDC Core 1.0](http://openid.net/specs/openid-connect-core-1_0.html).

The academic service itself provides the required token service endpoint as required by [OIDC Core 1.0](http://openid.net/specs/openid-connect-core-1_0.html). Additionally, the token endpoint MUST allow JWT assertions, so the EduID Mobile App can initiate the OIDC process at the Academic Service. After successful authorization the EduID Mobile App acts as a [Token Agent](http://openid.bitbucket.org/draft-native-application-agent-core-01.html) to the acadmic service. In this role the EduID Mobile App uses the OAuth2 authorization codes to the client as defined in the [OIDC Native Application Agent Core 1.0](http://openid.bitbucket.org/draft-native-application-agent-core-01.html).

## Flow Support

The academic service MUST use an [OIDC Authorization Flow](http://openid.net/specs/openid-connect-core-1_0.html) to obtain the client authorization from the authorization service.

If the EduID Mobile App requests authorization, the authentication request MUST contain the ```prompt```-claim with the value 'none'.

After the Authorization has completed, the EduID Authorization Service is no longer accessed. The EduID Mobile App will use its token to request sub tokens via OAuth2 JWT Bearer assertions.

## Initiating the Authorization

The EduID Mobile App initiates the Authorization through a self-assigned client assertion. The client assertion is targeted for the academic service using a code and an id_token for the academic service. The id_token holds a JWS. The JWS is signed and the provided ```kid``` header is set to ```urn:oidc:CLAIM```, where claim can be of the value ```email```, ```profile```, ```address```, or ```phone```. In this case the academic service generates the shared key through concatenation of the string values provided the scope claims in the order listed in [OIDC Core 1.0, Section 5.4](http://openid.net/specs/openid-connect-core-1_0.html#ScopeClaims).

The Academic Service MUST trigger all requests through redirects as a response to the incoming assertion request. If the EduID Service and the Academic Service accept the request for the provided ```sub```-claim, the Academic service MUST create a response according to [OAuth2 section 4.1.4](https://tools.ietf.org/html/rfc6749#section-4.1.4).

The EduID Mobile App will include a JWK signing key into the assertion. This JWK key is used for signing assertions for sub token requests. The academic service MUST store this JWK for validating sub-request issues.

### Identifying the authorizing party

The EduID Mobile App will send an ```azp```-claim as part of the assertion. The azp holds the link to the authorizing party's OAuth2 root.

### sub-claim validation

The JWS always contains a ```sub```claim. This claim MUST match the sub claim in the id_token presented to the
academic service as result of the Authorization Code Flow by the EduID Authorization Service.

The academic service MUST accept a request only if the ```sub```-claims of the EduID Authorization Service and the EduID Mobile App matches and the JWS is validated with the respective id_token claims.

### Issuer Validation

If the ```azp```-claim is present in the id_token presented by the EduID Authorization Service it MUST match the ```iss```-claim of the assertion's JWS. If the ```azp-claim``` is not present in the ```id_token```, then the academic service MUST NOT validate the assertion ```iss```-claim

## Issuing the primary access_token and id_token

The primary access_token is issued to the EduID Mobile App through the EduID Authorization Service. Therefore, the academic service MUST pass the access_token and the refresh_token to the EduID Mobile App.

The use of a separate sign key for secondary tokens is RECOMMENDED, if the academic service uses an independent session management and not OAuth2 Tokens.

The academic serivce MUST encrypt the JWK if the EduID Mobile App's assertion contains an JWK encryption key.

## Issuing the secondary access_token

The EduID Mobile App will use [JWT assertions](https://tools.ietf.org/html/rfc7523) for issuing sub-tokens for third party apps. The secondary token assertion will be signed using the JWK provided in the EduID Mobile App's initial assertion. The assertion request will contain a client authorization using the Authorization header.

The academic service MAY initiate a normal authorization request as with the primary key from the EduID Authorization service or the authorization service associated with the primary key. In this case the academic service MUST verify that the ```sub```-claim of the Authorization response matches the primary token.

The academic service MAY immediately accept the assertion and issue its own authorization token to the EduID Mobile App. This is the RECOMMENDED behavior if the academic service implements its own session and access management.

The sub token is tied to the EduID Mobile App's primary token in both cases. If the third party app uses its refresh token, then this token MUST be tied again to the EduID Mobile App's primary token.

## Secondary Token revocation

The EduID Mobile App MAY revoke the secodary token. It may use the initial secodary access or refresh token. The EduID Mobile App will revoke the initial secondary token for a third party.

## Refreshing Tokens

The academic service SHOULD handle refresh tokens for third party apps independently.

If refresh_tokens are used, the refresh token will get consumed immediately. MUST refresh the tokens using the routines for issuing the initial token pair.

Third party apps MAY refresh their tokens without the EduID Mobile App.

The academic service MUST keep the initial secondary access_token and refresh_token that were sent to the EduID Mobile App.
