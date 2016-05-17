# Swiss Edu-ID Service Extensions

The key words 'MUST', 'MUST NOT', 'REQUIRED', 'SHALL', 'SHALL NOT', 'SHOULD', 'SHOULD NOT', 'RECOMMENDED', 'MAY', and 'OPTIONAL' in this document are to be interpreted as described in [RFC2119](https://tools.ietf.org/html/rfc2119).

## Personas and Scenarios

### Student

### Lecturer at different institutions

### Researcher using different facilities

### App developer

### System administrator

## Key Challenges for meaningful and trustworthy authenthication and authorization services

1. Service exposure and discovery

2. Functional service aggregration

3. User-Service Mapping

4. Protocol selection

5. Cascading Authorisation

6. Device management

7. App management

8. User service management

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

## References
