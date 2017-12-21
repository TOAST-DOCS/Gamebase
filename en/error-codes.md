Game > Gamebase > Error Code

## Client SDK

| Category        | Platform           | Error                                    | Error Code | Notes                                    |
| --------------- | ------------------ | ---------------------------------------- | ---------- | ---------------------------------------- |
| Common          | AOS, UNITY<br/>IOS | NOT_INITIALIZED<br/>TCGB\_ERROR\_NOT\_INITIALIZED | 1          | Gamebase not initialized.                  |
|                 | AOS, UNITY<br/>IOS | NOT\_LOGGED\_IN<br/>TCGB\_ERROR\_NOT\_LOGGED\_IN | 2          | Login is required.                              |
|                 | AOS, UNITY<br/>IOS | INVALID_PARAMETER<br/>TCGB\_ERROR\_INVALID\_PARAMETER | 3          | Invalid parameter.                             |
|                 | AOS, UNITY<br/>IOS | INVALID\_JSON\_FORMAT<br/>TCGB\_ERROR\_INVALID\_JSON\_FORMAT | 4          | Error in JSON format.                           |
|                 | AOS, UNITY<br/>IOS | USER_PERMISSION<br/>TCGB\_ERROR\_USER\_PERMISSION | 5          | Not permitted.                                |
|                 | AOS, UNITY<br/>IOS | NOT_SUPPORTED<br/>TCGB\_ERROR\_NOT\_SUPPORTED | 10         | The function is not supported.                           |
|                 | UNITY<br/>IOS      | NOT\_SUPPORTED\_ANDROID<br/>TCGB\_ERROR\_NOT\_SUPPORTED\_ANDROID | 11         | Not supported by Android.                 |
|                 | UNITY<br/>IOS      | NOT\_SUPPORTED\_IOS<br/>TCGB\_ERROR\_NOT\_SUPPORTED\_IOS | 12         | Not supported by iOS.                     |
|                 | UNITY              | NOT\_SUPPORTED\_UNITY\_EDITOR            | 13         | Not supported by Editor.                  |
|                 | UNITY              | NOT\_SUPPORTED\_UNITY\_STANDALONE        | 14         | Not supported by Standalone.              |
|                 | UNITY              | NOT\_SUPPORTED\_UNITY\_WEBGL             | 15         | Not supported by WebGL.                   |
| Network(Socket) | AOS, UNITY<br/>IOS | SOCKET\_RESPONSE\_TIMEOUT<br/>TCGB\_ERROR\_SOCKET\_RESPONSE\_TIMEOUT | 101        | No response due to bad network connection.                 |
|                 | AOS, UNITY<br/>IOS | SOCKET_ERROR<br/>TCGB\_ERROR\_SOCKET\_ERROR | 110        | Socket error.                                    |
|                 | AOS, UNITY<br/>IOS | UNKNOWN_ERROR<br/>TCGB\_ERROR\_UNKNOWN\_ERROR | 999        | Unknown socket error.                             |
| Launching       | AOS, UNITY<br/>IOS | LAUNCHING\_SERVER\_ERROR<br/>TCGB\_ERROR\_LAUNCHING\_SERVER\_ERROR | 2001       | Error in launching server.                             |
|                 | AOS, UNITY<br/>IOS | LAUNCHING\_NOT\_EXIST\_CLIENT\_ID<br/>TCGB\_ERROR\_LAUNCHING\_NOT\_EXIST\_CLIENT\_ID | 2002       | The client ID does not exist.                    |
|                 | AOS, UNITY<br/>IOS | LAUNCHING\_UNREGISTERED\_APP<br/>TCGB\_ERROR\_LAUNCHING\_UNREGISTERED\_APP | 2003       | The app is not registered.                         |
|                 | AOS, UNITY<br/>IOS | LAUNCHING\_UNREGISTERED\_CLIENT<br/>TCGB\_ERROR\_LAUNCHING\_UNREGISTERED\_CLIENT | 2004       | The client (version) is not registered.            |
| Auth            | AOS, UNITY<br/>IOS | AUTH\_USER\_CANCELED<br/>TCGB\_ERROR\_AUTH\_USER\_CANCELED | 3001       | Login has been cancelled.                            |
|                 | AOS, UNITY<br/>IOS | AUTH\_NOT\_SUPPORTED\_PROVIDER<br/>TCGB\_ERROR\_AUTH\_NOT\_SUPPORTED\_PROVIDER | 3002       | The authentication is not supported.                        |
|                 | AOS, UNITY<br/>IOS | AUTH\_NOT\_EXIST\_MEMBER<br/>TCGB\_ERROR\_AUTH\_NOT\_EXIST\_MEMBER | 3003       | The member does not exist or has withdrawn.                      |
|                 | AOS, UNITY<br/>IOS | AUTH\_INVALID\_MEMBER<br/>TCGB\_ERROR\_AUTH\_INVALID\_MEMBER | 3004       | Request for invalid member.                        |
|                 | AOS, UNITY<br/>IOS | AUTH\_INVALID\_MEMBER<br/>TCGB\_ERROR\_AUTH\_BANNED\_MEMBER | 3005       | The member has been banned.                               |
|                 | AOS, UNITY<br/>IOS | AUTH\_EXTERNAL\_LIBRARY\_ERROR<br/>TCGB\_ERROR\_AUTH\_EXTERNAL\_LIBRARY\_ERROR | 3009       | Error in external authentication library.                       |
| Auth (Login)    | AOS, UNITY<br/>IOS | AUTH\_TOKEN\_LOGIN\_FAILED<br/>TCGB\_ERROR\_AUTH\_TOKEN\_LOGIN\_FAILED | 3101       | Token login has failed.                         |
|                 | AOS, UNITY<br/>IOS | AUTH\_TOKEN\_LOGIN\_INVALID\_TOKEN\_INFO<br/>TCGB\_ERROR\_AUTH\_TOKEN\_LOGIN\_INVALID\_TOKEN\_INFO | 3102       | Invalid token information.                         |
|                 | AOS, UNITY<br/>IOS | AUTH\_TOKEN\_LOGIN\_INVALID\_LAST\_LOGGED\_IN\_IDP<br/>TCGB\_ERROR\_AUTH\_TOKEN\_LOGIN\_INVALID\_LAST\_LOGGED\_IN\_IDP | 3103       | Invalid last login IDP information.                   |
| IDP Login       | AOS, UNITY<br/>IOS | AUTH\_IDP\_LOGIN\_FAILED<br/>TCGB\_ERROR\_AUTH\_IDP\_LOGIN\_FAILED | 3201       | IDP login has failed.                        |
|                 | AOS, UNITY<br/>IOS | AUTH\_IDP\_LOGIN\_INVALID\_IDP\_INFO<br/>TCGB\_ERROR\_AUTH\_IDP\_LOGIN\_INVALID\_IDP\_INFO | 3202       | Invalid IDP information. (IDP information does not exist in Console.) |
| Add Mapping     | AOS, UNITY<br/>IOS | AUTH\_ADD\_MAPPING\_FAILED<br/>TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_FAILED | 3301       | Add mapping has failed.                          |
|                 | AOS, UNITY<br/>IOS | AUTH\_ADD\_MAPPING\_ALREADY\_MAPPED\_TO\_OTHER\_MEMBER<br/>TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_ALREADY\_MAPPED\_TO\_OTHER\_MEMBER | 3302       | Already mapped to other member.                      |
|                 | AOS, UNITY<br/>IOS | AUTH\_ADD\_MAPPING\_ALREADY\_HAS\_SAME\_IDP<br/>TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_ALREADY\_HAS\_SAME\_IDP | 3303       | Already mapped to same IDP.                     |
|                 | AOS, UNITY<br/>IOS | AUTH\_ADD\_MAPPING\_INVALID\_IDP\_INFO<br/>TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_INVALID\_IDP\_INFO | 3304       | Invalid IDP information. (IDP information does not exist in Console.) |
| Remove Mapping  | AOS, UNITY<br/>IOS | AUTH\_REMOVE\_MAPPING\_FAILED<br/>TCGB\_ERROR\_AUTH\_REMOVE\_MAPPING\_FAILED | 3401       | Remove mapping has failed.                          |
|                 | AOS, UNITY<br/>IOS | AUTH\_REMOVE\_MAPPING\_LAST\_MAPPED\_IDP<br/>TCGB\_ERROR\_AUTH\_REMOVE\_MAPPING\_LAST\_MAPPED\_IDP | 3402       | Cannot delete last mapped IDP.                |
|                 | AOS, UNITY<br/>IOS | AUTH\_REMOVE\_MAPPING\_LOGGED\_IN\_IDP<br/>TCGB\_ERROR\_AUTH\_REMOVE\_MAPPING\_LOGGED\_IN\_IDP | 3403       | Currently logged-in IDP.                      |
| Logout          | AOS, UNITY<br/>IOS | AUTH\_LOGOUT\_FAILED<br/>TCGB\_ERROR\_AUTH\_LOGOUT\_FAILED | 3501       | Logout has failed.                           |
| Withdrawal      | AOS, UNITY<br/>IOS | AUTH\_WITHDRAW\_FAILED<br/>TCGB\_ERROR\_AUTH\_WITHDRAW\_FAILED | 3601       | Withdrawal has failed.                              |
| Not Playable    | AOS, UNITY<br/>IOS | AUTH\_NOT\_PLAYABLE<br/>TCGB\_ERROR\_AUTH\_NOT\_PLAYABLE | 3701       | Not Playable. (due to maintenance or service closed)        |
| Auth(Unknown)   | AOS, UNITY<br/>IOS | AUTH\_UNKNOWN\_ERROR<br/>TCGB\_ERROR\_AUTH\_UNKNOWN\_ERROR | 3999       | Unknown error. (Undefined error.)           |
| Purchase        | AOS, UNITY<br/>IOS | PURCHASE\_NOT\_INITIALIZED<br/>TCGB\_ERROR\_PURCHASE\_NOT\_INITIALIZED | 4001       | Gamebase PurchaseAdapter not initialized.   |
|                 | AOS, UNITY<br/>IOS | PURCHASE\_USER\_CANCELED<br/>TCGB\_ERROR\_PURCHASE\_USER\_CANCELED | 4002       | Purchase has been cancelled.                             |
|                 | AOS, UNITY<br/>IOS | PURCHASE\_NOT\_FINISHED\_PREVIOUS\_PURCHASING<br/>TCGB\_ERROR\_PURCHASE\_NOT\_FINISHED\_PREVIOUS\_PURCHASING | 4003       | Previous purchase not completed.                       |
|                 | AOS, UNITY<br/>IOS | PURCHASE\_NOT\_ENOUGH\_CASH<br/>TCGB\_ERROR\_PURCHASE\_NOT\_ENOUGH\_CASH | 4004       | Cannot purchase due to shortage of cash of the store.             |
|                 | AOS, UNITY<br/>IOS | PURCHASE\_NOT\_SUPPORTED\_MARKET<br/>TCGB\_ERROR\_PURCHASE\_NOT\_SUPPORTED\_MARKET | 4010       | The store is not supported.                          |
|                 | AOS, UNITY<br/>IOS | PURCHASE\_EXTERNAL\_LIBRARY\_ERROR<br/>TCGB\_ERROR\_PURCHASE\_EXTERNAL\_LIBRARY\_ERROR | 4201       | Errro in external IAP library.                      |
|                 | AOS, UNITY<br/>IOS | PURCHASE\_UNKNOWN\_ERROR<br/>TCGB\_ERROR\_PURCHASE\_UNKNOWN\_ERROR | 4999       | Unknown error in purchase.                           |
| Push            | AOS, UNITY<br/>IOS | PUSH\_EXTERNAL\_LIBRARY\_ERROR<br/>TCGB\_ERROR\_PUSH\_EXTERNAL\_LIBRARY\_ERROR | 5101       | Error in external library.                          |
|                 | AOS, UNITY<br/>IOS | PUSH\_ALREADY\_IN\_PROGRESS\_ERROR<br/>TCGB\_ERROR\_PUSH\_ALREADY\_IN\_PROGRESS\_ERROR | 5102       | Previous PUSH API call not completed.              |
|                 | AOS, UNITY<br/>IOS | PUSH\_UNKNOWN\_ERROR<br/>TCGB\_ERROR\_PUSH\_UNKNOWN\_ERROR | 5999       | Unknown PUSH error. (Undefined PUSH error.)      |
| UI              | AOS, UNITY<br/>IOS | UI\_UNKNOWN\_ERROR<br/>TCGB\_ERROR\_UI\_UNKNOWN\_ERROR | 6999       | Unknown error. (Undefined error.)            |
| Server          | AOS, UNITY<br/>IOS | SERVER\_INTERNAL\_ERROR<br/>TCGB\_ERROR\_SERVER\_INTERNAL\_ERROR | 8001       | Error in internal server                                 |
|                 | AOS, UNITY<br/>IOS | SERVER\_REMOTE\_SYSTEM\_ERROR<br/>TCGB\_ERROR\_SERVER\_REMOTE\_SYSTEM\_ERROR | 8002       | Error occurred in server during remote integration                        |
|                 | AOS, UNITY<br/>IOS | SERVER\_UNKNOWN\_ERROR<br/>TCGB\_ERROR\_SERVER\_UNKNOWN\_ERROR | 8999       | Unknwon error in server                           |

