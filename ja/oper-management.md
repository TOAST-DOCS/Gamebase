## Game > Gamebase > Console ご利用ガイド > 管理

Gamebaseを使用するゲームに対する照会権限の管理、通知送信の設定、通知内訳の照会などの機能を使用することができます。

## Authorization

Gamebase Consoleの使用権限を管理できます。
![gamebase_manage_01_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_manage_01_202106.png)
* Gamebase Console使用権限管理
  * **ウィークリーレポート受信権限**：**ウィークリーレポートを**受信する権限
* 新しいメンバーを登録するには、NHN Cloudプロジェクトメンバー管理から追加する必要があります。
* 自分自身の権限は修正できません。

* Gamebase Consoleの使用権限管理
  * **販売現状のアクセス権限**：**有料**メニューに対するアクセス権限
  * **管理メニューアクセス権限**：その他のメニューに対するアクセス権限
  * **全てのメンバーのダウンロード権限**：**プロジェクトの全メンバー**の情報をダウンロードできる権限
* 新しいメンバーを登録したい場合、TOASTプロジェクトメンバー管理から追加する必要があります。
* 自分自身の権限は修正することができません。

## Alarm

Gamebaseの通知機能を使用してゲームユーザーの増加率や減少率、最小同時接続者数の変化などに対する通知を受け取ることができます。

### Alarm

![gamebase_manage_02_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_manage_02_202106.png)

#### (1) 減少通知
同時接続者数が減少したときに通知を受け取るかどうかを設定します。通知を受け取りたい場合、**減少通知**を**On**に設定します。

- **減少率**：同時接続が何%減少したときに通知を受け取るか指定します。
- **メンテナンスによる同時接続者数減少の通知を無視する**：アプリをメンテナンスする場合は、当然、同時接続者数が減少します。
  この場合、**メンテナンスによる同時接続者数減少の通知を無視する**を**On**に設定して通知を受け取らないようにすることができます。

#### (2) 増加通知
同時接続者数が増加したときに通知を受け取るように設定することができます。
機能を有効にすれば、運営者が通知を受け取る数値を設定することができます。

#### (3) メッセージ言語
通知送信メッセージの言語を選択することができます。現在は韓国語と英語のメッセージのみ対応しており、追加リクエストがある場合、他の言語も追加する予定です。

#### (4) 最小同時接続者数
**最小同時接続者数**に指定した数より少ない人数がアプリに接続した場合に通知を受け取るようにすることができます。例えば、**最小同時接続者数**を「500人」に設定した場合、同時接続者数が500人以下になったときに通知を受け取ることになります。最小設定値は100人で、100人未満には設定できません。

### Alarm Log

通知ログは、通知メニューの下にあり、通知が発生した履歴を照会することができます。
最大30日まで照会することができ、照会後に**Search**ボタンをクリックすると、リアルタイムでフィルタリングすることもできます。

![gamebase_manage_03_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_manage_03_202106.png)

- 発生時間：通知が送信された時間情報
- 過去の同時接続者数：通知が送信される前に取得した同時接続者数の情報
- 新規の同時接続者数：通知が送信される瞬間取得した同時接続者数の情報
- 変化率(設定値)：変化率の値は過去の同時接続者数と比較した新規の同時接続者数に関する情報。設定値は通知が発生した際に送信のために設定しておいた値

### Webhook
Gamebaseで基本提供されるSMS/Email以外で別途通知を受け取ることができるWebhook設定機能を提供します。
外部システムのWebhook URLを通して通知送信をリクエストする場合、一緒に通知を送信します。

#### (1) リスト照会
![gamebase_manage_04_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_manage_04_202106.png)
現在、通知を受け取ることができるWebhookに対する登録内訳を確認することができます。
登録されたWebhook URLが必要な場合、右の**URLコピー**をクリックして簡単にコピーすることができます。

#### (2) 登録
![gamebase_manage_05_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_manage_05_202106.png)
**登録**ボタンをクリックして外部システムから発行されたWebhook情報を登録することができます。
現在はDoorayとSlackのみ登録でき、今後、リクエストがあれば新しいリストを追加する予定です。

#### (3) 詳細照会/修正/削除
![gamebase_manage_06_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_manage_06_202106.png)
各項目をクリックすると、詳細情報を照会することができます。
登録された情報を変更したい場合は**修正**ボタンをクリックします。該当するWebhookが必要でない場合は、**削除**ボタンをクリックして項目を削除することもできます。

### Recipient List

アラームを受信するユーザーを設定できます。新しいメンバーを登録するにはNHN Cloudプロジェクトメンバー管理から追加する必要があります。
![gamebase_manage_07_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_manage_07_202106.png)
GamebaseではメールとSMSでアラームを送信できます。
メールとSMSは、NHN Cloud登録時に入力した情報を利用して送信し、メールアドレス/番号を間違えて登録した場合にはアラームを受け取れない場合もあります。携帯電話番号情報はNHN Cloudの**マイ情報管理**ページで確認できます。


## Config

GamebaseとNHN Cloudサービスの連携関連設定が行えます。

![gamebase_manage_08_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_manage_08_202106.png)

NHN Cloud Launchingに設定した情報をGamebase Launching APIの呼び出し時に一緒に受け取るかどうかを設定できます。NHN Cloud Launchingサービスを使用する場合にのみ機能をOn/Offできます。
