## Mobile Service > IAP > Apple Console Guide

> [Notice]<br>
> New IAP SDK suppoting subscription are released as [TOAST SDK](http://docs.toast.com/ko/TOAST/ko/toast-sdk/overview/).<br>
> The existing IAP SDK will not be developing new features.<br>
> This documents describes [TOAST SDK](http://docs.toast.com/ko/TOAST/ko/toast-sdk/overview/) specifications.


To use App store subscription, you should create a secret key and set a notification url in App Store Connect.<br>
After that, register secret key into IAP app property.<br>
Consumable product payment does not require above things.<br>



> Reference<br>
> https://help.apple.com/app-store-connect/#/devf341c0f01

## Create a shared secret key
```
You may generate a master shared secret, which is single code for all of your apps, <br>
or an app-specific shared secret for individual apps. 
```

### master shared secret key

![[]](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap-console-apple-shared-key-1.png)

<br>

### App specific shared secret key

![[]](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap-console-apple-shared-key-2.png)


### Register shared secret key into IAP App.
![[]](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap-console-apple-edit.png)



## Notification url
```
1. App Store Connect > My Apps > select App > App Information 
2. Enter IAP url and click save.
- URL : https://api-iap.cloud.toast.com/callback/subscription/{YOUR_PACKAGE_NAME}/AS
- {YOUR_PACKAGE_NAME} : app bundle id
```

