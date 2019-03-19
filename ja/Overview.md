## Game > Gamebase > 概要

ゲームプラットフォーム企業NHNの10年のノウハウを詰め込んだGamebaseを自信を持って推薦します。 
Gamebase SDKを適用すれば、ゲームに必要な共通サービスを簡単に利用できます。 

![Gamebase_summary](http://static.toastoven.net/prod_gamebase/Overview/Gamebase_overview_01_201903_jp.png)

## Key Features

### Authentication

Gamebaseは、様々なIdP(identity provider)のアカウントを利用したID・パスワードベースのOAuthログインと端末のUUIDを利用したゲストログインに対応しています。Gamebaseの認証は、独自の会員システムを構築せずに、外部IdPが提供する会員情報を利用して認証サービスを提供するサービスです。独自の会員システムがないということは、ユーザーのID・パスワードをGamebaseの内部に保存しないということを意味します。

* **様々な認証方式を一つのインターフェースで提供します。** 
  一つのインターフェースでAPIを提供することで、より迅速かつ手軽に外部IdPの追加開発ができるため、開発コストが削減されます。開発者は、複雑な認証フローやリーガルイシュー、ポリシーイシューなどを考えずに、簡単に認証機能を設計することができます。

* **様々な外部IdP認証を提供します。** 
  提供する外部認証は引き続きアップデートされる予定で、ゲームで使用したい認証方法がある場合は、[カスタマーセンター](https://toast.com/support/inquiry)までご連絡ください。

次は、Gamebaseで提供している外部認証リストです。

| 外部認証            | Android | iOS | Windows(based Unity) | Web(based JavaScript)    |
| ----------------- | ------------ | ------------ | ------------ | ------------ |
| Facebook          | O | O | O | O |
| Apple Game Center | O | | | |
| Google            | O | O | O | O |
| PAYCO             | O | O | O | O |
| NAVER             | O | O | O | O |
| Twitter			| O | O | |  |
| LINE				| O | O |  |  |

* **ゲストログインを提供します。**
  ゲストログインを利用すれば、ユーザーは何も入力しなくてもすぐにゲームにログインして簡単にゲームを始めることができます。ゲストログインをするだけでもGamebaseのユーザーIDが発行されるため、ゲームはOAuthログインユーザーかゲストログインユーザーかに関係なく同じようにユーザーのゲームデータを管理することができます。
  
* **独立した会員識別子を提供します。**
  初めてログインすると、GamebaseのユーザーIDが自動で作成され、ゲームではユーザーを区別する識別子として使用できます。ユーザーIDは、認証方式に関係なくすべてのユーザーに発行され、IdPに従属していないため、どのIdPを通してログインした場合でもゲーム内では同じ方式でユーザーを処理することができます。
  
* **ログアウト及びゲーム退会機能を提供します。**
  ログアウトした後に他の認証方式を選択してもう一度ログインすることができ、ゲームを退会すると、ユーザーのユーザーID及び関連するすべての情報がGamebaseから削除されます。
  
* **一人のゲームユーザーが複数の外部IdPを同時に使用することができるように、マッピング(mapping)機能を提供します。**
  例えば、Facebook認証を使用してゲームを利用しているユーザーがGoogle認証でも同じユーザーIDを使用することができるよう、マッピング機能を提供します。一つのユーザーIDにFacebookとGoogleの認証をマッピングすれば、ゲームユーザーは、あるデバイスではFacebook、他のデバイスではGoogleで認証してゲームをすることができます。

#### Reference

* [Android SDK ご利用ガイド > 認証](./aos-authentication)
* [iOS SDK ご利用ガイド > 認証](./ios-authentication)
* [Unity SDK ご利用ガイド > 認証](./unity-authentication)


### Gamebase Analytics

Gamebase SDKを適用すれば、売上、利用者、ゲームバランシング指標を無料で提供します。 
ゲームで発生する売上、同時接続者、利用者、レベル、アイテム販売など、ゲーム事業と運営に必要な指標サービスを提供します。
すぐに適用して、サービスに積極的に活用してみてください。
![Gamebase_analytics](http://static.toastoven.net/prod_gamebase/Overview/Gamebase_overview_02_201903_jp.png)

#### Reference

* [Console ご利用ガイド > Analytics](./oper-analytics) 
* 
### Launching

サービス中のゲームアプリは、最初に始めるとき様々な情報が必要です。Gamebaseでは、ゲームアプリを起動する初期段階において運営に必要なデータをゲームアプリに提供しており、これをLaunchingと呼んでいます。
起動情報は、Gamebase Consoleからリアルタイムで設定することができ、SDKを初期化したり起動状態を変更する際にゲームから確認することができます。

Gamebaseで提供される起動情報は、次の通りです。

* アプリステータス情報
	* ゲームクライアントのアップデート必要有無、ダウンロードURL
	* メンテナンス情報
* 緊急のお知らせ情報
* 認証情報
* ゲームアプリ内URLリスト

#### Reference

* [Android SDK ご利用ガイド > 初期化 > Launching Status](./aos-initialization/#launching-status)
* [iOS SDK ご利用ガイド > 初期化 > Launching Status](./ios-initialization/#launching-status)
* [Unity SDK ご利用ガイド > 初期化 > Launching Information](./unity-initialization/#launching-information)
* [Console ご利用ガイド > アプリ](./oper-app)：アプリ、クライアントステータス及びインストールURLの設定
* [Console ご利用ガイド > 運営](./oper-operation)：メンテナンス、お知らせ登録


### For Global

Gamebaseは、基本的にゲームのグローバルオープンに対応しており、グローバル環境におけるゲーム運営をサポートするため、次のような機能を提供します。

* **ゲームユーザーに表示されるメッセージは、すべて多国語処理が可能です。**
	* ゲームユーザーに表示されるメッセージをConsoleで入力する際に多国語で入力するようにし、ユーザーのデバイス言語の設定に合わせて言語を表示します。Consoleで韓国語、英語、日本語を入力すると、韓国語のデバイスを使用するユーザーには、韓国語のメッセージが表示されます。
* **国家フィルタリング機能を提供します。**
	* 運営中に特定国家のゲームユーザーにのみ緊急のお知らせメッセージやPushメッセージを送りたい場合、国家を指定してメッセージを表示することができます。
* **運営者の現地の標準時(local timezone)を選択して簡単に時間を入力できます。**
	* ベトナムでゲームを運営する場合、ベトナムの標準時(timezone)を選択すればベトナム時間ベースで入力できるため、韓国時間に変更する手間を省くことができます。

### Using the other TOAST Service

* ゲームで必要なTOASTサービスをより簡単に連携できるようにサポートします。 
  * GamebaseのユーザーIDで各サービスのAPIを使用することができるようにGamebaseでラッピング(wrapping)してAPIを提供します。したがってユーザーは、サービスのAPIを直接呼び出す必要がありません。
  * [Notification > Push](https://toast.com/service/notification/push)：Pushメッセージを送信してくれる統合Pushサービス
  * [Mobile Service > IAP](https://toast.com/service/mobile_service/iap)：統合アプリ内決済 サービス
  * [Game > Leaderboard](https://toast.com/service/game/leaderboard)：リアルタイムの大容量ランキングサービス
  * [Security > AppGuard](https://toast.com/service/security/appguard)：リアルタイムでアプリケーションのコード操作を防止するサービス

## Terms
次は、Gamebaseのサービス用語をまとめたものです。

| 用語      | 説明                                      |
| ------- | ---------------------------------------- |
| ゲームユーザーID  | Gamebase内部のユーザー識別子                     |
| デバイスキー  | デバイス識別子(iOS:IDFV、Android:Android ID)   |
| UUID    | Guest作成時に使用される端末識別子で、アプリを削除する前まで維持     |
| IdP     | Identify Providerで、認証提供者。Facebook、Google、Apple Game Center、PAYCOなど |
| IdPトークン  | IdP SDKから認証した後に受け取るアクセストークン(access token)  |
| IdPログイン| 外部IdPログイン(Facebook、Googleなど)           |

<br/>

## Service Architecture
次は、Gamebaseのサービス構造と簡単な説明です。
![論理構成図](http://static.toastoven.net/prod_gamebase/Overview/Gamebase_overview_03_201903_en.png)
<br>

| コンポーネント名           | 説明                                      |
| --------------- | ---------------------------------------- |
| Gamebase SDK    | - クライアント開発のためのSDK                       |
| Gamebase Server | - 内部/外部モジュール間のマッシュアップAPI(mashup API)を提供して内部ロジックを処理 <br>- クライアントの初期起動時にデータを提供 <br>-ユーザーを区別するキーの発行と管理、マッピング管理 <br>- ゲームごとの同時接続者数指標の収集及び管理 |
| Console         | - ウェブConsole                              |


## Platform Guide

### Client Developer's Guide


* [Android SDK ご利用ガイド](./aos-started/)
* [iOS SDK ご利用ガイド](./ios-started/)
* [Unity SDK ご利用ガイド](./unity-started/)

### Server Developer's Guide

* [APIガイド](./api-guide/)

### Operator's Guide

* [Console ご利用ガイド](./oper-operating-indicator/)

<br/>
## Funtional Guide

| Feature               | Description                              | Client                                   | Server                                   | Console                                  |
| --------------------- | ---------------------------------------- | ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| Login                 | ゲスト、3rd Party認証に対応  <br> - [対応IdP](./Overview/#authentication) |  [[Android](./aos-authentication/#login)] [[iOS](./ios-authentication/#login)] [[Unity](./unity-authentication/#login)] | [[トークン検証](./api-guide/#token-authentication)] <br> [[会員照会](./api-guide/#get-member)] | [[App] > 認証情報設定](./oper-app/#authentication-information) <br> [[Member] > 会員照会](./oper-member/#member) <br> - 基本情報、ログイン履歴、プレイ時間、決済履歴など |
| Logout                | ログアウト                                    | [[Android](./aos-authentication/#logout)]  [[iOS](./ios-authentication/#logout)] [[Unity](./unity-authentication/#logout)] |                                          |                                          |
| Withdraw              | ゲーム退会 <br> -  ゲームユーザーのユーザーID、マッピング情報などすべての情報を削除 | [[Android](./aos-authentication/#withdraw)] [[iOS](./ios-authentication/#withdraw)] [[Unity](./unity-authentication/#withdraw)] |                                          |                                          |
| Mapping               | 一つのユーザーIDに複数のIdPを連携する機能           | [[Android](./aos-authentication/#mapping)] [[iOS](./ios-authentication/#mapping)] [[Unity](./unity-authentication/#mapping)] |                                          |                                          |
| Analytics                  | リアルタイム指標, 売上指標, 利用者指標, バランシング指標 | [[Android](./aos-etc/#analytics)] [[iOS](./ios-etc/#analytics)] [[Unity](./unity-etc/#analytics)] |                                          | [[Analytics]](./oper-analytics)  ||
| Purchase(IAP)         | (TOASTサービスの連携) <br> アプリ内決済 <br> - 対応ストア：Google、App Store | [[Android](./aos-purchase/#purchase)] [[iOS](./ios-purchase/#purchase)] [[Unity](./unity-purchase/#purchase)] | [[Wrapping API](./api-guide/#purchaseiap)] | [[Purchase]](./oper-purchase/#app)<br> [- アイテム登録](./oper-purchase/#item) <br> [- 決済情報の照会](./oper-purchase/#transactions) |
| Push                  | (TOASTサービスの連携) <br> Pushメッセージの送信及び結果の確認 | [[Android](./aos-push/#push)] [[iOS](./ios-push/#push)] [[Unity](./unity-push/#push)] |                                          | [[Push]](./oper-push/#push) <br/>- リアルタイム、予約Push送信 |
| Leaderboard           | (TOASTサービスの連携) <br> リアルタイムの大容量ランキング照会及び登録 |                                          | [[Wrapping API](./api-guide/#leaderboard)] |                                          |
| Webview               | SDKで基本的なWebView UIを提供<br/>システムポップアップ、トースト(toast) UIを提供 | [[Android](./aos-ui/#webview)] [[iOS](./ios-ui/#webview)] [[Unity](./unity-ui/#webview)] |                                          |                                          |
| [Operator] Maintenance | (運営)メンテナンス機能                               |                                          | [[メンテナンス有無の確認](./api-guide/#maintenance)] | [[Maintenance]](./oper-operation/#maintenance)<br>- メンテナンス登録、メンテナンス解除 |
| [Operator] Notice      | (運営)緊急のお知らせ機能 <br> -  ゲームユーザーがアプリを起動する際にポップアップ形式でお知らせの確認が可能 |                                          |                                          | [[Notice]](./oper-operation/#notice) <br/>-お知らせ登録 |
| [Operator] Ban         | (運営)ゲームユーザー利用停止の登録及び解除 <br> -  ゲームユーザー利用停止の登録及び解除 | [[Android](./aos-authentication/#get-banned-user-information)] [[iOS](./ios-authentication/#get-banned-user-information)] [[Unity](./unity-authentication/#get-banned-user-infomation)] <br/> -利用停止中のゲームユーザー情報の確認 |       [ゲーム利用者の利用停止履歴照会](./api-guide/#ban-histories)                                         | [[Ban]](./oper-ban/#ban) <br/>-利用停止の登録及び解除 |


