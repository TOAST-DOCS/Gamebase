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
![[]](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap_google_Oauth_clientSecret_ja.png

##### 4. [OAuth Playground](https://developers.google.com/oauthplayground/) > oauthplayground 設定 > Use your own OAuth credentials 使用
![[그림 4] OAuth クライアント生成 3](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap_g_03.png)


##### 5. Step 1で https://www.googleapis.com/auth/androidpublisher 入力して Authorization code コード発給
![[그림 5] OAuth クライアント生成 4](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap_g_04.png)


##### 6. Step 2で Exchange authorization code for tokens ボタンを押してトークン発給
![[그림 6] OAuth クライアント生成 5](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap_g_05.png)


### Google Play連動注意事項

OAuth 認証情報生成後、以下のガイドを参考にプロジェクト設定を進めます。

> [参考]
> Google Guide : https://developers.google.com/android-publisher/getting_started

#### 1. Google Play Android Developer APIの有効化の状態を確認します。

```
  - https://console.developers.google.com > APIs & Services > Dashboard
```
![[]](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap-console-google-console-1.png)


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
7. ドメイン認証URLのhtmlファイル名をTOAST CONSOLEアプリの登録時に入力する。
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