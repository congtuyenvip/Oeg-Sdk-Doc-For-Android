Firebase
====================================================================
Import config file
"""""""""""""""""

- Import the google-services.json to your app or app's source folder

Configuration
"""""""""""""""""
- Resource configuration:

    + Add and change fbconf.xml 's value base on your config into <app>/src/<*>/res/values:

.. code-block:: xml
     
    <string name="default_notification_channel_id">default</string>
    <string name="default_notification_title">Notification</string>
    <string name="facebook_app_id">789654061565931</string>
    <string name="fb_login_protocol_scheme">fb789654061565931</string>
    
    + Add your notification with reouce identify named: "ic_stat_ic_notification" to your drawable folder. This notification icon will be used to show on status bar
    
    + Change your notication color
    
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
        
        