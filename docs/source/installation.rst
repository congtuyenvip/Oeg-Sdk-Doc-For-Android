Installation (Step by step)
====================================================================

Required Information
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Some information we must provided for the partner to use. Please ask for it if you don't see it. They are:

- Facebook config gile sdk_conf.xml
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
- Resource configuration: Add and change sdk_conf.xml 's value base on your config into <app>/src/<*>/res/values. For example

.. code-block::

   <?xml version="1.0" encoding="utf-8"?>
   <resources>
      <!-- for push notification -->
      <string name="default_notification_channel_id">default</string>
      <string name="default_notification_title">Notification</string>
      <!-- Facebook login config -->
      <string name="facebook_app_id">3947760065490409</string>
      <string name="facebook_client_token">your_facebook_client_token</string>
      <string name="fb_login_protocol_scheme">fb3947760065490409</string>

       <!-- In-App-Purchase config -->
      <string name="developer_public_key">MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAziRDZJl5sDgGYqq7tZ3aceWjM0JiKRpBGowgFgYExPjcJgKankZBWaBpYWPZNuIA/NvEcFHvwmrkMPzENG3fVh8gHNzcWg5zmmiRPf645Ch3+uN1tG6AL0npWUXOTDbtseDuTXO0oFN8y7AF/VOlv4m6qA4xcsSVWmXQBX+OMM9eb3uGEv1zOgFIpYztAHME1TNuSQYzq/Mk7MFo/SAMfCbeHZqcViN9fI7xNhXTruStDlYX7Deb6qEQqRwt0AMfdAufLa5rrKzHl+sCwCQ3pAc0cWNs7pwePyF2HJbaoP9iq25uN740kevLYPLqreCkRevtzWZx+VfX0u2XLSKY3wIDAQAB</string>
      <string name="google_web_client_id">365803140846-7js7sr1eo02j08jf2fjvk4jr2t6lsspv.apps.googleusercontent.com</string>

      <!-- Game config -->
      <integer name="game_id">14</integer>
      <string name="api_base_url">https://dev-api-sdk-game.oeg.vn/</string>

      <!-- Logging and Monitoring -->
      <!-- Sentry DSN for error tracking and logging (optional, leave empty if not using Sentry) -->
      <string name="sentry_dsn" translatable="false">https://your-sentry-dsn@sentry.io/project-id</string>

      <!-- environment config -->
      <bool name="debug_env">false</bool>

      <!-- User info field requirements -->
      <!-- Configure which fields are required when updating user information -->
      <!-- Set to true to make the field required, false to make it optional -->
      <bool name="user_info_full_name_required">true</bool>
      <bool name="user_info_birthday_required">true</bool>
      <bool name="user_info_email_required">true</bool>
      <bool name="user_info_phone_required">true</bool>
   </resources>

**User Info Field Requirements**

The SDK allows you to configure which user information fields are required when users update their profile. This is done through boolean configuration values in ``sdk_conf.xml``:

- **``user_info_full_name_required``**: Controls whether the full name field is required (default: ``true``)
- **``user_info_birthday_required``**: Controls whether the birthday field is required (default: ``true``)
- **``user_info_email_required``**: Controls whether the email field is required (default: ``true``)
- **``user_info_phone_required``**: Controls whether the phone number field is required (default: ``true``)

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
To enable Sentry, add your Sentry DSN to the ``sentry_dsn`` string resource in ``sdk_conf.xml`` (as shown in the Configuration section above). If the DSN is empty or not configured, Sentry initialization will be skipped and the SDK will continue to function normally.

**How it works:**
- The SDK uses `Timber <https://github.com/JakeWharton/timber>`_ for logging throughout the codebase
- When Sentry is configured, the SDK automatically integrates `sentry-android-timber` to forward all Timber logs to Sentry
- Logs are automatically tagged with the environment (debug or production) based on the ``debug_env`` configuration
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


