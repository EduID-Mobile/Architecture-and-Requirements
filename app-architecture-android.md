# Swiss Edu-ID App Architecture for the Android Platform


## Application Authorization

![Application Authorization Scheme](images/applicationAuthorizationAndroid.png)

1. Third Party App call Authorize method of Library
2. Library bind the service of edu-id App, and call authorize Service. A singleton is created in the library, and the callback of 3rd party app is saved locally.
3. The service switch the two app, and the user show edu-id App with a dialog message to authorize 3rd Party app. The connection between two apps is closed, and the user is on the edu-id App.4. When the user click “yes” or “no” to accept or reject application, the edu-id App bind the service of 3rd Party App edu-id Library, and call AuthorizationCallback Service.5. The service close the connection with edu-id App, switch the two app and call the singleton saved previously to execute callback method.