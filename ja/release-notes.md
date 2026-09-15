<!-- machine_translated: true -->

<!-- pre-align:aligned sig=4da805cd3daa -->

<a id="game-gamebase"></a>
## Game > Gamebase > リリースノート { #game-gamebase }

<a id="2021-04-19"></a>
### 2021. 04. 19. { #2021-04-19 }

<a id="2021-04-19-1"></a>
#### バグ修正
* [SDK] 2.21.1
	* (Android) HangameのログインをPAYCOで進める際にキャンセルするとクラッシュが発生する問題を修正
	* (iOS) bitcodeをサポートするように設定しても設定値が反映されない問題を修正

<a id="2021-04-13"></a>
### 2021. 04. 13. { #2021-04-13 }

<a id="2021-04-13-1"></a>
#### 機能追加
* [SDK] 2.21.0
	* (共通) Hangame日本認証を追加

<a id="2021-04-13-2"></a>
#### 機能改善/変更
* [Console] 
	* 会員 > メンバー: プッシュトークン照会時に広告性受信同意、夜間広告受信がtrueの場合、受信した日付も合わせて表示されるよう改善
	* 購入(IAP) > 決済情報: 追加情報が表示されるポップアップに文字列が折り返して表示されるよう改善
	* 購入(IAP) > 決済不正行為モニタリング
		* 1時間に固定されていた自動制裁検知期間をユーザーが入力(1時間〜48時間)できるよう改善
		* AND条件のみ設定可能だった件数、金額の自動制裁条件入力をOR条件も入力できるよう改善
* [SDK] 2.21.0	
	* (Android) 外部SDKアップデート: Facebook Android SDK(6.5.1)、Line Android SDK(5.4.0)
	* (iOS) bitcodeサポートが可能になるよう変更
	* (iOS) showWebView呼び出し時、閉じるボタンを最初に画面に表示されるよう修正
	
<a id="2021-04-13-3"></a>
#### バグ修正
* [SDK] 2.21.0
	* (Android) Proguardを適用したビルドで決済API呼び出し時にクラッシュが発生するエラーを修正

<a id="2021-03-30"></a>
### 2021. 03. 30. { #2021-03-30 }

<a id="2021-03-30-1"></a>
#### 機能改善/変更
* [SDK] 2.20.2
	* (Android) Google PlayストアのAndroid 11端末での決済エラーが解決されたBilling Client 3.0.3バージョンにアップデート

<a id="2021-03-23"></a>
### 2021. 03. 23. { #2021-03-23 }

<a id="2021-03-23-1"></a>
#### 機能改善/変更
* [Console] 会員 > ダウンロード: 1つのファイルに保存されるデータ数を改善(5万 -> 50万)
* [SDK] 2.20.2
	* (iOS) Facebook iOS SDKアップデート(9.1.0)
	* (iOS) 特定のケースでGamebaseAuthFacebookAdapterからopenURL delegateが呼び出されなかった問題を修正

<a id="2021-03-09"></a>
### 2021. 03. 09. { #2021-03-09 }

<a id="2021-03-09-1"></a>
#### 機能追加
* [Console] アプリ > 利用規約: GDPR利用規約を追加
* [Server API] IdP IDでGamebase user IDを取得するAPIを追加

<a id="2021-03-09-2"></a>
#### 機能改善/変更
* [SDK] 2.20.1
	* (iOS) iOS 14に対応してIDFA取得ロジックを修正: info.plistにNSUserTrackingUsageDescriptionフィールドを追加

<a id="2021-02-23"></a>
### 2021. 02. 23. { #2021-02-23 }

<a id="2021-02-23-1"></a>
#### 機能追加
* [Console] 
	* 運営 > キックアウト: クライアントバージョン別にキックアウトが可能な機能を追加
	* 購入(IAP) > ストア: Google Playストアの一回性領収書検証ステップを設定できる機能を追加
	
<a id="2021-02-23-2"></a>
#### バグ修正
* [SDK] 2.20.1
	* (Android) push-fcmモジュール初期化中にクラッシュが発生する可能性があるロジックを修正

<a id="2021-02-15"></a>
### 2021. 02. 15. { #2021-02-15 }

<a id="2021-02-15-1"></a>
#### バグ修正
* [Console] 購入(IAP) > 決済履歴: ファイルダウンロード時に商品名が誤って表示されるエラーを修正

<a id="2021-02-09"></a>
### 2021. 02. 09. { #2021-02-09 }

<a id="2021-02-09-1"></a>
#### 機能追加
* 共通利用規約機能の追加
	* [Console] 新規メニューオープン: アプリ > 利用規約、アプリ > 利用規約の配布
	* [SDK] 2.20.0
		* (共通) 利用規約WebViewを開くAPIの追加
		* (共通) 利用規約リストおよびユーザーごとの同意状況を照会するAPIの追加
		* (共通) ユーザーごとの利用規約同意状況をGamebaseサーバーに保存するAPIの追加

<a id="2021-02-09-2"></a>
#### 機能改善/変更
* [Console] アプリ > クライアント: クライアントバージョンをストアごとに区分して表示するよう画面を改善
* [SDK] 2.20.0
	* (共通) サポートタイプがTOAST組織商品(Online Contact)の場合、ログインしなくてもサポートが表示されるよう変更
	* (Unity) Warningログの削除
	* (Unity) Standalone WebViewにCEF 2.1.2をアップデート
		* URLの長さが2,048より長い場合にクラッシュが発生する問題を修正
		* Unity 2019でビルド時にライブラリパスが変更されるためPostProcessBuildを改善
		* stringの初期化を行わないことで断続的に発生するエラーを修正
		* Gamebase WebView使用中にWebViewがシーン(scene)を移動した後、再度開かれないバグを修正

<a id="2021-02-09-3"></a>
#### バグ修正
* [SDK] 2.20.0
	* (JavaScript) コンソールにサポート情報を入力しない場合、初期化時にエラーが発生する問題を修正
* [SDK] 2.19.1
	* (Unreal) Unityビルド中に除外されるファイルが発生する際のコンパイルエラーを修正

<a id="2021-01-26"></a>
### 2021. 01. 26. { #2021-01-26 }

```
Push > Push（旧）コンソールメニュー機能が除外されました。
```

<a id="2021-01-26-1"></a>
#### 機能追加
* [Console] 
	* 利用停止 > AppGuard: 条件ブロック機能を追加
	* 購入(IAP) > 決済不正行為モニタリング: Apple App Store を追加 
