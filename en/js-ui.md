## Game > Gamebase > JavaScript SDK User Guide > UI

## Alert

Displays a system alert.<br/>

### Simple Alert Dialog

Displays an alert dialog simply by entering a message.

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

Use the following API to call back the processed result after the alert dialog has been displayed.

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
| UI\_UNKNOWN\_ERROR                         | 6999          | Unknown error.(Undefined error).                      |

* Refer to the following document for all error codes.
    * [Error code](./error-code/#client-sdk)
