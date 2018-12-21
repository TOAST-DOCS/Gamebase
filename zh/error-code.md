## Game > Gamebase > 错误代码

## Client SDK 

| Category        | Platform           | Error                                    | Error Code | Description                                    |
| --------------- | ------------------ | ---------------------------------------- | ---------- | ---------------------------------------- |
| Common          | Android, UNITY<br/>IOS | NOT_INITIALIZED<br/>TCGB\_ERROR\_NOT\_INITIALIZED | 1          | Gamebase未初始化。                 |
|                 | Android, UNITY<br/>IOS | NOT\_LOGGED\_IN<br/>TCGB\_ERROR\_NOT\_LOGGED\_IN | 2          | 需要登录。                              |
|                 | Android, UNITY<br/>IOS | INVALID_PARAMETER<br/>TCGB\_ERROR\_INVALID\_PARAMETER | 3          | 无效的参数。                            |
|                 | Android, UNITY<br/>IOS | INVALID\_JSON\_FORMAT<br/>TCGB\_ERROR\_INVALID\_JSON\_FORMAT | 4          | JSON格式错误。                           |
|                 | Android, UNITY<br/>IOS | USER_PERMISSION<br/>TCGB\_ERROR\_USER\_PERMISSION | 5          | 无权限。                                |
|                 | Android, UNITY<br/>IOS | INVALID\_MEMBER<br/>TCGB\_ERROR\_INVALID\_MEMBER  | 6          | 无效的用户请求。                              |
|                 | Android, UNITY<br/>IOS | BANNED\_MEMBER<br/>TCGB\_ERROR\_BANNED\_MEMBER   | 7         | 被制裁的用户。                                |
|                 | Android, UNITY<br/>IOS | SAME\_REQUESTOR<br/>TCGB\_ERROR\_SAME\_REQUESTOR  | 8          | 在同一台设备上使用了相同的TransferKey。  |
|                 | Android, UNITY<br/>IOS | NOT\_GUEST\_OR\_HAS\_OTHERS<br/>TCGB\_ERROR\_NOT\_GUEST\_OR\_HAS\_OTHERS | 9          | 非游客帐户尝试了转移或帐户已关联了游客以外的IDP。 |
|                 | Android, UNITY<br/>IOS | NOT_SUPPORTED<br/>TCGB\_ERROR\_NOT\_SUPPORTED | 10         | 不支持此功能。                           |
|                 | UNITY<br/>IOS      | NOT\_SUPPORTED\_ANDROID<br/>TCGB\_ERROR\_NOT\_SUPPORTED\_ANDROID | 11         | Android不支持此功能。                 |
|                 | UNITY<br/>IOS      | NOT\_SUPPORTED\_IOS<br/>TCGB\_ERROR\_NOT\_SUPPORTED\_IOS | 12         | iOS不支持此功能。                     |
|                 | UNITY              | NOT\_SUPPORTED\_UNITY\_EDITOR            | 13         | Editor不支持此功能。                  |
|                 | UNITY              | NOT\_SUPPORTED\_UNITY\_STANDALONE        | 14         | Standalone不支持此功能。              |
|                 | UNITY              | NOT\_SUPPORTED\_UNITY\_WEBGL             | 15         | WebGL不支持此功能。                   |
|                 | Android            | ANDROID\_ACTIVITY\_DESTROYED             | 31         | Activity强制终止。                       |
|                 | Android            | ANDROID\_ACTIVEAPP\_NOT\_CALLED          | 32         | 未调用activeApp API。                 |
|                 | IOS                | IOS_GAMECENTER_DENIED                    | 51         | Gamecenter 登录被拒绝。                 |
| Network(Socket) | Android, UNITY<br/>IOS | SOCKET\_RESPONSE\_TIMEOUT<br/>TCGB\_ERROR\_SOCKET\_RESPONSE\_TIMEOUT | 101        | 网络状态不稳定，没有响应。                 |
|                 | Android, UNITY<br/>IOS | SOCKET_ERROR<br/>TCGB\_ERROR\_SOCKET\_ERROR | 110        | SOCKET错误。                                    |
|                 | Android, UNITY<br/>IOS | UNKNOWN_ERROR<br/>TCGB\_ERROR\_UNKNOWN\_ERROR | 999        | SOCKET未知错误。                             |
| Launching       | Android, UNITY<br/>IOS | LAUNCHING\_SERVER\_ERROR<br/>TCGB\_ERROR\_LAUNCHING\_SERVER\_ERROR | 2001       | LAUNCHING服务器错误。                             |
|                 | Android, UNITY<br/>IOS | LAUNCHING\_NOT\_EXIST\_CLIENT\_ID<br/>TCGB\_ERROR\_LAUNCHING\_NOT\_EXIST\_CLIENT\_ID | 2002       | 不存在客户端ID。                    |
|                 | Android, UNITY<br/>IOS | LAUNCHING\_UNREGISTERED\_APP<br/>TCGB\_ERROR\_LAUNCHING\_UNREGISTERED\_APP | 2003       | 未登记的APP。                         |
|                 | Android, UNITY<br/>IOS | LAUNCHING\_UNREGISTERED\_CLIENT<br/>TCGB\_ERROR\_LAUNCHING\_UNREGISTERED\_CLIENT | 2004       | 未登记的客户端（版本）。            |
| Auth            | Android, UNITY<br/>IOS | AUTH\_USER\_CANCELED<br/>TCGB\_ERROR\_AUTH\_USER\_CANCELED | 3001       | 登录已被取消。                            |
|                 | Android, UNITY<br/>IOS | AUTH\_NOT\_SUPPORTED\_PROVIDER<br/>TCGB\_ERROR\_AUTH\_NOT\_SUPPORTED\_PROVIDER | 3002       | 不支持的认证方式。                        |
|                 | Android, UNITY<br/>IOS | AUTH\_NOT\_EXIST\_MEMBER<br/>TCGB\_ERROR\_AUTH\_NOT\_EXIST\_MEMBER | 3003       | 不存在或已退出（删除数据）的用户。                      |
|                 | Android, UNITY<br/>IOS | AUTH\_EXTERNAL\_LIBRARY\_ERROR<br/>TCGB\_ERROR\_AUTH\_EXTERNAL\_LIBRARY\_ERROR | 3009       | 外部认证库错误。                       |
|                 | Android, UNITY<br/>IOS | AUTH_ALREADY_IN_PROGRESS_ERROR<br/>TCGB_ERROR_AUTH_ALREADY_IN_PROGRESS_ERROR | 3010       | 之前的认证过程未完成。                 |
|                 | Android, UNITY<br/>IOS | AUTH\_TRANSFERKEY\_EXPIRED<br/>TCGB\_ERROR\_AUTH\_TRANSFERKEY\_EXPIRED | 3031       | TransferKey已过期。  |
|                 | Android, UNITY<br/>IOS | AUTH\_TRANSFERKEY\_CONSUMED<br/>TCGB\_ERROR\_AUTH\_TRANSFERKEY\_CONSUMED| 3032       | TransferKey已使用。 |
|                 | Android, UNITY<br/>IOS | AUTH\_TRANSFERKEY\_NOT\_EXIST<br/>TCGB\_ERROR\_AUTH\_TRANSFERKEY\_NOT\_EXIST | 3033       | 无效的TransferKey。 |
| Auth (Login)    | Android, UNITY<br/>IOS | AUTH\_TOKEN\_LOGIN\_FAILED<br/>TCGB\_ERROR\_AUTH\_TOKEN\_LOGIN\_FAILED | 3101       | 令牌登录失败。                         |
|                 | Android, UNITY<br/>IOS | AUTH\_TOKEN\_LOGIN\_INVALID\_TOKEN\_INFO<br/>TCGB\_ERROR\_AUTH\_TOKEN\_LOGIN\_INVALID\_TOKEN\_INFO | 3102       | 无效的令牌信息。                        |
|                 | Android, UNITY<br/>IOS | AUTH\_TOKEN\_LOGIN\_INVALID\_LAST\_LOGGED\_IN\_IDP<br/>TCGB\_ERROR\_AUTH\_TOKEN\_LOGIN\_INVALID\_LAST\_LOGGED\_IN\_IDP | 3103       | 无近期登录的IdP信息。                   |
| IDP Login       | Android, UNITY<br/>IOS | AUTH\_IDP\_LOGIN\_FAILED<br/>TCGB\_ERROR\_AUTH\_IDP\_LOGIN\_FAILED | 3201       | IdP登录失败。                        |
|                 | Android, UNITY<br/>IOS | AUTH\_IDP\_LOGIN\_INVALID\_IDP\_INFO<br/>TCGB\_ERROR\_AUTH\_IDP\_LOGIN\_INVALID\_IDP\_INFO | 3202       | 无效的IdP信息。（Console中没有此IdP信息）。 |
| Add Mapping     | Android, UNITY<br/>IOS | AUTH\_ADD\_MAPPING\_FAILED<br/>TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_FAILED | 3301       | 添加映射（Mapping）失败。                          |
|                 | Android, UNITY<br/>IOS | AUTH\_ADD\_MAPPING\_ALREADY\_MAPPED\_TO\_OTHER\_MEMBER<br/>TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_ALREADY\_MAPPED\_TO\_OTHER\_MEMBER | 3302       | 已经与其他帐户映射（Mapping）。                      |
|                 | Android, UNITY<br/>IOS | AUTH\_ADD\_MAPPING\_ALREADY\_HAS\_SAME\_IDP<br/>TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_ALREADY\_HAS\_SAME\_IDP | 3303       | 已映射（Mapping）到相同的IdP。                     |
|                 | Android, UNITY<br/>IOS | AUTH\_ADD\_MAPPING\_INVALID\_IDP\_INFO<br/>TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_INVALID\_IDP\_INFO | 3304       | 无效的IdP信息。（Console中没有此IdP信息。|
|                 | Android, UNITY<br/>IOS | AUTH_ADD_MAPPING_CANNOT_ADD_GUEST_IDP<br/>TCGB_ERROR_AUTH_ADD_MAPPING_CANNOT_ADD_GUEST_IDP | 3305       | 游客IDP无法进行AddMapping。|
| Remove Mapping  | Android, UNITY<br/>IOS | AUTH\_REMOVE\_MAPPING\_FAILED<br/>TCGB\_ERROR\_AUTH\_REMOVE\_MAPPING\_FAILED | 3401       | 解除映射（Mapping）失败。                          |
|                 | Android, UNITY<br/>IOS | AUTH\_REMOVE\_MAPPING\_LAST\_MAPPED\_IDP<br/>TCGB\_ERROR\_AUTH\_REMOVE\_MAPPING\_LAST\_MAPPED\_IDP | 3402       | 无法解除最后映射（Mapping）的IdP。                |
|                 | Android, UNITY<br/>IOS | AUTH\_REMOVE\_MAPPING\_LOGGED\_IN\_IDP<br/>TCGB\_ERROR\_AUTH\_REMOVE\_MAPPING\_LOGGED\_IN\_IDP | 3403       | 当前登录的IdP。                      |
| Logout          | Android, UNITY<br/>IOS | AUTH\_LOGOUT\_FAILED<br/>TCGB\_ERROR\_AUTH\_LOGOUT\_FAILED | 3501       | 退出登录失败。                           |
| Withdrawal      | Android, UNITY<br/>IOS | AUTH\_WITHDRAW\_FAILED<br/>TCGB\_ERROR\_AUTH\_WITHDRAW\_FAILED | 3601       | 退出（删除数据）失败。                             |
| Not Playable    | Android, UNITY<br/>IOS | AUTH\_NOT\_PLAYABLE<br/>TCGB\_ERROR\_AUTH\_NOT\_PLAYABLE | 3701       | 无法玩游戏的状态(维护或已下线等)。        |
| Auth(Unknown)   | Android, UNITY<br/>IOS | AUTH\_UNKNOWN\_ERROR<br/>TCGB\_ERROR\_AUTH\_UNKNOWN\_ERROR | 3999       | 未知错误(未定义的错误)。           |
| Purchase        | Android, UNITY<br/>IOS | PURCHASE\_NOT\_INITIALIZED<br/>TCGB\_ERROR\_PURCHASE\_NOT\_INITIALIZED | 4001       | Gamebase PurchaseAdapter模块未初始化。   |
|                 | Android, UNITY<br/>IOS | PURCHASE\_USER\_CANCELED<br/>TCGB\_ERROR\_PURCHASE\_USER\_CANCELED | 4002       | 取消购买。                             |
|                 | Android, UNITY<br/>IOS | PURCHASE\_NOT\_FINISHED\_PREVIOUS\_PURCHASING<br/>TCGB\_ERROR\_PURCHASE\_NOT\_FINISHED\_PREVIOUS\_PURCHASING | 4003       | 尚未完成上一次购买。                       |
|                 | Android, UNITY<br/>IOS | PURCHASE\_NOT\_ENOUGH\_CASH<br/>TCGB\_ERROR\_PURCHASE\_NOT\_ENOUGH\_CASH | 4004       | 该商店的余额不足，无法结算。             |
|                 | Android, UNITY<br/>IOS | PURCHASE\_NOT\_SUPPORTED\_MARKET<br/>TCGB\_ERROR\_PURCHASE\_NOT\_SUPPORTED\_MARKET | 4010       | 不支持的商店。                          |
|                 | Android, UNITY<br/>IOS | PURCHASE\_EXTERNAL\_LIBRARY\_ERROR<br/>TCGB\_ERROR\_PURCHASE\_EXTERNAL\_LIBRARY\_ERROR | 4201       | 外部IAP库错误。                      |
|                 | Android, UNITY<br/>IOS | PURCHASE\_UNKNOWN\_ERROR<br/>TCGB\_ERROR\_PURCHASE\_UNKNOWN\_ERROR | 4999       | 未知的购买错误。                           |
| Push            | Android, UNITY<br/>IOS | PUSH\_EXTERNAL\_LIBRARY\_ERROR<br/>TCGB\_ERROR\_PUSH\_EXTERNAL\_LIBRARY\_ERROR | 5101       | 外部库错误。                          |
|                 | Android, UNITY<br/>IOS | PUSH\_ALREADY\_IN\_PROGRESS\_ERROR<br/>TCGB\_ERROR\_PUSH\_ALREADY\_IN\_PROGRESS\_ERROR | 5102       | 上一次的推送API调用未完成。              |
|                 | Android, UNITY<br/>IOS | PUSH\_UNKNOWN\_ERROR<br/>TCGB\_ERROR\_PUSH\_UNKNOWN\_ERROR | 5999       | 未知推送错误(未定义的推送错误)。      |
| UI              | Android, UNITY<br/>IOS | UI\_UNKNOWN\_ERROR<br/>TCGB\_ERROR\_UI\_UNKNOWN\_ERROR | 6999       | 未知错误(未定义的错误)。            |
| WebView         | UNITY                  | WEBVIEW\_UNKNOWN\_ERROR 								| 7999       | 调用WEBVIEW时发生了未知错误。(未定义的错误)。            |
| Server          | Android, UNITY<br/>IOS | SERVER\_INTERNAL\_ERROR<br/>TCGB\_ERROR\_SERVER\_INTERNAL\_ERROR | 8001       | 服务器内部错误                                 |
|                 | Android, UNITY<br/>IOS | SERVER\_REMOTE\_SYSTEM\_ERROR<br/>TCGB\_ERROR\_SERVER\_REMOTE\_SYSTEM\_ERROR | 8002       | 在服务器与外部通信中发生了错误。                        |
|                 | Android, UNITY<br/>IOS | SERVER\_UNKNOWN\_ERROR<br/>TCGB\_ERROR\_SERVER\_UNKNOWN\_ERROR | 8999       | 服务器发生了未知错误。                           |

