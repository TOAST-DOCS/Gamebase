## Game > Gamebase > Console ご利用ガイド > 会員

ゲームにログインした会員情報を照会します。


### Search Member

User IDを入力すると、会員情報を検索することができます。
ユーザーIDは、初めてログインする際にGamebaseが自動で発行するユーザー識別子です。送信時に混乱を防ぐため、同じ発音の文字を取り除き、「ABCDFGHJKLMNPQRSTWXYZ1346789」だけを使用しています。
IdP ID는 Id Provider에서 제공하는 아이디 정보로써 로그인 시 입력하는 정보가 아닌 Id Provider내의 고유 식별자를 의미합니다. 따라서 IdP ID 항목으로 검색하고자 할 경우 검색정보 입력에 주의가 필요합니다.

検索されたユーザーの詳細情報を上の方に表示し、ログイン、マッピング、決済、利用停止、プレイ時間などの履歴は下の方にタブ形式で表示されます。


### Detail Information
![image alt](./image/Operators_Guide/Console_Member_Member1_1.6.png)

**User **

- **ユーザID**：GamebaseのユーザーID
- **国家コード(USIM)**：ユーザー端末のUSIMの国家コードで、取得に失敗した場合は'ZZ'で表記されます。端末に設定されている国家コードを確認したい場合、下の**ログイン履歴**から確認してください。
- **最終ログイン時間**：ユーザーが最後にログインした時間
- **登録日**：ユーザーが始めてログインした時間
- **アカウント状態**
  - **正常**：正常なユーザー。**利用停止**ボタンをクリックして手動で利用停止状態に変更することができます。
  - **利用停止**：アビュージングなどにより利用停止(ban)状態となったユーザー。**利用停止解除**ボタンをクリックすると、手動で利用停止を解除することができます。
  - **退会**：退会したユーザー
- **푸시 토큰**: 유저의 푸시 토큰 정보 조회.

####계정 상태 변경
![image alt](./image/Operators_Guide/Console_Member_Member1_2.2.png)
조회한 유저의 계정 상태를 변경할 수 있는 기능입니다.
상태별 변경할 수 있는 경우는 아래와 같습니다.
- **정상**: 이용정지, 탈퇴상태로 변경이 가능합니다. 탈퇴시에는 해당 계정정보를 되돌릴 수 없으므로 처리 전 확인 및 주의가 필요합니다.
- **이용 정지**: 이용정지 해제를 진행할 수 있습니다.
- **탈퇴**: 해당 버튼이 노출되지 않습니다.

**Identity Provider **

Gamebaseでは、複数の外部IdPを連動することができます。つまり、ユーザーが一つのユーザーIDにFacebook、Googleの二つのIdPを登録してログインすることができます。SDKから**Login using a specific IdP**や**Add Mapping**APIを呼び出す際にIdPが登録されます。

- **IdP**：外部IdP(ゲスト、Facebook、PAYCO、Googleなど)
- **Idp ID**：外部IdPが提供するID(Facebook no、PAYCO IDなど)
- **登録日**：ユーザーが初めて該当するIdPを登録した時間

#### 매핑 추가

조회한 유저의 Id Provider 정보를 추가할 수 있는 기능입니다.
연결하고자하는 IdP를 가지고 있는 유저의 정보가 **정상**일 경우에만 매핑 추가 작업이 가능합니다.
* 제공 유저의 IdP 설정란에서 연결하고자 하는 Id Provider 정보를 1번 버튼을 통해 오른쪽화면에 추가한 후, 아래 매핑 추가 버튼을 통해 매핑 추가 작업을 진행합니다.
* 잘못 추가했다면 매핑 추가 버튼을 누르기 전에는 2번 버튼을 통해 언제든지 다른 Id provider로 교체 가능합니다.
* 제공 받는 유저가 GUEST정보만 가지고 있을 경우에는 새로운 Id Provider정보가 추가되면서 기존의 Guest정보는 유실되므로 작업 진행 시 주의가 필요합니다.
* 제공 유저의 Id Provider 정보가 한개일 경우 해당작업을 진행하면 제공 유저 정보는 **유실**상태로 변경되어 더이상 사용할 수 없으므로 작업 진행 전 확인이 필요합니다.
##### 제공 전 예시
![image alt](./image/Operators_Guide/Console_Member_AddMapping_1.3.png)
##### 제공 후 예시
![image alt](./image/Operators_Guide/Console_Member_AddMapping_2.1.png)

#### 매핑 해제
다중 매핑이 이루어진 계정의 경우 요청에 따라 Id Provider 정보 연동을 해제할 수 있습니다.
각각의 계정은 최소 1개의 연결정보가 존재해야 하므로 2개 이상의 연결정보가 존재할 때만 버튼이 활성화 됩니다.
* 버튼을 누르면 아래와 같이 연결된 Id Provider 정보와 함께 매핑 해제 버튼이 노출됩니다.
![image alt](./image/Operators_Guide/Console_Member_RemoveMapping_1.0.png)
* 해제 버튼을 누를 경우 아래와 같이 최종 확인창과 함께 Id Provider 정보를 확인합니다. 확인버튼을 누르시면 매핑 해제가 진행됩니다.
![image alt](./image/Operators_Guide/Console_Member_RemoveMapping_2.0.png)

