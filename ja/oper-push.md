## Game > Gamebase > Console ご利用ガイド > Push

アプリユーザーにPush通知を送信することができます。
Gamebaseでは、NHN Cloud Pushサービスを使用してPush通知を送信します。

## Push
> <font color="red">[重要]</font><br/>
>
> サービス中のゲームで**Gamebase SDK 2.6.0以上**のバージョンを適用した場合に使用するメニューです。

プッシュを送信した履歴と登録されたプッシュ予約リストを確認できます。

![push_01](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Push/jp/push_01_jp_240103.png)

### Registered List

予約リストからプッシュを選択すると、プッシュの予想送信時間と登録情報を確認できます。**送信キャンセル**ボタンを押して予約送信をキャンセルしたり、**コピー**ボタンを押して登録されたプッシュ登録情報を利用して新しいプッシュを登録できます。

### Send History

送信履歴リストからプッシュを選択すると、送信されたプッシュの詳細履歴を照会できます。
**コピー**ボタンを押すと、送信されたプッシュの登録情報を利用して簡単にプッシュを登録できます。

![push_02](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Push/jp/push_02_jp_240103.png)

### Register Push

新しいプッシュを登録するには**登録**ボタンを押します。
コンソールに登録した値が実際の端末でどのように表示されるかを右側のプレビューで確認できます。

![push_03](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Push/jp/push_03_jp_240103.png)

#### (1)送信タイプ

送信周期を選択します。
- **即時送信**：登録後、すぐにプッシュメッセージを送信します。
- **予約送信**：予約した時間にプッシュメッセージを送信します。ユーザーの端末国時間を基準にプッシュメッセージを送信するには**現地時間基準で送信**を選択します。**現地時間基準で送信**を選択し、「2017-01-01 09：00」に設定すると、韓国端末を使用するユーザーは韓国時間「2017-01-01 09：00」に、イギリスの端末を使用するユーザーはイギリス時間「2017-01-01 09：00」にプッシュメッセージを受信します。
- **繰り返し送信**：同じプッシュを定期的に送信します。ユーザーの端末国時間を基準にプッシュメッセージを送信するには**現地時間を基準に送信**を選択します。送信期間と時間を選択します。送信周期を**毎日**、**毎週**、**毎月**基準で入力します。
	- 毎日：毎日プッシュメッセージを送信します。
	- 毎週：送信する曜日を選択して特定の曜日にのみプッシュメッセージを送信します。曜日は複数選択できます。
	- 毎月：1か月を基準に任意の日にプッシュメッセージを送信します。日付(1～31)のみ入力できます。1つ以上の日付を入力できます。例えば1日、5日、10日に送信したい場合は1, 5, 10を入力します。

#### (2)送信対象
プッシュメッセージを送信する対象を選択します。
- **全体送信**：OSごとに選択できます。選択したOSを使用するユーザーは全員プッシュメッセージを受信します。
- **特定会員にのみ送信**特定の会員にのみプッシュメッセージを送信する時に選択します。入力されたユーザーIDでプッシュトークンを登録した端末にプッシュメッセージを送信します。対象ユーザーが5個の端末でゲームしている時は、5個の端末にプッシュメッセージを送信します。
- **グループ送信**：メッセージを送信したいユーザーリストをファイルで登録します。グループ送信は1回最大10,000人まで可能です。
- **タグ送信**：タグを利用してプッシュメッセージを送信できます。**プッシュ - タグ**メニューから登録されたタグへ送信することができ、タグを複数登録することもできます。タグフィルタ機能は次のとおりです。
	- AND：選択したタグを全て満たすユーザーにのみプッシュメッセージを送信します。
	- OR：選択したタグのうち、1つでも条件を満たすユーザーにプッシュメッセージを送信します。

#### (3)イベントキー
プッシュ送信統計に使用するイベントキーを選択します。
**選択**ボタンを押すとイベントキー選択ポップアップが表示され、**収集中**状態のイベントキーを選択できます。
![push_04](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Push/jp/push_04_jp_240103.png)

#### (4)対象国

