## Game > Gamebase > Unreal SDK使用ガイド > 開始する

Gamebase Unreal SDKの使用環境および初期設定の説明を行います。

### Environments

> [参考] 
>
> Unrealサポートバージョン
>
> * UE 4.24
> * 下位バージョンのUnrealのサポートが必要な場合は[サポート](https://toast.com/support/inquiry)へお問い合わせください。

#### Supported Platforms

* iOS
* Android
* Editor
    * 一部機能のみサポートします。

選択したプラットフォームでサポートしないGamebase APIを呼び出す時は、下記のエラーがコールバックで返り、コールバックがない場合はWarningログが出力されます。

* GamebaseErrorCode::NOT_SUPPORTED
* GamebaseErrorCode::NOT_SUPPORTED_IOS
* GamebaseErrorCode::NOT_SUPPORTED_ANDROID
* GamebaseErrorCode::NOT_SUPPORTED_UE4_STANDALONE
* GamebaseErrorCode::NOT_SUPPORTED_UE4_EDITOR

各APIでサポートするプラットフォームは、下記のアイコンで区別します。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#B60205; font-size: 10pt">■</span> UNREAL_EDITOR

## Installation

1. Gamebase Unreal SDKをダウンロードして、プロジェクトパスに`Plugins`フォルダを作成し、ダウンロードしたSDKを追加します。
2. Unrealエディタで`Settings > Plugins`ウィンドウを開き、`Project > Gamebase > Gamebase Plugin`プラグインを探して有効にします。

* [Download Gamebase Unreal SDK](/Download/#game-gamebase)

### Android Settings

* Plugins/Gamebase/Source/Gamebase/Gamebase_Android_UPL.xml
    * 使用する認証、決済、プッシュモジュールgradle依存性を追加します。

```xml
<buildGradleAdditions>
    <insert>
        dependencies {
            // >>> Gamebase Version
            def GAMEBASE_SDK_VERSION = 'x.x.x'

            implementation "com.toast.android.gamebase:gamebase-sdk:$GAMEBASE_SDK_VERSION"
            implementation "com.toast.android.gamebase:gamebase-sdk-base:$GAMEBASE_SDK_VERSION"

            // >>> Gamebase - Add Auth Adapter (使用するIdPに合わせてgamebase-adapter-authモジュールをgradle依存性に追加します。)
            //implementation "com.toast.android.gamebase:gamebase-adapter-auth-facebook:$GAMEBASE_SDK_VERSION"
            //implementation "com.toast.android.gamebase:gamebase-adapter-auth-google:$GAMEBASE_SDK_VERSION"
            //implementation "com.toast.android.gamebase:gamebase-adapter-auth-line:$GAMEBASE_SDK_VERSION"
            //implementation "com.toast.android.gamebase:gamebase-adapter-auth-naver:$GAMEBASE_SDK_VERSION"
            //implementation "com.toast.android.gamebase:gamebase-adapter-auth-payco:$GAMEBASE_SDK_VERSION"
            //implementation "com.toast.android.gamebase:gamebase-adapter-auth-twitter:$GAMEBASE_SDK_VERSION"

            // >>> Gamebase - Select Purchase Adapter (使用するマーケットのgamebase-adapter-purchaseモジュールをgradle依存性に追加します。)
            //implementation "com.toast.android.gamebase:gamebase-adapter-purchase-google:$GAMEBASE_SDK_VERSION"
            //implementation "com.toast.android.gamebase:gamebase-adapter-purchase-onestore:$GAMEBASE_SDK_VERSION"

            // >>> Gamebase - Select Push Adapter (使用するPushのgamebase-adapter-purchaseモジュールをgradle依存性に追加します。)
            //implementation "com.toast.android.gamebase:gamebase-adapter-push-fcm:$GAMEBASE_SDK_VERSION"
            //implementation "com.toast.android.gamebase:gamebase-adapter-push-tencent:$GAMEBASE_SDK_VERSION"

            // Add the TOAST Crash Reporter for NDK dependency
            implementation 'com.toast.android:toast-crash-reporter-ndk:0.21.0'
        }
    </insert>
</buildGradleAdditions>
```

#### Google Play認証および決済ができない問題

Google Playサービスで認証と決済を進行するには、Distribution設定が必要です。
詳細な内容は、下記の文書を参照してください。

* [Signing Projects for Release](https://docs.unrealengine.com/en-US/Platforms/Mobile/Android/DistributionSigning/index.html)

### iOS Settings

Gamebase SDK for Unrealを使用するにはUE4ソースコードをダウンロードして使用する必要があります。
関連ガイドは、下記の文書を参照してください。

* [Downloading Unreal Engine Source Code](https://docs.unrealengine.com/en-US/GettingStarted/DownloadingUnrealEngine/index.html)
* [Getting up and running](https://github.com/EpicGames/UnrealEngine#getting-up-and-running)

#### Sign in with Apple

Sign in with Apple機能を使用するにはentitlementにcom.apple.developer.applesigninキー値が追加されている必要があります。

* [Sign in with Apple Entitlement](https://developer.apple.com/documentation/bundleresources/entitlements/com_apple_developer_applesignin)

該当のキー値を追加せずにGamebsae AppleIdログインを進行する場合、下記のようなエラーが発生します。

```
Authorization failed: Error Domain=AKAuthenticationError Code=-7026 "(null)"

```

UE4(4.24.3)は該当機能をサポートしないため、[Engine/Source/Programs/UnrealBuildTool/Platform/IOS/IOSExports.cs](https://github.com/EpicGames/UnrealEngine/blob/release/Engine/Source/Programs/UnrealBuildTool/Platform/IOS/IOSExports.cs)ファイルの296行目の上に上記コードを追加する必要があります。

```cs
Text.AppendLine("\t<key>com.apple.developer.applesignin</key>");
Text.AppendLine("\t<array>");
Text.AppendLine("\t\t<string>Default</string>");
Text.AppendLine("\t</array>");
```

#### Remote Notification

Gamebase Push機能を使用するには、Project Settings > Platforms > iOSページでEnable Remote Notifications Support機能を有効化する必要があります。(Githubソースからのみ可能)

#### iOS SDKのWarningメッセージによるUnrealビルドエラー

iOS SDKで発生するWarningメッセージがUnrealビルド時、エラーに変換されてビルドに失敗する現象が発生する場合は[Engine/Source/Programs/UnrealBuildTool/Platform/IOS/IOSToolChain.cs](https://github.com/EpicGames/UnrealEngine/blob/release/Engine/Source/Programs/UnrealBuildTool/Platform/IOS/IOSToolChain.cs)ファイルの269行目にあるclangコンパイルオプションコードをコメント処理してください。

```cs
// Result += " -Wall -Werror";
```

## API Deprecate Governance

GamebaseでサポートしないAPIはDeprecate処理します。
DeprecatedされたAPIは、次の条件を満たすと、事前告知を行わずに削除される場合があります。

* 5回以上のマイナーバージョンアップデート
	* Gamebase Version Format - XX.YY.ZZ
		* XX : Major
		* YY : Minor
		* ZZ : Hotfix

* 5か月経過

