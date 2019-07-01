## Game > Gamebase > JavaScript SDK使用ガイド > UI

## Alert

システム通知を表示できます。<br/>

### Simple Alert Dialog

メッセージのみ入力し、簡単に通知ダイアログボックスを表示できます。

```js
toast.Gamebase.Util.showAlert(message, () => { ... }));
```

**Example**
```js
function showAlert() {
    toast.Gamebase.Util.showAlert('message', function() {
		console.log('Alert Closed');
    });
}
```

### Alert Dialog with Callback

通知ダイアログボックスを表示した後、処理結果をコールバックで受信したい場合は、次のAPIを使用します。

```js
toast.Gamebase.Util.showConfirm(message, (isConfirmed) => { ... });
```

**Example**
```js
function showConfirm() {
    toast.Gamebase.Util.showConfirm('message', function (isConfirmed) {
        if (isConfirmed) {
            console.log('OK Button Clicked');
        } else {
            console.log('Cancel Button Clicked');
        }
    });
}
```

## Error Handling

| Error                                      | Error Code    | Description                                               |
| ------------------------------------------ | ------------- | --------------------------------------------------------- |
| UI\_UNKNOWN\_ERROR                         | 6999          | 不明なエラーです(定義されていないエラーです)。                      |

* エラーコードの一覧は、次の文書を参照してください。
    * [エラーコード](./error-code/#client-sdk)
