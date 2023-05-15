## Game > Gamebase > コンソール使用ガイド > Analytics

アプリユーザーの状況、売上関連の指標を表とグラフで確認できます。
Analyticsは、次のメニューで構成されています。

* リアルタイムモニタリング：アプリユーザーのリアルタイム同時接続およびリアルタイム決済指標
* ユーザー指標：アプリユーザー中心の基本指標(DAU、MCU、NRU)および環境、流入/流出、リテンションなどの指標
* 売上指標：アプリの売上関連指標
* グループ同時接続：Gamebaseサービスユーザーが属するプロジェクトのグループ同時接続とグループ別基本指標
* 利用環境：インストールURL呼び出しに対する統計指標

## リアルタイムモニタリング
### リアルタイム同時接続

現在、アプリユーザーのリアルタイム同時接続指標およびメンテナンス、プッシュ情報を確認できます。

![gamebase_analytics_01_202004_2_ja](https://static.toastoven.net/prod_gamebase/gamebase_analytics_01_202004_2_ja.png)

#### 1. リアルタイム同時接続者(CCU)変化グラフ
1分ごとにデータを更新し、リアルタイムに変更された指標を確認できます。

* 同時接続者(CCU)：1分単位で測定されたリアルタイム同時接続者数(ログインユーザー数)
* 決済額：当日0時～24時にユーザーがGamebaseで決済した金額の合計。払い戻し、決済キャンセルなどの情報が含まれていない純粋な決済額です。
* 新規加入者(NRU)：新規加入者。当日0時～24時にログインログが初めて収集されたユーザー(memberno基準)

#### 2. メンテナンス情報
当日0時～24時にGamebaseに登録されたメンテナンス情報で、メンテナンス後の同時接続者の増減推移を確認できます。

#### 3. プッシュ情報
当日0時～24時にGamebaseに送信されたプッシュ情報で、送信後の同時接続者の増減推移を確認できます。

### ダッシュボード

リアルタイムにゲームユーザーの多様な指標をひと目で確認できます。

![gamebase_analytics_02_202004_2_ja](https://static.toastoven.net/prod_gamebase/gamebase_analytics_02_202004_2_ja.png)

#### 1. リアルタイムユーザー状況ダッシュボード
アプリユーザーおよび決済指標を確認できます。
日付が今日の場合は10分ごとに更新され、それ以外の時は、その日の指標を表示します。

* 同時接続者(CCU)：10分単位で測定されたリアルタイム同時接続者数(ログインユーザー数)
* 最大同時接続者(MCU)：0時～24時の最大同時接続者数(1分単位、CCUの最大値)
* 新規加入者(NRU)：当日0時～ 24時にログインログが初めて収集された新規加入者(memberno基準)
* デイリーユーザー(DAU)：日間memberno基準でログイン1回以上のアクティブユーザー数(Daily Active Users)
* 平均ゲーム時間(Avg.Playtime)：日付別の平均ゲーム時間(DAUのゲーム時間の合計/DAU)
* 累積ユーザー：Gamebaseインストール後、加入した全累積ユーザー数(memberno基準)
* 決済額：0時～24時にユーザーが決済した総決済額
* 決済件数：0時～24時のユーザーの総決済件数
* PU：有料商品を決済したユーザー(ゲームユーザー)(=再購入PU + 新規PU)
* 新規PU (NPU)：有料商品を初めて決済したユーザー(New Paying Users)
* ARPPU：決済ユーザー(PU) 1人あたりの平均決済額(=決済額/PU)
* ARPU：ゲームユーザー(DAU) 1人あたりの平均決済額(=決済額/DAU)

※ MCU、ACU、ARPUの場合、フィルタが全体の場合のみ確認できます。

#### 2. 主要指標リアルタイム変化グラフ
同時接続者(CCU)、新規加入者(NRU)、決済額、PU指標の変化を10分間隔で確認できます。

#### 3. リアルタイム占有率状況
ユーザーのOS、アプリバージョン、ストア、国についての占有率をグラフで確認できます。
日付が今日の場合はCCUを基準に、過去の日付の場合はDAUを基準に表示します。

* OS占有率：DAUのうち、OS別占有比率(当日はCCU基準)
* アプリバージョン占有率：DAUのうち、アプリバージョン占有比率(当日はCCU基準)
* ストア占有率：DAUのうち、ストア占有比率(当日はCCU基準)
* 国別占有率：DAUのうち、国別占有比率(当日はCCU基準)

## ユーザー指標
### ユーザー

ユーザーの基本指標を確認できます。

![gamebase_analytics_03_202004_2_ja](https://static.toastoven.net/prod_gamebase/gamebase_analytics_03_202212_1_ja.png)

#### 1. ユーザー状況
選択された期間のユーザー基本指標を表示します。

* DAU累積：日間memberno基準でログイン1回以上のアクティブユーザー数の合計(Daily Active Users)
* WAU累積：週間AUの合計(Weekly Active Users)。週間指標を選択すると、DAU項目がWAUに代替。
* MAU累積：月間AUの合計(Monthly Active Users)。月間指標を選択すると、DAU項目がMAUに代替。
* 最大同時接続者数(MCU)：0時～24時の最大同時接続者数。1分単位CCU値の最大値を1日単位で集計。
* 新規加入者(NRU)：新規加入者。当日0時～24時までログインログが初めて収集されたユーザー(memberno基準)
* 退会ユーザー：退会ユーザー。当日0時～24時までmembernoが削除されたユーザー数
* 平均ログイン回数：選択した期間の平均ログイン数
* 平均CCU：選択した期間のCCUの平均
* 平均ゲーム時間(Avg.Playtime(/DAU))： 照会された期間中に利用時間の平均(DAUの利用時間の合計 / DAU)

#### 2. 日別指標
選択された期間の日別ユーザー基本指標をグラフと表で表示します。

※ MCU、累積ユーザー(ACU)の場合、フィルタが全体の場合のみ確認できます。

### ユーザー環境
![gamebase_analytics_04_202004_2_ja](https://static.toastoven.net/prod_gamebase/gamebase_analytics_04_202004_2_ja.png)

利用環境ごとのユーザー指標を確認できます。

* 照会条件
    * OS：モバイルOS(Android、iOS)など
    * 国：ユーザーのモバイル端末に設定されている国
    * ストア：IdP：Facebook、Googleなど、ユーザーのIdPログイン/認証情報
    * アプリバージョン：実行されたアプリのバージョン情報
    * Device：ユーザーのデバイス端末の種類。Device選択時はPU、決済額は提供しない(週間、月間データも提供しない)。
* 照会値
    * デイリーユーザー(DAU)：日間memberno基準でログイン1回以上のアクティブユーザー数(Daily Active Users)
    * 新規加入者(NRU)：新規加入者。当日0時～24時にログインログが初めて収集されたユーザー(memberno基準)
    * PU：有料商品を決済したユーザー(ゲームユーザー)(=再購入PU + 新規PU)
    * 決済額：ユーザーが決済した総額

### ユーザーの流入と流出

アプリユーザーの流入、流出数の日付別推移を確認できます。

![gamebase_analytics_05_202004_2_ja](https://static.toastoven.net/prod_gamebase/gamebase_analytics_05_202004_2_ja.png)

* 流入ユーザー(新規+復帰)：流入ユーザーは新規加入者と復帰ユーザーの合計(=新規加入者 + 復帰ユーザー)。
* 新規加入者
    * 日別：新規加入者。当日0時～24時にログインログが初めて収集されたユーザー(memberno基準)
    * 月別：新規加入者。基準月のログインログが初めて収集されたユーザー(memberno基準)
* 復帰ユーザー
    * 日別：基準日から過去8日間ログが収集されなかったユーザー
    * 月別：前月のログが収集されなかったが、基準月のログが収集されたユーザー。
* 流出ユーザー(退会+離脱)：流出ユーザーは、退会ユーザーと離脱ユーザーの合計。(=退会ユーザー + 離脱ユーザー)
* 退会ユーザー
    * 日別：退会ユーザー。当日0時～24時にmembernoが削除されたユーザー数
    * 月別：退会したユーザー。基準月にmembernoが削除されたユーザー数
* 離脱ユーザー
    * 日別：基準日から過去7日間にログが収集されなかったユーザー
    * 月別：前月のログが収集されたユーザーのうち、基準月のログが収集されていないユーザー。
* 連続ユーザー
    * 月別：前月及び基準月のログが全て収集されたユーザー数

### Retention

Retentionは、特定の日に加入したユーザーが、加入した次の日から180日間、どれだけ残っているかを確認できる指標です。

![gamebase_analytics_06_202004_2_ja](https://static.toastoven.net/prod_gamebase/gamebase_analytics_06_202004_2_ja.png)

当日退会者除外オプションを選択すると、加入当日に退会したユーザーを除外して確認できます。

* 当日退会者除外：加入当日に退会したユーザーを新規ユーザーから除外して値を計算します。
    * 新規加入者(New User) =当日加入者 - 加入当日に退会した人
   例) 1月1日に100人新規加入、このうち20人が1月1日に退会したなら、実際の新規加入者を80人(100人-20人)と計算

### LTV
![gamebase_analytics_06_202004_1_ltv_ja](https://static.toastoven.net/prod_gamebase/gamebase_analytics_06_202004_1_ltv_ja.png)

LTVは、選択された利用者グループで、利用者1人の1年間の期待売上を表す推定指標です。

国/OS別、日付別にLTVチャートを提供し、下段の表にはLTV、累積NRU、累積PU、累積決済金額情報を詳細に確認できます。

#### 推定方法
Gamebaseでは、LTV推定方法として、登録後365日経過時点の累積ARPUを使用します。 

#### 利用者グループ条件
利用者グループの条件は以下の通りです。

* 加入日
* 国
* OS

#### 制限条件
LTVを正確に推定するために、以下の制限条件があります。

* 利用者グループの利用者数が1000人以上必要です。
* 利用者グループのPU(決済利用者数)が30人以上必要です。
* 利用者グループ中、最も新しい加入日から7日経過している必要があります。

### Life Cycle
![gamebase_analytics_06_202004_1_lifeCycle_ja](https://static.toastoven.net/prod_gamebase/gamebase_analytics_06_202004_1_lifeCycle_ja.png)

Life Cycleは、最初に利用者が流入した時点から、日別の利用者推移を確認できる指標です。データは最大3年まで提供されます。

* デイリーアクティブユーザー(DAU)：日間memberno基準ログイン1回以上のアクティブ利用者数(Daily Active Users)
* 最大同時接続者(MCU)：0時～24時までの最大同時接続者数。1分単位CCU値の最大値を1日単位で集計する。
* 新規加入者(NRU)：新規加入者。当日0時～24時までログインログが初めて収集された利用者(memberno基準)
* 退会利用者：退会した利用者。当日0時～24時までmembernoが削除された利用者
* 加入当日退会利用者：当日に加入し、当日に退会した利用者
* 平均CCU：選択された期間のCCUの平均
* 平均ゲーム時間 - Avg.Playtime(/DAU)：照会期間のPlaytime平均(DAUのPlaytimeの合計 / DAU)

### Frequency7

![gamebase_analytics_06_202004_1_frequency_ja](https://static.toastoven.net/prod_gamebase/gamebase_analytics_06_202004_1_frequency_ja.png)

Frequency7指標は、DAUの一週間間の訪問数と比率情報を提供します。ゲーム没入度、忠誠度などをひと目で把握できます。

Frequency7の基準は、以下の3つに分けられます。

* 訪問回数：7日間の合計訪問回数
* 連続訪問回数：7日間、該当日を含む連続訪問した回数
* 最大連続訪問回数：7日間の連続訪問した最大回数

上の3つの基準の計算方法例は次のとおりです。 
3/7を基準に3/1, 3/2, 3/3, 3/6, 3/7に訪問したユーザーがいる場合、基準に基づいた訪問回数は次のとおりです。

* 合計回数：5日(3/1, 3/2, 3/3, 3/6, 3/7)
* 連続訪問回数：2日(3/6, 3/7)
* 最大連続訪問回数：3日(3/1, 3/2, 3/3)

## 売上指標
### 決済額

決済額の指標を確認できます。

![gamebase_analytics_07_202004_2_ja](https://static.toastoven.net/prod_gamebase/gamebase_analytics_07_202212_1_ja.png)

#### 1. 決済額状況表
選択した期間の決済額を表示します。
総決済額と、主要ストアの国別決済額を確認できます。

#### 2. 売上推移
日付別に新規売上、再購入売上、PU(決済ユーザー)の推移をグラフで確認できます。
下記の表ではストア、国家、 IdP別の売上を確認できます。
日間照会時にのみ月単位累積決済金額を確認できます。

### 有料ユーザー

有料ユーザー(PU)の指標を確認できます。

![gamebase_analytics_08_202004_2_ja](https://static.toastoven.net/prod_gamebase/gamebase_analytics_08_202212_1_ja.png)

次は、グラフと表で使われる用語説明です。

* 決済額：ユーザーが決済した金額
* 新規決済額：新規決済ユーザー(NPU)が決済した金額
* DAU：日間memberno基準でログイン1回以上アクティブユーザー数(Daily Active Users)
* PU：有料商品を決済したユーザー(Paying User)。PU=再購入PU + 新規PU
* 新規PU(NPU)：有料商品を初めて決済したユーザー(New Paying Users)
* 再購入PU：累積PU - 新規PU (再購入PUは日間データで、前日基準で計算)
* PUR：有料ユーザーの比率(PU/DAU * 100)
* ARPU：１ユーザーあたりの平均決済額(決済額/DAU)
* ARPPU：有料ユーザー1人あたりの平均決済額(決済額/PU)
* ARPNPU：新規有料ユーザーの平均決済額(決済額/NPU)

### アイテム販売指標

Gamebaseに登録したアイテムの販売指標を確認できます。

![gamebase_analytics_09_202004_2_ja](https://static.toastoven.net/prod_gamebase/gamebase_analytics_09_202212_1_ja.png)

* アイテム：Gamebaseに登録したアイテムリスト
* ベストアイテムTop 10：販売金額別、販売数別に販売量が多いアイテムTop 10のリスト
* ストア：AppStore、Google Playなどのストア
* 決済額：ユーザーが決済したアイテム別決済額
* 決済件数：アイテム別決済件数
* PU：アイテム別購入者数
* 決済比率：アイテム別決済比重

### 初回購入
![gamebase_analytics_10_202004_2_ja](https://static.toastoven.net/prod_gamebase/gamebase_analytics_10_202212_1_ja.png)

新規有料ユーザーの初回購入に関する情報を確認できます。


新規有料ユーザーの加入後から最初の購入までの所要期間をD+0日からD+90日経過まで表示します。

新規有料ユーザーが購入したすべてのアイテムを決済額順に表示します。

* アイテム：新規PUが購入したアイテムリスト
* ストア：AppStore、Google Playなどのストア
* 新規PU(NPU)：有料商品を初めて決済したユーザー(New Paying Users)
* 新規決済額：新規PUの決済額

## グループ同時接続
### グループ同時接続者

Gamebaseサービスユーザーが属するすべてのプロジェクトの同時接続指標を確認できます。

![gamebase_analytics_11_202004_2_ja](https://static.toastoven.net/prod_gamebase/gamebase_analytics_11_202004_2_ja.png)

* リアルタイムグループ同時接続：Gamebaseサービスユーザーが属するプロジェクトのリアルタイム同時接続者(CCU)を表します。
* プロジェクトグループ同時接続：選択された期間、フィルタを基準にアプリユーザー数を表示します。

### グループ比較指標

Gamebaseサービスユーザーが属するプロジェクトを、フィルタと組み合わせてグループで比較できます。

![gamebase_analytics_12_202004_2_ja](https://static.toastoven.net/prod_gamebase/gamebase_analytics_12_202004_2_ja.png)

* DAU：日間memberno基準でログイン1回以上のアクティブユーザー数(Daily Active Users)
* NRU：当日新規加入者
* PU：有料商品を決済したユーザー(Paying User)(=再購入PU + 新規PU)
* 決済額：ユーザーが決済した金額

※グラフに表示されるグループ名は**{appId} _ {OS} _ {国}**の形式です。

## 環境
### インストールURL

インストールURL呼び出しに対する統計指標を確認できます。

![gamebase_analytics_13_202004_2_ja](https://static.toastoven.net/prod_gamebase/gamebase_analytics_13_202004_2_ja.png)

* 過去1週間のダウンロードURL呼び出し数：[アプリ > インストールURL]のインストールパスに、ゲームインストールAPIを呼び出した顧客数。アプリインストールマーケティング用で、Short URL呼び出した顧客数を測定し、顧客の反応を確認できます。
* ブラウザ別占有率(全体累積)：ブラウザ別にインストールURL呼び出し数の比率を確認できます。
* プラットフォーム別占有率(全体累積)：プラットフォーム別にインストールURL呼び出し数の比率を確認できます。

## Transmission

**転送指標**タブは、ゲームからAPIで指標を転送する時に確認できます。
転送指標の種類は下記の3個です。

* 利用者レベル：利用者レベル別に接続と売上情報を確認できます。
* ワールド/サーバー/チャンネル：ワールド/サーバー/チャンネル別に接続と売上情報をUser IdUser Id確認できます。
* クラス/職業：クラス/職業別に接続と売上情報を確認できます。

> [参考] 
>
> ワールド/サーバー/チャンネル、クラス/職業は、事前に登録された情報のみ処理します。
> 登録方法は、次の文書を参照してください。
>
> - [アプリ > Analytics Indicator](./oper-app/#analytics-indicator)

### Concurrent Status

選択された転送指標の種類と、各日付の接続、売上情報を確認できます。
同時接続者は、当日はCCUを提供し、日付別はDAU情報を提供します。当日の場合は10分単位で情報が更新されます。

![gamebase_analytics_14_202004_1_ja](https://static.toastoven.net/prod_gamebase/gamebase_analytics_14_202004_1_ja.png)

* CCU (Concurrent User)：10分単位で測定されたリアルタイム同時接続者数(ログイン利用者数)
* DAU (Daily Active User)：利用者ID基準で、1日に1回以上ログインしたアクティブ利用者数
* NRU (New Registered User)：当日の新規加入者
* 決済件数：有料商品の決済件数
* 決済金額：有料商品の決済金額

### Status By Level

レベル別に接続、売上状況を確認できます。

![gamebase_analytics_15_202004_1_ja](https://static.toastoven.net/prod_gamebase/gamebase_analytics_15_202004_1_ja.png)

* DAU (Daily Active User)：利用者ID基準で、1日に1回以上ログインしたアクティブ利用者数
* Avg.Playtime：該当レベルの日付別の全Playtimeの平均(DAUのPlaytimeの合計 / DAU)
* 決済金額：有料商品の決済金額
* 新規決済：新規決済利用者(NPU)が決済した金額
* 再購入決済：再購入決済利用者が決済した金額
* PU (Paying User)：有料商品を決済した利用者。(=再購入PU + 新規PU)
* 新規PU：有料商品を初めて決済した利用者
* 再購入PU：全体PU - 新規PU(再購入PUは1日間のデータで、前日を基準に計算)

### Status By Channel

ワールド/サーバー/チャンネル別に接続、売上状況を確認できます。

![gamebase_analytics_16_202004_1_ja](https://static.toastoven.net/prod_gamebase/gamebase_analytics_16_202004_1_ja.png)

* DAU (Daily Active User)：利用者ID基準で、1日に1回以上ログインしたアクティブ利用者数
* Avg.Playtime：該当レベルの日付別の全Playtimeの平均(DAUのPlaytimeの合計 / DAU)
* 決済金額：有料商品の決済金額
* 新規決済：新規決済利用者(NPU)が決済した金額
* 再購入決済：再購入決済利用者が決済した金額
* PU (Paying User)：有料商品を決済した利用者(=再購入PU + 新規PU)
* 新規PU：有料商品を初めて決済した利用者
* 再購入PU：全体PU - 新規PU(再購入PUは1日間のデータで、前日を基準に計算)

### Status By Class

クラス/職業別に接続、売上状況を確認できます。

![gamebase_analytics_17_202004_1_ja](https://static.toastoven.net/prod_gamebase/gamebase_analytics_17_202004_1_ja.png)

* DAU (Daily Active User)：利用者ID基準で、1日に1回以上ログインしたアクティブ利用者数
* Avg.Playtime：該当レベルの日付別の全Playtimeの平均(DAUのPlaytimeの合計 / DAU)
* 決済金額：有料商品の決済金額
* 新規決済：新規決済利用者(NPU)が決済した金額
* 再購入決済：再購入決済利用者が決済した金額
* PU (Paying User)：有料商品を決済した利用者。(=再購入PU + 新規PU)
* 新規PU：有料商品を初めて決済した利用者
* 再購入PU：全体PU - 新規PU(再購入PUは1日間のデータで、前日を基準に計算)

### Level Up

利用者のレベルアップ情報を確認できます。

* 達成レベル：達成したレベル
* レベルアップ達成利用者：該当レベルを達成した利用者数
* レベルアップ平均達成時間(分)：該当レベルを達成した利用者の平均達成時間(分)

![gamebase_analytics_18_202004_1_ja](https://static.toastoven.net/prod_gamebase/gamebase_analytics_18_202004_1_ja.png)

### Item Sales Status

選択された転送指標の種類ごとにアイテム販売状況を確認できます。
**条件**ボタンをクリックし、下記の照会値を選択できます。

* 決済金額
* 決済件数
* PU (Paying User)
* 新規PU

![gamebase_analytics_19_202004_1_ja](https://static.toastoven.net/prod_gamebase/gamebase_analytics_19_202004_1_ja.png)


### Item Sales TOP 50

選択された転送指標の種類および値に応じて、アイテム販売上位50個の項目を確認できます。

![gamebase_analytics_20_202004_1_ja](https://static.toastoven.net/prod_gamebase/gamebase_analytics_20_202004_1_ja.png)
