# User Experience

## Introduction

This part of the architecture describes the core user experience of the Swiss edu-ID Mobile App.

The Swiss edu-ID Mobile App has two core interaction modes:

* edu-ID Mode
* Authorization Mode

## Core User Experience

The core user experience is the Authorization Mode. The Authorization Mode is active if a third party app requests authorization for a service within the edu-ID Federation.

The Authorization Mode is split into three parts.

1. The Third Party User Experiences
1. The Operating System Experiences
1. The edu-ID Mobile App Experiences

For the three phases of the user experience it is necessary to understand that the *only* those experiences related to the edu-ID Mobile App are actually controllable.

For illustration purposes the authentication scheme for the edu-ID is called "App OAuth". This indicates that other federations may use the same scheme (as opposed to the system internal schemes for large commercial IDPs).

![common user experience workflow](images/eduid_app_ux_authflow.png)
The above workflow uses prototype user interfaces and does not replicated the final user experience. The visual presentation is subject for change in later stages of the project.

The common workflow from the user experience is:

1. The active app indicates that it requires access to an external service. The related user interface displays the possible options for authorization. (e.g. Password, Google, Facebook, App OAuth). The user experience for the authorization request is completely under control of the third party app maintainer.

2. The mobile operating system selects an appropriate app. Operating system uses its standard inter-app communication interfaces to allow the users to channel the request into the Swiss edu-ID Mobile App. On iOS the operating system launches an "App Selector"/"Sharing Menu" that allow users to select the appropriate authentication app. These pop-up menus are commonly used user interface elements.

3. The edu-ID Mobile App indicates to the user, what information has been requested and which services within the federation are capable of serving the requested data. This is done my presenting a list of services to the users.

4. The users can select one or more services to be authorized for the third party app. In case the app can handle only 1 authorization the users must select exactly 1 service. If no services were selected, all services are granted.

5. The users return to the third party app by accepting or rejecting the authorization request.

6. The third party app continues with its authorized interaction experiences.

It is important to recall that the edu-ID Mobile App can only control the steps 3-5 of the user experiences. The other elements of the user experience are integrated into the third party app's user experience or embedded into the operating system. In both cases the familiarity of the users can be safely assumed.

## Third Party App Experience Guidelines

Third party apps are under the solitary control of the app developer. The following guidelines give app developers an orientation how to integrate edu-ID Mobile App support.

Third party apps that require authorized interactions, often have a login view. For large commercial IDPs this view may already contain buttons so users can chose their preferred method of authorizing the app.

Because users can choose at the OS interaction level, which app they want to use for authorization, it is not guaranteed, which app is actually granting authorization. Therefore, it is RECOMMENDED to include a button for ```secure authorization``` or for ```app authorization```.

If an app does not support any other authorization scheme, an authorization button can be integrated in the app's main user interface or as a configuration element in the app settings.

Below an example authentication screen with multiple authentication schemes is displayed.

![Third Party App Authentication View](images/third_party_app_start_authorization.png)

## Operating System Experience

The OS interaction is fully controlled by the mobile OS. The only way of controlling this interaction is through an icon and the functional name.

The Android app selector for intents uses always the [colored app icon](03-artwork.md).

The iOS Share screen uses color icons for sharing apps (share extension), and a bitmask icon for bi-directional app communication (action extension). As the edu-ID Mobile App extension on iOS is implemented as an action extension, it uses a bitmask icon as displayed below.

![Authorization Selection on iOS](images/iOS_AppSelectionScreen.jpg)

## Utility Experiences

In edu-ID Mode the app provides access to utility functions. These include:

* Authentication with the edu-ID Service
* Profile View
* Unlinking the App from an Account

### Authentication with the edu-ID Service

The edu-ID Mobile App is persistently authorized with the edu-ID Service once the users authenticate. As long the edu-ID Mobile App is not authenticated for a user the edu-ID Mobile App will display a login screen.

![Authentication Screen](images/eduid_loginview_layout.jpg)

If a user selected the app authorization from a third party app without being authorized in the edu-ID Mobile App, step 3 of the core user experience will be preceded by edu-ID Authorization (by displaying the Login Screen).

### Profile View

If users start the edu-ID Mobile App in edu-ID Mode and the app is authorized for a user, then the app will start into the profile view. This is the primary interface for reviewing the personal data.

![Profile View](images/eduid_profileview_layout.jpg)

In a future iteration the profile view may provide interfaces to other persistent information such as account management functions, the list of authorized apps, or the list of used services. Presently, the only auxiliary function is the logout.

### Unlinking the App from an Account

If users decide to unlink the edu-ID Mobile App they can do so by using the "logout" button in the profile view. Before a user actually unlinks the app, a warning is presented in order to inform the users that logging out means also that all app authorizations will be rejected. At this point the users can stop unlinking the edu-ID Mobile App and return to the profile view.

![Logout Warning Screen](images/eduid_LogoutScreen_layout.jpg)

If the users confirm to logout, the all will return to the Login screen.

After logging out in the edu-ID Mobile App, the third party apps that were previously authorized will receive an error if they try to access the academic services. In this case third party apps should inform the users that a previous authorization has been invalidated and allow the users to reestablish the connection.

[Previous: Use Cases](01-use-cases.md) [Return Architecture Overview](00-overview.md) [Next: Artwork](03-artwork.md)