### Login History
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Member_LoginHistory1_1.2.png)

照会したユーザーのログイン内訳を照会します。
初めて照会するときは、直近1日分が照会され、照会したい日付をもう一度入力して照会することもできます。ただし、直近3ヶ月間(90日)の履歴のみ提供します。
SDKからログイン関連のAPIを呼び出すとき、履歴が追加されます。

- **Login Date**：ユーザーがアプリにログインした時間
- **Login Type**：ユーザーログイン時に使用した認証タイプ(IdP Login/Guestなど)。括弧の中の情報は、実際に使用されたIdProviderの情報。
- **OS / Ver**：ユーザーログイン時に使用したOS(IOS/Android/WebGLなど)及び該当するOSのバージョン情報
- **Device model**：ユーザーがアプリにログインする際に使用したデバイスのモデル名
- **Device Key**：ユーザーログイン時に使用したデバイスが保有している固有識別値(Android:Android id、iOS:IDFV)
- **Device Country Code**：ユーザーログイン時にデバイスに設定されている国家コード
- **USIM Country Code**：ユーザーログイン時にUSIMカードに設定されている国家コード
- **Telecom**：ユーザーログイン時に利用した通信キャリア情報
- **Network**：ユーザーログイン時に使用したネットワークタイプ(Wi-Fi/3G/LTEなど)
- **Language Code**：ユーザーログイン時に端末に設定されている言語コード情報
- **Store Code**：ユーザーがアプリをダウンロードしたストア情報
- **Client Version**：アプリをダウンロードした当時のクライアントバージョン情報
- **Gamebase SDK Version**：アプリに使用されたGamebase SDKのバージョン情報
- **etc**：その他、ログイン時に使用された上記項目以外の情報

### Mapping History
![image alt](./image/Operators_Guide/Console_Member_MappingHistory1_1.4.png)
照会したユーザーのマッピング、マッピング解除履歴を照会します。直近3ヶ月(90日)間の履歴がすべて表示されます。

- **IdP ID**：IdPログイン時に使用されるID情報
- **IdP**：マッピングされたIdPの情報
- **日付**：IdP IDとGamebase IDのマッピング関連の作業が行われた時刻
- **Type**：マッピング作業内容の詳細内訳
  - AAM：マッピング追加
  - ARM：マッピング解除
  - AFR：マッピング強制解除
  - GMG：ゲストアカウント作成
  - OMG：IdPアカウント作成

매핑된 IDP 이력을 클릭할 경우 해당 IdP를 기준으로 Gamebase ID에 매핑된 이력을 보여줍니다.
![image alt](./image/Operators_Guide/Console_Member_MappingHistory1_2.1.png)

### Purchase History
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Member_PurchaseHistory1_1.0.png)
照会したユーザーの商品購入内訳を照会します。
照会したい日付を入力して照会することができ、最大1ヶ月(30日)まで照会可能です。

- **Transaction ID**：Gamebase内で決済内容を区別して見られる固有番号
- **ストア**：決済したストアの情報
- **アイテム名**：ユーザーがアプリで購入した実際のアイテム名
- **価格**：ユーザーが購入したアイテムの価格
- **通貨**：ユーザーが購入時に使用した通貨の種類
- **소비 상태**：決済したアイテムが配布されたかどうかの内容
- **決済状態**：現在の決済進行状態
- **Store Reference Key**：ストアが発行する決済固有番号
- **결제예약일시**：ユーザーが購入を試みたり完了した時間
- **환불일시**：ユーザーがアイテムを払い戻した時間

### Ban History
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Member_BanHistory1_1.0.png)

照会したユーザーの利用停止内訳を照会することができます。
照会したい日付を入力して照会することができ、最大1ヶ月(30日)まで照会可能です。

- **開始日**：ユーザーの利用停止措置を適用した時間
- **終了日**：ユーザーの利用停止状態が終了した時間
- **テンプレート**：ユーザーに対する利用停止を登録する際に使用したテンプレート名
- **理由**：運営者がユーザーを利用停止状態にした実際の理由に関する情報
- **登録者/登録日**：利用停止を登録した運営者/システム情報及び日時
- **解除理由**：運営者が利用停止の解除を行う際に入力した実際の解除理由
- **解除登録者/解除登録日**：利用停止を解除した運営者/システム情報及び日時

### Playtime
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Member_Playtime1_1.2.png)
照会したユーザーがゲームをプレイした時間を日付ごとに照会します。
照会したい日付を入力して照会することができ、最大1ヶ月(30日)まで照会可能です。

### Withdraw History
![image alt](./image/Operators_Guide/Console_Member_WithdrawHistory1_1.1.png)
조회한 사용자가 탈퇴한 사용자라면 탈퇴 이력을 보여줍니다.
이 메뉴는 탈퇴 유저를 조회할 경우에만 나타나며 유저의 탈퇴경로를 조회할 수 있습니다.
