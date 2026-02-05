Usage
=====

Login and Register
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Call this method to show the authentication screen

.. code-block:: kotlin
    
        // authenticationCallBack is an interface which has 2 callbacks:
        // - onAuthenticationResult(action, authenticationResult, provider): called when login or register finishes
        // - onChangePassResult(changePasswordResult): called when change password finishes
        //
        // (orientation is optional for showing UI in preferred screen orientation (PORTRAIT or LANDSCAPE), default is PORTRAIT
        // intentFlag is optional for intent flags
        OegSdk.showLogin(authenticationCallBack, intentFlag = null, orientation = OegSdk.ScreenOrientation.PORTRAIT)

.. code-block:: java

        OegSdk.INSTANCE.showLogin(authenticationCallBack, null, OegSdk.ScreenOrientation.PORTRAIT);

which callback is:
        
.. code-block:: kotlin
    
        enum class AuthenticationAction {
            LOGIN,
            REGISTER
        }

        interface AuthenticationCallBack {
        
            /**
             * Call back when change password finish
             * @param changePasswordResult: if succeed changePasswordResult.data() will return true else error can be obtained by changePasswordResult.error()
             */
            fun onChangePassResult(changePasswordResult: WorkResult<Boolean>)

            /**
             * Call back when login or register finishes
             *
             * @param action: indicates whether this is a login or register action
             * @param authenticationResult: if succeed authentication info can be obtained by authenticationResult.data() else error can be obtained by authenticationResult.error()
             * @param provider: if login without SNS then it's null, else it's a string of provider, for example: "google" or "facebook". For REGISTER this is always null.
             */
            fun onAuthenticationResult(
                action: AuthenticationAction,
                authenticationResult: WorkResult<AuthenticationInfo>,
                provider: String?
            )
        }

.. code-block:: java

        public interface AuthenticationCallBack {
            void onChangePassResult(WorkResult<Boolean> changePasswordResult);
            void onAuthenticationResult(
                AuthenticationAction action,
                WorkResult<AuthenticationInfo> authenticationResult,
                String provider
            );
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

.. code-block:: kotlin
    
        // gameData: an object contain user information
        // callback: a callback return the status and data of IAP flow
        // intentFlag: optional intent flags
        // (orientation is optional for showing UI in prefer screen orientation ( PROTRAIT or LANDSCAPE), default is PORTRAIT
        OegSdk.showPayment(gameData: GameData, callback: IabCallBack, intentFlag = null, orientation = OegSdk.ScreenOrientation.PORTRAIT)

.. code-block:: java

        OegSdk.INSTANCE.showPayment(gameData, iabCallBack, null, OegSdk.ScreenOrientation.PORTRAIT);

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
        
.. code-block:: kotlin

        interface IabCallBack {
            /**
             * Call back when purchase finish
             * @param iabResult: if succeed purchase info can be obtained by iabResult.data() else error can be obtained by iabResult.error(). Purchase information as described <a href="http://developer.android.com/google/play/billing/billing_reference.html#purchase-data-table">here</a>
             */
            fun onIabResult(iabResult: WorkResult<PurchaseInfo>)
        }

.. code-block:: java

        public interface IabCallBack {
            void onIabResult(WorkResult<PurchaseInfo> iabResult);
        }

Change Password
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Call this method to show the change password screen

.. code-block:: kotlin

        // callBack: AuthenticationCallBack
        // intentFlag: optional intent flags
        // orientation: optional screen orientation
        OegSdk.showChangePassword(callBack, intentFlag = null, orientation = OegSdk.ScreenOrientation.PORTRAIT)

.. code-block:: java

        OegSdk.INSTANCE.showChangePassword(callBack, null, OegSdk.ScreenOrientation.PORTRAIT);

Update User Info
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Call this method to show the update user info screen

.. code-block:: kotlin

        // callBack: UserCallBack
        // requestCode: optional request code
        // intentFlag: optional intent flags
        // orientation: optional screen orientation
        OegSdk.showUpdateUserInfo(callBack, requestCode = REQUEST_CODE_PROFILING, intentFlag = null, orientation = OegSdk.ScreenOrientation.PORTRAIT)

.. code-block:: java

        OegSdk.INSTANCE.showUpdateUserInfo(callBack, REQUEST_CODE_PROFILING, null, OegSdk.ScreenOrientation.PORTRAIT);

        interface UserCallBack {
             /**
             * Call back when update info finish
             * @param result: if succeed data will return true else error can be obtained by result.error()
             */
            fun onUpdateInfoResult(result: WorkResult<Boolean>)
        }

.. code-block:: java

        public interface UserCallBack {
            void onUpdateInfoResult(WorkResult<Boolean> result);
        }

**User Info Field Requirements Configuration**

You can configure which fields are required when users update their information by setting boolean values in ``oeg_config.json`` (under ``coreConfig``):

- ``userInfoFullNameRequired``: Set to ``true`` to require full name (default: ``true``)
- ``userInfoBirthdayRequired``: Set to ``true`` to require birthday (default: ``true``)
- ``userInfoEmailRequired``: Set to ``true`` to require email (default: ``true``)
- ``userInfoPhoneRequired``: Set to ``true`` to require phone number (default: ``true``)

**Note:** Username is always required and cannot be configured.

When a required field is missing or not verified, the SDK will display a specific error message:
- Email: "Please fill in and verify your email!"
- Phone: "Please fill in and verify your phone number!"

Empty strings in user information fields will be automatically converted to ``null`` when sent to the server.

Logging Events
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Call this method to log custom events. The SDK automatically appends default parameters like ``app_id`` and ``user_name``.

.. code-block:: kotlin

        // eventName: String name of the event
        // params: Map<String, Any> of event parameters
        OegSdk.logEvent("event_name", mapOf("param1" to "value1"))

.. code-block:: java

        Map<String, Object> params = new HashMap<>();
        params.put("param1", "value1");
        OegSdk.INSTANCE.logEvent("event_name", params);
    
Logging Purchase
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Call this method to log purchase events.

.. code-block:: kotlin

        // price: Double - The amount of the purchase
        // currency: String - The currency code (e.g., "USD")
        // transactionId: String? - Optional transaction/order ID
        // productId: String? - Optional product ID
        // purchaseData: Map<String, Any>? - Optional additional purchase data
        OegSdk.logPurchase(9.99, "USD", "TRANS_123456", "prod_123", mapOf("item_category" to "gold"))

.. code-block:: java

        Map<String, Object> purchaseData = new HashMap<>();
        purchaseData.put("item_category", "gold");
        OegSdk.INSTANCE.logPurchase(9.99, "USD", "TRANS_123456", "prod_123", purchaseData);
    

Logout & Other method
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Use this method to clear the saved token and facebook credential when user want to switch to new account

.. code-block:: kotlin

        OegSdk.logout()

.. code-block:: java

        OegSdk.INSTANCE.logout();

Use this method to get stored ApiToken & stored uuid (return null if user haven't login yet)

.. code-block:: kotlin

        OegSdk.getAuthenticationInfo()

.. code-block:: java

        OegSdk.INSTANCE.getAuthenticationInfo();

Use this method to get saved user information, or fall back to minimal information from authentication if user info has not been fetched yet (return null if user haven't login yet)

.. code-block:: kotlin

        OegSdk.getSavedUserInfo()

.. code-block:: java

        OegSdk.INSTANCE.getSavedUserInfo();
