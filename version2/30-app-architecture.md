# Core App Architecture

The edu-ID Mobile App provides an authorization and authentication interface for securely connecting third party apps with services in the trust domain of Swiss academic services. The edu-ID Mobile App ensures that commercial and non-commercial third party apps can provide added value services based on the existing service infrastructure in Swiss higher education institutions. The edu-ID App's key function is to authorise third party apps on a user's device with academic services within the edu-ID federation. It helps to bridge the user/app store trust domain on the user devices and the trust domain within the Swiss Academic Service Federation.

The edu-ID Mobile App integrates interactions between the edu-ID infrastructure, services in the academic edu-ID federation, and third-party apps installed on the users' devices. It limits service and data exposure to third-parties to authorised sources.

This document describes the architecture of the edu-ID Mobile App.

## Introduction



This document relies on the [service architecture](30-app-architecture.md) and the [user experience](02-user-experience.md) parts of the edu-ID Mobile App architecture.

The app can be launched through two processes.

* stand-alone launch through the device's app launcher.
* protocol-authorization requests from third party apps.

## Components

The edu-ID Mobile APP has 6 core components.

* Profile manager
* Protocol discovery
* Service authorization manager
* App authorization manager
* App access provider

### Profile Manager

### Protocol Discovery

### Service Authorization Manager

### App Authorization Manager

### Service Management Manager

If a [Service Usage Component](20-service-architecture.md) is available in the federation, then this information will be included into the service management.

### App Access Provider

## User Interfaces

### Login Interface

The login interface is displayed whenever the app is launched and no active authorization for an edu-ID user is present. This can be during stand-alone launches or during protocol authorization requests

The login interface consists of the same interface elements for entering user credentials as the web-based edu-ID Service interfaces.

### Logout Interface

The logout interface provides an intermediate step before users can revoke their edu-ID authorization for the device.

The logout interface informs the users about the consequences and displays affected services and apps.

Users can decide via the logout interface if they want to continue logging out or if they want to keep the device authorization.

### Profile Information Interface

The profile information interface allows users to see the edu-ID identity that is connected to the present device.

### App Authorization Interface

The service authorization interfaces is launched before app authorization during protocol authorization requests of third party apps. The service authorization interface allows users to select services they want to grant a requesting app access.

Users can select all or some services for authorization. Alternatively, users can choose to completely reject an authorization request.

The information displayed in the app authorization interface is the result of the protocol discovery for a third party app's protocol authorization request. This result includes all Federation Services that support the requested protocols.

Users can narrow down the provided information via two filters.

* a personal filter, and
* an institution filter

The personal filter relies on the service manager to filter only services that were accessed previously.

The institution filter relies on profile information and includes only services, for which a user has active affiliations. If no affiliation information is present in a user profile, then the institution filter is not available.

In addition users can use a __search__ to narrow down the service list. The search targets the service's display names and host address.

### Service Authorization Interface

The service authorization interface allows users to introspect  granted authorizations. Through this interface users can see which apps have active authorizations and which services they use.

Users can actively revoke service and app authorizations through the service authorization interface.

The service authorization interface does not allow to request service authorizations.

## Security Considerations

Whenever possible the edu-ID Mobile App MUST be configured for data encryption and memory randomization. This ensures device-level data protection.

The edu-ID Mobile App uses only token authorization for accessing the edu-ID Service as well as Federation Services. User credentials MUST NOT get stored on the device. On Android and iOS this is defined at built time.

Additional authentication (e.g. biometric, beacon, network level, geo-location) using device sensors MAY be requested at the level of the edu-ID authentication, service authorizations, and app authorization if enhanced security requirements become necessary. Such extensions will be tied to the respective components and are not considered independently at the level of the app architecture.