プッシュメッセージを送信する国を選択します。
- **全ての国**：すべてのユーザーにプッシュメッセージを送信します。
- **一部の国**：選択した国のユーザーにのみプッシュメッセージを送信します。
 追加する国名を入力すると、自動的に完成されて入力されます。入力する国がない場合は[サポート](https://toast.com/support/inquiry)へご連絡ください。

> [参考]
>
> 国判断基準
> ユーザーの**USIM国コード**を基準に判断し、USIMがない場合は、端末に設定されている国を基準にプッシュメッセージが表示されます。

#### (5)メッセージタイプ

> [参考]
>
> 韓国の「情報通信網法」を遵守するために提供される機能です。
> プッシュ送信時、情報性メッセージではない場合は「(広告)」という文言を冒頭に入力し、メッセージに連絡先と受信停止方法が記載されている必要があります。

- **広告性**：入力したメッセージの冒頭に「(広告)」という文言が追加されます。また入力した連絡先と受信停止方法が記載されて送信されます。全体送信などの広告性プッシュメッセージは国内では情報通信網法を遵守するために必ず「広告性」メッセージで送信する必要があります。
- **情報性**：入力したメッセージのみプッシュ送信されます。韓国端末ではない(USIM国コードが韓国ではない)ユーザーには情報性メッセージで送信してください。


> <font color="red">[重要]</font><br/>
>
> **広告性**を選択した後、入力メッセージに「(広告)」文言を入力すると「(広告)」文言が重複するため注意してください。

#### (6)メッセージ内容

ユーザーに表示するプッシュメッセージを入力します。

メッセージは複数の言語で登録できます。入力した言語以外の言語を使用するユーザーには基本言語で表示されます。言語を追加するには下の**メッセージ追加**ボタンをクリックします。リストにない言語を追加するには[サポート](https://toast.com/support/inquiry)へご連絡ください。
「基本言語へ自動翻訳」ボタンを選択すると、基本言語で入力された内容を元に内容を翻訳して各項目に設定された言語に合わせて内容が入力されます。

> [参考]
>
> プッシュメッセージを送信したが端末にメッセージが届かない場合
> 大部分はユーザーのプッシュトークンを登録していない場合です。ユーザーのプッシュトークンを登録しているかを確認してください。
> プラットフォームでプッシュトークンを登録する方法は、次の文書を参照してください。
>
> - [Android > Register Push](./aos-push/#2-register-push)
> - [iOS > Register Push](./ios-push/#2-register-push)
> - [Unity > Register Push](./unity-push/#2-register-push)

#### (7)メッセージ文字色(Only Android)

Androidでは端末に表示されるプッシュメッセージの文字色を指定できます。
タイトルと内容の色をそれぞれ指定することができ、文字色選択ウィンドウで色を選択するか、RGB Hex値を直接入力できます。
選択された文字色は、右側のプレビュー画面で確認できます。

#### (8)メッセージクリックアクション(任意項目)
プッシュメッセージをクリックするときに移動するURLまたはSchemeを設定できます。

#### (9)ユーザー定義フィールド(任意項目)
プッシュメッセージに一緒に伝達したいユーザー定義キーを設定できます。
フィールド追加ボタンを利用して項目を作成することができ、最大10個まで作成可能です。

#### (8)通知音(選択項目)

端末でプッシュを受信する時に再生される通知音を設定できます。
**通知音追加**ボタンを押して設定できます。設定しない場合は基本通知音が再生されます。
外部URLアドレスまたはアプリ内に配布された通知音ファイルパスを入力します。

#### (9)大アイコン(Only Android)

**大アイコン追加**ボタンを押すとプッシュメッセージ受信時に右側に表示されるアイコンイメージを設定できます。
大アイコンイメージを設定してプッシュメッセージを送信すると、Android端末でのみイメージが表示され、iOSでは表示されません。
**外部**を選択して表示するアイコンイメージの外部URLを入力するか、**内部**を選択してアプリ内に配布したアイコンイメージファイルのパスを入力します。
外部イメージの場合は、右側のプレビューでアイコンを事前に確認できます。

#### (10)ボタン

プッシュメッセージの受信時に、メッセージの下部に表示されるボタンを最大3個まで設定して一緒に送信できます。
ボタンは4種類で、タイプごとに動作方式と設定する値が異なります。

- アプリを開く：プッシュメッセージからボタンを押すとアプリを実行します。ボタン名のみ指定できます。
- URLを開く：プッシュメッセージからボタンを押すとコンソールで入力したURLに移動します。アプリスキーマで特定アプリを実行したり、Webページを開くことができます。
- レスポンス：プッシュメッセージからボタンを押すと、レスポンスできるウィンドウが表示されます。iOSでは送信ボタン名を一緒に設定できます。
- 閉じる：プッシュメッセージからボタンを押すと、プッシュが閉じられます。ボタン名のみ指定できます。

#### (11)メディア(iOS)

iOSプッシュメッセージの受信時に動的に実行されるメディアを追加できます。
**IMAGE**、**GIF**、**VIDEO**、**AUDIO**項目を設定することができます。接続する外部URLまたは内部ファイルパスを入力できます。

#### (12)メディア(Android)

Androidプッシュメッセージの受信時に実行されるメディアを追加できます。
現在Androidでは**IMAGE**項目のみ設定できます。接続する外部URLまたは内部ファイルパスを入力できます。


## Statistics

プッシュメッセージ、トークン、受信設定に関連する指標を表とグラフで確認できます。
統計は、次のメニューで構成されています。

- 送信/受信：プッシュメッセージの送信および受信関連指標
- トークン登録：プッシュトークン登録および削除関連指標
- 受信設定：プッシュ受信設定関連指標

### 送信/受信
![push_05](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Push/jp/push_05_jp_240103.png)

1. 送信/受信統計

選択された期間のプッシュメッセージ送信/受信統計を表示します。

* 送信率：期間内の全体送信成功数 / 全体送信数
* 受信率：期間内の全体受信成功数 / 全体送信成功数
* 受信確認率：期間内の全体受信確認数 / 全体受信成功数

2. プッシュ送信リスト
選択された期間のプッシュメッセージ送信リスト

3. 時間別データ
選択された期間の時間間隔に基づいたプッシュ送信、送信失敗、受信、受信確認指標を表示します。

* 選択された期間が24時間以下の場合にのみ**分**単位の選択が可能です。

### トークン登録

![push_06](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Push/jp/push_06_jp_240103.png)

1. トークン登録統計

選択された期間のトークン登録、削除統計を表示します。

* タイプ：トークンタイプごとの登録占有率
* 国：使用者の国ごとのトークン登録占有率
* 言語：ユーザー言語ごとのトークン登録占有率

2. 国
選択された期間のユーザーの国ごとのトークン登録、削除統計を表示します。

3. 言語
選択された期間のユーザーの言語ごとのトークン登録、削除統計を表示します。

### 受信設定
選択された期間の受信設定関連統計を表示します。
![push_07](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Push/jp/push_07_jp_240103.png)

|区分|情報性|広告性|夜間広告性|
|------|:---:|:---:|:---:|
|全て同意| O | O | O |
|全て拒否| | | |
|情報性同意、広告全て拒否| O | | |
|情報性および広告昼間同意、夜間拒否| O | O | |

## Event Key
プッシュ送信統計に使用するイベントキーを管理できます。
![push_08](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Push/jp/push_08_jp_240103.png)
Pushからプッシュメッセージを送信する時に使用するイベントキーを登録できます。

### Event Key register
![push_09](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Push/jp/push_09_jp_240103.png)

### Event Key detail
登録されたイベントキーを管理できます。
![push_10](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Push/jp/push_10_jp_240103.png)

上部の**削除**、**修正**ボタンを押してイベントキー情報を修正または削除できます。

## Authentication
プッシュ送信に使用する証明書を管理できます。
![push_11](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Push/jp/push_11_jp_240103.png)

各証明書の**登録**、**修正**、**削除**ボタンを押して証明書を登録、修正、削除できます。

### Authentication register
![push_12](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Push/jp/push_12_jp_240103.png)
![push_13](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Push/jp/push_13_jp_240103.png)
![push_14](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Push/jp/push_14_jp_240103.png)
![push_15](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Push/jp/push_15_jp_240103.png)

## Tag

Provides a tag function that can send push messages by grouping users according to specific criteria.

![push_16](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Push/jp/push_16_jp_240103.png)

You can register a tag name to be used when sending push messages from NHN Cloud Push.

### Tag register

![push_17](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Push/jp/push_17_jp_240103.png)

### Tag detail

You can manage the registered tags and manage the list of users registered in the tags.

![push_18](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Push/jp/push_18_jp_240103.png)

You can modify or delete tag information by clicking the **Modify** or **Delete** buttons at the top, and you can register or delete users in the tag using the user ID management function at the bottom.

#### Add users

##### Add a single user

![push_19](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Push/jp/push_19_jp_240103.png)

##### Add using a file

![push_20](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Push/jp/push_20_jp_240103.png)

If you click the **Add** button, the registration popup appears as shown above, and you can input users by entering an ID directly or by registering a file.

**File registration** allows you to register up to 1,000 users at a time.

#### Delete users

![push_21](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Push/jp/push_21_jp_240103.png)

To delete a user registered in a tag, select the checkbox on the left in the user list and click the **Delete** button.

## Setting
プッシュ関連設定が行えます。
![push_22](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Push/jp/push_22_jp_240103.png)

### メッセージ受信および確認設定
送信されたメッセージの受信と確認情報を収集する機能です。

### 重複メッセージ防止設定
内容が完全に同じメッセージが複数送信リクエストされても設定した時間の間は送信しない機能です。

* 1分から60分まで1分間隔で設定できます。

### 広告表示文言位置設定
広告性メッセージを送信する時に表示される広告の表示位置を設定できます。

* 右側**プレビュー**ボタンを押すと、設定した広告表示文言位置のプッシュ例を確認できます。

![push_23](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Push/jp/push_23_jp_240103.png)

### トークン設定

トークン有効期限設定およびアプリタイプを設定できます。

* トークン有効期限：1日から730日まで設定できます。
* アプリタイプ
	* 多重トークン：UID(ユーザーID)は複数のトークンを持つことができます。1人のユーザーが同時に複数の端末で使用できるアプリです。
	* 単一トークン：UIDは1つのトークンのみ持つことができます。1人のユーザーが1つの端末でのみ使用できるアプリです。

### 送信履歴の保存

メッセージ送信履歴をNHN Cloud Log & Crash Searchに保存する機能です。

* AppKey： Log & Crash Searchアプリキーです。
* Log Source： Gamebaseで提供するPUSH APIを利用して送信したPUSH履歴をLog & Crash Searchで検索できます。
* Log Level： Log & Crash Searchへ送信するログレベルです。
	* ALL：すべてのログがLog & Crash Searchへ送信されます。
	* INFO：有効期限が切れたトークンと送信失敗ログがLog & Crash Searchへ送信されます。
	* ERROR：送信失敗ログがLog & Crash Searchへ送信されます。

### 広告受信同意案内メッセージ予約設定
広告メッセージの受信に同意してから満2年が経過したユーザーに受信再同意メッセージを自動的に送信します。満2年になる月ごとに設定した日時にメッセージが送信されます。
* 使用するかどうか：広告受信同意案内メッセージ予約設定機能使用有無です。
* 予約日：プッシュメッセージを伝達する日付です。 3に設定すると毎月3日にメッセージが送信されます。
* 予約時間：プッシュメッセージを送信する時間です。予約日を3、予約時間を11に設定すると、毎月3日午前11時にメッセージが送信されます。
* 案内メッセージ
	* タイトル：プッシュメッセージのタイトルを設定できます。
	* 内容：プッシュメッセージの内容を設定できます。広告性メッセージ受信同意日時日本語識別子(###AD_AGREEMENT_DATE_TIME###)を本文に入れるとメッセージ送信時に該当トークンの同意日時に置換されます。メッセージ内容に受信同意事実と受信に同意した日付、受信同意維持または撤回方法を必ず含める必要があります。
