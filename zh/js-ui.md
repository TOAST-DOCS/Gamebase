## Game > Gamebase > JavaScript SDK使用指南 > UI

## Alert

可显示系统警报。<br/>

### Simple Alert Dialog

可仅输入信息，显示简单的警报对话框。

```js
toast.Gamebase.Util.showAlert(message, () => { ...}));
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

显示警报对话框后，若想收到对处理结果的回拨，使用如下API。

```js
toast.Gamebase.Util.showConfirm(message, (isConfirmed) => { ...});
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
| UI\_UNKNOWN\_ERROR                         | 6999          | 未知错误（未定义的错误）。                      |

* 全部错误代码请参考如下文件。
    * [错误代码](./error-code/#client-sdk)
