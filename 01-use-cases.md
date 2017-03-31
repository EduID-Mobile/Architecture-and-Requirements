# Swiss edu-ID Mobile App Use Cases

## Authorization Types of Mobile Apps

Today, many different types of mobile apps are available that have different requirements for authentication and authorization.

1. Single-user personal apps

2. Apps for anonymous communication or collaboration.

3. Apps that rely on and are tied to one distinct infrastructure service

4. Apps that may provide authorized access to more than one infrastructure service

Single user apps are tied to a single user and run anonymously on the device. All data is tied to the device. Any infrastructure service in the background needs only to distinguish individual devices.

Apps for anonymous communication or collaboration provide communication means between unauthorized actors. All transactions between users are tied to the personal devices of the actors. Actors may have to opportunity to limit the communication through shared channels. These channels can be shared through visual cues (such as QR-Codes), channel names, location, time, or a combination of the previous identifiers. Again, any infrastructure service in the background needs only to distinguish individual devices.

These types of apps authorize transactions on the **device level**. **Device-level authentication** does not allow the identification of actors and restricts multi-device persistency for an actor.

In order to allow the identification of actors or to provide multi-device persistency it is necessary to authenticate and often to authorize an actor across devices. In such scenarios it is necessary to authorize transactions on the **actor level**, typically through one or more credentials and challenges that verify the identity of an actor. **Actor-level authentication** enables actors to switch devices or applications while maintaining the state of their actions.  **Actor-level authentication** allows restricting the access, use, and manipulation of resources depending on the privileges of an actor.

The EduID Infrastructure concerns the **actor-level authentication and authorization** and thus supports more specific authorization schemes of the connected infrastructure services within its federation.

### Service-bound Mobile Apps

Many mobile apps rely on exactly one infrastructure service in the backend. These apps are permanently bound to their service instance. This **service binding** is typically achieved by including a distinct URL as part of the app's business logic. Such apps are typically maintained by the service provider of the underpinning service instance.

A special case of **service-bound mobile apps** are customized generic apps of a commercial product that are bound to one instance of the product. Such customized apps are often called **branded apps** because customization often includes some or only customizations to the service providers corporate identity.

Examples for **service-bound mobile apps** are the SWITCH Drive app or the ETHZ Polybox app that are bound to instances of the OwnCloud Service that run at SWITCH or ETHZ.

**Service-bound mobile apps** and their infrastructure service might be treated as separate services from the perspective of the EduID Infrastructure, while an infrastructure service accepts both, its regular authentications as well as authentications for its mobile app. Such generalization is valid because for each infrastructure service there is one mobile app.

### Protocol-bound Mobile Apps

Other mobile apps do not rely on a specific infrastructure service but on the availability of well-defined service protocols. Such apps are bound to one or more protocols and can connect to any infrastructure service that supports these protocols. This **protocol binding** relies on well-defined protocols that are implemented as part of the apps business logic for communicating with infrastructure services. Such apps are typically not maintained by the provider of one service instance. This reduces costs for the service provider that originate from maintaining customized mobile apps.  

Examples for **protocol-bound mobile apps** are the official OwnCloud App, the NextCloud App, the Adobe Connect App, the Moodle App, and the Mobler App. Some **protocol-bound mobile apps** can connect to multiple services at the same time (e.g., the OwnCloud App, the NextCloud App, or Mobler App), while others support only one service connection at the same time (e.g., the Adobe Connect App or the Moodle App).

A special case of **protocol-bound mobile apps** are **vendor-specific mobile apps** that are tightly related to an infrastructure product. These apps can connect only to infrastructures service instances of one product, because they rely on vendor-specific and/or proprietary protocols. Examples for such **vendor-specific mobile apps** are the Moodle App, the Adobe Connect App, and the OwnCloud App.

**Instance-related branding** is also possible for **protocol-bound mobile apps**. In this case an infrastructure service must provide branding information that can change the user interface on demand. This kind of branding does not require any modification of an app's business logic.

