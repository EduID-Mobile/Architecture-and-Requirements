# Native Application Integration Layer

## Introduction

The Native Application Integration Layer abstracts OAuth2-specific, authentication protocol specific and operating system specific interactions for third party applications that run natively on the users' devices.

Apps are expected to rely on public protocols

The NAIL is designed to facilitate simultaneous connections to several service endpoints. Apps that can handle only one simultaneous authorization MAY limit this support. Otherwise it is always safe to assume that average users in the Swiss academic domain have access to more than one Federation Service that meets an app's protocol requirements.

While NAIL supports the protocol of the Swiss EduID Mobile App, it is agnostic to the authorizing app. The only requirement is that the authorizing app uses the same data formats.

## NAIL API Overview

The NAIL API supports two interactions with the EduID trust domains.

1. Access requets to authorizing apps (7)
2. Authorized access to service endpoints (9)

### Protocol Discovery and Authorization

The NAIL protocol requires an app to present a set of information, including but not limited to its bundle id and name.

### Authorized Service Requests

### Access Restrictions

Scoped requests

- Apps may not access endpoints outside their scope

Simple Bearer Tokens Handling

Refreshing Access Tokens


## Javascript Integration for Apache Cordova

