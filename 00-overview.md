# Swiss edu-ID Mobile App

The Swiss edu-ID mobile app aims for improving the integration of mobiles apps with IT services provided by Swiss Universities.

The edu-ID Mobile App provides an authorization and authentication interface for securely connecting third party apps with services in the trust domain of Swiss academic services. The edu-ID Mobile App ensures that commercial and non-commercial third party apps can provide added value services based on the existing service infrastructure in Swiss higher education institutions. The edu-ID App's key function is to authorise third party apps on a user's device with academic services within the edu-ID federation. It helps to bridge the user/app store trust domain on the user devices and the trust domain within the Swiss Academic Service Federation.

This part of the project documents the architectural concepts underpinning the Swiss edu-ID Mobile App and its surrounding ecosystem.

The documentation has the following parts.

* Introduction and Overview (this document)
* [Use Cases](01-use-cases.md)
* [User Experience](02-user-experience.md)
* [Artwork](03-artwork.md)
* [Technical Terminology](10-terminology.md)
* [Service Architecture](21-service-architecture-oidc.md)
* [App Architecture](30-app-architecture.md)
* [iOS-specific Architecture](32-ios-app-architecture.md)
* [Native Application Integration Layer (NAIL)](40-nail-api.md)
* [Service Operation Model](50-operational-model.md)

# Source code

* [iOS Mobile App](https://github.com/BLC-HTWChur/eduid-app-ios)
* Android Mobile App (Not Yet Released under an Open Source License)
* [OAuth2 Demo Service](https://github.com/BLC-HTWChur/eduid-coreservice)

Moodle Plugins for Federation Service Demonstration

* [Moodle RSD2 Discovery Plugin](https://github.com/arael/moodle_rsd)
* [Moodle edu-ID OAuth2 Plugin](https://github.com/BLC-HTWChur/moodle_eduid)

# Contributors

* Christian Glahn (HTW Chur)
* Ulrich Hauser-Ehninger (HTW Chur)
* Riccardo Mazza (USI)
* Goran Jusic (USI)
* Yann Cuttaz (USI)
* Ioana Gatzka (FHNW)
* Michael Hausherr (FHNW)

## License

The architecture documents are licensed under the [Creative Commons Version 4.0 Attribution Share-Alike](LICENSE.md). Commercial use of these documents is allowed under the conditions of the  license.

For more details please refer to the [LICENSE.md](LICENSE.md) document that came with this repository or visit the [Creative Commons web-site](https://creativecommons.org/licenses/by-sa/4.0/).

2016 CC BY-SA 4.0

## Presentations and publications

* [Mobile Identities with the EduID Mobile App](http://htw.ac/2kUfc0H) at the Swiss EduID Event in Bern 2016
* [EduID Mobile App Use-Cases, Concepts and Implementation](http://htw.ac/2pl7HoJ) at the Swiss EduID Integrators Workshop in Zurich, 2017.

| | Return to Architecture Overview | [Next: Use Cases](01-use-cases.md) |
| :---- | :----: | ----: |