**Protocol-bound mobile apps** and the infrastructure services cannot be treated as separate services, because the mobile app acts as a **client** or **user-agent** to the infrastructure services it can connect to. Different to **service-bound mobile apps** where an app authentication can be treated as equal as an direct authentication for the underpinning service, app authorizations of **protocol-bound mobile apps** would imply that **all** service instances that satisfy the protocol requirements would have to accept or reject an app authorization simultaneously. This is unwanted in all cases, in which different infrastructure instances provide a service for different groups of actors.

If **service-bound mobile apps** are treated as clients to a service rather than independent services, then **service-bound mobile apps** are a special case of **protocol-bound mobile apps**.

### Authentication challenge of protocol-bound mobile apps

One particular challenge with protocol-bound mobile apps is that these apps need to get informed about the instance of an infrastructure service. This information is passed typically in the form of an URL by the actor to the app's business logic. Only after this information is presented, a protocol-bound mobile app can initiate the service authorization.

Given that the service URL provides the initial connection information for the authorization, it is presented before an app has been authorized to access the service and before an authorizing party (in our case the EduID Service) could verify the validity of such request. This procedure also informs an app about the success of a actor connecting to an service instance.

Ideally, an app would be presented with the service information and the connection endpoints of the service only after the authorization has been granted. In this case an app would not be able to determine, whether an authentication or authorization succeeded or even took place.

All use-cases in this document refer to scenarios, in which **protocol-bound mobile apps** can be applied and that are concerning in one way or the other the authentication challenge.

## The Problem of Integrating Service independent Mobile Apps into Service Federations

The OAuth2 specification defines a protocol framework for providing access to protected resources exposed via web-services.

The OAuth2 specification defines 2 types of clients:

* Confidential clients, which can maintain confidentiality of credentials towards third parties.

* Public clients, which cannot not reliably maintain confidentiality of credentials towards third parties.

The specification differentiates between three types of applications:

* server-hosted web-applications,
* user-agent-based applications that run in a web-browser, and
* native applications that are installed on a user's device.

These types are considered as confidential or public clients depending on the trust domain to which a clients belongs. Given the often integration of web-applications they are considered as "confidential clients". On the other hand user-agent-based and native applications are considered as public clients as these run outside of a legally constrained trust domain. This means these applications run on infrastructure that neither belongs to the authorizing entity nor to the application provider.

Section 9 of the OAuth2 specification describes authorization handling for native applications based on user-agent that creates a connection to the authorizing party using an

* External user-agent (web-browser) or
* Embedded user-agent (web-view).

While the specification considers only embedded user-agent authorization as a security risk, both approaches are not reliably distinguishable for the authorizing party. An authorizing party that entirely relies on user-agent-based authorization can only do so for verified and trusted native applications. This verification has to be performed for each release of a native application or the application must be released through a fully trusted source (e.g., the same department within the organization). A latter client, however, might be likewise considered as a confidential client that is controlled by the authorizing party.

This problem does not exist for user-agent-based applications as they cannot manipulate the behaviour of the underpinning user-agent, whereas native applications typically run with the same or higher privileges than the authorizing user-agent.

This problem is relatively small in centralised federations with very few (e.g. one) authorization services that connect to many resource services, while the access to these services are managed via the authorization service. In such environments the authorizing party plays a central role for managing access of applications and actors to resource services. This is not the case in distributed federations with many resource and many authorization services because access control is not and often cannot get centralized.

The examples of the OAuth2 specification implicitly assume that all parties are known to each other prior to the actual authorization process. In the examples of OAuth2 a client "knows" the authorization endpoint either through configuration or by asking users for their authorization endpoint. In distributed federations configuring authorization services with each app is highly inefficient and not flexible. The remaining alternative that balances flexibility within the envisioned OAuth2 protocol would be to request authorization endpoints. However, this exposes security-relevant infrastructure to third parties.

In combination of the user agent problem with the prior exposure of authorization infrastructure allows rouge parties to gather full user credentials before the authorizing party is contacted. Within a distributed federation this means that this third party does not only have access to the requested resource service but to all resource services of a federation. In order to reduce this problem, system administrators and developers of native applications have to put increased effort into building and maintaining trust relations. Typically outside of the formal trust relations for the federation.

