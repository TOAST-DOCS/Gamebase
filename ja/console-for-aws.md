## Game > Gamebase > Console for AWS

AWS marketplaceを介して会員登録を行ったユーザーがログインするまでのコンソールの基本設定と使用方法を説明します。
Gamebase for AWSのコンソール
は、以下のような機能を提供しています。
*プロジェクト、サービス管理
*サービスを利用する会員の管理

## クイックガイド

コンソールから提供している基本機能についてクイックガイドです。

![Gamebase-for-aws_quick-guide](https://static.toastoven.net/prod_gamebase/console-for-aws/console_Gamebase-for-aws_quick-guide-01-202108-ja.png)
![Gamebase-for-aws_quick-guide](https://static.toastoven.net/prod_gamebase/console-for-aws/console_Gamebase-for-aws_quick-guide-02-202108-ja.png)
![Gamebase-for-aws_quick-guide](https://static.toastoven.net/prod_gamebase/console-for-aws/console_Gamebase-for-aws_quick-guide-03-202108-ja.png)
![Gamebase-for-aws_quick-guide](https://static.toastoven.net/prod_gamebase/console-for-aws/console_Gamebase-for-aws_quick-guide-04-202108-ja.png)

## プロジェクトの管理

Gamebaseはプロジェクト単位で利用が可能で、これに従って課金を行います。

### プロジェクト生成
* Aws marketplaceのページを介して登録した管理者ユーザーのみプロジェクトを作成·修正·削除が可能です。
* プロジェクト名と説明を入力すると、プロジェクトのIDは自動的に作成されます。
* プロジェクトを作成するとGamebaseサービスは自動的に有効化されます。
* プロジェクト作成後に提携が必要な場合は、プロジェクトメンバーとして一般会員を追加できます


![Gamebase-for-aws_project-create](https://static.toastoven.net/prod_gamebase/console-for-aws/console_Gamebase-for-aws_project-01-202108-ja.png)
![Gamebase-for-aws_project-create](https://static.toastoven.net/prod_gamebase/console-for-aws/console_Gamebase-for-aws_project-02-202108-ja.png)

1.	+「新プロジェクト」ボタンをクリックしてプロジェクトを作成します。
2.	「プロジェクト名」と「プロジェクト説明」を入力します。
3.	「確認」ボタンをクリックしてプロジェクトを作成します。
4.	プロジェクトが作成されると、メニューにプロジェクト名が表示されます。
5.	「ダッシュボード」画面でプロジェクト情報を確認します。

### プロジェクト編集

プロジェクト詳細画面の右上の編集ボタンをクリックすると、プロジェクト編集画面が表示されます。
* 編集画面ではプロジェクト情報の修正·削除、利用中のサービスの無効化が可能です。
* プロジェクト名と説明は修正であるが、プロジェクトIDは修正不可です。
* プロジェクトで利用中のサービスがない場合はプロジェクトの削除は可能です。
* プロジェクトを削除すると、プロジェクトのリソースはすべて削除され復旧は不可能です。
	
### プロジェクト変更

* 上部にあるプロジェクト名をクリックすると、登録されたプロジェクトリストを確認できます。


## 会員 

| 区分     | 管理者 | 一般 | 
| ------ | ------------ | ------------ | 
| 定義     | プロジェクトの管理 | 該当サービスのみ利用可能 | 
| 登録方法 | aws marketplace 購読ページを介して加入  | 管理者のコンソールから作成した会員 | 
| 권한 | - プロジェクトの管理: 生成/修正/削除<br>- 一般会員管理
 | 権限が付与されているコンソールのみアクセス可能 | 

### プロジェクトへ一般会員追加

以下の手順でコンソールにアクセスできる一般会員を追加することが可能です。

#### 1. 一般会員を追加

![Gamebase-for-aws_create-ID](https://static.toastoven.net/prod_gamebase/console-for-aws/console_Gamebase-for-aws_member-01-202108-ja.png)

1.	**名前 > 一般グループ管理**をクリックすると、一般会員管理ページが表示されます。
2.	**+会員追加**ボタンをクリックします。
3.	ID、氏名、メールを入力すると、入力したメールアドレスにパスワード設定確認のメールが送信されます。
4.	受信したメールを利用してパスワードを変更すると、追加されたIDでログインが可能になります。


#### 2. プロジェクトメンバーに追加

![Gamebase-for-aws_project-member](https://static.toastoven.net/prod_gamebase/console-for-aws/console_Gamebase-for-aws_member-02-202108-ja.png)

1.	プロジェクトのダッシュボード画面で+プロジェクトメンバー追加ボタンをクリックします。
2.	1で追加した一般会員IDを入力して、グループ権限を付与し、現在のプロジェクトにアクセスすることができます。


### グループの権限管理

一般会員のサービスコンソールへのアクセス権限を管理できます。
**名前 > 一般/グループ管理**をクリックして、既存グループの管理ページにて権限グループの作成、修正、削除が可能です。

![Gamebase-for-aws_project-member](https://static.toastoven.net/prod_gamebase/console-for-aws/console_Gamebase-for-aws_member-03-202108-ja.png)
1. 追加しようとするグループ名を入力し、「追加」ボタンをクリックします。 
2. 追加されたグループ名を選択すると、画面の右側にグループの権限を修正できる画面が表示されます。 
3. グループ修正画面でサービスメニュー別にRead、ALLの権限付与が可能です。 付与しようとする権限をチェックし、保存ボタンをクリックします。

