## Game > Gamebase > Store 設定ガイド > Google 設定ガイド

Google一般商品および購読商品のアプリ内決済のためにGoogle Play請求サービスと連動させる必要があります。
Google Play請求サービスは、Google PlayコンソールとGoogle APIコンソールで生成された値を使用します。
さらにGoogle購読商品の決済のため、Notificationを設定する必要があります。
Notificationの設定が正しくなければ購読決済が進みません。

## Google Application Key
下記情報をIAPアプリ情報に登録します。

| Key | Description                                             |
| ---------------------------------- | ---------------------------------------------- |
| Google In App Purchase License Key | Google Play Public KEY(RSA)       |
| Google API Client ID               | Google API Project OAuth Client ID            |
| Google API Client Secret           | Google API Project OAuth Client Secret        |
| Refresh Token For Google OAuth     | Google Play Developer 勘定を通じて獲得したRefresh Token |

## Googleが提供するコンソール
| Console        | Location                              |
| -------------- | ------------------------------- |
| Google Play Console | https://play.google.com/console/developers |
| Google API Console | https://console.developers.google.com/apis/dashboard |

## Google Playコンソール

### Google In App Purchase License Key 確認
```
https://play.google.com/console/developers > [Select App] > Monetize > Monetization setup > Licensing
```
![[]](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/2020-google_license_en.png)

## Google APIコンソール

