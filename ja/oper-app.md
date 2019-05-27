## Game > Gamebase > Console ご利用ガイド > アプリ

TOAST Consoleで **Game > Gamebase > App**をクリックしてアプリの基本情報を設定することができます。

* **アプリ**：アプリ情報管理
* **クライアント**：クライアントバージョンとステータス情報管理
* **インストール URL**：アプリのストアごとのインストールURL管理


## App

Gamebaesサービスを有効にすると自動でアプリが作成され、該当するメニューでは登録された情報の修正のみ可能です。
一つのTOAST プロジェクトにつき一つのGamebaseアプリを管理することができるため、アプリを追加で登録したり削除することはできません。Gamebaseサービスを無効にするとアプリに登録された情報が削除されます。
各項目ごとの詳細説明は、下の**Properties**項目を参考にします。

![gamebase_app_01_201812.png](https://static.toastoven.net/prod_gamebase/gamebase_app_01_201812.png)

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
![gamebase_app_02_201812.png](https://static.toastoven.net/prod_gamebase/gamebase_app_02_201812.png)

テスト端末に登録されるとGamebaseを使用するアプリがメンテナンス中でも正常にゲームにアクセスすることができます。
テスト端末を登録するためには、**Device Key**を入力する必要があります。直接入力したり**ゲームユーザID**を照会して登録することができます。
これ以上使用しないテスト端末を削除することもできます。

> [参考] 
>テスト端末は、最大100本まで登録することができます。

#### (1) 照会

アプリに登録された全てのテスト端末を確認することができます。検索ワードを**Search**に入力して検索条件に合ったテスト端末を検索することができます。

#### (2) 登録

照会画面から**登録**ボタンをクリックすると、テスト端末を登録することができる画面が表示されます。**Device Key**を直接入力したり**ゲームユーザID**を検索してテスト端末を登録することができます。

![gamebase_app_02_201901.png](https://static.toastoven.net/prod_gamebase/gamebase_app_02_201901.png)

**(A) ゲームユーザIDを通じた登録**

タイプにユーザIDを選択し、ゲームユーザIDを入力して**検索**ボタンをクリックすると画面下にユーザーのログインログ内訳が照会されます。照会された内訳からテスト端末に登録するDevice Keyを選択して**追加情報**を入力し、**登録**ボタンをクリックすると、該当するDevice keyがテスト端末情報に登録されます。

> [参考] 
> 追加情報には、ユーザーが確認しやすいように別称を入力してください。例) iPhone6テスト、トースト様のiPad

**(B)Device Keyを通した登録**

登録するDevice keyを知っている場合、タイプに**Device Key**を選択して直接テスト端末を登録することができます。
Device Keyと登録する端末の**追加情報**を入力した後、登録ボタンを押すとテスト端末に登録されます。

> [참고] 
> 추가 정보에는 사용자가 알아보기 편한 별칭을 입력하시면 됩니다. 예시) iPhone 6 테스트, 토스트님 아이패드

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


![gamebase_app_04_201812.png](https://static.toastoven.net/prod_gamebase/gamebase_app_04_201812.png)

**[例] facebook_permission format **
* Facebook의 경우, OAuth 인증 시도 시, Facebook에 요청할 정보의 종류를 설정해야 합니다.

```json
{ "facebook_permission": [ "public_profile", "email"] }
```

![gamebase_app_05_201812.png](https://static.toastoven.net/prod_gamebase/gamebase_app_05_201812.png)

**Reference URL**<br />

- [Facebookの開発者サイト](https://developers.facebook.com/)
- [Facebookの権限](https://developers.facebook.com/docs/facebook-login/permissions/)

##### Android & iOS & Unity
TOAST Console에서의 설정 외에 추가 설정은 없습니다.



#### 2. Google

##### Google Cloud Console

![gamebase_app_06_201812.png](https://static.toastoven.net/prod_gamebase/gamebase_app_06_201812.png)

1. Google 인증을 위해서는 Google Cloud Console에서 **Web Application Client ID**를 발급받아 Gamebase Console에 입력해야 합니다.
2. 승인된 리디렉션 URI 란에 다음 값을 입력합니다.
	* https://alpha-id-gamebase.toast.com/oauth/callback
	* https://beta-id-gamebase.toast.com/oauth/callback
	* https://id-gamebase.toast.com/oauth/callback

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

![gamebase_app_07_201812.png](https://static.toastoven.net/prod_gamebase/gamebase_app_07_201812.png)

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

![gamebase_app_07_201812.png](https://static.toastoven.net/prod_gamebase/gamebase_app_07_201812.png)

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
		* **XCode > Target > Info > URL Types**에 **tcgb.{Bundle ID}.twitter**를 추가해야 합니다.

* Twitter 추가 인증 정보 입력 예제

![Twitter URL Types](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-auth-001_1.7.0.png)

#### 7. LINE

**입력 필드**
- Client ID: {LINE Channel ID}
- Secret Key: {LINE Channel Secret}

**Reference URL**

- [LINE Developer Console](https://developers.line.me/console/)

##### iOS
LINE Login 기능을 사용하기 위하여, Xcode에 추가 설정이 필요합니다.
- URL Schemes를 설정해야 합니다.
	* **XCode > Target > Info > URL Types**에 **line3rdp.{App Bundle ID}**를 추가해야 합니다.

- Info.plist파일을 설정해야합니다.
	* LINE에서 발급받은 ChannelID를 설정합니다.
	```
	<key>LineSDKConfig</key>
	<dict>
    	<key>ChannelID</key>
    	<string>{Issued LINE ChannleID}</string>
	</dict>
	```
	* ATS 설정을 위하여 scheme을 등록합니다.
	```
	<key>LSApplicationQueriesSchemes</key>
	<array>
    	<string>lineauth</string>
    	<string>line3rdp.{App Bundle ID}</string>
	</array>
	```
- LINE Login을 사용하기 위한 프로젝트 설정은 다음 링크를 참고합니다. (인증 필요)
* [LINK \[LINE Developer Guide\]](https://developers.line.biz/en/docs/ios-sdk/objective-c/overview/)



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
![gamebase_app_13_201901.png](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_app_13_201901.png)
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
>  <font color="orange">[참고] </font>
>  업데이트 버튼을 누르면 설치 URL 메뉴에서 설정한 각각의 스토어 주소로 연결됩니다.
>  예를 들면 클라이언트가 App store로 설정되어 있고 설치 URL 메뉴에서 App store 관련 설정이 존재한다면 설정한 주소로 이동되며 만약 설치 URL 메뉴에 설정이 되어 있지 않을 경우 공통(Common) URL로 연결됩니다.
>  
- <font color="white" style="background-color:#CCCCCC">終了</font>：サービスできません。<br/> サービスが終了されたバージョンの場合に選択します。<br />下は、「終了」の状態のときにGamebase SDKで基本的に提供するポップアップです。
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client_ended_1.0.png)

> [参考] 
> サービス状態ごとに表示するメッセージ設定
> **アップデートを推奨します。(サービス中)**、**アップデートが必ず必要です。**、**終了**状態の場合にユーザーに表示する案内メッセージを多国語で設定することができます。
> サービス状態を選択すると、各状態に合った基本メッセージが5カ国語(韓国語、英語、日本語、中国語簡体字、中国語繁体字)で提供され、必要な場合、言語を追加したり基本メッセージの文章を変更することができます。
> ![gamebase_app_18_201812.png](https://static.toastoven.net/prod_gamebase/gamebase_app_18_201812.png)

#### (4) サーバーアドレス
クライアントで利用するサーバーアドレス(IP、URL)を入力します。
**アプリ**タブでサーバーアドレスを入力するとすべてのクライアントに適用されるため、クライアントごとに他のサーバーアドレスを使用したい場合にのみサーバーアドレスを入力します。

#### (5) Debug log
Gamebae SDK의 Debug Log 출력 여부를 Console을 통하여 실시간으로 변경할 수 있습니다.
설정되어 있지 않으면 기본적으로 Gamebase SDK 내부에 설정된 값을 우선으로 동작하고 Gamebase Console에서 Debug Log 출력 여부를 설정하실 수 있습니다.
Gamebase SDK에 Debug Log가 'OFF"상태이더라도 Console에서 'ON'으로 설정하시면 단말기에 Gamebase Debug Log가 출력됩니다.

## Installed URL

![gamebase_app_19_201812.png](https://static.toastoven.net/prod_gamebase/gamebase_app_19_201812.png)

* ゲームをインストールするためのストアURL情報を管理します。
* 클라이언트 상태 중 <font color="white" style="background-color:#2AB1A6">업데이트 권장(서비스 중)</font> 또는 <font color="white" style="background-color:#A1A1A1">업데이트 필수</font> 일 때 각각의 스토어 별로 제공할 주소들에 대한 값을 설정합니다.
* ユーザーがPCやモバイルで短縮URLをクリックすると、ユーザー端末情報(デバイス、OS、ストアなど)を利用して入力されたサイトへリダイレクションします。
* ストア情報がなかったり、ストアへの移動に失敗した場合、「COMMON」に設定されているURLへ移動します。

_[例1] Android端末からSMSで受信したインストールURLをクリックする場合
**(Device:mobile,OS:Android,Store:なし)**Androidのうち、代表ストアに指定されたモバイルURLへ移動。代表ストアが「Google Play」の場合、「Google Play」モバイルに設定されているURLへ移動。
_[例2] 「One Store」からダウンロードしたアプリでゲームしていたユーザーが「アップデートが必ず必要です。」のポップアップウィンドウから「今すぐアップデート」のボタンをクリックした場合_
**(Device:mobile,OS:Android,Store:One Store)**「One Store」モバイルに設定されているURLへ移動(One Storeモバイルインストールページ)<br/>
_[例3] PCからインストールURLを入力した場合_
**(Device:PC、OS:Windows、Store:なし)** COMMON PCに設定されているURLへ移動


### Properties

入力されたインストールURL情報を変更したい場合、**修正**ボタンをクリックします。

![gamebase_app_20_201812.png](https://static.toastoven.net/prod_gamebase/gamebase_app_20_201812.png)

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
Guest로 로그인한 유저가 다른 아이디 제공자를 연동하지 않고 다른 단말기에서 이어서 게임을 할 수 있는 기능을 제공합니다.
사용자는 현재 게임중인 단말에서 기기 이전을 위한 Key를 발급 받아 이전하려는 단말기에 Key를 입력하는 것만으로 쉽게 게임 기기를 변경할 수 있습니다.
기기이전 기능은 기본적으로 비활성화 되어 있으며 사용을 원하는 경우에는 기기이전 메뉴에서 **사용하기** 클릭하여 기능을 활성화 해야 합니다.

![gamebase_app_20_201812.png](https://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_TransferAccount1_1.0.png)
1) 기기 이전 기능을 사용하기 위해서는 **사용하기** 클릭하여 기능을 활성화 해야 합니다. 사용하기 버튼을 누르면 기기 이전 기능 사용을 위해 필요한 값을 설정하는 화면으로 전환됩니다.

![gamebase_app_20_201812.png](https://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_TransferAccount1_2.0.png)
기기 이전 기능에 필요한 값들을 설정 할 수 있는 화면입니다.
각 항목에 대한 설명은 아래와 같습니다.

#### 발급
기기 이전 발급키의 형식을 설정합니다.
사용자의 기기 이전키는 ID만 사용하거나 ID, 비밀번호 두 개의 키를 이용할 수 있습니다. ID, 비밀번호의 형식은 게임에서 원하는 소문자, 대문자, 숫자 조합으로 구성할 수 있습니다.
1) **ID 자동 발급 형식** : 기기 이전 ID 발급 형식을 설정합니다. 설정 항목은 아래와 같습니다.
  - **숫자(최소길이12)** : 숫자로만 이루어진 아이디를 발급합니다. 발급되는 ID의 최소길이는 12자입니다.
  - **숫자+소문자(최소길이10)** : 숫자와 소문자의 조합으로만 이루어진 ID를 발급합니다. 발급되는 ID의 최소길이는 10자입니다.
  - **숫자+대문자(최소길이10)** : 숫자와 대문자의 조합으로만 이루어진 ID를 발급합니다. 발급되는 ID의 최소길이는 10자입니다.
  - **숫자+소문자+대문자(최소길이:9)** : 숫자, 소문자, 대문자의 조합으로 이루어진 ID를 발급합니다. 발급되는 ID의 최소길이는 9자입니다.
  - **소문자+대문자(최소길이9)** : 소문자와 대문자의 조합으로만 이루어진 ID를 발급합니다. 발급되는 ID의 최소길이는 9자입니다.

2) **비밀번호 자동 발급 형식** : 기기 이전 ID를 이용하여 로그인할 때 사용할 비밀번호 발급 형식을 설정합니다. 설정 항목은 아래와 같습니다.
  - **비밀번호 사용 안함** : 비밀번호를 사용하지 않을 때 선택합니다. 해당항목을 선택하면 아래 검증항목에서 아이디의 유효시간만 설정할 수 있습니다.
  - **숫자(최소길이12)** : 숫자로만 이루어진 비밀전호를 발급합니다. 발급되는 비밀번호의 최소길이는 12자입니다.
  - **숫자+소문자(최소길이10)** : 숫자와 소문자의 조합으로만 이루어진 비밀번호를 발급합니다. 발급되는 비밀번호의 최소길이는 10자입니다.
  - **숫자+대문자(최소길이10)** : 숫자와 대문자의 조합으로만 이루어진 ID를 발급합니다. 발급되는 비밀번호의 최소길이는 10자입니다.
  - **숫자+소문자+대문자(최소길이:9)** : 숫자, 소문자, 대문자의 조합으로 이루어진 ID를 발급합니다. 발급되는 비밀번호의 최소길이는 9자입니다.
  - **소문자+대문자(최소길이9)** : 소문자와 대문자의 조합으로만 이루어진 ID를 발급합니다. 발급되는 비밀번호의 최소길이는 9자입니다.

#### 검증
발급된 기기이전키의 검증 조건을 설정합니다.
기기이전키의 검증시 이전횟수나 유효기간, 실패시 차단 등의 설정을 하실 수 있습니다.
3) **기기 이전 횟수** : 발급된 아이디의 기기 이전이 가능한 횟수를 설정합니다. 무제한/일회성 중 한가지를 선택해야 합니다.
4) **유효 기간** : 발급된 계정의 유효 시간을 설정합니다. 발급된 기기 이전 ID는 이 설정값의 영향을 받습니다. 무제한/기간 설정 중 한가지를 선택해야 합니다.
5) **실패 시 재검증 차단 여부** : 유저가 아이디를 이용하여 로그인을 시도했을 때 실패했을 경우 재시도에 대한 설정을 추가로 설정하실 수 있습니다. 해당 항목을 체크하면 차단 관련 설정을 추가로 진행해야 합니다.
6) **차단 기준 횟수** : 실패 시 재검증 차단 여부가 선택되었을 때 설정할 수 있는 항목입니다. 유저가 최대로 실패할 수 있는 횟수를 설정할 수 있습니다. 최소 1회 이상은 설정되어야 합니다.
7) **차단 기간** : 유저가 검증에 실패하여 계정이 차단되었을 경우 다시 재시도를 할 수 있는 시간에 대한 설정을 할 수 있습니다. 영구 차단/기간 지정 중 한가지를 선택해야 합니다.


#### 초기 설정 완료 이후
![gamebase_app_20_201812.png](https://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_TransferAccount1_3.0.png)
최초 설정이 완료되면 유저는 기기 이전 기능의 비활성화만 가능하며 설정 변경이 필요할 경우 고객센터에 문의하시기 바랍니다.
**사용 안함** 버튼을 클릭하여 기능을 비활성화 할 수 있고 기존에 발급된 기기이전키는 모두 삭제되기 때문에 활성화 이후에는 비활성화 여부를 신중하게 선택해야 합니다.
