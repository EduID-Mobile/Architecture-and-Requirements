# Operational Model

## Introduction

The Core Swiss EduID infrastructure is designed as centralised authentication and identity management within a highly distributed authorization federation. The Swiss EduID Mobile App relies on the central authentication and partly on the IDP capabilities of the Swiss EduID environment.

Early in the project it became clear that mobile use cases are of no particular interest wihtin the scope for the centralised infrastructure of the overarching EduID activities and are unlikely to be integrated as a single service.

This document outlines a distributied operation model for connecting native mobile apps to academic services within the swiss academic federation.

This initial version of the operation model outlines the relation of

* The core EduID Infrastructure
* Client management
* Dictionary services

## Centralised vs. Distributed Federation Management

The original operation model for the Swiss EduID Mobile App considered that supportive dictionary services are integrated with the central EduID Infrastructre. The central integration of Swiss EduID services allows the deep integration of the services into the Swiss EduID infrastructure. This allows to keep consistent information across all aspects of the authentication and authorization infrastructure.

A distributed approach to integrate mobile capabilities to academic federation services allows removes the need for central coordination and support by relying on shared standards but distributed infrastructures. This allows the EduID Infrastructure as well as the mobile authentication and authorization services independently. Participating institutions may adopt the service together or separately from the Swiss EduID infrastructure. Furthermore, dictionary services could be decoupled from central infrastructure and thus limits the need for exchanging personal data between parties within the academic federation.

## Core EduID Infrastructure

Within a distributed infrastructure the Swiss EduID services would act solitarily as authentication service. This infrastructure would be hosted and maintained by SWITCH and related parties.

## App-related Infrastructure

Clients for the EduID Mobile App can be managed by a separate service for authorizing Swiss Mobile App Clients. This service is responsible for maintaining app releases and controlling app availability for the different mobile platforms. As institutions who are active in mobile app development already have distribution processes in place this can be performed by the participating institutions.

The client authorization is part of the OAuth2 protocol and commercially available OAuth2 solutions already integrate client authorization, this could be easily integrated into the central Swiss EduID infrastructure. Alternatively, the clients could authorize separately against a client authorization service and use the Swiss EduID authentication services as resource services within the OAuth2 protocol. In this case the client authorization would be easily hosted at one or more institutions within the Swiss academic federation. As such service would extend existing authorization services, this could be performed at the institutions with very limited overhead as part of the already existing activities.

## Dictionary services

Dictionary services are used by the eduID Mobile App to enhance the user experience. Two dictionary services are part of the eduID Mobile App architecture.

* The Service Dictionary
* The Personal Service Dictionary

The service dictionary is part of the service discovery, that allows the EduID Mobile App to determine the services in the federation that provide requested service endpoints.

The personal serivce dictionary provides support for enhancing the user experience for granting app access. Technically, this information is already available to the EduID service. A distributed API for this service would only allow




In the case of a distributed dictionary services, Federation Members may inject effective dictionary services as a institutional attribute to an EduID entry. This would allow the EduID Mobile App to preselect the effective dictionary services for the effective EduID user.

## Institutional Infrastructure

Service Integration
