## Game > Gamebase > Console ご利用ガイド > アプリ

NHN Cloud Consoleで **Game > Gamebase > App**をクリックしてアプリの基本情報を設定することができます。

* **アプリ**：アプリ情報管理
* **クライアント**：クライアントバージョンとステータス情報管理
* **インストール URL**：アプリのストアごとのインストールURL管理

## App

Gamebaseサービスを有効化すると、自動的にアプリが作成され、該当メニューでは登録された情報の修正のみ可能です。
NHN Cloudプロジェクト1つにつき1つのGamebaseアプリを管理することができるため、アプリを追加で登録したり削除することはできません。Gamebaseサービスを無効化すると、アプリに登録された情報が削除されます。
各項目の詳細説明は、以下の詳細項目を参照してください。

### 基本情報
![gamebase_app_01_202009](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_app_01_202009.png)

#### (1)インストールURL
アプリのインストールと広報に利用できる短縮URL情報です。
アプリが配布されたストアが複数の場合でも、1つの短縮URLで管理できます。
詳細な動作および管理方法は、次のリンクを参照してください。[インストールURL管理](./oper-app/#installed-url)

> [参考]
> Gamebaseを有効にすると自動的に作成されるため、変更はできません。

#### (2)テスト決済を含めるかどうか
アプリ指標にテスト決済も含めて表示するかどうかを選択します。
デフォルト設定は「テスト決済を含める」に設定されており、「テスト決済を除外」に設定すると、Analytics売上指標からテスト決済は全て除外して表示されます。
> [参考1]
> 指標表示設定に関係なく、データはテスト決済と実際の決済を常に記録しているため、テスト決済の表示はいつ変更しても実際のデータ収集には影響がありません。

> [参考2]
> テスト決済除外設定以降から発生する決済については、Analytics売上指標からテスト決済が除外されます。

> [参考3]
> テストデータはGoogleおよびAppStoreのみサポートしており、他のストアはサポートしません。
> 各ストアのテスト指標基準は次の通りです。
> * Google：Googleコンソールに登録したテストアカウントを利用して決済を行った履歴
> * AppStore：Sandbox環境でテスト決済を行った履歴

#### (3)退会猶予期間
アプリの退会猶予機能を使用したい場合、退会を猶予する期間を設定します。
基本設定は「7日」に設定されていて、1日～30日まで設定できます。
> [参考]
> 退会猶予期間中は正常にサービスが利用できます。

### サーバーアドレス
![gamebase_app_01_202004](https://static.toastoven.net/prod_gamebase/gamebase_app_02_202012.png)

- ゲームでゲームサーバーアドレス(IP、URLなど)をリアルタイムで受け取る必要がある時に使用します。
- サーバーアドレスを設定すると、クライアントの初期化後に「ローンチ情報」で入力された情報を確認できます。
- クライアントの状態ごとにサーバーアドレスを設定することができ、ローンチ情報でサーバーアドレスを確認できます。
- ゲームで必要な場合にのみ入力し、そうでない場合には空白にしておきます。

### 言語設定
![gamebase_app_01_202004](https://static.toastoven.net/prod_gamebase/gamebase_app_03_202004.png)
- 各メニューの多言語設定でデフォルトで表示する言語をあらかじめ指定できます。
- 多言語項目を表示する時、選択した言語が表示され、基本言語も設定した項目が選択されています。
- 使用したくない場合は該当欄を空白にしてください。

### 認証情報
![gamebase_app_01_202004](https://static.toastoven.net/prod_gamebase/gamebase_app_04_202004.png)

アプリでログインする時に使用するIdPの認証情報を登録、修正、削除できます。

外部認証のクライアントID、シークレットキー(secret key)だけでなく、コールバックURLと追加情報を設定できます。
認証情報の横にある**+**ボタンを押すと、情報を追加できます。**-**ボタンを押すと情報を削除できます。
各Idpの詳細な設定方法は[Authentication Information](#authentication-information)を参照してください。
> [参考]
> **トークン再検証**とは？
> クライアントでLatest Login APIを呼び出す時に外部IdPのトークンを再検証するかどうかを設定します。
> **検証しない**を選択すると、外部IdPのトークンを再検証せず、内部トークンの検証だけを行います。
> **常に検証**を選択すると、Gamebaseで発行した内部トークンだけでなく、外部IdPトークンも常に有効性を検証します。

### アプリ内URL
![gamebase_app_01_202004](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_app_05_202009.png)
クライアントの再配布を行わずに、アプリ内でよく使用するURLをConsoleを利用してリアルタイムで修正できます。

- 利用約款
- 個人情報同意
- 利用停止規定

ゲームで必要な場合にのみ入力し、そうでない場合には空白にしてください。
設定した情報は、クライアント初期化後に「ローンチ情報」で確認できます。

### サポート
サポート関連の設定を進行できます。
現在Gamebaseでは3つのサポート形式を提供しており、選択したサポートタイプごとに設定できる項目が異なります。
サポートタイプ別の設定は以下の通りです。

#### 1. 開発会社独自のサポート
![gamebase_app_19_202009.png](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_app_19_202009.png)
開発会社で独自にサポートを使用している場合に設定します。
設定項目は以下の通りです。
* **サポートURL** ：現在提供または使用している開発会社独自のサポートアドレスを入力します。
* **連絡先**：サポートの連絡先を入力します。この情報はGamebase SDKを介して取得できます。

#### 2. Gamebase提供のサポート
![gamebase_app_19_202009.png](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_app_20_202009.png)
Gamebaseで提供するサポート機能を使用したい時に設定します。
設定項目は以下の通りです。
* **サポートURL** ：顧客からのお問い合わせを受けるページ情報を提供します。このURLはGamebase提供サポートを選択する場合は自動的に作成され、このURLから顧客のお問い合わせを別のWebページを介して受信できます。
* **連絡先**：サポートの連絡先を入力します。この情報はGamebase SDKを介して取得できます。

* **サポート言語**：サポートセンターでサポートする言語を選択します。プロジェクト自体の言語設定とは別に設定され、 韓国語、英語、日本語、中国語(簡体字/繁体字)、ロシア語をサポートします。ご希望の言語がない場合はサポートセンターまでお問い合わせください。

#### 3. NHN Cloud組織商品(Online Contact)
![gamebase_app_19_202009.png](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_app_21_202102.png)
NHN Cloudで組織ごとに提供されるOnline contact商品を使用する場合に設定します。
設定項目は以下の通りです。
* **サポートURL**：NHN Cloud Online Contactで提供されるアドレスを入力します。この情報はNHN Cloud Online Contactに接続して確認できます。
* **連絡先**：サポートの連絡先を入力します。この情報はGamebase SDKを介して追加で情報を受け取ることができます。
* **OC組織Key** : NHN Cloud Online Contactサポートのお問い合わせを確認するためのKeyを入力します。この情報を入力しない場合、サポートページ内で入信したお問い合わせを確認することができないため、確認後に入力する必要があります。詳細な連携方法は、以下の内容を参照してください。
> [参考] NHN Cloud Online ContactとGamebase間の連携
> Gamebase内でNHN Cloud Online Contactと連携する場合、次のプロセスに従ってSSOログインAPI Keyを発行してGamebase内に設定すると、サポートサービスを正常に利用できます。
> サポートの安定的なサービスを提供するために、以下の順序通りに進行してください。
>
> 1) NHN Cloud Online Contactに会員連携方式設定
> サービス管理 -> ヘルプセンター -> 会員連携
> ![gamebase_app_22_202102.png](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_app_22_202102.png)
> 会員連携有効化：有効化
> ログインタイプ：GET方式
> Token検証URL: https://gamebase-web.cloud.toast.com/tcgb-web/v1.0/apps/{appId}/online-contact/login-status
> **{appId}**部分は、設定したいGamebaseのプロジェクトIDを確認した後、入力してください。
> 
> 2) OC組織Keyを取得してOC組織Key項目に入力
> 全体管理 -> 契約サービス状況 -> 組織情報に移動した後、OC組織情報のOC組織KeyをコピーしてGamebase OC組織Key項目に入力
> ![gamebase_app_25_202102.png](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_app_25_202102.png)
>
> 3) NHN Cloud Online contactサポートページアドレスを取得してサポートURLに入力
> ヘルプセンター -> 下位メニュー選択 -> 右上の「ヘルプセンター」をクリック
> ![gamebase_app_02_201812.png](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_app_26_202009.png)
> ブラウザ上部に表示されたアドレスをGamebaseサポートURL項目に入力
> ![gamebase_app_27_202102.png](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_app_27_202102.png)
>

### Test Device

![gamebase_app_02_202004_ja](https://static.toastoven.net/prod_gamebase/gamebase_app_02_202004_ja.png)
テスト端末に登録されると、Gamebaseを使用するアプリがメンテナンス中でも正常にゲームにアクセスできます。
テスト端末を登録するには**Device Key**または**IP**情報を登録する必要があります。直接入力するか、**ゲームユーザーID**を照会して登録できます。
メンテナンス時にゲームがプレイできるようにしたり、端末ごとにDebug Logを出力するかどうかを設定してテスト端末を管理できます。
今後使用しないテスト端末を削除することもできます。
接続履歴確認ボタンを押すと、該当端末を利用して**メンテナンスが進行される間の接続時間と詳細接続ログ**を確認できます。
![gamebase_app_02_201812.png](http://static.toastoven.net/prod_gamebase/gamebase_app_09_201912.png)

> [参考]
> テスト端末は最大100個まで登録できます。

#### (1)照会

アプリに登録された全てのテスト端末を確認できます。検索ワードを**Search**に入力して検索条件に合ったテスト端末を検索できます。

#### (2)登録

照会画面で**登録**ボタンを押すと、テスト端末を登録できる画面が表示されます。**Device Key**を直接入力するか、**ゲームユーザーID**を検索してテスト端末を登録できます。

![gamebase_app_03_202004_reg_ja](https://static.toastoven.net/prod_gamebase/gamebase_app_03_202004_reg_ja.png)

**(1)ゲームユーザーIDを利用して登録**

タイプはユーザーIDを選択し、ゲームユーザーIDを入力して**検索**ボタンを押すと、画面下にユーザーのログインログ履歴を照会します。照会された履歴からテスト端末に登録したいDevice Keyを選択し、**追加情報**を入力して**登録**ボタンを押すと、該当Device keyがテスト端末情報に登録されます。



**(2)Device KeyまたはIPを利用して登録**

登録したいDevice keyまたはIP情報を知っている場合、任意のタイプを選択して直接テスト端末を登録できます。
登録したい端末の**端末名**およびデバッグログ、メンテナンスを無視するかどうかを入力した後、登録ボタンを押すとテスト端末に登録されます。

> [参考]
> 端末名にはユーザーが認識しやすいエイリアスを入力してください。例) iPhone 6テスト、TOASTのiPad

#### (3)削除

![gamebase_app_03_202004_del_ja](http://static.toastoven.net/prod_gamebase/gamebase_app_03_202004_del_ja.png)

テスト端末照会画面で削除したいテスト端末をチェックした後、左上の削除ボタンを押すと、テスト端末情報が削除されます。削除された情報は復旧できないため、削除する前にもう一度確認してから削除してください。

### Authentication Information

#### 1. Facebook
Facebookの開発サイトに登録したアプリの{アプリID}と{アプリシークレットコード}をGamebase Consoleに入力します。
この時、ログインする際に必要な{Facebook Permission}もJSON Stringタイプで追加情報欄に入力する必要があります。

**入力フィールド**

- ClientID：{AppID}
- Secret Key：{App Secret Code}
- 追加情報：Facebook Permission (json format)

![gamebase_app_04_202004_ja](https://static.toastoven.net/prod_gamebase/gamebase_app_04_202004_ja.png)

**[例] facebook_permission format **
* Facebookの場合、OAuth認証試行時、Facebookにリクエストする情報の種類を設定する必要があります。

```json
{ "facebook_permission": [ "public_profile", "email"] }
```

![gamebase_app_05_202004_ja](https://static.toastoven.net/prod_gamebase/gamebase_app_05_202004_ja.png)

**Reference URL**<br />

- [Facebookの開発者サイト](https://developers.facebook.com/)
- [Facebookの権限](https://developers.facebook.com/docs/facebook-login/permissions/)

##### Android & iOS & Unity
NHN Cloud Consoleでの設定の他に追加設定はありません。



#### 2. Google

##### Google Cloud Console

![gamebase_app_06_202004_ja](https://static.toastoven.net/prod_gamebase/gamebase_app_06_202004_ja.png)

Google認証を行うには、Google Cloud Consoleで**Web Application Client ID**を発行し、Gamebase Consoleに入力する必要があります。
承認されたリダイレクトURI欄に次の値を入力します。
	* https://alpha-id-gamebase.toast.com/oauth/callback
	* https://beta-id-gamebase.toast.com/oauth/callback
	* https://id-gamebase.toast.com/oauth/callback

<br/>

Google iOS認証を行うには、Google Cloud Consoleで**iOS Client ID**を発行し、Gamebase Consoleに入力する必要があります。

**APIs & Services > CREATE CREDENTIALS > OAuth client ID**を選択した後、

![gamebase_app_google_ios_1.png](https://static.toastoven.net/prod_gamebase/gamebase_app_google_ios_1.png)

**Application type**は**iOS**を選択し、Bundle IDを入力します。

![gamebase_app_google_ios_2.png](https://static.toastoven.net/prod_gamebase/gamebase_app_google_ios_2.png)

#### Gamebase Console

![gamebase_app_google_ios_4.png](https://static.toastoven.net/prod_gamebase/gamebase_app_google_ios_4.png)

**入力フィールド**<br />

- Web Application ID : {Google Web Application Client ID}
- iOS Client ID : {Google iOS Client ID}
- Secret Key : {Google Web Application Client secret}

##### iOS
* [Gamebase > iOS SDK使用ガイド > 始める > IdP Settings > Google](./ios-started/#google)


#### 3. Apple Game Center
Appleの開発者サイトに登録されたBundleIDをGamebase Consoleに入力します。

**入力フィールド**<br />

- ClientID：{Bundle ID}<br />

![gamebase_app_08_201812.png](https://static.toastoven.net/prod_gamebase/gamebase_app_08_201812.png)

**Reference URL**<br />

- [Apple Developerサイト](https://developer.apple.com/)
- [Apple iTunes Connect](https://itunesconnect.apple.com/)

#### 4. PAYCO
PAYCO Client IDを申請して発行された{client_id}及び{client_secret}をGamebase Consoleに入力します。

**入力フィールド**<br />

- ClientID：{PAYCO client_id}
- Secret Key：{PAYCO client_secret}
- 追加情報：PAYCO Service & Service Name (JSON format)

##### Additional Info Settings

* **NHN Cloud Console > Gamebase > App > 認証情報 > 追加情報**項目にJSON string形式の情報を設定する必要があります。
* PAYCOの場合、PAYCO SDKで要求する**service_code**と**service_name**を設定する必要があります。

* PAYCO追加認証情報の入力例

```json
{ "service_code": "Your Service Code", "service_name": "Your Service Name" }
```

##### iOS
* [Gamebase > iOS SDK使用ガイド > 始める > IdP settings > PAYCO](./ios-started/#payco)

#### 5.NAVER
NAVER Developersサイトで申請して発行された{client_id}および{client_secret}をGamebase Consoleに入力します。
この時、ログイン同意ウィンドウで表示するアプリケーション名である**service_name**を設定する必要があります。

**入力フィールド**<br />

- Client ID: {NAVER client_id}
- Secret Key: {NAVER client_secret}
- 追加情報：NAVER Application Name (json format)

**Reference URL**
- [NAVER Developers - アプリケーション登録](https://developers.naver.com/apps/#/register)
- [NAVER Developers - クライアントIDとクライアントシークレットの確認](https://developers.naver.com/docs/common/openapiguide/#/appregister.md)

##### Additional Info Settings

* **NHN Cloud Console > Gamebase > App > 認証情報 > 追加情報& Callback URL**の**追加情報**項目にJSON String形式の情報を設定する必要があります。
* NAVERの場合、ログイン同意ウィンドウに表示するアプリ名である**service_name**を設定する必要があります。

```json
{"service_name": "Your Service Name" }
```

##### iOS
* [Gamebase > iOS SDK使用ガイド > 始める > IdP settings > NAVER](./ios-started/#NAVER)

#### 6. Twitter
Twitter Application Managementサイトでアプリを登録して発行した{Consumer Key}および{Consumer Secret}をGamebase Consoleに入力します。

**入力フィールド**

- Client ID：{Twitter Consumer Key}
- Secret Key：{Twitter Consumer Secret}

**Reference URL**<br />
- [Twitter Application Management](https://apps.twitter.com/)

##### Android
 > <font color="red">[注意]</font><br/>
 >
 > 2019年7月25日から、TwitterではTLS 1.0、TLS 1.1のサポートを中断し、TLS1.2のみサポートしています。
 > それにより、Android 4.3 (Jellybean、API Level 18)以下の端末ではAndroid WebViewによるTwitterログインができません。
 >
 > すなわち、Android 4.4以上(KitKat、API Level 19)の端末でのみTwitterログインを使用できます。

##### iOS
* [Gamebase > iOS SDK使用ガイド > 始める > IdP settings > Twitter](./ios-started/#twitter)

#### 7. LINE

**入力フィールド**

- Region : {LINE Channel Region}
- Client ID：{LINE Channel ID}
- Secret Key：{LINE Channel Secret}

**Reference URL**<br />

- [LINE Developer Console](https://developers.line.me/console/)

##### iOS
* [Gamebase > iOS SDK使用ガイド > 始める > IdP settings > LINE](./ios-started/#line)

#### 8. Sign In with Apple
Sign In with Apple機能を使用するには、AppStore Connect、Gamebase Console、そしてXcodeの設定が必要です。

##### AppStore Connect Settings
* [Certificates, Identifiers & Profiles \> Keysへ](https://developer.apple.com/account/resources/authkeys/list)

###### Certificates, Identifiers & Profiles > Keys > 追加(+)
1. `Sign In with Apple`チェックボックスを選択して設定を行います。
![Check SignInWithApple](./image/Operators_Guide/Console_App_Auth_appleid0_1.0.png)
2. `Sign in with Apple`を使用するBundle IDを選択します。
![ChooseAPrimaryAppID](./image/Operators_Guide/Console_App_Auth_appleid1_1.0.png)
3. <span style="color:#e11d21">Privatekey</span>をダウンロードして、作成された<span style="color:#e11d21">Key IDを </span>確認します。
![DownloadPrivateKey](./image/Operators_Guide/Console_App_Auth_appleid2_1.0.png)
4. Certificates, Identifiers & Profiles > Identifiers > 対象アプリを選択 > `Sign In with Apple`を有効化します。
    * `Enable as a primary App ID`に設定します。
![DownloadPrivateKey](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_appleid3_1.0.png)

##### Gamebase Console > App Settings
[NHN Cloud Consoleへ](https://console.toast.com/)

* Gamebase
![SecretKey設定](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_appleid4_1.0.png)

###### Client ID Settings
> アプリのBundle IDを設定します。

###### Secret Key Settings
> Apple Developer Account設定で取得した値(**TeamID**、**KeyID**、**PrivateKey**)にJSON文字列を作成して設定します。

* **teamId**：開発者アカウントの右上の値を設定します。
* **keyId**：Certificates, Identifiers & Profiles > Keys > Sign In with Appleをチェックし、作成された値を設定します。
![SecretKey設定](./image/Operators_Guide/Console_App_Auth_appleid5_1.0.png)
* **privateKey**：上のKeysでキーを作成した時に作成されたPrivateKeyファイルの内容を設定します。 (ダウンロードしたファイルを開き、下記のスクリーンショットのように赤い四角形部分の値を使用します)
![SecretKey設定](./image/Operators_Guide/Console_App_Auth_appleid7_1.0.png)

上の値を下記の例のようにJSONで作って設定します。

```json
{
    "teamId":"2UH5Cxxxx",
    "keyId":"3C3FXYxxxx",
    "privateKey":"MIGTAgEAMBMGByqGSM49AgEGCCqGSM49AwEHBHkwdwIBA.. 中略"
}
```

> <font color="red">[注意]</font><br/>
>
> privateKeyに改行が入らないように注意してください。

###### Additional Info Settings
[Sign In with AppleのAuthorizationScopeの詳細](https://developer.apple.com/documentation/authenticationservices/asauthorizationscope?language=occ)

Gamebase Console > AppでAppleを追加すると、基本値に下記のJSON値が設定されます。
現在(2019.11)はScopeの種類が`full_name`、`email`のみ存在し、Gamebaseではこの2つ値をデフォルト値に設定します。
```json
{ "authorization_scope":["full_name", "email"] }
```

##### Xcode Project Settings
> <font color="red">[注意]</font><br/>
>
> Xcode 11以上でのみ**Sign In with Apple**機能を使用するプロジェクトをビルドできます。

1. Target選択 > Signing & Capabilities > Sign In with Apple項目を追加します。
![Capability_SignInWithApple](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_appleid8_1.0.png)
2. Target選択 > Build Phases > Link Binary With Libraries > Authentication.frameworkを**Optional**で追加します。
![AuthenticationServices.framework](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_appleid9_1.0.png)

> <font color="red">[注意]</font><br/>
> OptionalではなくRequiredに設定されている場合、iOS 12以下の端末ではアプリ実行時にランタイムクラッシュが発生します。


##### iOS 12バージョン以下をサポートするための設定(Sign In with Apple JS)

> <font color="red">[注意]</font><br/>
>
> Gamebase SDK iOS 2.13.0以上のバージョンでは、iOS 12以下のバージョンでのWebViewを利用したSign In with Apple機能を使用できます。
>
> 2.13.0以前バージョンを使用していたゲームの場合でも、下の**iOS 12バージョン以下をサポートするための設定**を参考にして既存プロジェクトを設定し、
>
> Gambase SDK iOS 2.13.0以上を適用すると、iOS 12以下のバージョンでSign In with Apple機能を使用できます。


* iOS 12以下のバージョンでSign In with Appleを使用するには、Sign In with Apple JSを使用して、Webページからログインする必要があります。
* Apple IDログインWebページでは、Appleアカウントとパスワードを入力してログインできます。

**以下の手順に従って、Apple開発者サイトから新しいService IDを登録する必要があります。**

1. Service IDを追加<br/>
![Create new Service ID](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_SignInWithAppleJS_AppStore_01.png)
2. Service IDとして使用する識別子を設定(一般的にはbundle ID + **.区分する文字列**)<br/>
![Create new Service ID](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_SignInWithAppleJS_AppStore_02.png)
3. 登録されたService IDを確認後、修正<br/>
![Create new Service ID](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_SignInWithAppleJS_AppStore_03.png)
4. 下部のSign In with Apple項目のConfigureを押す<br/>
![Create new Service ID](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_SignInWithAppleJS_AppStore_04.png)
5. Primary App IDを設定(既にSign In with Appleを使用していた場合は、該当アプリのBundle IDを設定)<br/>
![Create new Service ID](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_SignInWithAppleJS_AppStore_05.png)
6. Apple IDで認証した後、認証情報を受けとるCallback URLを設定<br/>
![Create new Service ID](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_SignInWithAppleJS_AppStore_06.png)
7. 設定して保存<br/>
![Create new Service ID](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_SignInWithAppleJS_AppStore_07.png)


**上で設定したService IDをNHN Cloud Gamebase Console > Gamebase > アプリ > 認証情報 > Apple > Service IDに入力します。***

> <font color="red">[注意]</font><br/>
>
> Sign In with Appleが設定されていない場合は、残りの項目も設定が必要です。

1. Apple開発者サイトで、設定したService IDを以下のようにService ID項目に追加します。(既にSign In with Apple設定値がある場合は、他の値は変更する必要がありません。)
![Set Service ID for Sign In with Apple JS](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_SignInWithAppleJS_TOAST_01.png)


#### 9. WEIBO

##### Weibo Console

1. Weibo Developersサイトで申請して発行された{client_id}および{client_secret}をGamebase Consoleに入力します。
この時、ログイン時に必要な{scope}またはJSON String形式で追加情報欄に入力する必要があります。


![gamebase_app_29_202012.png](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_app_29_202012.png)

2. callback URL欄に次の値を入力します。
	* Authorization callback page : https://api.weibo.com/oauth2/default.html
	* Cancel authorization callback page : https://api.weibo.com/oauth2/default.html


**入力フィールド**

 -  ClientID：{App Key}
 -  Secret Key：{App Secret}
 - 追加情報：scope（json format）


**Additional Info Settings**

* Scope

Applicationで必要とする権限を表します。
Weiboガイド文書に従ってデフォルトですべての権限が宣言されています。
必要に応じて追加/削除/変更できます。

* oauthApiUrl

内部的にWeibo Open APIを呼び出すためのドメインです。
変更してはいけません。


![gamebase_app_28_202012.png](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_app_28_202012.png)


**Reference URL**
- [Weibo Developer](https://open.weibo.com/)


## Client

クライアント情報をOS(iOS、Android、Unity WebGL、Unity Standalone)、バージョンごとに管理することができます。

### Client List

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client1_1.2.png)
現在登録されたクライアントリストを確認できます。
OSごとに区分されて表示され、アイコン内の数字はクライアント登録時に入力したバージョンを意味します。
アイコンリストはサービス状態が <font color="white" style="background-color:#eed14c">テスト</font>、<font color="white" style="background-color:#eba34b">ベータサービス</font>、<font color="white" style="background-color:#eb7e4b">審査中</font>、<font color="white" style="background-color:#88C637">サービス</font>、<font color="white" style="background-color:#2AB1A6">アップデート推奨(サービス中)</font>のリストのみ表示されます。OS別の右下にある矢印をクリックすると<font color="white" style="background-color:#A1A1A1">アップデート必須</font>、<font color="white" style="background-color:#CCCCCC">終了</font> 状態のクライアントリストを確認できます。
アイコンの色をサービス状態で区分して、ひと目でサービスの状態を把握できます。

### Properties

Gamebase Consoleで管理するクライアント登録情報を説明します。
**クライアント**タブで**AOS登録**、**iOS登録**ボタンなどを押すと、クライアント登録画面が表示されます。登録されたクライアントの入力値を修正または削除したい場合は、アイコンリストからアイコンを押すか、クライアント全体リストからクライアントを選択してください。
![gamebase_app_13_201812.png](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_app_13_202012.png)
#### (1)ストア
(<font color="red">必須</font>)クライアントを配布するストアを選択します。
OSごとに選択できるストアが異なります。
#### (2)ゲームバージョン
(<font color="red">必須</font>)クライアントバージョンを入力します。
ゲームで定めたルールに従って文字列で入力してください。
#### (3)サービス状態
(<font color="red">必須</font>)クライアントのサービス状態を選択します。
状態は <font color="white" style="background-color:#eed14c">テスト</font>, <font color="white" style="background-color:#eba34b">ベータサービス</font>, <font color="white" style="background-color:#eb7e4b">審査中</font>, <font color="white" style="background-color:#88C637">サービス</font>, <font color="white" style="background-color:#2AB1A6">アップデート推奨(サービス中)</font>, <font color="white" style="background-color:#A1A1A1">アップデート必須</font>, <font color="white" style="background-color:#CCCCCC">終了</font>の6つです。

- <font color="white" style="background-color:#F8BB28">テスト</font>：内部テスト
- <font color="white" style="background-color:#eba34b">ベータサービス</font>：サービスサーバーではない別のベータサーバーに接続が必要な場合に選択します。
- <font color="white" style="background-color:#FB8F37">審査中</font>：ストア審査中

- <font color="white" style="background-color:#88C637">サービス中</font>：正常サービス
- <font color="white" style="background-color:#2AB1A6">アップデート推奨(サービス中)</font>：正常サービス。 <br/>より安定的なバージョンを使用するように誘導するためにポップアップを表示します。<br/>新しいバージョンをダウンロードして利用するように誘導しますが、ユーザーが望む場合は、現在のバージョンで継続してサービスを利用できます。<br />以下は「アップデート推奨(サービス中)」状態の時にGamebase SDKがデフォルトで提供するポップアップです。

- <font color="white" style="background-color:#A1A1A1">アップデート必須</font>：サービス不可。<br/>現在ゲームでサービスをサポートしないバージョンのため、最新バージョンインストール案内ポップアップを表示します。<br />以下は「アップデート必須」状態の時にGamebase SDKがデフォルトで提供するポップアップです。 <br/> 「アップデート必須」状態のときにポップアップボタンを追加できます。 **詳細表示ボタン追加**から**ボタン追加**を選択する場合、接続するURLを設定できます。

![gamebase_app_37_202205.png](https://static.toastoven.net/prod_gamebase/gamebase_app_37_202205.png)

>  <font color="red">[注意] </font>
>  **アップデート必須とメンテナンスが同時に設定**されている場合、サービス状態は「アップデート必須」になります。
> メンテナンス進行中に、ユーザーにアップデート必須ポップアップを表示したくない場合は、メンテナンス完了後にサービスの状態を「アップデート必須」に変更する必要があります。
>  <font color="orange">[参考] </font>
> アップデートボタンを押すと、インストールURLメニューで設定したそれぞれのストアアドレスに接続されます。
> 例えばクライアントがApp storeに設定されていて、インストールURLメニューでApp store関連設定が存在する場合、設定したアドレスに移動し、インストールURLメニューに設定されていない場合は共通(Common) URLに接続されます。

- <font color="white" style="background-color:#CCCCCC">終了</font>：サービス不可。 <br/> サービスが終了したバージョンの場合に選択します。<br />以下は、「終了」状態の時にGamebase SDKがデフォルトで提供するポップアップです。

![gamebase_app_15_201812.png](https://static.toastoven.net/prod_gamebase/gamebase_app_15_201812.png)

> [参考]
> サービスの状態に基づいて表示するメッセージ設定
> **アップデート推奨(サービス中)**、**アップデート必須**、**終了**状態の場合、ユーザーに表示する案内メッセージを多言語で設定できます。
> サービス状態を選択すると、アプリに設定されている言語設定情報に基づいて各状態に合った基本メッセージが提供されます。言語を追加したり基本メッセージの文言を変更することもできます。
> 以前に各状態で設定した各言語の設定がある場合は、アプリの言語設定情報に関係なく以前に登録した内容を呼び出して表示されます。
> アプリの言語設定に設定された情報がない場合、5個(韓国語、英語、日本語、簡体字、繁体字)の言語で基本メッセージが提供されます。言語を追加したり基本メッセージの文言を変更することもできます。
> ![gamebase_app_18_202004](https://static.toastoven.net/prod_gamebase/gamebase_app_18_202004.png)

#### (4)サーバーアドレス
クライアントで利用するサーバーアドレス(IP、URL)を入力します。
**アプリ**タブでサーバーアドレスを入力すると、すべてのクライアントに適用されるため、クライアントごとに別にサーバーアドレスを使用したい時のみサーバーアドレスを入力します。

#### (5) Debug log
Gamebae SDKのDebug Logを出力するかどうかを、コンソールでリアルタイムに変更できます。
設定されていない場合は、基本的にGamebase SDK内部に設定された値で動作し、GamebaseコンソールでDebug Logを出力するかどうかを設定できます。
Gamebase SDKのDebug Logが'OFF'状態でも、コンソールで'ON'に設定すれば端末にGamebase Debug Logが出力されます

#### (6)メモ
該当クライアントの簡単なメモを30文字以内で入力できます。

## Terms Of Service
ゲームに表示する約款を作成し、構成を設定します。
![gamebase_app_30_202102](https://static.toastoven.net/prod_gamebase/gamebase_app_30_202102.png)
### (1)作成された約款リスト
- **+**ボタンを押して約款を追加で作成できます。
![gamebase_app_31_202102](https://static.toastoven.net/prod_gamebase/gamebase_app_31_202102.png)

### (2)約款の国タイプ

### (3)約款の対象国
- 国タイプがその他の国の場合、対象国を追加で選択できます。

### (4)約款の構成
- ドラッグアンドドロップ方式で、約款項目の順序を指定できます。
- 約款項目は上位約款5個、下位約款5個、合計25個を作成できます。

### (5)約款項目作成
- 約款項目リストから約款構成を選択した後、**追加**ボタンを押すと、その約款の下位約款が作成されます。
- 下位約款を選択した場合、約款を作成できません。

### (6)選択した約款の詳細情報
- 約款名
	- 約款を管理するための約款名です。
- 約款同意
	- 約款の同意が必須かどうかです。
- 詳細ページ
	- なし：詳細ページが存在しない場合です。
	- URL入力：詳細ページのURLを設定できます。
	- 直接入力：詳細ページを作成できます。
![gamebase_app_32_202102](https://static.toastoven.net/prod_gamebase/gamebase_app_32_202102.png)
- 表示するテキスト
	- ゲームに表示するテキストです。
	- **+**ボタンを押して言語を追加できます。
	- サポートしない言語をリクエストする場合、チェックされた言語をデフォルトで表示します。
- プッシュ同意
	- なし：プッシュ関連の同意ではない場合です。
	- 広告性受信同意：広告性プッシュの受信同意が必要な約款です。
	- 広告性夜間受信同意：広告性夜間プッシュ受信の同意が必要な約款です。

> <font color="red">[注意]</font><br/>
>
> 保存ボタンを押さなければ作成した約款の内容が適用されません。
> 保存時、現在選択されている約款詳細情報のみ保存されます。
>


## Terms Of Service Deploy

ゲームに表示する約款配布および配布履歴です。
![gamebase_app_33_202102](https://static.toastoven.net/prod_gamebase/gamebase_app_33_202102.png)

### (1)基本約款設定
![gamebase_app_35_202102](https://static.toastoven.net/prod_gamebase/gamebase_app_35_202102.png)

- 作成した約款のうち、設定された配布国以外の国から接続する場合、デフォルトで表示される約款を選択します。

> <font color="red">[注意]</font><br/>
>
> 基本約款は設定しない場合もあります。基本約款が設定されていない場合、配布された国以外の国からの接続時、約款が表示されません。
>

### (2)約款リスト

- 現在作成されている約款リストです。

### (3)プレビュー
![gamebase_app_36_202102](https://static.toastoven.net/prod_gamebase//gamebase_app_36_202102.png)

- 約款リストから選択した約款をプレビューできます。

### (4)約款配布および配布履歴
#### 配布
- 約款リストから選択した約款を配布できます。
- 「約款の再同意」にチェックした後に配布を行うと、既に約款に同意したユーザーにも新たに約款ウィンドウが表示されます。文言などの単純な修正の際は「約款の再同意」をチェックする必要がありません。

#### 配布履歴
![gamebase_app_34_202102](https://static.toastoven.net/prod_gamebase/gamebase_app_34_202102.png)
- 約款リストから選択した約款の配布履歴です。

## Installed URL

ゲームをインストールするためのストアURL情報を管理します。

![gamebase_app_19_202004_ja](https://static.toastoven.net/prod_gamebase/gamebase_app_19_202004_ja.png)

* クライアント状態が<font color="white" style="background-color:#2AB1A6">アップデート推奨(サービス中)</font>または<font color="white" style="background-color:#A1A1A1">アップデート必須</font>の時、ストアごとに提供するアドレスの値を設定します。
* ユーザーがPCやモバイルで短縮URLをクリックすると、ユーザー端末情報(デバイス、オペレーションシステム、ストアなど)を利用して入力されたサイトにリダイレクトします。
* ストア情報がない、もしくはストア移動に失敗した場合は、'COMMON'に設定されたURLに移動します。

_[例1] Android端末からSMSで受信したインストールURLをクリックする場合
**(Device:mobile,OS:Android,Store:なし)**Androidのうち、代表ストアに指定されたモバイルURLへ移動。代表ストアが「Google Play」の場合、「Google Play」モバイルに設定されているURLへ移動。
_[例2] 「One Store」からダウンロードしたアプリでゲームしていたユーザーが「アップデートが必ず必要です。」のポップアップウィンドウから「今すぐアップデート」のボタンをクリックした場合_
**(Device:mobile,OS:Android,Store:One Store)**「One Store」モバイルに設定されているURLへ移動(One Storeモバイルインストールページ)<br/>
_[例3] PCからインストールURLを入力した場合_
**(Device:PC、OS:Windows、Store:なし)** COMMON PCに設定されているURLへ移動

### Properties

入力されたインストールURL情報を変更したい場合、**修正**ボタンをクリックします。

![gamebase_app_20_202004_ja](https://static.toastoven.net/prod_gamebase/gamebase_app_20_202004_ja.png)

- 各項目はPC、モバイルごとにそれぞれ設定することができます。PCとモバイルを分ける必要がない場合、同じ値をそれぞれ入力してください。
- 探しているストアがリストに表示されない場合、[カスタマーセンター](https://toast.com/support/inquiry)までご連絡ください。該当するストアを追加いたします。

#### (1) Common
ストア情報がなかったり、ストアへの移動に失敗したときに接続されるアドレスを設定します。

#### (2) Android
AndroidユーザーがインストールURLを実行した際に接続されるアドレスを設定します。

#### (3) iOS
iOSユーザーがインストールURLを実行した際に接続されるアドレスを設定します。

#### (4) Standalone
Standaloneでサービスされているアプリから接続されるアドレスを設定します。Standaloneは、PCでのみ動作しますので、PC設定のみ行ってください。

## Transfer account
ゲストでログインしたゲームユーザーが、他のID提供者と連携を行わずに、他の端末で続けてゲームをプレイできる機能を提供します。
ユーザーは、現在ゲーム中の端末で移行のためのキーを発行し、移行する端末にキーを入力することで、簡単にゲーム端末を変更できます。
**端末移行**機能は、デフォルトで無効になっています。使用するには**端末移行**で**使用する**をクリックします。

![gamebase_app_21_202004_ja](https://static.toastoven.net/prod_gamebase/gamebase_app_21_202004_ja.png)
**使用する**ボタンをクリックした後、端末移行に必要な情報を入力します。

![gamebase_app_22_202004_ja](https://static.toastoven.net/prod_gamebase/gamebase_app_22_202004_ja.png)
端末移行機能に必要な値を設定できる画面です。
各項目の説明は下記の通りです。

#### 発行
端末移行発行キーの形式を設定します。
端末移行キーはIDのみまたは、IDとパスワードのキーを利用できます。ID、パスワードの形式はゲームで任意の小文字、大文字、数字の組み合わせで構成できます。

1. **ID自動発行形式**：端末移行ID発行形式を設定します。設定項目は下記の通りです。

- **数字(最小：12)**：数字のみで構成されたIDを発行します。発行されるIDの長さは12文字が最小です。
- **数字+小文字(最小：10)**：数字と小文字の組み合わせで構成されたIDを発行します。発行されるIDの長さは10文字が最小です。
- **数字+大文字(最小：10)**：数字と大文字の組み合わせで構成されたIDを発行します。発行されるIDの長さは10文字が最小です。
- **数字+小文字+大文字(最小：9)**：数字、小文字、大文字の組み合わせで構成されたIDを発行します。発行されるIDの長さは9文字が最小です。
- **小文字+大文字(最小：9)**：小文字と大文字の組み合わせで構成されたIDを発行します。発行されるIDの長さは9文字が最小です。
2. **パスワード自動発行形式**：端末移行IDを利用してログインする時に使用するパスワード発行形式を設定します。設定項目は下記の通りです。
- **パスワード使用しない**：パスワードを使用しない時に選択します。この項目を選択する場合は、下記の検証項目でIDの有効時間のみ設定できます。
- **数字(最小：12)**：数字のみで構成されたパスワードを発行します。発行されるパスワードの長さは12文字が最小です。
- **数字+小文字(最小：10)**：数字と小文字の組み合わせで構成されたパスワードを発行します。発行されるパスワードの長さは10文字が最小です。
- **数字+大文字(最小：10)**：数字と大文字の組み合わせで構成されたIDを発行します。発行されるパスワードの長さは10文字が最小です。
- **数字+小文字+大文字(最小：9)**：数字、小文字、大文字の組み合わせで構成されたIDを発行します。発行されるパスワードの長さは9文字が最小です。
- **小文字+大文字(最小：9)**：小文字と大文字の組み合わせで構成されたIDを発行します。発行されるパスワードの長さは9文字が最小です。

#### 検証
発行された端末移行キーの検証条件を設定します。
端末移行キーを検証する時、移行回数や有効期間、失敗時遮断などを設定できます。
3. **端末移行回数**：発行されたIDの端末移行可能回数を設定します。無制限、1回のどちらかを選択する必要があります。
4. **有効期間**：発行されたアカウントの有効時間を設定します。発行された端末移行IDは、この設定値の影響を受けます。無制限、期間設定のどちらかを選択する必要があります。
5. **失敗時、再検証を遮断するかどうか**：ログインに失敗した場合、特定時間アカウントを遮断します。選択すると追加設定項目が表示されます。
6. **遮断基準回数**：**失敗時、再検証を遮断するかどうか**を選択すると表示されます。入力した回数、検証に失敗した場合、アカウントが遮断されます。 1回以上に設定する必要があります。
7. **遮断期間**：アカウント遮断時、何分後に検証を試行できるかを設定します。**永久遮断**、**期間指定**のどちらかを選択します。**期間指定**を選択すると、自由に遮断時間/分を指定できます。

#### 初期設定完了後
![gamebase_app_20_201812.png](https://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_TransferAccount1_3.0.png)
初期設定が完了すると、ゲームユーザーは端末移行機能の無効化のみ可能です。設定の変更が必要な場合はサポートにお問い合わせください。
**使用しない**ボタンをクリックして機能を無効化できます。既に発行された端末移行キーはすべて削除されるため、有効化した後は無効化するかどうかを慎重に選択する必要があります。

## Analytics indicator
Analyticsに指標を記録するための転送指標を確認および設定できます。
ユーザーレベル(INT)ごと、ワールド/サーバー/チャンネルごと、クラス/職業ごとに項目が分かれていて、ユーザーレベルの場合は実際にAnalyticsに転送されたレベル項目のみ表示され、ワールド/サーバー/チャンネルごと、クラス/職業ごとの項目では、このメニューで登録された項目のみAnalyticsに指標として記録されます。
### ユーザーレベル(INT)ごと
Analyticsシステムに送信されたレベル指標項目を確認できます。
この項目では別の修正項目がなく、照会のみ行えます。
![gamebase_app_20_201812.png](https://static.toastoven.net/prod_gamebase/gamebase_analytics_indicator_01_202003.png)

### ワールド/サーバー/チャンネル別、クラス/職業別照会
各項目に設定されている転送指標項目を確認できます。
照会画面では、設定された項目の指標を記録したくない場合に削除ボタンを押して登録された項目の削除ができます。
項目を削除した後は、**Analyticsメニューで指標に表示されず、**削除した項目の指標が記録されなくなるため、注意が必要です。
![gamebase_app_20_201812.png](https://static.toastoven.net/prod_gamebase/gamebase_analytics_indicator_02_202003.png)

### ワールド/サーバー/チャンネル別、クラス/職業別登録
Analytics指標に記録したい情報を新たに登録できます。
下の方にある追加ボタンを利用して登録できます。**項目は最大100個**まで新規で登録できます。
登録画面では登録されたデータの**指標画面表示項目の修正だけを提供**し、削除したい場合は再度照会画面に移動して削除を行う必要があります。
![gamebase_app_20_201812.png](https://static.toastoven.net/prod_gamebase/gamebase_analytics_indicator_03_202003.png)

#### (1) ChannelId / ClassId：Analytics内に記録するセパレータ情報を入力します。指標を記録したい時に設定するID情報を入力してください。
#### (2)指標画面表示：1番項目に入力したIDに送信された指標を画面に表示する時に表示したい内容を入力します。この情報は既に登録された指標項目も修正できます。
#### (3)削除：登録画面では新たに追加された項目のみ削除できます。
