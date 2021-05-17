## Game > Gamebase > Error Code

## Client SDK

| Category        | Platform           | Error                                    | Error Code | Description                                    |
| --------------- | ------------------ | ---------------------------------------- | ---------- | ---------------------------------------- |
| Common          | Android, UNITY<br/>IOS | NOT_INITIALIZED<br/>TCGB\_ERROR\_NOT\_INITIALIZED | 1          | Gamebase is not initialized.               |
|                 | Android, UNITY<br/>IOS | NOT\_LOGGED\_IN<br/>TCGB\_ERROR\_NOT\_LOGGED\_IN | 2          | Login is required.                              |
|                 | Android, UNITY<br/>IOS | INVALID_PARAMETER<br/>TCGB\_ERROR\_INVALID\_PARAMETER | 3          | Invalid parameter                             |
|                 | Android, UNITY<br/>IOS | INVALID\_JSON\_FORMAT<br/>TCGB\_ERROR\_INVALID\_JSON\_FORMAT | 4          | Invalid JSON format                           |
|                 | Android, UNITY<br/>IOS | USER_PERMISSION<br/>TCGB\_ERROR\_USER\_PERMISSION | 5          | User is not authorized.                               |
|                 | Android, UNITY<br/>IOS | INVALID\_MEMBER<br/>TCGB\_ERROR\_INVALID\_MEMBER  | 6          | Request for invalid member.                              |
|                 | Android, UNITY<br/>IOS | BANNED\_MEMBER<br/>TCGB\_ERROR\_BANNED\_MEMBER   | 7         | Named member has been banned.                                |
|                 | Android, UNITY<br/>IOS | NOT_SUPPORTED<br/>TCGB\_ERROR\_NOT\_SUPPORTED | 10         | The function is not supported.                         |
|                 | UNITY<br/>IOS      | NOT\_SUPPORTED\_ANDROID<br/>TCGB\_ERROR\_NOT\_SUPPORTED\_ANDROID | 11         | The function is not supported by Android.                |
|                 | UNITY<br/>IOS      | NOT\_SUPPORTED\_IOS<br/>TCGB\_ERROR\_NOT\_SUPPORTED\_IOS | 12         | The function is not supported by iOS.                    |
|                 | UNITY              | NOT\_SUPPORTED\_UNITY\_EDITOR            | 13         | The function is not supported by Editor.                 |
|                 | UNITY              | NOT\_SUPPORTED\_UNITY\_STANDALONE        | 14         | The function is not supported by Standalone.              |
|                 | UNITY              | NOT\_SUPPORTED\_UNITY\_WEBGL             | 15         | The function is not supported by WebGL.               |
|                 | Android            | ANDROID\_ACTIVITY\_DESTROYED             | 31         | Activity was terminated by force.                       |
|                 | Android            | ANDROID\_ACTIVEAPP\_NOT\_CALLED          | 32         | The activeApp API has not been called.                 |
|                 | IOS                | IOS_GAMECENTER_DENIED                    | 51         | Gamecenter login has been denied.                 |
| Network(Socket) | Android, UNITY<br/>IOS | SOCKET\_RESPONSE\_TIMEOUT<br/>TCGB\_ERROR\_SOCKET\_RESPONSE\_TIMEOUT | 101        | There is no response due to bad network connection.              |
|                 | Android, UNITY<br/>IOS | SOCKET_ERROR<br/>TCGB\_ERROR\_SOCKET\_ERROR | 110        | Socket error                                  |
|                 | Android, UNITY<br/>IOS | UNKNOWN_ERROR<br/>TCGB\_ERROR\_UNKNOWN\_ERROR | 999        | Unknown socket error                          |
| Launching       | Android, UNITY<br/>IOS | LAUNCHING\_SERVER\_ERROR<br/>TCGB\_ERROR\_LAUNCHING\_SERVER\_ERROR | 2001       | Launching server error                           |
|                 | Android, UNITY<br/>IOS | LAUNCHING\_NOT\_EXIST\_CLIENT\_ID<br/>TCGB\_ERROR\_LAUNCHING\_NOT\_EXIST\_CLIENT\_ID | 2002       | Named client ID does not exist.                 |
|                 | Android, UNITY<br/>IOS | LAUNCHING\_UNREGISTERED\_APP<br/>TCGB\_ERROR\_LAUNCHING\_UNREGISTERED\_APP | 2003       | Named app is not registered.                       |
|                 | Android, UNITY<br/>IOS | LAUNCHING\_UNREGISTERED\_CLIENT<br/>TCGB\_ERROR\_LAUNCHING\_UNREGISTERED\_CLIENT | 2004       | Named client (version) is not registered.           |
| Auth            | Android, UNITY<br/>IOS | AUTH\_USER\_CANCELED<br/>TCGB\_ERROR\_AUTH\_USER\_CANCELED | 3001       | Login is cancelled.                          |
|                 | Android, UNITY<br/>IOS | AUTH\_NOT\_SUPPORTED\_PROVIDER<br/>TCGB\_ERROR\_AUTH\_NOT\_SUPPORTED\_PROVIDER | 3002       | The authentication is not supported.                    |
|                 | Android, UNITY<br/>IOS | AUTH\_NOT\_EXIST\_MEMBER<br/>TCGB\_ERROR\_AUTH\_NOT\_EXIST\_MEMBER | 3003       | Named member does not exist or has withdrawn.                    |
|                 | Android, UNITY<br/>IOS | AUTH\_EXTERNAL\_LIBRARY\_INITIALIZATION\_ERROR<br/>TCGB\_ERROR\_AUTH\_EXTERNAL\_LIBRARY\_INITIALIZATION\_ERROR | 3006       | Failed to initialize the external authentication library.                       |
|                 | Android, UNITY<br/>IOS | AUTH\_EXTERNAL\_LIBRARY\_ERROR<br/>TCGB\_ERROR\_AUTH\_EXTERNAL\_LIBRARY\_ERROR | 3009       | Error in external authentication library                    |
|                 | Android, UNITY<br/>IOS | AUTH\_ALREADY\_IN\_PROGRESS\_ERROR<br/>TCGB\_ERROR\_AUTH\_ALREADY\_IN\_PROGRESS\_ERROR | 3010       | The previous authentication process has not been completed.                 |
| TransferAccount | Android, UNITY<br/>IOS | SAME\_REQUESTOR<br/>TCGB\_ERROR\_SAME\_REQUESTOR | 8 | The issued TransferKey has been used on the same device.  |
|                 | Android, UNITY<br/>IOS | NOT\_GUEST\_OR\_HAS\_OTHERS<br/>TCGB\_ERROR\_NOT\_GUEST\_OR\_HAS\_OTHERS | 9          | You have tried transferring with a non-guest account or the account is linked with a non-guest IdP. |
|                 | Android, UNITY<br/>IOS | AUTH\_TRANSFERACCOUNT\_EXPIRED<br/>TCGB\_ERROR\_AUTH\_TRANSFERACCOUNT\_EXPIRED | 3041 | TransferAccount has expired. |
|                 | Android, UNITY<br/>IOS | AUTH\_TRANSFERACCOUNT\_BLOCK<br/>TCGB\_ERROR\_AUTH\_TRANSFERACCOUNT\_BLOCK | 3042 | The account transfer feature is locked because invalid TransferAccount is attempted multiple times. |
|                 | Android, UNITY<br/>IOS | AUTH\_TRANSFERACCOUNT\_INVALID\_ID<br/>TCGB\_ERROR\_AUTH\_TRANSFERACCOUNT\_INVALID\_ID | 3043 | Invalid TransferAccount ID. |
|                 | Android, UNITY<br/>IOS | AUTH\_TRANSFERACCOUNT\_INVALID\_PASSWORD<br/>TCGB\_ERROR\_AUTH\_TRANSFERACCOUNT\_INVALID\_PASSWORD | 3044 | Invalid password for TransferAccount. |
|                 | Android, UNITY<br/>IOS | AUTH\_TRANSFERACCOUNT\_CONSOLE\_NO\_CONDITION<br/>TCGB\_ERROR\_AUTH\_TRANSFERACCOUNT\_CONSOLE\_NO\_CONDITION | 3045 | TransferAccount is not configured. <br/> Configure it in the NHN Cloud Gamebase Console first. |
|                 | Android, UNITY<br/>IOS | AUTH\_TRANSFERACCOUNT\_NOT\_EXIST<br/>TCGB\_ERROR\_AUTH\_TRANSFERACCOUNT\_NOT\_EXIST | 3046 | TransferAccount doesn't exist. Get the issued TransferAccount first. |
|                 | Android, UNITY<br/>IOS | AUTH\_TRANSFERACCOUNT\_ALREADY\_EXIST\_ID<br/>TCGB\_ERROR\_AUTH\_TRANSFERACCOUNT\_ALREADY\_EXIST\_ID | 3047 | TransferAccount already exist. |
|                 | Android, UNITY<br/>IOS | AUTH\_TRANSFERACCOUNT\_ALREADY\_USED<br/>TCGB\_ERROR\_AUTH\_TRANSFERACCOUNT\_ALREADY\_USED | 3048 | TransferAccount has already been used. |
| Auth (Login)    | Android, UNITY<br/>IOS | AUTH\_TOKEN\_LOGIN\_FAILED<br/>TCGB\_ERROR\_AUTH\_TOKEN\_LOGIN\_FAILED | 3101       | Token login has failed.                       |
|                 | Android, UNITY<br/>IOS | AUTH\_TOKEN\_LOGIN\_INVALID\_TOKEN\_INFO<br/>TCGB\_ERROR\_AUTH\_TOKEN\_LOGIN\_INVALID\_TOKEN\_INFO | 3102       | Invalid token information                        |
|                 | Android, UNITY<br/>IOS | AUTH\_TOKEN\_LOGIN\_INVALID\_LAST\_LOGGED\_IN\_IDP<br/>TCGB\_ERROR\_AUTH\_TOKEN\_LOGIN\_INVALID\_LAST\_LOGGED\_IN\_IDP | 3103       | Invalid last login IdP information                    |
| IDP Login       | Android, UNITY<br/>IOS | AUTH\_IDP\_LOGIN\_FAILED<br/>TCGB\_ERROR\_AUTH\_IDP\_LOGIN\_FAILED | 3201       | IdP login has failed.                       |
|                 | Android, UNITY<br/>IOS | AUTH\_IDP\_LOGIN\_INVALID\_IDP\_INFO<br/>TCGB\_ERROR\_AUTH\_IDP\_LOGIN\_INVALID\_IDP\_INFO | 3202       | Invalid IdP information(IdP information does not exist in the Console.). |
| Add Mapping     | Android, UNITY<br/>IOS | AUTH\_ADD\_MAPPING\_FAILED<br/>TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_FAILED | 3301       | Add mapping has failed.                       |
|                 | Android, UNITY<br/>IOS | AUTH\_ADD\_MAPPING\_ALREADY\_MAPPED\_TO\_OTHER\_MEMBER<br/>TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_ALREADY\_MAPPED\_TO\_OTHER\_MEMBER | 3302       | Already mapped to another member.                     |
|                 | Android, UNITY<br/>IOS | AUTH\_ADD\_MAPPING\_ALREADY\_HAS\_SAME\_IDP<br/>TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_ALREADY\_HAS\_SAME\_IDP | 3303       | Already mapped to same IdP.                   |
|                 | Android, UNITY<br/>IOS | AUTH\_ADD\_MAPPING\_INVALID\_IDP\_INFO<br/>TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_INVALID\_IDP\_INFO | 3304       | Invalid IdP information (IdP information does not exist in the Console.) |
|                 | Android, UNITY<br/>IOS | AUTH\_ADD\_MAPPING\_CANNOT\_ADD\_GUEST\_IDP<br/>TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_CANNOT\_ADD\_GUEST\_IDP | 3305       | AddMapping is not available with Guest IdP. |
| Add Mapping Forcibly | Android, UNITY<br/>IOS | AUTH\_ADD\_MAPPING\_FORCIBLY\_NOT\_EXIST\_KEY<br/>TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_FORCIBLY\_NOT\_EXIST\_KEY | 3311       | ForcingMappingKey does not exist. <br/>Check ForcingMappingTicket again. |
|                      | Android, UNITY<br/>IOS | AUTH\_ADD\_MAPPING\_FORCIBLY\_ALREADY\_USED\_KEY<br/>TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_FORCIBLY\_ALREADY\_USED\_KEY | 3312       | ForcingMappingKey has already been used. |
|                      | Android, UNITY<br/>IOS | AUTH\_ADD\_MAPPING\_FORCIBLY\_EXPIRED\_KEY<br/>TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_FORCIBLY\_EXPIRED\_KEY | 3313       | ForcingMappingKey has expired. |
|                      | Android, UNITY<br/>IOS | AUTH\_ADD\_MAPPING\_FORCIBLY\_DIFFERENT\_IDP<br/>TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_FORCIBLY\_DIFFERENT\_IDP | 3314       | ForcingMappingKey is used from a different IdP. <br/>The issued ForcingMappingKey is used to forcibly map to the same IdP. |
|                      | Android, UNITY<br/>IOS | AUTH\_ADD\_MAPPING\_FORCIBLY\_DIFFERENT\_AUTHKEY<br/>TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_FORCIBLY\_DIFFERENT\_AUTHKEY | 3315       | ForcingMappingKey is used from a different account. <br/>The issued ForcingMappingKey is used to forcibly map to the same IdP and account. |
| Remove Mapping  | Android, UNITY<br/>IOS | AUTH\_REMOVE\_MAPPING\_FAILED<br/>TCGB\_ERROR\_AUTH\_REMOVE\_MAPPING\_FAILED | 3401       | Remove mapping has failed.                         |
|                 | Android, UNITY<br/>IOS | AUTH\_REMOVE\_MAPPING\_LAST\_MAPPED\_IDP<br/>TCGB\_ERROR\_AUTH\_REMOVE\_MAPPING\_LAST\_MAPPED\_IDP | 3402       | Cannot delete last mapped IdP.              |
|                 | Android, UNITY<br/>IOS | AUTH\_REMOVE\_MAPPING\_LOGGED\_IN\_IDP<br/>TCGB\_ERROR\_AUTH\_REMOVE\_MAPPING\_LOGGED\_IN\_IDP | 3403       | Currently logged-in IdP                  |
| Logout          | Android, UNITY<br/>IOS | AUTH\_LOGOUT\_FAILED<br/>TCGB\_ERROR\_AUTH\_LOGOUT\_FAILED | 3501       | Logout has failed.                          |
| Withdrawal      | Android, UNITY<br/>IOS | AUTH\_WITHDRAW\_FAILED<br/>TCGB\_ERROR\_AUTH\_WITHDRAW\_FAILED | 3601       | Withdrawal has failed.                             |
|                 | Android, UNITY<br/>IOS | AUTH\_WITHDRAW\_ALREADY\_TEMPORARY\_WITHDRAW<br/>TCGB\_ERROR\_AUTH\_WITHDRAW\_ALREADY\_TEMPORARY\_WITHDRAW | 3602   | The user is already in the process of temporary withdrawal. |
|                 | Android, UNITY<br/>IOS | AUTH\_WITHDRAW\_NOT\_TEMPORARY\_WITHDRAW<br/>TCGB\_ERROR\_AUTH\_WITHDRAW\_NOT\_TEMPORARY\_WITHDRAW | 3603       | The user is not in the process of temporary withdrawal. |
| Not Playable    | Android, UNITY<br/>IOS | AUTH\_NOT\_PLAYABLE<br/>TCGB\_ERROR\_AUTH\_NOT\_PLAYABLE | 3701       | Not playable(due to maintenance or service closed)        |
| Auth(Unknown)   | Android, UNITY<br/>IOS | AUTH\_UNKNOWN\_ERROR<br/>TCGB\_ERROR\_AUTH\_UNKNOWN\_ERROR | 3999       | Unknown error(Undefined error)           |
| Purchase        | Android, UNITY<br/>IOS | PURCHASE\_NOT\_INITIALIZED<br/>TCGB\_ERROR\_PURCHASE\_NOT\_INITIALIZED | 4001       | Gamebase PurchaseAdapter is not initialized.   |
|                 | Android, UNITY<br/>IOS | PURCHASE\_USER\_CANCELED<br/>TCGB\_ERROR\_PURCHASE\_USER\_CANCELED | 4002       | Purchase is cancelled.                           |
|                 | Android, UNITY<br/>IOS | PURCHASE\_NOT\_FINISHED\_PREVIOUS\_PURCHASING<br/>TCGB\_ERROR\_PURCHASE\_NOT\_FINISHED\_PREVIOUS\_PURCHASING | 4003       | Previous purchase is not completed.                      |
|                 | UNITY                  | PURCHASE\_NOT\_ENOUGH\_CASH                                        | 4004       | Cannot purchase due to shortage of cash of the store.             |
|                 | Android, UNITY<br/>IOS | PURCHASE\_INACTIVE\_PRODUCT\_ID<br/>TCGB\_ERROR\_PURCHASE\_INACTIVE\_PRODUCT\_ID | 4005       | The status of the product is not active. |
|                 | Android, UNITY<br/>IOS | PURCHASE\_NOT\_EXIST\_PRODUCT\_ID<br/>TCGB\_ERROR\_PURCHASE\_NOT\_EXIST\_PRODUCT\_ID | 4006       | Payment is requested with a non-existent GamebaseProductID. |
|                 | Android, UNITY<br/>IOS | PURCHASE\_NOT\_SUPPORTED\_MARKET<br/>TCGB\_ERROR\_PURCHASE\_NOT\_SUPPORTED\_MARKET | 4010       | The store is not supported.                         |
|                 | Android, UNITY<br/>IOS | PURCHASE\_EXTERNAL\_LIBRARY\_ERROR<br/>TCGB\_ERROR\_PURCHASE\_EXTERNAL\_LIBRARY\_ERROR | 4201       | Error in external IAP library                   |
|                 | Android, UNITY<br/>IOS | PURCHASE\_UNKNOWN\_ERROR<br/>TCGB\_ERROR\_PURCHASE\_UNKNOWN\_ERROR | 4999       | Unknown error in purchase                         |
| Push            | Android, UNITY<br/>IOS | PUSH\_EXTERNAL\_LIBRARY\_ERROR<br/>TCGB\_ERROR\_PUSH\_EXTERNAL\_LIBRARY\_ERROR | 5101       | Error in external library                      |
|                 | Android, UNITY<br/>IOS | PUSH\_ALREADY\_IN\_PROGRESS\_ERROR<br/>TCGB\_ERROR\_PUSH\_ALREADY\_IN\_PROGRESS\_ERROR | 5102       | Previous PUSH API call is not completed.             |
|                 | Android, UNITY<br/>IOS | PUSH\_UNKNOWN\_ERROR<br/>TCGB\_ERROR\_PUSH\_UNKNOWN\_ERROR | 5999       | Unknown push error(Undefined push error)     |
| UI              | Android, UNITY<br/>IOS | UI\_IMAGE\_NOTICE\_TIMEOUT<br/>TCGB\_ERROR\_UI\_IMAGE\_NOTICE\_TIMEOUT | 6901       | Timed out while displaying image notice.            |
|                 | Android, UNITY<br/>IOS | UI\_CONTACT\_FAIL\_INVALID\_URL<br/>TCGB\_ERROR\_UI\_CONTACT\_FAIL\_INVALID\_URL | 6911       | Failed to create a Customer Center webview URL.            |
|                 | Android, UNITY<br/>IOS | UI\_CONTACT\_FAIL\_FAIL\_ISSUE\_SHORT\_TERM\_TICKET<br/>TCGB\_ERROR\_UI\_CONTACT\_FAIL\_ISSUE\_SHORT\_TERM\_TICKET | 6912       | Failed to issue a temporary ticket for user identification.            |
|                 | Android, UNITY<br/>IOS | UI\_TERMS\_NOT\_EXIST\_IN\_CONSOLE<br/>TCGB\_ERROR\_UI\_TERMS\_NOT\_EXIST\_IN\_CONSOLE | 6921       | Terms & conditions information is not registered with the console. |
|                 | Android, UNITY<br/>IOS | UI\_TERMS\_NOT\_EXIST\_FOR\_DEVICE\_COUNTRY<br/>TCGB\_ERROR\_UI\_TERMS\_NOT\_EXIST\_FOR\_DEVICE\_COUNTRY | 6922       | Terms & conditions appropriate for the device's country code is not registered with the console. |
|                 | Android, UNITY<br/>IOS | UI\_TERMS\_UNREGISTERED\_SEQ<br/>TCGB\_ERROR\_UI\_TERMS\_UNREGISTERED\_SEQ | 6923       | Unregistered terms and conditions Seq value.            |
|                 | Android, UNITY<br/>IOS | UI\_TERMS\_ALREADY\_IN\_PROGRESS\_ERROR<br/>TCGB\_ERROR\_UI\_TERMS\_ALREADY\_IN\_PROGRESS\_ERROR | 6924       | The Terms API called previously has not been completed yet.<br/>Please try again later. |
|                 | Android, UNITY         | UI\_TERMS\_ANDROID\_DUPLICATED\_VIEW | 6925       | Unfinished terms & conditions WebView has been called again. |
| WebView         | Android, UNITY<br/>IOS | WEBVIEW\_INVALID\_URL<br/>TCGB\_ERROR\_WEBVIEW\_INVALID\_URL   		| 7001       | Invalid URL.            |
|                 | Android, UNITY<br/>IOS | WEBVIEW\_TIMEOUT<br/>TCGB\_ERROR\_WEBVIEW\_TIMEOUT 	    	        | 7002       | Timed out while displaying WebView.            |
|                 | Android, UNITY<br/>IOS | WEBVIEW\_HTTP\_ERROR<br/>TCGB\_ERROR\_WEBVIEW\_HTTP\_ERROR 	     	| 7003       | Failed to display WebView due to an HTTP error.            |
|                 | UNITY                  | WEBVIEW\_UNKNOWN\_ERROR 								| 7999       | An unknown error has occurred while calling the WebView (Undefined error).            |
| Server          | Android, UNITY<br/>IOS | SERVER\_INTERNAL\_ERROR<br/>TCGB\_ERROR\_SERVER\_INTERNAL\_ERROR | 8001       | Error in internal server                               |
|                 | Android, UNITY<br/>IOS | SERVER\_REMOTE\_SYSTEM\_ERROR<br/>TCGB\_ERROR\_SERVER\_REMOTE\_SYSTEM\_ERROR | 8002       | Error occurred in server during remote integration.                     |
|                 | Android, UNITY<br/>IOS | SERVER\_UNKNOWN\_ERROR<br/>TCGB\_ERROR\_SERVER\_UNKNOWN\_ERROR | 8999       | Unknown error in server                   |

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
|         | -4001002              | Invalid Gambase product |
