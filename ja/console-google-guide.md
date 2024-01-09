## Game > Gamebase > ストアコンソールガイド > Googleコンソールガイド

> [告知]
> 本文書はGoogle Playにリリースされたアプリの情報を[[Gamebase IAP](https://docs.toast.com/ko/Game/Gamebase/ko/oper-purchase/)コンソールに登録および連動させる方法について扱います。
> Google Playにアプリをリリースするためのより詳細なコンソール設定関連事項については、Googleが提供するGoogle Play Consoleガイドを参照してください。

## Google Cloudプロジェクトのリンク
- Google Playに登録されたアプリに関する情報を連動させるにはGoogle PlayにリンクするGoogle Cloudプロジェクトが必要です。
- Google Play Consoleの**設定** > **APIアクセス**ページに移動
    - 新規Google Cloudプロジェクトを作成するか、既存プロジェクトにリンクしてGoogle PlayとGoogle Cloudプロジェクトをリンクします。
    - すでにリンクされているGoogle Cloudプロジェクトがある場合はリンクされたGoogle Cloudプロジェクトの状態が表示されます。
    - [Google Cloud Consoleメインページ](https://console.cloud.google.com/home/dashboard)で事前にプロジェクトを作成してから既存プロジェクトにリンクすることもできます。

![Google Cloudプロジェクトのリンク](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/StoreConsoleGuide/GooglePlay/ja/google_play_console_api_access_01_ja_231220.png)

- Google PlayとGoogle Cloudの間のプロジェクト接続が正常に完了したら、Google Play Consoleの**設定** > **APIアクセス**ページにリンクされているGoogle CloudプロジェクトとAPIリストが表示されます。
- 以下のガイドに沿って設定を完了した後、画面のようにリンクされたプロジェクトの状態が表示されない場合は、Google Cloudとの接続部分を確認してください。

![Google Cloudプロジェクト接続](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/StoreConsoleGuide/GooglePlay/ja/google_play_console_api_access_02_ja_231220.png)

- 新規プロジェクト作成後、連動認証設定を行うにはOAuth同意画面設定などが必要です。
    - **OAuth同意画面設定**関連の詳細については、画面内のガイドおよび[Googleが提供するOAuth2ガイド](https://developers.google.com/identity/protocols/oauth2/)を参照してください。

![Google Cloudプロジェクト接続](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/StoreConsoleGuide/GooglePlay/ja/oauth_consent_screen_ja_231220.png)


## 2つの連動認証方式を提供

![NHN Cloud IAPアプリ設定](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/StoreConsoleGuide/GooglePlay/ja/store_info_registration_ja_231220.png)

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
      ![最高管理者モデル設定](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/StoreConsoleGuide/GooglePlay/ja/store_info_registration_supervisor_ja_231220.png)


2. [Google Cloud Console APIおよびサービス](https://console.cloud.google.com/apis/dashboard)ダッシュボードに移動
    - Google Play管理アカウントと連動するプロジェクトを選択
    - **ユーザー認証情報**メニューに移動
    - **ユーザー認証情報を作成** > **OAuthクライアントID**を選択

   ![最高管理者モデル設定](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/StoreConsoleGuide/GooglePlay/ja/google_cloud_credentials_01_ja_231220.png)

3. 新規OAuth Client基本情報の入力
    - アプリケーションタイプ：Webアプリケーションの指定
    - 名前：管理者がNHN Cloud IAP用であることを区別することができる適切な名称を指定
    - 承認されたjavascript原本：無視
    - 承認されたリダイレクトURI：追加後に`https://developers.google.com/oauthplayground`を入力

   ![最高管理者モデル設定](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/StoreConsoleGuide/GooglePlay/ja/google_cloud_credentials_02_ja_231220.png)

4. 新規OAuth Clientが作成されると、作成されたOAuth Clientの2つの情報についての案内ポップアップが表示されます。
    - クライアントID：NHN Cloud IAPアプリ情報設定画面の`Google API Client ID`の値を入力します。
    - クライアントセキュリティパスワード：NHN Cloud IAPアプリ情報設定画面の`Google API Client Secret`の値を入力します。
    - 上記の2つの情報はOAuth Client情報画面で再確認することができ、Googleが提供するOAuth Client情報JSONダウンロードファイルでも確認できます。

   ![最高管理者モデル設定](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/StoreConsoleGuide/GooglePlay/ja/google_cloud_credentials_03_ja_231220.png)

5. [Google OAuth 2 Playground](https://developers.google.com/oauthplayground)ページに移動
    - 画面右上の歯車ボタンを押すと表示される**OAuth 2.0 Configuration**ポップアップ設定でUse your own OAuth credentialsチェックボックスをチェックします。
    - OAuth 2.0 Configurationポップアップ設定のClient ID、Client Secretにステップ4で取得した情報を入力します。
    - 左側のAPI権限範囲選択画面で`Google Play Android Developer API`を選択します。
        - 左側の選択画面下の範囲入力部分に`https://www.googleapis.com/auth/androidpublisher`を入力しても構いません。
    - すべての設定値の入力と選択が完了したら左下のAuthorize APIボタンをクリックします。

   ![最高管理者モデル設定](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/StoreConsoleGuide/GooglePlay/ja/google_oauth2.0_playground_01_ja_231220.png)

6. Authorize APIボタンをクリックするとGoogle OAuth 2 PlaygroundページがStep 2に変わり、追加情報が表示されます。
    - Refresh Token：NHN Cloud IAPアプリ情報設定画面の`Refresh Token For Google Oauth`の値を入力します。

   ![最高管理者モデル設定](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/StoreConsoleGuide/GooglePlay/ja/google_oauth2.0_playground_02_ja_231220.png)

## サービスアカウント認証モデル
- 最高管理者アカウントが委任した権限を持つGoogle内サービスアカウントの認証を代行するモデルであり、 2022年4月NHN Cloud IAPに新規追加された認証サポートモデルです。
- 権限の範囲を委任されるサービスアカウントの作成および管理は最高管理者(Googleの実際の管理アカウント)がGoogleコンソールで行う必要があります。
- サービスアカウントは最高管理者によって最高管理者に準ずる範囲の権限を受任することもでき、最高管理者が権限を持つ特定アプリだけに限定された範囲の権限を受任することもできます。
    - 委任権限範囲の設定戦略は、お客様の意図した特性に合わせて設定してください。
- 最高管理者認証モデルが持つ権限の範囲に負担を感じる場合は、Googleコンソール内でサービスアカウント作成後にこの認証モデルを使用してください。 ([Googleガイド](https://developers.google.com/identity/protocols/oauth2/service-account))

1. NHN Cloud IAPサービスアカウント認証モデル特化入力情報
    - `サービスアカウント連動情報`：サービスアカウント認証モデルガイドのステップ5を参照 
      ![サービスアカウントモデル設定](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/StoreConsoleGuide/GooglePlay/ja/store_info_registration_service_account_ja_231220.png)

2. [Google Cloud Console](https://console.cloud.google.com/apis/dashboard) APIおよびサービスページに移動
    - Google Play管理アカウントと連携するプロジェクトを選択
    - **ユーザー認証情報**メニューに移動
    - **ユーザー認証情報を作成** > **サービスアカウント**を選択

   ![サービスアカウントモデル設定](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/StoreConsoleGuide/GooglePlay/ja/google_cloud_service_accounts_01_ja_231220.png)

3. 新規サービスアカウントの基本情報の入力
    - 管理しやすい形の基本情報を入力した後、`作成して続行`をクリックします。
    - サービスアカウントへのアクセス権限付与で役割を`所有者`と選択します。
    - 追加選択事項で作成するサービスアカウントの管理者メールを入力すると、その管理者にサービスアカウント設定の権限が付与されます。
        - Google Cloud Console内に存在しない管理者メールアドレスの場合は、入力したメールアドレスに管理者招待メールが送信されます。

   ![サービスアカウントモデル設定](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/StoreConsoleGuide/GooglePlay/ja/google_cloud_service_accounts_02_ja_231220.png)

4. サービスアカウントへの権限委任
    - **Google Play Console** > **APIアクセス**メニューに移動
    - Google Cloud Consoleで作成したサービスアカウントが画面下部のサービスアカウントリストに表示されます。作成したサービスアカウント右側の**権限付与**リンクをクリックします。
    - 権限の範囲は最高管理者アカウントが持つアプリ全体またはアプリ個別単位で複数指定することができ、範囲内で権限の種類を指定できます。
    - 範囲は顧客の意図に合わせて設定しますが、権限の種類のうち`アプリ情報表示(読み取り専用)`、`財務データ表示`、`注文および購読管理`の権限は必ず委任設定する必要があります。
    - 委任設定された権限は反映されるまで数日かかる場合があります。委任権限が反映されるまで最長7日程度かかった事例がありますのでご参考ください。

   ![サービスアカウントモデル設定](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/StoreConsoleGuide/GooglePlay/ja/google_play_console_api_access_03_ja_231220.png)

5. APIおよびサービスページの新しく作成されたサービスアカウントリストのうち、作成した**サービスアカウント**の詳細情報画面に移動
    - キータブに移動して、キー追加 > 新しいキーを作成
    - 新規ポップアップで`JSON`形式を選択して作成をクリック
    - キーの作成が完了すると、自動的にJSON形式のファイルをGoogleからダウンロードします。
    - そのファイルをWindowsのメモ帳などの純粋なテキストエディタで開き、すべての内容をコピーした後、NHN Cloud IAPアプリ情報設定画面の`サービスアカウント連動情報`に入力する必要があります。
    - ダウンロードしたファイルは最初のダウンロード後に、再ダウンロードすることはできませんので、大切に保管して入力してください。
    - ステップ4で言及したように、Google内部でまだ権限が反映されていない場合は権限なし登録エラーが発生することがありますので、このような場合は時間をおいて再試行してください。

   ![サービスアカウントモデル設定](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/StoreConsoleGuide/GooglePlay/ja/google_cloud_service_accounts_03_ja_231220.png)


## 共通入力情報
### Package Name
- Google Play Consoleから登録したアプリをビルド時に指定したpackageNameがあります。この値はGoogle内でアプリの固有指定子として活用されます。
- この値をNHN Cloud IAPアプリ設定の`Store App ID`に入力します。

### InAppPurchase License Public Key
- **Google Play Console** > 該当アプリダッシュボード > 収入創出メニューに移動
- 画面下部にBase64エンコードされたライセンス公開鍵が表記されます。この値をNHN Cloud IAPアプリ設定の`Google In App Purchase License Key`に入力します。

![IAPライセンスキー](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/StoreConsoleGuide/GooglePlay/ja/google_play_console_monetization_setup_01_ja_231220.png)

### マーケット検証の省略
- NHN Cloud IAPが提供するGoogle決済の検証は大きく2つのステップに分けられます。2つのステップのうち**ステップ2を省略するかどうかを設定**します。
- 省略は推奨仕様ではなく、まれに発生するGoogleサーバー障害状況に対応する一時的な決済承認状態を維持できます。 (デフォルト値は`NO`)
    - 省略しても、流入する決済検証リクエストが無条件正常決済として処理されるわけではありません。
    - 購読決済の検証は省略オプションが適用される範囲ではありません。

#### Google検証ステップ
| ステップ | 説明        |
| --------------- | ----------------------------- |
| ステップ1 | 検証がリクエストされた決済情報が改ざんされかたどうかを確認      |
| ステップ2 | 検証がリクエストされた決済情報のGoogleサーバー側の最新ヘルスチェックおよび再検証 |


## Googleシステム内のリアルタイム購読情報イベント配信設定
- Googleが提供する購読購入のリアルタイム状態配信イベントをNHN Cloud IAPサーバーが受け取って処理するように設定できます。
- Google Cloud Platformに決済プロフィールを登録し(クレジットカード必要)、使用状態に変更する必要があります。
- この配信イベント設定をGoogle内で行わない場合、購読決済の更新情報がユーザーのアプリクライアント実行アクションに基づいて反映されるため、購読決済を使用する場合は設定することを推奨します。
- 過去には購読リアルタイム配信イベントのプッシュ受信のために受信するアドレスのドメイン検証をGoogle Webマスターツールで行う必要がありましたが、現在はドメイン検証を要求しません。

1. [Google Cloud Pub/Sub Console](https://console.cloud.google.com/cloudpubsub)に移動
    - Google Playアプリにリンクされたプロジェクトを正しく選択する必要があります。
    - **トピック(Topic)**メニューから新しいトピックを作成します。
    - トピックIDは管理しやすい任意の名称を使用してください。

   ![購読情報配信設定](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/StoreConsoleGuide/GooglePlay/ja/google_cloud_pub_sub_01_ja_231220.png)
   ![購読情報配信設定](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/StoreConsoleGuide/GooglePlay/ja/google_cloud_pub_sub_02_ja_231220.png)

2. 作成したトピックに**メインメンバーを追加**を行います。
    - 詳細メンバー情報`google-play-developer-notifications@system.gserviceaccount.com`を入力
    - 役割`掲示/購読` > `掲示/購読掲示者`を選択

   ![購読情報配信設定](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/StoreConsoleGuide/GooglePlay/ja/google_cloud_pub_sub_03_ja_231220.png)
   ![購読情報配信設定](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/StoreConsoleGuide/GooglePlay/ja/google_cloud_pub_sub_04_ja_231220.png)

3. **購読**メニューに移動し、IAPサーバーに伝える購読を作成します。
    - 購読IDは管理しやすい値を入力
    - Cloud Pub/Subトピックを選択：前に作成したトピックを選択
    - 転送タイプ：プッシュ選択
    - エンドポイントURL：`https://api-iap.cloud.toast.com/callback/subscription/{YOUR_PACKAGE_NAME}/GG`入力
    - `{YOUR_PACKAGE}`部分にはNHN Cloud App設定のStoreAppIDに入力した値と同じアプリビルド時に使用したパッケージ名を入力します。

      ![購読情報配信設定](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/StoreConsoleGuide/GooglePlay/ja/google_cloud_pub_sub_05_ja_231220.png)
   ![購読情報配信設定](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/StoreConsoleGuide/GooglePlay/ja/google_cloud_pub_sub_06_ja_231220.png)


4. テスト用のGamebaseサンドボックス環境を使用する場合は、ステップ3の購読をサンドボックス用に作成する必要があります。
    - GamebaseサンドボックスエンドポイントURL入力：`https://sandbox-api-iap.cloud.toast.com/callback/subscription/{YOUR_PACKAGE_NAME}/GG`
5. [Google Play Console](https://play.google.com/console)のアプリダッシュボードに移動
    - **収入創出設定設定** > **Google Play決済**画面のリアルタイム開発者通知トピック名にステップ1で作成したトピックの全体名称を入力

   ![購読情報配信設定](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/StoreConsoleGuide/GooglePlay/ja/google_play_console_monetization_setup_02_ja_231220.png)
