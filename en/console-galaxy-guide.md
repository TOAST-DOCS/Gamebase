<!-- pre-align:aligned sig=2e36ec0ee64f -->

<a id="game-gamebase-store-console-guide-galaxy-console-guide"></a>
## Game > Gamebase > Store Console Guide > Galaxy Console Guide { #game-gamebase-store-console-guide-galaxy-console-guide }

To use Galaxy Store in IAP, you should enter PackageName at app registration.

<a id="check-package-name"></a>
## Check Package Name { #check-package-name }
After binary file registration, Check the package name.

[Galaxy Store Seller Portal](https://seller.samsungapps.com/main/sellerMain.as) > App > Select App > Binary
 ![[]](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/StoreConsoleGuide/GalaxyStore/ko/galaxy_store_01_kr.png)
 

<a id="iap-public-key"></a>
## Create IAP Public Key { #iap-public-key }

> [Note]
> https://developer.samsung.com/iap/isn/requirements.html#Create-an-IAP-key-in-Seller-Portal

* [Galaxy Store Seller Portal](https://seller.samsungapps.com/main/sellerMain.as) > Seller Support > IAP Service > IAP Key > Create IAP Key

<a id="registering-app-from-the-console"></a>
## Registering app from the console { #registering-app-from-the-console }
Please enter Package Name in the Store App ID.
![[]](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/StoreConsoleGuide/GalaxyStore/en/store_info_registration_en_231226.png)
<a id="register-real-time-server-notification-isn"></a>
## Register Real-time Server Notification (ISN) { #register-real-time-server-notification-isn }

* App > Select an app > <strong>In App Purchase</strong> > More > <strong>Instant Server Notification (ISN)</strong>
![galaxy_isn](https://static.toastoven.net/prod_iap/console_galaxy/galaxy_isn.png)

* ISN url: ```https://api-iap.nhncloudservice.com/markets/GALAXY/notification/{Galaxy Store Package Name}/receive```
* If you're using the Gamebase sandbox, enter the ISN url as ```https://sandbox-api-iap.nhncloudservice.com/markets/GALAXY/notification/{Galaxy Store Package Name}/receive```

