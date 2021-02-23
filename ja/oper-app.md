## Game > Gamebase > コンソール使用ガイド > アプリ

NHN Cloud Consoleで **Game > Gamebase > App**をクリックしてアプリの基本情報を設定することができます。

* **アプリ**：アプリ情報管理
* **クライアント**：クライアントバージョンとステータス情報管理
* **インストール URL**：アプリのストアごとのインストールURL管理


## アプリ

Gamebaesサービスを有効にすると自動でアプリが作成され、該当するメニューでは登録された情報の修正のみ可能です。
一つのNHN Cloud プロジェクトにつき一つのGamebaseアプリを管理することができるため、アプリを追加で登録したり削除することはできません。Gamebaseサービスを無効にするとアプリに登録された情報が削除されます。
各項目ごとの詳細説明は、下の**Properties**項目を参考にします。

### Properties

![gamebase_app_01_202004_ja](http://static.toastoven.net/prod_gamebase/gamebase_app_01_202004_ja.png)

#### (1) インストールURL
アプリインストールとPRに利用できる短縮URLの情報です。
アプリがリリースされたストアが複数ある場合にも、一つの短縮URLで管理することができます。
詳細な動作及び管理方法は、次のリンクを参考にします。[インストールURL管理](./oper-app/#url)

> [参考] 
> Gamebaseを有効にすると自動で作成されるため、変更はできません。

####(2) サーバーアドレス
ゲームからゲームサーバーアドレス(IP、URLなど)をリアルタイムで受け取らなければならないときに使用します。
サーバーアドレスを設定するとクライアント初期化後に「Launching情報」から入力された情報を確認することができます。
クライアントの状態に応じて伝達するサーバーアドレスを設定できます。例えば、クライアントの状態がテスト中または審査中の場合は、各項目に設定されたサーバーアドレス値が最初のローンチ情報に伝達されます。
ゲームで必要な場合にのみ入力し、そうでない場合は、空白のままにしておいてください。

####(3) サポート情報
カスタマーサポートのページの他にEmail、電話番号などの情報を入力します。
カスタマーサポートのページがある場合は、下の**アプリ内URL**の**サポート**に情報を入力します。
> [参考] <br/>
> 入力された情報は、Gamebaseが提供するメンテナンス詳細ページに表示されます。

####(4) 認証情報
アプリからログインする際に使用するIdPの認証情報を登録、修正、削除することができます。

外部認証のクライアントID、シークレットキー(secret key)だけでなく、コールバックURLと追加情報を設定することができます。
認証情報の横の**+**ボタンをクリックすると情報を追加することができ、**-**ボタンをクリックすると情報を削除することができます。
Idpごとの詳細な設定方法は、[Authentication Information](#authentication-information)をご参考ください。
> [参考] 
> **トークン再検証**とは？
> クライアントからLatest Login APIを呼び出す際に外部IdPのトークンを再検証するかどうかを設定します。
> **検証しない**を選択すると外部IdPのトークンを再検証せずに内部トークンだけを検証します。
> **常に検証する**を選択するとGamebaseから発行された内部トークンだけでなく、外部IdPトークンに対する有効性検証を常に行います。


####(5) アプリ内URL
クライアントをもう一度リリースすることなく、アプリ内で頻繁に使用するURLをConsoleを通してリアルタイムで修正することができます。

- 利用規約 
- 個人情報同意
- 利用停止規定 
- カスタマーセンターのURL 

ゲームで必要な場合にのみ入力し、そうでない場合は、空白のままにしておいてください。
設定した情報は、クライアント初期化後に「Launching情報」から入力された情報を確認することができます。

### テスト端末
![gamebase_app_02_202004_ja](https://static.toastoven.net/prod_gamebase/gamebase_app_02_202004_ja.png)

テスト端末に登録されるとGamebaseを使用するアプリがメンテナンス中でも正常にゲームにアクセスすることができます。
テスト端末を登録するためには、**Device Key**を入力する必要があります。直接入力したり**ゲームユーザID**を照会して登録することができます。
これ以上使用しないテスト端末を削除することもできます。

> [参考] 
>テスト端末は、最大100本まで登録することができます。

#### (1) 照会

アプリに登録された全てのテスト端末を確認することができます。検索ワードを**Search**に入力して検索条件に合ったテスト端末を検索することができます。

#### (2) 登録

照会画面から**登録**ボタンをクリックすると、テスト端末を登録することができる画面が表示されます。**Device Key**を直接入力したり**ゲームユーザID**を検索してテスト端末を登録することができます。

![gamebase_app_03_202004_reg_ja](https://static.toastoven.net/prod_gamebase/gamebase_app_03_202004_reg_ja.png)

**(A) ゲームユーザIDを通じた登録**

タイプにユーザIDを選択し、ゲームユーザIDを入力して**検索**ボタンをクリックすると画面下にユーザーのログインログ内訳が照会されます。照会された内訳からテスト端末に登録するDevice Keyを選択して**追加情報**を入力し、**登録**ボタンをクリックすると、該当するDevice keyがテスト端末情報に登録されます。

> [参考]
> 追加情報には、ユーザーが確認しやすいように別称を入力してください。例) iPhone6テスト、NHN CloudのiPad

**(B)Device Keyを通した登録**

登録するDevice keyを知っている場合、タイプに**Device Key**を選択して直接テスト端末を登録することができます。
Device Keyと登録する端末の**追加情報**を入力した後、登録ボタンを押すとテスト端末に登録されます。

> [参考]
> 追加情報にはユーザーがわかりやすいエイリアスを入力してください。例) iPhone 6テスト、NHN CloudのiPad

#### (3) 削除

![gamebase_app_03_202004_del_ja](http://static.toastoven.net/prod_gamebase/gamebase_app_03_202004_del_ja.png)

テスト端末照会画面から削除するテスト端末をチェックした後、左上の削除ボタンをクリックするとテスト端末情報が削除されます。削除された情報は復旧できないため、削除する前にもう一度確認してから削除してください。

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

1. Google認証を行うには、Google Cloud Consoleで**Web Application Client ID**を発行し、Gamebase Consoleに入力する必要があります。
2. 承認されたリダイレクトURI欄に次の値を入力します。
	* https://alpha-id-gamebase.toast.com/oauth/callback
	* https://beta-id-gamebase.toast.com/oauth/callback
	* https://id-gamebase.toast.com/oauth/callback

##### iOS

> <font color="red">[注意]</font><br/>
>
> Gamebase iOS SDK 1.12.2バージョンで、URLスキームの設定方法が変更されました。使用SDKバージョンに合ったガイドを確認して設定してください。
>

* 1.12.1以下
	* AdditionalInfoを設定する必要があります。
		* **NHN Cloud Console > Gamebase > App > 認証情報 > 追加情報& Callback URL**の**追加情報**項目に、JSON string形式の情報を設定する必要があります。
		* Googleの場合、iOSアプリで必要な情報**url_scheme_ios_only**を設定する必要があります。
		* **url_scheme_ios_only**の値は、XcodeのURLスキームに登録された値のいずれか1つと一致する必要があります。

	* URLスキームsを設定する必要があります。
		* **XCode > Target > Info > URL Types**

* 1.12.2以上
	* URLスキームを設定する必要があります。
		* **XCode > Target > Info > URL Types**に`tcgb.{Bundle ID}.google`を追加する必要があります。

* Google追加認証情報の入力例

```json
{ "url_scheme_ios_only": "Your URL Schemes" }
```

![gamebase_app_07_201812.png](https://static.toastoven.net/prod_gamebase/gamebase_app_07_201812.png)

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

- ClientID：{Payco client_id}
- Secret Key：{Payco client_secret}
- 追加情報：Payco Service & Service Name (JSON format)

##### Android & Unity
- AdditionalInfoを設定する必要があります。
    * **NHN Cloud Console > Gamebase > App > 認証情報 > 追加情報**項目に、JSON string形式の情報を設定する必要があります。
    * PAYCOの場合、PaycoSDKで要求する**service_code**と**service_name**を設定する必要があります。

* PAYCO追加認証情報の入力例

```json
{ "service_code": "HANGAME", "service_name": "Your Service Name" }
```

##### iOS

> <font color="red">[注意]</font><br/>
>
> Gamebase iOS SDK 1.12.2バージョンでURLスキームの設定方法が変更されました。使用SDKバージョンに合ったガイドを参照してください。
>

* 1.12.1以下
	* AdditionalInfoを設定する必要があります。
		* **NHN Cloud Console > Gamebase > App > 認証情報 > 追加情報& Callback URL**の**追加情報**項目に、JSON string形式の情報を設定する必要があります。
		* PAYCOの場合、PaycoSDKで要求する**service_code**と**service_name**を設定する必要があります。

* 1.12.2以上
	* AdditionalInfoを設定する必要があります。
		* **NHN Cloud Console > Gamebase > App > 認証情報 > 追加情報& Callback URL**の**追加情報**項目に、JSON string形式の情報を設定する必要があります。
		* PAYCOの場合、PaycoSDKで要求する**service_code**と**service_name**を設定する必要があります。
	* URLスキームを設定する必要があります。
		* **XCode > Target > Info > URL Types**に`tcgb.{Bundle ID}.payco`を追加する必要があります。

* PAYCO追加認証情報の入力例

```json
{ "service_code": "HANGAME", "service_name": "Your Service Name" }
```

![gamebase_app_07_201812.png](https://static.toastoven.net/prod_gamebase/gamebase_app_07_201812.png)

#### 5.NAVER
NAVER Developersサイトで申請して発行された{client_id}及び{client_secret}をGamebase Consoleに入力します。
この時、ログイン同意ウィンドウで表示するアプリケーション名である**service_name**を設定する必要があり、iOSの場合はさらに**url_scheme_ios_only**値をJSON String形式で追加情報欄に入力する必要があります。

**入力フィールド**<br />

- Client ID：{NAVER client_id}
- Secret Key：{NAVER client_secret}
- 追加情報：NAVER Application Name & iOS URLスキーム(json format)

**Reference URL**<br />
- [NAVER Developers - アプリケーション登録](https://developers.naver.com/apps/#/register)
- [NAVER Developers - クライアントIDとクライアントシークレットの確認](https://developers.naver.com/docs/common/openapiguide/#/appregister.md)

##### Android & Unity
* **NHN Cloud Console > Gamebase > App > 認証情報 > 追加情報& Callback URL**の**追加情報**項目に、JSON String形式の情報を設定する必要があります。
	* NAVERの場合、ログイン同意ウィンドウに表示するアプリ名である**service_name**を設定する必要があります。

```json
{"service_name": "Your Service Name" }
```

##### iOS

> <font color="red">[注意]</font><br/>
>
> Gamebase iOS SDK 1.12.2バージョンでURLスキームの設定方法が変更されました。使用SDKバージョンに合ったガイドを参照してください。
>

* 1.12.1以下
	* **NHN Cloud Console > Gamebase > App > 認証情報 > 追加情報& Callback URL**の**追加情報**項目に、JSON String形式の情報を設定する必要があります。
		* NAVERの場合、ログイン同意ウィンドウに表示するアプリ名である**service_name**を設定する必要があります。
		* iOSアプリで必要な情報の**url_scheme_ios_only**を追加で設定する必要があります。

	* URLスキームsを設定する必要があります。
		* **XCode > Target > Info > URL Types**

* 1.12.2以上
	* **NHN Cloud Console > Gamebase > App > 認証情報 > 追加情報& Callback URL**の**追加情報**項目に、JSON String形式の情報を設定する必要があります。
		* NAVERの場合、ログイン同意ウィンドウに表示するアプリ名である**service_name**を設定する必要があります。

	* URLスキームを設定する必要があります。
		* **XCode > Target > Info > URL Types**に`tcgb.{Bundle ID}.naver`を追加する必要があります。

* NAVER追加認証情報の入力例

```json
{ "url_scheme_ios_only": "Your URLスキームs", "service_name": "Your Service Name" }
```

![gamebase_app_07_201812.png](https://static.toastoven.net/prod_gamebase/gamebase_app_07_201812.png)

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

 > <font color="red">[注意]</font><br/>
 >
 > Gamebase iOS SDK 1.14.0バージョンでURLスキームの設定方法が変更されました。使用SDKバージョンに合ったガイドを参照してください。
 >

* 1.13.0以下
	* 別途URLスキームを設定する必要はありません。

* 1.14.0以上
	* URLスキームを設定する必要があります。
		* **XCode > Target > Info > URL Types**に**tcgb.{Bundle ID}.twitter**を追加する必要があります。
* Twitter追加認証情報の入力例

![Twitter URL Types](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-auth-001_1.7.0.png)

#### 7. LINE

**入力フィールド**
- Client ID：{LINE Channel ID}
- Secret Key：{LINE Channel Secret}

**Reference URL**<br />

- [LINE Developer Console](https://developers.line.me/console/)

##### iOS
LINE Login機能を使用するには、Xcodeに追加設定を行う必要があります。
- URLスキームsを設定する必要があります。

  * **XCode > Target > Info > URL Types**に**line3rdp.{App Bundle ID}**を追加する必要があります。

- Info.plistファイルを設定する必要があります。
	* LINEで発行したChannelIDを設定します。
	```
	<key>LineSDKConfig</key>
	<dict>
    	<key>ChannelID</key>
    	<string>{Issued LINE ChannleID}</string>
	</dict>
	```
	* ATS設定のためにschemeを登録します。
	```
	<key>LSApplicationQueriesSchemes</key>
	<array>
    	<string>lineauth</string>
    	<string>line3rdp.{App Bundle ID}</string>
	</array>
	```
- LINE Loginを使用するためのプロジェクト設定は、次のリンクを参照してください。(認証必要)
* [LINK \[LINE Developer Guide\]](https://developers.line.biz/en/docs/ios-sdk/objective-c/overview/)


#### 8. Sign In with Apple
Sign In with Apple機能を使用するには、AppStore Connect、Gamebase Console、そしてXcodeの設定が必要です。

##### AppStore Connect Settings
* [Certificates, Identifiers & Profiles \> Keysへ](https://developer.apple.com/account/resources/authkeys/list)

###### Certificates, Identifiers & Profiles > Keys > 追加(+)
1. `Sign In with Apple`チェックボックスを選択して設定を行います。
![Check SignInWithApple](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_appleid0_1.0.png)
2. `Sign in with Apple`を使用するBundle IDを選択します。
![ChooseAPrimaryAppID](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_appleid1_1.0.png)
3. <span style="color:#e11d21">Privatekey</span>をダウンロードして、作成された<span style="color:#e11d21">Key IDを </span>確認します。
![DownloadPrivateKey](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_appleid2_1.0.png)
4. Certificates, Identifiers & Profiles > Identifiers > 対象アプリを選択 > `Sign In with Apple`を有効化します。
    * `Enable as a primary App ID`に設定します。
![DownloadPrivateKey](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_appleid3_1.0.png)

##### Gamebase Console > App Settings
[NHN Cloud Consoleへ](https://console.toast.com/)

* Gamebase
![SecretKey設定](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_appleid5_1.0.png)


###### Client ID Settings
> アプリのBundle IDを設定します。

###### Secret Key Settings
> Apple Developer Account設定で取得した値(**TeamID**、**KeyID**、**PrivateKey**)にJSON文字列を作成して設定します。

* `teamId`：開発者アカウントの右上の値を設定します。
* `keyId`：Certificates, Identifiers & Profiles > Keys > Sign In with Appleをチェックし、作成された値を設定します。
![SecretKey設定](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_appleid7_1.0.png)
* `privateKey`：上のKeysでキーを作成した時に作成されたPrivateKeyファイルの内容を設定します。 (ダウンロードしたファイルを開き、下記のスクリーンショットのように赤い四角形部分の値を使用します)
![SecretKey設定](./image/Operators_Guide/Console_App_Auth_appleid7_1.0.png)

上の値を下記の例のようにJSONで作って設定します。


```json
{
    "teamId":"2UH5Cxxxx",
    "keyId":"3C3FXYxxxx",
    "privateKey":"MIGTAgEAMBMGByqGSM49AgEGCCqGSM49AwEHBHkwdwIBA.. 中略"
}
```

###### Additional Info Settings
[Sign In with AppleのAuthorizationScopeの詳細](https://developer.apple.com/documentation/authenticationservices/asauthorizationscope?language=occ)

Gamebase Console > AppでAppleを追加すると、基本値に下記のJSON値が設定されます。
現在(2019.11)はScopeの種類が`full_name`、`email`のみ存在し、Gamebaseではこの2つ値をデフォルト値に設定します。

```json
{ "authorization_scope":["full_name", "email"] }
```

##### Xcode Project Settings
> <font color="red">[注意]</font><br/>
> Xcode 11以上でのみ`Sign In with Apple`機能を使用するプロジェクトをビルドできます。


1. Target選択 > Signing & Capabilities > Sign In with Apple項目を追加します。
![Capability_SignInWithApple](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_appleid8_1.0.png)
2. Target選択 > Build Phases > Link Binary With Libraries > Authentication.frameworkを**Optional**で追加します。
![AuthenticationServices.framework](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_appleid9_1.0.png)
    - ```注意```: OptionalではなくRequiredに設定されている場合、iOS 11以下の端末ではアプリ実行時にruntime crashが発生します。

## Client

クライアント情報をOS(iOS、Android、Unity WebGL、Unity Standalone)、バージョンごとに管理することができます。

### クライアントリスト
![gamebase_app_12_202004_ja](http://static.toastoven.net/prod_gamebase/gamebase_app_12_202004_ja.png)
現在登録されているクライアントのリストを確認することができます。
OSごとに分かれて表示され、アイコンにある数字は、クライアントを登録する際に入力したバージョンを意味します。
アイコンリストは、サービス状態が<font color="white" style="background-color:#F8BB28">テスト</font>、<font color="white" style="background-color:#FB8F37">審査中</font>、<font color="white" style="background-color:#88C637">サービス</font>、<font color="white" style="background-color:#2AB1A6">アップデートお勧め(サービス中)</font>のリストのみ表示されます。各OS右下の矢印をクリックすると<font color="white" style="background-color:#A1A1A1">アップデート必須</font>、<font color="white" style="background-color:#CCCCCC">終了</font>状態のクライアントリストを確認することができます。
アイコンカラーがサービス状態ごとに区分されていて、一目でサービスの状態を把握することができます。

### Properties

Gamebase Consoleから管理するクライアント登録情報について説明します。
**クライアント**のタブから**AOS登録**、**iOS登録**ボタンなどをクリックすると、クライアント登録画面が表示されます。登録されたクライアントの入力値を修正したり削除したい場合、アイコンリストからアイコンをクリックしたり、クライアント全体のリストから修正したり削除したいクライアントを選択してください。
![gamebase_app_13_202004_ja](https://static.toastoven.net/prod_gamebase/gamebase_app_13_202004_ja.png)
#### (1) ストア
(<font color="red">必須</font>)クライアントをリリースするストアを選択します。
OSごとに選択可能なストアが異なります。
#### (2) クライアントバージョン
(<font color="red">必須</font>) クライアントバージョンを入力します。
ゲームで決めたルールに沿って文字列で入力してください。
#### (3) サービス状態
(<font color="red">必須</font>) クライアントのサービス状態を選択します。
状態は、<font color="white" style="background-color:#F8BB28">テスト</font>、<font color="white" style="background-color:#FB8F37">審査中</font>、<font color="white" style="background-color:#88C637">サービス</font>、<font color="white" style="background-color:#2AB1A6">アップデートお勧め(サービス中)</font>、<font color="white" style="background-color:#A1A1A1">アップデート必須</font>、<font color="white" style="background-color:#CCCCCC">終了</font>の6つです。

- <font color="white" style="background-color:#F8BB28">テスト</font>：内部テスト
- <font color="white" style="background-color:#FB8F37">審査中</font>：ストア審査中。安定化指標を追加で設定することができます。
![gamebase_app_15_201812.png](https://static.toastoven.net/prod_gamebase/gamebase_app_15_201812.png)

> [参考]
> 「安定化指標」とは？<br/>
> Gamebase内部でGamebase APIを呼び出す際に渡す指標ログです。
> サービス状態が「審査中」のときに安定化指標を有効にすると、審査中にGamebase内部で問題が発生した場合に比べ、より簡単に問題を確認することができます。
> Gamebase Consoleからリアルタイムで安定化指標を使用するかどうかとログレベルを設定することができます。
![gamebase_app_15_201812.png](https://static.toastoven.net/prod_gamebase/gamebase_app_15_201812.png)

- <font color="white" style="background-color:#88C637">サービス中</font>：サービスが正常に動作しています。
- <font color="white" style="background-color:#2AB1A6">アップデートお勧め(サービス中)</font>：サービスが正常に動作しています。                               <br/>より安定的なバージョンを使用するように誘導するためにポップアップを表示します。新しいバージョンをダウンロードして利用するように誘導しますが、ユーザーが希望する場合、現在のバージョンでも引き続きサービスを利用することができます。<br />下は、「アップデートを推奨します。(サービス中)」の状態のときにGamebase SDKで基本的に提供するポップアップです。

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client_updateRecommended_1.0.png)

- <font color="white" style="background-color:#A1A1A1">アップデート必須</font>：サービスできません。<br/>現在ゲームでサービスに対応していないバージョンで、最新バージョンのインストールを案内するポップアップを表示します。<br />下は、「アップデートが必ず必要です。」の状態のときにGamebase SDKで基本的に提供するポップアップです。
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client_updateRequired_1.1.png)

>  <font color="red">[注意] </font> 
>  **アップデートが必ず必要な場合とメンテナンスが同時に設定**されている場合、サービス状態は、「アップデートが必ず必要です。」になります。
>  メンテナンス中にユーザーに対し「アップデートが必ず必要です。」のポップアップを表示したくない場合、メンテナンス完了後にサービス状態を「アップデートが必ず必要です。」に変更する必要があります。
>  <font color="orange">[参考] </font>
> アップデートボタンを押すと、インストールURLメニューで設定した各々のストアアドレスに接続されます。
> 例えば、クライアントがApp storeに設定されていて、インストールURLメニューでApp store関連設定が存在する場合は設定したアドレスに移動し、インストールURLメニューに設定されていない場合は共通(Common)URLに接続されます。
>  
- <font color="white" style="background-color:#CCCCCC">終了</font>：サービスできません。<br/> サービスが終了されたバージョンの場合に選択します。<br />下は、「終了」の状態のときにGamebase SDKで基本的に提供するポップアップです。
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client_ended_1.0.png)

> [参考] 
> サービス状態ごとに表示するメッセージ設定
> **アップデートを推奨します。(サービス中)**、**アップデートが必ず必要です。**、**終了**状態の場合にユーザーに表示する案内メッセージを多国語で設定することができます。
> サービス状態を選択すると、各状態に合った基本メッセージが5カ国語(韓国語、英語、日本語、中国語簡体字、中国語繁体字)で提供され、必要な場合、言語を追加したり基本メッセージの文章を変更することができます。
> ![gamebase_app_18_202004_ja](https://static.toastoven.net/prod_gamebase/gamebase_app_18_202004_ja.png)

#### (4) サーバーアドレス
クライアントで利用するサーバーアドレス(IP、URL)を入力します。
**アプリ**タブでサーバーアドレスを入力するとすべてのクライアントに適用されるため、クライアントごとに他のサーバーアドレスを使用したい場合にのみサーバーアドレスを入力します。

#### (5) デバッグログ
Gamebae SDKのDebug Logを出力するかどうかを、コンソールでリアルタイムに変更できます。
設定されていない場合は、基本的にGamebase SDK内部に設定された値で動作し、GamebaseコンソールでDebug Logを出力するかどうかを設定できます。
Gamebase SDKのDebug Logが'OFF'状態でも、コンソールで'ON'に設定すれば端末にGamebase Debug Logが出力されます

## インストールURL

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

#### (1) 共通
ストア情報がなかったり、ストアへの移動に失敗したときに接続されるアドレスを設定します。

#### (2) Android
AndroidユーザーがインストールURLを実行した際に接続されるアドレスを設定します。

#### (3) iOS
iOSユーザーがインストールURLを実行した際に接続されるアドレスを設定します。

## 端末移行(Transfer Account)
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
![gamebase_app_23_202004_ja](https://static.toastoven.net/prod_gamebase/gamebase_app_23_202004_ja.png)
初期設定が完了すると、ゲームユーザーは端末移行機能の無効化のみ可能です。設定の変更が必要な場合はサポートにお問い合わせください。
**使用しない**ボタンをクリックして機能を無効化できます。既に発行された端末移行キーはすべて削除されるため、有効化した後は無効化するかどうかを慎重に選択する必要があります。
