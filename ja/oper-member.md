## Game > Gamebase > Console ご利用ガイド > 会員

ゲームにログインした会員情報を照会します。


### Search Member

User IDを入力すると、会員情報を検索することができます。
ユーザーIDは、初めてログインする際にGamebaseが自動で発行するユーザー識別子です。送信時に混乱を防ぐため、同じ発音の文字を取り除き、「ABCDFGHJKLMNPQRSTWXYZ1346789」だけを使用しています。

検索されたユーザーの詳細情報を上の方に表示し、ログイン、マッピング、決済、利用停止、プレイ時間などの履歴は下の方にタブ形式で表示されます。




### Detail Information
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Member_Member1_1.4.png)

**User ** 

- **ユーザID**：GamebaseのユーザーID
- **国家コード(USIM)**：ユーザー端末のUSIMの国家コードで、取得に失敗した場合は'ZZ'で表記されます。端末に設定されている国家コードを確認したい場合、下の**ログイン履歴**から確認してください。
- **最終ログイン時間**：ユーザーが最後にログインした時間
- **登録日**：ユーザーが始めてログインした時間
- **アカウント状態**
  - **正常**：正常なユーザー。**利用停止**ボタンをクリックして手動で利用停止状態に変更することができます。
  - **利用停止**：アビュージングなどにより利用停止(ban)状態となったユーザー。**利用停止解除**ボタンをクリックすると、手動で利用停止を解除することができます。
  - **退会**：退会したユーザー

**Identity Provider ** 

Gamebaseでは、複数の外部IdPを連動することができます。つまり、ユーザーが一つのユーザーIDにFacebook、Googleの二つのIdPを登録してログインすることができます。SDKから**Login using a specific IdP**や**Add Mapping**APIを呼び出す際にIdPが登録されます。

- **IdP**：外部IdP(ゲスト、Facebook、PAYCO、Googleなど)
- **Idp ID**：外部IdPが提供するID(Facebook no、PAYCO IDなど)
- **登録日**：ユーザーが初めて該当するIdPを登録した時間

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
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Member_MappingHistory1_1.2.png)

照会したユーザーのマッピング、マッピング解除履歴を照会します。直近3ヶ月(90日)間の履歴がすべて表示されます。

- **ユーザIDベース**：照会されたユーザーIDを基に照会します。
- **IdP IDベース**：照会されたユーザーIDに現在マッピングされているIdP IDを基に照会します。
  照会されたユーザーIDにFacebook、Gooogle IdPがマッピングされている場合、二つのIdP IDがリストに表示されます。

- **IdP ID**：IdPログイン時に使用されるID情報
- **IdP**：マッピングされたIdPの情報
- **日付**：IdP IDとGamebase IDのマッピング関連の作業が行われた時刻
- **Type**：マッピング作業内容の詳細内訳
  - AAM：マッピング追加
  - ARM：マッピング解除
  - AFR：マッピング強制解除
  - GMG：ゲストアカウント作成
  - OMG：IdPアカウント作成

### Purchase History
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Member_PurchaseHistory1_1.0.png)
照会したユーザーの商品購入内訳を照会します。
照会したい日付を入力して照会することができ、最大1ヶ月(30日)まで照会可能です。

- **決済番号**：Gamebase内で決済内容を区別して見られる固有番号
- **ストア**：決済したストアの情報
- **アイテム名**：ユーザーがアプリで購入した実際のアイテム名
- **価格**：ユーザーが購入したアイテムの価格
- **通貨**：ユーザーが購入時に使用した通貨の種類
- **Consume**：決済したアイテムが配布されたかどうかの内容
- **決済状態**：現在の決済進行状態
- **Store Reference Key**：ストアが発行する決済固有番号
- **登録日付**：ユーザーが購入を試みたり完了した時間
- **払戻日付**：ユーザーがアイテムを払い戻した時間

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
