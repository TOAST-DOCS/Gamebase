## Game > Gamebase > Console ご利用ガイド > 運営指標

アプリを利用するユーザーの現状を指標及びグラフで確認することができます。
メニュー構成は、モニタリング、グループ同時接続、インストールURL統計、販売現状です。


## Monitoring
![operation-indicator_01_201812](https://static.toastoven.net/prod_gamebase/operation-indicator_01_201812.png)
現在、アプリを利用しているユーザーの全体統計及び現在予約されているPush現状、予約されているメンテナンス内訳を確認することができます。
5分が経過すると、自動で画面が「更新」され、リアルタイムで変更された指標を確認することができます。

* 基本指標
	* CCU(concurrent connected users)：リアルタイムの同時接続者数
	* MCU(maximum concurrent users)：一日の最大同時接続数(リアルタイム、日付ごとの照会が可能)
	* DAU(daily active users)：一日の間ゲームを使用した純ユーザー数(リアルタイム、日付ごとの照会が可能)
	* NRU(new registered users)：一日の新規ユーザー数(リアルタイム、日付ごとの照会が可能)
* 占有率チャート：ゲームユーザー占有率のパイチャート
	* OSごと：Android、iOS、WebGLなど
	* 国家ごと：SDKから取得したUSIM国家ベース
	*バージョンごと：Consoleに登録されたクライアントバージョンごとの占有率
* 同時接続変化グラフ：当日の00:00から現在時刻までの同時接続変化グラフ
	* メンテナンス及びPushによる同時接続数の変化が確認しやすいようにメンテナンス、Pushの内訳をグラフに別途表示しています。
	* 관련데이터는 검색조건 우측의 버튼을 통해 다운로드를 받을 수 있습니다.

## User Statistics
![operation-indicator_02_201812](https://static.toastoven.net/prod_gamebase/operation-indicator_02_201812.png)
DAU、MCU、NRU、CCU AVGの現状をグラフで確認することができます。
現在、アプリを使用しているゲームユーザーの推移の変化を一目で確認することができます。
右上の期間選択バーを利用して確認したい日付を選択して確認することもできます。
各項目ごとの説明は、次の通りです。

* 項目に対する説明
	* DAU(daily active users)：一日の間ゲームを使用した純ユーザー数(リアルタイム、日付ごとの照会が可能)
	* MCU(maximum concurrent users)：一日の最大同時接続者数(リアルタイム、日付ごとの照会が可能)
	* NRU(new registered users)：一日の新規ユーザー数(リアルタイム、日付ごとの照会が可能)
	* CCU AVG(concurrent connected users average)：リアルタイムの同時接続者数の平均値

## Concurrent Group User
![operation-indicator_03_201812](https://static.toastoven.net/prod_gamebase/operation-indicator_03_201812.png)
自分が属したプロジェクトのグループ同時接続統計を確認することができます。権限のある複数のプロジェクトのOSごとのリアルタイムの同時接続者数を一目で確認できます。


## Installed URL Statistics
![operation-indicator_04_201812](https://static.toastoven.net/prod_gamebase/operation-indicator_04_201812.png)
インストールURLの呼び出しに対する統計データを確認することができます。

* 日付ごとのインストールURL呼び出し数の変化グラフ
* ブラウザごとの占有率：Internet Explorer、Chromeなど
* プラットフォームごとの占有率：Android、iOSなど
  

## Statistics
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Monitoring_Statistics1_1.2.png)
販売現状画面を通じてアプリの売上現状を簡単に確認することができます。
右上の**通貨**から確認したい通貨を選択して通貨ごとの売上高を確認することもできます。

#### (1) 日付ごとの販売現状統計グラフ
日付ごとの販売現状と推移を折れ線グラフで簡単に把握することができます。

#### (2) 月ごとの売上現状
一ヶ月単位または現在進行している当月の全体の売上をストアごと及び総合データで統計を出し、表示します。

#### (3) 日付ごとの売上現状
アプリから登録したストアごとの売上現状を日付ごとに照会することができます。
その月の当日のデータまですべて照会することができます。
月ごとに確認するものより詳細な売上現状を確認することができます。イベントの進行期間または問題が発生したときの売上現状を比較して分析することができます。