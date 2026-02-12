## Game > Gamebase > Error Code

## Client SDK

| Category        | Platform           | Error                                    | Error Code | Description                                    |
| --------------- | ------------------ | ---------------------------------------- | ---------- | ---------------------------------------- |
| Common          | Android<br/>Unity<br/>iOS | NOT_INITIALIZED<br/>TCGB_ERROR_NOT_INITIALIZED | 1          | Gamebase is not initialized.                 |
|                 | Android<br/>Unity<br/>iOS | NOT_LOGGED_IN<br/>TCGB_ERROR_NOT_LOGGED_IN | 2          | Sign in is required.                              |
|                 | Android<br/>Unity<br/>iOS | INVALID_PARAMETER<br/>TCGB_ERROR_INVALID_PARAMETER | 3          | Invalid parameter.                             |
|                 | Android<br/>Unity<br/>iOS | INVALID_JSON_FORMAT<br/>TCGB_ERROR_INVALID_JSON_FORMAT | 4          | JSON format error.                           |
|                 | Android<br/>Unity<br/>iOS | USER_PERMISSION<br/>TCGB_ERROR_USER_PERMISSION | 5          | You don't have permission.                                |
|                 | Android<br/>Unity<br/>iOS | INVALID_MEMBER<br/>TCGB_ERROR_INVALID_MEMBER  | 6          | The request is for the wrong member.                              |
|                 | Android<br/>Unity<br/>iOS | BANNED_MEMBER<br/>TCGB_ERROR_BANNED_MEMBER   | 7         | Sanctioned member.                                |
|                 | Android<br/>Unity<br/>iOS | NOT_SUPPORTED<br/>TCGB_ERROR_NOT_SUPPORTED | 10         | This feature is not supported.                           |
|                 | Unity<br/>iOS             | NOT_SUPPORTED_ANDROID<br/>TCGB_ERROR_NOT_SUPPORTED_ANDROID | 11         | This feature is not supported on Android.                 |
|                 | Unity<br/>iOS             | NOT_SUPPORTED_IOS<br/>TCGB_ERROR_NOT_SUPPORTED_IOS | 12         | This feature is not supported on iOS.                     |
|                 | Unity                     | NOT_SUPPORTED_UNITY_EDITOR            | 13         | This feature is not supported by the Editor.                  |
|                 | Unity                     | NOT_SUPPORTED_UNITY_STANDALONE        | 14         | This feature is not supported in Standalone.              |
|                 | Unity                     | NOT_SUPPORTED_UNITY_WEBGL             | 15         | This feature is not supported by WebGL.                   |
|                 | Android                   | ANDROID_ACTIVITY_DESTROYED             | 31         | The activity was force terminated.                       |
|                 | Android                   | ANDROID_ACTIVEAPP_NOT_CALLED          | 32         | The activeApp API was not called.                 |
|                 | iOS                       | TCGB_ERROR_IOS_GAMECENTER_DENIED                    | 51         | Gamecenter login was denied.                 |
|                 | iOS                       | TCGB_ERROR_IOS_CANNOT_OPEN_URL                    | 52         | Your app can't handle URL schemes.                 |
| Network(Socket) | Android<br/>Unity<br/>iOS | SOCKET_RESPONSE_TIMEOUT<br/>TCGB_ERROR_SOCKET_RESPONSE_TIMEOUT | 101        | No response due to unstable network conditions.                 |
|                 | Android<br/>Unity<br/>iOS | SOCKET_ERROR<br/>TCGB_ERROR_SOCKET_ERROR | 110        | Socket error.                                    |
|                 | Android<br/>Unity<br/>iOS | SOCKET_UNKNOWN_ERROR<br/>TCGB_ERROR_SOCKET_UNKNOWN_ERROR | 999        | Unknown socket error.                             |
| Launching       | Android<br/>Unity<br/>iOS | LAUNCHING_SERVER_ERROR<br/>TCGB_ERROR_LAUNCHING_SERVER_ERROR | 2001       | Launch server error.                             |
|                 | Android<br/>Unity<br/>iOS | LAUNCHING_NOT_EXIST_CLIENT_ID<br/>TCGB_ERROR_LAUNCHING_NOT_EXIST_CLIENT_ID | 2002       | Client ID is missing.                    |
|                 | Android<br/>Unity<br/>iOS | LAUNCHING_UNREGISTERED_APP<br/>TCGB_ERROR_LAUNCHING_UNREGISTERED_APP | 2003       | This is an unregistered app.                         |
|                 | Android<br/>Unity<br/>iOS | LAUNCHING_UNREGISTERED_CLIENT<br/>TCGB_ERROR_LAUNCHING_UNREGISTERED_CLIENT | 2004       | Unregistered client (version).            |
| Auth            | Android<br/>Unity<br/>iOS | AUTH_USER_CANCELED<br/>TCGB_ERROR_AUTH_USER_CANCELED | 3001       | Your login has been canceled.                            |
|                 | Android<br/>Unity<br/>iOS | AUTH_NOT_SUPPORTED_PROVIDER<br/>TCGB_ERROR_AUTH_NOT_SUPPORTED_PROVIDER | 3002       | This authentication method is not supported.                        |
|                 | Android<br/>Unity<br/>iOS | AUTH_NOT_EXIST_MEMBER<br/>TCGB_ERROR_AUTH_NOT_EXIST_MEMBER | 3003       | A member that doesn't exist or has left.                      |
|                 | Android<br/>Unity<br/>iOS | AUTH_EXTERNAL_LIBRARY_INITIALIZATION_ERROR<br/>TCGB_ERROR_AUTH_EXTERNAL_LIBRARY_INITIALIZATION_ERROR | 3006       | Failed to initialize external authentication library.                       |
|                 | Android<br/>Unity<br/>iOS | AUTH_EXTERNAL_LIBRARY_ERROR<br/>TCGB_ERROR_AUTH_EXTERNAL_LIBRARY_ERROR | 3009       | External authentication library error.                       |
|                 | Android<br/>Unity<br/>iOS | AUTH_ALREADY_IN_PROGRESS_ERROR<br/>TCGB_ERROR_AUTH_ALREADY_IN_PROGRESS_ERROR | 3010       | The previous authentication process did not complete.                 |
|                 | Android<br/>Unity<br/>iOS | AUTH_INVALID_GAMEBASE_TOKEN<br/>TCGB_ERROR_AUTH_INVALID_GAMEBASE_TOKEN | 3011       | You were logged out because your Gamebase Access Token is invalid.<br/>Try signing in again. |
|                 | Android<br/>Unity<br/>iOS | AUTH_AUTHENTICATION_SERVER_ERROR<br/>TCGB_ERROR_AUTH_AUTHENTICATION_SERVER_ERROR | 3012       | An error occurred from the authentication server. |
| TransferAccount | Android<br/>Unity<br/>iOS | SAME_REQUESTOR<br/>TCGB_ERROR_SAME_REQUESTOR | 8 | The issued TransferAccount was used on the same terminal. |
|                 | Android<br/>Unity<br/>iOS | NOT_GUEST_OR_HAS_OTHERS<br/>TCGB_ERROR_NOT_GUEST_OR_HAS_OTHERS | 9 | You tried to transfer from a non-Guest account, or your account is associated with a non-Guest IdP. |
|                 | Android<br/>Unity<br/>iOS | AUTH_TRANSFERACCOUNT_EXPIRED<br/>TCGB_ERROR_AUTH_TRANSFERACCOUNT_EXPIRED | 3041 | The TransferAccount has expired. |
|                 | Android<br/>Unity<br/>iOS | AUTH_TRANSFERACCOUNT_BLOCK<br/>TCGB_ERROR_AUTH_TRANSFERACCOUNT_BLOCK | 3042 | The account transfer feature is locked because you entered an invalid TransferAccount multiple times. |
|                 | Android<br/>Unity<br/>iOS | AUTH_TRANSFERACCOUNT_INVALID_ID<br/>iCGB_ERROR_AUTH_TRANSFERACCOUNT_INVALID_ID | 3043 | TransferAccount's Id is invalid. |
|                 | Android<br/>Unity<br/>iOS | AUTH_TRANSFERACCOUNT_INVALID_PASSWORD<br/>TCGB_ERROR_AUTH_TRANSFERACCOUNT_INVALID_PASSWORD | 3044 | The Password for TransferAccount is invalid. |
|                 | Android<br/>Unity<br/>iOS | AUTH_TRANSFERACCOUNT_CONSOLE_NO_CONDITION<br/>TCGB_ERROR_AUTH_TRANSFERACCOUNT_CONSOLE_NO_CONDITION | 3045 | TransferAccount is not set. <br/> Set up TransferAccount in the NHN Cloud Gamebase console first. |
|                 | Android<br/>Unity<br/>iOS | AUTH_TRANSFERACCOUNT_NOT_EXIST<br/>TCGB_ERROR_AUTH_TRANSFERACCOUNT_NOT_EXIST | 3046 | The TransferAccount doesn't exist, please get a TransferAccount first. |
|                 | Android<br/>Unity<br/>iOS | AUTH_TRANSFERACCOUNT_ALREADY_EXIST_ID<br/>TCGB_ERROR_AUTH_TRANSFERACCOUNT_ALREADY_EXIST_ID | 3047 | TransferAccount already exists. |
|                 | Android<br/>Unity<br/>iOS | AUTH_TRANSFERACCOUNT_ALREADY_USED<br/>TCGB_ERROR_AUTH_TRANSFERACCOUNT_ALREADY_USED | 3048 | TransferAccount is already used. |
| Auth (Login)    | Android<br/>Unity<br/>iOS | AUTH_TOKEN_LOGIN_FAILED<br/>TCGB_ERROR_AUTH_TOKEN_LOGIN_FAILED | 3101       | Token login failed.                         |
|                 | Android<br/>Unity<br/>iOS | AUTH_TOKEN_LOGIN_INVALID_TOKEN_INFO<br/>TCGB_ERROR_AUTH_TOKEN_LOGIN_INVALID_TOKEN_INFO | 3102       | The token information is invalid.                        |
|                 | Android<br/>Unity<br/>iOS | AUTH_TOKEN_LOGIN_INVALID_LAST_LOGGED_IN_IDP<br/>TCGB_ERROR_AUTH_TOKEN_LOGIN_INVALID_LAST_LOGGED_IN_IDP | 3103       | I don't have the IdP information for the last time I signed in.                   |
| IDP Login       | Android<br/>Unity<br/>iOS | AUTH_IDP_LOGIN_FAILED<br/>TCGB_ERROR_AUTH_IDP_LOGIN_FAILED | 3201       | IdP login failed.                        |
|                 | Android<br/>Unity<br/>iOS | AUTH_IDP_LOGIN_INVALID_IDP_INFO<br/>TCGB_ERROR_AUTH_IDP_LOGIN_INVALID_IDP_INFO | 3202       | The IdP information is invalid (there is no corresponding IdP information in the console). |
|                 | Unity<br/>iOS             | AUTH_IDP_LOGIN_EXTERNAL_AUTHENTICATION_REQUIRED<br/>TCGB_ERROR_AUTH_IDP_LOGIN_EXTERNAL_AUTHENTICATION_REQUIRED | 3203       | You must be logged in to your IdP before requesting a Gamebase login. |
| Add Mapping     | Android<br/>Unity<br/>iOS | AUTH_ADD_MAPPING_FAILED<br/>TCGB_ERROR_AUTH_ADD_MAPPING_FAILED | 3301       | Adding a mapping failed.                          |
|                 | Android<br/>Unity<br/>iOS | AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER<br/>TCGB_ERROR_AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER | 3302       | It's already mapped to another member.                      |
|                 | Android<br/>Unity<br/>iOS | AUTH_ADD_MAPPING_ALREADY_HAS_SAME_IDP<br/>TCGB_ERROR_AUTH_ADD_MAPPING_ALREADY_HAS_SAME_IDP | 3303       | It's already mapped to the same IdP.                     |
|                 | Android<br/>Unity<br/>iOS | AUTH_ADD_MAPPING_INVALID_IDP_INFO<br/>TCGB_ERROR_AUTH_ADD_MAPPING_INVALID_IDP_INFO | 3304       | The IdP information is invalid (there is no corresponding IdP information in the console). |
|                 | Android<br/>Unity<br/>iOS | AUTH_ADD_MAPPING_CANNOT_ADD_GUEST_IDP<br/>TCGB_ERROR_AUTH_ADD_MAPPING_CANNOT_ADD_GUEST_IDP | 3305       | AddMapping is not possible with guest IdPs. |
| Add Mapping Forcibly | Android<br/>Unity<br/>iOS | AUTH_ADD_MAPPING_FORCIBLY_NOT_EXIST_KEY<br/>TCGB_ERROR_AUTH_ADD_MAPPING_FORCIBLY_NOT_EXIST_KEY | 3311       | The ForcingMappingKey does not exist. <br/>ForcingMappingTicket again. |
|                 | Android<br/>Unity<br/>iOS | AUTH_ADD_MAPPING_FORCIBLY_ALREADY_USED_KEY<br/>TCGB_ERROR_AUTH_ADD_MAPPING_FORCIBLY_ALREADY_USED_KEY | 3312       | The ForcingMappingKey has already been used. |
|                 | Android<br/>Unity<br/>iOS | AUTH_ADD_MAPPING_FORCIBLY_EXPIRED_KEY<br/>TCGB_ERROR_AUTH_ADD_MAPPING_FORCIBLY_EXPIRED_KEY | 3313       | The ForcingMappingKey has expired. |
|                 | Android<br/>Unity<br/>iOS | AUTH_ADD_MAPPING_FORCIBLY_DIFFERENT_IDP<br/>TCGB_ERROR_AUTH_ADD_MAPPING_FORCIBLY_DIFFERENT_IDP | 3314       | The ForcingMappingKey was used for a different IdP. <br/>The ForcingMappingKey issued is used to attempt to force mapping to the same IdP. |
|                 | Android<br/>Unity<br/>iOS | AUTH_ADD_MAPPING_FORCIBLY_DIFFERENT_AUTHKEY<br/>TCGB_ERROR_AUTH_ADD_MAPPING_FORCIBLY_DIFFERENT_AUTHKEY | 3315       | The ForcingMappingKey was used for a different account. <br/>The ForcingMappingKey issued is used to attempt to force mapping to the same IdP and account. |
| Remove Mapping  | Android<br/>Unity<br/>iOS | AUTH_REMOVE_MAPPING_FAILED<br/>TCGB_ERROR_AUTH_REMOVE_MAPPING_FAILED | 3401       | Deleting a mapping failed.                          |
|                 | Android<br/>Unity<br/>iOS | AUTH_REMOVE_MAPPING_LAST_MAPPED_IDP<br/>TCGB_ERROR_AUTH_REMOVE_MAPPING_LAST_MAPPED_IDP | 3402       | You can't delete the last mapped IdP.                |
|                 | Android<br/>Unity<br/>iOS | AUTH_REMOVE_MAPPING_LOGGED_IN_IDP<br/>TCGB_ERROR_AUTH_REMOVE_MAPPING_LOGGED_IN_IDP | 3403       | The IdP you're currently logged into.                      |
| Logout          | Android<br/>Unity<br/>iOS | AUTH_LOGOUT_FAILED<br/>TCGB_ERROR_AUTH_LOGOUT_FAILED | 3501       | Logout failed.                           |
| Withdrawal      | Android<br/>Unity<br/>iOS | AUTH_WITHDRAW_FAILED<br/>TCGB_ERROR_AUTH_WITHDRAW_FAILED | 3601       | Unsubscribe failed.                             |
|                 | Android<br/>Unity<br/>iOS | AUTH_WITHDRAW_ALREADY_TEMPORARY_WITHDRAW<br/>TCGB_ERROR_AUTH_WITHDRAW_ALREADY_TEMPORARY_WITHDRAW | 3602   | The user has already requested a temporary withdrawal.                    |
|                 | Android<br/>Unity<br/>iOS | AUTH_WITHDRAW_NOT_TEMPORARY_WITHDRAW<br/>TCGB_ERROR_AUTH_WITHDRAW_NOT_TEMPORARY_WITHDRAW | 3603       | You are not the user who requested temporary withdrawal.                     |
| Not Playable    | Android<br/>Unity<br/>iOS | AUTH_NOT_PLAYABLE<br/>TCGB_ERROR_AUTH_NOT_PLAYABLE | 3701       | Unplayable state (such as maintenance or out of service).        |
| Auth(Unknown)   | Android<br/>Unity<br/>iOS | AUTH_UNKNOWN_ERROR<br/>TCGB_ERROR_AUTH_UNKNOWN_ERROR | 3999       | Unknown error (undefined error).           |
| Purchase        | Android<br/>Unity<br/>iOS | PURCHASE_NOT_INITIALIZED<br/>TCGB_ERROR_PURCHASE_NOT_INITIALIZED | 4001       | The Gamebase PurchaseAdapter was not initialized.   |
|                 | Android<br/>Unity<br/>iOS | PURCHASE_USER_CANCELED<br/>TCGB_ERROR_PURCHASE_USER_CANCELED | 4002       | Your purchase has been canceled.                             |
|                 | Android<br/>Unity<br/>iOS | PURCHASE_NOT_FINISHED_PREVIOUS_PURCHASING<br/>TCGB_ERROR_PURCHASE_NOT_FINISHED_PREVIOUS_PURCHASING | 4003       | The previous purchase was not completed.                       |
|                 | Unity                     | PURCHASE_NOT_ENOUGH_CASH                                        | 4004       | Unable to checkout because the store is low on cache (Unity only) |
|                 | Android<br/>Unity<br/>iOS | PURCHASE_INACTIVE_PRODUCT_ID<br/>TCGB_ERROR_PURCHASE_INACTIVE_PRODUCT_ID | 4005       | The product is not active.  |
|                 | Android<br/>Unity<br/>iOS | PURCHASE_NOT_EXIST_PRODUCT_ID<br/>TCGB_ERROR_PURCHASE_NOT_EXIST_PRODUCT_ID | 4006       | Payment was requested with a GamebaseProductID that doesn't exist. |
|                 | Android<br/>Unity<br/>iOS | PURCHASE_LIMIT_EXCEEDED<br/>TCGB_ERROR_PURCHASE_LIMIT_EXCEEDED | 4007       | You've exceeded your monthly purchase limit. |
|                 | Android<br/>Unity<br/>Unreal<br/>iOS | PURCHASE\_PENDING<br/>TCGB\_ERROR\_PURCHASE\_PENDING | 4008       | 결제를 완료하려면 추가 확인이 필요합니다. |
|                 | Android<br/>Unity<br/>iOS | PURCHASE_NOT_SUPPORTED_MARKET<br/>TCGB_ERROR_PURCHASE_NOT_SUPPORTED_MARKET | 4010       | This store is not supported.                          |
|                 | Android<br/>Unity<br/>iOS | PURCHASE_EXTERNAL_LIBRARY_ERROR<br/>TCGB_ERROR_PURCHASE_EXTERNAL_LIBRARY_ERROR | 4201       | External IAP library error.                      |
|                 | Android<br/>Unity<br/>iOS | PURCHASE_UNKNOWN_ERROR<br/>TCGB_ERROR_PURCHASE_UNKNOWN_ERROR | 4999       | Unknown purchase error.                           |
| Push            | Android<br/>Unity<br/>iOS | PUSH_EXTERNAL_LIBRARY_ERROR<br/>TCGB_ERROR_PUSH_EXTERNAL_LIBRARY_ERROR | 5101       | External library error.                          |
|                 | Android<br/>Unity<br/>iOS | PUSH_ALREADY_IN_PROGRESS_ERROR<br/>TCGB_ERROR_PUSH_ALREADY_IN_PROGRESS_ERROR | 5102       | The previous push API call did not complete.              |
|                 | Android<br/>Unity<br/>iOS | PUSH_UNKNOWN_ERROR<br/>TCGB_ERROR_PUSH_UNKNOWN_ERROR | 5999       | Unknown push error (undefined push error).      |
| UI              | Android<br/>Unity<br/>iOS | UI_IMAGE_NOTICE_TIMEOUT<br/>TCGB_ERROR_UI_IMAGE_NOTICE_TIMEOUT | 6901       | Image announcement timed out while displaying.            |
|                 | Android<br/>Unity         | UI_IMAGE_NOTICE_NOT_SUPPORTED_OS | 6902       | For rolling types, image announcements are not supported on devices with API 19 or lower.            |
|                 | Android<br/>Unity<br/>iOS | UI_CONTACT_FAIL_INVALID_URL<br/>TCGB_ERROR_UI_CONTACT_FAIL_INVALID_URL | 6911       | Failed to generate the contact center webview URL.            |
|                 | Android<br/>Unity<br/>iOS | UI_CONTACT_FAIL_FAIL_ISSUE_SHORT_TERM_TICKET<br/>TCGB_ERROR_UI_CONTACT_FAIL_ISSUE_SHORT_TERM_TICKET | 6912       | Failed to issue a temporary ticket to identify the user.            |
|                 | Android<br/>Unity<br/>iOS | UI_TERMS_NOT_EXIST_IN_CONSOLE<br/>TCGB_ERROR_UI_TERMS_NOT_EXIST_IN_CONSOLE | 6921       | Terms information is not registered in the console. |
|                 | Android<br/>Unity<br/>iOS | UI_TERMS_NOT_EXIST_FOR_DEVICE_COUNTRY<br/>TCGB_ERROR_UI_TERMS_NOT_EXIST_FOR_DEVICE_COUNTRY | 6922       | The terms and conditions information for your device's country code is not registered in the console. |
|                 | Android<br/>Unity<br/>iOS | UI_TERMS_UNREGISTERED_SEQ<br/>TCGB_ERROR_UI_TERMS_UNREGISTERED_SEQ | 6923       | The value of the unregistered terms Seq.            |
|                 | Android<br/>Unity<br/>iOS | UI_TERMS_ALREADY_IN_PROGRESS_ERROR<br/>TCGB_ERROR_UI_TERMS_ALREADY_IN_PROGRESS_ERROR | 6924       | The Terms API call has not yet completed.<br/>Try again in a few minutes. |
|                 | Android<br/>Unity         | UI_TERMS_ANDROID_DUPLICATED_VIEW | 6925       | The Terms webview was invoked again without exiting. |
|                 | Android<br/>Unity<br/>iOS | UI\_GAME\_NOTICE\_FAIL\_INVALID\_URL<br/>TCGB\_ERROR\_UI\_GAME\_NOTICE\_FAIL\_INVALID\_URL       | 6941 | Failed to generate the game notice URL. |
|                 | Android<br/>Unity         | UI\_GAME\_NOTICE\_FAIL\_ANDROID\_DUPLICATED\_VIEW | 6942       | The game notice was called again before the previous popup was closed. |
|                 | Android<br/>Unity<br/>iOS | UI_UNKNOWN_ERROR<br/>TCGB_ERROR_UI_UNKNOWN_ERROR | 6999       | Unknown error (undefined error).            |
| WebView         | Android<br/>Unity<br/>iOS | WEBVIEW_INVALID_URL<br/>TCGB_ERROR_WEBVIEW_INVALID_URL           | 7001       | Invalid URL.            |
|                 | Android<br/>Unity<br/>iOS | WEBVIEW_TIMEOUT<br/>TCGB_ERROR_WEBVIEW_TIMEOUT                     | 7002       | The webview timed out while displaying.            |
|                 | Android<br/>Unity<br/>iOS | WEBVIEW_HTTP_ERROR<br/>TCGB_ERROR_WEBVIEW_HTTP_ERROR 	        | 7003       | Failed to display the webview due to an HTTP error.            |
|                 | Android<br/>Unity         | WEBVIEW_OPENED_NEW_BROWSER_BEFORE_CLOSE                           | 7004       | Displayed a new webview before exiting the Browser-like webview. |
|                 | Unity                     | WEBVIEW_UNKNOWN_ERROR | 7999       | An unknown error occurred when calling the webview (undefined error).            |
| Server          | Android<br/>Unity<br/>iOS | SERVER_INTERNAL_ERROR<br/>TCGB_ERROR_SERVER_INTERNAL_ERROR | 8001       | Server internal errors                                 |
|                 | Android<br/>Unity<br/>iOS | SERVER_REMOTE_SYSTEM_ERROR<br/>TCGB_ERROR_SERVER_REMOTE_SYSTEM_ERROR | 8002       | The server encountered an error during external integration.                        |
|                 | Android<br/>Unity<br/>iOS | SERVER_INVALID_RESPONSE<br/>TCGB_ERROR_SERVER_INVALID_RESPONSE | 8003       | The server returned an invalid response.                        |
|                 | Android<br/>Unity<br/>iOS | SERVER_UNKNOWN_ERROR<br/>TCGB_ERROR_SERVER_UNKNOWN_ERROR | 8999       | An unknown error occurred on the server.                           |

