## Game > Gamebase > 商店控制台指南 > MyCard控制台指南

## 注册MyCard应用程序   

- MyCard does not provide a web console where customers can log in. (As of July 2023)
- Integration with MyCard is processed according to the information entered in the NHN Cloud IAP console.
- Therefore, make sure you contact MyCard in advance on the information to be entered.

```
1. The information for App Name and Store App ID in the screen below will be linked to the facGameName and facGameId items of MyCard when processing payments.
 - When contacting MyCard, refer to the facGameName and facGameId.
 - Make sure you enter the correct information.
2. If the game is in Live, modifying the App Name and Store App ID may cause payment errors and failures.
 - Make sure you enter the correct information before going live.
```
![MyCard Store Information Registration](https://static.toastoven.net/prod_gamebase/StoreConsoleGuide/gamebase_iap_mycard_console_guide_en_01_230808.png)