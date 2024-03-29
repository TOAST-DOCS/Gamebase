## 決済メニューを利用するにあたって
![purchase_01](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Purchase/jp/purchase_01_jp_240103.png)
決済メニューを利用するには決済指標のための通貨を先に選択する必要があります。
最初に一度だけ設定できます。Analytics売上指標には設定された通貨コードで指標が表示されます。
一度選択した通貨コードは変更できないため、慎重に選択してください。

## Game > Gamebase > コンソール使用ガイド > 決済

アプリ内決済に関連する情報を登録し、履歴を照会できます。
GamebaseではNHN Cloud IAP(In-App Purchase、アプリ内決済)サービスを使用します。

## Store

ゲーム内でアイテムを販売するためにストアを登録します。
**Store**タブの**ストア情報リスト**から新しいストアを登録したり、登録済みのストアを管理することができます。
![purchase_02](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Purchase/jp/purchase_02_jp_240103.png)

### Register

新しいストアを登録したい場合、**ストア情報リスト**画面の**登録**ボタンをクリックします。

![purchase_03](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Purchase/jp/purchase_03_jp_240103.png)

* **ストア**登録する外部ストアを選択します。登録するストアがない場合、[カスタマーセンター](https://toast.com/support/inquiry)までご連絡ください。
* **アプリ名**登録するゲームの名前を入力します。
* **ストアアプリID**ストアから発行された情報を入力します。
* **使用有無**ストアを使用するかどうかを選択します。

> [参考] Google Play Storeの領収書検証
>
> Google領収書検証システムに障害が発生した時は、**消費性商品の領収書検証設定**を1段階検証にして、Gamebase内部的なシグネチャ検証のみで正常に決済が可能です。
> 定期購入商品は、設定値に関係なく常に2段階検証を行います。

### Modify

照会リストから登録されたストアの詳細情報を照会したり、情報を変更することができます。

![purchase_04](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Purchase/jp/purchase_04_jp_240103.png)

- 照会リストから登録されたストアを選択すると、詳細情報を照会することができます。
- **修正**ボタンをクリックすればストアアプリIDを除くアプリ名、ストアアプリ、使用有無情報を修正することができます。
- **削除**ボタンをクリックすればストアの情報を削除することができます。ただし、使用有無の状態が未使用のストアのみ削除できます。

## Product
ストアで販売する商品を登録できます。
**商品**タブで新しい商品を登録したり、登録した商品を管理できます。

- (1) **フィルタ**：使用有無、ストア/ストアアイテムID/商品ID、商品名フィルタを提供して簡単に検索できます。検索値がない場合は、すべてのストアの商品リストが表示されます。
- (2) **登録**：1つのストアアイテムIDを利用して、複数の商品を登録できます。
- (3z) **ストアアイテムの状態変更**：1つのストアアイテムIDに登録された商品の使用有無を一度に変更できます。

![purchase_05](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Purchase/jp/purchase_05_jp_240103.png)

### Register

新しい商品を登録するには**商品リスト**画面の**登録**ボタンをクリックします。
#### 1. 直接入力を利用した登録方法
![purchase_06](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Purchase/jp/purchase_06_jp_240103.png)

* (1) **商品ID**：決済リクエスト時に使用する商品IDを入力します。該当IDを通してSDKで購入APIを呼び出すと入力した商品の購入が進行されます。
* (2) **商品名**：決済される商品の名前を入力します。この場所に入力した内容を元に決済履歴照会および指標で該当商品名が表示されます。
* (3) **使用有無**：商品の使用有無を選択します。SDKで商品リストをリクエストする時、使用有無が「USE」の商品だけが商品リストに伝達されます。
* (4) **商品追加**：追加商品を登録する時、**+**ボタンを選択すると商品入力欄が追加されます。
* (5) **ストア**：登録する外部ストアを選択します。登録するストアがない場合、**ストア**メニューで先にストアを登録する必要があります。
* (6) **ストアアイテムID**：ストア登録後に発行されたID情報を入力します。Gamebase商品に登録されたリストは選択したストアに決済をリクエストした時、この部分に入力した内容を利用して決済が行われます。
* (7) **ストアアイテムタイプ**：登録する商品タイプを選択します。Google Play、App Storeの場合、定期購入アイテムを登録できます。それ以外のストアを選択すると消費性アイテムとして登録されます。

#### 2. ファイルアップロードを利用した登録方法
![purchase_07](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Purchase/jp/purchase_07_jp_240103.png)

* ファイルをアップロードして商品を登録できます。
* ファイルをアップロードしての商品登録は、一度に最大1,000個まで可能です。
* 入力形式が正しくない場合、商品登録ができないため、入力形式に注意する必要があります。
* ファイルのエンコード形式が「UTF-8」ではない場合、日本語が正常に保存できないことがあります。
* ファイルの登録に失敗した場合、結果ウィンドウの**ダウンロード**から失敗したリストをダウンロードして確認できます。

### Modify

照会リストで、登録された商品の詳細情報の照会や情報の変更を行えます。
![purchase_08](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Purchase/jp/purchase_08_jp_240103.png)

- 照会リストで各アイテムを選択すると、登録されたアイテムの詳細情報を照会できます。
- **修正**ボタンをクリックすると、ストアとアイテム番号、商品タイプ以外の情報を変更できます。
- 修正が可能な項目は**商品名**、**使用有無**、**ストアアイテムID**項目です。それ以外の項目は修正できませんので、登録時に注意してください。
- **ストアアイテムID**はすでに登録されている別の**ストアアイテムID**にのみ修正が可能で、新規**ストアアイテムID**は商品登録する必要があります。

## Transactions

決済情報を照会できます。
検索タイプを自由に選択して決済情報を照会できます。
決済履歴照会結果は、右上の**ダウンロード**ボタンをクリックしていつでもダウンロードできます。

### Transaction Status Code
決済ステータスコードはユーザーが決済を進める過程で発生した状況を表すコードです。  

> [参考]
> 決済処理プロセスは大きく4段階に分かれます。
> 
> [予約] - [決済] - [検証] - [完了]
> 
> - 予約：ユーザーの決済を準備する段階で、決済に必要な情報をGamebaseに伝達します
> - 決済：マーケットで決済を進める過程
> - 検証：マーケットから伝達された結果を検証するプロセス
> - 完了：検証完了後にマーケットごとに必要な最後のプロセス

- 決済予約完了：[予約]段階が完了した状態
- 決済検証失敗： [検証]段階で検証が失敗した状態
- 決済検証完了： [検証]段階が完了した状態
- 決済完了：すべての決済プロセスが終わった状態
- 返金完了：ユーザー または運営者により返金された状態
- 進行中キャンセル：ユーザーが決済進行中にキャンセルした状態

> [参考]
> **決済予約完了**状態は、次のような理由で発生する可能性があります。
> - 決済画面でユーザーが決済をキャンセルした場合
> - 決済進行中にユーザーがアプリを終了した場合
> - 決済進行中にマーケットから応答が届かない場合(一時的なマーケットの障害、ユーザーのネットワーク切断など)
> - マーケットで決済処理が完了していない同じ商品を購入しようとした場合
> 
> 
> **決済検証完了**態から変更がない場合はサポートにお問い合わせください。

### 決済履歴照会
![purchase_09](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Purchase/jp/purchase_09_jp_240103.png)

#### カテゴリー

決済履歴は2つのカテゴリーで照会できます。

- **全体**:すべての決済履歴を照会
- **商品情報未登録決済**:決済は完了したが、商品情報が不足しているためアイテムの支給ができない決済履歴を照会 


#### Search conditions
選択した検索タイプに応じて表示される検索項目が異なります。

##### (1)一般検索
![purchase_10](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Purchase/jp/purchase_10_jp_240103.png)

下記の検索条件を満たす結果を検索できます。

- **検索期間**：ユーザーが購入を試行した期間、降順/昇順からソート方法を選択できる。
- **ストア**：検索するストア情報
- **ストアアイテムID**：ストアに登録した後に発行されたID情報
- **アイテム**：検索するアイテムを選択
- **ユーザーID**：決済したユーザーID
- **決済状態**：検索する決済状態基準


##### (2) Transaction ID検索
![purchase_11](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Purchase/jp/purchase_11_jp_240103.png)

決済時に生成されるTransaction IDを利用して検索できます。

##### (3)領収書検索
![purchase_12](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Purchase/jp/purchase_12_jp_240103.png)
決済時に支給された領収書情報を利用して検索できます。

#### 検索結果
#### [全体]検索結果
検索結果項目は以下の通りです。

![purchase_13](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Purchase/jp/purchase_13_jp_240103.png)

- **Transaction ID**：Gamebase内で決済を区別することができる固有番号
- **ストア**：決済されたストア情報
- **ユーザーID**：決済したユーザーID
- **商品名**：ユーザーがアプリで購入した実際の商品名
- **商品ID(ストアアイテムID)**：ユーザーがアプリで購入した実際の商品IDおよびストアで実際に決済されたストアアイテムID
- **商品タイプ**:ユーザーがアプリで購入した実際の商品タイプ
- **価格/通貨の種類**:ユーザーが購入したアイテムの価格および通貨の種類
- **消費状態**：決済したアイテムの支給状態
- **決済状態**：決済の現在進行状態
- **Store Reference Key**：ストアで発行する決済固有番号
- **決済予約日時**：ユーザーが購入を試行した時間
- **決済日時**：ユーザーが購入を完了した時間
- **返金日時**：ユーザーアイテムが返金された時間
- **追加情報**：SDKから決済リクエストした時に伝達した追加情報(Developer payload)

##### 決済状態変更
検索した決済情報の状態は以下の通りです。

- **決済完了(Success)**
    - 決済処理が正常に完了したことを意味します。
    - Refund状態に変更できます。
- **決済予約完了(Reserved)**
	- ストアでは決済がそれ以上行われないか、決済検証まで行われなかったことを意味します。
	- Success, Refund状態に変更できます。
      - ただし、Google Play StoreではSuccess状態の変更ができません。
- **決済検証失敗(Failure)**
	- ストアで決済を進行したが決済検証でエラーが発生したことを意味します。
	- Success、Refund状態に変更できます。
- **返金完了(Refund)**
	- 管理者が手動でストアから返金処理をアップデートしたことを意味します。
	- 他の決済状態に変更できます。
- **進行中キャンセル(UserClose)**
	- ユーザーが決済進行中にキャンセル


###### Success変更
![purchase_14](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Purchase/jp/purchase_14_jp_240103.png)
決済進行時に発行された**領収書番号**、**価格**、**通貨**情報を入力すると、状態を変更できます。

###### Refund変更
![purchase_15](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Purchase/jp/purchase_15_jp_240103.png)
追加情報を入力しないで状態を選択した後、変更を選択します。
変更された決済情報は、変更できないため、慎重に確認してください。

##### 領収書の検証

![purchase_16](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Purchase/jp/purchase_16_jp_240103.png)

* 照会された領収書の決済が有効かどうかを検証できます。
* 各フィールドを比較した結果を確認できます。ストアから受け取ったレスポンス値をJSON形式で提供するため、必要な場合はデータを直接確認できます。
* 現在はApp Store決済のみ検証できます。


##### 決済履歴照会
検索した決済情報のTransaction IDをクリックして決済履歴を照会できます。
![purchase_17](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Purchase/jp/purchase_17_jp_240103.png)

###### (1)付加情報および領収書照会
それぞれの決済状態ごとに右矢印をクリックすると、付加情報と領収書情報を確認できます。


#### [商品情報未登録決済]検索結果
検索結果項目は次のとおりです。

![purchase_18](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Purchase/jp/purchase_18_jp_240103.png)

- **Transaction ID**: Gamebase内で決済を区別できる固有番号
- **ストア**:決済されたストア情報
- **ユーザーID**:決済したユーザーID
- **商品名**:ユーザーがアプリで購入した実際の商品名
- **商品ID(ストアアイテムID)**:ユーザーがアプリで購入した実際の商品ID及びストアに実際に決済されたストアアイテムID
- **商品タイプ**:ユーザーがアプリで購入した実際の商品タイプ。
- **価格/通貨の種類**:ユーザーが購入したアイテムの価格及び通貨の種類
- **消費状態**:決済したアイテムの支給状況
- **決済状態**:決済の現在進行状態
- **Store Reference Key**:ストアで発行してくれる決済固有の番号。
- **追加情報**: SDKから決済リクエスト時に伝達した追加情報(Developer payload)
- **商品ID登録**:商品情報の手動登録

##### 商品ID登録
![purchase_19](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Purchase/jp/purchase_19_jp_240103.png)
* 不足しているアイテム情報を手動で選択して支給できます。 


## 決済アビューズモニタリング

決済に関する不正行為情報を照会して自動制裁/解除を設定することができます。

### 返金履歴照会

![purchase_20](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Purchase/jp/purchase_20_jp_240103.png)

下記の検索条件を利用して決済および返金情報を照会できます。
決済および返金履歴は、右上の**ダウンロード**ボタンをクリックしていつでもダウンロードできます。

#### 検索条件
- **返金日時**：返金処理された時間
- **ユーザーID**：決済したユーザーID
- **返金件数**：ユーザーが返金を受けた回数。入力した回数以上が照会されます。
- **返金金額**：ユーザーが返金を受けた金額。入力した金額以上が照会されます。
- **ストア**：返金したストア

#### 検索結果
- **ユーザーID**：決済したユーザーID
- **ストア**：決済したストア情報
- **返金件数**：ユーザーが返金を受けた回数
- **返金金額**：ユーザーが返金を受けた金額
- **決済件数(照会期間)**：ユーザーが照会期間内に決済した回数
- **決済金額(照会期間)**：ユーザーが照会期間内に決済した金額
- **決済件数(累積)**：ユーザーが決済した累積回数
- **決済金額(累積)**：ユーザーが決済した総額
- **決済件数(過去3か月)**：ユーザーが過去3か月間に決済した回数
- **決済金額(過去3か月)**：ユーザーが過去3か月に決済した金額
- **状態**：ユーザーの現在状態
- **状態変更**：ユーザーの状態に応じて利用停止または停止解除

#### 状態変更
![purchase_21](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Purchase/jp/purchase_21_jp_240103.png)

照会したゲームユーザーのアカウント状態を変更することができる機能です。
各状態からは、次のように変更できます。
- **正常**：利用停止、退会状態に変更できます。退会させると該当アカウントの情報を復元できないため、処理前によく確認してください。
- **利用停止**：利用停止の解除を行うことができます。
- **退会**：該当ボタンが表示されません。

#### 決済履歴の確認

検索されたリストからユーザーIDをクリックすると、検索期間の決済詳細履歴を照会できます。

![purchase_22](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Purchase/jp/purchase_22_jp_240103.png)

#### 決済履歴
- **決済予約日時**：ユーザーが購入を試行した時間
- **決済日時**：ユーザーが購入を完了した時間
- **返金日時**：ユーザーアイテムが返金された時間
- **Transaction ID**：Gamebase内で決済を区別することができる固有番号
- **ストア**：決済したストア情報
- **アイテム名**：ユーザーがアプリで購入した実際のアイテム名
- **価格**：ユーザーが購入したアイテムの価格
- **通貨単位**：ユーザーが購入時に使用した通貨の種類
- **決済状態**：決済の現在進行状態

### 決済アビューズ自動解除履歴照会

![purchase_23](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Purchase/jp/purchase_23_jp_240103.png)

以下の検索条件を利用して決済アビューズ自動解除ユーザー情報を検索できます。

#### 検索条件
- **検索期間**：利用停止猶予が始まった時間を基準に照会されます。
- **ユーザーID**：利用停止猶予ユーザーID
- **決済件数**：利用停止猶予期間中にユーザーが決済した回数です。入力した回数以上が照会されます。
- **決済金額**：利用停止猶予期間中にユーザーが決済した金額です。入力した金額以上が照会されます。

#### 検索結果
- **ユーザーID**：利用停止猶予ユーザーID
- **利用停止猶予期間**：利用停止猶予開始および終了時間
- **決済回数(解除条件)**：利用停止を解除するために決済しなければならない総回数
- **決済金額(解除条件)**：利用停止を解除するために決済しなければならない総額
- **解除条件を満たす**：解除条件を満たすための条件(AND、OR)
- **決済件数(猶予期間)**：利用停止猶予期間中にユーザーが決済した総累積回数
- **決済金額(猶予期間)**：利用停止猶予期間中にユーザーが決済した総額

#### 決済履歴確認

検索されたリストでユーザーIDをクリックすると、検索期間の決済詳細履歴を照会できます。
(ただし、決済履歴がないユーザーはクリックできません。)

![purchase_24](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Purchase/jp/purchase_24_jp_240103.png)

#### 決済履歴
- **決済日時**：ユーザーが購入を完了した時間
- **Transaction ID**：Gamebase内で決済を区別することができる固有の番号
- **ストア**：決済されたストア情報
- **ユーザーID**：決済したユーザーID
- **ストアアイテムID**：ユーザーがアプリで購入した実際のストアアイテムID
- **決済金額**：ユーザーが決済した金額

#### 決済に関する不正行為の自動制裁設定

自動制裁設定を使用するには、**使用**ボタンをクリックして設定値を入力します。

![purchase_25](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Purchase/jp/purchase_25_jp_240103.png)

#### 設定情報

![purchase_26](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Purchase/jp/purchase_26_jp_240103.png)

* **利用停止期間**自動制裁適用時の利用停止期間を入力します。
    * **永久停止**：永久に利用を停止する時に選択します。
    * **期間指定**：利用停止期間を入力します。日(day)
* **自動制裁条件設定**設定された条件に一致するユーザーは自動制裁処理されます。最低1個以上を設定する必要があります。
    * **返金件数**：不正行為に該当する返金件数を入力します。
    * **返金金額**：不正行為に該当する返金額を入力します。
* **自動制裁メッセージ設定**
    * ユーザーに表示する利用停止メッセージを入力します。
    * ユーザーに表示するメッセージを多言語で入力して簡単に再使用できるようにテンプレートを提供します。あらかじめ登録したテンプレートを選択して登録します。
* **リーダーボードの削除**
    * 自動制裁を行う時、該当ゲームユーザーのLeaderboardデータも同時に削除するかどうかを設定します。
    * 選択して登録すると、自動制裁適用時にリーダーボードからゲームユーザーのデータが削除されます。<font color="red">該当データは復旧できないため、</font> 注意が必要です。

#### 決済アビューズ自動解除設定

自動解除設定を使用するには**使用**ボタンをクリックして設定値を入力します。
自動解除設定を有効にするには、自動制裁設定が<font color="red">有効</font>になっている必要があります。

![purchase_27](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Purchase/jp/purchase_27_jp_240103.png)

#### 設定情報

![purchase_28](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Purchase/jp/purchase_28_jp_240103.png)

* **利用停止日時解除期間**：自動解除の適用時、利用停止猶予期間を入力します。
* **利用停止解除条件設定**：自動解除に必要な条件を設定します。1つ以上設定する必要があります。
    * **決済件数**：自動解除のために決済しなければならない件数を入力します。
    * **決済金額**：自動解除のために決済しなければならない金額を入力します。
* **利用停止解除メッセージ**
    * ユーザーに表示する利用停止解除メッセージを入力します。
    * ユーザーに表示するメッセージを多言語で入力して簡単に再使用できるようにテンプレートを提供します。あらかじめ登録したテンプレートを選択して登録します。
