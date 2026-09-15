<!-- pre-align:aligned sig=83150409a851 -->

<a id="game-gamebase-store-console-guide-amazon-appstore-console-guide"></a>
## Game > Gamebase > Store 設定ガイド > Amazon Appstoreコンソールガイド { #game-gamebase-store-console-guide-amazon-appstore-console-guide }

<a id="amazon-developer-console"></a>
## Amazon Developer Console { #amazon-developer-console }
1. [Amazon開発者コンソール](https://developer.amazon.com/)にアカウントを登録し、Amazon AppStore管理メニューでアプリを作成します。
   ![Amazon開発者コンソール](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/amazon_developer_console_eng.png)
2. NHN Cloud IAPは、AndroidプラットフォームのAmazon AppStoreアプリのみを公式サポートします。Androidプラットフォームを選択し、アプリ名を入力たら`Create app`ボタンをクリックしてアプリを作成します。
   ![アプリプラットフォーム選択](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/amazon_appmenu_0_eng.png)
3. アプリを作成すると、次のように作成されたアプリのリストを確認できます。
   ![AppStore開発者コンソールのアプリリスト](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/amazon_appmenu_1_eng.png)
4. アプリ作成後に追加情報を入力し、開発者コンソールで提供する情報をNHN Cloud IAPコンソールのアプリ設定に入力する必要があります。
5. このガイドの内容はAmazon AppStoreに登録されたアプリの情報とNHN Cloud IAP間のアプリ情報を接続するためのガイドだけを扱っており、<br/>より詳細なAmazon AppStoreのアプリ登録手順については[Amazonのガイド文書](https://developer.amazon.com/apps-and-games/documentation)を参照してください。

<a id="setting-values-required-for-linking"></a>
## 接続に必要な設定値 { #setting-values-required-for-linking }
![NHN Cloud Gamebaseストア設定画面](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/amazon_iap_console_ja.png)
<a id="store-app-id"></a>
### Store App ID { #store-app-id }
- [アプリリスト](https://developer.amazon.com/apps-and-games/console/apps/list.html)画面で作成されたアプリの名前をクリックすると詳細設定メニューに移動します。
- 詳細設定メニューで、編集をタップして 'App SKU'を入力する必要があります。App SKUの値を[NHN Cloud Gamebaseコンソール>ストア]のストア情報登録画面の'ストアアプリID'に入力する必要があります。
  ![Amazon開発者コンソールのアプリ詳細設定画面](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/amazon_appmenu_2_eng.png)


<a id="amazon-shared-key"></a>
### Amazon Shared Key { #amazon-shared-key }
- Amazon開発者コンソールの[Settings -> Identityメニュー](https://developer.amazon.com/settings/console/sdk/shared-key)に移動すると、以下のような画面で共有キーを確認できます。
  ![Amazon開発者コンソールのIdentity画面](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/amazon_appmenu_3_eng.png)
- この値をNHN Cloud Gamebaseコンソールアプリ設定の'Amazon Shared Key'項目に入力する必要があります。