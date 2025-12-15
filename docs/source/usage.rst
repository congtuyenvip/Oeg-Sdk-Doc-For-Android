Usage
=====

Login and Register
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Call this method to show the authentication screen

.. code-block:: java
    
        // authenticationCallBack is interface which have 3 call back for login and register
        // onLoginResult(authenticationResult, provider) see funcation's docs
        // onRegisterResult(authenticationResult) see funcation's docs
        // onChangePassResult(changePasswordResult: WorkResult<Boolean>)
        // (orientation is optional for showing UI in prefer screen orientation ( PROTRAIT or LANDSCAPE), default is PORTRAIT
        // intentFlag is optional for intent flags
        OegSdk.showLogin(authenticationCallBack, intentFlag = null, orientation = OegSdk.ScreenOrientation.PORTRAIT)

which callback is:
        
.. code-block:: kotlin
    
        interface AuthenticationCallBack {

            /**
             * Call back when change password finish
             * @param authenticationResult: if succeed changePasswordResult.data() will return true else error can be obtained by changePasswordResult.error()
             */
            fun onChangePassResult(changePasswordResult: WorkResult<Boolean>)
            /**
             * Call back when login with or without sns finish
             * @param authenticationResult: if succeed authentication info can be obtained by authenticationResult.data() else error can be obtained by authenticationResult.error()
             * @param provider: if login without sns then it's null else it's string of provider for ex: "google" or "facebook"
             */
            fun onLoginResult(authenticationResult: WorkResult<AuthenticationInfo>, provider: String?)

            /**
             * Call back when login with or without sns finish
             * @param authenticationResult: if succeed authentication info can be obtained by authenticationResult.data() else error can be obtained by authenticationResult.error()
             */
            fun onRegisterResult(authenticationResult: WorkResult<AuthenticationInfo>)
        }

and WorkResult is:

.. code-block:: kotlin

        data class WorkResult<out T>(
            //[data] if succeed
            internal val data: T? = null, 
            //[error] if any error occurs
            internal val error: SdkError? = null
        ) 

with SdkError:


.. code-block:: kotlin

        class SdkError(
            // internal Sdk defined code
            val code: Int,
            // the error message
            message: String,
            // the origin error
            cause: Throwable? = null
        ) : Throwable(message, cause)



IAP
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Call this method to start the payment process

.. code-block:: java
    
        // gameData: an object contain user information
        // callback: a callback return the status and data of IAP flow
        // intentFlag: optional intent flags
        // (orientation is optional for showing UI in prefer screen orientation ( PROTRAIT or LANDSCAPE), default is PORTRAIT
        OegSdk.showPayment(gameData: GameData, callback: IabCallBack, intentFlag = null, orientation = OegSdk.ScreenOrientation.PORTRAIT)

.. code-block:: kotlin

        // the content of this class is depended on the partner
        @Parcelize
        open class GameData(
                @SerializedName("server_id")
                val serverId: Long,
                @SerializedName("level")
                val level: Long,
                @SerializedName("role_id")
                val roleId: String,
                @SerializedName("account_id")
                val accountId: String = "",
                @SerializedName("ext_info")
                val extInfo: String = "",
                @SerializedName("product_id")
                var productId: String = "",
                @SerializedName("platform")
                val platform: String = "Android"
        ) : Parcelable
        
.. code-block:: java

        interface IabCallBack {
            /**
             * Call back when purchase finish
             * @param iabResult: if succeed purchase info can be obtained by iabResult.data() else error can be obtained by iabResult.error(). Purchase information as described <a href="http://developer.android.com/google/play/billing/billing_reference.html#purchase-data-table">here</a>
             */
            fun onIabResult(iabResult: WorkResult<PurchaseInfo>)
        }

Change Password
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Call this method to show the change password screen

.. code-block:: java

        // callBack: AuthenticationCallBack
        // intentFlag: optional intent flags
        // orientation: optional screen orientation
        OegSdk.showChangePassword(callBack, intentFlag = null, orientation = OegSdk.ScreenOrientation.PORTRAIT)

Update User Info
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Call this method to show the update user info screen

.. code-block:: java

        // callBack: UserCallBack
        // requestCode: optional request code
        // intentFlag: optional intent flags
        // orientation: optional screen orientation
        OegSdk.showUpdateUserInfo(callBack, requestCode = REQUEST_CODE_PROFILING, intentFlag = null, orientation = OegSdk.ScreenOrientation.PORTRAIT)

        interface UserCallBack {
             /**
             * Call back when update info finish
             * @param result: if succeed data will return true else error can be obtained by result.error()
             */
            fun onUpdateInfoResult(result: WorkResult<Boolean>)
        }

**User Info Field Requirements Configuration**

You can configure which fields are required when users update their information by setting boolean values in ``sdk_conf.xml``:

- ``user_info_full_name_required``: Set to ``true`` to require full name (default: ``true``)
- ``user_info_birthday_required``: Set to ``true`` to require birthday (default: ``true``)
- ``user_info_email_required``: Set to ``true`` to require email (default: ``true``)
- ``user_info_phone_required``: Set to ``true`` to require phone number (default: ``true``)

**Note:** Username is always required and cannot be configured.

When a required field is missing or not verified, the SDK will display a specific error message:
- Email: "Please fill in and verify your email!"
- Phone: "Please fill in and verify your phone number!"

Empty strings in user information fields will be automatically converted to ``null`` when sent to the server.
    

Logout & Other method
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Use this method to clear the saved token and facebook credential when user want to switch to new account

.. code-block:: java

        OegSdk.logout()

Use this method to get stored ApiToken & stored uuid (return null if user haven't login yet)

.. code-block:: java

        OegSdk.getAuthenticationInfo()
