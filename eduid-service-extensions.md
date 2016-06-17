# Swiss Edu-ID Service Extensions

This Document describes the service extensions for the edu-ID Federation Service.

## Conventions

### Notational Conventions

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document  are to be interpreted as described in [RFC2119](https://tools.ietf.org/html/rfc2119).

Certain security-related terms are to be understood in the sense defined in [RFC4949](https://tools.ietf.org/html/rfc4949).  These terms include, but are not limited to, "authentication", "authorization", "certificate", "confidentiality", "credential", "encryption", "identity", "sign", "signature", "trust", "validate", and "verify".

### Protocol

Protocols describe sets of data exchange sequences between two connected but not integrated components. For components that are connected via a network, a protocol refers to the application layer of the OSI Model.

A protocol __handshake__ refers to the steps within a protocol for achieving a functional result.

### Service

A service describes a functional component for storing, accessing, processing, manipulating and deleting data.

Throughout this document the term __service__ is only used in the context of machine-to-machine interaction.

A __web-service__ describes a service that is available over the Internet using HTTP.

### Protocol Endpoints

A protocol endpoint refers to specific functions of a service that are part of the protocol. A protocol may involve protocol endpoints of different and distributed services.

## Swiss EduID Extended Services

The Swiss EduID Architecture provides the 4 core functions.

* __Federation Management__
* __Profile or Attribute Management__
* __Affiliation Management__
* __Authentication__

In order to enable authorised federation users to manage access of third party solutions to their resources, additional __usage management__ is required. Usage management is much like the affiliation management with the core difference that usage management enables federation users to identify their academic service usage.

As OAuth is designed as a service-oriented and distributed architecture, an additional __Service-Authorisation__ is required that allows gaining authorised access to the services within the federation.

In order to enable end-users to select which of their services they want to expose to external parties, such as other apps, it is necessary to track, which services are used by the users. Such __usage management__ extends the affiliation index of the EduID Architecture.

## edu-ID Authentication and Authorization and OAuth2

OAuth2 defines 2 service endpoints for authorization tasks.

* Authorization endpoint
* Token endpoint

The authorzation endpoint focuses on user-agent-based (Web-browser-based) authorization schemes and is not intended for client authorization. This is beyond the scope of the present architecture.

The token endpoint is the client endpoint for authenticating and authorizing clients as well as asserting access to resource services.

While federation services have their own token endpoint, from the view point of the edu-ID service federation services as well as other endpoints of the edu-ID service are "Resource Services".

### Token endpoint

The token endpoint is specified in [OAuth2 Section 4](https://tools.ietf.org/html/rfc6749).

The edu-ID Token endpoint has three main tasks:

1. Authorizing edu-ID Mobile App Instances,
2. Authenticating device owners as federation users, and
3. Asserting service grant tokens for federation services.

The token endpoint responds to the service calls as defined in section 4 of the OAuth2 specification.

The token endpoint responds only to HTTP POST requests. Any request using another HTTP method will receive a "400 Bad Request" error.

The token endpoint issues access tokens. The access tokens contain the field ```mac_algorithm```. Clients MUST use the specified algorithm for signing or encrypting tokens. Possible algorithms are defined in the [JSON Web Algorithms (JWA) specification](https://tools.ietf.org/html/rfc7518).

#### Authorizing edu-ID Mobile App Instances

This section is based on [OAuth 2 Section ]

The edu-ID Mobile App versions are clients for the edu-ID Service. Each client instance needs to receive individual authorization before it can call any other service. For this purpose every client instance MUST authorize itself using its client credentials.

Each client authorizes itself using a unique device id as provided by the platform operating system as well as the global client credentials.

The global client client credentials bind an edu-ID Mobile App version to the edu-ID Trust Domain. The global client credentials are issued for each client version separately and MUST NOT be used with other clients. The client credentials are part of the edu-ID Mobile App packages that are distributed through the vendors App Stores.

The client MUST present the client credentials as a [JWT Request Token](eduid_app_request_token.md) in the request's Authorization header using the Bearer scheme.

The request payload MUST contain the parameter ```grant_type```. The payload SHOULD NOT contain other parameters. Any other parameter presented to the service during app authorization will be ignored.

Example Request:

```
POST /service/eduid.php/token HTTP/1.1
www.eduid.ch
Authorization: Bearer eyJhbGc[...]NCJ9.eyJq[...]J9.232d[...]cb34b
Content-type: application/json

{"grant_type":"client_credentials"}
```

If the edu-ID Service accepts the credentials, then the token endpoint responds the __client access token__ in form of a JSON structure containing 5 items.

* access_token
* token_type
* kid
* mac_key
* mac_algorithm

This client registration is unique for every edu-ID Mobile App instance. The client MUST store this information persistently.

#### Authenticating users

Device owners can authenticate themselves as federation users through their edu-ID Mobile App instance as a client as specified in [OAuth2 section 4.3.2](https://tools.ietf.org/html/rfc6749).

The client MUST authenticate itself through an Authorization header using the __client access token__. The Authorization header MUST use JWT tokens or MAC tokens.

JWT Tokens are the same format as the request grant token, but clients MAY omit the name claim. For signing the clients MUST use the provided key_id and specify the signing algorithm as provided by the client access token.

MAC Tokens are generated as described in [MAC Token RFC draft](https://tools.ietf.org/html/draft-ietf-oauth-v2-http-mac-05). The MAC tokens MUST get signed using the client access token's mac_key using the algorithm provided in the token's mac_algorithm field.

The payload of the request MUST contain the following fields:

* grant_type
* username
* password

The grant_type MUST contain the value ```password```

The password must be presented as provided on the user interface of the edu-ID Mobile App.

__NOTE FOR FUTURE DEVELOPMENT__: The payload might be RSA or DSA encrypted using the public key of the edu-ID Service. The payload must then be presented as JWE Tokens.

Example Request:

```
POST /service/eduid.php/token HTTP/1.1
www.eduid.ch
Authorization: MAC kid=as3.321qas,ts=1465473771,mac=af4[...]d64
Content-type: application/json

{"grant_type":"password","username":"foo@switch.ch","password":"h3llow0rld"}
```

If the password authentication is accepted by the edu-ID Service, the token endpoint responds the __user access token__ in form of a JSON structure containing 5 items.

* access_token
* token_type
* kid
* mac_key
* mac_algorithm

This user access token is unique for every edu-ID Mobile App instance and authenticated user. The client MUST store this information persistently. If a client issues a new user authentication request, the edu-ID Service MUST revoke any active user access token issued to the same client.

#### Service assertion

This section is bassed on [OAuth2 Section 4.1.3 and Section 4.1.4](https://tools.ietf.org/html/rfc6749)

In order to grant access for Federation Services to third party apps the edu-ID Mobile App MUST authorize itself to the individual Federation Service through the edu-ID Service. This process is called assertion and provides all information that enables the Federation service to accept the authorization.

The edu-ID Mobile App must obtain a __service grant token__ from the token endpoint through a "access_code" request.

The client must authorize itself through an Authorization header using the __user access token__. The Authorization header MUST either use JWT tokens or MAC tokens.

JWT Tokens are the same format as the request grant token, but clients MAY omit the name claim. For signing the clients MUST use the provided key_id and specify the signing algorithm as provided by the client access token.

MAC Tokens are generated as described in [MAC Token RFC draft](https://tools.ietf.org/html/draft-ietf-oauth-v2-http-mac-05). The MAC tokens MUST get signed using the client access token's mac_key using the algorithm provided in the token's mac_algorithm field.

The payload MUST contain the following parameters:

* grant_type
* redirect_uri
* client_id
* code

The grant_type MUST contain the value ```authorization_code```

The code MUST match the user access token' access_token.

The client_id MUST match the edu-ID mobile app's client identifier (as provided in the client_credentials phase).

The request_uri MUST match an OAuth2 token endpoint or the home page address of a connected service.

If the edu-ID Service accepts the assertion request, then it will respond a JSON data structure with an access token, a final redirect uri and a token type. In case of successfull assertion the access token will be a JWT signed for the target Federation service. The token type will always be ```urn:ietf:oauth:param:jwt-bearer```.

The client MUST NOT alter the provided access token.

The client MUST use the provided access token for sending a client_credentials request to the Federation service's token end point. The Federation token endpoint is provided in the redirect_uri field of the response. The client MUST use the redirect URI provided by the edu-ID Service.

__Note__: The additional ```redirect_uri``` diverts from [OAuth2 Section 4.1.4](https://tools.ietf.org/html/rfc6749) in order to allow more flexible service assertion. For OAuth2 compliant clients the ```redirect_uri``` in the response is equal to the requested ```redirect_uri```.

Example request:

```
POST /service/eduid.php/token HTTP/1.1
www.eduid.ch
Authorization: MAC kid=as3.321qas,ts=1465473771,mac=af4[...]d64
Content-type: application/json

{"grant_type":"authorization_code","redirect_uri":"https://moodle.htwchur.ch","code":"d4f21as[....]d.Bd2","client_id":"ch.eduid.app.ios.20160609"}
```

Example response:

```
HTTP/1.1 200 OK
Content-type: application/json

{"access_token":"eyJhbGc[...]NCJ9.eyJq[...]J9.22ac[...]a445","token_type":"urn:ietf:oauth:param:jwt-bearer", "redirect_uri":"https://moodle.htwchur.ch/local/powertla/rest.php/identity/auth/token"}
```

### Non-standard extension to the token endpoint

#### Token Validation

Endpoint: ```token/validate```

Federation Services MAY validate a service grant token by its token index as provided in the ```jti``` claim of the JWT presentd by the client.

The Federation Service MUST authorize using a JWT or MAC token based on a private service token.

The response will include the following claims as presented in the JWT.

* sub
* azp
* iat
* email

The claims MUST match the claims presented in the token.

The edu-ID Service MUST NOT verify token ids that were not issued to the authorized Federation Service.

Example request:

```
POST /service/eduid.php/token/validate HTTP/1.1
www.eduid.ch
Authorization: MAC kid=B_sk.5Sxas,ts=1465474674,mac=c4a[...]3eb
Content-type: application/json

{"jti":"ks-4.eXq56"}
```

Example response:

```
HTTP/1.1 200 OK
Content-type: application/json

{"sub":"cad1232141a234214141defb134","azp":"ch.eduid.app.android.20160609","iat":"1465473874","email":"foo@switch.ch"}
```

If the token id can not be validated for the requesting service the token endpoint will respond a "404 Not Found" error.

## Service Discovery

The service discovery provide the means to identify services within the federation.

It is available only to authorized users at the edu-ID Service.
The service-discovery endpoint has 3 modes:

* search mode (endpoint ```service-discovery```)
* user mode (endpoint ```service-discovery/user```)
* federation mode (endpoint ```service-discovery/federation```)

### Search mode

Endpoint: ```service-discovery```

In search mode the service accepts an URL parameter. This URL MUST point to the homePageLink that is presented in the Federation Service's URL. In search mode the service discovery will return service information about a service matching the search URL regardless whether the authorized user used the service already.

In order to search the federation services a client must POST a request containing 1 URL. The request MUST be authorized using either JWT or MAC tokens based on the user access token.

The service responds only to authorized users. The response contains four entries:

* name - the display name of the service
* link - the homePageLink of the service (the same as the requested URL)
* token_endpoint - the target link for service authorization.
* info - additional service needed by clients

### User mode

Endpoint: ```service-discovery/user```

In user mode the service returns service information for the requesting user. The service information presented in the response can be used by a client for determinatiting the protocols provided by a service using the protocol discovery.

In order to obtain the user services a client MUST send a GET request. This request MUST be authorized using either a JWT or a MAC token using the user access token.

The service responds only to authorized users. The response contains four entries:

* name - the display name of the service
* link - the homePageLink of the service (the same as the requested URL)
* token_endpoint - the target link for service authorization.
* info - additional service needed by clients

### Federation mode

Endpoint: ```service-discovery/federation```

In federation mode the service-discovery allows adding services to the federation. The federation mode is only available for authorized users.

In federation mode the client MUST use a PUT request to insert the service information into the federation. The service information MUST contain the following attributes.

* service_uuid - a [UUID]() for the service instance.
* name - display name for the service
* mainurl - homepage link of the service
* token_endpoint - authorization endpoint for the edu-ID Mobile App
* rsdurl - the uri to the rsd file that used by the protocol discover)
* info - OPTIONAL client information

On successful integration the service will return a service token with the following attributes.

* access_token
* token_type
* kid
* mac_key
* mac_algorithm

The service token MUST be stored for access by the federation service. This token MUST be used to 2 purposes.

1. Verifying service grant tokens issued to clients for authorization at the Federation service.
2. Access federation services such as for verifying a service grant token that has been presented by a client.

## Protocol Discovery

The protocol-discovery endpoint has two modes.

* Service Mode (endpoint ```protocol-discovery/service```)
* Protocol Mode (endpoint ```protocol-discovery/protocol```)

### Service Mode

Endpoint: ```protocol-discovery/service```

In service mode the protocol discovery expects a list of service URLs. These URLs MUST match the main urls of the services provided by the service discovery. If these services expose any protocols using RSD, then the full RSD document is included into the result set.

Example Request

```
POST /service/eduid.php/protocol-discovery/service HTTP/1.1
www.eduid.ch
Authorization: MAC kid=B_sk.5Sxas,ts=1465474674,mac=c4a[...]3eb
Content-type: application/json

["https://moodle.htwchur.ch","https://toolbox.switch.ch"]
```

### Protocol Mode

Endpoint ```protocol-discovery/protocol```

In protocol mode the client passes a list of protocols that are intended to be used for the same service. The result set will include only those services that match ALL protocol APIs that were presented in the requested list.

Example Request

```
POST /service/eduid.php/protocol-discovery/protocol HTTP/1.1
www.eduid.ch
Authorization: MAC kid=B_sk.5Sxas,ts=1465474674,mac=c4a[...]3eb
Content-type: application/json

["org.moodle.mobile","gov.adlnet.xapi"]
```

NOTE: clients MUST NOT rely that the service RSD exposes the org.ietf.oath protocol. If no oauth protocol is exposed, then the client MUST use the service discovery search for identifying the token_endpoint for authorization.

NOTE: If no RSD is presented for a given URL, then the Federation Service does not expose any public services.


## References

* [RFC-6749: The OAuth 2.0 Autorization Framework](https://tools.ietf.org/html/rfc6749)
* [RFC-7521: Assertion Framework for OAuth 2.0 Client Authentication and Authorization Grants](https://tools.ietf.org/html/rfc7521)
* [RFC-7523: JSON Web-Tokens (JWT)](https://tools.ietf.org/html/rfc7523)
* [RFC-7797: JSON Web Signature](https://tools.ietf.org/html/rfc7797)
* [OAuth 2.0 Message Authentication Code (MAC) Tokens (IETF RFC Draft)](https://tools.ietf.org/html/draft-ietf-oauth-v2-http-mac-05)