<br/>
<br/>

## Server
| Module  | Error Code            | Description                              |
| ------- | --------------------- | ---------------------------------------- |
| Common  | -4000001<br/>-4000006 | 使用无效的参数类型调用API <br/>- 例) 该参数声明为int类型，但以string类型调用API。|
|         | -4000002<br/>-4000004 | 必要的参数被省略或缺少值时                  |
|         | -4000003              | Request body中传递了未定义的值          |
|         | -4000005              | 当省略必要的参数，或使用不当的值调用时           |
|         | -4010001              | 调用的APP ID无效                            |
|         | -4010002              | 调用的APP key无效                             |
|         | -4010003              | 在未认证的客户端调用了需要认证的API时      |
|         | -4010004              | 调用的秘钥(secret key)无效                |
|         | -4060001              | HTTP标头中设置的Content-Type不正确             |
|         | -4060002              | 调用了Depreated API版本                     |
|         | -4060003              | 调用了不支持的API版本，或使用无效的URI调用时 |
|         | -4090001 ~ 4          | 内部DB相关错误                              |
|         | -4150001              | 传递了无效格式的JSON数据                      |
|         | -5000001 ~ 15         | 内部系统错误                                |
| Gateway | -4010202              | 调用的APP ID无效                            |
|         | -4010203              | 无效的访问令牌                           |
|         | -4010204              | 禁用/退出（删除数据）/ 账户丢失等无效用户             |
|         | -4040201              | 已调用API的TOAST服务未启用时 <br/>- 例) 在未启用Leaderboard服务的状态下，通过Gamebase调用了Leaderboard API时<br/>或Gamebase本身未启用时 |
|         | -4040202              | 调用了未定义的API时                   |
|         | -4120201              | 在一些提供的SANDBOX 系统中，以实际环境的服务器地址进行API调用 (或者反向调用) <br/>- 如果出现错误，需确认服务器调用地址 |
|         | -5000201 ~ 7          | Gateway内部系统错误                        |
| Member  | -4000402              | 用户ID输入错误时                        |
|         | -4000403              | 请求了无效的用户时                         |
|         | -4000404              | 请求了无效Auth时 |
|         | -4040401              | 请求了不存在或退出（删除数据）的用户时                |
|         | -4100401              | 请求了已退出（删除数据）的用户时                      |
|         | -4220401              | 用户Auth数据不正常时                 |
