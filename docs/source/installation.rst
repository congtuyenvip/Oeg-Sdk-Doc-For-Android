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
      <string name="fb_login_protocol_scheme">fb3947760065490409</string>


       <!-- In-App-Purchase config -->
      <string name="developer_public_key">MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAziRDZJl5sDgGYqq7tZ3aceWjM0JiKRpBGowgFgYExPjcJgKankZBWaBpYWPZNuIA/NvEcFHvwmrkMPzENG3fVh8gHNzcWg5zmmiRPf645Ch3+uN1tG6AL0npWUXOTDbtseDuTXO0oFN8y7AF/VOlv4m6qA4xcsSVWmXQBX+OMM9eb3uGEv1zOgFIpYztAHME1TNuSQYzq/Mk7MFo/SAMfCbeHZqcViN9fI7xNhXTruStDlYX7Deb6qEQqRwt0AMfdAufLa5rrKzHl+sCwCQ3pAc0cWNs7pwePyF2HJbaoP9iq25uN740kevLYPLqreCkRevtzWZx+VfX0u2XLSKY3wIDAQAB</string>
      <string name="google_client_id">365803140846-7js7sr1eo02j08jf2fjvk4jr2t6lsspv.apps.googleusercontent.com</string>

      <!-- Game config -->
      <integer name="game_id">14</integer>

      <!-- environment config -->
      <bool name="debug_env">false</bool>
   </resources>

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