* [SDK] 2.19.0
	* (Unreal) SDK リリース: 2.16.0〜2.19.0 の累積内容を反映
		* [Android 設定ツール](https://docs.toast.com/ko/Game/Gamebase/ko/unreal-started/#android-settings) を提供: Gamebase_Android_UPL.xml ファイルを修正する代わりに、設定ツールをご使用ください。
		* カスタマーセンター機能を追加	
		* 認証を追加: Hangame、Weibo
		* Galaxy Store を追加
		* 決済アイテム情報にローカライズされた商品情報を追加: localizedTitle、localizedDescription
		* Android 設定ツールを提供
		* Unreal 4.26 をサポート

<a id="2021-01-26-2"></a>
#### 機能改善/変更
* [Console]
	* 管理 > 権限: 売上アクセス権限を削除 [関連のお知らせへ](https://www.toast.com/kr/support/notice/detail/2101)
* [SDK] 2.19.1
	* (iOS) Weibo IdPAdapter の構造を変更	

<a id="2021-01-12"></a>
### 2021. 01. 12. { #2021-01-12 }

```
GamebaseのXCodeの最小サポートバージョンが10から11に変更されました。
```

<a id="2021-01-12-1"></a>
#### 機能追加
* [Console] Push 新規メニューを追加
	* 統計: Push 送信、受信、トークン登録などの Push 統計を確認
	* イベントキー: Push 送信統計に使用するイベントキーを管理
	* 証明書: Push 送信に使用する証明書を管理
	* 設定: Push 関連の設定値を管理
	
<a id="2020-12-29"></a>
### 2020. 12. 29. { #2020-12-29 }

<a id="2020-12-29-1"></a>
#### 機能追加
* [SDK] 2.19.0
	* (共通) Weibo 認証を追加
	* (Android) Sign In with Apple 認証を追加
	
<a id="2020-12-29-2"></a>
#### 機能改善/変更
* [Console]
	* アプリ > アプリ: サーバーアドレス管理にベータサービスサーバーを追加
	* アプリ > クライアント: クライアントステータスにベータサービスを追加、クライアントの追加情報を登録できるメモ機能を追加
	* 購入(IAP) > 商品: 検索条件を追加 - 使用有無
	* 購入(IAP) > 決済情報: 決済履歴にストアテスト決済件を表示
	* 購入(IAP) > 販売状況メニューの終了: Analytics > 売上指標と機能が統合されました。
	* Analytics > 利用環境 > インストールURL メニューの終了
* [SDK] 2.19.0
	* (共通) 起動状態コードを追加: ベータサービス(205)

<a id="2020-12-29-3"></a>
#### バグ修正
* [SDK] 2.19.0
    * (Unity) WebSocketで再試行時にOutOfMemoryExceptionが発生する問題を修正
* [SDK] 2.19.1
	* (Android) Weiboログイン試行後、別のIdPでログイン時にクラッシュが発生する問題を修正
	
<a id="2020-12-15"></a>
### 2020. 12. 15. { #2020-12-15 }

<a id="2020-12-15-1"></a>
#### 機能追加
* Gamebase カスタマーセンターページを開く際に、ゲームで定義した extra data を渡す機能: SDK 2.18.2
	* [Console] カスタマーセンター > お客様お問い合わせ: お客様お問い合わせの詳細表示画面で、追加登録した extra data を確認可能
* [SDK] 2.18.2
	* (共通) デベロッパー独自のカスタマーセンターを開く際に additionalURL フィールドを追加
	* (共通) 決済アイテム情報に、地域化された商品情報を追加: localizedTitle、localizedDescription

<a id="2020-12-15-2"></a>
#### 機能改善/変更
* [Console]
	* Analytics: フィルター検索後に日付を変更しても、選択した検索条件が維持されるように改善
	* Push > プッシュ: Tencent Push を削除
	* 購入 (IAP) > 決済情報: 払い戻し状態で領収書検証ボタンが表示されないように変更
* [SDK] 2.18.2
    * (共通) TOAST SDK アップデート: [Android(0.24.2)](https://docs.toast.com/ko/TOAST/ko/toast-sdk/release-notes-android/#0242-20201124)、[iOS(0.27.1)](https://docs.toast.com/ko/TOAST/ko/toast-sdk/release-notes-ios/#0271-20201124)、[Unity(0.21.3)](https://docs.toast.com/ko/TOAST/ko/toast-sdk/release-notes-unity/#0213-20201124)
	* (Android) 暗号化ロジックのセキュリティ警告解決のための外部 SDK アップデート: Payco Login SDK(1.5.3)、Hangame ID SDK(1.3.2)
	* (Android) Tencent Push モジュールを削除
	* (Android) Gamebase Android SDK 2.6.0 で deprecated になった関数を削除
		* GamebaseConfiguration.Builder.setFCMSenderId()
		* GamebaseConfiguration.Builder.setTencentAccessKey()
		* GamebaseConfiguration.Builder.setTencentAccessId()
	* (iOS) showWebView: 誤った URL を渡した場合にエラーを返し、受け取った URL はエンコードせずそのまま使用
	* (iOS) 大文字・小文字を問わずカスタムスキームが動作するように変更
	* (Unity) GamebaseRequest.GamebaseConfiguration クラスのフィールドを deprecated: zoneType、fcmSenderId

<a id="2020-12-15-3"></a>
#### バグ修正
* [Console]
	* 購入 (IAP) > アイテム: ファイルでアイテムを大量登録すると重複登録される問題を修正
* [SDK] 2.18.2
    * (Android) 5.0〜6.0 OS 端末でWebビューのカスタムスキームが動作しない問題を修正

<a id="2020-12-2"></a>
### 2020. 12. 2. { #2020-12-2 }

<a id="2020-12-2-1"></a>
#### 機能追加
* [Console] 
	* Gamebase 権限の細分化機能を追加: 24種
	* Analytics > グループ指標: プロジェクト別の新規登録者数・決済金額の比較グラフを追加    
	* メンバー > 会員: 下部にカスタマーセンター問い合わせ履歴の照会タブを追加
	* クーポン > クーポン発行: 発行済みクーポンに追加でクーポンを発行できるクーポン追加発行機能を追加（1回あたり10万件）

<a id="2020-12-2-2"></a>
#### 機能改善/変更
* [Console]
    * Analytics > リアルタイムモニタリング: 前日の指標と比較してデータが上昇・下降した場合の表示色を変更
		* 上昇: 青色→赤色、下降: 赤色→青色
	* Analytics > 売上指標 > 決済金額: 国別のみ比較可能だった売上データをストア別・IdP 別でも確認できるように追加
	* 運営 > お知らせ: 詳細リンクにカスタマーセンターを連携できるよう設定を追加
	* カスタマーセンター > お問い合わせ: 回答送信設定に自動翻訳機能を追加
	* クーポン > クーポン発行: クーポンの初回発行数量を最大5万件から最大100万件に増加

<a id="2020-12-2-3"></a>
#### バグ修正
* [Console]
    * 購入（IAP）> 決済情報: 照会したデータが多い場合にファイルをダウンロードできない問題を修正

<a id="2020-11-10"></a>
### 2020. 11. 10. { #2020-11-10 }

<a id="2020-11-10-1"></a>
#### 機能追加
* Galaxy ストア追加: SDK 2.18.0

<a id="2020-11-10-2"></a>
#### 機能改善/変更
* [SDK] 2.18.0
    * (Android) TOAST SDK アップデート: [Android(0.24.1)](https://docs.toast.com/ko/TOAST/ko/toast-sdk/release-notes-android/#0240-20201027)-GooglePlay Billing Library v.3.0.1 適用
    * (Android) WebView SSL セキュリティ警告への対応処理を追加
    * (iOS) iOS 13 以上から提供される SceneDelegate 対応 API を追加

<a id="2020-11-10-3"></a>
#### バグ修正
* [SDK] 2.18.1
    * (Android) 2.18.0 で Google 決済後にクラッシュが発生する問題を修正

<a id="2020-10-27"></a>
### 2020. 10. 27. { #2020-10-27 }

<a id="2020-10-27-1"></a>
#### 機能追加
* Unreal SDK 機能追加: SDK 2.15.0
    * 既存のすべてのイベントシステムを統合する GamebaseEventHandler を追加
        * ServerPush、Observer 機能を含み、プロモーション決済イベントおよび Push イベントを確認可能
    * API 追加
    	* 商品 ID で決済をリクエストし、追加情報（UserPayload）を入力して決済完了時に確認できる決済 API を追加
    	* イメージお知らせ表示: showImageNotices
    	* Push トークン情報確認: queryTokenInfo
    * Push トークン登録時に NotificationOption 設定により、アプリがフォアグラウンド（foreground）状態でも Push 通知を受信できる機能を追加
    * WebViewConfiguration contentMode 設定を追加

<a id="2020-10-27-2"></a>
#### 機能改善/変更
* [SDK] 2.17.1
    * (iOS) 特定の指標の送信時にエラーメッセージを追加して送信: Push 登録失敗時、ゲーム指標送信時
    * (Unity) Unity 2017.2.5 サポート
* [SDK] 2.15.0
    * (Unreal) TOAST SDK アップデート: Android(0.23.0), iOS(0.26.0), Unity(0.21.0)    

<a id="2020-10-27-3"></a>
#### バグ修正
* [Console]
    * Analytics > 利用者指標: 週間・月間の平均 CCU 計算ロジックを修正し、異常に表示される問題を修正
    * Push > プッシュ: タイトルを入力せずにタイトルの文字色を黒以外の色に設定すると、タイトルに「null」と表示される問題を修正
	* クーポン > クーポン発行: 発行済みクーポンが 5 万個以上の場合、ファイルがダウンロードされない問題を修正
* [SDK] 2.17.1
    * (Unity) イメージお知らせと Web ビューを順に呼び出すと、後から呼び出した API が動作しないエラーを修正	
* [SDK] 2.15.0    
    * (Unreal) 決済モジュールに ProGuard 宣言が漏れているエラーを修正


<a id="2020-10-13"></a>
### 2020. 10. 13. { #2020-10-13 }

```
Hangame認証の使用を希望する場合は、事前にカスタマーセンターまでお問い合わせください。
```

<a id="2020-10-13-1"></a>
#### 機能追加
* Hangame IdP 認証の追加: SDK 2.17.0

<a id="2020-10-13-2"></a>
#### 機能改善/変更
* [SDK] 2.17.0
    * (共通) カスタマーセンターの添付画像クリック時のダウンロードをサポート
    * (共通) TOAST SDK アップデート: Android(0.23.2), Unity(0.21.2)
    * (iOS) TCGBMember.regDate、TCGBMember.lastLoginDate の型を long long に変更
    * (iOS) WebView で URL およびタイトル変更時にタイトルを再表示できるようにロジックを変更

<a id="2020-10-13-3"></a>
#### バグ修正
* [SDK] 2.17.0
    * (iOS) PAYCO 認証: lastLoggedInProvider ログイン後にログアウト呼び出し時、ログアウトコールバックが届かない問題を修正
* [SDK] 2.17.1
    * (Android) 2.17.0 で ImageNotice API 呼び出し時に kotlinx-coroutine モジュールでクラッシュが発生する問題を修正
	
<a id="2020-09-22"></a>
### 2020. 09. 22. { #2020-09-22 }

<a id="2020-09-22-1"></a>
#### 機能追加
* カスタマーセンター機能追加
    * [Console] カスタマーセンターメニューオープン: カスタマー問い合わせ処理、FAQ/お知らせ管理 
    * [SDK] 2.16.0
	* (共通) API追加(Gamebase.Contact.requestContactURL): カスタマーセンターURLを返す
	* (共通) カスタマーセンターAPIにuserNameを設定できるようにContactConfigurationパラメーターを追加 
		
<a id="2020-09-22-2"></a>
#### 機能改善/変更
* [Console] 
    * Analyticsメニュー共通: 国別フィルターの並び替え基準を変更（指標降順 -> 国名昇順）     
    * Analytics > 売上指標: ストア別ダッシュボードに当該ストアの国別決済金額のほかに、決済金額の合計も表示 

<a id="2020-09-16"></a>
### 2020. 09. 16. { #2020-09-16 }

<a id="2020-09-16-1"></a>
#### 機能改善/変更
* [SDK] 2.15.1
    * (iOS) TOAST SDK アップデート: iOS(0.27.0)
	* iOS 14 beta の変更事項に対応した IAP SDK の新バージョンが適用されました。[TOAST SDK Release Notes](https://docs.toast.com/ko/TOAST/ko/toast-sdk/release-notes-ios/#0270-20200911)

<a id="2020-09-15"></a>
### 2020. 09. 15. { #2020-09-15 }

<a id="2020-09-15-1"></a>
#### 機能追加
* [SDK] 2.15.0
    * (JavaScript) ハンゲームポイント決済 API に GamebaseProductId を追加

<a id="2020-09-15-2"></a>
#### バグ修正
* [Console]
    * 購入(IAP) > 決済情報: 領収書検証の表示が正しく行われなかった問題を修正

<a id="2020-08-25"></a>
### 2020. 08. 25. { #2020-08-25 }

```
Gamebase SDK 2.15.0 で、Google Billing Client モジュールが更新されました。

「gamebase-adapter-purchase-google」を使用している場合、Gamebase SDK 2.15.0 未満のバージョンから 2.15.0 以上にアップグレードする際は、必ず以前のバージョンの「Game Client Version」を「アップデート必須」に設定する必要があります。

アイテムの購入中にエラーが発生した場合、再処理が実行されます。複数の端末で異なる Billing Client バージョンが適用されている状態では、再処理の実行中に問題が発生する可能性があるためです。
```

<a id="2020-08-25-1"></a>
#### 機能追加
* [SDK] 2.15.0
    * (共通) プッシュトークン登録時に NotificationOption 設定により、アプリがフォアグラウンド (foreground) 状態でもプッシュ通知を受信できる機能を追加しました。
    * (共通) プッシュ API を追加しました: プッシュトークン情報の確認 (Gamebase.Push.queryTokenInfo API)
* [SDK] 2.9.1
    * (Unreal) Unreal 4.22〜4.25 をサポートしました。
    * (Unreal) PLCrashReporter の問題をサポートしました: [ガイド](http://docs.toast.com/ko/Game/Gamebase/ko/unreal-started/#ios-settings)

<a id="2020-08-25-2"></a>
#### 機能改善/変更
* [Console]
    * プッシュ > プッシュ: 宣伝目的のプッシュ通知送信時に、発信者の連絡先および受信撤回の同意方法を入力しなくても送信できるように修正しました。
* [SDK] 2.15.0
    * (共通) TOAST SDK をアップデートしました: Android(0.23.0)、iOS(0.26.0)、Unity(0.21.0)
    * (iOS) 決済 payload の null チェックロジックを追加しました。
* [SDK] 2.9.1
    * (Unreal) iOS Plugin 内部の Gamebase SDK for iOS バージョンをアップデートしました (2.9.1)。
    * (Unreal) UObject リファレンス処理が欠落していた箇所を修正しました。

<a id="2020-08-25-3"></a>
#### バグ修正
* [Console]
    * プッシュ > プッシュ: プッシュ通知の繰り返し送信時に、時間情報が入力されたタイムゾーンに関係なく、常に UTC+9 で計算されて送信されていた問題を修正しました。
    
<a id="2020-08-19"></a>
### 2020. 08. 19. { #2020-08-19 }

<a id="2020-08-19-1"></a>
#### バグ修正
* [Console]
    * Analytics 全メニュー: Excel ダウンロードができない問題を修正しました。

<a id="2020-08-11"></a>
### 2020. 08. 11. { #2020-08-11 }

<a id="2020-08-11-1"></a>
#### 機能改善/変更
* [Console]
    * Analytics > 利用者指標 > Retention: % に加えて数値を追加で表示します。
* [SDK] 2.14.0
    * (iOS) PAYCO IdP の定数値を削除しました: PAYCO 文字列によって Apple 審査がリジェクトされるケースが発生したため削除しました。
    * (iOS, Unity) TCGBWebViewConfiguration に contentMode 設定を追加しました。
* [Server]
    * クーポン消費 API のエラーコードを追加しました: クーポンコードに英字・数字以外の値を入力した場合 (Error Code: -4000205)

<a id="2020-07-28"></a>
### 2020. 07. 28. { #2020-07-28 }

<a id="2020-07-28-1"></a>
#### 機能追加
* [Console]
    * Analytics: WAU（Weekly Active User）、MAU（Monthly Active User）指標を追加
* [SDK] 2.13.0
    * (Unity) Standalone: イメージ公知表示 API を追加

<a id="2020-07-28-2"></a>
#### 機能改善/変更
* [Console]
    * アプリ > アプリ: iOS 12 以下で Sign In With Apple 認証を行うための情報を追加入力できるように修正
* [SDK] 2.13.0
    * (Android) イメージ公知のポップアップ画像の比率計算ロジックを修正
    * (iOS) Sign In With Apple 認証: iOS 12 以下をサポート

<a id="2020-07-28-3"></a>
#### バグ修正
* [Console]
    * 運営 > イメージ公知: コピー機能および対象国を選択後、全国に変更した際に反映されないエラーを修正
* [SDK] 2.13.0
    * (Android) WebView 終了時に終了コールバックで ANDROID_ACTIVITY_DESTROYED(31) エラーが返される問題を修正
    * (Android) 決済モジュールに ProGuard の宣言が欠落しているエラーを修正

<a id="2020-07-14"></a>
### 2020. 07. 14. { #2020-07-14 }

<a id="2020-07-14-1"></a>
#### 機能追加
* イメージお知らせ: 表示期間と優先度に応じてゲーム内イメージポップアップを表示
    * [Console] 運営 > イメージお知らせ: メニューを追加
    * [SDK] 2.12.0: イメージお知らせ表示APIを追加

<a id="2020-07-14-2"></a>
#### 機能改善/変更
* [Console] 
    * 購入(IAP) > 商品: アイテム番号で商品を照会できるように追加
    * メンバー > 会員: 退会猶予状態のユーザーを正常状態に変更できるように改善
    * メンバー > ダウンロード: ログインログ履歴にdeviceKey、IdPコード項目を追加
* [SDK] 2.12.0
    * (iOS) Facebook SDKアップデート(7.1.1)
    * (iOS) configurationに設定されたstoreCode(default=AS)でGamebaseの初期化を試みます
    * (iOS) コンテンツを読み込めないWebView表示時に**[閉じる]**ボタンがなく閉じられない問題を修正
    * (Unity) TOAST Unity SDKアップデート(0.20.1.1)
    
<a id="2020-06-23"></a>
### 2020. 06. 23. { #2020-06-23 }

<a id="2020-06-23-1"></a>
#### 機能追加
* [SDK] 2.11.0
	* 決済API追加: 商品IDで決済リクエスト、追加情報(UserPayload)を入力して決済完了時に確認できます

<a id="2020-06-23-2"></a>
#### 機能改善/変更
* [Console] 
	* 購入(IAP) > 商品: ストアアイテムIDに複数のGamebase商品を登録して管理できるように改善

<a id="2020-06-09"></a>
### 2020. 06. 09. { #2020-06-09 }

<a id="2020-06-09-1"></a>
#### 機能改善/変更
* [Console] 
	* メンバー > 会員: **[退会履歴照会]** 画面に退会猶予状態（退会猶予、退会キャンセル、即時退会）を追加表示
* [SDK] 2.10.1
	* (iOS) ユーザーのPush設定の初期化時に言語コードが設定されていない場合、デバイスの言語に設定されるよう変更

<a id="2020-06-09-2"></a>
#### バグ修正
* [Console] 
	* クーポン > クーポン発行: クーポン統計のダウンロード時にSMSで送信した履歴がダウンロードされない問題を修正

* [SDK] 2.10.1
	* (Unity) iOS PluginでViewControllerが設定されていないため、ログイン呼び出し時に失敗する問題を修正
	* (JavaScript) 初期化時にStoreCodeを入力しないとエラーが発生する問題を修正

<a id="2020-05-26"></a>
### 2020. 05. 26. { #2020-05-26 }

<a id="2020-05-26-1"></a>
#### 機能追加
* [Console] 
	* クーポン > クーポン発行: 発送統計機能、クーポン発送履歴ダウンロード機能を追加
* [SDK] 2.10.0
	* (共通) 既存のすべてのイベントシステムを統合する GamebaseEventHandler を追加
		* ServerPush、Observer 機能を含んでおり、プロモーション決済イベントおよび Pushイベントも確認可能

<a id="2020-05-26-2"></a>
#### 機能改善/変更
* [Console] 
	* 全体: 共通デザインガイドに合わせてボタン/タグ UI を修正
* [SDK] 2.10.0 
	* (Unity) StandaloneWebviewAdapter 内部の CefWebview バージョンアップデート: v2.0.4
		* WebviewIndex の検証ロジックを改善
		* Webview 作成時に断続的に NullReferenceException が発生するエラーを改善
	* (Unity) GamebaseErrorCode にソケット接続に関するエラーコードを追加: SOCKET_CONNECTION_TIMEOUT、SOCKET_CONNECTION_FAIL

<a id="2020-05-12"></a>
### 2020. 05. 12. { #2020-05-12 }

<a id="2020-05-12-1"></a>
#### 機能追加
* [SDK] 2.9.0
	* (Unreal) SDK 新規リリース
	
<a id="2020-05-12-2"></a>
#### 機能改善/変更
* [Console] 
	* アプリ > アプリ: 退会猶予期間を変更したユーザーのToastアカウントを保存するように改善
	* メンバー > 会員: マッピング履歴照会時に情報が正しく表示されない問題を修正
	* 購入(IAP) > ストア: テスト、旧) Onestoreは新規登録ができないように修正

<a id="2020-05-12-3"></a>
#### バグ修正
* [SDK] 2.9.1
	* (Android) マッピング後に指標レベルがnullになり、決済指標に正常に反映されないエラーを修正
	* (iOS) Unrealエンジンでビルドすると、warningをビルドエラーと判定してビルドできない問題を修正

<a id="2020-04-29"></a>
### 2020. 04. 29. { #2020-04-29 }

<a id="2020-04-29-1"></a>
#### バグ修正
* [SDK] 2.9.1 
	* (Unity) Initialize後にコンソールでクライアントのサービス状態を変更するとエラーが発生する問題を修正
		* 問題が発生するバージョン: v2.8.0以降	
		* 問題のあるプラットフォーム: Standalone、WebGL、Editor
		
<a id="2020-04-28"></a>
### 2020. 04. 28. { #2020-04-28 }

<a id="2020-04-28-1"></a>
#### 機能追加
* 退会猶予機能
	* [SDK] 2.9.0
		* (共通) API 追加: 退会猶予申請、退会猶予申請キャンセル、退会猶予状態での即時退会、ユーザーの退会猶予有無の確認
	* [Console]
		* アプリ > アプリ: 退会猶予期間を設定できる機能を追加

<a id="2020-04-28-2"></a>
#### 機能改善/変更
* [SDK] 2.9.0
	* (共通) TOAST SDK アップデート: Android(v0.21.0)、iOS(v0.23.0)、Unity(0.20.1)
	* (共通) PAYCO Login SDK アップデート: Android(v1.5.0)、iOS(v1.4.0)
* [Console]
	* 全メニュー: コンソールボタン、タグデザインの修正
	* 運営 > メンテナンス、運営 > お知らせ、Push: 多言語自動翻訳機能のサポート
	* メンバー > 会員: 退会猶予ユーザーの会員照会時に猶予満了期間を追加表示

<a id="2020-04-14"></a>
### 2020. 04. 14. { #2020-04-14 }

<a id="2020-04-14-1"></a>
#### 機能改善/変更
* [Console] 
	* Analytics 共通: TUI チャートバージョンアップデート、Frequency7 指標に適用
* [SDK] 2.8.1 
	* (共通) Analytics 送信結果確認のための内部指標を追加
	
<a id="2020-04-14-2"></a>
#### バグ修正
* [Console] 
	* Analytics 共通: 国名が長くなった場合にスクロールが領域を超えるイシューを修正
	* Analytics > リアルタイムモニタリング: データ保存中に照会リクエスト時に指標が 0 で表示される現象を修正
* [SDK] 2.8.1 
	* (Android) プロセス再起動後にクラッシュが発生する可能性があるコードを修正
	* (JavaScript) credentialInfo ログインで Hangame IdP にログインできない問題を修正
	
<a id="2020-03-24"></a>
### 2020. 03. 24. { #2020-03-24 }

<a id="2020-03-24-1"></a>
#### 機能追加
* [Console] 
	* 新規メニュー公開: Analytics > ユーザー指標 > Frequency 7
		* DAU の 1 週間の訪問数と割合の情報を提供します。ゲームへの没入度やロイヤリティなどを一目で把握できます。
	* クーポン > クーポン発行: クーポン SMS 送信機能を追加
* [SDK] 2.8.0
	* (共通) 決済および商品情報に商品タイプや地域価格などの情報を追加
	* (Unity) StandaloneWebviewAdapter 内の CefWebview を v2.0.1 バージョンにアップデート
		* PopupType が PASS_INFO の場合、ポップアップを表示せずにポップアップ情報を渡す機能を追加
 	* (Javascript) ハンゲームチャネリング対応: ハンゲーム IdP 認証、ハンコイン決済を追加


<a id="2020-03-24-2"></a>
#### 機能改善/変更
* [Console] 
	* アプリ > 転送指標設定: 事前に登録したメタフィルターのみ転送指標に使用できるように制限
		* メタフィルターの数を制限しており、それを超えて転送した場合は指標が表示されませんので注意してください。: レベル (5,000 個)、ワールド/サーバー/チャンネル (100 個)、職業/クラス (100 個)
* [SDK] 2.8.0 
	* (共通) コンソールに登録されていないアプリバージョンで初期化に失敗した場合、ストアに移動できるポップアップが追加で表示されるよう改善
	* (Android) ログイン直後に決済関連 API を呼び出す際、初期化タイミング問題で失敗が発生する可能性があるコードを修正

<a id="2020-03-24-3"></a>
#### バグ修正
* [Console] 
	* 売上指標 > 決済金額
		* チャートのツールチップに表示される通貨がウォン (KRW) に固定されていたため、アプリで設定した通貨で表示されるよう修正
		* 月別照会時に 2 月の指標が表示されないイシューを修正
		
<a id="2020-03-10"></a>
### 2020. 03. 10. { #2020-03-10 }

<a id="2020-03-10-1"></a>
#### 機能追加

- [Console] 
	- アプリ  >  アプリ: Analytics 売上指標を表示する際のテスト決済を含めるかどうかの設定  
    		- 「テスト決済除外」に設定すると、Analytics 売上指標からテスト決済がすべて除外されて表示されます。 
		- 購入 (IAP): 購入 (IAP) メニューへの初回アクセス時に決済指標の通貨コードを設定 
	- 初回のみ設定可能で、Analytics 売上指標には設定された通貨コードで指標が表示されます。  
  	- モバイルコンソール (TOAST アプリを含む) に「デスクトップ表示」機能を追加

<a id="2020-03-10-2"></a>
#### 機能改善/変更

- [Console] 
  	- アプリ  >  インストールURL: 入力可能な URL スキーム (scheme) を追加適用 
    		- 従来: 共通 ('http://', 'https://')、Android ('market://') 
    		- 追加: iOS ('itms://', 'itmss://', 'itms-apps://')、Android ('intent://')
- [SDK] 2.7.2 
  	- (Unity) FacebookAdapter の改善 
    		- v7.9.4〜v7.18.1 バージョンまでの互換性テスト
    		- Null 例外処理 
  	- (Unity) StandaloneWebviewAdapter の改善 
    		- Web ページをテクスチャ (texture) としてエクスポートする機能を追加
    		- マルチ Web ビューのサポート 
    		- Cookie の削除オプションを追加 
    		- テクスチャ (texture) のサイズ変更のサポート 
		- スクロールバーの表示/非表示のサポート 
    		- ページ読み込み完了の通知 
    		- 透明背景のサポート 
  	- (Unity) エディターで Android/iOS プラットフォームを選択して Initialize API を呼び出すとエラーが発生する問題を修正

<a id="2020-03-10-3"></a>
#### バグ修正
- [Console] 
  	- Analytics: 通貨コードがコイン性の場合に売上指標が「0」と表示される問題を修正

<a id="2020-02-25"></a>
### 2020. 02. 25. { #2020-02-25 }

<a id="2020-02-25-1"></a>
#### 機能追加
* [Console] 
	* クーポン > クーポン発行: 発行したクーポンを設定したストアでのみ使用できる機能を追加

<a id="2020-02-25-2"></a>
#### 機能改善/変更
* [SDK] 2.7.1
	* (Common) Guestでログイン後にGetAuthProviderUserIDを呼び出すと値を返すように修正
* [Console]
	* アプリ > アプリ: 同一クライアントバージョン削除後に再登録する際の通知ロジックを追加
	* 購入(IAP) > Item: 登録時に定期購入商品登録のための登録フィールド値を追加(App Store - Shared secret、Google store - Domain authentication File Names)

<a id="2020-02-25-3"></a>
#### バグ修正
* [Console]
	* Analytics > リアルタイムモニタリング > リアルタイム指標: 断続的にPush送信後にccu項目に空の値またはinfinityと表示される現象を修正
	* Analytics > 送信指標
		* グリッドにデータがあった後になくなった場合にNo Dataに更新されないバグを修正
		* フィルター名が短い場合にボタンの配置が縦に表示される現象を修正

<a id="2020-02-11"></a>
### 2020. 02. 11. { #2020-02-11 }

<a id="2020-02-11-1"></a>
#### 機能追加
* [Console] 
	* Analytics > 利用者指標 > Life Cycle メニューを新規オープン。プロジェクト作成から利用者指標の流れをグラフで一目で把握できる機能を提供します。
	* 管理 > 権限：ウィークリーレポート受信権限項目を追加
		* 実際の「ウィークリーレポート」メールは3月から送信される予定です。

<a id="2020-02-11-2"></a>
#### 機能改善/変更
* [サーバーAPI] 退会API呼び出し時に regUser の長さに関するバリデーションを追加
* [Console] 
	* Analytics：Grid、Chartに日本語フォントを適用
	* 購入：エラー発生時に表示されるポップアップメッセージをユーザーが直感的に把握できるよう改善

<a id="2020-02-11-3"></a>
#### バグ修正
* [Console]
	* Analytics：日本語に言語変更した際、通貨が「円（JPY）」と表示されていたのを「ウォン（KRW）」と表示されるよう修正

<a id="2020-01-21"></a>
### 2020. 01. 21. { #2020-01-21 }

<a id="2020-01-21-1"></a>
#### 機能追加
* [SDK] 2.7.0
	* (Unity) NaverCafePLUG のサポートを追加

<a id="2020-01-21-2"></a>
#### バグ修正
* [SDK] 2.7.0
	* (Android) サーバーレスポンスに traceError の必須パラメーターがない場合でもクラッシュが発生しないよう修正
	* (Android) Firebase 設定が欠落している場合に例外が発生しないよう修正
	* (Unity) Web ログイン時に gamebase://dismiss スキームの処理を追加
	* (Unity) リリースビルド時に Webview が間欠的に表示されない問題を修正
* [Console]
	* Analytics：ユーザーセッション満了時にログインページへリダイレクトされない現象を修正

<a id="2020-01-14"></a>
### 2020. 01. 14. { #2020-01-14 }

<a id="2020-01-14-1"></a>
#### 機能追加
* [サーバーAPI] ユーザー退会APIを追加

<a id="2020-01-14-2"></a>
#### 機能改善/変更
* [SDK] 2.6.3
	* (Unity) Standalone Webview の改善：CefWebview を更新
	* (Unity) ログイン後にエラーが発生して欠落していた .dll ファイルを追加
		* ToastCommon.dll、vcruntime140.dll

<a id="2020-01-14-3"></a>
#### バグ修正
* [SDK] 2.6.3
	* (Unity) Login(CredentialInfo) API 呼び出し時にエラーが発生していた問題を修正

<a id="2019-12-24"></a>
### 2019. 12. 24. { #2019-12-24 }

<a id="2019-12-24-1"></a>
#### 機能追加
* クーポン > クーポン発行：キーワードクーポン機能を追加

<a id="2019-12-24-2"></a>
#### 機能改善/変更
* [Console]
	* 購入 > 決済情報照会：追加情報カラムを追加
* [SDK] 2.6.2
	* (共通) TOAST SDK を更新：Android(0.19.4)、iOS(0.20.1)、Unity(0.18.0)
	* (iOS) Naver SDK バージョンを更新 (4.1.0)

<a id="2019-12-10"></a>
### 2019. 12. 10. { #2019-12-10 }

<a id="2019-12-10-1"></a>
#### 機能追加
* アプリ > アプリ: メンテナンス中にQAテスト端末を登録する際、IPアドレスでも登録できる機能を追加

<a id="2019-12-10-2"></a>
#### バグ修正
* [Console]
	* 意味が合わない日本語の文言を修正
* [SDK] 2.6.1
	* (Android) Gamebase.initialize() を呼び出す前に Gamebase.login() を呼び出した際にクラッシュが発生する問題を修正
	* (Android) TOAST Analytics User Data を Java のアドレス値として誤って送信する問題を修正
	* (Android) IAP 商品を有効化していない場合に発生するクラッシュを修正
	* (iOS) AddMapping（強制、Forcibly）使用時にマッピングされない問題を修正
	* (iOS) Unity Plugin で PushConfiguration の displayLanguageCode を設定しない場合、NSNull オブジェクトによりクラッシュが発生する問題を修正

<a id="2019-11-26"></a>
### 2019. 11. 26. { #2019-11-26 }

<a id="2019-11-26-1"></a>
#### バグ修正
* [Console]
	* クーポン > クーポン発行: セッション期限切れ後にクーポンをダウンロードした際、異常なファイルとしてダウンロードされる問題を修正
	* Analytics > リアルタイムモニタリング > ダッシュボード: 前日のデータが 0 で表示される現象を修正
	* TOAST 商品（IAP、Push、AppGuard など）関連メニューへのアクセス時に商品が無効化されている場合、無効化ページが正常に表示されない問題を修正

<a id="2019-11-20"></a>
### 2019. 11. 20. { #2019-11-20 }

<a id="2019-11-20-1"></a>
#### バグ修正
* [SDK] 2.6.1
	* (Unity) iOS Plugin ファイルがパッケージに含まれておらず iOS ビルド時にエラーが発生するため、該当ファイルを追加: 'toast_sdk_wrap.m'
	* (Unity) UnityEditor で Standalone 以外のプラットフォームで実行した際、Store Code が Empty として入力され初期化に失敗するエラーを修正
	* (Unity) Initialize API 内部の zone type 処理部分のエラーにより NullReferenceException が発生するエラーを修正

<a id="2019-11-13"></a>
### 2019. 11. 13. { #2019-11-13 }

<a id="2019-11-13-1"></a>
#### バグ修正
* GamebaseSettingTool
	* Gamebase v2.6.0 アップデート時にファイルが正常に変更されないエラーを修正

<a id="2019-11-12"></a>
### 2019. 11. 12. { #2019-11-12 }

```
Gamebase SDK 2.6.0 未満のバージョンから 2.6.0 にアップグレードする場合、必ず Upgrade Guide ドキュメントに記載されている変更内容を反映してください。
ガイドの場所: Game > Gamebase > Upgrade Guide
```

<a id="2019-11-12-1"></a>
#### 機能追加
* クーポンサービス新規オープン：クーポンを大量に作成・管理する機能
	* [Console] Coupon メニュー新規オープン
	* [Server API] クーポン確認および消費 API を追加
* 自動決済不正行為機能を追加
	* [Console] 購入 (IAP) > 決済不正行為モニタリング メニュー新規オープン
		* 決済不正行為自動制裁設定機能
		* 決済不正行為条件を検索後、手動で利用停止が可能
* Google 定期購入機能を追加
	* [SDK] 2.6.0 Android
* [SDK] 2.6.0
	* (共通) データを Log&Crash に送信してさまざまな分析に利用できるよう TOAST Logger を追加
	* (iOS) Sign In with Apple 認証を追加
	* (Android) Gamebase Android SDK は Bintray を通じて配布されるため、gradle 設定のみで Gamebase を使用できます

<a id="2019-11-12-2"></a>
#### 機能改善/変更
* [SDK] 2.6.0
	* (Unity) ログイン時に LaunchingStatus を更新するロジックに誤りがあったため修正
	* (Unity) Debug Log を送信する機能を Gamebase コンソールで設定した場合、クライアントでログ送信が無限に繰り返されるエラーを修正
* [Console]
	* アプリ > アプリ：サーバーアドレスをサービス状態別（テスト、審査中、サービス）に入力できるよう変更
	* 購入 (IAP) > 決済情報：検索条件を選択して検索できるよう UI を変更

<a id="2019-10-29"></a>
### 2019. 10. 29. { #2019-10-29 }

<a id="2019-10-29-1"></a>
#### 機能改善/変更
* [Console]
	* Analytics：パイチャートツールチップを変更
	* Analytics > リアルタイムモニタリング：Push 送信対象の追加作業

<a id="2019-10-15"></a>
### 2019. 10. 15. { #2019-10-15 }

<a id="2019-10-15-1"></a>
#### 機能改善/変更
* [SDK] 2.5.2
	* (iOS) UIWebView を WKWebView に置き換え
* Sample App
	* Gamebase SDK アップデート(v2.4.0)
	* Smart Downloader 適用(v1.5.8)、Leaderboard 適用
	* 機能追加: ゲームリソースのダウンロード、Leaderboard、TAA 指標連携、端末移行機能、強制マッピング機能
	* 改善/変更: ServerPush リスナー追加、Observer メンテナンス検知追加
	* ゲームリニューアル
		
<a id="2019-10-15-2"></a>
#### バグ修正
* [Console]	
	* 管理 > 権限: 権限の変更が正常に行われない問題を修正
	* モバイル
		* Datepicker 選択時にキーボードが有効化される現象を修正
		* Analytics: ARPPU 項目に NRU 値が表示される現象を修正
		
<a id="2019-09-24"></a>
### 2019. 09. 24. { #2019-09-24 }

<a id="2019-09-24-1"></a>
#### 機能改善/変更
* [Console]
	* アプリ > クライアント: Web クライアント登録時もストアを選択して登録できるよう UI を修正
		
<a id="2019-09-24-2"></a>
#### バグ修正
* [Console]	
	* 管理 > 権限: 権限の変更が正常に行われない問題を修正
	* モバイル
		* Datepicker 選択時にキーボードが有効化される現象を修正
		* Analytics: ARPPU 項目に NRU 値が表示されていた現象を修正
	
<a id="game-gamebase-1"></a>
### 2019. 09. 10. { #game-gamebase-1 }

<a id="game-gamebase-1-1"></a>
#### 機能追加
* [Console]
	* Analytics: チャンネル/ワールド/サーバー、職業/クラスの転送指標にレベル指標を追加表示
	
<a id="game-gamebase-1-2"></a>
#### 機能改善/変更
* [Console]
	* Analytics: グリッドレンダリングのパフォーマンス改善(tui-grid 4.4.x 適用)
* [SDK] 2.5.1
	* (iOS) GamebasePushAdapter で使用中の TCPushSDK を 1.7.0 にアップデート
		* TCPushSDK が Static Library から Framework ファイルに変更されたため、プロジェクトに TCPushSDK.framework を追加する必要があります。
	
<a id="game-gamebase-2"></a>
### 2019. 08. 27. { #game-gamebase-2 }

<a id="game-gamebase-2-1"></a>
#### 機能追加
* [SDK] 2.5.0
	* Console で入力した CS URL をウェブビューで開く API を提供
	
<a id="game-gamebase-2-2"></a>
#### 機能改善/変更
* [Console]
	* Analytics: チャートのパフォーマンス改善

<a id="game-gamebase-2-3"></a>
#### バグ修正
* [SDK] Setting Tool 1.4.3
	* Script ファイルの位置を Editor フォルダ配下に移動してビルドエラーを解決
	* Mac OS で Multilanguage に Language ファイルのフルパスを指定すると動作しない問題を修正
	
<a id="game-gamebase-3"></a>
### 2019. 08. 02. { #game-gamebase-3 }

<a id="game-gamebase-3-1"></a>
#### バグ修正
* [SDK] Setting Tool 1.4.3
	* Script ファイルの位置を Editor フォルダ配下に移動してビルドエラーを解決
	* Mac OS で Multilanguage に Language ファイルのフルパスを指定すると動作しない問題を修正

<a id="game-gamebase-4"></a>
### 2019. 07. 23. { #game-gamebase-4 }

<a id="game-gamebase-4-1"></a>
#### 機能追加
* [Console]
	* メンバー > ダウンロード 新規メニュー公開: 登録日、最終ログイン日時を基準にゲームユーザーの一覧を照会し、ファイルとしてダウンロードできます。

<a id="game-gamebase-4-2"></a>
#### 機能改善/変更
* [Console] モバイル
	* メンテナンス、Pushの登録と修正が可能です。
* [SDK] 2.4.4
	* (共通) メンバーエラーコードのフォーマットを変更しました。
	* (Unity) GamebaseServerPushType にキーを追加しました (TRANSFER_KICKOUT)。
* Setting Tool
	* フォルダー構造を変更しました: `既存のSettingToolを完全に削除してから再インストールしてください。`
	* 多言語処理サポートを追加しました。

<a id="game-gamebase-4-3"></a>
#### バグ修正
* [Console]
	* Analytics > 利用者指標: チャートのX軸の日付が重なる問題を修正しました。

<a id="game-gamebase-4-4"></a>
#### バグ修正
* [Console]
	* Analytics > 利用者指標: チャートのX軸の日付が重なる問題を修正しました。

<a id="game-gamebase-5"></a>
### 2019. 07. 11. { #game-gamebase-5 }

<a id="game-gamebase-5-1"></a>
#### 機能改善/変更
* [Console] Analytics
	* レベルアップクエリのパフォーマンスを改善しました。
	* チャート内に min、max 情報を表示するようにしました。
	* 多言語処理を適用しました（中国語）。

<a id="game-gamebase-5-2"></a>
#### バグ修正
* [SDK] 2.4.3
	* (iOS) 認証試行時にエラーが発生した場合、フォーマットに合わないエラーメッセージのパースを試みることでクラッシュが発生する問題を修正しました。
	* (Unity) iOS および Android 向けビルド時に AddMappingForcibly API が動作しないエラーを修正しました。
	* (Unity) RequestRetryTransaction API 呼び出し時に iOSPlugin で JSON パースエラーが発生する問題を修正しました。

<a id="game-gamebase-6"></a>
### 2019. 07. 01. { #game-gamebase-6 }

<a id="game-gamebase-6-1"></a>
#### バグ修正
* [Console]
	* 管理 > アラーム: Webhook 設定後にアラーム設定値の修正が失敗する現象を修正しました。

<a id="game-gamebase-7"></a>
### 2019. 06. 27. { #game-gamebase-7 }

<a id="game-gamebase-7-1"></a>
#### バグ修正
* [Console]
	* 利用停止: 利用停止の一括登録用ファイルのアップロードが失敗する現象を修正しました。
* [SDK] Setting Tool 1.4.1
	* GamebaseSettingTool 実行時に既存の設定情報を取得できないエラーが発生する問題を修正しました。

<a id="game-gamebase-8"></a>
### 2019. 06. 25. { #game-gamebase-8 }

<a id="game-gamebase-8-1"></a>
#### 機能追加
* 転送指標機能の追加
    * [Console]Analytics > 転送指標: Level、Channel、Class 別の指標確認メニューを公開
		* リアルタイム状況
		* レベル別状況
		* ワールド/サーバー/チャンネル別状況
		* クラス/職業別状況
		* レベルアップ
		* アイテム販売状況
		* アイテム販売 TOP 50

<a id="game-gamebase-8-2"></a>
#### 機能改善/変更
* [SDK] 2.4.2
	* (共通) LaunchingInfo に JSON string 形式の TOAST Launching 情報を追加
	* (iOS) LINE SDK アップデート (v5.0.1)
		* LINE Adapter の最小サポート OS バージョンが iOS 10 に変更
		* LINE アプリを通じたログイン機能を追加

<a id="game-gamebase-8-3"></a>
#### バグ修正
* [SDK] 2.4.2
	* (共通) Analytics バグ修正: ログアウト、退会、アカウント移行時に保存された指標データを初期化するように修正
	* (iOS) ネットワーク接続の問題により断続的にクラッシュが発生していた現象を修正

<a id="game-gamebase-9"></a>
### 2019. 06. 13. { #game-gamebase-9 }

<a id="game-gamebase-9-1"></a>
#### バグ修正
* [SDK] 2.4.1
	* (iOS) Analytics 指標送信時に一部のパラメーターが欠落し、指標が正常に出力されないバグを修正
	
<a id="game-gamebase-10"></a>
### 2019. 05. 28. { #game-gamebase-10 }

<a id="game-gamebase-10-1"></a>
#### 機能追加
* HANGAME mix 日本決済追加
    * [SDK] 2.4.0
    	* (Unity)Standalone 日本外部決済追加
    	* (Unity)Standalone 日本 HANGAME 認証追加
    * [Console] 
    	* 購入 > ストア: 「HANGAME mix(JAPAN)」ストア追加
    	* アプリ > クライアント: Windows クライアント登録時のストア設定項目追加
    	* アプリ > インストールURL: Windo インストール URL 追加時のストア設定項目追加

<a id="game-gamebase-10-2"></a>
#### 機能改善/変更
* [SDK] 2.4.0
	* (共通) 指標関連 Class 変更
        * LevelUpData Class: userLevel、levelUpTime パラメータが必須に変更 / その他フィールド削除 [詳細 [Android](http://docs.toast.com/ko/Game/Gamebase/ko/aos-etc/#game-user-data-settings) / [iOS](http://docs.toast.com/ko/Game/Gamebase/ko/ios-etc/#game-user-data-settings) / [Unity](http://docs.toast.com/ko/Game/Gamebase/ko/unity-etc/#game-user-data-settings) / [JavaScript](http://docs.toast.com/ko/Game/Gamebase/ko/js-etc/#game-user-data-settings)]
        * GameUserData Class: classId(ゲームユーザーの職業) フィールド追加 [詳細 [Android](http://docs.toast.com/ko/Game/Gamebase/ko/aos-etc/#level-up-trace) / [iOS](http://docs.toast.com/ko/Game/Gamebase/ko/ios-etc/#level-up-trace) / [Unity](http://docs.toast.com/ko/Game/Gamebase/ko/unity-etc/#level-up-trace) / [JavaScript](http://docs.toast.com/ko/Game/Gamebase/ko/js-etc/#level-up-trace)]
    * (Android)Naver SDK バージョンアップデート (v4.2.5): Naver SDK バグ修正 (Naver ログイン中にアプリアイコンからアプリを再起動した場合、Activity が強制終了されることで認証プロセスが中断される問題を解決)
    * (Unity)StandaloneWebview が 32bit ビルドをサポートしました (SDK サイズが 53.6MB から 99.2MB に増加)
* [Server]
    * LTV クエリ修正および failover ロジック修正
* [Console]
    * LTV Grid ComplexColumns サポートおよび Excel ダウンロードのサポート

<a id="game-gamebase-11"></a>
### 2019. 05. 16. { #game-gamebase-11 }

<a id="game-gamebase-11-1"></a>
#### 機能追加
* [Console]
	* 端末移行機能追加（新規メニュー）
		* アプリ > 機器移行（Transfer account）：機器移行機能を使用するための設定値を保存します
		* 会員 > 機器移行：発行されたキーのステータスおよび履歴を照会します

<a id="game-gamebase-11-2"></a>
#### 機能改善/変更
* [Console]
	* アプリ：AppleGameCenter、China IdP に「トークン再検証」オプション Off
		* 該当 IdP では実際の外部 IdP チェックを行わず内部トークンのみチェックするため、意味のないオプションとして削除
	* 会員：マッピング追加可能な条件の変更
		* 変更前：Guestアカウント
		* 変更後：Guestアカウント、Missingアカウント

<a id="game-gamebase-11-3"></a>
#### バグ修正
* [SDK] 2.3.1
	* (Android) 2.3.0 バージョンで Twitter ログインができなかった問題を修正
* [Console]
	* 会員：購入履歴で領収書検証ができなかった問題を修正
	* Kickout：照会リクエスト時に認証チェックを追加し、異常動作していたイシューを修正
	
<a id="game-gamebase-12"></a>
### 2019. 04. 23. { #game-gamebase-12 }

```
Gamebaseを使用すると、50以上の中国ストアとの連携が可能です。
中国でのリリースにご興味がある場合は、カスタマーセンターまでお問い合わせください。
```

<a id="game-gamebase-12-1"></a>
#### 機能追加
* [SDK] 2.3.0
	* (Android/Unity) 中国ストアの認証/決済を追加

<a id="game-gamebase-12-2"></a>
#### 機能改善/変更
* [SDK] 2.3.0
	* (共通) Launching Status Code を追加: 「審査中 (204)」、「テスト中 (203)」
	* (Android) 最近ログインした Provider でログインし、WebSocket の応答失敗を受け取った場合 (Timeout、network disable など)、AuthToken を削除しないように修正
	* (Android) IdP ログイン時に AuthAdapter 内部で発生する MemoryLeak を修正

<a id="game-gamebase-13"></a>
### 2019. 04. 11. { #game-gamebase-13 }

<a id="game-gamebase-13-1"></a>
#### 機能改善/変更
* [SDK] 2.2.2
	* (Unity) SDK ログを改善
* [Console]
	* Analytics メニューの多言語処理を適用
	* セキュリティ審査関連の脆弱性パッチ

<a id="game-gamebase-13-2"></a>
#### バグ修正
* [SDK] 2.2.2
	* (Android) Gamebase 初期化前に TransferAccount API を呼び出した際、コールバックが返ってこない問題を修正
	* (iOS) showBlockingPopup を NO に設定した場合、Gamebase 初期化コールバックが呼び出されない問題を修正
	* (Unity) AddMappingForcibly API を呼び出すとクラッシュが発生する問題を修正

<a id="game-gamebase-14"></a>
### 2019. 04. 02. { #game-gamebase-14 }

<a id="game-gamebase-14-1"></a>
#### バグ修正
* [SDK] 2.2.1
	* (Unity) Unity Editor で Android プラットフォームを選択してプレイすると、初期化時にサーバーでエラーが発生する問題を修正

<a id="game-gamebase-15"></a>
### 2019. 03. 26. { #game-gamebase-15 }

<a id="game-gamebase-15-1"></a>
#### 機能追加
* TransferAccount 機能追加：ゲストユーザーがマッピングなしで最大 2 つのキーを使用して新しいデバイスに移行できる機能
    - (SDK共通) 追加された API 
		* TransferAccountInfo 発行 API (issueTransferAccount)
		* 発行された TransferAccountInfo を使用してアカウント移行をリクエストする API (transferAccountWithIdPLogin)
		* 発行された TransferAccountInfo を確認する API (queryTransferAccount)
		* すでに発行された TransferAccountInfo を更新する API (renewTransferAccount)		
	- (Server API)
		* 発行された TransferAccount の ID/PW を検証するサーバー API (validateTransferAccount)
    - (console) 会員メニューのマッピング履歴照会タブで Transfer 履歴を確認できます
* 強制マッピング機能追加：すでに他のアカウントに連携されている IdP アカウントをマッピングできる機能
	- (SDK共通) 追加された API 
		* 強制マッピングする API (addMappingForcibly)

<a id="game-gamebase-15-2"></a>
#### 機能改善/変更
* [SDK] 2.2.0
	* (Android) IAP SDK バージョンを最新バージョンの v1.5.3 にアップデート
	* (iOS) LINE SDK の App ログイン機能が無効化
		* LINE SDK v4 のバグにより iOS 12 でアプリログインが失敗する問題があったため、Gamebase Line Adapter で Web ログインのみをサポートするよう変更
	* (Unity) GamebaseMainActivity の Package Name が変更
		* com.toast.gamebase.activity.GamebaseMainActivity -> com.toast.android.gamebase.activity.GamebaseMainActivity

<a id="game-gamebase-16"></a>
### 2019. 02. 26. { #game-gamebase-16 }

<a id="game-gamebase-16-1"></a>
#### 機能改善/変更
* [SDK] 2.1.0
	* (共通) TransferKey API 削除
		* issueTransferKey : TransferKey 発行
		* requestTransfer : TransferKey 検証
		
<a id="game-gamebase-16-2"></a>
#### バグ修正
* [SDK] 2.1.0
	* (Android) Gamebase 初期化前に onActivityResult() が呼び出されて異常動作していたバグを修正
	* (iOS) Gamecenter を Gamebase 以外のロジックでログインした後、Gamebase を通じて Gamecenter ログインを試みる際に反応がないバグを修正

<a id="game-gamebase-17"></a>
### 2019. 01. 29. { #game-gamebase-17 }

```
Gamebase 2.0 の改善された全体指標を活用するには、SDK のアップデートが必要です。
```

<a id="game-gamebase-17-1"></a>
#### 機能追加
* Console
	* Analytics : Gamebase 2.0 指標の新規公開
	* アプリ : クライアントのデバッグログをリアルタイムで変更できる機能を追加
* [SDK] 2.0.0
	* (共通) Custom 指標のための API を追加 (購入成功の場合は SDK 内部で自動送信)
		* setGameUserData : ゲームログイン後にユーザーレベル情報を送信
		* traceLevelUpData : レベルアップ追跡のため、ゲームユーザーがレベルアップしたときに呼び出し
    * (JavaScript) SDK の新規リリース

<a id="game-gamebase-17-2"></a>
#### 機能改善/変更
* [SDK] 2.0.0
	* (Android) Push SDK の更新 (android:1.7.0)
	* (Android) Adapter API の変更
		* Launching 情報の伝達
		* logout、withdraw API へのコールバック追加
	* (iOS) IAP SDK の更新
		* 決済失敗時に断続的にクラッシュが発生していた現象を修正

<a id="game-gamebase-17-3"></a>
#### バグ修正
* [SDK] 2.0.0
	* (iOS) iOS 12 以上のシミュレーターで debugMode On 状態で Gamebase 初期化時にクラッシュが発生していた現象を修正

<a id="game-gamebase-18"></a>
### 2018. 12. 27. { #game-gamebase-18 }

<a id="game-gamebase-18-1"></a>
#### 機能追加
* Console
	* Push - 繰り返し送信機能を追加

<a id="game-gamebase-18-2"></a>
#### 機能改善/変更
* [SDK] 1.14.5
	* deprecated されていた以下の API が削除されました。
		* (void)Gamebase.WebView.showWebBrowser(Activity, String)
		* (void)Gamebase.Network.addOnChangedListener(NetworkManager.OnChangedListener)
		* (void)Gamebase.Network.removeOnChangedListener(NetworkManager.OnChangedListener)
		* (void)Gamebase.Launching.addOnUpdatedListener(LaunchingOnUpdateListener)
		* (void)Gamebase.Launching.removeOnUpdatedListener(LaunchingOnUpdateListener)
	* 決済モジュール(gamebase-adapter-purchase-iap)が修正されました。
		* IAP SDK を 1.5.2 にアップデート
		* Client では使用されない IAP TEST Store を削除
		* 決済再処理ロジック(requestRetryTransaction)でデータが不完全なときに呼び出しが失敗する問題を修正
		* クラッシュを防ぐため、すべての IAP SDK 呼び出し箇所に例外処理を追加
* Console
	* 認証が解除されたときに Rest API リクエストでもログインページに移動するよう修正
	* IAP Transaction 照会フィルターを追加

<a id="game-gamebase-19"></a>
### 2018. 11. 15. { #game-gamebase-19 }

<a id="game-gamebase-19-1"></a>
#### 機能追加
* Console
	* 中国語を適用
	* 会員：購入履歴にApp Store領収書検証機能を追加

<a id="game-gamebase-19-2"></a>
#### 機能改善/変更
* Console
	* カレンダーの多言語サポートを追加
* [SDK] 1.14.2
	* (Android) メンテナンス時、データ構造においてメンテナンス開始/終了時間を意味するepoch timeのタイプを既存のStringからlongに変更：既存のGamebase Unityと連携後にメンテナンス呼び出し時にタイプ不一致によりコールバックが返ってこない現象による修正
	* (iOS) Provider Profileの取得メソッド呼び出し時、返却されるTCGBAuthProviderProfileオブジェクトのdescriptionメソッドのJSON文字列構造が変更されたことにより、Gamebase iOS SDK 1.14.0とUnity Plugin 1.14.0適用時にクラッシュが発生する可能性がある構造を修正

<a id="game-gamebase-19-3"></a>
#### バグ修正
* Console
	* Push：特定対象への送信後に登録されたPush項目をコピーして登録する際に登録が失敗していた問題を修正
* [SDK] 1.14.2
	* (Android) エミュレーター環境でストアアプリ（PlayStore、OneStoreなど）がない状態で「アプリのインストール/アップデート」時にストア未確認によるクラッシュバグを修正
	* (Unity) ShowWebView API呼び出し時にパラメータにCallbackを設定しない場合にクラッシュが発生する問題を修正
	* (Unity) iOS SDKの削除済みAPIを呼び出すコードがあり、コンパイル時にエラーが発生するバグを修正

<a id="game-gamebase-20"></a>
### 2018. 10. 23. { #game-gamebase-20 }

<a id="game-gamebase-20-1"></a>
#### 機能追加
* Console
	* IAP：決済情報メニューにApp Store領収書検証機能を追加
* [SDK] 1.14.0
	* (共通) Gamebase Webviewにファイル添付機能を追加：Android の API 19、Kitcat では正常に動作しません。
	
<a id="game-gamebase-20-2"></a>
#### 機能改善/変更
* Console
	* IAP：決済情報メニューの決済履歴ダウンロードの検索条件を改善（1日→30日）
* [SDK] 1.14.0
	* (共通) 利用停止/メンテナンスについて、ユーザーがコンソールに作成したメッセージをURLエンコードして送信し、クライアントでデコードして処理するように修正
	* (iOS) Payco SDKのバージョンを1.2.4に更新
	* (Unity) GamebaseSDKSettingオブジェクトがあるシーンに戻る際、オブジェクトが重複して生成されないように改善
	* Remove API : Webview, Network, Launching
		* (Android) 5件
			- (void)Gamebase.WebView.showWebBrowser(Activity, String)
			- (void)Gamebase.Network.addOnChangedListener(NetworkManager.OnChangedListener)
			- (void)Gamebase.Network.removeOnChangedListener(NetworkManager.OnChangedListener)
			- (void)Gamebase.Launching.addOnUpdatedListener(LaunchingOnUpdateListener)
			- (void)Gamebase.Launching.removeOnUpdatedListener(LaunchingOnUpdateListener)
		* (iOS) 9件
			- [TCGBUtil showToastWithMessage:duration:]
			- [TCGBWebView showWebBrowserWithURL:viewController:]
			- [TCGBWebView showWebViewWithURL:viewController:configuration:]
			- [TCGBLaunching addObserverOnChangedStatusNotification:]
			- [TCGBLaunching removeObserverOnChangedStatusNotification:]
			- [TCGBLaunching addUpdateStatusNotification]
			- [TCGBLaunching removeUpdateStatusNotification]
			- [TCGBNetwork addObserverOnChangedNetworkStatusWithHandler:]
			- [TCGBNetwork removeObserverOnChangedNetworkStatusWithHandler:]
		* (Unity) 7件
			- ShowWebBrowser(string url)
			- ShowWebView(GamebaseRequest.Webview.GamebaseWebViewConfiguration configuration)
			- ShowToast(string message, int duration)
			- AddUpdateStatusListener(GamebaseCallback.DataDelegate<GamebaseResponse.Launching.LaunchingStatus> callback) 
			- RemoveUpdateStatusListener(GamebaseCallback.DataDelegate<GamebaseResponse.Launching.LaunchingStatus> callback)
			- AddOnChangedStatusListener(GamebaseCallback.DataDelegate<GamebaseNetworkType> callback)
			- RemoveOnChangedStatusListener(GamebaseCallback.DataDelegate<GamebaseNetworkType> callback)
			
	* Deprecated  API 
		* (Android) 2件
			- (void)Gamebase.WebView.showWebView(Activity, String)
			- (void)Gamebase.WebView.showWebView(Activity, String, GamebaseWebViewConfiguration)
		* (iOS) 1件
			- [TCGBGamebase languageCode]
		* (Unity) 1件
			- GetLanguageCode()
* [SDK] Setting Tool		
	* ポップアップおよびUIの改善
	
<a id="game-gamebase-20-3"></a>
#### バグ修正
* [SDK] 1.14.1
	* (Android) Auth API呼び出し後、コールバックでAuth APIを重複呼び出した際に正常に呼び出されないバグを修正
	
<a id="game-gamebase-21"></a>
### 2018. 10. 11. { #game-gamebase-21 }

<a id="game-gamebase-21-1"></a>
#### バグ修正
* Console
	* 利用停止：一括登録時に発生していたエラーを修正
	
<a id="game-gamebase-22"></a>
### 2018. 09. 20. { #game-gamebase-22 }

<a id="game-gamebase-22-1"></a>
#### バグ修正
* Console
	* 管理：ページアドレスのエラーによりアラームページの処理に失敗する問題を修正

<a id="game-gamebase-23"></a>
### 2018. 09. 13. { #game-gamebase-23 }

<a id="game-gamebase-23-1"></a>
#### 機能追加
* Console	
	* メンバー: アカウントの IdP 追加および削除機能を追加、IdP ID 検索機能を追加
	* Push: Push ステータス別に送信履歴を照会する機能を追加
* [SDK] 1.13.0
	* (iOS) App Store Promotion IAP をサポートするための API を追加


<a id="game-gamebase-23-2"></a>
#### 機能改善/変更
* [SDK] 1.13.0
	* (共通) IAP SDK 最新バージョン適用 (android:1.5.1、iOS:1.6.0)
	* (Android) Push API 呼び出し時、Gamebase の初期化/ログイン状態に応じて、呼び出し失敗時のエラーメッセージをより明確に改善
		* 初期化前の呼び出し : NOT_INITIALIZED(1)
		* 初期化後の呼び出し時に Push モジュールがない場合 : NOT_SUPPORTED(10)
		* 初期化成功およびログイン前の呼び出し : NOT_LOGGED_IN(2)		
	* (iOS) authProviderProfileWithIDPCode api の呼び出し結果の構造が 1 depth に変更されました (Android、Unity と統一)
	* (Unity) ログに表示する json データを見やすくするために出力フォーマットを改善
* Console
	* 利用停止 : AppGuard を使用した利用停止登録 UI を改善 - 機能オフ時のデータ初期化、Leaderboard データ削除設定を状態が「on」の場合にのみ表示するよう改善
	
<a id="game-gamebase-23-3"></a>
#### バグ修正
* [SDK] 1.13.0
	* (Android) NaverCafe SDK との競合により Naver ログイン時に発生していた問題を解決
	* (Unity) Unity 2017.2 以降のバージョンで Editor Play Mode 終了時、websocke close 処理で発生していたエラーを修正
* Console
	* App : 情報修正時に削除ボタンの後の内容が切れる現象を修正
		
<a id="game-gamebase-24"></a>
### 2018. 08. 28. { #game-gamebase-24 }

<a id="game-gamebase-24-1"></a>
#### 機能追加
* Console	
	* 会員：アカウント状態変更機能追加、Push Token 照会追加
	* 運営指標（ユーザー統計）：本日の退会者、当日登録後退会者指標追加

<a id="game-gamebase-24-2"></a>
#### 機能改善/変更
* [SDK] 1.12.2
	* (Android) WebSocket タイムアウト時（API 呼び出し時間経過）、クラッシュが発生する可能性があるバグに対して防御ロジックを追加
	* (iOS) Google Auth Adapter、Naver Auth Adapter の Callback URL Scheme 設定を改善
		* コンソールに「url_scheme_ios_only」の値を設定しない場合、Default URL Scheme が設定されるよう改善しました。Default URL Scheme を使用するには、XCode > Target > Info > URL Types に tcgb.{Bundle ID}.google または tcgb.{Bundle ID}.naver を登録する必要があります。
	* (iOS) Payco Auth Adapter を改善
		* URL Scheme が未設定のため、意図しない URL Scheme が呼び出されていた問題を修正しました。設定方法が変更されたため、アップデートには必ず URL Scheme の設定が必要です（XCode > Target > Info > URL Types に tcgb.{Bundle ID}.payco を登録）。
* Console
	* 会員：ID マッピング履歴照会機能追加（直近 3 か月照会 → 照会期間を直接設定するよう変更）
	* 購入（IAP）：決済情報の Excel ダウンロードを 1 日に制限、アイテム削除機能を削除
	
<a id="game-gamebase-24-3"></a>
#### バグ修正
* [SDK] 1.12.2
	* (Android) auth-twitter-adapter を含む状態で TargetSdk 28 でビルドした際に初期化エラーが発生する問題を修正

<a id="game-gamebase-25"></a>
### 2018. 08. 09. { #game-gamebase-25 }

<a id="game-gamebase-25-1"></a>
#### 機能改善/変更
* [SDK] 1.12.1
	* (共通) IAP SDK 最新バージョン適用 (1.5.0)
	* (共通) Gamebase メンテナンスページで、メンテナンス時間を端末の設定国時刻に合わせて表示するよう改善
	* (共通) メンテナンスページを外部ページとして使用する際に、コンソールに入力したメンテナンス情報を使用できる機能を追加
	* (共通) IdP にマッピングされたユーザーが Guest マッピングを試みた場合にエラーが発生します (TCGB_ERROR_AUTH_ADD_MAPPING_CANNOT_ADD_GUEST_IDP)
	* (共通) 認証 API 重複呼び出し時にエラーが発生します (AUTH_ALREADY_IN_PROGRESS_ERROR)
	* (Android) TencentPush SDK アップデート (3.2.3)
	* (Android) Onestore v17 (API v5) サポート: Gamebase では v16 (ストアコード=TS) は提供しません。
	* (iOS) エラーコード追加: Gamecenter ログイン拒否 (TCGB_ERROR_IOS_GAMECENTER_DENIED)
* [SDK] Setting Tool
	* フォルダ名変更: TOAST -> Toast
	* エラー発生時のポップアップ通知追加: File Download 失敗、File Extract 失敗、XML パース失敗

<a id="game-gamebase-25-2"></a>
#### バグ修正
* [SDK] 1.12.1
	* (iOS) Naver ログイン時にプロフィール情報の取得失敗によりログインできないバグを修正: プロフィール情報の取得に失敗してもログインは成功するよう変更
* Console
	* 決済履歴: 「Reserved」状態で決済ステータスが変更されないバグと、Excel ダウンロード時にフィルタリングが適用されない問題を修正

<a id="game-gamebase-26"></a>
### 2018. 07. 24. { #game-gamebase-26 }

<a id="game-gamebase-26-1"></a>
#### 機能の改善/変更
* [SDK] 1.12.0
	* (iOS)Gamebase 初期化時に Debug Log に使用中のアダプターのバージョン情報およびアプリのビルド情報を出力する機能を追加
	* (iOS)CocoaPods を通じて配布される Naver Auth Adapter に含まれていた Naver ID Login SDK のバイナリが削除され、依存関係の設定方式に変更
* Console
	* Web クライアント登録時に選択できるサービス状態に制限を適用: アップデート推奨、アップデート必須は選択不可
* [SDK] Setting Tool
	* Setting Tool の最新バージョンがある場合、アップデート通知機能を追加
	* 内部の null 例外を修正

<a id="game-gamebase-26-2"></a>
#### バグ修正
* [SDK] 1.12.0
	* (Unity)IssueTransferKey API 呼び出し時に例外が発生するバグを修正
	* (Unity)Unity Google Adapter を削除: 既存に GoogleAdapter を使用していた開発会社は、以下のアップデートガイドを参照してください

**Unity Google Adapter アップデートガイド**

* Unity SDK 1.6.0以上 1.11.0以上のバージョンを使用している場合、1.12.0バージョンにアップデートする前に、以下の内容を必ず確認する必要があります。(1.6.0未満のバージョンを使用している場合は、GoogleAdapter を使用していないため影響はありません。)
	1. Setting Tool の設定
        * GoogleAdapter が削除されたため、Unity タブに Google 項目が表示されなくなります。
        * Google 認証を使用する場合は、各プラットフォームタブで Google 項目を有効にします。
            * Android > Authentication > Google を選択して設定します
            * iOS > Authentication > Google を選択して設定します
    2. Gamebase Login API (変更なし)
        * Gamebase.Login(GamebaseAuthProvider.GOOGLE, callback);
    3. GPGS 機能を使用する場合
        * GPGS SDK for Unity を維持します
        * GPGS 関連のロジックはアプリで別途管理します
    4. GPGS 機能を使用しない場合
        * GPGS SDK for Unity を削除します

<a id="game-gamebase-27"></a>
### 2018. 07. 05. { #game-gamebase-27 }

<a id="game-gamebase-27-1"></a>
#### 機能追加
* Line IdP 追加 : iOS

<a id="game-gamebase-27-2"></a>
#### 機能改善/変更
* [SDK] 1.11.1
	* (共通) Guest ログイン後に AddMapping 成功時、loginForLastLoggedInPrivder を実行すると、AddMapping が成功した IdP アカウントを使用してログインするよう変更

<a id="game-gamebase-27-3"></a>
#### バグ修正
* [SDK] 1.11.1
	* (共通) メンテナンス解除後、後続の API 処理 (login/push/purchase など) が行われないバグを修正
	* (Android) Gamebase.addObserver() を通じて ObserverMessage を受信した場合、ObserverMessage.data.code の型が int ではなく String になるバグを修正
* Console
	* Windows クライアント登録時にストアコードが誤って登録される問題を修正

<a id="game-gamebase-28"></a>
### 2018. 06. 26. { #game-gamebase-28 }

<a id="game-gamebase-28-1"></a>
#### 機能追加
* iOS Google IdP 追加 : iOS
* Twitter IdP 追加 : Android、iOS
* Line IdP 追加 : Android のみ提供。iOS は 2018 年 7 月提供予定です。
* Server API 追加
	* getSimpleLaunching : クライアントアプリ起動時に提供される起動情報確認用 API

<a id="game-gamebase-28-2"></a>
#### 機能改善/変更
* [SDK] 1.11.0
	* (共通) LocalizedString 日本語翻訳追加
	* (共通) 認証 API 呼び出し時に初期化、ログインを行っていない場合に明確にエラーコードを区別できるよう内部ロジックを改善
	* (Android) 'android.permission.READ_PHONE_STATE' 権限を削除
	* (Android) GamebaseConfiguration.Builder の必須設定値である setAppId、setAppVersion をコンストラクタで入力できるよう変更
	* (Android) GamebaseConfiguration.Builder の setServerApiVerseion API を削除
	* (Android) getAuthBanInfo() API、class AuthBanInfo の名前を変更 : getBanInfo()、class BanInfo
	* Naver ID Login SDK 更新 : iOS(4.0.10)
* Sample App
	* ServerPush 機能および Observer 機能追加
	* Gamebase SDK 更新 : Android(1.9.0)、iOS(1.9.0)、Unity(1.10.1)

<a id="game-gamebase-29"></a>
### 2018. 06. 11. { #game-gamebase-29 }

<a id="game-gamebase-29-1"></a>
#### バグ修正
* [SDK] 1.10.1
	* (Unity) Unity Adapter がない場合に AddMapping API を呼び出すと内部的にログインとして処理されていたバグを修正

<a id="game-gamebase-30"></a>
### 2018. 06. 07. { #game-gamebase-30 }

<a id="game-gamebase-30-1"></a>
#### 機能追加
* [SDK] 1.10.0
	* (Unity) StandaloneWebviewAdapter: html ソースレンダリングをサポート

<a id="game-gamebase-30-2"></a>
#### 機能改善/変更
* [SDK] 1.10.0
	* (Unity) Unity Adapter のインターフェースを修正
		* v1.10.0 以上を使用する場合は、UnityAdapter のバージョンアップグレードが必要です (GamebaseUnitySDK_FacebookAdapter_v1.5.0、GamebaseUnitySDK_StandaloneWebviewAdapter_v1.7.0)
	* (Unity) Login API 呼び出し時に Unity Adapter がない場合、ネイティブ (Android/iOS) のログイン API を呼び出すようロジックを変更: facebook、Google
	* (Unity) 各 Adapter のフォルダ構造および名前のタイポを修正
		* パス: Assets/Gamebase/Scripts/Adapter => Assets/Gamebase/Adapter
		* タイポ: Adapater => Adapter

<a id="game-gamebase-31"></a>
### 2018. 05. 29. { #game-gamebase-31 }

<a id="game-gamebase-31-1"></a>
#### 機能追加
* [Console] 指標 (Operating indicator) のダウンロード機能を追加
	* モニタリング > 「同時接続変化」
	* ユーザー統計 > 「日別指標変化」
	* グループ同時接続 > 「日間グループ同時接続変化」


<a id="game-gamebase-31-2"></a>
#### バグ修正
* [SDK] 1.9.1
	* (iOS) Gamebase WebView の NavigationBar 領域にタイトル、戻るボタン、閉じるボタンが表示されない現象を修正

<a id="game-gamebase-32"></a>
### 2018. 05. 18. { #game-gamebase-32 }

<a id="game-gamebase-32-1"></a>
#### 機能改善/変更
* [SDK] 1.9.0
	* Unity SDK (1.9.0) の Google Adapter を新バージョン (1.6.2) に差し替えて再配布
    	- 5/3 に配布された Unity SDK (1.9.0) に適用された Google Adapter を最新バージョンに差し替えます (1.6.1 → 1.6.2)

<a id="game-gamebase-33"></a>
### 2018. 05. 03. { #game-gamebase-33 }

<a id="game-gamebase-33-1"></a>
#### 機能追加
* Transfer機能の追加
    - ゲストユーザーがマッピングなしで新しいデバイスに移行できる機能
    - (SDK共通)追加されたAPI 
		* Transfer Key発行API (IssueTransferKey)
		* 発行されたTransferKeyを使用してアカウント移行をリクエストするAPI (RequestTransfer)
    - (console)メンバーメニューのマッピング履歴照会タブでTransfer履歴の確認が可能
* 利用停止登録時にユーザーのリーダーボード（ランキング）データを削除できるオプションを追加（TOAST Leaderboardを使用する場合に限る）
    - 利用停止登録メニューを利用するか、App Guard連携ページで使用可能

<a id="game-gamebase-33-2"></a>
#### バグ修正
* [SDK] 1.9.0
	* (iOS) Naverアカウントを使用したログイン中にApp to Webログインを試みた際、サーバーから受け取ったSchemeの形式が変更され、ログインできない現象を修正
    * (iOS) AdapterからUnderlyingErrorオブジェクトを受け取り、ユーザーに渡されるエラーオブジェクトを生成するロジックで、メッセージおよびUnderlying Errorが設定されないバグを修正
    * (Android) Heartbeatで不正なユーザーと判定された場合に利用停止ポップアップが表示されないよう修正（iOSと同じロジックに修正）

<a id="game-gamebase-34"></a>
### 2018. 04. 12. { #game-gamebase-34 }

<a id="game-gamebase-34-1"></a>
#### バグ修正
* [SDK] 1.8.1
	* (Android. iOS) registerPush呼び出し時にdisplayLanguageCodeにnullを渡すとregisterPushが失敗するバグを修正

<a id="game-gamebase-35"></a>
### 2018. 04. 09. { #game-gamebase-35 }

<a id="game-gamebase-35-1"></a>
#### バグ修正
* [SDK] 1.8.1
	* (Unity) UnityAndroidプラットフォームで以下の機能を使用した際、モジュールの初期化が行われずNullReferenceExceptionが発生する問題を修正
		* Launching、Purchase、Push、Util、Webview

<a id="game-gamebase-36"></a>
### 2018. 04. 05. { #game-gamebase-36 }

<a id="game-gamebase-36-1"></a>
#### 機能追加
* Kick out 機能追加
    - 現在ゲーム中の全ユーザーの接続を切断する機能（メンテナンス時にゲームの全ユーザーの接続を切断したい場合に使用できます）
    - (console) メニュー追加
    - (SDK 共通) kick out イベントを受け取れる API 追加
* メンテナンスウェブページを、ユーザーが Console で入力した HTML ページとして使用できるよう機能を改善
    - 以前は Gamebase が提供するウェブページまたは外部ウェブページへの接続のみ可能でした
    - ウェブサーバーがない場合でも、メンテナンスページをユーザーが希望する形式で作成できます
* Observer 機能開発および API 追加
    - (SDK 共通) メンテナンスなどのアプリステータス / ネットワーク状態 / ユーザー状態（利用停止）の変更に対する Listener を Observer 登録を通じて一括処理できるよう API 追加

<a id="game-gamebase-36-2"></a>
#### 機能改善/変更
* [SDK] 1.8.0
	* (共通) Observer 機能追加に伴い、次の API を Deprecated: LaunchingStatus Listener、Network Listener（既存ユーザーは引き続き使用可能）
	* (iOS) ペイコ簡易ログイン 3rd SDK v1.2.2 適用: ログイン成功時にトークン有効期限情報（expires_in）を提供、iPhoneX ログイン UI 改善
	* (iOS) iPhoneX 対応のため、Webview 使用インターフェースを修正

<a id="game-gamebase-36-3"></a>
#### バグ修正
* 国コード（country code）が 10 文字以上の場合、同時接続データが保存されない現象を修正
* [SDK] 1.8.0
	* (Setting Tool) Unity Facebook Adapter をチェックするとエラーが発生するバグを修正

<a id="game-gamebase-37"></a>
### 2018. 03. 13. { #game-gamebase-37 }

<a id="game-gamebase-37-1"></a>
#### バグ修正
* [SDK] 1.7.1
	* (Unity) Inspector で設定された SetDebugMode の値が反映されないバグを修正
	* (Unity) Standalone、WebGL: Display Language で使用されるリソースファイルの欠落を修正
	* (Unity) Google Adapter 1.6.2 リリース: Google Adapter 1.6.1 で AuthCode が Empty で返され、認証が失敗するバグを修正

<a id="game-gamebase-38"></a>
### 2018. 02. 22. { #game-gamebase-38 }

<a id="game-gamebase-38-1"></a>
#### 機能追加
* [SDK] 1.7.0
	* Naver IdP 認証追加
	* Display Language 設定追加: 端末の言語とは別に、ゲーム内でゲームユーザーの表示言語を設定できる Display 言語を追加しました。

<a id="game-gamebase-39"></a>
### 2018. 01. 25. { #game-gamebase-39 }

<a id="game-gamebase-39-1"></a>
#### 機能追加

* [Console]
	* [Push] PUSH 入力値コピー機能追加
	* [Operating indicator > グループ同時接続] 日別グループ同時接続変化グラフ追加

* [SDK] 1.6.0
	* (Unity) Standalone WinSDK 追加
		* 64ビット対応
		* 認証対応: facebook、google、payco

<a id="game-gamebase-39-2"></a>
#### 機能改善/変更
* [Console]
	* [Operating indicator > モニタリング] プロジェクト作成前に設定されたシステムメンテナンス項目が表示される問題を修正
	* [App > アプリ] テスト端末登録画面の改善 - User ID のログイン履歴をもとに簡単に端末を登録できるよう改善
	* [Operation > メンテナンス] メンテナンスプレビュー画面の改善

<a id="game-gamebase-39-3"></a>
#### バグ修正
* [SDK] 1.6.0
	* (iOS) WebView 呼び出し時にクラッシュが発生する可能性がある箇所への防御ロジック処理


<a id="game-gamebase-40"></a>
### 2017. 12. 21. { #game-gamebase-40 }

<a id="game-gamebase-40-1"></a>
#### 機能追加

* [Console]
	* [Push] 現地タイムゾーン送信（Local Time Push）機能を追加
	* [Operating indicator > 販売状況] マーケット別売上チャートを追加
	* [Operating indicator > ユーザー統計] アプリリリース以降のユーザー指標の推移を確認するメニューを追加
	* [Operation > メンテナンス] メンテナンス状態でユーザーに表示するメンテナンスページの登録方法を追加
		* 既存：Gamebase 自体が提供するページ、外部ページ URL の入力
		* 追加：Console で入力したメンテナンス内容を外部ページに渡す機能を追加

* [SDK] 1.5.0
	* WebView が閉じられる際に発生する Close Callback を追加
	* WebView で使用する Custom Scheme の Event を受け取れる機能を追加
	* Unity Setting Tool を新規リリース

<a id="game-gamebase-40-2"></a>
#### 機能改善/変更
* [Console]
	* [App > クライアント] クライアントステータス変更時に、以前ゲームで登録したユーザー向け表示メッセージ情報を再利用できるよう修正

<a id="game-gamebase-40-3"></a>
#### バグ修正
* [SDK] 1.5.0
	* （Unity）UnityEditor でゲストログインができない現象を修正
	* （Unity）TOAST Console に Facebook 認証情報を登録せずに Gamebase.Login("facebook") API を呼び出した場合、KeyNotFoundException が発生する問題に対して防御コードを追加


<a id="game-gamebase-41"></a>
### 2017. 11. 30. { #game-gamebase-41 }

<a id="game-gamebase-41-1"></a>
#### 機能追加

* [Console]
	* [Management > アラーム] Webhook アラームを登録する機能を追加しました。
	* [Operating indicator > モニタリング] Push 送信履歴照会を追加しました。

<a id="game-gamebase-41-2"></a>
#### 機能改善/変更
* [Console]
	* [Operating indicator > モニタリング] チャートの色を変更し、Timezone の問題を修正しました。DAU 計算ロジックを変更しました（ログイン時間基準 → 接続時間基準）。
* [API] [メンテナンス照会 API](./api-guide/#check-maintenance-set) の結果を List から単一オブジェクトに変更しました。

<a id="game-gamebase-41-3"></a>
#### バグ修正
* [Console]
	* [Push] Push 登録時にデフォルト言語が選択されていない状態でも登録されてしまう不具合を修正しました。


<a id="game-gamebase-42"></a>
### 2017. 11. 23. { #game-gamebase-42 }

<a id="game-gamebase-42-1"></a>
#### 機能追加

* [SDK] 1.4.0 アップデート
	* (Unity)Gamebase Facebook Adapter を追加: Android、iOS、WebGL、Standalone Platform および UnityEditor をサポートしました。

<a id="game-gamebase-42-2"></a>
#### 機能改善/変更
* [SDK] 1.4.0 アップデート
	* (iOS)close/back ボタンのリソースがない場合に、「x」「<」などのテキストで表示されていた問題をデフォルト値で置き換えました。

<a id="game-gamebase-42-3"></a>
#### バグ修正
* [SDK] 1.4.0 アップデート
	* (Android)Gamebase が提供するポップアップを使用しない場合、利用停止情報が null で返されるエラーを修正しました。
	* (iOS)WebView 起動後、デバイスを回転させると NavigationBar のタイトルがリセットされるエラーを修正しました。
	* (iOS)WebView の NavigationBar の高さをカスタマイズする際に、NavigationBar の背景部分が重なって表示されるエラーを修正しました。

<a id="game-gamebase-43"></a>
### 2017. 10. 26. { #game-gamebase-43 }

<a id="game-gamebase-43-1"></a>
#### 機能追加

* [SDK] 1.3.0 アップデート
	* Credential を使用した AddMapping API を追加しました。

<a id="game-gamebase-43-2"></a>
#### 機能改善/変更
* [Console]
	* TC Push エラーコードに応じたメッセージ処理を適用しました。
	* 利用停止テンプレートメッセージの登録画面を Input Textbox から TextArea に変更しました。
	* TC の新しい権限追加に伴い、管理メニューが正常に表示されなかった問題を修正しました。
* [SDK] 1.3.0 アップデート	
	* (Unity)CredentialInfo を使用する Login API 呼び出し時に iOS Plugin で JSON パースができなかったバグを修正しました。
	
<a id="game-gamebase-44"></a>
### 2017. 09. 21. { #game-gamebase-44 }

<a id="game-gamebase-44-1"></a>
#### 機能追加

* 利用停止（ユーザーペナルティ）機能を追加しました。
* [SDK] 1.2.0 アップデート
	* 利用停止ユーザー向けポップアップを表示するようにしました。
* [Console]
	* カスタマーサポート（メール、電話番号）の登録
	* Ban メニューを開放しました。
	* Member メニュー: ユーザーの購入履歴照会機能を追加しました。


<a id="game-gamebase-44-2"></a>
#### 機能改善/変更

* [Console]
	* 各メニューにおいてユーザーの国基準で時刻を表示するようにしました。
	* 販売状況の小数点以下の価格処理を改善しました。
	* 同時接続変化量アラートの多言語対応（英語/韓国語から選択可能）

<a id="game-gamebase-45"></a>
### 2017. 08. 24. { #game-gamebase-45 }

<a id="game-gamebase-45-1"></a>
#### 機能改善/変更

* [SDK] 1.1.6 アップデート
	* Push API を追加（iOS 向け）: SetSandboxMode

<a id="game-gamebase-46"></a>
### 2017. 07. 20. { #game-gamebase-46 }

<a id="game-gamebase-46-1"></a>
#### 機能改善/変更

* Gamebase 商品の利用停止時に関連データを削除するための日次バッチ機能を追加しました。
* [SDK] 1.1.5 アップデート
	* システムポップアップ API を追加（showAlertWithTitle）
	* 国コードを大文字で返すように変更しました（Android）。
	* TCPush SDK 1.4.1 にアップデートしました。
	* IAP SDK 1.3.3.20170627 にアップデートしました。
* [Console]
	* 外部連携モジュールでエラーが発生した際、追跡のための TrackingTime を追加表示するようにしました。

<a id="game-gamebase-47"></a>
### 2017. 05. 25. { #game-gamebase-47 }

<a id="game-gamebase-47-1"></a>
#### 機能改善/変更

* Gamebase 商品の利用停止時に関連データを削除するための日次バッチ機能を追加しました。
* [SDK] 1.1.4 アップデート
	* ランタイム中に決済 Store を変更できる API を提供しました。
	* (Android)TCPushSdk v1.4 を適用し、Tencent Push 機能を提供しました。
* [Console]
	* 多言語対応を追加しました。
	* すべてのメニューの Create、Update、Modify 実行時に Audit log 機能を追加しました。

<a id="game-gamebase-48"></a>
### 2017. 04. 20. { #game-gamebase-48 }

<a id="game-gamebase-48-1"></a>
#### 機能改善/変更

* [SDK] 1.1.3 アップデート
	* (Android)起動構造およびポップアップ/メンテナンスページを改善しました。カスタムメンテナンスページの設定機能を追加しました。
	* (Android)認証構造を改善し、ログを追加しました。認証 Adapter および SDK バージョンのログを出力するようにしました。

<a id="game-gamebase-48-2"></a>
#### バグ修正
* [SDK] 1.1.3 アップデート
	* (Android)Facebook SDK v4.19.0 以降で初期化時にクラッシュするエラーを修正しました。


<a id="game-gamebase-49"></a>
### 2017. 04. 04. { #game-gamebase-49 }

<a id="game-gamebase-49-1"></a>
#### 機能改善/変更
* [SDK] 1.1.2 アップデート
    * ゲーム起動時のメンテナンス、緊急お知らせポップアップを改善しました。
    * Unity Plugin のデバッグログを追加し、例外の詳細処理を改善しました。
* [API] [IAP](./api-guide/#purchase-iap) API 連携: アイテム照会、未消費履歴照会
* [API] checkAccessToken API のレスポンス結果に、ログイン時に使用した IdP の関連情報を含むスペックを追加しました。
* [Console] メンテナンス、緊急お知らせ: クライアントバージョン選択時に、ゲームで使用しないストアは表示されないように変更しました。

<a id="game-gamebase-49-2"></a>
#### バグ修正
* [Console] iOS クライアント登録時にマーケットが異常表示されていた問題を修正しました。

<a id="game-gamebase-50"></a>
### 2017. 03. 21. { #game-gamebase-50 }

<a id="game-gamebase-50-1"></a>
#### 機能改善/変更
* [SDK] 1.1.0 アップデート
    * 外部 AccessToken を受け取り、idPLogin を行うインターフェースを追加しました。
    * [UI 機能追加](./aos-ui): Custom Webview、AlertDialog
* [API] [Leaderboard](./api-guide/#leaderboard)、[IAP](./api-guide/#purchase-iap) API 連携

<a id="game-gamebase-51"></a>
### 2017. 03. 09. { #game-gamebase-51 }

<a id="game-gamebase-51-1"></a>
#### 新規商品リリース
* ゲームに共通して必要な機能を提供し、手軽かつ効率的にゲーム開発が行えるようサポートするサービスです。
	* 多様な認証をサポート: Guest、サードパーティ（Google、Facebook、GameCenter など）認証
	* ログアウトおよび会員退会機能を提供します。
	* 1 人のユーザーが複数の外部 IDP を同時に使用できるよう、マッピング機能を提供します。
	* ゲーム運営のためのゲームアプリ状態管理、メンテナンス、緊急お知らせなどの機能を Web コンソールで提供します。
	* リアルタイムで運営指標を確認できる Web コンソール画面を提供します。
	* TOAST Cloud 商品連携: PUSH、IAP