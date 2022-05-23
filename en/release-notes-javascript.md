## Game > Gamebase > Release Notes > JavaScript

### 2.19.1 (2021.02.09) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.19.1/GamebaseSDK-JavaScript.zip)

#### Bug Fixes
* Fixed an error that occurs during initialization when console has no Customer Center information

### 2.19.0 (December 29, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.19.0/GamebaseSDK-JavaScript.zip)

#### More Features
* [SDK] 2.19.0
	* (Common) Weibo authentication added
	
#### Feature Updates
* [SDK] 2.19.0
	* (Common) Launching status code added: beta service (205)

### 2.15.0 (September 15, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.15.0/GamebaseSDK-JavaScript.zip)

#### More Features
* [SDK] 2.15.0
    * (JavaScript) Added GamebaseProductId to Purchase API of Hangame points
    
### 2.10.1 (June 9, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.10.1/GamebaseSDK-JavaScript.zip)

#### Bug Fixes
* [SDK] 2.10.1
	* (JavaScript) Fixed errors that occur if StoreCode is not entered during initialization

### 2.10.0 (May 26, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.10.0/GamebaseSDK-JavaScript.zip)

#### More Features
* [SDK] 2.10.0
	* (Common) Added GamebaseEventHandler which has all previous event systems 
		* Includes ServerPush and Observer, and checks promotional purchase or push events 

### 2.9.0 (April 28, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.9.0/GamebaseSDK-JavaScript.zip)

#### More Features
* Suspension of Membership Withdrawal 
	* [SDK 2.9.0]
		* (Common) Added API: Apply for suspension of withdrawal, Cancel application for suspension of withdrawal, Immediately withdraw while on suspension, and Check if user's withdrawal is suspended  

### 2.8.1 (April 14, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.8.1/GamebaseSDK-JavaScript.zip)
	
#### Bug Fixes
* [SDK] 2.8.1 
	* (JavaScript) Modified an issue in which credentialInfo login is unavailable with Hangame IdP

### 2.8.0 (March 24, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.8.0/GamebaseSDK-JavaScript.zip)

#### More Features 
* [SDK] 2.8.0
 	* (Javascript) Supports Hangame channeling: Hangame IdP authentication and purchase with Hancoin 

#### Feature Updates 
* [SDK] 2.8.0 
	* (Common) Updated to further show a popup to move to stores when it fails to initialize on an app version not registered on console 

### 2.6.2 (December 24, 2019)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.6.2/GamebaseSDK-JavaScript.zip)

#### Features Updates/Changes
* [SDK] 2.6.2
	* 불필요한 오류 로그 제거

### 2.4.4 (July 23, 2019)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.4/GamebaseSDK-JavaScript.zip)

#### Feature Updates
* [SDK] 2.4.4
	* (Common) Format changed for member error code
	
### 2.4.2 (June 25, 2019)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.2/GamebaseSDK-JavaScript.zip)

#### Features Updates/Changes
* [SDK] 2.4.2
	* (Common) Add TOAST Launching information in the JSON string format to LaunchingInfo


### 2.4.0 (May 28, 2019)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.0/GamebaseSDK-JavaScript.zip)

#### Feature Updates/Changes
* [SDK] 2.4.0
  * (Common) Change of Classes Relevant to Indicators 
        * LevelUpData Class: Changed userLevel and levelUpTime as required parameters; the other fields are deleted [See Details: [Android](./aos-etc/#game-user-data-settings) / [iOS](./ios-etc/#game-user-data-settings) / [Unity](./unity-etc/#game-user-data-settings) / [JavaScript](./js-etc/#game-user-data-settings)]
            * GameUserData Class: Added the classId (game user's profession) field [See Details: [Android](./aos-etc/#level-up-trace) / [iOS](./ios-etc/#level-up-trace) / [Unity](./unity-etc/#level-up-trace) / [JavaScript](./js-etc/#level-up-trace)]

### 2.0.0 (2019.01.29)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.0.0/GamebaseSDK-JavaScript.zip)

```
Gamebase 2.0의 개선된 전체 지표를 활용하기 위해서는 SDK 업데이트가 필요합니다.
```

#### 기능 추가
* [SDK] 2.0.0
	* (공통)Custom 지표를 위한 API 추가 (구매 성공의 경우 SDK내부에서 자동 전송)
		* setGameUserData : 게임 로그인 이후 유저 레벨 정보 전송
		* traceLevelUpData : 레벨업 추적을 위하여 게임 유저의 레벨업이 되었을 때 호출
    * (JavaScript)SDK 신규 배포