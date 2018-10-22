## Game > Gamebase > Console ご利用ガイド > アプリ

TOAST Consoleで **Game > Gamebase > App**をクリックしてアプリの基本情報を設定することができます。

* **アプリ**：アプリ情報管理
* **クライアント**：クライアントバージョンとステータス情報管理
* **インストール URL**：アプリのストアごとのインストールURL管理


## App

Gamebaesサービスを有効にすると自動でアプリが作成され、該当するメニューでは登録された情報の修正のみ可能です。
一つのTOAST プロジェクトにつき一つのGamebaseアプリを管理することができるため、アプリを追加で登録したり削除することはできません。Gamebaseサービスを無効にするとアプリに登録された情報が削除されます。
各項目ごとの詳細説明は、下の**Properties**項目を参考にします。

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_App1_1.2.png)

### Properties

Gamebase Consoleで管理するアプリ情報を説明します。

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_App2_1.2.png)

#### (1) インストールURL
アプリインストールとPRに利用できる短縮URLの情報です。
アプリがリリースされたストアが複数ある場合にも、一つの短縮URLで管理することができます。
詳細な動作及び管理方法は、次のリンクを参考にします。[インストールURL管理](./oper-app/#installed-url)

> [参考] 
> Gamebaseを有効にすると自動で作成されるため、変更はできません。

####(2) サーバーアドレス
ゲームからゲームサーバーアドレス(IP、URLなど)をリアルタイムで受け取らなければならないときに使用します。
サーバーアドレスを設定するとクライアント初期化後に「Launching情報」から入力された情報を確認することができます。
ゲームで必要な場合にのみ入力し、そうでない場合は、空白のままにしておいてください。

####(3) カスタマーセンター情報
カスタマーセンターのページの他にEmail、電話番号などの情報を入力します。
カスタマーセンターのページがある場合は、下の**アプリ内URL**の**カスタマーセンター**に情報を入力します。
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

### Test Device
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_App3_1.0.png)

テスト端末に登録されるとGamebaseを使用するアプリがメンテナンス中でも正常にゲームにアクセスすることができます。
テスト端末を登録するためには、**Device Key**を入力する必要があります。直接入力したり**ゲームユーザID**を照会して登録することができます。
これ以上使用しないテスト端末を削除することもできます。

> [参考] 
>テスト端末は、最大100本まで登録することができます。

#### (1) 照会

アプリに登録された全てのテスト端末を確認することができます。検索ワードを**Search**に入力して検索条件に合ったテスト端末を検索することができます。

#### (2) 登録

照会画面から**登録**ボタンをクリックすると、テスト端末を登録することができる画面が表示されます。**Device Key**を直接入力したり**ゲームユーザID**を検索してテスト端末を登録することができます。

**(1) ゲームユーザIDを通じた登録**

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_App3_2.1.png)

タイプにユーザIDを選択し、ゲームユーザIDを入力して**検索**ボタンをクリックすると画面下にユーザーのログインログ内訳が照会されます。照会された内訳からテスト端末に登録するDevice Keyを選択して**追加情報**を入力し、**登録**ボタンをクリックすると、該当するDevice keyがテスト端末情報に登録されます。

> [参考] 
> 追加情報には、ユーザーが確認しやすいように別称を入力してください。例) iPhone6テスト、トースト様のiPad

**(2)Device Keyを通した登録**
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_App3_3.0.png)

登録するDevice keyを知っている場合、タイプに**Device Key**を選択して直接テスト端末を登録することができます。
Device Keyと登録する端末の**追加情報**を入力した後、登録ボタンを押すとテスト端末に登録されます。

#### (3) 削除

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_App3_4.0.png)

テスト端末照会画面から削除するテスト端末をチェックした後、左上の削除ボタンをクリックするとテスト端末情報が削除されます。削除された情報は復旧できないため、削除する前にもう一度確認してから削除してください。

### Authentication Information

#### 1. Facebook
Facebookの開発サイトに登録したアプリの{アプリID}と{アプリシークレットコード}をGamebase Consoleに入力します。
この時、ログインする際に必要な{Facebook Permission}もJSON Stringタイプで追加情報欄に入力する必要があります。

**入力フィールド** 

- ClientID：{AppID} 
- Secret Key：{App Secret Code} 
- 追加情報：Facebook Permission (json format) 


![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_facebook_1.0.png)