<br/>
<br/>

## Server
| Module  | Error Code            | Description                              |
| ------- | --------------------- | ---------------------------------------- |
| Common  | -4000001<br/>-4000006 | Called API with invalid parameter type. <br/>e.g) The parameter is declared in the int type, but API has been called with string-format data. |
|         | -4000002<br/>-4000004<br/>-4000005 | Required parameter is missing or called with an inappropriate value. |
|         | -4000003              | Undefined value has been delivered to request body. |
|         | -4000007              | Called the API version no longer supported |
|         | -4000008              | Exceeds parameter length |
|         | -4010001              | Called invalid App ID. |
|         | -4010002              | Called invalid Appkey. |
|         | -4010003              | Unauthenticated client called an API which requires authentication. |
|         | -4010004              | Called invalid secret key. |
|         | -4060001              | Set invalid content-type in the HTTP header. |
|         | -4060002              | A (deprecated) API version has been called |
|         | -4060003              | An unsupported API version has been called or an API version has been called with a wrong URI |
|         | -4090001 ~ 4          | Error related with internal DB |
|         | -4150001              | Delivered invalid json data format. |
|         | -5000001 ~ 15         | Error in internal system |
| Gateway | -4010202              | Called invalid App ID. |
|         | -4010203              | Invalid access token |
|         | -4010204              | Invalid user due to banning/withdrawal/missing account |
|         | -4010208              | Gamebase Access Token or IdP Access Token expired |
|         | -4040201              | NHN Cloud for called API is not enabled. <br> e.g) When a Leaderboard API is called via Gamebase when the product is not used. <br/>Or, when Gamebase itself is not enabled. |
|         | -4040202              | Called undefined API. |
|         | -4120201              | Some of the SANDBOX systems provided sent a call for API to the server address of actual environment (or the other way around) <br/>- When this error occurs, the server call address needs to be confirmed |
|         | -5000201 ~ 8          | Error in internal system of Gateway |
| Launching | -4040303            | When requesting initialization with the unregistered client version<br>the client version and store code currently registered in the console need to be confirmed |
| Member  | -4000402              | Entered invalid user ID. |
|         | -4000403              | Requested for invalid member. |
|         | -4000404              | Requested for invalid authentication. |
|         | -4000409              | Using TransferAccount information issued for device transfer on same device |
|         | -4040401              | Requested for a member who does not exist or has withdrawn. |
|         | -4040403              | Requested for invalid TransferAccount |
|         | -4100402              | Requested for already used TransferAccount  |
|         | -4100403              | Requested for expired TransferAccount |
|         | -4100401              | Requested for a member who has withdrawn. |
|         | -4220401              | User authentication data is not normal. |
| IdP     | -4000901              | A (blocked) user has requested validation of TransferAccount |
|         | -4000920              | An error has occurred during internal password encryption. If this problem persists, please contact Customer Center. |
|         | -4000921 ~ 2          | Internal error occurred while issuing TransferAccount. If error continues, contact Customer Center for inquiries. |
|         | -4000923              | To change password in the manual mode, enter the same characters as current password. |
|         | -4000924              | Internal error. If this problem persists, please contact Customer Center. |
|         | -4000925              | To change ID in the manual mode, enter current ID. |
|         | -4000927              | The password is wrong during TransferAccount verification |
|         | -4040920              | The ID does not exist during TransferAccount verification |
|         | -5110920              | Error in TransferAccount issuance. If error continues, contact Customer Center for inquiries. |
| Coupon  | -100002<br>-4000205   | Invalid coupon code  <br>- User requested for Consume Coupon with invalid coupon codes <br>- Coupon registered on console is currently disabled <br>- Missing or called invalid store code, after store was specified on console |
|         | -100004               | Coupon code already manually expired at the request |
|         | -100005               | Coupon already used |
|         | -100007 ~ 8           | Coupon not in service period |
|         | -100011               | Exceeded available coupon limits |
|         | -999999               | Internal error in the coupon system. If error continues, contact Customer Center for inquiries. |
| IAP     | 5000                  | CONSUME FAILED |
|         | 5018                  | Payment already consumed |
|         | 1100                  | Missing required parameter or sent invalid parameter |
|         | 5032                  | Send invalid type of Json data |
|         | 9999                  | UNKNOWN ERROR |
|         | -4001002              | Invalid Gamebase product |
