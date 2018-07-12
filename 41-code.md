# Code documentation and repositories references

## Code

The code so far produced is divided in new building blocks and integration. The building blocks are plugins and parts of the infrastructure which has been created from scratch while the integration is existing code modified to suit the Swiss EduID Mobile project.

## Building blocks

These are the building blocks produced so far:

1. [Eduid-iOS](https://github.com/BLC-HTWChur/EduID-iOS) is the iOS Swiss eduID Mobile app for managing user profiles on the iOS devices. 

2. EDUID_MOBILE_APP_REPO_FOR_ANDROID_TBD is the Android Swiss eduID Mobile app for managing user profiles on the android devices. 

3. [EduIDPlugin](https://github.com/BLC-HTWChur/EduIDPlugin) is the cordova NAIL plugin for third party apps. Both iOS and android are supported. We have used tested this plugin in the Moodle Mobile App.

4. [swisseduid](https://github.com/BLC-HTWChur/swisseduid) is the moodle auth plugin for the Swiss EduID Authentication.


## Integration efforts

1. [moodlemobile2](https://github.com/BLC-HTWChur/moodlemobile2) is the Moodle Mobile App clone with some add-hoc code for the EduID Authentication integration through the EduIDPlugin.

Since the moodle mobile app manages the user authentication in three separated controllers the EduIDPlugin hook has been placed inside the service helper inside the following file:

```/www/core/components/login/services/helper.js```

The helper code calls the [EduIDPlugin](https://github.com/BLC-HTWChur/EduIDPlugin) functions defined by the [NAIL API](40-nail-api.md). The helper contains references to the EduIDPlugin so both are needed for the EduID integration.

### Moodle Mobile app integration details

The following views have been changed to add the button 'ID app'. This button starts the Swiss EduID Mobile authorization and token exchange.

1.```www/core/components/login/templates/site.html```

2.```www/core/components/login/templates/credentials.html```

3.```www/core/components/login/templates/reconnect.html```

The following views have been changed by adding the management of the button 'ID app' pressing event. The service helper contains the code of the function called here.

1.```www/core/components/login/controllers/site.js```

2.```www/core/components/login/controllers/credentials.js```

3.```www/core/components/login/controllers/reconnect.js```


## Seting up the the environment

In order to set up the EduID authentication chain, after you have registered your Moodle instance on the EduID server, the following steps are needed.

1. Clone the [swisseduid](https://github.com/BLC-HTWChur/swisseduid) under MOODLE_ROOT/auth directory and complete the installation by visiting https://MOODLE_DOMAIN/admin/index.php page.

2. Build and install the [Eduid-iOS](https://github.com/BLC-HTWChur/EduID-iOS) on your iOS device. For the android use this repo EDUID_MOBILE_APP_REPO_FOR_ANDROID_TBD.

3. Clone the [moodlemobile2](https://github.com/BLC-HTWChur/moodlemobile2) and follow the instructions [here](http://docs.moodle.org/dev/Setting_up_your_development_environment_for_Moodle_Mobile_2) to set up the ionic environment for the Moodle mobile app.

4. Add the [EduIDPlugin](https://github.com/BLC-HTWChur/EduIDPlugin) to the [moodlemobile2](https://github.com/BLC-HTWChur/moodlemobile2) and build it.

5. Start the [moodlemobile2](https://github.com/BLC-HTWChur/moodlemobile2) on your device and press the 'ID App' button.

6. Select your Moodle instance in the EduID Mobile app.

7. At this point you should have a working session.


| [Previous: NAIL API](40-nail-api.md) | [Return to Architecture Overview](00-overview.md) | [Next: Operation Model](50-operational-model.md)|
| :---- | :----: | ----: |
