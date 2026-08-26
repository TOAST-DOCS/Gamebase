<!-- pre-align:aligned sig=33a84d66e7f0 -->

<a id="game-gamebase-release-notes-server-api"></a>
## Game > Gamebase > リリースノート > Server API { #game-gamebase-release-notes-server-api }

<a id="2026-08-25"></a>
### 2026. 08. 25. { #2026-08-25 }

<a id="2026-08-25-added-features"></a>
#### 機能の追加
* Google のチャージバック関連APIの追加

<a id="2026-03-24"></a>
### 2026. 03. 24. { #2026-03-24 }

<a id="2026-03-24-added-features"></a>
#### 機能の追加
* ユーザーIDでプッシュトークンを照会するAPIの追加

<a id="february-24-2026"></a>
### 2026. 02. 24. { #february-24-2026 }

<!-- TODO: translate body -->

<a id="february-24-2026-added-features"></a>
#### 機能追加

<!-- TODO: translate body -->

<a id="december-23-2025"></a>
### 2025. 12. 23. { #december-23-2025 }

<!-- TODO: translate body -->

<a id="december-23-2025-added-features"></a>
#### 機能追加

<!-- TODO: translate body -->

<a id="august-27-2024"></a>
### 2024. 08. 27. { #august-27-2024 }

<a id="august-27-2024-added-features"></a>
#### 機能追加
* 決済履歴照会API request bodyに'paymentToken'フィールドを追加

<a id="october-31-2023"></a>
### 2023. 10. 31. { #october-31-2023 }

<a id="october-31-2023-added-features"></a>
#### 機能追加
* 購読の現在状態を照会する"Get subscriptions Status" APIが追加

<a id="august-17-2023"></a>
### 2023. 08. 17. { #august-17-2023 }

<!-- TODO: translate body -->

<a id="august-17-2023-added-features"></a>
#### 機能追加

<!-- TODO: translate body -->

<a id="july-25-2023"></a>
### 2023. 07. 25. { #july-25-2023 }

<a id="july-25-2023-added-features"></a>
#### 機能追加
* 購読照会API request bodyに'includeInactiveGoogleStatuses'フィールドを追加
* 購読照会APIレスポンスに'renewTime'フィールドを追加
* 購読照会APIに一度にN個のストアを対象に照会できるように'marketIds'フィールドを追加

<a id="december-27-2022"></a>
### 2022. 12. 27. { #december-27-2022 }

<a id="december-27-2022-added-features"></a>
#### 機能追加

* Google Playストアの購読中の商品キャンセルAPIを追加
* 購読照会APIレスポンスに「linkedPaymentId」フィールドを追加

<a id="december-27-2022-feature-updates"></a>
#### 機能改善/変更
* 特定の決済シナリオを介してアイテム購入時に未消費決済履歴照会APIのレスポンス結果でgamebaseProductIdがnullになる問題を修正

<a id="august-23-2022"></a>
### 2022. 08. 23. { #august-23-2022 }

<a id="august-23-2022-added-features"></a>
#### 機能追加
* サーバーURL追加
	* https://api-gamebase.nhncloudservice.com

<a id="july-26-2022"></a>
### 2022. 07. 26. { #july-26-2022 }

<a id="july-26-2022-added-features"></a>
#### 機能追加
* 未消費決済履歴照会APIで一度にN個のストアを対象に照会できるように「marketIds」を追加

<a id="june-30-2022"></a>
### 2022. 06. 30. { #june-30-2022 }

<a id="june-30-2022-added-features"></a>
#### 機能追加
* 退会ユーザーがApple ID認証を使用している場合、Apple ID AccessToken満了APIの呼び出しを追加
* 未消費決済履歴照会APIレスポンスに「paymentId」フィールドを追加

<a id="june-14-2022"></a>
### 2022. 06. 14. { #june-14-2022 }

<a id="june-14-2022-added-features"></a>
#### 機能追加
* 決済トランザクション照会APIを追加
* 未消費決済履歴照会APIレスポンスに「isTestPurchase」フィールドを追加

<a id="may-24-2022"></a>
### 2022. 05. 24. { #may-24-2022 }

<a id="may-24-2022-added-features"></a>
#### 機能追加
* 利用停止および利用停止解除APIを追加

<a id="may-10-2022"></a>
### 2022. 05. 10. { #may-10-2022 }

