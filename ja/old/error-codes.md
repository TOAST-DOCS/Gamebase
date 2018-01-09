## Game > Gamebase > Error Code

## Client SDK 

| Category        | Platform           | Error                                    | Error Code | Description                                    |
| --------------- | ------------------ | ---------------------------------------- | ---------- | ---------------------------------------- |
| Common          | AOS, UNITY<br/>IOS | NOT_INITIALIZED<br/>TCGB\_ERROR\_NOT\_INITIALIZED | 1          | Gamebase 초기화가 되어있지 않습니다.                 |
|                 | AOS, UNITY<br/>IOS | NOT\_LOGGED\_IN<br/>TCGB\_ERROR\_NOT\_LOGGED\_IN | 2          | 로그인이 필요합니다.                              |
|                 | AOS, UNITY<br/>IOS | INVALID_PARAMETER<br/>TCGB\_ERROR\_INVALID\_PARAMETER | 3          | 잘못된 파라미터입니다.                             |
|                 | AOS, UNITY<br/>IOS | INVALID\_JSON\_FORMAT<br/>TCGB\_ERROR\_INVALID\_JSON\_FORMAT | 4          | JSON 포맷 에러입니다.                           |
|                 | AOS, UNITY<br/>IOS | USER_PERMISSION<br/>TCGB\_ERROR\_USER\_PERMISSION | 5          | 권한이 없습니다.                                |
|                 | AOS, UNITY<br/>IOS | NOT_SUPPORTED<br/>TCGB\_ERROR\_NOT\_SUPPORTED | 10         | 지원하지 않는 기능입니다.                           |
|                 | UNITY<br/>IOS      | NOT\_SUPPORTED\_ANDROID<br/>TCGB\_ERROR\_NOT\_SUPPORTED\_ANDROID | 11         | Android에서 지원하지 않는 기능입니다.                 |
|                 | UNITY<br/>IOS      | NOT\_SUPPORTED\_IOS<br/>TCGB\_ERROR\_NOT\_SUPPORTED\_IOS | 12         | iOS에서 지원하지 않는 기능입니다.                     |
|                 | UNITY              | NOT\_SUPPORTED\_UNITY\_EDITOR            | 13         | Editor에서 지원하지 않는 기능입니다.                  |
|                 | UNITY              | NOT\_SUPPORTED\_UNITY\_STANDALONE        | 14         | Standalone에서 지원하지 않는 기능입니다.              |
|                 | UNITY              | NOT\_SUPPORTED\_UNITY\_WEBGL             | 15         | WebGL에서 지원하지 않는 기능입니다.                   |
| Network(Socket) | AOS, UNITY<br/>IOS | SOCKET\_RESPONSE\_TIMEOUT<br/>TCGB\_ERROR\_SOCKET\_RESPONSE\_TIMEOUT | 101        | 네트워크 상태가 불안정하여 응답이 없습니다.                 |
|                 | AOS, UNITY<br/>IOS | SOCKET_ERROR<br/>TCGB\_ERROR\_SOCKET\_ERROR | 110        | 소켓 에러                                    |
|                 | AOS, UNITY<br/>IOS | UNKNOWN_ERROR<br/>TCGB\_ERROR\_UNKNOWN\_ERROR | 999        | 소켓 알 수 없는 에러                             |
| Launching       | AOS, UNITY<br/>IOS | LAUNCHING\_SERVER\_ERROR<br/>TCGB\_ERROR\_LAUNCHING\_SERVER\_ERROR | 2001       | 런칭 서버 에러입니다.                             |
|                 | AOS, UNITY<br/>IOS | LAUNCHING\_NOT\_EXIST\_CLIENT\_ID<br/>TCGB\_ERROR\_LAUNCHING\_NOT\_EXIST\_CLIENT\_ID | 2002       | Client ID가 존재하지 않습니다.                    |
|                 | AOS, UNITY<br/>IOS | LAUNCHING\_UNREGISTERED\_APP<br/>TCGB\_ERROR\_LAUNCHING\_UNREGISTERED\_APP | 2003       | 등록되지 않은 App 입니다.                         |
|                 | AOS, UNITY<br/>IOS | LAUNCHING\_UNREGISTERED\_CLIENT<br/>TCGB\_ERROR\_LAUNCHING\_UNREGISTERED\_CLIENT | 2004       | 등록되지 않은 Client (version) 입니다.            |
| Auth            | AOS, UNITY<br/>IOS | AUTH\_USER\_CANCELED<br/>TCGB\_ERROR\_AUTH\_USER\_CANCELED | 3001       | 로그인이 취소되었습니다.                            |
|                 | AOS, UNITY<br/>IOS | AUTH\_NOT\_SUPPORTED\_PROVIDER<br/>TCGB\_ERROR\_AUTH\_NOT\_SUPPORTED\_PROVIDER | 3002       | 지원하지 않는 인증 방식입니다.                        |
|                 | AOS, UNITY<br/>IOS | AUTH\_NOT\_EXIST\_MEMBER<br/>TCGB\_ERROR\_AUTH\_NOT\_EXIST\_MEMBER | 3003       | 존재하지 않거나 탈퇴한 회원입니다.                      |
|                 | AOS, UNITY<br/>IOS | AUTH\_INVALID\_MEMBER<br/>TCGB\_ERROR\_AUTH\_INVALID\_MEMBER | 3004       | 잘못된 회원에 대한 요청입니다.                        |
|                 | AOS, UNITY<br/>IOS | AUTH\_INVALID\_MEMBER<br/>TCGB\_ERROR\_AUTH\_BANNED\_MEMBER | 3005       | 제재된 회원입니다.                               |
|                 | AOS, UNITY<br/>IOS | AUTH\_EXTERNAL\_LIBRARY\_ERROR<br/>TCGB\_ERROR\_AUTH\_EXTERNAL\_LIBRARY\_ERROR | 3009       | 외부 인증 라이브러리 에러입니다.                       |
| Auth (Login)    | AOS, UNITY<br/>IOS | AUTH\_TOKEN\_LOGIN\_FAILED<br/>TCGB\_ERROR\_AUTH\_TOKEN\_LOGIN\_FAILED | 3101       | 토큰 로그인에 실패하였습니다.                         |
|                 | AOS, UNITY<br/>IOS | AUTH\_TOKEN\_LOGIN\_INVALID\_TOKEN\_INFO<br/>TCGB\_ERROR\_AUTH\_TOKEN\_LOGIN\_INVALID\_TOKEN\_INFO | 3102       | 토큰 정보가 유효하지 않습니다.                        |
|                 | AOS, UNITY<br/>IOS | AUTH\_TOKEN\_LOGIN\_INVALID\_LAST\_LOGGED\_IN\_IDP<br/>TCGB\_ERROR\_AUTH\_TOKEN\_LOGIN\_INVALID\_LAST\_LOGGED\_IN\_IDP | 3103       | 최근에 로그인한 IdP 정보가 없습니다.                   |
| Idp Login       | AOS, UNITY<br/>IOS | AUTH\_IDP\_LOGIN\_FAILED<br/>TCGB\_ERROR\_AUTH\_IDP\_LOGIN\_FAILED | 3201       | Idp 로그인에 실패하였습니다.                        |
|                 | AOS, UNITY<br/>IOS | AUTH\_IDP\_LOGIN\_INVALID\_IDP\_INFO<br/>TCGB\_ERROR\_AUTH\_IDP\_LOGIN\_INVALID\_IDP\_INFO | 3202       | Idp 정보가 유효하지 않습니다. (Console에 해당 Idp 정보가 없습니다.) |
| Add Mapping     | AOS, UNITY<br/>IOS | AUTH\_ADD\_MAPPING\_FAILED<br/>TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_FAILED | 3301       | 맵핑 추가에 실패하였습니다.                          |
|                 | AOS, UNITY<br/>IOS | AUTH\_ADD\_MAPPING\_ALREADY\_MAPPED\_TO\_OTHER\_MEMBER<br/>TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_ALREADY\_MAPPED\_TO\_OTHER\_MEMBER | 3302       | 이미 다른 멤버에 맵핑되어있습니다.                      |
|                 | AOS, UNITY<br/>IOS | AUTH\_ADD\_MAPPING\_ALREADY\_HAS\_SAME\_IDP<br/>TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_ALREADY\_HAS\_SAME\_IDP | 3303       | 이미 같은 IDP에 맵핑되어있습니다.                     |
|                 | AOS, UNITY<br/>IOS | AUTH\_ADD\_MAPPING\_INVALID\_IDP\_INFO<br/>TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_INVALID\_IDP\_INFO | 3304       | Idp 정보가 유효하지 않습니다. (Console에 해당 Idp 정보가 없습니다.) |
| Remove Mapping  | AOS, UNITY<br/>IOS | AUTH\_REMOVE\_MAPPING\_FAILED<br/>TCGB\_ERROR\_AUTH\_REMOVE\_MAPPING\_FAILED | 3401       | 맵핑 삭제에 실패하였습니다.                          |
|                 | AOS, UNITY<br/>IOS | AUTH\_REMOVE\_MAPPING\_LAST\_MAPPED\_IDP<br/>TCGB\_ERROR\_AUTH\_REMOVE\_MAPPING\_LAST\_MAPPED\_IDP | 3402       | 마지막에 맵핑된 Idp는 삭제할 수 없습니다.                |
|                 | AOS, UNITY<br/>IOS | AUTH\_REMOVE\_MAPPING\_LOGGED\_IN\_IDP<br/>TCGB\_ERROR\_AUTH\_REMOVE\_MAPPING\_LOGGED\_IN\_IDP | 3403       | 현재 로그인되어있는 Idp 입니다.                      |
| Logout          | AOS, UNITY<br/>IOS | AUTH\_LOGOUT\_FAILED<br/>TCGB\_ERROR\_AUTH\_LOGOUT\_FAILED | 3501       | 로그아웃에 실패하였습니다.                           |
| Withdrawal      | AOS, UNITY<br/>IOS | AUTH\_WITHDRAW\_FAILED<br/>TCGB\_ERROR\_AUTH\_WITHDRAW\_FAILED | 3601       | 탈퇴에 실패하였습니다.                             |
| Not Playable    | AOS, UNITY<br/>IOS | AUTH\_NOT\_PLAYABLE<br/>TCGB\_ERROR\_AUTH\_NOT\_PLAYABLE | 3701       | 플레이할 수 없는 상태입니다. (점검 또는 서비스 종료 등)        |
| Auth(Unknown)   | AOS, UNITY<br/>IOS | AUTH\_UNKNOWN\_ERROR<br/>TCGB\_ERROR\_AUTH\_UNKNOWN\_ERROR | 3999       | 알수 없는 에러입니다. (정의 되지 않은 에러입니다.)           |
| Purchase        | AOS, UNITY<br/>IOS | PURCHASE\_NOT\_INITIALIZED<br/>TCGB\_ERROR\_PURCHASE\_NOT\_INITIALIZED | 4001       | Gamebase PurchaseAdapter가 초기화되지 않았습니다.   |
|                 | AOS, UNITY<br/>IOS | PURCHASE\_USER\_CANCELED<br/>TCGB\_ERROR\_PURCHASE\_USER\_CANCELED | 4002       | 구매가 취소되었습니다.                             |
|                 | AOS, UNITY<br/>IOS | PURCHASE\_NOT\_FINISHED\_PREVIOUS\_PURCHASING<br/>TCGB\_ERROR\_PURCHASE\_NOT\_FINISHED\_PREVIOUS\_PURCHASING | 4003       | 이전 구매가 완료되지 않았습니다.                       |
|                 | AOS, UNITY<br/>IOS | PURCHASE\_NOT\_ENOUGH\_CASH<br/>TCGB\_ERROR\_PURCHASE\_NOT\_ENOUGH\_CASH | 4004       | 해당 스토어의 캐쉬가 부족하여 결제할 수 없습니다.             |
|                 | AOS, UNITY<br/>IOS | PURCHASE\_NOT\_SUPPORTED\_MARKET<br/>TCGB\_ERROR\_PURCHASE\_NOT\_SUPPORTED\_MARKET | 4010       | 지원하지 않는 스토어입니다.                          |
|                 | AOS, UNITY<br/>IOS | PURCHASE\_EXTERNAL\_LIBRARY\_ERROR<br/>TCGB\_ERROR\_PURCHASE\_EXTERNAL\_LIBRARY\_ERROR | 4201       | 외부 IAP 라이브러리 에러입니다.                      |
|                 | AOS, UNITY<br/>IOS | PURCHASE\_UNKNOWN\_ERROR<br/>TCGB\_ERROR\_PURCHASE\_UNKNOWN\_ERROR | 4999       | 알수없는 구매 에러입니다.                           |
| Push            | AOS, UNITY<br/>IOS | PUSH\_EXTERNAL\_LIBRARY\_ERROR<br/>TCGB\_ERROR\_PUSH\_EXTERNAL\_LIBRARY\_ERROR | 5101       | 외부 라이브러리 에러입니다.                          |
|                 | AOS, UNITY<br/>IOS | PUSH\_ALREADY\_IN\_PROGRESS\_ERROR<br/>TCGB\_ERROR\_PUSH\_ALREADY\_IN\_PROGRESS\_ERROR | 5102       | 이전 PUSH API 호출이 완료되지 않았습니다.              |
|                 | AOS, UNITY<br/>IOS | PUSH\_UNKNOWN\_ERROR<br/>TCGB\_ERROR\_PUSH\_UNKNOWN\_ERROR | 5999       | 알수 없는 푸시 에러입니다. (정의되지 않은 푸시 에러입니다.)      |
| UI              | AOS, UNITY<br/>IOS | UI\_UNKNOWN\_ERROR<br/>TCGB\_ERROR\_UI\_UNKNOWN\_ERROR | 6999       | 알수 없는 에러입니다. (정의되지 않은 에러입니다.)            |
| Server          | AOS, UNITY<br/>IOS | SERVER\_INTERNAL\_ERROR<br/>TCGB\_ERROR\_SERVER\_INTERNAL\_ERROR | 8001       | 서버 내부 에러                                 |
|                 | AOS, UNITY<br/>IOS | SERVER\_REMOTE\_SYSTEM\_ERROR<br/>TCGB\_ERROR\_SERVER\_REMOTE\_SYSTEM\_ERROR | 8002       | 서버에서 외부 연동중 에러 발생                        |
|                 | AOS, UNITY<br/>IOS | SERVER\_UNKNOWN\_ERROR<br/>TCGB\_ERROR\_SERVER\_UNKNOWN\_ERROR | 8999       | 서버에서 알 수 없는 에러                           |

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
|         | -4040201              | 호출한 API에 대한 TOAST Cloud 상품이 활성화되어 있지 않을 때 <br/>예) Leaderboard 상품을 사용하지 않는 상태에서 Gamebase 를 통해 Leaderboard API를 호출할 때 <br/>혹은 Gamebase 자체가 활성화되어 있지 않을 때 |
|         | -4040202              | 정의되어 있지 않은 API를 호출한 경우                   |
|         | -5000201 ~ 7          | Gateway 내부 시스템 오류                        |
| Member  | -4000402              | 사용자 ID를 잘못 입력했을 때                        |
|         | -4000403              | 잘못된 회원에 대한 요청일 때                         |
|         | -4000404              | 잘못된 인증(auth) 대한 요청일 때 |
|         | -4040401              | 존재하지 않거나 탈퇴한 회원에 대한 요청일 때                |
|         | -4100401              | 이미 탈퇴한 회원에 대한 요청일 때                      |
|         | -4220401              | 사용자 인증(auth) 데이터가 정상적이지 않을 때                 |

