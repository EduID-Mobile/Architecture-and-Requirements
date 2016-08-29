	# Swiss EduID Mobile App Use Cases

## The Problem of Integrating Mobile Apps into Service Federations

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

This problem is relatively small in centralised federations with very few (e.g. one) authorization services that connect to many resource services. In such environments the authorizing party plays a central role for managing access of native applications to resource services. This is not the case in distributed federations with many resource and many authorization services because there is a central control instance missing.

The examples of the OAuth2 specification implicitly assume that all parties are known to each other prior to the actual authorization process. In the examples of OAuth2 a client "knows" the authorization endpoint either through configuration or by asking users for their authorization endpoint. In distributed federations configuring authorization services with each app is highly inefficient and not flexible. The remaining alternative that balances flexibility within the envisioned OAuth2 protocol would be to request authorization endpoints. However, this exposes security-relevant infrastructure to third parties.

In combination of the user agent problem with the prior exposure of authorization infrastructure allows rouge parties to gather full user credentials before the authorizing party is contacted. Within a distributed federation this means that this third party does not only have access to the requested resource service but to all resource services of a federation. In order to reduce this problem, system administrators and developers of native applications have to put increased effort into building and maintaining trust relations. Typically outside of the formal trust relations for the federation.

While this is not specific to mobile apps, mobile app are more affected than desktop applications as mobile devices have a broader device capabilities that many desktop systems that are unavailable to web-based applications. Additionally, the usage of mobile devices and the apps installed on them have a wider application scenario, including location-based activities, augmented reality, sensor-based data collection, and off-line use. Again, many of these functions are also available on specialised desktop hardware, but the user experience and the ubiquity of mobile devices made broader applications practical.

## edu-ID Mobile App Ecosystem Stakeholders

Human stakeholders play a key role in the process of machine-to-machine communication across trust domains. Therefore, the experience of integrating and using the infrastructure must not add complexity to the app development, system maintenance, or device usage. Four key stakeholders are identified for the architecture.

* Federation Management

* Service Provider

* Mobile Device Owner

* Application Developer
