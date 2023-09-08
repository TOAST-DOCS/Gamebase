## Game > Gamebase > Store Console Guide > Apple Console Guide

To use purchase of Apple subscription products, you need to generate a secret key and set the Notification URL in App Store Connect.
Register the secret key in the IAP app information.
Purchase of Apple consumable products does not require any specific settings.

> Reference
https://help.apple.com/app-store-connect/#/devf341c0f01

## Creating a Shared Secret Key
```
A shared secret key can be created as a master key common to all apps, or it can be created for each app.
Register the secret key in the IAP app information.
```

### Master Shared Secret Key
```
1. Go to App Store Connect
2. Click [My Apps]
3. Click [Master Shared Secret]
```
![[]](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap-console-apple-shared-key-1.png)

<br>

### Shared Secret Key for Each App
```
1. Go to App Store Connect
2. Click [My Apps] > Click [App] you want to create > Click [Features] on the toolbar
3. Click [App-Specific Shared Secret]
```
![[]](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap-console-apple-shared-key-2.png)


### Entering the Shared Secret Key in IAP App Information
```
1. In Gamebase > Purchase (IAP) > Store menu, select registered App store information
2. Enter Apple Shared Secret
```
![[]](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap-console-apple-edit-gamebase.png)


## Registering a Notification URL
**Notification V2** is not supported. Please use **V1**.
```
1. Go to App Store Connect > My Apps > App Information > General Information
2. Register the IAP URL in Production Server URL.
- URL: https://api-iap.cloud.toast.com/callback/subscription/{YOUR_PACKAGE_NAME}/AS
- {YOUR_PACKAGE_NAME} : app bundle id
```
- Notification URL information must be registered in order to make subscription, promotion exchange, and refund information updated in Gamebase.

