## Game > Gamebase > 오류 코드

## Client SDK 

| Category        | Platform           | Error                                    | Error Code | Description                                    |
| --------------- | ------------------ | ---------------------------------------- | ---------- | ---------------------------------------- |
| Common          | Android, UNITY<br/>IOS | NOT_INITIALIZED<br/>TCGB\_ERROR\_NOT\_INITIALIZED | 1          | Gamebase가 초기화되어 있지 않습니다.                 |
|                 | Android, UNITY<br/>IOS | NOT\_LOGGED\_IN<br/>TCGB\_ERROR\_NOT\_LOGGED\_IN | 2          | 로그인이 필요합니다.                              |
|                 | Android, UNITY<br/>IOS | INVALID_PARAMETER<br/>TCGB\_ERROR\_INVALID\_PARAMETER | 3          | 잘못된 파라미터입니다.                             |
|                 | Android, UNITY<br/>IOS | INVALID\_JSON\_FORMAT<br/>TCGB\_ERROR\_INVALID\_JSON\_FORMAT | 4          | JSON 형식 오류입니다.                           |
|                 | Android, UNITY<br/>IOS | USER_PERMISSION<br/>TCGB\_ERROR\_USER\_PERMISSION | 5          | 권한이 없습니다.                                |
|                 | Android, UNITY<br/>IOS | INVALID\_MEMBER<br/>TCGB\_ERROR\_INVALID\_MEMBER  | 6          | 잘못된 회원에 대한 요청입니다.                              |
|                 | Android, UNITY<br/>IOS | BANNED\_MEMBER<br/>TCGB\_ERROR\_BANNED\_MEMBER   | 7         | 제재된 회원입니다.                                |
|                 | Android, UNITY<br/>IOS | SAME\_REQUESTOR<br/>TCGB\_ERROR\_SAME\_REQUESTOR  | 8          | 발급한 TransferKey를 동일한 기기에서 사용했습니다.  |
|                 | Android, UNITY<br/>IOS | NOT\_GUEST\_OR\_HAS\_OTHERS<br/>TCGB\_ERROR\_NOT\_GUEST\_OR\_HAS\_OTHERS | 9          | 게스트가 아닌 계정에서 이전을 시도했거나 계정에 게스트 이외의 IDP가 연동되어 있습니다. |
|                 | Android, UNITY<br/>IOS | NOT_SUPPORTED<br/>TCGB\_ERROR\_NOT\_SUPPORTED | 10         | 지원하지 않는 기능입니다.                           |
|                 | UNITY<br/>IOS      | NOT\_SUPPORTED\_ANDROID<br/>TCGB\_ERROR\_NOT\_SUPPORTED\_ANDROID | 11         | Android에서 지원하지 않는 기능입니다.                 |
|                 | UNITY<br/>IOS      | NOT\_SUPPORTED\_IOS<br/>TCGB\_ERROR\_NOT\_SUPPORTED\_IOS | 12         | iOS에서 지원하지 않는 기능입니다.                     |
|                 | UNITY              | NOT\_SUPPORTED\_UNITY\_EDITOR            | 13         | Editor에서 지원하지 않는 기능입니다.                  |
|                 | UNITY              | NOT\_SUPPORTED\_UNITY\_STANDALONE        | 14         | Standalone에서 지원하지 않는 기능입니다.              |
|                 | UNITY              | NOT\_SUPPORTED\_UNITY\_WEBGL             | 15         | WebGL에서 지원하지 않는 기능입니다.                   |
|                 | Android            | ANDROID\_ACTIVITY\_DESTROYED             | 31         | Activity가 강제종료되었습니다.                       |
|                 | Android            | ANDROID\_ACTIVEAPP\_NOT\_CALLED          | 32         | activeApp API가 호출되지 않았습니다.                 |
|                 | IOS                | IOS_GAMECENTER_DENIED                    | 51         | Gamecenter 로그인이 거부되었습니다.                 |
| Network(Socket) | Android, UNITY<br/>IOS | SOCKET\_RESPONSE\_TIMEOUT<br/>TCGB\_ERROR\_SOCKET\_RESPONSE\_TIMEOUT | 101        | 네트워크 상태가 불안정하여 응답이 없습니다.                 |
|                 | Android, UNITY<br/>IOS | SOCKET_ERROR<br/>TCGB\_ERROR\_SOCKET\_ERROR | 110        | 소켓 오류입니다.                                    |
|                 | Android, UNITY<br/>IOS | UNKNOWN_ERROR<br/>TCGB\_ERROR\_UNKNOWN\_ERROR | 999        | 소켓 알 수 없는 오류입니다.                             |
| Launching       | Android, UNITY<br/>IOS | LAUNCHING\_SERVER\_ERROR<br/>TCGB\_ERROR\_LAUNCHING\_SERVER\_ERROR | 2001       | 론칭 서버 오류입니다.                             |
|                 | Android, UNITY<br/>IOS | LAUNCHING\_NOT\_EXIST\_CLIENT\_ID<br/>TCGB\_ERROR\_LAUNCHING\_NOT\_EXIST\_CLIENT\_ID | 2002       | 클라이언트 ID가 없습니다.                    |
|                 | Android, UNITY<br/>IOS | LAUNCHING\_UNREGISTERED\_APP<br/>TCGB\_ERROR\_LAUNCHING\_UNREGISTERED\_APP | 2003       | 등록되지 않은 앱입니다.                         |
|                 | Android, UNITY<br/>IOS | LAUNCHING\_UNREGISTERED\_CLIENT<br/>TCGB\_ERROR\_LAUNCHING\_UNREGISTERED\_CLIENT | 2004       | 등록되지 않은 클라이언트(버전)입니다.            |
| Auth            | Android, UNITY<br/>IOS | AUTH\_USER\_CANCELED<br/>TCGB\_ERROR\_AUTH\_USER\_CANCELED | 3001       | 로그인이 취소되었습니다.                            |
|                 | Android, UNITY<br/>IOS | AUTH\_NOT\_SUPPORTED\_PROVIDER<br/>TCGB\_ERROR\_AUTH\_NOT\_SUPPORTED\_PROVIDER | 3002       | 지원하지 않는 인증 방식입니다.                        |
|                 | Android, UNITY<br/>IOS | AUTH\_NOT\_EXIST\_MEMBER<br/>TCGB\_ERROR\_AUTH\_NOT\_EXIST\_MEMBER | 3003       | 존재하지 않거나 탈퇴한 회원입니다.                      |
|                 | Android, UNITY<br/>IOS | AUTH\_EXTERNAL\_LIBRARY\_ERROR<br/>TCGB\_ERROR\_AUTH\_EXTERNAL\_LIBRARY\_ERROR | 3009       | 외부 인증 라이브러리 오류입니다.                       |
|                 | Android, UNITY<br/>IOS | AUTH_ALREADY_IN_PROGRESS_ERROR<br/>TCGB_ERROR_AUTH_ALREADY_IN_PROGRESS_ERROR | 3010       | 이전 인증 프로세스가 완료되지 않았습니다.                 |
|                 | Android, UNITY<br/>IOS | AUTH\_TRANSFERKEY\_EXPIRED<br/>TCGB\_ERROR\_AUTH\_TRANSFERKEY\_EXPIRED | 3031       | TransferKey의 유효기간이 만료됐습니다.  |
|                 | Android, UNITY<br/>IOS | AUTH\_TRANSFERKEY\_CONSUMED<br/>TCGB\_ERROR\_AUTH\_TRANSFERKEY\_CONSUMED| 3032       | TransferKey가 이미 사용됐습니다. |
|                 | Android, UNITY<br/>IOS | AUTH\_TRANSFERKEY\_NOT\_EXIST<br/>TCGB\_ERROR\_AUTH\_TRANSFERKEY\_NOT\_EXIST | 3033       | TransferKey가 유효하지 않습니다. |
| Auth (Login)    | Android, UNITY<br/>IOS | AUTH\_TOKEN\_LOGIN\_FAILED<br/>TCGB\_ERROR\_AUTH\_TOKEN\_LOGIN\_FAILED | 3101       | 토큰 로그인에 실패했습니다.                         |
|                 | Android, UNITY<br/>IOS | AUTH\_TOKEN\_LOGIN\_INVALID\_TOKEN\_INFO<br/>TCGB\_ERROR\_AUTH\_TOKEN\_LOGIN\_INVALID\_TOKEN\_INFO | 3102       | 토큰 정보가 유효하지 않습니다.                        |
|                 | Android, UNITY<br/>IOS | AUTH\_TOKEN\_LOGIN\_INVALID\_LAST\_LOGGED\_IN\_IDP<br/>TCGB\_ERROR\_AUTH\_TOKEN\_LOGIN\_INVALID\_LAST\_LOGGED\_IN\_IDP | 3103       | 최근에 로그인한 IdP 정보가 없습니다.                   |
| IDP Login       | Android, UNITY<br/>IOS | AUTH\_IDP\_LOGIN\_FAILED<br/>TCGB\_ERROR\_AUTH\_IDP\_LOGIN\_FAILED | 3201       | IdP 로그인에 실패했습니다.                        |
|                 | Android, UNITY<br/>IOS | AUTH\_IDP\_LOGIN\_INVALID\_IDP\_INFO<br/>TCGB\_ERROR\_AUTH\_IDP\_LOGIN\_INVALID\_IDP\_INFO | 3202       | IdP 정보가 유효하지 않습니다(Console에 해당 IdP 정보가 없습니다). |
| Add Mapping     | Android, UNITY<br/>IOS | AUTH\_ADD\_MAPPING\_FAILED<br/>TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_FAILED | 3301       | 매핑 추가에 실패했습니다.                          |
|                 | Android, UNITY<br/>IOS | AUTH\_ADD\_MAPPING\_ALREADY\_MAPPED\_TO\_OTHER\_MEMBER<br/>TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_ALREADY\_MAPPED\_TO\_OTHER\_MEMBER | 3302       | 이미 다른 멤버에 매핑되어 있습니다.                      |
|                 | Android, UNITY<br/>IOS | AUTH\_ADD\_MAPPING\_ALREADY\_HAS\_SAME\_IDP<br/>TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_ALREADY\_HAS\_SAME\_IDP | 3303       | 이미 같은 IdP에 매핑되어 있습니다.                     |
|                 | Android, UNITY<br/>IOS | AUTH\_ADD\_MAPPING\_INVALID\_IDP\_INFO<br/>TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_INVALID\_IDP\_INFO | 3304       | IdP 정보가 유효하지 않습니다(Console에 해당 IdP 정보가 없습니다). |
|                 | Android, UNITY<br/>IOS | AUTH_ADD_MAPPING_CANNOT_ADD_GUEST_IDP<br/>TCGB_ERROR_AUTH_ADD_MAPPING_CANNOT_ADD_GUEST_IDP | 3305       | Guest IDP로는 AddMapping이 불가능합니다. |
| Remove Mapping  | Android, UNITY<br/>IOS | AUTH\_REMOVE\_MAPPING\_FAILED<br/>TCGB\_ERROR\_AUTH\_REMOVE\_MAPPING\_FAILED | 3401       | 매핑 삭제에 실패했습니다.                          |
|                 | Android, UNITY<br/>IOS | AUTH\_REMOVE\_MAPPING\_LAST\_MAPPED\_IDP<br/>TCGB\_ERROR\_AUTH\_REMOVE\_MAPPING\_LAST\_MAPPED\_IDP | 3402       | 마지막에 매핑된 IdP는 삭제할 수 없습니다.                |
|                 | Android, UNITY<br/>IOS | AUTH\_REMOVE\_MAPPING\_LOGGED\_IN\_IDP<br/>TCGB\_ERROR\_AUTH\_REMOVE\_MAPPING\_LOGGED\_IN\_IDP | 3403       | 현재 로그인되어 있는 IdP입니다.                      |
| Logout          | Android, UNITY<br/>IOS | AUTH\_LOGOUT\_FAILED<br/>TCGB\_ERROR\_AUTH\_LOGOUT\_FAILED | 3501       | 로그아웃에 실패했습니다.                           |
| Withdrawal      | Android, UNITY<br/>IOS | AUTH\_WITHDRAW\_FAILED<br/>TCGB\_ERROR\_AUTH\_WITHDRAW\_FAILED | 3601       | 탈퇴에 실패했습니다.                             |
| Not Playable    | Android, UNITY<br/>IOS | AUTH\_NOT\_PLAYABLE<br/>TCGB\_ERROR\_AUTH\_NOT\_PLAYABLE | 3701       | 플레이할 수 없는 상태입니다(점검 또는 서비스 종료 등).        |
| Auth(Unknown)   | Android, UNITY<br/>IOS | AUTH\_UNKNOWN\_ERROR<br/>TCGB\_ERROR\_AUTH\_UNKNOWN\_ERROR | 3999       | 알 수 없는 오류입니다(정의되지 않은 오류).           |
| Purchase        | Android, UNITY<br/>IOS | PURCHASE\_NOT\_INITIALIZED<br/>TCGB\_ERROR\_PURCHASE\_NOT\_INITIALIZED | 4001       | Gamebase PurchaseAdapter가 초기화되지 않았습니다.   |
|                 | Android, UNITY<br/>IOS | PURCHASE\_USER\_CANCELED<br/>TCGB\_ERROR\_PURCHASE\_USER\_CANCELED | 4002       | 구매가 취소되었습니다.                             |
|                 | Android, UNITY<br/>IOS | PURCHASE\_NOT\_FINISHED\_PREVIOUS\_PURCHASING<br/>TCGB\_ERROR\_PURCHASE\_NOT\_FINISHED\_PREVIOUS\_PURCHASING | 4003       | 이전 구매가 완료되지 않았습니다.                       |
|                 | Android, UNITY<br/>IOS | PURCHASE\_NOT\_ENOUGH\_CASH<br/>TCGB\_ERROR\_PURCHASE\_NOT\_ENOUGH\_CASH | 4004       | 해당 스토어의 캐시가 부족하여 결제할 수 없습니다.             |
|                 | Android, UNITY<br/>IOS | PURCHASE\_NOT\_SUPPORTED\_MARKET<br/>TCGB\_ERROR\_PURCHASE\_NOT\_SUPPORTED\_MARKET | 4010       | 지원하지 않는 스토어입니다.                          |
|                 | Android, UNITY<br/>IOS | PURCHASE\_EXTERNAL\_LIBRARY\_ERROR<br/>TCGB\_ERROR\_PURCHASE\_EXTERNAL\_LIBRARY\_ERROR | 4201       | 외부 IAP 라이브러리 오류입니다.                      |
|                 | Android, UNITY<br/>IOS | PURCHASE\_UNKNOWN\_ERROR<br/>TCGB\_ERROR\_PURCHASE\_UNKNOWN\_ERROR | 4999       | 알 수 없는 구매 오류입니다.                           |
| Push            | Android, UNITY<br/>IOS | PUSH\_EXTERNAL\_LIBRARY\_ERROR<br/>TCGB\_ERROR\_PUSH\_EXTERNAL\_LIBRARY\_ERROR | 5101       | 외부 라이브러리 오류입니다.                          |
|                 | Android, UNITY<br/>IOS | PUSH\_ALREADY\_IN\_PROGRESS\_ERROR<br/>TCGB\_ERROR\_PUSH\_ALREADY\_IN\_PROGRESS\_ERROR | 5102       | 이전 푸시 API 호출이 완료되지 않았습니다.              |
|                 | Android, UNITY<br/>IOS | PUSH\_UNKNOWN\_ERROR<br/>TCGB\_ERROR\_PUSH\_UNKNOWN\_ERROR | 5999       | 알 수 없는 푸시 오류입니다(정의되지 않은 푸시 오류).      |
| UI              | Android, UNITY<br/>IOS | UI\_UNKNOWN\_ERROR<br/>TCGB\_ERROR\_UI\_UNKNOWN\_ERROR | 6999       | 알 수 없는 오류입니다(정의되지 않은 오류).            |
| WebView         | UNITY                  | WEBVIEW\_UNKNOWN\_ERROR 								| 7999       | 웹뷰 호출시 알 수 없는 오류가 발생했습니다.(정의되지 않은 오류).            |
| Server          | Android, UNITY<br/>IOS | SERVER\_INTERNAL\_ERROR<br/>TCGB\_ERROR\_SERVER\_INTERNAL\_ERROR | 8001       | 서버 내부 오류                                 |
|                 | Android, UNITY<br/>IOS | SERVER\_REMOTE\_SYSTEM\_ERROR<br/>TCGB\_ERROR\_SERVER\_REMOTE\_SYSTEM\_ERROR | 8002       | 서버에서 외부 연동 중 오류가 발생했습니다.                        |
|                 | Android, UNITY<br/>IOS | SERVER\_UNKNOWN\_ERROR<br/>TCGB\_ERROR\_SERVER\_UNKNOWN\_ERROR | 8999       | 서버에서 알 수 없는 오류가 발생했습니다.                           |

