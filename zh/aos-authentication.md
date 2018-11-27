## Game > Gamebase > Android SDK使用指南 > 认证

## 登录

Gamebase默认支持Guest登录。

* 使用游客以外的Provider登录，需要Provider AuthAdapter。
* AuthAdapter和第三方提供的SDK设置，请参考以下内容。
  [第三方提供的SDK指南](aos-started#3rd-party-provider-sdk-guide)


### 登录流程

多数游戏在标题页上实现登录。
* 当App首次安装和启动时，游戏用户可以在标题页选择要进行的IdP(identity provider)类型。
* 登录过一次后，您将不会看到IdP选择画面，将使用之前登录的IdP类型进行认证。

上述逻辑可以按以下顺序实现。

![auth flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_001_1.10.0.png)
![auth flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_002_1.10.0.png)
![auth flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_003_1.10.0.png)
![auth flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_004_1.10.0.png)
![auth flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_005_1.10.0.png)
![auth flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_006_1.10.0.png)

#### 1. 按上一次的登录类型认证

* 如果存在已做过的认证的记录，则尝试进行认证，不需要输入ID和密码。
* 调用**Gamebase.loginForLastLoggedInProvider()**。

#### 1-1. 如果认证成功

* 恭喜！认证成功。
* 可以使用**Gamebase.getUserID()**获取用户ID并实现游戏逻辑。

#### 1-2. 如果认证失败

* 网络错误
    * 由于突发的网络问题，认证失败，错误代码为 **SOCKET_ERROR(110)** 或 **SOCKET_RESPONSE_TIMEOUT(101)**。需要重新调用**Gamebase.loginForLastLoggedInProvider()**，或稍后再试。
* 禁用游戏用户。
    * 由于用户是禁用状态，认证失败，错误代码为**BANNED_MEMBER(7)**。
    * 请使用**Gamebase.getBanInfo()**确认制裁信息，并告知游戏用户无法进行游戏的原因。
    * 初始化Gamebase 时调用**GamebaseConfiguration.Builder.enablePopup(true)** 和**enableBanPopup(true)**，Gamebase会自动弹出禁用的窗口。
* 其他错误
    * 因为使用上一次的登录类型认证失败，请进行**3.使用指定的IdP进行认证**。

#### 2. 使用指定的IdP进行认证

* 通过直接指定IdP类型来尝试进行认证。
    * 可认证的类型在**AuthProvider**类中声明。
* 调用**Gamebase.login(activity, idpType, callback)**API。

#### 2-1. 如果认证成功

* 恭喜！ 认证成功。
* 可以用**Gamebase.getUserID()**获取用户ID并实现游戏逻辑。

#### 2-2. 如果认证失败

* 网络错误
    * 由于突发的网络问题，认证失败，错误代码为 **SOCKET_ERROR(110)**或**SOCKET_RESPONSE_TIMEOUT(101)**，需要重新调用 **Gamebase.login(activity, idpType, callback)**或稍后再试。
* 禁用游戏用户。
    * 由于用户是禁用状态，认证失败，错误代码为**BANNED_MEMBER(7)**。
    * 请使用**Gamebase.getBanInfo()**确认制裁信息，并告知游戏用户无法进行游戏的原因。
    * 初始化Gamebase 时调用**GamebaseConfiguration.Builder.enablePopup(true)** 和 **enableBanPopup(true)**，Gamebase会自动弹出禁用窗口。
* 其他错误
	* 通知游戏用户发生了错误，并返回到游戏用户可以选择认证IdP类型的页面（通常是标题页面或登录页面）。

### Login as the Latest Login IdP

尝试使用最近登录的IdP登录。<br/>
如果该登录的令牌已过期，或者令牌认证失败，则返回失败。<br/>
此时，须实现对该IdP的登录。


**API**

```java
+ (void)Gamebase.loginForLastLoggedInProvider(Activity activity, GamebaseDataCallback<AuthToken> callback);
```

**示例**

```java
Gamebase.loginForLastLoggedInProvider(activity, new GamebaseDataCallback<AuthToken>() {
    @Override
    public void onCallback(AuthToken data, GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            // 登录成功
            Log.d(TAG, "Login successful");
            String userId = Gamebase.getUserID();
        } else {
            if (exception.getCode() == GamebaseError.SOCKET_ERROR ||
                    exception.getCode() == GamebaseError.SOCKET_RESPONSE_TIMEOUT) {
                // Socket error 表示网络暂时不可用。
                // 请确认网络状态或稍后再试。
                new Thread(new Runnable() {
                    @Override
                    public void run() {
                        try {
                            Thread.sleep(2000);
                            onLoginForLastLoggedInProvider(activity);
                        } catch (InterruptedException e) {}
                    }
                }).start();
            } else if (exception.getCode() == GamebaseError.BANNED_MEMBER) {
                // 尝试登录的游戏用户已被禁用。
                // 如果调用GamebaseConfiguration.Builder.enablePopup(true).enableBanPopup(true)
                // Gamebase自动弹出禁用窗口。
                //
                // 如果要根据游戏UI实现禁用弹出窗口，请使用Gamebase.getBanInfo()
                // 确认制裁信息并告知游戏用户无法进行游戏的原因。
                BanInfo banInfo = Gamebase.getBanInfo();
            } else {
                // 如果发生其他错误，则尝试使用指定的IdP进行认证。
                Gamebase.login(activity, provider, logincallback);
            }
        }
    }
});
```
### 游客登录

Gamebase支持游客登录。

* 尝试通过为设备创建的唯一密钥来登录Gamebase。
* 建议基于IdP的登录方式，因为游客登录方式，在删除App或设备初始化时，账户可能会被删除。

请参考下面的示例代码，了解如何实现游客登录。


**API**

```java
+ (void)Gamebase.login(Activity activity, AuthProvider.GUEST, GamebaseDataCallback<AuthToken> callback);
```

**示例**

```java
private static void onLoginForGuest(final Activity activity) {
    Gamebase.login(activity, AuthProvider.GUEST, new GamebaseDataCallback<AuthToken>() {
        @Override
        public void onCallback(AuthToken data, GamebaseException exception) {
            if (Gamebase.isSuccess(exception)) {
                // 登录成功
                Log.d(TAG, "Login successful");
                String userId = Gamebase.getUserID();
            } else {
                if (exception.getCode() == GamebaseError.SOCKET_ERROR ||
                        exception.getCode() == GamebaseError.SOCKET_RESPONSE_TIMEOUT) {
                    //  Socket error 表示网络暂时不可用。
                    // 请确认网络状态或稍后再试。
                    new Thread(new Runnable() {
                        @Override
                        public void run() {
                            try {
                                Thread.sleep(2000);
                                onLoginForGuest(activity);
                            } catch (InterruptedException e) {}
                        }
                    }).start();
                } else if (exception.getCode() == GamebaseError.BANNED_MEMBER) {
                    // 尝试登录的游戏用户已被禁用。
                    // 如果调用GamebaseConfiguration.Builder.enablePopup(true).enableBanPopup(true)
                    // Gamebase自动弹出禁用窗口。
                    //
                    //如果要根据游戏UI实现禁用弹出窗口，请使用Gamebase.getBanInfo()
                    // 确认制裁信息并告知游戏用户无法进行游戏的原因。
                    BanInfo banInfo = Gamebase.getBanInfo();
                } else {
                    // 登录失败
                    Log.e(TAG, "Login failed- "
                            + "errorCode: " + exception.getCode()
                            + "errorMessage: " + exception.getMessage());
                }
            }
        }
    });
}
```


### IdP登录

以下是允许您使用特定IdP登录的示例代码。<br/>
您可以在** AuthProvider **类中确认可以登录的IdP类型。

**API**

```java
+ (void)Gamebase.login(Activity activity, AuthProvider provider, GamebaseDataCallback<AuthToken> callback);
```

**示例**

```java
private static void onLoginForGoogle(final Activity activity) {
    Gamebase.login(activity, AuthProvider.GOOGLE, new GamebaseDataCallback<AuthToken>() {
        @Override
        public void onCallback(AuthToken data, GamebaseException exception) {
            if (Gamebase.isSuccess(exception)) {
                // 登录成功
                Log.d(TAG, "Login successful");
                String userId = Gamebase.getUserID();
            } else {
                if (exception.getCode() == GamebaseError.SOCKET_ERROR ||
                        exception.getCode() == GamebaseError.SOCKET_RESPONSE_TIMEOUT) {
                    // Socket error 表示网络暂时不可用。
                    //  请确认网络状态或稍后再试。
                    new Thread(new Runnable() {
                        @Override
                        public void run() {
                            try {
                                Thread.sleep(2000);
                                onLoginForGoogle(activity);
                            } catch (InterruptedException e) {}
                        }
                    }).start();
                } else if (exception.getCode() == GamebaseError.BANNED_MEMBER) {
                    // 尝试登录的游戏用户已被禁用。
                    // 如果调用GamebaseConfiguration.Builder.enablePopup(true).enableBanPopup(true)
                    // Gamebase自动弹出禁用窗口
                    //
                    //如果要根据游戏UI实现禁用弹出窗口，请使用Gamebase.getBanInfo()
                    // 确认制裁信息并告知游戏用户无法进行游戏的原因。
                    BanInfo banInfo = Gamebase.getBanInfo();
                } else {
                    // 登录失败
                    Log.e(TAG, "Login failed- "
                            + "errorCode: " + exception.getCode()
                            + "errorMessage: " + exception.getMessage());
                }
            }
        }
    });
}
```

### Credential登录

是通过IdP提供的SDK在游戏中进行认证后，并使用获取到的访问令牌，登录到Gamebase的接口。

* Credential参数设定方法

| keyname                                  | a use                                    | 值类型                                     |
| ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| AuthProviderCredentialConstants.PROVIDER_NAME | 设定IdP 类型                               | AuthProvider.GOOGLE<br> AuthProvider.FACEBOOK<br>AuthProvider.PAYCO<br>AuthProvider.NAVER |
| AuthProviderCredentialConstants.ACCESS_TOKEN | 设置登录IdP后收到的认证信息（访问令牌）<br/>不用于Google认证|                                          |
| AuthProviderCredentialConstants.AUTHORIZATION_CODE | 输入登录Google后可以获取的OTAC(一次性验证码) |                                          |

> [参考]
>
> 当要在游戏中使用外部服务（例如Facebook）的特有功能时，可能需要它。
>

<br/>

> <font color="red">[注意]</font><br/>
>
> 外部SDK要求的开发事项需使用外部SDK的API实现，Gamebase不支持。
>

**API**

```java
+ (void)Gamebase.login(Activity activity, Map<String, Object> credentialInfo, GamebaseDataCallback<AuthToken> callback);
```

**示例**

```java
private static void onLoginWithCredential(final Activity activity) {
    Map<String, Object> credentialInfo = new HashMap<>();
    credentialInfo.put(AuthProviderCredentialConstants.PROVIDER_NAME, AuthProvider.FACEBOOK);
    credentialInfo.put(AuthProviderCredentialConstants.ACCESS_TOKEN, facebookAccessToken);

    Gamebase.login(activity, credentialInfo, new GamebaseDataCallback<AuthToken>() {
        @Override
        public void onCallback(AuthToken data, GamebaseException exception) {
            if (Gamebase.isSuccess(exception)) {
                // 登录成功
                Log.d(TAG, "Login successful");
                String userId = Gamebase.getUserID();
            } else {
                if (exception.getCode() == GamebaseError.SOCKET_ERROR ||
                        exception.getCode() == GamebaseError.SOCKET_RESPONSE_TIMEOUT) {
                    // Socket error 表示网络暂时不可用。
                    // 请确认网络状态或稍后再试。
                    new Thread(new Runnable() {
                        @Override
                        public void run() {
                            try {
                                Thread.sleep(2000);
                                onLoginWithCredential(activity);
                            } catch (InterruptedException e) {}
                        }
                    }).start();
                } else if (exception.getCode() == GamebaseError.BANNED_MEMBER) {
                    // 尝试登录的游戏用户已被禁用。
                    // 如果调用GamebaseConfiguration.Builder.enablePopup(true).enableBanPopup(true)
                    // Gamebase自动弹出禁用窗口。
                    //
                    // 如果要根据游戏UI实现禁用弹出窗口，请使用Gamebase.getBanInfo()
                    // 确认制裁信息并告知游戏用户无法进行游戏的原因
                    BanInfo banInfo = Gamebase.getBanInfo();
                } else {
                    // 登录失败
                    Log.e(TAG, "Login failed- "
                            + "errorCode: " + exception.getCode()
                            + "errorMessage: " + exception.getMessage());
                }
            }
        }
    });
}
```

### 认证附加信息设定

[控制台使用指南](./oper-app/#authentication-information)

## 退出登录

尝试从登录中的IdP退出。 通常，在游戏的设置画面有退出登录（退出账号）按钮，然后点击该按钮执行。
即使退出登录成功，也会保留游戏用户数据。
如果退出登录成功，将会删除IDP认证记录，则下次登录时将显示ID和密码输入窗口。<br/><br/>

以下是点击“退出登录”按钮时的示例代码。

**API**

```java
+ (void)Gamebase.logout(Activity activity, GamebaseCallback callback);
```

**示例**

```java
private static void onLogout(final Activity activity) {
    Gamebase.logout(activity, new GamebaseCallback() {
        @Override
        public void onCallback(GamebaseException exception) {
            if (Gamebase.isSuccess(exception)) {
            	// 退出登录成功
                Log.d(TAG, "Logout successful");
            } else {
                if (exception.getCode() == GamebaseError.SOCKET_ERROR ||
                        exception.getCode() == GamebaseError.SOCKET_RESPONSE_TIMEOUT) {
                    // Socket error 表示网络暂时不可用。
                    // 请确认网络状态或稍后再试。
                    new Thread(new Runnable() {
                        @Override
                        public void run() {
                            try {
                                Thread.sleep(2000);
                                onLogout(activity);
                            } catch (InterruptedException e) {}
                        }
                    }).start();
                } else {
                    // 退出登录失败
                    Log.e(TAG, "Logout failed- "
                            + "errorCode: " + exception.getCode()
                            + "errorMessage: " + exception.getMessage());
                }
            }
        }
    });
}
```

## 退出（删除数据）

以下是游戏用户登录状态下，“退出（删除数据）”的示例代码。<br/><br/>

* 如果退出（删除数据）成功，则将删除与登录的IdP账户相关联的游戏用户数据。
* 您可以使用该IdP重新登录并生成新的游戏用户数据。
* 这意味着退出Gamebase，并不是退出IdP帐户。
* 成功退出（删除数据）时，也将退出IdP登录。

> <font color="red">[注意]</font><br/>
>
> 如果您正在使用多个IdP，则所有IdP都将被解除关联，Gamebase用户数据将被删除。
>

**API**

```java
+ (void)Gamebase.withdraw(Activity activity, GamebaseCallback callback);
```

**示例**

```java
private static void onWithdraw(final Activity activity) {
    Gamebase.withdraw(activity, new GamebaseCallback() {
        @Override
        public void onCallback(GamebaseException exception) {
            if (Gamebase.isSuccess(exception)) {
            	// 成功退出（删除数据）
                Log.d(TAG, "Withdraw successful");
            } else {
                if (exception.getCode() == GamebaseError.SOCKET_ERROR ||
                        exception.getCode() == GamebaseError.SOCKET_RESPONSE_TIMEOUT) {
                    // Socket error 表示网络暂时不可用。
                    //请确认网络状态或稍后再试。
                    new Thread(new Runnable() {
                        @Override
                        public void run() {
                            try {
                                Thread.sleep(2000);
                                onWithdraw(activity);
                            } catch (InterruptedException e) {}
                        }
                    }).start();
                } else {
                    // 退出（删除数据）失败
                    Log.e(TAG, "Withdraw failed- "
                            + "errorCode: " + exception.getCode()
                            + "errorMessage: " + exception.getMessage());
                }
            }
        }
    });
}
```

## 映射（Mapping）

映射（Mapping）是将已登录的现有游戏帐号和IdP帐户关联或解除关联的功能。

在大多数游戏中，可以将多个IdP帐户映射（Mapping）到同一个游戏账户。<br/>Gamebase的Mapping API，允许将您的其他IdP帐户和您之前登录的游戏帐户进行关联或解除关联操作。

换句话说，当您尝试使用同步的IdP帐户登录时，可以始终使用相同的用户ID登录。<br/><br/>

请注意，每个IdP只能关联一个同域名帐户。<br/>
例如，如果您已经关联了Google帐户，则无法添加其他Google帐户。<br/>
以下是帐户关联的示例。<br/><br/>

* Gamebase 用户ID: 123bcabca
    * Google ID: aa
    * Facebook ID: bb
    * Apple Game Center ID: cc
    * Payco ID: dd
* Gamebase 用户 ID : 456abcabc
    * Google ID: ee
    * Google ID: ff **-> 由于您已关联Google ee帐户，因此无法添加其他Google帐户。**

Mapping API中有添加映射和解除映射的功能。

### 添加映射（Mapping）的流程

映射（Mapping）可以按以下顺序实现。

#### 1. 登录
Mapping是为当前帐户添加IdP帐户链接，因此您必须先登录。
首先通过调用登录API登录。

#### 2. 映射（Mapping）

调用**Gamebase.addMapping(activity, idpType, callback)**尝试进行映射（Mapping）。

#### 2-1. 如果映射（Mapping）成功

* 恭喜！ 已成功添加与当前账户关联的IdP帐户。
* 映射（Mapping）成功后，“当前登录的IdP”也不会更改。 换句话说，在已使用Google帐户登录后，成功映射（Mapping）到您的Facebook帐户，也不会将您“当前登录的IdP”从Google更改为Facebook，会保持Google的状态。
* 映射（Mapping）只是添加IdP关联。

#### 2-2. 如果映射（Mapping）失败

* 网络错误
	* 由于突发的网络问题，认证失败，错误代码为**SOCKET_ERROR(110)**和**SOCKET_RESPONSE_TIMEOUT(101)**，则重新调用**Gamebase.login(activity, idpType, callback)**或稍后再试。
* 与其他帐户关联时发生的错误
	* 错误代码为**AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER(3302)**， 则表示尝试映射（Mapping）的IdP上的帐户已关联了另一个帐户，要解除已关联的帐户，请使用该帐户登录并通过调用**Gamebase.withdraw()**退出（删除数据），或者通过调用**Gamebase.removeMapping()**解除关联，再尝试映射（Mapping）。
* 关联相同IdP帐户发生的错误
	* 错误代码为**AUTH_ADD_MAPPING_ALREADY_HAS_SAME_IDP(3303)**，则表示已关联了与IDP相同类型的账户。
        * Gamebase映射（Mapping），每个IdP只能关联一个游戏帐户。例如，已经关联了PAYCO帐户，则无法添加其他PAYCO帐户。
        * 要关联同一IdP中的另一个帐户，请调用**Gamebase.removeMapping()**，解除关联后，再尝试映射（Mapping）。
* 其他错误
    * 尝试映射（Mapping）失败。

### 添加映射（Mapping）

在登录特定IdP状态下，尝试用其他IdP Mapping。<br/>

以下是尝试映射（Mapping）到Facebook的示例。

**API**

```java
+ (void)Gamebase.addMapping(Activity activity, AuthProvider authProvider, null, GamebaseDataCallback<AuthToken> callback);
```

**示例**

```java
private static void addMappingForFacebook(final Activity activity) {
    Gamebase.addMapping(activity, AuthProvider.FACEBOOK, null, new GamebaseDataCallback<AuthToken>() {
        @Override
        public void onCallback(AuthToken result, GamebaseException exception) {
            if (Gamebase.isSuccess(exception)) {
                // 成功映射（Mapping）
                Log.d(TAG, "Add Mapping successful");
                String userId = Gamebase.getUserID();
            } else {
                if (exception.getCode() == GamebaseError.SOCKET_ERROR ||
                        exception.getCode() == GamebaseError.SOCKET_RESPONSE_TIMEOUT) {
                    // Socket error 表示网络暂时不可用。
                    // 请确认网络状态或稍后再试 。
                    new Thread(new Runnable() {
                        @Override
                        public void run() {
                            try {
                                Thread.sleep(2000);
                                addMappingForFacebook(activity);
                            } catch (InterruptedException e) {}
                        }
                    }).start();
                } else if (exception.getCode() == GamebaseError.AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER) {
                    // 您尝试映射（Mapping）的IDP帐户已经关联到其他帐户
                    // 为了强制解除关联，您需要退出（删除数据）或解除映射（Mapping）。
                    Log.e(TAG, "Add Mapping failed- ALREADY_MAPPED_TO_OTHER_MEMBER");
                } else if (exception.getCode() == GamebaseError.AUTH_ADD_MAPPING_ALREADY_HAS_SAME_IDP) {
                    // 您尝试映射（Mapping）的IDP帐户已存在。
                    // Gamebase映射（Mapping），每个IDP帐户只能关联一个游戏帐户。
                    // 如果要更改IDP帐户，则必须解除已关联的帐户。
                    Log.e(TAG, "Add Mapping failed- ALREADY_HAS_SAME_IDP");
                } else {
                    // 添加映射（Mapping）失败
                    Log.e(TAG, "Add Mapping failed- "
                            + "errorCode: " + exception.getCode()
                            + "errorMessage: " + exception.getMessage());
                }
            }
        }
    });
}
```

### 使用Credential AddMapping

游戏中直接使用ID Provider提供的SDK进行认证后，使用发行的访问令牌，进行GameBase AddMapping的接口。

* Credential参数设定方法

| keyname                                  | a use                                    | 值类型                                     |
| ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| AuthProviderCredentialConstants.PROVIDER_NAME | 设定IdP类型                                | AuthProvider.GOOGLE<br> AuthProvider.FACEBOOK<br>AuthProvider.PAYCO<br>AuthProvider.NAVER |
| AuthProviderCredentialConstants.ACCESS_TOKEN | 设置登录IdP后收到的认证信息（访问令牌）<br/>不用于Google认证|                                          |
| AuthProviderCredentialConstants.AUTHORIZATION_CODE |输入登录Google后可以获取的OTOC(一次性验证码)|                                          |

> [参考]
>
> 当要在游戏中使用外部服务（例如Facebook）的特有功能时，可能需要它。
>

<br/>

> <font color="red">[注意]</font><br/>
>
> 外部SDK要求的开发事项需使用外部SDK的API实现，Gamebase不支持。
>

**API**

```java
+ (void)Gamebase.addMapping(Activity activity, Map<String, Object> credentialInfo, null, GamebaseDataCallback<AuthToken> callback);
```

**示例**

```java
private static void addMappingWithCredential(final Activity activity) {
    Map<String, Object> credentialInfo = new HashMap<>();
    credentialInfo.put(AuthProviderCredentialConstants.PROVIDER_NAME, AuthProvider.FACEBOOK);
    credentialInfo.put(AuthProviderCredentialConstants.ACCESS_TOKEN, facebookAccessToken);

    Gamebase.addMapping(activity, credentialInfo, new GamebaseDataCallback<AuthToken>() {
        @Override
        public void onCallback(AuthToken data, GamebaseException exception) {
            if (Gamebase.isSuccess(exception)) {
                // 成功添加Mapping
                Log.d(TAG, "Add Mapping successful");
                String userId = Gamebase.getUserID();
            } else {
                if (exception.getCode() == GamebaseError.SOCKET_ERROR ||
                        exception.getCode() == GamebaseError.SOCKET_RESPONSE_TIMEOUT) {
                    // Socket error 表示网络暂时不可用 。
                    // 请确认网络状态或稍后再试。
                    new Thread(new Runnable() {
                        @Override
                        public void run() {
                            try {
                                Thread.sleep(2000);
                                addMappingWithCredential(activity);
                            } catch (InterruptedException e) {}
                        }
                    }).start();
                } else if (exception.getCode() == GamebaseError.AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER) {
                    //您尝试映射（Mapping）的IDP帐户已经关联到其他帐户。
                    // 为了强制解除关联，您需要退出（删除数据）或解除映射（Mapping）。
                    Log.e(TAG, "Add Mapping failed- ALREADY_MAPPED_TO_OTHER_MEMBER");
                } else if (exception.getCode() == GamebaseError.AUTH_ADD_MAPPING_ALREADY_HAS_SAME_IDP) {
                    // 您尝试映射（Mapping）的IDP帐户已存在。
                    // Gamebase映射（Mapping），每个IDP帐户只能关联一个游戏帐户。
                    // 如果要更改IDP帐户，则必须解除已关联的帐户。
                    Log.e(TAG, "Add Mapping failed- ALREADY_HAS_SAME_IDP");
                } else {
                    // 添加映射（Mapping）失败
                    Log.e(TAG, "Add Mapping failed- "
                            + "errorCode: " + exception.getCode()
                            + "errorMessage: " + exception.getMessage());
                }
            }
        }
    });
}
```

### 解除映射（Mapping）

解除特定IDP的关联。如果尝试解除当前登录中的帐户，则会失败。<br/>
解除关联之后，Gamebase将该IdP退出登录。

**API**

```java
+ (void)Gamebase.removeMapping(Activity activity, AuthProvider authProvider, null, GamebaseDataCallback<AuthToken> callback);
```

**示例**

```java
private static void removeMappingForFacebook(final Activity activity) {
    Gamebase.removeMapping(activity, AuthProvider.FACEBOOK, new GamebaseCallback() {
        @Override
        public void onCallback(GamebaseException exception) {
            if (Gamebase.isSuccess(exception)) {
                // 成功解除映射（Mapping）
                Log.d(TAG, "Remove mapping successful");
            } else {
                if (exception.getCode() == GamebaseError.SOCKET_ERROR ||
                        exception.getCode() == GamebaseError.SOCKET_RESPONSE_TIMEOUT) {
                    // Socket error 表示网络暂时不可用。
                    // 请确认网络状态或稍后再试。
                    new Thread(new Runnable() {
                        @Override
                        public void run() {
                            try {
                                Thread.sleep(2000);
                                removeMappingForFacebook(activity);
                            } catch (InterruptedException e) {}
                        }
                    }).start();
                } else if (exception.getCode() == GamebaseError.AUTH_REMOVE_MAPPING_LOGGED_IN_IDP) {
                    //无法解除当前登录中的帐号映射（Mapping）。
                    // 您必须使用其他帐户登录或退出（删除数据），才能解除映射（Mapping）。
                    Log.e(TAG, "Remove Mapping failed- LOGGED_IN_IDP");
                } else {
                    //解除映射（Mapping）失败
                    Log.e(TAG, "Remove mapping failed- "
                            + "errorCode: " + exception.getCode()
                            + "errorMessage: " + exception.getMessage());
                }
            }
        }
    });
}
```


## Gamebase用户的信息
在使用Gamebase完成认证过程后，制作App时可获取到所需的信息。

> <font color="red">[注意]</font><br/>
>
> 如果您使用"Gamebase.loginForLastLoggedInProvider()" API登录，则无法获取认证信息。
>
> 如果需要认证信息，代替“Gamebase.loginForLastLoggedInProvider”使用IDPCode和同一个{IDP_CODE}作为参数，来使用“Gamebase.login(activity, IDP_CODE, callback)” API登录，才可以获取到正常的认证信息。

### 获取Gamebase的认证信息
可以获取Gamebase发行的认证信息。

**API**

```java
+ (String)Gamebase.getUserID();
+ (String)Gamebase.getAccessToken();
+ (String)Gamebase.getLastLoggedInProvider();
+ (BanInfo)Gamebase.getBanInfo();
```

**示例**

```java

// Obtaining Gamebase UserID
String userId = Gamebase.getUserID();

// Obtaining Gamebase AccessToken
String accessToken = Gamebase.getAccessToken();

// Obtaining Last Logged In Provider
String lastLoggedInProvider = Gamebase.getLastLoggedInProvider();

// Obtaining Ban Information
BanInfo banInfo = Gamebase.getBanInfo();
```


### 为外部IDP获取认证信息

从外部认证SDK，可获取访问令牌、用户ID、Profile等信息。

**API**

```java
+ (String)Gamebase.getAuthProviderUserID(AuthProvider authProvider);
+ (String)Gamebase.getAuthProviderAccessToken(AuthProvider authProvider);
+ (AuthProviderProfile)Gamebase.getAuthProviderProfile(AuthProvider authProvider);
```

**示例**

```java
// 获取用户ID。
String userId = Gamebase.getAuthProviderUserID(AuthProvider.FACEBOOK);

// 获取访问令牌。
String accessToken = Gamebase.getAuthProviderAccessToken(AuthProvider.FACEBOOK);

// 获取用户Profile信息。
AuthProviderProfile profile = Gamebase.getAuthProviderProfile(AuthProvider.FACEBOOK);
Map<String, Object> profileMap = profile.information;
```

### 获取被禁用的用户信息

如果在Gamebase Console中登记为受到制裁的游戏用户，当该用户尝试登录时，
可能会看到以下限制信息代码。您可以使用**Gamebase.getBanInfo()**方法，确认制裁信息。

* BANNED_MEMBER(7)

## TransferKey
获取密钥以将访客帐户转移到其他设备的功能。
此密匙称为**TransferKey**。
获取的TransferKey可以通过从另一台设备调用**requestTransfer**API来转移帐户。

> `注意`
> 只有在游客登录方式时，才能发放TransferKey。
> 只有游客登录或未登录的状态，才能使用TransferKey转移帐户。
> 如果您登录的游客帐户已映射（Mapping）到其他外部IdP（Google，Facebook，Payco等），则不支持转移帐户。

### Issue TransferKey 
发放TransferKey以转移游客帐户。
TransferKey的格式为**包含“小写/大写/数字”的8位数的字符串**。
同时，发行发放时间和到期时间，格式为epoch time。
* 参考：https://www.epochconverter.com/

**API**

```java
+ (void)Gamebase.issueTransferKey(int expiredTime, GamebaseDataCallback<TransferKeyInfo> callback);
```

**示例**

```java
Gamebase.issueTransferKey(3600 * 24, new GamebaseDataCallback<TransferKeyInfo>() {
    @Override
    public void onCallback(TransferKeyInfo transferKeyData, GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            Log.d(TAG, "Issue TransferKey successful");
            Log.i(TAG, "transferKey : " + transferKeyData.getTransferKey());
            Log.i(TAG, "regDate : " + transferKeyData.getRegDate());
            Log.i(TAG, "expireDate : " + transferKeyData.getExpireDate());
            ...
        } else {
            Log.e(TAG, "Issue TransferKey failed");
            ...
        }
    }
});
```

### 将游客账户转移到另一台设备
通过**issueTransfer** API发放的TransferKey转移帐户的功能。
在成功转移帐户后，在TransferKey的原设备上显示转移完成的消息，并且用在游客登录时将创建新帐户。
在成功转移帐户的新设备上，您可以继续使用原游客帐户。

> `注意`
> 如果在游客登录状态下转移成功，原设备中登录过的游客信息将丢失。

**API**

```java
+ (void)Gamebase.requestTransfer(String transferKey, GamebaseDataCallback<AuthToken> callback);
```

**示例**

```java
Gamebase.requestTransfer(transferKey, new GamebaseDataCallback<AuthToken>() {
    @Override
    public void onCallback(AuthToken data, GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            Log.d(TAG, "Transfer account successful");
            ...
        } else {
            Log.e(TAG, "Transfer account failed");
            ...
        }
    }
});
```

## Error处理

| Category       | Error                                    | Error Code | Description                              |
| -------------- | ---------------------------------------- | ---------- | ---------------------------------------- |
| Auth           | INVALID\_MEMBER                          | 6          | 无效的用户请求。                   |
|                | BANNED\_MEMBER                           | 7          | 被制裁的用户。                               |
|                | AUTH\_USER\_CANCELED                     | 3001       | 登录信息已被取消 。                           |
|                | AUTH\_NOT\_SUPPORTED\_PROVIDER           | 3002       | 不支持的认证方式 。                  |
|                | AUTH\_NOT\_EXIST\_MEMBER                 | 3003       | 不存在或已退出（删除数据）的用户。               |
|                | AUTH\_EXTERNAL\_LIBRARY\_ERROR           | 3009       | 外部认证库错误。<br/>请确认DetailCode和DetailMessage。  |
|                | AUTH_ALREADY_IN_PROGRESS_ERROR           | 3010       | 之前的认证过程未完成。 |
| TransferKey    | SAME\_REQUESTOR                          | 8          | 在同一台设备上使用了相同的TransferKey。 |
|                | NOT\_GUEST\_OR\_HAS\_OTHERS              | 9          | 非游客帐户尝试了转移或帐户已关联了游客以外的IDP。|
|                | AUTH\_TRANSFERKEY\_EXPIRED               | 3031       | TransferKey已过期。 |
|                | AUTH\_TRANSFERKEY\_CONSUMED              | 3032       | TransferKey已使用。 |
|                | AUTH\_TRANSFERKEY\_NOT\_EXIST            | 3033       | 无效的TransferKey。 |
| Auth (Login)   | AUTH\_TOKEN\_LOGIN\_FAILED               | 3101       | 令牌登录失败。                          |
|                | AUTH\_TOKEN\_LOGIN\_INVALID\_TOKEN\_INFO | 3102       | 无效的令牌信息 。                       |
|                | AUTH\_TOKEN\_LOGIN\_INVALID\_LAST\_LOGGED\_IN\_IDP | 3103       | 无近期登录的IdP信息。                   |
| IDP Login      | AUTH\_IDP\_LOGIN\_FAILED                 | 3201       | IdP登录失败。                         |
|                | AUTH\_IDP\_LOGIN\_INVALID\_IDP\_INFO     | 3202       | 无效的IdP信息。（Console中没有此IdP信息）。 |
| Add Mapping    | AUTH\_ADD\_MAPPING\_FAILED               | 3301       | 添加映射（Mapping）失败。                           |
|                | AUTH\_ADD\_MAPPING\_ALREADY\_MAPPED\_TO\_OTHER\_MEMBER | 3302       | 已经与其他帐户映射（Mapping）。                    |
|                | AUTH\_ADD\_MAPPING\_ALREADY\_HAS\_SAME\_IDP | 3303       |  已映射（Mapping）到相同的IdP。                   |
|                | AUTH\_ADD\_MAPPING\_INVALID\_IDP\_INFO   | 3304       | 无效的IdP信息。（Console中没有此IdP信息）。 |
|                | AUTH_ADD_MAPPING_CANNOT_ADD_GUEST_IDP    | 3305       | 游客IDP无法进行AddMapping。 |
| Remove Mapping | AUTH\_REMOVE\_MAPPING\_FAILED            | 3401       | 解除映射（Mapping）失败。                           |
|                | AUTH\_REMOVE\_MAPPING\_LAST\_MAPPED\_IDP | 3402       | 无法解除最后映射（Mapping）的IdP。                |
|                | AUTH\_REMOVE\_MAPPING\_LOGGED\_IN\_IDP   | 3403       | 当前登录的IdP。                      |
| Logout         | AUTH\_LOGOUT\_FAILED                     | 3501       | 退出登录失败。                           |
| Withdrawal     | AUTH\_WITHDRAW\_FAILED                   | 3601       | 退出（删除数据）失败。                            |
| Not Playable   | AUTH\_NOT\_PLAYABLE                      | 3701       | 无法玩游戏的状态(维护或已下线等)。         |
| Auth(Unknown)  | AUTH\_UNKNOWN\_ERROR                     | 3999       | 未知错误(未定义的错误)。             |

* 所有错误代码，请参考以下文档。
    * [Entire Error Codes](./error-code/#client-sdk)

**AUTH_EXTERNAL_LIBRARY_ERROR**

* 该错误为各IdP的SDK中发生的错误。
* 确认错误代码方式如下。

```java
Gamebase.login(activity, AuthProvider.GOOGLE, additionalInfo, new GamebaseDataCallback<AuthToken>() {
    @Override
    public void onCallback(AuthToken data, GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            Log.d(TAG, "Login successful");
            ...
        } else {
            Log.e(TAG, "Login failed");

            // Gamebase Error Info
            int errorCode = exception.getCode();
            String errorMessage = exception.getMessage();

            if (errorCode == GamebaseError.AUTH_EXTERNAL_LIBRARY_ERROR) {
                // Third Party Detail Error Info
                int moduleErrorCode = exception.getDetailCode();
                String moduleErrorMessage = exception.getDetailMessage();
                
                ...
            }
        }
    }
});
```

* IdP SDK的错误代码，请参考相应Developer页面。
