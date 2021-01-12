## Game > Gamebase > Android SDK ご利用ガイド > はじめる

## Environments

AndroidでGamebaseを利用するためのシステム環境は、次の通りです。

> [最小仕様]
>
> Android API 16 (JellyBean, 4.1)以上
> Gradle Android Plugin 2.3.0 以上 <br/>
> 開発環境:Android Studio

## Setting

* Gamebase Android SDKを使用する前に、TOAST ConsoledからアプリIDを発行する必要があります。アプリIDを発行するためには、TOAST Consoleから**(+)サービス選択**をクリックし、**Game > Gamebase**をクリックしてサービスを有効にします。
* 使用するGamebaseバージョン、使用する認証、決済、プッシュモジュールをbuild.gradleファイルで宣言してください。
	* Gamebaseの最新バージョンは[jCenter(LINK)](https://jcenter.bintray.com/com/toast/android/gamebase/gamebase-sdk/)で確認できます。
	* Gamebaseで依存しているライブラリをダウンロードするには`mavenCentral()`保存場所を追加してください。

```groovy
repositories {
    jcenter()
    mavenCentral()
    ...
}

dependencies {
    implementation fileTree(dir: 'libs', include: ['*.jar'])

    // >>> Gamebase Version
    def GAMEBASE_SDK_VERSION = 'x.x.x'

    // >>> Gamebase - Add Auth Adapter
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-facebook:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-google:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-line:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-naver:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-payco:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-twitter:$GAMEBASE_SDK_VERSION"

    // >>> Gamebase - Select Purchase Adapter
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-google:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-onestore:$GAMEBASE_SDK_VERSION"

    // >>> Gamebase - Select Push Adapter
    implementation "com.toast.android.gamebase:gamebase-adapter-push-fcm:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-push-tencent:$GAMEBASE_SDK_VERSION"
}
```

## 3rd-Party Provider SDK Guide

* [Facebook for developers](https://developers.facebook.com/docs/android)
* [Google APIs for Android](https://developers.google.com/android/guides/overview)
* [Naver for developers](https://developers.naver.com/docs/login/android/)
* [Twitter Android Developer's guide - Log in with Twitter](https://dev.twitter.com/web/sign-in/implementing)
* [Twitter Android Developer's guide - Authentication](https://developer.twitter.com/en/docs/basics/authentication/overview)
* [Line for developers](https://developers.line.biz/en/docs/android-sdk/integrate-line-login/)
* [PaycoID SDK for developers](https://developers.payco.com/guide/development/apply/android)

## API Reference

* API Referenceは、SDKの中に含まれています。

## Sample Codes

* ビルドおよび実行が可能なSample Projectは[ダウンロード](https://docs.toast.com/en/Download/)ページで配布しているGamebase Android SDKのZipファイルに含まれています。

## API Deprecate Governance

GamebaseでサポートしないAPIは、使用していないもの(deprecate)として処理します。
使用していない(deprecated) APIは、次の条件を満たす場合、事前告知を行わずに削除されることがあります。

* 5回以上のマイナーバージョンアップデート
	* Gamebaseバージョン形式 - XX.YY.ZZ
		* XX：Major
		* YY：Minor
		* ZZ：Hotfix
* 最低5ヶ月経過

