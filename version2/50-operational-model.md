# Operational Model

## Introduction

The Core Swiss edu-ID infrastructure is designed as centralised authentication and identity management within a highly distributed authorization federation. The Swiss edu-ID Mobile App relies on the central authentication and partly on the IDP capabilities of the Swiss edu-ID environment.

Early in the project it became clear that mobile use cases are of no particular interest within the scope for the centralised infrastructure of the overarching edu-ID activities and are unlikely to be integrated as a single service.

This document outlines a distributed operation model for connecting native mobile apps to academic services within the swiss academic federation.

This initial version of the operation model outlines the relation of

* The core edu-ID Infrastructure

* Client management

* Dictionary services

## Centralised vs. Distributed Federation Management

The original operation model for the Swiss edu-ID Mobile App considered that supportive dictionary services are integrated with the central edu-ID Infrastructure. The central integration of Swiss edu-ID services allows the deep integration of the services into the Swiss edu-ID infrastructure. This allows to keep consistent information across all aspects of the authentication and authorization infrastructure.

A distributed approach to integrate mobile capabilities to academic federation services allows removes the need for central coordination and support by relying on shared standards but distributed infrastructures. This allows the edu-ID Infrastructure as well as the mobile authentication and authorization services independently. Participating institutions may adopt the service together or separately from the Swiss edu-ID infrastructure. Furthermore, dictionary services could be decoupled from central infrastructure and thus limits the need for exchanging personal data between parties within the academic federation.

## Core edu-ID Infrastructure

Within a distributed infrastructure the Swiss edu-ID services would act solely as authentication service. This infrastructure would be hosted and maintained by SWITCH and related parties.

Ideally all web-service endpoints that directly link to the Swiss edu-ID main service components should be hosted and operated by SWITCH. This includes all OAuth2 service endpoints.

## App-related Infrastructure

Clients for the edu-ID Mobile App can be managed by a separate service for authorizing Swiss Mobile App Clients. This service is responsible for maintaining app releases and controlling app availability for the different mobile platforms. As institutions who are active in mobile app development already have distribution processes in place this can be performed by the participating institutions.

The client authorization is part of the OAuth2 protocol and commercially available OAuth2 solutions already integrate client authorization, this could be easily integrated into the central Swiss edu-ID infrastructure. Alternatively, the clients could authorize separately against a client authorization service and use the Swiss edu-ID authentication services as resource services within the OAuth2 protocol. In this case the client authorization would be easily hosted at one or more institutions within the Swiss academic federation. As such service would extend existing authorization services, this could be performed at the institutions with very limited overhead as part of the already existing activities.

## Dictionary services

Dictionary services are used by the edu-ID Mobile App to enhance the user experience. Two dictionary services are part of the edu-ID Mobile App architecture.

* The Protocol Discovery Dictionary

* The Service Usage Management Dictionary

The protocol discovery dictionary allows the edu-ID Mobile App to determine the services in the federation that provide requested service endpoints. This service relies on links between the edu-ID Federation Index.

If the protocol discovery component is operated outside the core edu-ID Service, a link to query the federation index MUST be provided.

The personal service usage management dictionary provides support for enhancing the user experience for granting app access. The personal service dictionary keeps track of the relation of users and the services they use (have Successfully authenticated in the past). This allows the app filtering services that users have not used before and omit those services which were not previously accessed by a user.

The service usage management is tightly connected to service assertion, the affiliation index and the federation index. If this component is operated outside the core edu-ID Infrastructure, it MUST be ensured that personal information can be accessed between the related components. Most importantly, the service assertion MUST always report on user assertions to Federation services.

## Institutional Infrastructure

Individual Services need to integrate two basic services:

* Token-based authorization as specified in OAuth2 and the related specifications.

* an RSD provider that exposes the supported web-service protocols.

A number of services already support OAuth2, including MS Active Directory. Many modern services already support OAuth2 as an authentication scheme.

The majority of services uses some form of token-based authorization. For open source products existing token authorization can be expanded with relative ease to match the OAuth2 specification requirements.

The RSD provider is only necessary for service instances that wish to expose their web-services to mobile applications. RSD is designed for providing simple interfaces as easy as static text files. Static RSD files are suitable for services with web-service endpoints that do not change and are not configurable. For configurable services, dynamic RSD generation is simple in the context of modern web-service architectures.

Both components add not more overhead to system administration as the existing AAI integration.

Maintaining these components is typically performed by the institutions that run the services. For services with shared instances, the maintenance of service extensions can be shared across institutions.

The Service integration for both components has been demonstrated for the Moodle LMS platform.


## Potentially interested institutions within the Swiss Academic Domain

The following institutions have special apps available or are presently developing them. Some of these apps are of greater interest for the academic community or may benefit from authorized access to Federation Services.

* HTW Chur
* USI
* FHNW
* UZH
* ETHZ
* UNIGE

First contacts to commercial app developers have been initiated and positive responses have been declared but actual committment depends on the actual integration into the edu-ID Service infrastructure or into a different but equally scalable operation model.

__DISCLAIMER__: Concrete integration tests and collaboration committments are beyond the scope of this architecture document at the time of this writing.
