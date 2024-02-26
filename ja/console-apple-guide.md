## Game > Gamebase > Store 設定ガイド > Apple 設定ガイド

> この文書、はApp Storeにリリースされたアプリの情報を[Gamebase](https://docs.nhncloud.com/ja/Game/Gamebase/ja/Overview/)コンソールに登録及び連動させる方法を説明します。
> 連動方法は**(新)領収書検証 + Notification V2**, **(旧)領収書検証 + Notification V1**の2つの方法に分かれています。

## NHN Cloud SDK iOSバージョン別の連動方式
| バージョン       | 連動方式                                                |
|-----------|----------------------------------------------------------|
| v1.8.0以上 | (新)領収書検証+Notification V2, (旧)領収書検証+Notification V1 |
| v1.7.*    | (新)領収書検証+Notification V2                              |
| v1.6.2以下 | (旧)領収書検証+Notification V1                              |

## (新)領収書検証 + Notification V2
### アプリ内購入キーの生成
> **参考** 
> [https://developer.apple.com/help/app-store-connect/configure-in-app-purchase-settings/generate-keys-for-in-app-purchases](https://developer.apple.com/help/app-store-connect/configure-in-app-purchase-settings/generate-keys-for-in-app-purchases)

1. **App Store Connect** > **ユーザーおよびアクセス** > **統合** > **キー**タブをクリック
2. **キー** > **アプリ内課金**をクリック
3. **アプリ内課金キーの生成**をクリック
4.キー名を入力し、**生成**をクリック
5. **アプリ内課金キーのダウンロード**をクリック
![[]](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/AppStore/jp/app_store_connect_01_jp_240226.png)

### Storeアプリ情報にアプリ内購入キーを入力
1. [コンソール](https://console.nhncloud.com)で組織およびプロジェクトを選択し、**Game** > **Gamebase** > **IAP** > **ストア** > **登録** またはAppを選択し、**修正**をクリック
2. Store APP ID: **App Bundle ID**を入力
3. 領収書検証およびノーティ方式: **(新)領収書検証 + Notification V2** 選択
4. ダウンロードしたアプリ内購入キー、Key ID、Issuer ID入力
![[]](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/AppStore/jp/store_info_01_jp_240226.png)

### Notification V2 URL登録
1. **App Store Connect** > **アプリ** > **アプリ選択** > **配信** > **一般** > **アプリ情報** > **App Storeサーバー通知**
2. **プロダクションサーバーURL** または **SandboxサーバーURL**編集をクリック
3. 通知バージョン: **バージョン2通知**を選択
4. サーバーURL: `https://api-iap.cloud.toast.com/callback/subscription/{APP_BUNDLE_ID}/AS/v2`を入力

## (旧)領収書検証 + Notification V1 (Deprecated予定)
- Appleサブスクリプション商品決済を使用するには、App Store Connectで**共有パスワード**の作成と**Notification V1 URL**の設定が必要です。
- 共有パスワードはIAPアプリ情報に登録します。
- Apple一般商品決済は別途設定が必要ありません。

### 共有パスワードの作成
> **参考**
> すべてのアプリのための単一のパスワードである**基本共有パスワード**または個々のアプリのための**アプリ共有パスワード**を作成できます。
> [https://developer.apple.com/help/app-store-connect/configure-in-app-purchase-settings/generate-a-shared-secret-to-verify-receipts](https://developer.apple.com/help/app-store-connect/configure-in-app-purchase-settings/generate-a-shared-secret-to-verify-receipts)

#### 基本共有パスワード
1. **App Store Connect** > **ユーザーとアクセス** > **統合** > **共有パスワード**タブをクリック
2. **作成**をクリック
![[]](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/AppStore/jp/app_store_connect_02_jp_240226.png)

#### アプリ共有パスワード
1. **App Store Connect** > **アプリ** > **アプリを選択** > **配信** > **一般** > **アプリ情報** > **アプリ共有パスワード** > **管理**をクリック
2. **作成**をクリック
![[]](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/AppStore/jp/app_store_connect_03_jp_240226.png)

### IAPアプリ情報にShared Secretを入力
1. [コンソール](https://console.nhncloud.com)で組織およびプロジェクトを選択し、**Mobile Service** > **IAP** > **App** > **追加** またはAppを選択し、**編集**をクリック
2. Store APP ID: **App Bundle ID**を入力
3. 領収書検証およびノーティ方式: **(旧)領収書検証 + Notification V1** 選択
4. Shared Secretを入力
![[]](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/AppStore/jp/store_info_02_jp_240226.png)

### Notification V1 URL登録
1. **App Store Connect** > **アプリ** > **アプリ選択** > **配信** > **一般** > **アプリ情報** > **App Storeサーバー通知**
2. **プロダクションサーバーURL** または **SandboxサーバーURL**編集をクリック
3. 通知バージョン: **バージョン1通知**を選択
4. サーバーURL: `https://api-iap.cloud.toast.com/callback/subscription/{APP_BUNDLE_ID}/AS`入力

## (旧)領収書検証 + Notification V1 → (新)領収書検証 + Notification V2 変更時の注意事項
- App運営中に変更する場合、障害が発生する可能性がありますので、必ず点検中に変更してください。
- **(新)領収書検証 + Notification V2** ガイドを参考にしてApp点検中に行ってください。
    - Product Appの変更前に、Sandbox Appで十分にテストしてから作業してください。
- 点検を終えた後,ユーザーがAppの最新バージョンを使用するように強制アップデートが必要です。
    - 最新バージョンでない場合、ユーザーがアプリを利用する際にエラーが発生する可能性があります。
