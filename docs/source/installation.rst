Installation (Step by step)
====================================================================

Required Information
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Some information we must provided for the partner to use. Please ask for it if you don't see it. They are:

- OEG config file oeg_config.json
- Firebase google-services.json file
- SDK workflow and server api documentation
- Android keystore and password for it

Services
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
These Services may need to be enabled to allow the SDK to function

- Android In app Purchase
- Firebase
- Facebook Login
- Sentry (optional, for error tracking and logging)
- Adjust (optional, for tracking)

Import sdk
"""""""""""""""""

This can be easy done by adding implementation block to your app/game module

.. code-block::

   android {
      // ...
   }

   dependencies {
      // Replace <latest_version> with latest sdk version
      implementation("io.github.congtuyenvip:oeg-sdk:<latest_version>")
   }

add in setting.gradle.kts

.. code-block::

   dependencyResolutionManagement {
    repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
    repositories {
        google()
        mavenCentral()
        maven { url = uri("https://jitpack.io") }
    }
}



Configuration
"""""""""""""""""
- JSON configuration: Add ``oeg_config.json`` to your app's assets folder ``<app>/src/main/assets/oeg_config.json``.

.. code-block:: json

   {
     "coreConfig": {
       "gameId": 14,
       "apiBaseUrl": "https://dev-api-sdk-game.oeg.vn/",
       "debugEnv": false,
       "userInfoFullNameRequired": true,
       "userInfoBirthdayRequired": true,
       "userInfoEmailRequired": true,
       "userInfoPhoneRequired": true,
       "defaultNotificationChannelId": "default",
       "defaultNotificationTitle": "Notification"
     },
     "snsConfig": {
       "facebookAppId": "YOUR_FB_APP_ID",
       "facebookClientToken": "YOUR_FB_CLIENT_TOKEN",
       "fbLoginProtocolScheme": "fbYOUR_FB_APP_ID",
       "googleWebClientId": "YOUR_GOOGLE_WEB_CLIENT_ID"
     },
     "loggingConfig": {
       "sentryDsn": "https://your-sentry-dsn@sentry.io/project-id"
     },
     "trackingConfig": {
       "adjustAppToken": "your_adjust_token",
       "adjustEnvironment": "sandbox",
       "eventNameTokenMapping": {
         "login": "token_for_login",
         "registration": "token_for_register"
       }
     }
   }

**User Info Field Requirements**

The SDK allows you to configure which user information fields are required when users update their profile. This is done through boolean configuration values in ``oeg_config.json`` under ``coreConfig``:

- **``userInfoFullNameRequired``**: Controls whether the full name field is required (default: ``true``)
- **``userInfoBirthdayRequired``**: Controls whether the birthday field is required (default: ``true``)
- **``userInfoEmailRequired``**: Controls whether the email field is required (default: ``true``)
- **``userInfoPhoneRequired``**: Controls whether the phone number field is required (default: ``true``)

**Important Notes:**

- Username is always required and cannot be configured
- When a field is set as required (``true``), users must fill in and verify that field before they can save their profile
- When a field is set as optional (``false``), users can skip that field
- Empty string values in user information will be automatically converted to ``null`` when sent to the server
- Each required field has its own specific error message if validation fails

Sentry and Logging
"""""""""""""""""
The SDK integrates with `Sentry <https://sentry.io/>`_ for error tracking and logging. This integration is **optional** and can be enabled by providing a Sentry DSN in your configuration.

**Features:**
- Automatic error tracking and crash reporting
- Timber log integration - all Timber logs are automatically sent to Sentry
- Performance monitoring with transaction tracing
- Environment-based configuration (debug/production)

**Configuration:**
To enable Sentry, add your Sentry DSN to the ``sentryDsn`` field in ``loggingConfig`` object in ``oeg_config.json``. If the DSN is empty or not configured, Sentry initialization will be skipped and the SDK will continue to function normally.

**How it works:**
- The SDK uses `Timber <https://github.com/JakeWharton/timber>`_ for logging throughout the codebase
- When Sentry is configured, the SDK automatically integrates `sentry-android-timber` to forward all Timber logs to Sentry
- Logs are automatically tagged with the environment (debug or production) based on the ``debugEnv`` configuration
- Error tracking and performance monitoring are enabled automatically when Sentry is initialized

**Logging in your app:**
You can use Timber in your application code to log messages that will automatically be sent to Sentry (if configured):

.. code-block:: kotlin

   import timber.log.Timber

   // Log debug messages
   Timber.d("Debug message")

   // Log errors
   Timber.e(exception, "Error occurred")

   // Log warnings
   Timber.w("Warning message")

   // Log info
   Timber.i("Info message")

**Note:** If Sentry is not configured, Timber logs will still work locally using the default debug tree, but they won't be sent to Sentry.

- In your app initialization code (make sure your class in your <application android:name="<your app class name>">"

.. code-block::

        class <your app class name> : Application() {
            override fun onCreate() {
                super.onCreate()
                OegSdk.init(this)
                // Other init code
            }
         }

- **Importance: Make sure to add the app's fingerprint to Firebase's app settings to enable Google login.** Detail instructions are `here <https://support.google.com/firebase/answer/9137403?hl=en>`_                 

.. autosummary::
   :toctree: generated
