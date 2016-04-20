# Swiss Edu-ID Mobile App Integration Framework

The key words 'MUST', 'MUST NOT', 'REQUIRED', 'SHALL', 'SHALL NOT', 'SHOULD', 'SHOULD NOT', 'RECOMMENDED', 'MAY', and 'OPTIONAL' in this document are to be interpreted as described in [RFC2119](https://tools.ietf.org/html/rfc2119).

## Terminology

* __EduID Service__: The EduID Service is the authorisation endpoint for the federation of academic services. There is one EduID Service for the entire federation.

* __EduID App__: The EduID App provides a trusted environment on the users' mobile device. The EduID App is a navtive mobile app that is installed on a user's mobile device.

* __Third Party App__: A thrid party app refers to mobile apps on the users' devices that are not tightly integrated with a service infrastructure.

* __Academic Service__: An academic service is system that offers a set of functions to authorised users within the federation of academic institutions.

* __Service Endpoint__: A service endpoint refers to a URI that provides access to specific service function.

* __Protocol Endpoint__: A protocol endpoint combines several service endpoints that are used to achieve the functionality of a certain protocol.

* __Federation__: The federation are all academic services that are connected to the EduID Service.

* __User Credentials__: The user credentials are used to identify a user against the EduID Service. Typically they include username and password, but can also include other types of information.

* __Token__: A token is a string of alpha-numeric characters that is used to authorise a request to a service endpoint.

  * __request token__: A request token is a single use token that is used during the authorisation process.

  * __access token__: An access token is a token that identifies a series of requests to service endpoints.


## Academic Service - EduID App Interaction

The Edu-ID App is the key function to authorise third party apps on a user's device with the academic services of the EduID federation. The EduID app will authenticate itself via a service grant token that is issued by the EduID service for one academic service in the federation. If provided to an academic service, the academic service can validate the token with the EduID service. A service grant token is bound to one academic service and can be validated only by that service.

If a service grant token has been used by an academic service, the EduID service will immediately invalidate the service grant token and present the user information to the academic service. The EduID Service will track, which EduID Apps have requested access to which services for allowing users to invalidate their devices within the federation.

On the grounds of the user information an academic service can authorise a user's EduID app via an eduid access token. The service access token is unique for each EduID app instance. Academic services MAY additionally associate a time-to-live (TTL) interval to the eduid access token.

The eduid access token authorises a user on a given device. The eduid access token is a shared secret between the EduID App and the Academic Service. The eduid access token MUST ONLY be used with the app authorisation protocol endpoint of the service.

The EduID App uses the eduid access token to authorise a third party app for one or more protocol endpoints of an academic service. In this process the EduID will present its access token, the App ID, and the requested protocols to the academic service. The academic service MAY reject to authorise certain App ID-protocol combinations.

If a protocol request by the EduID App is granted, the academic service will send an app access token to the EduID App. The EduID App will provide this app access token to the requesting third party app. This app token is unique for the requested App ID and service protocols. Academic services MAY additionally associate a TTL interval to the app access token that can be different to the TTL for the related eduid access token.

App access tokens are used as shared secrets between the academic service and a third party app. The EduID App MUST NOT use app access tokens unless it requests to invalidate them.

As long an app access token is valid a third party app may use the app access token to access the authorised protocol endpoints on behalf of the user.

If an academic service invalidates an app access token the thrid party app MUST receive an error message that indicate that the app needs to re-authorise.

If an academic service invalidates an eduid access token, it MUST invalidate all app access tokens that were granted using the eduid access token.

A user or the EduID app MAY request to invalidate the eduid access token for a given device. In this case all related app access tokens related to this eduid access token MUST be also invalidated.

The EduID Service MAY request the academic services to invalidate certain devices. In this case the eduid access token and the related app tokens MUST be invalidated.

## References

URI
