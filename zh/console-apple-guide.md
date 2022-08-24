## Game > Gamebase > 商店控制台指南 > Apple控制台指南

当使用Apple订阅支付，您需要在App Store Connect中生成secret key并设置Notification url。
在IAP应用程序信息中注册Secret Key。
Apple一般商品支付不需要特殊设置。

> 参考
> https://help.apple.com/app-store-connect/#/devf341c0f01

## 创建shared secret key  
```
shared secret key可以创建为所有应用程序共用的主密钥，还可以为每个应用程序创建密钥。
在IAP应用程序信息中注册secret key。
```

### master shared secret key
```
1. App Store Connect
2. 点击[My Apps]。
3. 点击[Master Shared Secret]。   
```
![[]](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap-console-apple-shared-key-1.png)

<br>

### 各应用程序shared secret key
```
1. App Store Connect
2. 点击[My Apps] > 点击将要创建的[应用程序] > 点击toolbar的[Features]。
3. 点击[App-Specific Shared Secret]。
```
![[]](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap-console-apple-shared-key-2.png)


### 在IAP应用程序信息输入shared secret key
```
1. 选择在“Gamebase > 购买（IAP）>商店”菜单中注册的App store信息。
2. 输入Apple Shared Secret。
```
![[]](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap-console-apple-edit-gamebase.png)



## 注册Notification URL
```
1. App Store Connect > 我的应用程序 > 应用程序信息 > 一般信息 
2. 在Production服务器URL中注册IAP URL。
- URL : https://api-iap.cloud.toast.com/callback/subscription/{YOUR_PACKAGE_NAME}/AS
- {YOUR_PACKAGE_NAME} : app bundle id
```
- 必须注册Notification URL信息，才能在Gamebase中更新订阅、促销交换及退款信息。