* Androidデベロッパーズガイド
	* [Android Developers - アプリ内課金の管理](http://developer.android.com/google/play/billing/billing_admin.html)
	* [Android Developers - OAuth認証](https://developers.google.com/identity/protocols/OAuth2WebServer)

### OAuth クライアント生成
```
Google Playコンソールと同一のアカウントでGoogle APIコンソールにプロジェクトを生成します。
下記の図を参照してOAuth 認証に必要な情報3 つを生成します。
1) Client ID  
2) Client Secret  
3) Refresh Token  
```

##### 1. https://console.cloud.google.com/apis/credentials でOAuthクライアントを生成(ウェブアプリケーション)
![[그림 1] OAuth クライアント生成 1](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap_google_credentials_ja.png)

##### 2. 承認された redirection urlに https://developers.google.com/oauthplayground 入力
![[그림 2] OAuth クライアント生成 2](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap_google_Oauth_ja.png)

##### 3. 作成後にポップアップウィンドウからクライアントID /クライアントシークレットをコピーする
![[]](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap_google_Oauth_clientSecret_ja.png)

##### 4. [OAuth Playground](https://developers.google.com/oauthplayground/) > oauthplayground 設定 > Use your own OAuth credentials 使用
![[그림 4] OAuth クライアント生成 3](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap_g_03.png)


## 2つの連動認証方式を提供

![NHN Cloud IAPアプリ設定](https://static.toastoven.net/prod_gamebase/StoreConsoleGuide/gamebase_google_iap_console_ja_202210.png)

- Google連携を行うには、GoogleのAndroid開発者APIにアクセスするための認証が必要です。
- NHN Cloud IAPは、2つの認証モデルを提供し、それぞれ認証のために異なる特化情報が必要です。
- モデル別特化情報のほか、共通で必要な情報もGoogle連携および決済確認のために必要なため、**共通入力情報もあわせて確認**する必要があります。
    - `Store App ID`：共通入力情報ガイドPackage Name参照
    - `Google In App Purchase License Key`：共通入力情報ガイドInAppPurchase License Public Key参照
    - `マーケット連動検証省略`：共通入力情報ガイドマーケット検証省略を参照
- モデル特化/共通必要情報はGoogleが提供する以下のガイドラインに従ってください。 [Google Cloud Platform Console](https://console.cloud.google.com/apis/dashboard), [Google Play Console](https://play.google.com/console/developers), [Google Developer Console](https://developers.google.com/oauthplayground/)などから入手できます。
- Google Playアプリの決済情報を確認するための[Google Android Publisher API](https://developers.google.com/android-publisher)はOAuth2必須認証対象APIです。


## 最高管理者(Supervisor)認証モデル
- 登録するアプリをGoogleコンソールで管理するGoogleアカウントの認証を代行するモデルであり、NHN Cloud IAPがデフォルト値として提供してきた認証モデルです。
- Google Consoleでは、1つのGoogleアカウントに複数のアプリを登録管理することができ、同じアカウントに登録されたアプリのNHN Cloud IAP設定情報はすべて同じです。
- 次の案内に従って合計3つの情報を確認してNHN Cloud IAPのアプリ情報として入力する必要があります。この情報は該当GoogleアカウントのOAuth認証に使用されます。

1. NHN Cloud IAP Google最高管理者認証モデル特化入力情報
    - `Google API Client ID`：最高管理者認証モデルガイドのステップ4を参照
    - `Google API Client Secret`：最高管理者認証モデルガイドのステップ4を参照
    - `Refresh Token For Google Oauth`：最高管理者認証モデルガイドのステップ6を参照
      ![最高管理者モデル設定](https://static.toastoven.net/prod_gamebase/StoreConsoleGuide/gamebase_google_iap_console_supervisor_ja_202210.png)


### Google Play連動注意事項

OAuth 認証情報生成後、以下のガイドを参考にプロジェクト設定を進めます。

> [参考]
> Google Guide : https://developers.google.com/android-publisher/getting_started

#### 1. Google Play Android Developer APIの有効化の状態を確認します。

4. 新規OAuth Clientが作成されると、作成されたOAuth Clientの2つの情報についての案内ポップアップが表示されます。
    - クライアントID：NHN Cloud IAPアプリ情報設定画面の`Google API Client ID`の値を入力します。
    - クライアントセキュリティパスワード：NHN Cloud IAPアプリ情報設定画面の`Google API Client Secret`の値を入力します。
    - 上記の2つの情報はOAuth Client情報画面で再確認することができ、Googleが提供するOAuth Client情報JSONダウンロードファイルでも確認できます。

   ![最高管理者モデル設定](https://static.toastoven.net/prod_iap/console_google/google_supervisor_step_03.png)

5. [Google OAuth 2 Playground](https://developers.google.com/oauthplayground)ページに移動
    - 画面右上の歯車ボタンを押すと表示される**OAuth 2.0 Configuration**ポップアップ設定でUse your own OAuth credentialsチェックボックスをチェックします。
    - OAuth 2.0 Configurationポップアップ設定のClient ID、Client Secretにステップ4で取得した情報を入力します。
    - 左側のAPI権限範囲選択画面で`Google Play Android Developer API`を選択します。
        - 左側の選択画面下の範囲入力部分に`https://www.googleapis.com/auth/androidpublisher`を入力しても構いません。
    - すべての設定値の入力と選択が完了したら左下のAuthorize APIボタンをクリックします。

   ![最高管理者モデル設定](https://static.toastoven.net/prod_iap/console_google/google_supervisor_step_04.png)

6. Authorize APIボタンをクリックするとGoogle OAuth 2 PlaygroundページがStep 2に変わり、追加情報が表示されます。
    - Refresh Token：NHN Cloud IAPアプリ情報設定画面の`Refresh Token For Google Oauth`の値を入力します。

   ![最高管理者モデル設定](https://static.toastoven.net/prod_iap/console_google/google_supervisor_step_05.png)

## サービスアカウント認証モデル
- 最高管理者アカウントが委任した権限を持つGoogle内サービスアカウントの認証を代行するモデルであり、 2022年4月NHN Cloud IAPに新規追加された認証サポートモデルです。
- 権限の範囲を委任されるサービスアカウントの作成および管理は最高管理者(Googleの実際の管理アカウント)がGoogleコンソールで行う必要があります。
- サービスアカウントは最高管理者によって最高管理者に準ずる範囲の権限を受任することもでき、最高管理者が権限を持つ特定アプリだけに限定された範囲の権限を受任することもできます。
    - 委任権限範囲の設定戦略は、お客様の意図した特性に合わせて設定してください。
- 最高管理者認証モデルが持つ権限の範囲に負担を感じる場合は、Googleコンソール内でサービスアカウント作成後にこの認証モデルを使用してください。 ([Googleガイド](https://developers.google.com/identity/protocols/oauth2/service-account))

1. NHN Cloud IAPサービスアカウント認証モデル特化入力情報
    - `サービスアカウント連動情報`：サービスアカウント認証モデルガイドのステップ5を参照 
      ![サービスアカウントモデル設定](https://static.toastoven.net/prod_gamebase/StoreConsoleGuide/gamebase_google_iap_console_service_account_ja_202210.png)

2. [Google Cloud Console](https://console.cloud.google.com/apis/dashboard) APIおよびサービスページに移動
    - Google Play管理アカウントと連携するプロジェクトを選択
    - **ユーザー認証情報**メニューに移動
    - **ユーザー認証情報を作成** > **サービスアカウント**を選択

   ![サービスアカウントモデル設定](https://static.toastoven.net/prod_iap/console_google/google_service_account_step_02.png)

3. 新規サービスアカウントの基本情報の入力
    - 管理しやすい形の基本情報を入力した後、`作成して続行`をクリックします。
    - サービスアカウントへのアクセス権限付与で役割を`所有者`と選択します。
    - 追加選択事項で作成するサービスアカウントの管理者メールを入力すると、その管理者にサービスアカウント設定の権限が付与されます。
        - Google Cloud Console内に存在しない管理者メールアドレスの場合は、入力したメールアドレスに管理者招待メールが送信されます。

   ![サービスアカウントモデル設定](https://static.toastoven.net/prod_iap/console_google/google_service_account_step_03.png)

4. サービスアカウントへの権限委任
    - **Google Play Console** > **APIアクセス**メニューに移動
    - Google Cloud Consoleで作成したサービスアカウントが画面下部のサービスアカウントリストに表示されます。作成したサービスアカウント右側の**権限付与**リンクをクリックします。
    - 権限の範囲は最高管理者アカウントが持つアプリ全体またはアプリ個別単位で複数指定することができ、範囲内で権限の種類を指定できます。
    - 範囲は顧客の意図に合わせて設定しますが、権限の種類のうち`アプリ情報表示(読み取り専用)`、`財務データ表示`、`注文および購読管理`の権限は必ず委任設定する必要があります。
    - 委任設定された権限は反映されるまで数日かかる場合があります。委任権限が反映されるまで最長7日程度かかった事例がありますのでご参考ください。

   ![サービスアカウントモデル設定](https://static.toastoven.net/prod_iap/console_google/google_service_account_step_05.png)

5. APIおよびサービスページの新しく作成されたサービスアカウントリストのうち、作成した**サービスアカウント**の詳細情報画面に移動
    - キータブに移動して、キー追加 > 新しいキーを作成
    - 新規ポップアップで`JSON`形式を選択して作成をクリック
    - キーの作成が完了すると、自動的にJSON形式のファイルをGoogleからダウンロードします。
    - そのファイルをWindowsのメモ帳などの純粋なテキストエディタで開き、すべての内容をコピーした後、NHN Cloud IAPアプリ情報設定画面の`サービスアカウント連動情報`に入力する必要があります。
    - ダウンロードしたファイルは最初のダウンロード後に、再ダウンロードすることはできませんので、大切に保管して入力してください。
    - ステップ4で言及したように、Google内部でまだ権限が反映されていない場合は権限なし登録エラーが発生することがありますので、このような場合は時間をおいて再試行してください。

   ![サービスアカウントモデル設定](https://static.toastoven.net/prod_iap/console_google/google_service_account_step_04.png)


#### 2. Google Play Developer Consoleでリンクされたプロジェクトを確認します。
 
```
  - https://play.google.com/apps/publish > Settings > Developer account > API access
```
![[]](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap-console-google-console-2.png)

#### 3. Google Play Developer ConsoleでリンクされたプロジェクトがGoogleAPIのOAuthクライアントを作成プロジェクトと同じであることを確認します。
![[]](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap_google_linked_ja.png)

## Googleリアルタイムデベロッパー通知設定

> [参考]
> Google Cloud (https://cloud.google.com) Platformを使用しなければなりません。
> Google Cloud Platform に決済プロフィールを登録し、使用状態に変更しなければなりません。


### Google Cloud > ビッグデータ > 掲示/購読

掲示/購読コンソール(https://console.cloud.google.com/cloudpubsub)で下記の作業を行います。

#### トピックを作成 (プロダクト > Pub/Sub)

```
1. トピックを作成されると、オプションの権限をクリックするかトピック名をクリックしてトピック詳細情報ページに移動します。
2. トピックの詳細情報ページで、役割の選択をクリックし掲示/購読の掲示者を選択します。
3. メンバーの追加ボタンをクリックし google-play-developer-notifications@system.gserviceaccount.com を入力します。
4. 保存ボタンをクリックします。
```
![[] Topic 만들기](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap_google_createTopic_ja.png)
![[] Topic 수정하기](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap_google_create_subscription_ja.png)

#### サブスクリプションの作成
```
1. トピック名の右の設定ボタンをクリック > 新しいサブスクリプションを追加 
2. 配信タイプ
- [エンドポイントURLでプッシュ] 選択
- URL :  https://api-iap.cloud.toast.com/callback/subscription/{YOUR_PACKAGE_NAME}/GG
- {YOUR_PACKAGE_NAME} : google package name
```
![[] Subscription 만들기](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap_google_new_subscirption_ja.png)
![[] Subscription 만들기](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap_google_create_subscription_ja.png)


#### IAP ドメイン検証

```
1. https://console.cloud.google.com/apis/credentials/domainverification
2. [ドメイン確認]タブで[ドメイン追加] をクリックします。
3. https://api-iap.cloud.toast.comを入力します。
4. [今移動]ボタンを押してウェブマスターセンターに移動します。
5. ウェブマスターセンターでプロパティを合わせることをクリックします。
6. [プロパティ追加]に https://api-iap.cloud.toast.com を入力します。
7. ドメイン認証URLのhtmlファイル名をNHN Cloud CONSOLEアプリの登録時に入力する。
    -> ex) https://api-iap.cloud.toast.com/googleabc.htmlの場合、googleabc.html入力
8. [推奨方法] 下段の [ロボットではない] クリック後、[OK] をクリックします。
9. 認証に成功すると最後のイメージと同じ画面が表示されます。 この画面が表示されないと購読決済を正常に使用できません。
```
![[] domain verification](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap-console-domain-verification_ja_1.png)
![[] domain verification](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap_google_add_domain_ja.png)
![[] domain verification](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap-console-domain-verification_ja_3.png)
![[] domain verification](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/google_domain_auth.png)
![[] domain verification](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap-console-domain-verification_ja_4.png)
![[] domain verification](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap-console-domain-verification_ja_5.png)

### Realtime developer notifications
````
https://play.google.com/console/developers > [Select App] > Monetize > Monetization setup > Real-time developer notificqations
````
![[]realtime notification](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/2020-google_realtime_notification_en.png)