While this is not specific to mobile apps, mobile app are more affected than desktop applications as mobile devices have a broader device capabilities that many desktop systems that are unavailable to web-based applications. Additionally, the usage of mobile devices and the apps installed on them have a wider application scenario, including location-based activities, augmented reality, sensor-based data collection, and off-line use. Again, many of these functions are also available on specialised desktop hardware, but the user experience and the ubiquity of mobile devices made broader applications practical.

## Use Cases for the edu-ID Mobile App

For the edu-ID Mobile App four initial use cases were identified. The use cases were collected in cooperation with the participants of the eduhub SIG Mobile Learning. All use cases rely on the [edu-ID Mobile App user experience](03-user-experience.md).

### Mobile Learning Use Case

Integrating mobile technologies into higher education is still a challenge in higher education. While pedagogies become clearer, the integration of mobile tools into overall educational proceses remains a key challenge. This use case has three facets

1. Extended responsive design
2. Extended mobility
3. Location-based activities

#### Extended responsive design

Provided by FHNW, HTW Chur, USI, and ZHAW.

All recent versions of web-based learning management systems (LMS) provide so called responsive user interfaces. Browser-based responsive design provide good solutions for quick connections to the learning management system and users can navigate the system using well-known metaphors. However, responsive designs require a standing connection to the learning management systems and as all browser-based apps they have limited access to the advanced device capabilities.

