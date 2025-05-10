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
        OegSdk.showLogin(authenticationCallBack, orientation = OegSdk.ScreenOrientation.PORTRAIT)

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
        // (orientation is optional for showing UI in prefer screen orientation ( PROTRAIT or LANDSCAPE), default is PORTRAIT
        OegSdk.showPayment(gameData: GameData, callback: IabCallBack, orientation = OegSdk.ScreenOrientation.PORTRAIT)

.. code-block:: kotlin

        // the content of this class is depended on the partner
        @Parcelize
        open class GameData(
            @SerializedName("server_id")
            val serverId: Int,
            @SerializedName("level")
            val level: Int,
            @SerializedName("role_id")
            val roleId: Int,
            @SerializedName("account_id")
            val accountId: Int = 0
        ) : Parcelable
        
.. code-block:: java

        interface IabCallBack {
            /**
             * Call back when purchase finish
             * @param iabResult: if succeed purchase info can be obtained by iabResult.data() else error can be obtained by iabResult.error(). Purchase information as described <a href="http://developer.android.com/google/play/billing/billing_reference.html#purchase-data-table">here</a>
             */
            fun onIabResult(iabResult: WorkResult<PurchaseInfo>)
        }
    

Logout & Other method
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Use this method to clear the saved token and facebook credential when user want to switch to new account

.. code-block:: java

        OegSdk.logOut()

Use this method to get stored ApiToken & stored uuid (return null if user haven't login yet)

.. code-block:: java

        OegSdk.getAuthenticationInfo()