<br/>
<br/>
## Server
| Module  | Error Code            | Description                              |
| ------- | --------------------- | ---------------------------------------- |
| Common  | -4000001<br/>-4000006 | 잘못된 파라미터 유형으로 API 호출 <br/>예) 파라미터는 int형으로 선언돼 있는데, string형 데이터로 API가 호출됨 |
|         | -4000002<br/>-4000004 | 필수 파라미터가 생략되었거나 값이 없을 때                  |
|         | -4000003              | Request body에 정의되지 않은 값이 전달된 경우          |
|         | -4000005              | 필수 파라미터가 생략되었거나, 부적절한 값으로 호출될 때          |
|         | -4010001              | 잘못된 앱 ID가 호출됨                            |
|         | -4010002              | 잘못된 앱 키가 호출됨                             |
|         | -4010003              | 인증되지 않은 클라이언트에서 인증이 필요한 API를 호출한 경우      |
|         | -4010004              | 잘못된 비밀 키(secret key)가 호출됨                |
|         | -4060001              | HTTP 헤더에 Content-Type을 잘못 설정             |
|         | -4090001 ~ 4          | 내부 DB 관련 오류                              |
|         | -4150001              | 잘못된 형식의 JSON 데이터 전달                      |
|         | -5000001 ~ 15         | 내부 시스템 오류                                |
| Gateway | -4010202              | 잘못된 앱 ID가 호출됨                            |
|         | -4010203              | 유효하지 않은 액세스 토큰                           |
|         | -4010204              | 이용 정지/탈퇴/계정 유실 등 유효하지 않은 사용자             |
|         | -4040201              | 호출한 API에 대한 TOAST 서비스가 활성화되어 있지 않을 때 <br/>예) Leaderboard 서비스를 사용하지 않는 상태에서 Gamebase를 통해 Leaderboard API를 호출할 때 <br/>혹은 Gamebase 자체가 활성화되어 있지 않을 때 |
|         | -4040202              | 정의되어 있지 않은 API를 호출한 경우                   |
|         | -5000201 ~ 7          | Gateway 내부 시스템 오류                        |
| Member  | -4000402              | 사용자 ID를 잘못 입력했을 때                        |
|         | -4000403              | 잘못된 회원을 요청했을 때                         |
|         | -4000404              | 잘못된 Auth를 요청했을 때 |
|         | -4040401              | 존재하지 않거나 탈퇴한 회원을 요청할 때                |
|         | -4100401              | 이미 탈퇴한 회원을 요청할 때                      |
|         | -4220401              | 사용자 Auth 데이터가 정상적이지 않을 때                 |