Many LMS platforms offer dedicated mobile apps for their systems to bridge the gap. This is also the case for [Moodle](http://www.moodle.org), a widely used LMS in Swiss Higher Education. Due to specific functional limitations of the Moodle App, USI has developed the iCorsi App. iCorsi is an alternative native App for the Moodle platform. The key function of both apps are improved user interfaces, extended offline capabilities, and improved resource uploading. Apart from this extended features, both apps resemble the concept of the responsive counter parts on the web.

Presently the Moodle App can be integrated into Moodle systems through managed accounts. A SAML2 integration has been added, recently. However, the SAML integration needs activation in customised versions of the app, which tends to confuse students. The alternative approach is the concept of "alternative logins". Alternative logins assign a password to the user accounts, so users can log into a system using an alternative authentication scheme for that system.

In this scenario students and lecturers have to to remember alternative passwords. Also all participants have to be registered on the platform when the alternative login information is generated. On the federation management side this undermines the password security policy, because the passwords have to be stored for the LMS while with the federation signle sign on no user credentials are present on the system. For system administrators or course managers this means overhead work for user management that would be otherwise part of the federation single sign on.

With the edu-ID Mobile App the integration of the Native Application Integration Layer (NAIL) API allows both apps to use the standard authorizations for the Swiss Academic Domain without adding alternative user management requirements to the local LMS instance. Students and Lecturers don't have to remember alternative usernames and passwords. System administrators don't have to coordinate alternative authorizations on the system or maintain customised versions of a native app but use the officially release software.

For alternative variants such as the iCorsi app, it would be possible for students and lecturers to connect to Moodle instances of other institutions using the known mobile environment. Also other institutions with similar requirements as USI could easily adopt the app without having to support app development or customisation in house.

#### Extended mobility

Provided by HTW Chur and UZH.

Mobile learning includes also activities that are typically not performed in onlie or class room settings. This means that an app offers alternative learning activities that are particularly tailored or optimised for supporting student (and lecturer) mobility on top of existing learning resources. At the same time, lecturers and students are interested that the learning experiences are also documented in the universities LMS, so they can be considered to collaboration, learning support, and ultimatively for grading.

The Mobler App by the BLC of the HTW Chur is one case of such extended mobility. The Mobler App is a flash card learning app that has no service infrastructure on its own account. It can load learning resources as well as user interface customisations from different LMS instances. It already supports the simulteneous use of several authorized connections. Students can use the Mobler App to practice exam questions whenever they choose. Mobler uses the LMSes' question pool/question bank features for loading test questions. Lecturers can arrange the questions' availability as part of the online course management. The student activity on the mobile devices is also reported back to the LMS, so the student's mobile activity can be used for student support, for course personalization, or for inclass collaboration.

The Mobler app has been tested in Switzerland at the HTW Chur (BSc Level), ETHZ (Professional Further Education), and UZH (BSc Level) in customized environments. Mobler can work with Moodle and Ilias Systems if these systems expose standard protocol web-service endpoints. OpenOLAT support is presently prepared.

Like in the case of Extended responsive design, Mobler builds on top of the LMSes standard interactive authorization and presently relies on the availability of user crendentials in the LMS. An alternative PIN-based authetication is also available, where a signle sign on PIN is generated on the LMS that can be used for personalised system access. The PIN generation requires a special plugin for each LMS that wishes to use the Mobler app.

Both authentication schemes create significant overhead for the students. Evaluations have shown that both alternative passwords and PIN-based authentications create a significant barrier for students. This causes that around 5-10% of the students cannot manage independently to authenticate with the system without assistance of a tutor or professor. Typical reasons for this are listed below.

* Wrong usernames
* Wrong password
* Wrong authententication system selection
* Usage of passwords instead of PINs
* Usage of usernames instead of PINs
* PIN reuse

In medium-sized and large groups between 100 and 500 students this requires substansive human resources and teaching time for bridging this barrier.

For the app development this creates also an overhead, because the different authentication schemes and additional user interfaces for selecting the authentication endpoint increase the complexity of the business logic. If PIN authentication should be used, then this adds significant overhead for maintaining the respective platform extensions.

With the edu-ID Mobile App and integration of its NAIL API into the Mobler app, students and lecturers can just authorize the Mobler App and automatically provide all relevant authorizations to LMSes where a person has active accounts. This increases learning activity participation and lowers support and maintenance time.

#### Location-based activities

Provided by ETHZ.

The OMLETH developed at the institute of Geo-information Systems at ETHZ is a mobile learning solutions for location-based activities. OMLETH is developed under the umbrella of the ETHZ' educational innovation program "innovedum".

In OMLETH lecturers can place "points of interests" (POI) to locations on a map and assign learning activities to these locations. The POIs and activities can be arranged and sequenced by the lecturer to build different learning scenarios. Students can use the OMLETH mobile app to move to the different POI and get there access to the related learning activities. The lecturers can monitor the student activity on a live map and can send messages to the students.

The OMLETH system relies on personalised experiences. Therefore, it requires authenticated users. One of the key challenges is the setup of the system for classes and cohorts.

With the edu-ID Mobile App students and lecturers can authenticate just with their university accounts and by using LMS information about courses and learning tasks, the OMLETH system could preconfigure itself. This would lower the administrative overhead for setting up location-based learning activities. Furthermore, other institutions could use OMLETH in their environment without altering the overall business logic of the OMLETH system.

#### Augmented Reality

Provided by HES-SO.

The Cyberlearn team at HES-SO implemented a Augmented Reality system that is integrated with the universities' Moodle system. The solution allows to connect and overlay learning resources defined in the learning management system into real world experiences via head-on displays.

Presently each head-on display has to be actively linked to the LMS system that provides the relevant learning resources. This typically involves a smart phone that the student carries together with the head-on display. The present system reqiures system administrators to connect the display technology with the relevant resources in the LMS through and alternative authoriztion scheme.

With the edu-ID Mobile App and wider adoption of smart wearable devices such as smart watches and head-on displays students could authorize directly on their smart phones and an AR app on the phone would automatically connect the head-on display of the HES-SO AR Solution.

#### Mobile Assessment

Provided by UZH and ZHAW.

E-Assessment becomes increasingly relevant for all academic institutions. Bring your own device (BYOD) policies become increasingly relevant for upscaling as the  uptake of e-assessment practices increases in the Swiss institutions. BYOD also implies that students may bring any computing device for participating in an e-assessment exam. Today, this already involves tablets as alternatives to laptop computers. Swiss universities have developed already solutions for supporting e-assessment of tablets. A prominent example is the Beaxi App of the University of Basel. Alternatively, the ETHZ reported that it plans a mobile version of its safe exam browser, which is already actively used within the Swiss academic community. Presently, such apps require alternative authentication.

With the edu-ID Mobile App e-assessment apps on tablets could use the official authorization of the Swiss academic community. By connecting to authorized LMSes these apps could even identify active exams automatically. This could increase the acceptance of e-assessments in BYOD settings.

### Mobile Video Conferencing

Provided by HTW Chur.

Video conferencing has become an important tool for scientific collaboration and educational activities alike. Web-based video conferencing solutions have liberated video conferencing from dedicated hardware and installations.

All contemporary web-based video conferencing solutions have special mobile apps that enable users to join video conferences from smart phones and tablets. Switch provides the widely used Adobe Connect and the more recent Vydeo platforms. For both platforms there is option to authenticate their mobile apps through the existing single sign-on solutions of the Swiss academic federation.

Presently, video conference organisers must open the video conference to anonymous guest access if parties wish to join a video conference from their mobile devices. While this guest access can be protected through a public password, there is no option for the video conference organisers to identify guest participants until these participants actively unveil their identity (by voice or video) in the running video conference session. If participants wish to join through the respective mobile app they have to type the full web address of the video conference room, a guest username, and the guest password in the app. For recurring events, like online classes, this has to be done for every session.

With the edu-ID Mobile App commercial vendors can add support for app-based authorization for their plattform specific protocols. This allows to fully benefit from already existing user experience of the native app for managed video conferences. Participants don't have to enter cryptic URLs, usernames, and passwords but can join with their institutional identities. In these scenarios also video conference hosts could join a video conference meeting from their mobile device and have the moderation tools available. For video conference organisers this also allows to actively restrict the participants for closed meetings.

### Commercial Partner Use Case

Provided by FHNW.

Many academic institutions partner with commercial parties to provide benefits for their students and staff. Among these partnerships are transportation discounts with the SBB (Swiss National Trainservices). Students can buy discount tickets for their visits on campus. Such partnerships are specifically relevant for institutions with distributed campus locations in different cities, such as FHNW.

Mobile tickets become increasingly relevant especially in the public transportation sector. The public transport companies have no interest to integrate proprietary identity systems for the academic institutions into their mobile payment solutions. Consequently, students can only obtain discounted tickets on staffed counters where their student affiliation can be verified.

With the edu-ID Mobile App the academic institutions would agree on a verification protocol with the SBB payment solutions. Students could connect their student account to the SBB mobile app and authorize access to the respective verification service. This would allow students to directly benefit from discount opportunities for public transportation and benefitting from mobile payment solutions as they could personally identify themselve to the mobile payment system through the cryptographically protected authorization scheme of OAuth2 and JWT. For the institutions there would be a transparent reporting mechanism if the protocol requires to report every validated ticket back into the universities administrative system.

### Campus Information Apps

Provided by ETHZ, UNIGE, and USI.

Campus information apps were among the first mobile apps in higher education. Serveral univeristies within the Swiss academic domain have implemented or are presently implementing respective apps. Campus information apps are typically highly tailored to a university's IT infrastructure.

Typically, campus information apps combine public information, such as mensa plans or room maps, with personalised information such as lesson schedules and group communication. Such personalisation requires authorized access to the supportive infrastructure.

Presently, this is done through customised solutions. These solutions work well in constrained environments. However, several institutions offer joint study programmes. E.g. UNIGE, UNIL and EPFL as well as ETHZ and UZH offer joint programs in which students attend activities at the participating institutions. USI and SUPSI cooperate insentively in the area of information system integration. Similarly, practically all Swiss UAS are distributed organisations in which the member institutions have varying degrees of freedom, autonomy, and system integration.

With the edu-ID Mobile App campus information apps can integrate also information for cooperation programms for personalising the user experience in the varying apps. If IT Systems support the same web-service protocols, even deeper integration of these apps for enriching the study experience of mobile students can get achieved. The different campus information apps could also rely on a unified user management that allows them to differentiate between students, staff, and visitors.

### Mobile Web-applications

Provided by HTW Chur (via Facebook).

Recent versions of mobile operating systems allow apps to extend browser behavior. Facebook started in September 2016 to provide a browser extension on iOS that allows mobile users to authorize for browser apps via their Facebook app.

With the edu-ID Mobile App users can handle OAuth2 authorizations to browser apps. Instead of being redirected to a login page, mobile users would simply authorize the browser app via the edu-ID Mobile App without having to enter their password.

## edu-ID Mobile App Ecosystem Stakeholders

Human stakeholders play a key role in the process of machine-to-machine communication across trust domains. Therefore, the experience of integrating and using the infrastructure must not add complexity to the app development, system maintenance, or device usage. Four key stakeholders are identified for the architecture.

* Federation Management

* Academic Institutions

* Service Provider

* Mobile Device Owner

* Application Developer

### Benefits for Federation Management

#### Transparent transitions between trust domains

The key benefit for federation management is that institutions do not need to implement alternative authorization schemes for integrating mobile apps. This reduces the risk for multiple use of passwords in different systems and in case that one of these system has an information breach, that this potentially affects the entire federation.

The edu-ID Mobile App consists of a set of platform-specific apps. For each mobile platform there is only one app that authenticates with the federation. The edu-ID also eliminates the risk that native apps can undermine authorization levels of the web-protocol.

#### Multi factor authorization can benefit directly from device capabilities

The edu-ID Mobile app creates new opportunities for multi-factor authentication using device capabilities, including biometric authentication or token-based authentication for mobile devices.

### Benefits for Academic Institutions

For academic institutions the edu-ID Mobile App makes it easier to cooperate with m-commerce partners.

Adademic institutions also benefit from the edu-ID Mobile App as they can easier integrate mobile apps into their service portfolio.

### Benefits for Service Providers

Service providers benefit from the edu-ID Mobile App in two ways.

1. Narrow access profiles (protocol scope)
2. Easier integration of third party apps into service infrastructure

OAuth2 scoping allows application fencing at the level of service endpoints. Academic service providers gain better control of what functions of the systems mobile apps can access and which ones they cannot. If native apps violate this agreement, then the OAuth2 protocol provides anchor points for reporting and automatically restricting these violations.

As the edu-ID Mobile App authorizes publicised protocols, it becomes easier for service providers to integrate third party apps into their service infrastructure. Third party apps may support proprietary protocols of specific web-service products such as video conferencing platforms, or rely on interoperability standards as they are published for examplee by ISO, IEEE, IETF, OASIS, or the IMS Global Consortium.

### Benefits for Mobile Device Owners

Mobile device owners include students, staff, and visitors of academic institutions alike. They benefit from the edu-ID Mobile App by having a central place for managing their federation affiliation and their app authorizations to their federation services. Using the app reduces the need to entering the username and password through cumbersome mobile device keyboards.

Also it decreases the risk that device owners accidentally expose their user credentials to third party apps. With persistent authorizations for the edu-ID Mobile App, students have to enter their password on their mobile device only in an interoductionary session and then the authorziation remains active as long they study or as long as they use a device.

In the educational setting, students and lecturers can just authorize a mobile app to access information about their courses without having to choose their institutions' LMS, to remember alternative passwords or to request one-time PINs. Yet, the edu-ID Mobile App maintains control by requesting explicit confirmation for authorizations. This lowers the barrier for students because they can require to install just one app on their device and use just one set of credentials for all services and apps, completely independent from the tools that are actually used in their curriculum at their institutions.

### Benefits for Application Developers

Developers of native apps benefit from the edu-ID Mobile App as they can rely on a standards based and well documented authorization scheme.
The edu-ID Mobile App eliminates the need for alternative authentication schemes or special platform extensions. This simplifies the app maintenance process and reduces resource requirements.

Furthermore, app developers can move faster to market by defining or building on data-exchange protocols. By using the NAIL API Framework, much of proprietary authentication logic can be replaced with a few lines of code. As the NAIL API can accept app specific authororization tokens, app developers can even offload OAuth2 related authorization logic into the NAIL API Framework without using an external authorization app.

App developers can easier integrate multiple authorized services into a single app. This makes it easier for them to support the user experience in environments where users have to use the same protocols for connecting to different systems.

| | [Return to Architecture Overview](00-overview.md) | [Next: User Experience](02-user-experience.md) |
| :---- | :----: | ----: |
