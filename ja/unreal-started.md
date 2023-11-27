## Game > Gamebase > Unreal SDK使用ガイド > 開始する

Gamebase Unreal SDKの使用環境および初期設定の説明を行います。

## Environments

> [参考] 
>
> Unrealサポートバージョン
>
> * UE 4.26 ~ UE 5.0
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
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNREAL_WINDOWS

#### Dependencies

* [Gamebase Android SDK - Dependencies](./aos-started/#dependencies)
* [Gamebase iOS SDK - Dependencies](./ios-started/#setting)

## Installation

1. Gamebase Unreal SDKをダウンロードして、プロジェクトパスに`Plugins`フォルダを作成し、ダウンロードしたSDKを追加します。
2. Unrealエディタで`Settings > Plugins`ウィンドウを開き、`Project > Gamebase > Gamebase Plugin`プラグインを探して有効にします。

* [Download Gamebase Unreal SDK](/Download/#game-gamebase)

### Android Settings

1. エディタのメニュー **Edit > Project Settings**を選択します。
2. Project SettingsウィンドウでPluginカテゴリーから**Gamebase - Android**を選択します。

![Unreal Project Settings - Android](https://static.toastoven.net/prod_gamebase/UnrealDevelopersGuide/unreal-developers-guide-started-android-setttings-2.57.0.png)

* Authentication
    * 使用するIdPを有効にします。
    * Hangame IdPを使用する時は、サポートへお問い合わせください。
* Purchase
    * 使用するストアを選択します。
    * ONE Store
        * View Option - 全体決済画面(Full)とポップアップ決済画面(Popup)のいずれかを選択します。
* Push
    * 使用したいプッシュサービスを有効にします。


#### Google Play認証および決済ができない問題

Google Playサービスに認証と決済を行うには、Distributionの設定が必要です。
詳細な内容は、以下の文書を参照してください。 

* [Signing Projects for Release](https://docs.unrealengine.com/en-US/Platforms/Mobile/Android/DistributionSigning/index.html)

#### AndroidX適用

* Gamebase Android SDK 2.25.0からAndroidXが導入され、[UPL(Unreal Plugin Language)](https://docs.unrealengine.com/4.27/en-US/SharingAndReleasing/Mobile/UnrealPluginLanguage/)ファイルの下に設定を追加する必要があります。

```xml
<gradleProperties>    
  <insert>
    android.useAndroidX=true      
    android.enableJetifier=true    
  </insert>  
</gradleProperties>
```

#### multidex適用

* Gamebase Unreal SDK 2.26.0からGamebase内部で設定したmultidex関連内容が削除されたため、[UPL(Unreal Plugin Language)](https://docs.unrealengine.com/4.27/en-US/SharingAndReleasing/Mobile/UnrealPluginLanguage/)ファイルに以下の設定を追加する必要があります。

```xml
<buildGradleAdditions>
  <insert>
  android {
    defaultConfig {
      multiDexEnabled true
    }
  }
  </insert>
</buildGradleAdditions>

<androidManifestUpdates>
    <addAttribute tag="application" name="android:name" value="androidx.multidex.MultiDexApplication"/>
</androidManifestUpdates>
```

### iOS Settings

Gamebase SDK for Unrealを使用するには`UE4 Githubソースコード`を使用する必要があり、Epic gamesに会員登録した後、Githubアカウントを接続するとUnrealEngine repositoryが表示されます。
See below for relevant guides. 

* [Downloading Unreal Engine Source Code](https://docs.unrealengine.com/5.0/en-US/downloading-unreal-engine-source-code/)
* [Getting up and running](https://github.com/EpicGames/UnrealEngine#getting-up-and-running)

>`!重要`
> このプロセスを無視すると、以下のガイドリンクが正常に動作しなかったりGamebase SDK for Unrealを使用できません。

#### Project Settings

1. エディタのメニュー**Edit > Project Settings**を選択します。
2. Project SettingsウィンドウでPluginカテゴリーから**Gamebase - iOS**を選択します。

![Unreal Project Settings - iOS](https://static.toastoven.net/prod_gamebase/UnrealDevelopersGuide/unreal-developers-guide-started-ios-setttings-2.57.0.png)

* Path
    * Xcode Path：Xcodeのパスを入力します。 (デフォルト値： /Applications/Xcode.app)

* Authentication
    * 使用したいIdPを有効にします。
* Purchase
    * 使用したいストアを選択します。
* Push
    * 使用したいプッシュサービスを有効にします。

#### Sign in with Apple

Sign in with Apple機能を使用するにはentitlementにcom.apple.developer.applesigninキー値が追加されている必要があります。

* [Sign in with Apple Entitlement](https://developer.apple.com/documentation/bundleresources/entitlements/com_apple_developer_applesignin)

該当のキー値を追加せずにGamebsae AppleIdログインを進行する場合、下記のようなエラーが発生します。

```
Authorization failed: Error Domain=AKAuthenticationError Code=-7026 "(null)"
```

UE4(4.24.3)は該当機能をサポートしないため、[Engine/Source/Programs/UnrealBuildTool/Platform/IOS/IOSExports.cs](https://github.com/EpicGames/UnrealEngine/blob/release/Engine/Source/Programs/UnrealBuildTool/Platform/IOS/IOSExports.cs)ファイルの上に上記コードを追加する必要があります。

```cs
// AS-IS
if (bRemoteNotificationsSupported)
{
    Text.AppendLine("\t<key>aps-environment</key>");
    Text.AppendLine(string.Format("\t<string>{0}</string>", bForDistribution ? "production" : "development"));
}

// TO-BE
if (bRemoteNotificationsSupported)
{
    Text.AppendLine("\t<key>aps-environment</key>");
    Text.AppendLine(string.Format("\t<string>{0}</string>", bForDistribution ? "production" : "development"));
    Text.AppendLine("\t<key>com.apple.developer.applesignin</key>");
    Text.AppendLine("\t<array>");
    Text.AppendLine("\t\t<string>Default</string>");
    Text.AppendLine("\t</array>");
}
```

#### Facebook SDK

Facebook IdPを使用するには[Engine/Source/Programs/UnrealBuildTool/Platform/IOS/IOSToolChain.cs](https://github.com/EpicGames/UnrealEngine/blob/4.24/Engine/Source/Programs/UnrealBuildTool/Platform/IOS/IOSToolChain.cs)ファイルで以下のコードを追加する必要があります。

```cs
// need to tell where to load Framework dylibs
Result += " -rpath /usr/lib/swift";                 // 追加コード
Result += " -rpath @executable_path/Frameworks";
```

#### Remote Notification

1. Gamebase Remote Notification機能を使用するには、Project Settings > Platforms > iOSページでEnable Remote Notifications Support機能を有効化する必要があります。(Githubソースからのみ可能)
2. Foregroundプッシュ通知を受け取るには[Engine/Source/Runtime/ApplicationCore/Private/IOS/IOSAppDelegate.cpp](https://github.com/EpicGames/UnrealEngine/blob/4.26/Engine/Source/Runtime/ApplicationCore/Private/IOS/IOSAppDelegate.cpp)ファイルから以下のコードを削除するか

        - (void)userNotificationCenter:(UNUserNotificationCenter *)center
            willPresentNotification:(UNNotification *)notification
                withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler
        {
            // Received notification while app is in the foreground
            HandleReceivedNotification(notification);
        
            completionHandler(UNNotificationPresentationOptionNone);
        }

   次のように修正する必要があります。
        
            // AS-IS
            completionHandler(UNNotificationPresentationOptionNone);
            
            // TO-BE
            completionHandler(UNNotificationPresentationOptionAlert);

#### Rich Push Notification

次のようなイシューによりRich Push Notification機能を使用できません。

* Unrealはプロジェクトに[Notification Service Extension](https://developer.apple.com/documentation/usernotifications/unnotificationserviceextension?language=objc)を追加できる方法を提供しません。
    * [NHN Cloud Push Notification Service Extension作成](https://docs.toast.com/en/TOAST/en/toast-sdk/push-ios/#notification-service-extension)

#### iOS SDKのWarningメッセージによるUnrealビルドエラー

iOS SDKで発生するWarningメッセージがUnrealビルド時、エラーに変換されてビルドに失敗する現象が発生する場合は[Engine/Source/Programs/UnrealBuildTool/Platform/IOS/IOSToolChain.cs](https://github.com/EpicGames/UnrealEngine/blob/release/Engine/Source/Programs/UnrealBuildTool/Platform/IOS/IOSToolChain.cs)ファイルのclangコンパイルオプションコードをコメント処理してください。

```cs
// Result += " -Wall -Werror";
```

#### PLCrashReporter

UE4で使用中のPLCrashReporterが`arm64e` architectureをサポートしておらず、該当architectureを使用するデバイスでメモリアドレス値を取得できないイシューがあります。

NHN Cloud Log & Crash Searchでクラッシュ分析を行うゲーム開発会社は、次のガイドを参照してUE4内部PLCrashReporterを修正する必要があります。

1. GamebaseSDK-Unreal/Source/Gamebase/ThirdParty/IOS/GamebaseSDK-iOS/externals/plcrashreporter.zipファイルを解凍します。
2. UE4内部PLCrashReporterのaファイルとheaderファイルを解凍したファイルと交換します。
    * Engine/Source/ThirdParty/PLCrashReporter/plcrashreporter-master-xxxxxxx

### Windows Settings

1. エディタのメニュー **Edit > Project Settings**を選択します。
2. Project SettingsウィンドウでPluginカテゴリーから**Gamebase - Windows**を選択します。

![Unreal Project Settings - Windows](https://static.toastoven.net/prod_gamebase/UnrealDevelopersGuide/unreal-developers-guide-started-windows-setttings-2.57.0.png)

* Authentication
    * 使用する IdP を有効にします。
* Purchase
    * 使用したいストアを選択します。
    * Epic Store
        * EOSサービス情報を各項目に合わせて入力します。

#### Epic Storeサービス

* UE 4.27以降のバージョンでサポートされ、エンジン内部にEOSSDKモジュールが使用されています。
* Epic Storeを使用するためには、EOSSDKを使用してログインする必要があります。
* Gamebase で使用する EOS のバージョンは 1.15.5.0 で、エンジンパス `Engine\Source\ThirdParty\EOSSDK\SDK` に該当バージョンをインストールしてアップグレードする必要があります。
    * [参考: EOS SDKアップグレードガイド](https://docs.unrealengine.com/5.2/en/upgrading-the-eos-sdk-in-unreal-engine/)
* ゲーム起動時にEOS Handleの設定が必要です。
    * エンジンに含まれているOnline Subsystem EOSを使用する場合、下記のコードのように設定できます。

            ```cpp 
            #include "OnlineSubsystemEOS.h" 
            #include "IEOSSDKManager.h"
            #include "GamebaseStandalonePurchaseEpicAdapterModule.h"

            void UGamebasePurchaseEpicSupportTestCase::SetEosPlatformInstance()
            {
                IOnlineSubsystem* Subsystem = Online::GetSubsystem(GetWorld());

                if (const FOnlineSubsystemEOS* EosSubsystem = static_cast<FOnlineSubsystemEOS*>(Subsystem))
                {
                    EOS_HPlatform PlatformHandle = *EosSubsystem->EOSPlatformHandle;
                    FGamebaseStandalonePurchaseEpicAdapterModule::SetEosPlatformInstance(*Handle);
                }
            }
            ```

        > `OnlineSubsystemEOS.h`ヘッダーを含めるとビルドエラーが発生するので、OnlineSubsystemEOSプラグイン内のPrivateフォルダの中のHeaderをPublicに移動する必要があります。(参考: [EOS関連お問い合わせ](https://eoshelp.epicgames.com/s/question/0D54z00007QIJjhCAH/cant-call-get-voice-chat-user-interface-from-game-instance-using-the-eos-plugin-and-eos-voice-plugins-on-unreal-engine4?language=en_US))
        > - SocketSubsystemEOS.h 
        > - EOSSettings.h
        > - EOSHelpers.h
        > - [Platform]/[Platform]EOSHelpers.h


## API Deprecate Governance

GamebaseでサポートしないAPIはDeprecate処理します。
DeprecatedされたAPIは、次の条件を満たすと、事前告知を行わずに削除される場合があります。

* 5回以上のマイナーバージョンアップデート
	* Gamebase Version Format - XX.YY.ZZ
		* XX : Major
		* YY : Minor
		* ZZ : Hotfix

* 5か月経過