<br/>
<br/>
## Server
| Module  | Error Code            | Description                              |
| ------- | --------------------- | ---------------------------------------- |
| Common  | -4000001<br/>-4000006 | Called API with invalid parameter type <br/>e.g) The parameter is declared in the int type, but API has been called with a string-format data |
|         | -4000002<br/>-4000004 | A required parameter is missing or has no value |
|         | -4000003              | Undefined value has been delivered to request body |
|         | -4000005              | A required parameter is missing or called with an inappropriate value |
|         | -4010001              | Called a wrong app ID                    |
|         | -4010002              | Called a wrong appkey                    |
|         | -4010003              | Unauthenticated client called an API which requires authentication |
|         | -4010004              | Called a wrong secret key                |
|         | -4060001              | Set a wrong content-type in the HTTP header |
|         | -4090001 ~ 4          | Error related with Internal DB           |
|         | -4150001              | Delivered a wrong-format Json data         |
|         | -5000001 ~ 15         | Error of internal system                 |
| Gateway | -4010202              | Called a wrong app ID                    |
|         | -4010203              | Invalid access token                     |
|         | -4010204              | Invalid user due to ban/withdraw/account missing |
|         | -4040201              | TOAST Cloud products of a called API are not enabled  <br/>e.g) When a Leaderboard API is called via Gamebase when the product is not used  <br/> Or, when Gamebase itself is not enabled |
|         | -4040202              | Called undefined API                     |
|         | -5000201 ~ 7          | Error of internal system of Gateway      |
| Member  | -4000402              | Entered a wrong user ID                  |
|         | -4000403              | Requested for a wrong member             |
|         | -4000404              | Requested for a wrong Auth <br /> |
|         | -4040401              | Requested for a member who does not exist or has withdrawn |
|         | -4100401              | Requested for a member who has withdrawn |
|         | -4220401              | User Auth data is not normal             |
