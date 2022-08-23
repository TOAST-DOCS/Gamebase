## Game > Gamebase > 错误代码

## Client SDK

| Category        | Platform           | Error                                    | Error Code | Description                                    |
| --------------- | ------------------ | ---------------------------------------- | ---------- | ---------------------------------------- |
| Common          | Android, UNITY<br/>IOS | NOT_INITIALIZED<br/>TCGB\_ERROR\_NOT\_INITIALIZED | 1          | 未初始化Gamebase。                 |
|                 | Android, UNITY<br/>IOS | NOT\_LOGGED\_IN<br/>TCGB\_ERROR\_NOT\_LOGGED\_IN | 2          | 需要登录。                              |
|                 | Android, UNITY<br/>IOS | INVALID_PARAMETER<br/>TCGB\_ERROR\_INVALID\_PARAMETER | 3          | 是无效的参数。                            |
|                 | Android, UNITY<br/>IOS | INVALID\_JSON\_FORMAT<br/>TCGB\_ERROR\_INVALID\_JSON\_FORMAT | 4          | 是JSON格式错误。                           |
|                 | Android, UNITY<br/>IOS | USER_PERMISSION<br/>TCGB\_ERROR\_USER\_PERMISSION | 5          | 无权限。                                |
|                 | Android, UNITY<br/>IOS | INVALID\_MEMBER<br/>TCGB\_ERROR\_INVALID\_MEMBER  | 6          | 是无效的用户请求。                              |
|                 | Android, UNITY<br/>IOS | BANNED\_MEMBER<br/>TCGB\_ERROR\_BANNED\_MEMBER   | 7         | 是被禁用的用户。                                |
|                 | Android, UNITY<br/>IOS | NOT_SUPPORTED<br/>TCGB\_ERROR\_NOT\_SUPPORTED | 10         | 不支持此功能。                           |
|                 | UNITY<br/>IOS          | NOT\_SUPPORTED\_ANDROID<br/>TCGB\_ERROR\_NOT\_SUPPORTED\_ANDROID | 11         | Android不支持此功能。                 |
|                 | UNITY<br/>IOS          | NOT\_SUPPORTED\_IOS<br/>TCGB\_ERROR\_NOT\_SUPPORTED\_IOS | 12         | iOS不支持此功能。                     |
|                 | UNITY                  | NOT\_SUPPORTED\_UNITY\_EDITOR            | 13         | Editor不支持此功能。                  |
|                 | UNITY                  | NOT\_SUPPORTED\_UNITY\_STANDALONE        | 14         | Standalone不支持此功能。              |
|                 | UNITY                  | NOT\_SUPPORTED\_UNITY\_WEBGL             | 15         | WebGL不支持此功能。                   |
|                 | Android                | ANDROID\_ACTIVITY\_DESTROYED             | 31         | Activity强制终止。                       |
|                 | Android                | ANDROID\_ACTIVEAPP\_NOT\_CALLED          | 32         | 未调用activeApp API。                 |
|                 | IOS                    | IOS_GAMECENTER_DENIED                    | 51         | Gamecenter登录被拒绝。                 |
| Network(Socket) | Android, UNITY<br/>IOS | SOCKET\_RESPONSE\_TIMEOUT<br/>TCGB\_ERROR\_SOCKET\_RESPONSE\_TIMEOUT | 101        | 网络状态不稳定，没有响应。                 |
|                 | Android, UNITY<br/>IOS | SOCKET_ERROR<br/>TCGB\_ERROR\_SOCKET\_ERROR | 110        | SOCKET错误                                    |
|                 | Android, UNITY<br/>IOS | SOCKET\_UNKNOWN_ERROR<br/>TCGB\_ERROR\_SOCKET\_UNKNOWN\_ERROR | 999        | SOCKET未知错误                             |
| Launching       | Android, UNITY<br/>IOS | LAUNCHING\_SERVER\_ERROR<br/>TCGB\_ERROR\_LAUNCHING\_SERVER\_ERROR | 2001       | LAUNCHING服务器错误                             |
|                 | Android, UNITY<br/>IOS | LAUNCHING\_NOT\_EXIST\_CLIENT\_ID<br/>TCGB\_ERROR\_LAUNCHING\_NOT\_EXIST\_CLIENT\_ID | 2002       | 不存在客户端ID。                    |
|                 | Android, UNITY<br/>IOS | LAUNCHING\_UNREGISTERED\_APP<br/>TCGB\_ERROR\_LAUNCHING\_UNREGISTERED\_APP | 2003       | 是未登记的APP。                         |
|                 | Android, UNITY<br/>IOS | LAUNCHING\_UNREGISTERED\_CLIENT<br/>TCGB\_ERROR\_LAUNCHING\_UNREGISTERED\_CLIENT | 2004       | 是未登记的客户端（版本）。            |
| Auth            | Android, UNITY<br/>IOS | AUTH\_USER\_CANCELED<br/>TCGB\_ERROR\_AUTH\_USER\_CANCELED | 3001       | 已取消登录。                            |
|                 | Android, UNITY<br/>IOS | AUTH\_NOT\_SUPPORTED\_PROVIDER<br/>TCGB\_ERROR\_AUTH\_NOT\_SUPPORTED\_PROVIDER | 3002       | 是不支持的认证方式。                        |
|                 | Android, UNITY<br/>IOS | AUTH\_NOT\_EXIST\_MEMBER<br/>TCGB\_ERROR\_AUTH\_NOT\_EXIST\_MEMBER | 3003       | 是不存在的或已退出（删除数据）的用户。                      |
|                 | Android, UNITY<br/>IOS | AUTH\_EXTERNAL\_LIBRARY\_INITIALIZATION\_ERROR<br/>TCGB\_ERROR\_AUTH\_EXTERNAL\_LIBRARY\_INITIALIZATION\_ERROR | 3006       | 第三方认证库初始化失败 |
|                 | Android, UNITY<br/>IOS | AUTH\_EXTERNAL\_LIBRARY\_ERROR<br/>TCGB\_ERROR\_AUTH\_EXTERNAL\_LIBRARY\_ERROR | 3009       | 外部认证库错误                       |
|                 | Android, UNITY<br/>IOS | AUTH\_ALREADY\_IN\_PROGRESS\_ERROR<br/>TCGB\_ERROR\_AUTH\_ALREADY\_IN\_PROGRESS\_ERROR | 3010       | 未完成以前的认证过程。                 |
|                 | Android, UNITY<br/>IOS | AUTH\_INVALID\_GAMEBASE\_TOKEN<br/>TCGB\_ERROR\_AUTH\_INVALID\_GAMEBASE\_TOKEN | 3011       | 因为Gamebase Access Token无效，您已注销。<br/>请重新登录。 |
| TransferAccount | Android, UNITY<br/>IOS | SAME\_REQUESTOR<br/>TCGB\_ERROR\_SAME\_REQUESTOR | 8 | 在同一台设备上使用了相同的TransferKey。  |
|                 | Android, UNITY<br/>IOS | NOT\_GUEST\_OR\_HAS\_OTHERS<br/>TCGB\_ERROR\_NOT\_GUEST\_OR\_HAS\_OTHERS | 9 | 非游客帐户尝试了转移或帐户已关联了游客以外的IDP。 |
|                 | Android, UNITY<br/>IOS | AUTH\_TRANSFERACCOUNT\_EXPIRED<br/>TCGB\_ERROR\_AUTH\_TRANSFERACCOUNT\_EXPIRED | 3041 | TransferAccount的有效期已过。|
|                 | Android, UNITY<br/>IOS | AUTH\_TRANSFERACCOUNT\_BLOCK<br/>TCGB\_ERROR\_AUTH\_TRANSFERACCOUNT\_BLOCK | 3042 | 因连续输入错误的TransferAccount，账号转移被封。 |
|                 | Android, UNITY<br/>IOS | AUTH\_TRANSFERACCOUNT\_INVALID\_ID<br/>TCGB\_ERROR\_AUTH\_TRANSFERACCOUNT\_INVALID\_ID | 3043 | TransferAccount的Id无效。|
|                 | Android, UNITY<br/>IOS | AUTH\_TRANSFERACCOUNT\_INVALID\_PASSWORD<br/>TCGB\_ERROR\_AUTH\_TRANSFERACCOUNT\_INVALID\_PASSWORD | 3044 | TransferAccount的Password无效。|
|                 | Android, UNITY<br/>IOS | AUTH\_TRANSFERACCOUNT\_CONSOLE\_NO\_CONDITION<br/>TCGB\_ERROR\_AUTH\_TRANSFERACCOUNT\_CONSOLE\_NO\_CONDITION | 3045 | 未设置TransferAccount。<br/>请先在NHN Cloud Gamebase Console中进行设置。 |
|                 | Android, UNITY<br/>IOS | AUTH\_TRANSFERACCOUNT\_NOT\_EXIST<br/>TCGB\_ERROR\_AUTH\_TRANSFERACCOUNT\_NOT\_EXIST | 3046 | 不存在TransferAccount。请先获取TransferAccount。|
|                 | Android, UNITY<br/>IOS | AUTH\_TRANSFERACCOUNT\_ALREADY\_EXIST\_ID<br/>TCGB\_ERROR\_AUTH\_TRANSFERACCOUNT\_ALREADY\_EXIST\_ID | 3047 | 已存在此TransferAccount。|
|                 | Android, UNITY<br/>IOS | AUTH\_TRANSFERACCOUNT\_ALREADY\_USED<br/>TCGB\_ERROR\_AUTH\_TRANSFERACCOUNT\_ALREADY\_USED | 3048 | 已使用此TransferAccount。|
| Auth (Login)    | Android, UNITY<br/>IOS | AUTH\_TOKEN\_LOGIN\_FAILED<br/>TCGB\_ERROR\_AUTH\_TOKEN\_LOGIN\_FAILED | 3101       | 令牌登录失败                         |
|                 | Android, UNITY<br/>IOS | AUTH\_TOKEN\_LOGIN\_INVALID\_TOKEN\_INFO<br/>TCGB\_ERROR\_AUTH\_TOKEN\_LOGIN\_INVALID\_TOKEN\_INFO | 3102       | 是无效的令牌信息。                        |
|                 | Android, UNITY<br/>IOS | AUTH\_TOKEN\_LOGIN\_INVALID\_LAST\_LOGGED\_IN\_IDP<br/>TCGB\_ERROR\_AUTH\_TOKEN\_LOGIN\_INVALID\_LAST\_LOGGED\_IN\_IDP | 3103       | 是无近期登录的IdP信息。                   |
| IdP Login       | Android, UNITY<br/>IOS | AUTH\_IDP\_LOGIN\_FAILED<br/>TCGB\_ERROR\_AUTH\_IDP\_LOGIN\_FAILED | 3201       | IdP登录失败                         |
|                 | Android, UNITY<br/>IOS | AUTH\_IDP\_LOGIN\_INVALID\_IDP\_INFO<br/>TCGB\_ERROR\_AUTH\_IDP\_LOGIN\_INVALID\_IDP\_INFO | 3202       | 是无效的IdP信息（Console中没有此IdP信息）。 |
| Add Mapping     | Android, UNITY<br/>IOS | AUTH\_ADD\_MAPPING\_FAILED<br/>TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_FAILED | 3301       | 添加映射（Mapping）失败                          |
|                 | Android, UNITY<br/>IOS | AUTH\_ADD\_MAPPING\_ALREADY\_MAPPED\_TO\_OTHER\_MEMBER<br/>TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_ALREADY\_MAPPED\_TO\_OTHER\_MEMBER | 3302       | 已经与其他帐户进行了映射（Mapping）。                      |
|                 | Android, UNITY<br/>IOS | AUTH\_ADD\_MAPPING\_ALREADY\_HAS\_SAME\_IDP<br/>TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_ALREADY\_HAS\_SAME\_IDP | 3303       | 已映射（Mapping）到相同的IdP。                     |
|                 | Android, UNITY<br/>IOS | AUTH\_ADD\_MAPPING\_INVALID\_IDP\_INFO<br/>TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_INVALID\_IDP\_INFO | 3304       | 是无效的IdP信息（Console中没有此IdP信息）。|
|                 | Android, UNITY<br/>IOS | AUTH\_ADD\_MAPPING\_CANNOT\_ADD\_GUEST\_IDP<br/>TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_CANNOT\_ADD\_GUEST\_IDP | 3305       | 游客IDP无法进行AddMapping。|
| Add Mapping Forcibly | Android, UNITY<br/>IOS | AUTH\_ADD\_MAPPING\_FORCIBLY\_NOT\_EXIST\_KEY<br/>TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_FORCIBLY\_NOT\_EXIST\_KEY | 3311       | 强制映射密钥(ForcingMappingKey)不存在，<br/>请再确认ForcingMappingTicket。 |
|                      | Android, UNITY<br/>IOS | AUTH\_ADD\_MAPPING\_FORCIBLY\_ALREADY\_USED\_KEY<br/>TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_FORCIBLY\_ALREADY\_USED\_KEY | 3312       | 已使用强制映射密钥(ForcingMappingKey)。|
|                      | Android, UNITY<br/>IOS | AUTH\_ADD\_MAPPING\_FORCIBLY\_EXPIRED\_KEY<br/>TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_FORCIBLY\_EXPIRED\_KEY | 3313       | 强制映射密钥(ForcingMappingKey)的有效期已过。|
|                      | Android, UNITY<br/>IOS | AUTH\_ADD\_MAPPING\_FORCIBLY\_DIFFERENT\_IDP<br/>TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_FORCIBLY\_DIFFERENT\_IDP | 3314       | 强制映射密钥(ForcingMappingKey)已用于其他IdP。 <br/>对同样的IdP进行强制映射时需使用获取的ForcingMappingKey。 |
|                      | Android, UNITY<br/>IOS | AUTH\_ADD\_MAPPING\_FORCIBLY\_DIFFERENT\_AUTHKEY<br/>TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_FORCIBLY\_DIFFERENT\_AUTHKEY | 3315       | 强制映射密钥(ForcingMappingKey)已用于其他账号。<br/>是对同样的IdP进行强制映射时需使用获取的ForcingMappingKey。 |
| Remove Mapping  | Android, UNITY<br/>IOS | AUTH\_REMOVE\_MAPPING\_FAILED<br/>TCGB\_ERROR\_AUTH\_REMOVE\_MAPPING\_FAILED | 3401       | 解除映射（Mapping）失败                          |
|                 | Android, UNITY<br/>IOS | AUTH\_REMOVE\_MAPPING\_LAST\_MAPPED\_IDP<br/>TCGB\_ERROR\_AUTH\_REMOVE\_MAPPING\_LAST\_MAPPED\_IDP | 3402       | 无法解除最后映射（Mapping）的IdP。                |
|                 | Android, UNITY<br/>IOS | AUTH\_REMOVE\_MAPPING\_LOGGED\_IN\_IDP<br/>TCGB\_ERROR\_AUTH\_REMOVE\_MAPPING\_LOGGED\_IN\_IDP | 3403       | 是当前登录的IdP。                      |
| Logout          | Android, UNITY<br/>IOS | AUTH\_LOGOUT\_FAILED<br/>TCGB\_ERROR\_AUTH\_LOGOUT\_FAILED | 3501       | 退出登录失败                           |
| Withdrawal      | Android, UNITY<br/>IOS | AUTH\_WITHDRAW\_FAILED<br/>TCGB\_ERROR\_AUTH\_WITHDRAW\_FAILED | 3601       | 退出（删除数据）失败                             |
|                 | Android, UNITY<br/>IOS | AUTH\_WITHDRAW\_ALREADY\_TEMPORARY\_WITHDRAW<br/>TCGB\_ERROR\_AUTH\_WITHDRAW\_ALREADY\_TEMPORARY\_WITHDRAW | 3602   | 用户已临时退出。|
|                 | Android, UNITY<br/>IOS | AUTH\_WITHDRAW\_NOT\_TEMPORARY\_WITHDRAW<br/>TCGB\_ERROR\_AUTH\_WITHDRAW\_NOT\_TEMPORARY\_WITHDRAW | 3603       | 用户未临时退出。|
| Not Playable    | Android, UNITY<br/>IOS | AUTH\_NOT\_PLAYABLE<br/>TCGB\_ERROR\_AUTH\_NOT\_PLAYABLE | 3701       | 是无法玩游戏的状态(维护或已下线等)。       |
| Auth(Unknown)   | Android, UNITY<br/>IOS | AUTH\_UNKNOWN\_ERROR<br/>TCGB\_ERROR\_AUTH\_UNKNOWN\_ERROR | 3999       | 是未知错误(未定义的错误)。          |
| Purchase        | Android, UNITY<br/>IOS | PURCHASE\_NOT\_INITIALIZED<br/>TCGB\_ERROR\_PURCHASE\_NOT\_INITIALIZED | 4001       | 未初始化Gamebase PurchaseAdapter模块。   |
|                 | Android, UNITY<br/>IOS | PURCHASE\_USER\_CANCELED<br/>TCGB\_ERROR\_PURCHASE\_USER\_CANCELED | 4002       | 取消购买。                             |
|                 | Android, UNITY<br/>IOS | PURCHASE\_NOT\_FINISHED\_PREVIOUS\_PURCHASING<br/>TCGB\_ERROR\_PURCHASE\_NOT\_FINISHED\_PREVIOUS\_PURCHASING | 4003       | 尚未完成上一次购买。                       |
|                 | UNITY                  | PURCHASE\_NOT\_ENOUGH\_CASH                                        | 4004       | 该商店的余额不足，无法结算。             |
|                 | Android, UNITY<br/>IOS | PURCHASE\_INACTIVE\_PRODUCT\_ID<br/>TCGB\_ERROR\_PURCHASE\_INACTIVE\_PRODUCT\_ID | 4005       | 此商品为非激活状态。  |
|                 | Android, UNITY<br/>IOS | PURCHASE\_NOT\_EXIST\_PRODUCT\_ID<br/>TCGB\_ERROR\_PURCHASE\_NOT\_EXIST\_PRODUCT\_ID | 4006       | 您使用不存在的GamebaseProductID请求了支付。 |
|                 | Android, UNITY<br/>IOS | PURCHASE\_LIMIT\_EXCEEDED<br/>TCGB\_ERROR\_PURCHASE\_LIMIT\_EXCEEDED | 4007       | 超过了一个月购买限额。 |
|                 | Android, UNITY<br/>IOS | PURCHASE\_NOT\_SUPPORTED\_MARKET<br/>TCGB\_ERROR\_PURCHASE\_NOT\_SUPPORTED\_MARKET | 4010       | 是不支持的商店。                          |
|                 | Android, UNITY<br/>IOS | PURCHASE\_EXTERNAL\_LIBRARY\_ERROR<br/>TCGB\_ERROR\_PURCHASE\_EXTERNAL\_LIBRARY\_ERROR | 4201       | 外部IAP库错误                     |
|                 | Android, UNITY<br/>IOS | PURCHASE\_UNKNOWN\_ERROR<br/>TCGB\_ERROR\_PURCHASE\_UNKNOWN\_ERROR | 4999       | 未知的购买错误                           |
| Push            | Android, UNITY<br/>IOS | PUSH\_EXTERNAL\_LIBRARY\_ERROR<br/>TCGB\_ERROR\_PUSH\_EXTERNAL\_LIBRARY\_ERROR | 5101       | 是外部库错误                          |
|                 | Android, UNITY<br/>IOS | PUSH\_ALREADY\_IN\_PROGRESS\_ERROR<br/>TCGB\_ERROR\_PUSH\_ALREADY\_IN\_PROGRESS\_ERROR | 5102       | 上一次的推送API调用未完成。              |
|                 | Android, UNITY<br/>IOS | PUSH\_UNKNOWN\_ERROR<br/>TCGB\_ERROR\_PUSH\_UNKNOWN\_ERROR | 5999       | 是未知推送错误(未定义的推送错误)。      |
| UI              | Android, UNITY<br/>IOS | UI\_IMAGE\_NOTICE\_TIMEOUT<br/>TCGB\_ERROR\_UI\_IMAGE\_NOTICE\_TIMEOUT | 6901       | 显示ImageNotice时出现timeout。            |
|                 | Android, UNITY<br/>IOS | UI\_CONTACT\_FAIL\_INVALID\_URL<br/>TCGB\_ERROR\_UI\_CONTACT\_FAIL\_INVALID\_URL | 6911       | 客户服务Webview URL创建失败               |
|                 | Android, UNITY<br/>IOS | UI\_CONTACT\_FAIL\_FAIL\_ISSUE\_SHORT\_TERM\_TICKET<br/>TCGB\_ERROR\_UI\_CONTACT\_FAIL\_ISSUE\_SHORT\_TERM\_TICKET | 6912       | 无法提供识别用户的临时Ticket。           |
|                 | Android, UNITY<br/>IOS | UI\_TERMS\_NOT\_EXIST\_IN\_CONSOLE<br/>TCGB\_ERROR\_UI\_TERMS\_NOT\_EXIST\_IN\_CONSOLE | 6921       | 在控制台中未注册条款信息。|
|                 | Android, UNITY<br/>IOS | UI\_TERMS\_NOT\_EXIST\_FOR\_DEVICE\_COUNTRY<br/>TCGB\_ERROR\_UI\_TERMS\_NOT\_EXIST\_FOR\_DEVICE\_COUNTRY | 6922       | 在控制台中未注册符合终端机国家代码的条款信息。|
|                 | Android, UNITY<br/>IOS | UI\_TERMS\_UNREGISTERED\_SEQ<br/>TCGB\_ERROR\_UI\_TERMS\_UNREGISTERED\_SEQ | 6923       | 是未注册的条款Seq值。            |
|                 | Android, UNITY<br/>IOS | UI\_TERMS\_ALREADY\_IN\_PROGRESS\_ERROR<br/>TCGB\_ERROR\_UI\_TERMS\_ALREADY\_IN\_PROGRESS\_ERROR | 6924       | 未完成Terms API的调用。<br/>请稍后再试。|
|                 | Android, UNITY         | UI\_TERMS\_ANDROID\_DUPLICATED\_VIEW | 6925       | 条款Webview未终止的状态下再次被调用。|                                                       
|                 | Android, UNITY<br/>IOS | UI\_UNKNOWN\_ERROR<br/>TCGB\_ERROR\_UI\_UNKNOWN\_ERROR | 6999       | 是未知错误(未定义的错误)。            |
| WebView         | Android, UNITY<br/>IOS | WEBVIEW\_INVALID\_URL<br/>TCGB\_ERROR\_WEBVIEW\_INVALID\_URL           | 7001       | URL出错。           |
|                 | Android, UNITY<br/>IOS | WEBVIEW\_TIMEOUT<br/>TCGB\_ERROR\_WEBVIEW\_TIMEOUT                     | 7002       | 显示Webview时出现了超时错误。            |
|                 | Android, UNITY<br/>IOS | WEBVIEW\_HTTP\_ERROR<br/>TCGB\_ERROR\_WEBVIEW\_HTTP\_ERROR 	        | 7003       | 因HTTP错误，无法显示Webview。            |
|                 | Android, UNITY         | WEBVIEW\_OPENED\_NEW\_BROWSER\_BEFORE\_CLOSE                           | 7004       | 关闭Browser形式的Webview之前显示了新的Webview。 |
|                 | UNITY                  | WEBVIEW\_UNKNOWN\_ERROR 								| 7999       | 调用Webview时发生了未知错误。(未定义的错误)。            |
| Server          | Android, UNITY<br/>IOS | SERVER\_INTERNAL\_ERROR<br/>TCGB\_ERROR\_SERVER\_INTERNAL\_ERROR | 8001       | 服务器内部错误                                 |
|                 | Android, UNITY<br/>IOS | SERVER\_REMOTE\_SYSTEM\_ERROR<br/>TCGB\_ERROR\_SERVER\_REMOTE\_SYSTEM\_ERROR | 8002       | 服务器与外部通信时发生了错误。                        |
|                 | Android, UNITY<br/>IOS | SERVER\_UNKNOWN\_ERROR<br/>TCGB\_ERROR\_SERVER\_UNKNOWN\_ERROR | 8999       | 在服务器发生了未知错误。                           |

