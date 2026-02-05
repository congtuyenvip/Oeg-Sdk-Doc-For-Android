Firebase
====================================================================
Import config file
"""""""""""""""""

- Import the google-services.json to your app or app's source folder

Configuration
"""""""""""""""""
- JSON configuration:

    + Configure Notification and Facebook details in ``oeg_config.json`` inside ``src/main/assets/``:

.. code-block:: json

    {
      "coreConfig": {
        "defaultNotificationChannelId": "default",
        "defaultNotificationTitle": "Notification"
      },
      "snsConfig": {
        "facebookAppId": "YOUR_FB_APP_ID",
        "fbLoginProtocolScheme": "fbYOUR_FB_APP_ID"
      }
    }

    + Add your notification with resource identify named: "ic_stat_ic_notification" to your drawable folder. This notification icon will be used to show on status bar

    + Change your notification color

.. code-block:: xml

    <color name="notificationIconColor">#04aa46</color>

- Unregister/Register on new firebase token call back whenever firebase token changes then this call back is called

.. code-block:: java

        OegSdk.registerNewPushDeviceTokenCallback(newTokenCallback)
        OegSdk.unregisterNewPushDeviceTokenCallback()
- Get push notification - firebase token

.. code-block:: java

        OegSdk.getPushDeviceToken(callback)

- Register callback when received push notification message. By default it will show notification with title and body data

.. code-block:: java

        OegSdk.registerOnPushNotificationMessageReceivedCallback(callback)