## Game > Gamebase > リリースノート > JavaScript

### 2.19.1 (2021.02.09)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.19.1/GamebaseSDK-JavaScript.zip)

#### バグ修正 
* コンソールにサポート情報を入力しない場合、初期化時にエラーが発生する問題を修正

### 2.19.0 (2020.12.29)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.19.0/GamebaseSDK-JavaScript.zip)

#### 機能追加
* [SDK] 2.19.0
	* (共通) Weibo認証を追加
	
#### 機能改善/変更
* [SDK] 2.19.0
	* (共通)ローンチステータスコード追加：ベータサービス(205)

### 2.15.0 (2020.09.15)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.15.0/GamebaseSDK-JavaScript.zip)

#### 機能追加
* [SDK] 2.15.0
    * (JavaScript)ハンゲームポイント決済APIにGamebaseProductIdを追加

### 2.10.1 (2020.06.09)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.10.1/GamebaseSDK-JavaScript.zip)

#### バグ修正
* [SDK] 2.10.1
	* (JavaScript)初期化時、StoreCodeを入力していない場合、エラーが発生する問題を修正

### 2.10.0 (2020.05.26)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.10.0/GamebaseSDK-JavaScript.zip)

#### 機能追加
* [SDK] 2.10.0
	* (共通)既存のすべてのイベントシステムを統合するGamebaseEventHandlerを追加
		* ServerPush、Observer機能が含まれていて、プロモーション決済イベントおよびプッシュイベントも確認可能

### 2.9.0 (2020.04.28)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.9.0/GamebaseSDK-JavaScript.zip)

#### 機能追加
* 退会猶予機能
	* [SDK] 2.9.0
		*(共通)API追加：退会猶予申請、退会猶予申請キャンセル、退会猶予状態から即時退会、ユーザーの退会猶予状態を確認

### 2.8.1 (2020.04.14)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.8.1/GamebaseSDK-JavaScript.zip)

#### バグ修正
* [SDK] 2.8.1 
	* (JavaScript) AdditionalInfoログインにおいて、Hangame IdPでログインできない問題を修正

### 2.8.0 (2020.03.24)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.8.0/GamebaseSDK-JavaScript.zip)

#### 機能追加
* [SDK] 2.8.0
 	* (Javascript)ハンゲームチャネリングをサポート：ハンゲームIdP認証、ハンコイン決済を追加

#### 機能改善/変更
* [SDK] 2.8.0 
	* (共通)コンソールに登録されていないアプリバージョンで初期化に失敗した時、ストアに移動できるポップアップが表示されるように改善
	
### 2.6.2 (2019.12.24)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.6.2/GamebaseSDK-JavaScript.zip)
#### 機能改善/変更
* [SDK] 2.6.2
	* 불필요한 오류 로그 제거

### 2.4.4 (2019.07.23)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.4/GamebaseSDK-JavaScript.zip)

#### 機能改善/変更
* [SDK] 2.4.4
	* (共通)会員エラーコードフォーマットを変更

### 2.4.2 (2019.06.25)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.2/GamebaseSDK-JavaScript.zip)

#### 機能改善/変更
* [SDK] 2.4.2
	* (共通)LaunchingInfoにJSON string形式のTOAST Launching情報を追加

#### バグ修正
* [SDK] 2.4.2
	* (共通)Analyticsのバグを修正：ログアウト、退会、アカウント移行時に保存された指標データを初期化するように修正

### 2.4.0 (2019.05.28)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.0/GamebaseSDK-JavaScript.zip)

#### 機能改善/変更
* [SDK] 2.4.0
	* (共通)指標関連Class変更
        * LevelUpData Class：userLevel、levelUpTimeパラメータが必須に変更 / その他フィールド削除[詳細表示[Android](./aos-etc/#game-user-data-settings) / [iOS](./ios-etc/#game-user-data-settings) / [Unity](./unity-etc/#game-user-data-settings) / [JavaScript](./js-etc/#game-user-data-settings)]
        * GameUserData Class：classId(ゲームユーザーの職業)フィールド追加[詳細表示[Android](./aos-etc/#level-up-trace) / [iOS](./ios-etc/#level-up-trace) / [Unity](./unity-etc/#level-up-trace) / [JavaScript](./js-etc/#level-up-trace)]
    
### 2.0.0 (2019.01.29)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.0.0/GamebaseSDK-JavaScript.zip)

```
Gamebase 2.0の改善された全体指標を活用するためには、SDKのアップデートが必要です。
```

#### 機能追加
* [SDK] 2.0.0
	* (共通)Custom指標のためのAPIを追加(購入成功の場合、SDK内部で自動伝送)
		* setGameUserData：ゲームログイン後、ゲームユーザーレベル情報を伝送
		* traceLevelUpData：レベルアップ追跡のために、ゲームユーザーがレベルアップした時に呼び出す
    * (JavaScript)SDK新規配布