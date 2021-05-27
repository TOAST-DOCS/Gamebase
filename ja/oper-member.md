## Game > Gamebase > Console ご利用ガイド > 会員

ゲームにログインした会員情報を照会します。

## Search Member

User IDを入力すると、会員情報を検索することができます。
ユーザーIDは、初めてログインする際にGamebaseが自動的に発行するユーザー識別子です。送信時に混乱を防ぐため、同じ発音の文字を取り除き、「ABCDFGHJKLMNPQRSTWXYZ1346789」だけを使用しています。
IdP IDはIdPで提供するID情報で、ログイン時に入力する情報ではなく、IdP内の固有識別子を意味します。したがって、IdP ID項目で検索する場合は検索情報の入力に注意してください。

検索されたユーザーの詳細情報を上の方に表示し、ログイン、マッピング、決済、利用停止、プレイ時間などの履歴は下の方にタブ形式で表示されます。


### Detail Information
![gamebase_member_01_201812](https://static.toastoven.net/prod_gamebase/gamebase_member_01_201812.png)

**User **

- **유저 ID**: Gamebase 사용자 아이디
- **국가코드(USIM)**: 사용자 단말기의 USIM 국가 코드로 수집에 실패하면 'ZZ'로 표기됩니다. 단말기에 설정된 국가 코드를 확인하고 싶다면 하단의 **로그인 이력**에서 확인하세요.
- **마지막 로그인 시간**: 사용자가 가장 마지막에 로그인한 시간
- **가입일**: 사용자가 최초로 로그인한 시간
- **계정 상태**
  - **정상**: 정상 사용자.
  - **이용정지**: 어뷰징 등으로 이용 정지(ban)된 사용자. 우측 상단의 계정 상태 변경 메뉴를 통해 이용정지를 해제할 수 있습니다.
  - **탈퇴**: 탈퇴한 사용자.
- **푸시 부가정보 조회**: 게임 유저의 푸시 토큰 및 태그 정보 조회.
![gamebase_member_12_202104](https://static.toastoven.net/prod_gamebase/gamebase_member_12_202104.png)

#### アカウント状態変更
![gamebase_member_02_201812](https://static.toastoven.net/prod_gamebase/gamebase_member_02_201812.png)

照会したゲームユーザーのアカウント状態を変更できる機能です。
各状態から、以下のように状態を変更できます。
- **正常**：利用停止、退会状態に変更できます。退会時は該当アカウント情報を元に戻せないため、処理前によく確認してください。
- **利用停止**：利用停止解除ができません。
- **退会**：該当ボタンが表示されません。

**Identity Provider **

Gamebaseでは、複数の外部IdPを連動することができます。つまり、ユーザーが一つのユーザーIDにFacebook、Googleの二つのIdPを登録してログインすることができます。SDKから**Login using a specific IdP**や**Add Mapping**APIを呼び出す際にIdPが登録されます。

- **IdP**：外部IdP(ゲスト、Facebook、PAYCO、Googleなど)
- **Idp ID**：外部IdPが提供するID(Facebook no、PAYCO IDなど)
- **登録日**：ユーザーが初めて該当するIdPを登録した時間

#### マッピング追加

照会したゲームユーザーのIdP情報を追加できる機能です。
接続するIdPを持っているゲームユーザーの情報が**正常**の時のみ、マッピング追加作業を行えます。
* 提供ユーザーのIdP設定欄で、接続するIdP情報を1回ボタンをクリックして右側の画面に追加した後、下にある**マッピング追加**ボタンをクリックします。
* 誤って追加した場合、**マッピング追加**ボタンをクリックする前であれば、2回ボタンをクリックしていつでも他のIdPに変更できます。
* ゲームユーザーがゲスト情報のみ持っている時は、新しいIdP情報が追加され、既存のゲスト情報は消滅するため注意してください。
* 提供ユーザーのIdP情報が1つの時は、作業を行うと提供ユーザー情報が**消滅**状態に変更されて二度と使用できないため、よく確認してください。
##### 提供例
![gamebase_member_03_201812](https://static.toastoven.net/prod_gamebase/gamebase_member_03_201812.png)

#### マッピング解除
多重マッピングしたアカウントは、リクエストに応じてIdP情報の連携を解除できます。
各々のアカウントは、少なくとも1個の接続情報が存在する必要があるため、2個以上の接続情報がある時のみボタンが有効になります。
* ボタンをクリックすると、下記のように接続したIdP情報と共に**マッピング解除**ボタンが表示されます。

![gamebase_member_04_201812](https://static.toastoven.net/prod_gamebase/gamebase_member_04_201812.png)

* **解除**ボタンをクリックすると、下記のように最終確認ウィンドウが表示されるので、IdP情報を確認します。**確認**ボタンをクリックすると、マッピングが解除されます。
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Member_RemoveMapping_2.0.png)

### Login History
![gamebase_member_05_201812](https://static.toastoven.net/prod_gamebase/gamebase_member_05_201812.png)

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
![gamebase_member_06_201812](https://static.toastoven.net/prod_gamebase/gamebase_member_06_201812.png)
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

マッピングされたIdP履歴をクリックした場合、該当IdPを基準にGamebase IDにマッピングされた履歴を表示します。
![gamebase_member_07_201812](https://static.toastoven.net/prod_gamebase/gamebase_member_07_201812.png)

### Purchase History
![gamebase_member_08_201812](https://static.toastoven.net/prod_gamebase/gamebase_member_08_201812.png)
照会したユーザーの商品購入内訳を照会します。
照会したい日付を入力して照会することができ、最大1ヶ月(30日)まで照会可能です。

- **Transaction ID**：Gamebase内で決済内容を区別して見られる固有番号
- **ストア**：決済したストアの情報
- **アイテム名**：ユーザーがアプリで購入した実際のアイテム名
- **価格**：ユーザーが購入したアイテムの価格
- **通貨**：ユーザーが購入時に使用した通貨の種類
- **消費状態**：決済したアイテムが配布されたかどうかの内容
- **決済状態**：現在の決済進行状態
- **Store Reference Key**：ストアが発行する決済固有番号
- **決済予約日時**：ユーザーが購入を試行または完了した時間
- **払戻日時**：ユーザーのアイテムが払い戻された時間

### Ban History
![gamebase_member_09_201812](https://static.toastoven.net/prod_gamebase/gamebase_member_09_201812.png)

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
![gamebase_member_10_201812](https://static.toastoven.net/prod_gamebase/gamebase_member_10_201812.png)
照会したユーザーがゲームをプレイした時間を日付ごとに照会します。
照会したい日付を入力して照会することができ、最大1ヶ月(30日)まで照会可能です。

### Withdraw History
![image alt](https://static.toastoven.net/prod_gamebase/gamebase_member_11_202006.png)
照会したユーザーが退会したゲームユーザーの場合、退会履歴を表示します。
このメニューは、退会したゲームユーザーを照会する場合にのみ表示され、ゲームユーザーの退会経路を照会できます。

## Transfer account
**端末移行**機能を使用する場合にのみ使用できます。[端末移行機能の有効化](./oper-app/#transfer-account)
ゲームユーザーの端末移行キーの発行および検証履歴を確認できます。遮断されたキーの遮断解除や、有効期限が切れたキーの再発行ができます。

![image alt](https://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Member_TransferAccount1_1.0.png)
**端末移行発行キー**
- **ID**：ゲームユーザーに発行された端末移行ID
- **発行日時**：端末移行IDが発行された日時
- **満了日時**：発行された端末移行IDの満了日時
- **状態**：発行された端末移行IDの現在の状態
  - <font color="white" style="background-color:#88C637">正常</font>：発行されたキーが正常な状態です。該当キーを利用して端末移行が可能です。
  - <font color="white" style="background-color:#FB8F37">遮断</font>：発行されたキーが遮断されている状態です。発行されたキーを利用して端末移行ができません。
  - <font color="white" style="background-color:#A1A1A1">満了</font>：発行されたキーの使用期限が切れた状態です。発行されたキーを利用して端末移行ができません。

**端末移行履歴**
該当ゲームユーザーに発行されたキーの履歴を照会できます。
デフォルトで最後に発行されたキーが選択されていて、他のキーを選択すると、選択したキーの履歴を照会できます。

### 端末移行キーの再発行
![image alt](https://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Member_TransferAccount_Renewal1_1.0.png)
**再発行**ボタンをクリックすると、新しい端末移行キーを再発行できます。再発行すると、以前に発行されたキーは使用できません。
- **ID、パスワード再発行**：ID、パスワードを再発行します。
- **パスワード再発行**：IDは以前に発行されたIDをそのまま使用し、パスワードのみ再発行します。

#### 再発行時の注意事項
- パスワードは再発行時に一度だけ表示されるため、再発行した後、必ず情報を別途保管してください。
- 保管していなかった場合、パスワードを探す方法がないため、再発行する必要があります。
- 有効期限が切れたキーは、満了日時が更新されますが、アカウントの状態が正常/遮断の場合は満了日時が更新されません。