<a id="may-10-2022-added-features"></a>
#### 機能追加
* 特定期間に退会したユーザーを照会するAPIの追加

<a id="september-14-2021"></a>
### 2021. 09. 14. { #september-14-2021 }


<a id="september-14-2021-bug-fixes"></a>
#### バグ修正
* Leaderboard Wrapping API修正
	* 「複数のユーザースコア/ExtraData登録」APIのマッピングエラーを修正

<a id="march-09-2021"></a>
### 2021. 03. 09. { #march-09-2021 }

<a id="march-09-2021-added-features"></a>
#### 機能追加
* IdP IDでGamebase user IDを取得するAPIを追加
    
<a id="august-11-2020"></a>
### 2020. 08. 11. { #august-11-2020 }

<a id="august-11-2020-feature-updates"></a>
#### 機能改善/変更
* クーポン消費APIのエラーコード追加：クーポンコードに英数字以外の値を入力した場合(Error Code:-4000205)
    
<a id="february-11-2020"></a>
### 2020. 02. 11. { #february-11-2020 }

<a id="february-11-2020-feature-updates"></a>
#### 機能改善/変更
* 退会APIを呼び出す時、regUserの長さに対する有効性チェック(validation)を追加

<a id="january-14-2020"></a>
### 2020. 01. 14. { #january-14-2020 }

<a id="january-14-2020-added-features"></a>
#### 機能追加
* ユーザー退会APIを追加

<a id="november-12-2019"></a>
### 2019. 11. 12. { #november-12-2019 }

<a id="november-12-2019-added-features"></a>
#### 機能追加
* クーポンサービス新規オープン：クーポンを大量に作成し、管理する機能
	* クーポン確認および消費APIを追加
	
<a id="may-28-2019"></a>
### 2019.05.28 { #may-28-2019 }

<a id="may-28-2019-feature-updates"></a>
#### 機能改善/変更
* LTVクエリー修正およびfailoverロジック修正

<a id="2019-03-26"></a>
### 2019.03.26 { #2019-03-26 }

<a id="2019-03-26-1"></a>
#### 機能追加
* TransferAccount機能追加：ゲストユーザーがマッピングを行わずに最大2個のキーを利用して新しい端末に移行できる機能
	* 発行されたTransferAccountのID/PWを検証するサーバーAPI (validateTransferAccount)

<a id="2018-06-26"></a>
### 2018.06.26 { #2018-06-26 }

<a id="2018-06-26-1"></a>
#### 機能追加
* getSimpleLaunching：クライアントアプリ起動時に提供されるLaunching情報の確認用API

<a id="2017-11-30"></a>
### 2017.11.30 { #2017-11-30 }

<a id="2017-11-30-1"></a>
#### 機能改善/変更
* [メンテナンス照会API](./api-guide/#check-maintenance-set)結果をListから単一オブジェクトに変更

<a id="2017-04-04"></a>
### 2017.04.04 { #2017-04-04 }

<a id="2017-04-04-1"></a>
#### 機能改善/変更
* [IAP](./api-guide/#purchase-iap) API連携：アイテム照会、未消費内訳照会
* checkAccessToken APIレスポンス結果に、ログイン時に使用されたIdP関連情報を含むスペックを追加

<a id="2017-03-21"></a>
### 2017.03.21 { #2017-03-21 }

<a id="2017-03-21-1"></a>
#### 機能改善/変更
* [Leaderboard](./api-guide/#leaderboard), [IAP](./api-guide/#purchase-iap) API連携

<a id="2017-03-09"></a>
### 2017.03.09 { #2017-03-09 }

<a id="2017-03-09-1"></a>
#### 新規サービスリリース
* ゲームで共通して必要な機能を提供し、簡単かつ効率的にゲーム開発ができるようにサポートするサービスです。
	* 多様な認証をサポート：ゲストログイン、3rd Party(Google、Facebook、GameCenterなど)認証
	* ログアウトおよび会員退会機能を提供
	* 1人のUserが複数の外部IDPを同時に使用できるようにmapping機能を提供
	* ゲーム運営のためのゲームアプリ状態管理、メンテナンス、緊急告知などの機能をWebコンソールで提供
	* リアルタイムに運営指標を確認できるWebコンソール画面を提供
	* TOAST Cloudサービスと連携：PUSH、IAP