<br/>
<br/>

## Server
| Module  | Error Code            | Description                              |
| ------- | --------------------- | ---------------------------------------- |
| Common  | -4000001<br/>-4000006 | 使用无效的参数类型调用API。<br/>- 例) 该参数声明为int类型，但以string类型调用API。|
|         | -4000002<br/>-4000004<br>-4000005 | 当省略必要的参数，或使用不当的值调用时 |
|         | -4000003              | Request body中传递了未定义的值。 |
|         | -4000007              | 调用不支持的API版本。 |
|         | -4000008              | 已超过参数长度。 |
|         | -4010001              | 调用的APP ID无效。 |
|         | -4010002              | 调用的APP key无效。 |
|         | -4010003              | 在未认证的客户端调用了需要认证的API时 |
|         | -4010004              | 调用的秘钥(secret key)无效 |
|         | -4060001              | HTTP标头中设置的Content-Type不正确。|
|         | -4060002              | 调用了Depreated API版本 |
|         | -4060003              | 调用了不支持的API版本，或使用无效的URI调用时 |
|         | -4090001 ~ 4          | 内部DB相关错误 |
|         | -4150001              | 传递了无效格式的JSON数据。 |
|         | -5000001 ~ 15         | 内部系统错误 |
| Gateway | -4010202              | 调用的APP ID无效 |
|         | -4010203              | 无效的Access Token |
|         | -4010204              | 禁用/退出（删除数据）/ 账户丢失等无效用户 |
|         | -4010208              | Gamebase Access Token已过期或IdP Access Token已过期。|
|         | -4040201              | 已调用API的NHN Cloud服务未启用时 <br/>- 例) 在未启用Leaderboard服务的状态下，通过Gamebase调用了Leaderboard API时<br/>或Gamebase本身未启用时 |
|         | -4040202              | 调用了未定义的API时 |
|         | -4120201              | 在一些提供的SANDBOX 系统中，以实际环境的服务器地址进行API调用 (或者反向调用) <br/>- 如果出现错误，需确认服务器调用地址 |
|         | -5000201 ~ 8          | Gateway内部系统错误 |
| Launching | -4040303              | 若想通过尚未注册的客户端版本请求初始化，<br>需要在控制台中查询注册的客户端版本和商店代码。 |
| Member  | -4000402              | 用户ID输入错误时 |
|         | -4000403              | 请求了无效的用户时 |
|         | -4000404              | 请求了无效Auth时 |
|         | -4000409              | 在相同终端机中使用为迁移终端机获得的TransferAccount信息 |
|         | -4040401              | 请求了不存在或退出（删除数据）的用户时 |
|         | -4040403              | 请求了不存在的TransferAccount时 |
|         | -4100402              | 请求了已使用的TransferAccount时 |
|         | -4100403              | 请求了有效期结束的TransferAccount时 |
|         | -4100401              | 请求了已退出（删除数据）的用户时 |
|         | -4220401              | 用户Auth数据不正常时 |
| IdP     | -4000901              | 被禁用的(block)用户请求TransferAccount验证时 |
|         | -4040920              | TransferAccount有效性验证中，ID不存在时 |
|         | -4000921 ~ 2          | 发放TransferAccount时发生内部错误。持续发生时需要通过客户服务咨询 |
|         | -4000923              | 若欲按照用户手册(MANUAL)方式更改PASSWORD，输入与当前PASSWORD相同的字符串 |
|         | -4000924              | 内部错误/若反复发生，需咨询客户服务。|
|         | -4000925              | 若欲按照用户手册(MANUAL)方式更改ID，输入当前使用中的ID。|
|         | -4000927              | TransferAccount有效性验证中，PASSWORD错误时 |
|         | -4040920              | TransferAccount有效性验证中，ID不存在时 |
|         | -5110920              | TransferAccount发放系统错误。持续发生时需要通过客户服务咨询。|
| Coupon  | -100002<br>-4000205   | 是无效的优惠券代码。<br>- 用户以错误的优惠券代码请求消费。<br>- 在控制台中注册的优惠券状态为非激活状态。<br>- 在控制台中指定商店时，商店代码被省略或调用错误的商店代码。 |
|         | -100004               | 依据请求已手动完成处理的优惠券代码。|
|         | -100005               | 已使用的优惠券 |
|         | -100007 ~ 8           | 非优惠券使用期时 |
|         | -100011               | 超出可使用的优惠券限制数量时 |
|         | -999999               | 优惠券系统内部错误。持续发生时需要通过客服中心咨询。|
| IAP     | 5000                  | CONSUME FAILED |
|         | 5018                  | 已进行消费的支付项 |
|         | 1100                  | 省略必需参数并传输错误的参数 |
|         | 5032                  | 传送错误形式的JSON数据。|
|         | 9999                  | UNKNOWN ERROR |
|         | -4001002              | 是无效的Gamebase商品。|
