<!-- pre-align:aligned sig=430e8193739d -->

<a id="game-gamebase-store-console-guide-huawei-appgallery-console-guide"></a>
## Game > Gamebase > Store 設定ガイド > Huawei AppGalleryコンソールガイド { #game-gamebase-store-console-guide-huawei-appgallery-console-guide }

<a id="huawei-developer-console"></a>
## Huawei Developer Console { #huawei-developer-console }
1. [ファーウェイ 開発者コンソール](https://developer.huawei.com/consumer/en/console)にアカウントを登録し <br/>
   エコシステムサービス / アプリサービス -> [AppGallery Connectコンソール](https://developer.huawei.com/consumer/en/service/josp/agc/index.html#/)に移動します。
   ![ファーウェイ 開発者コンソールでAppGallery Connect選択](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/huawei_console_kor.png)
2. [AppGallery Connectコンソール](https://developer.huawei.com/consumer/en/service/josp/agc/index.html#/)でMy Appsメニューに移動したら、新規アプリ登録や既存アプリの管理を行うことができます。
   ![AppGalleryコンソールメイン画面](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/huawei_console_app_main_eng.png)
   ![MyAppsメニューで新規アプリ登録](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/huawei_console_app_01_eng.png)
3. NHN Cloud Gamebaseは、ファーウェイ ストア連携の場合、Android APKパッケージタイプとモバイルデバイスのみをサポートします。<br/>
   また、アプリは最終的にプロジェクトに連携されている必要があるため、初めてアプリを作成する時から事前にプロジェクトを作成するか、作成済みのプロジェクトを選択して連携させておく必要があります。
   ![ファーウェイ 新規アプリ登録画面](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/huawei_console_app_02_eng.png)
5. アプリ作成後、追加情報を入力し、ファーウェイ AppGallery Connectコンソールで確認できる情報をNHN Cloud IAコンソールのアプリ設定に入力する必要があります。
6. このガイドの内容は、ファーウェイ AppGallery Connectに登録されたアプリの情報とNHN Cloud Gamebase間のアプリ情報を接続するための 情報だけを扱っており、<br/>より詳細なファーウェイ アプリギャラリーのアプリ登録手順については[Amazonのガイド文書](https://developer.huawei.com/consumer/en/doc/development/HMSCore-Guides/introduction-0000001050033062)を参照してください。

<a id="setting-values-required-for-linking"></a>
## 接続に必要な設定値 { #setting-values-required-for-linking }
![NHN Cloud Gamebaseアプリ設定ポップアップ](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/huawei_iap_console_ja.png)
<a id="store-app-id"></a>
### Store App ID { #store-app-id }
- AppGallery Connect > MyProject > Project Settings画面でアプリに関連付けられたプロジェクト関連情報を 確認できます。
- この画面の`App Information - Package Name`値をNHN Cloud Gamebaseコンソールアプリ設定の'ストアアプリID'に入力する必要があります。
    - この値はAppGallery Connect > MyApp > App画面でも確認できます。

![AppGallery Connectプロジェクト基本情報画面](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/huawei_console_app_06_eng.png)

<a id="huawei-app-id-app-secret"></a>
### Huawei App ID & App Secret { #huawei-app-id-app-secret }
- AppGallery Connect > MyProject > Project Settings画面でアプリに関連付けられたプロジェクト関連情報を確認できます。
- この画面の`App Information - OAuth 2.0 Client ID`値をNHN Cloud Gamebaseコンソールアプリ設定の'Huawei App ID'値に入力する必要があります。
- この画面の`App Information - OAuth 2.0 Client Secret`値をNHN Cloud Gamebaseコンソールアプリ設定の'Huawei App Secret'値に入力する必要があります。

![AppGallery Connectプロジェクト基本情報画面](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/huawei_console_app_06_eng.png)

<a id="huawei-developer-public-key"></a>
### Huawei Developer Public Key { #huawei-developer-public-key }
- AppGallery Connect > MyProject > Earn > In-App Purchases画面でIAP関連情報を確認できます。
- 初めてこの画面に移動した状態でIAPを有効にするボタンを押してIAPを有効化すると、作成された`Public Key`値を確認できます。<br/>
  この値をNHN Cloud Gamebaseコンソールアプリ設定の'Huawei Developer Public Key'値に入力する必要があります。

![AppGallery Connect IAP画面](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/huawei_console_app_05_eng.png)