**[例] facebook_permission format **
* Facebook의 경우, OAuth 인증 시도 시, Facebook에 요청할 정보의 종류를 설정해야 합니다.

```json
{ "facebook_permission": [ "public_profile", "email"] }
```

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_facebook_permission_1.0.png)

**Reference URL**<br />

- [Facebookの開発者サイト](https://developers.facebook.com/)
- [Facebookの権限](https://developers.facebook.com/docs/facebook-login/permissions/)

##### Android & iOS & Unity
TOAST Console에서의 설정 외에 추가 설정은 없습니다.



#### 2. Google

##### Google Cloud Console

1. Google 인증을 위해서는 Google Cloud Console에서 **Web Application Client ID**를 발급받아 Gamebase Console에 입력해야 합니다.
	* ![google console](http://static.toastoven.net/prod_gamebase/DevelopersGuide/aos-google-console-001_1.11.0.png)
	* ![google console](http://static.toastoven.net/prod_gamebase/DevelopersGuide/aos-google-console-002_1.11.0.png)
2. 승인된 리디렉션 URI 란에 다음 값을 입력합니다.
	* https://alpha-id-gamebase.toast.com/oauth/callback
	* https://beta-id-gamebase.toast.com/oauth/callback
	* https://id-gamebase.toast.com/oauth/callback
	* ![google console](http://static.toastoven.net/prod_gamebase/DevelopersGuide/aos-google-console-003_1.11.0.png)
  
##### iOS

> <font color="red">[주의]</font><br/>
>
> Gamebase iOS SDK 1.12.2 버전에서 URL Scheme의 설정 방법이 변경 되었습니다. 사용 SDK 버전에 맞는 가이드를 확인하여 설정하시기 바랍니다.
>

* 1.12.1 이하
	* AdditionalInfo를 설정해야 합니다.
		* **TOAST Console > Gamebase > App > 인증 정보 > 추가 정보 & Callback URL**의 **추가 정보** 항목에 JSON string 형태의 정보를 설정해야 합니다.
		* GOOGLE의 경우, iOS 앱에서 필요한 정보 **url_scheme_ios_only**의 설정이 필요합니다.
		* **url_scheme_ios_only**의 값은 Xcode의 URL Scheme에 등록된 값들 중 한개와 일치해야 합니다.

	* URL Schemes를 설정해야 합니다.
		* **XCode > Target > Info > URL Types**

* 1.12.2 이상
	* URL Scheme를 설정해야 합니다.
		* **XCode > Target > Info > URL Types**에 `tcgb.{Bundle ID}.google`를 추가해야 합니다.

* GOOGLE 추가 인증 정보 입력 예제

```json
{ "url_scheme_ios_only": "Your URL Schemes" }
```

![Google URL Types](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-auth-001_1.7.0.png)


#### 3. Apple Game Center
Appleの開発者サイトに登録されたBundleIDをGamebase Consoleに入力します。

**入力フィールド**<br />

- ClientID：{Bundle ID}<br />

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_apple_1.0.png)

**Reference URL**<br />

- [Apple Developerサイト](https://developer.apple.com/)
- [Apple iTunes Connect](https://itunesconnect.apple.com/)

#### 4. PAYCO
PAYCO Client IDを申請して発行された{client_id}及び{client_secret}をGamebase Consoleに入力します。

**入力フィールド**<br />

- ClientID: {Payco client_id}
- Secret Key: {Payco client_secret}
- 추가정보: Payco Service & Service Name (JSON format)

##### Android & Unity
- AdditionalInfo를 설정해야 합니다.
    * **TOAST Console > Gamebase > App > 인증 정보 > 추가 정보** 항목에 JSON string 형태의 정보를 설정해야 합니다.
    * PAYCO의 경우, PaycoSDK에서 요구하는 **service_code**와 **service_name**을 설정해야 합니다.

* PAYCO 추가 인증 정보 입력 예제

```json
{ "service_code": "HANGAME", "service_name": "Your Service Name" }
```

##### iOS

> <font color="red">[주의]</font><br/>
>
> Gamebase iOS SDK 1.12.2 버전에서 URL Scheme의 설정 방법이 변경 되었습니다. 사용 SDK 버전에 맞는 가이드를 확인하여 설정하시기 바랍니다.
>

* 1.12.1 이하
	* AdditionalInfo를 설정해야 합니다.
		* **TOAST Console > Gamebase > App > 인증 정보 > 추가 정보** 항목에 JSON string 형태의 정보를 설정해야 합니다.
		* PAYCO의 경우, PaycoSDK에서 요구하는 **service_code**와 **service_name**을 설정해야 합니다.

* 1.12.2 이상
	* AdditionalInfo를 설정해야 합니다.
		* **TOAST Console > Gamebase > App > 인증 정보 > 추가 정보** 항목에 JSON string 형태의 정보를 설정해야 합니다.
		* PAYCO의 경우, PaycoSDK에서 요구하는 **service_code**와 **service_name**을 설정해야 합니다.
	* URL Scheme를 설정해야 합니다.
		* **XCode > Target > Info > URL Types**에 `tcgb.{Bundle ID}.payco`를 추가해야 합니다.

* PAYCO 추가 인증 정보 입력 예제

```json
{ "service_code": "HANGAME", "service_name": "Your Service Name" }
```

![Payco URL Types](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-auth-001_1.7.0.png)

#### 5.NAVER
NAVER Developersサイトで申請して発行された{client_id}及び{client_secret}をGamebase Consoleに入力します。
이때, 로그인 동의 창에서 표시할 애플리케이션 이름인 **service_name** 을 설정해야 하고, iOS 의 경우에는 추가로 **url_scheme_ios_only** 값을 JSON String 형태로 추가 정보란에 입력해야 합니다.

**入力フィールド**

- Client ID: {NAVER client_id}
- Secret Key: {NAVER client_secret}
- 追加情報: NAVER Application Name & iOS url scheme (json format)

**Reference URL**<br />
- [NAVER Developers - アプリケーション登録](https://developers.naver.com/apps/#/register)
- [NAVER Developers - クライアントIDとクライアントシークレット確認](https://developers.naver.com/docs/common/openapiguide/#/appregister.md)

##### Android & Unity
* **TOAST Console > Gamebase > App > 인증 정보 > 추가 정보 & Callback URL**의 **추가 정보** 항목에 JSON String 형태의 정보를 설정해야합니다.
	* NAVER의 경우, 로그인 동의 창에 표시할 앱 이름인 **service_name**을 설정해야 합니다.

```json
{"service_name": "Your Service Name" }
```

##### iOS

> <font color="red">[주의]</font><br/>
>
> Gamebase iOS SDK 1.12.2 버전에서 URL Scheme의 설정 방법이 변경 되었습니다. 사용 SDK 버전에 맞는 가이드를 확인하여 설정하시기 바랍니다.
>

* 1.12.1 이하
	* **TOAST Console > Gamebase > App > 인증 정보 > 추가 정보 & Callback URL**의 **추가 정보** 항목에 JSON String 형태의 정보를 설정해야합니다.
		* NAVER의 경우, 로그인 동의 창에 표시할 앱 이름인 **service_name**을 설정해야 합니다.
		* iOS 앱에서 필요한 정보인 **url_scheme_ios_only**를 추가로 설정해야 합니다.

	* URL Schemes를 설정해야 합니다.
		* **XCode > Target > Info > URL Types**

* 1.12.2 이상
	* **TOAST Console > Gamebase > App > 인증 정보 > 추가 정보 & Callback URL**의 **추가 정보** 항목에 JSON String 형태의 정보를 설정해야합니다.
		* NAVER의 경우, 로그인 동의 창에 표시할 앱 이름인 **service_name**을 설정해야 합니다.

	* URL Scheme를 설정해야 합니다.
		* **XCode > Target > Info > URL Types**에 `tcgb.{Bundle ID}.naver`를 추가해야 합니다.

* NAVER 추가 인증 정보 입력 예제

```json
{ "url_scheme_ios_only": "Your URL Schemes", "service_name": "Your Service Name" }
```

![Naver URL Types](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-auth-001_1.7.0.png)


#### 6. Twitter
Twitter Application Management 사이트에서 앱을 등록하고 발급받은 {Consumer Key} 및 {Consumer Secret}을 Gamebase Console에 입력합니다.

**입력 필드**

- Client ID: {Twitter Consumer Key}
- Secret Key: {Twitter Consumer Secret}

**Reference URL**
- [Twitter Application Management](https://apps.twitter.com/)

##### iOS

 > <font color="red">[주의]</font><br/>
 >
 > Gamebase iOS SDK 1.14.0 버전에서 URL Scheme의 설정 방법이 변경 되었습니다. 사용 SDK 버전에 맞는 가이드를 확인하여 설정하시기 바랍니다.
 >

 * 1.13.0 이하
	* 별도의 URL Scheme 설정이 필요하지 않습니다.

* 1.14.0 이상
	* URL Scheme를 설정해야 합니다.
		* **XCode > Target > Info > URL Types**에 `tcgb.{Bundle ID}.twitter`를 추가해야 합니다.

#### 7. LINE

**입력 필드**
- Client ID: {LINE Channel ID}
- Secret Key: {LINE Channel Secret}

**Reference URL**

- [LINE Developer Console](https://developers.line.me/console/)

##### iOS
LINE Login 기능을 사용하기 위하여, Xcode에 추가 설정이 필요합니다.
- URL Schemes를 설정해야 합니다.
	* **XCode > Target > Info > URL Types**에 `line3rdp.{App Bundle ID}`를 추가해야 합니다.

- Info.plist파일을 설정해야합니다.
	* LINE에서 발급받은 ChannelID를 설정합니다.
	```xml
	<key>LineSDKConfig</key>
	<dict>
    	<key>ChannelID</key>
    	<string>{Issued LINE ChannleID}</string>
	</dict>
	```
	* ATS 설정을 위하여 scheme을 등록합니다.
	```xml
	<key>LSApplicationQueriesSchemes</key>
	<array>
    	<string>lineauth</string>
    	<string>line3rdp.{App Bundle ID}</string>
	</array>
	```
- LINE Login을 사용하기 위한 프로젝트 설정은 다음 링크를 참고합니다. (인증 필요)
* [LINK \[LINE Developer Guide\]](https://developers.line.me/en/docs/line-login/ios/try-line-login/)



## Client

クライアント情報をOS(iOS、Android、Unity WebGL、Unity Standalone)、バージョンごとに管理することができます。

### Client List
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client1_1.2.png)
現在登録されているクライアントのリストを確認することができます。
OSごとに分かれて表示され、アイコンにある数字は、クライアントを登録する際に入力したバージョンを意味します。
アイコンリストは、サービス状態が<font color="white" style="background-color:#F8BB28">テスト</font>、<font color="white" style="background-color:#FB8F37">審査中</font>、<font color="white" style="background-color:#88C637">サービス</font>、<font color="white" style="background-color:#2AB1A6">アップデートお勧め(サービス中)</font>のリストのみ表示されます。各OS右下の矢印をクリックすると<font color="white" style="background-color:#A1A1A1">アップデート必須</font>、<font color="white" style="background-color:#CCCCCC">終了</font>状態のクライアントリストを確認することができます。
アイコンカラーがサービス状態ごとに区分されていて、一目でサービスの状態を把握することができます。

### Properties

Gamebase Consoleから管理するクライアント登録情報について説明します。
**クライアント**のタブから**AOS登録**、**iOS登録**ボタンなどをクリックすると、クライアント登録画面が表示されます。登録されたクライアントの入力値を修正したり削除したい場合、アイコンリストからアイコンをクリックしたり、クライアント全体のリストから修正したり削除したいクライアントを選択してください。
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client6_1.0.png)
#### (1) ストア
(<font color="red">必須</font>)クライアントをリリースするストアを選択します。
OSごとに選択可能なストアが異なります。
#### (2) ゲームバージョン
(<font color="red">必須</font>) クライアントバージョンを入力します。
ゲームで決めたルールに沿って文字列で入力してください。
#### (3) サービス状態
(<font color="red">必須</font>) クライアントのサービス状態を選択します。
状態は、<font color="white" style="background-color:#F8BB28">テスト</font>、<font color="white" style="background-color:#FB8F37">審査中</font>、<font color="white" style="background-color:#88C637">サービス</font>、<font color="white" style="background-color:#2AB1A6">アップデートお勧め(サービス中)</font>、<font color="white" style="background-color:#A1A1A1">アップデート必須</font>、<font color="white" style="background-color:#CCCCCC">終了</font>の6つです。

- <font color="white" style="background-color:#F8BB28">テスト</font>：内部テスト
- <font color="white" style="background-color:#FB8F37">審査中</font>：ストア審査中。安定化指標を追加で設定することができます。
  ![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client4_1.1.png)

> [参考]
> 「安定化指標」とは？<br/>
> Gamebase内部でGamebase APIを呼び出す際に渡す指標ログです。
> サービス状態が「審査中」のときに安定化指標を有効にすると、審査中にGamebase内部で問題が発生した場合に比べ、より簡単に問題を確認することができます。
> Gamebase Consoleからリアルタイムで安定化指標を使用するかどうかとログレベルを設定することができます。

- <font color="white" style="background-color:#88C637">サービス中</font>：サービスが正常に動作しています。
- <font color="white" style="background-color:#2AB1A6">アップデートお勧め(サービス中)</font>：サービスが正常に動作しています。                               <br/>より安定的なバージョンを使用するように誘導するためにポップアップを表示します。新しいバージョンをダウンロードして利用するように誘導しますが、ユーザーが希望する場合、現在のバージョンでも引き続きサービスを利用することができます。<br />下は、「アップデートを推奨します。(サービス中)」の状態のときにGamebase SDKで基本的に提供するポップアップです。
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client_updateRecommended_1.0.png)

- <font color="white" style="background-color:#A1A1A1">アップデート必須</font>：サービスできません。<br/>現在ゲームでサービスに対応していないバージョンで、最新バージョンのインストールを案内するポップアップを表示します。<br />下は、「アップデートが必ず必要です。」の状態のときにGamebase SDKで基本的に提供するポップアップです。
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client_updateRequired_1.1.png)
>  <font color="red">[注意] </font> 
>  **アップデートが必ず必要な場合とメンテナンスが同時に設定**されている場合、サービス状態は、「アップデートが必ず必要です。」になります。
>  メンテナンス中にユーザーに対し「アップデートが必ず必要です。」のポップアップを表示したくない場合、メンテナンス完了後にサービス状態を「アップデートが必ず必要です。」に変更する必要があります。

- <font color="white" style="background-color:#CCCCCC">終了</font>：サービスできません。<br/> サービスが終了されたバージョンの場合に選択します。<br />下は、「終了」の状態のときにGamebase SDKで基本的に提供するポップアップです。
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client_ended_1.0.png)

> [参考] 
> サービス状態ごとに表示するメッセージ設定
> **アップデートを推奨します。(サービス中)**、**アップデートが必ず必要です。**、**終了**状態の場合にユーザーに表示する案内メッセージを多国語で設定することができます。
> サービス状態を選択すると、各状態に合った基本メッセージが5カ国語(韓国語、英語、日本語、中国語簡体字、中国語繁体字)で提供され、必要な場合、言語を追加したり基本メッセージの文章を変更することができます。
> ![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client5_1.1.png)

#### (4) サーバーアドレス
クライアントで利用するサーバーアドレス(IP、URL)を入力します。
**アプリ**タブでサーバーアドレスを入力するとすべてのクライアントに適用されるため、クライアントごとに他のサーバーアドレスを使用したい場合にのみサーバーアドレスを入力します。


## Installed URL

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_InstallUrl1_1.2.png)

ゲームをインストールするためのストアURL情報を管理します。
ユーザーがPCやモバイルで短縮URLをクリックすると、ユーザー端末情報(デバイス、OS、ストアなど)を利用して入力されたサイトへリダイレクションします。
ストア情報がなかったり、ストアへの移動に失敗した場合、「COMMON」に設定されているURLへ移動します。

_[例1] Android端末からSMSで受信したインストールURLをクリックする場合
**(Device:mobile,OS:Android,Store:なし)**Androidのうち、代表ストアに指定されたモバイルURLへ移動。代表ストアが「Google Play」の場合、「Google Play」モバイルに設定されているURLへ移動。
_[例2] 「One Store」からダウンロードしたアプリでゲームしていたユーザーが「アップデートが必ず必要です。」のポップアップウィンドウから「今すぐアップデート」のボタンをクリックした場合_
**(Device:mobile,OS:Android,Store:One Store)**「One Store」モバイルに設定されているURLへ移動(One Storeモバイルインストールページ)<br/>
_[例3] PCからインストールURLを入力した場合_
**(Device:PC、OS:Windows、Store:なし)** COMMON PCに設定されているURLへ移動


### Properties

入力されたインストールURL情報を変更したい場合、**修正**ボタンをクリックします。

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_InstallUrl2_1.2.png)

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


