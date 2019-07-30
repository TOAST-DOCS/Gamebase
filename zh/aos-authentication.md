## Game > Gamebase > Android SDK使用指南 > 认证

## Login

Gamebase默认支持Guest登录。

* 使用游客以外的Provider登录，需要Provider AuthAdapter。
* AuthAdapter和第三方提供的SDK设置，请参考以下内容。
  [第三方提供的SDK指南](aos-started#3rd-party-provider-sdk-guide)


### Login Flow

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


### Login with IdP

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

### Login with Credential

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

### Authentication Additional Information Settings

[控制台使用指南](./oper-app/#authentication-information)

## Logout

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

## Withdraw

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

## Mapping

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

### Add Mapping Flow

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

### Add Mapping

在登录特定IdP状态下，尝试用其他IdP Mapping。<br/>

以下是尝试映射（Mapping）到Facebook的示例。

**API**

```java
+ (void)Gamebase.addMapping(Activity activity, String providerName, null, GamebaseDataCallback<AuthToken> callback);
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
                return;
            }

            // 添加映射（Mapping）失败
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
                // 강제로 연동을 해제하기 위해서는 해당 계정의 탈퇴나 Mapping 해제를 하거나, 다음과 같이
                // ForcingMappingTicket을 획득 후, addMappingForcibly() 메소드를 이용하여 강제 매핑을 시도합니다.
                Log.e(TAG, "Add Mapping failed- ALREADY_MAPPED_TO_OTHER_MEMBER");
                final ForcingMappingTicket ticket = ForcingMappingTicket.from(exception);
                final String forcingMappingKey = ticket.forcingMappingKey;

                Gamebase.addMappingForcibly(activity, credentialInfo, forcingMappingKey, new GamebaseDataCallback<AuthToken>() {
                    @Override
                    public void onCallback(AuthToken data, GamebaseException exception) {
                        ...
                        // 자세한 내용은 addMappingForcibly API 문서를 참고하세요.    
                    }
                }
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
    });
}
```

### Add Mapping with Credential

游戏中直接使用IdP提供的SDK进行认证后，使用发行的访问令牌，进行GameBase AddMapping的接口。

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
                return;
            }

            // 添加映射（Mapping）失败
            if (exception.getCode() == GamebaseError.SOCKET_ERROR ||
                    exception.getCode() == GamebaseError.SOCKET_RESPONSE_TIMEOUT) {
                // Socket error 表示网络暂时不可用。
                // 请确认网络状态或稍后再试 。
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
                // 您尝试映射（Mapping）的IDP帐户已经关联到其他帐户
                // 강제로 연동을 해제하기 위해서는 해당 계정의 탈퇴나 Mapping 해제를 하거나, 다음과 같이
                // ForcingMappingTicket을 획득 후, addMappingForcibly() 메소드를 이용하여 강제 매핑을 시도합니다.
                Log.e(TAG, "Add Mapping failed- ALREADY_MAPPED_TO_OTHER_MEMBER");
                final ForcingMappingTicket ticket = ForcingMappingTicket.from(exception);
                final String forcingMappingKey = ticket.forcingMappingKey;

                Gamebase.addMappingForcibly(activity, credentialInfo, forcingMappingKey, new GamebaseDataCallback<AuthToken>() {
                    @Override
                    public void onCallback(AuthToken data, GamebaseException exception) {
                        ...
                        // 자세한 내용은 addMappingForcibly API 문서를 참고하세요.    
                    }
                }
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
    });
}
```

### Add Mapping Forcibly
특정 IdP에 이미 매핑되어있는 계정이 있을 때, **강제로** 매핑을 시도합니다.
**강제 매핑**을 시도할 때는 AddMapping API에서 획득한 `ForcingMappingTicket`이 필요합니다.

다음은 Facebook에 강제 매핑을 시도하는 예시입니다.

**API**

```java
+ (void)Gamebase.addMappingForcibly(Activity activity, String providerName, String forcingMappingKey, Map<String, Object> additionalInfo, GamebaseDataCallback<AuthToken> callback);
```

**Example**

```java
private static void addMappingForciblyFacebook(final Activity activity) {
    Gamebase.addMapping(activity, AuthProvider.FACEBOOK, null, new GamebaseDataCallback<AuthToken>() {
        @Override
        public void onCallback(AuthToken result, GamebaseException exception) {
            if (Gamebase.isSuccess(exception)) {
                // 成功添加Mapping
                Log.d(TAG, "Add Mapping successful");
                String userId = Gamebase.getUserID();
                return;
            }

            // 우선 addMapping API 호출 및, 이미 연동되어있는 계정으로 매핑을 시도하여, 다음과 같이, ForcingMappingTicket을 얻을 수 있습니다.
            if (exception.getCode() == GamebaseError.AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER) {
                // ForcingMappingTicket 클래스의 from() 메소드를 이용하여 ForcingMappingTicket 인스턴스를 얻습니다.
                final ForcingMappingTicket ticket = ForcingMappingTicket.from(exception);
                final String forcingMappingKey = ticket.forcingMappingKey;

                // 강제 매핑을 시도합니다.
                Gamebase.addMappingForcibly(activity, AuthProvider.FACEBOOK, forcingMappingKey, null, new GamebaseDataCallback<AuthToken>() {
                    @Override
                    public void onCallback(AuthToken data, GamebaseException addMappingForciblyException) {
                        if (Gamebase.isSuccess(addMappingForciblyException)) {
                            // 강제 매핑 추가 성공
                            Log.d(TAG, "Add Mapping Forcibly successful");
                            String userId = Gamebase.getUserID();
                            return;
                        }           

                        // 강제 매핑 추가 실패
                        // 에러 코드를 확인하고 적절한 처리를 진행합니다.
                    }
                }
            } else {
                ...
            }
        }
    });
}
```


### Add Mapping Forcibly with Credential
특정 IdP에 이미 매핑되어있는 계정이 있을 때, **강제로** 매핑을 시도합니다.
**강제 매핑**을 시도할 때는 AddMapping API에서 획득한 `ForcingMappingTicket`이 필요합니다.

게임에서 직접 IdP에서 제공하는 SDK로 먼저 인증하고 발급받은 액세스 토큰 등을 이용하여, Gamebase AddMappingForcibly를 호출 할 수 있는 인터페이스입니다.

* Credential 파라미터 설정방법

| keyname                                  | a use                                    | 값 종류                                     |
| ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| AuthProviderCredentialConstants.PROVIDER_NAME | IdP 유형 설정                                | AuthProvider.GOOGLE<br> AuthProvider.FACEBOOK<br>AuthProvider.PAYCO<br>AuthProvider.NAVER |
| AuthProviderCredentialConstants.ACCESS_TOKEN | IdP 로그인 이후 받은 인증 정보(액세스 토큰)설정.<br/>Google 인증 시에는 사용 안 함. |                                          |
| AuthProviderCredentialConstants.AUTHORIZATION_CODE | Google 로그인 이후 획득할 수 있는 OTOC(one time authorization code) 입력 |                                          |

> [참고]
>
> 게임 내에서 외부 서비스(Facebook 등)의 고유 기능을 사용해야 할 때 필요할 수 있습니다.
>

<br/>

> <font color="red">[주의]</font><br/>
>
> 외부 SDK에서 지원 요구하는 개발사항은 외부 SDK의 API를 사용하여 구현해야 하며, Gamebase에서는 지원하지 않습니다.
>

다음은 Facebook에 강제 매핑을 시도하는 예시입니다.

**API**

```java
+ (void)Gamebase.addMappingForcibly(Activity activity, Map<String, Object> credentialInfo, String forcingMappingKey, GamebaseDataCallback<AuthToken> callback);
```

**Example**

```java
private static void addMappingForciblyFacebook(final Activity activity) {
    final Map<String, Object> credentialInfo = new HashMap<>();
    credentialInfo.put(AuthProviderCredentialConstants.PROVIDER_NAME, AuthProvider.FACEBOOK);
    credentialInfo.put(AuthProviderCredentialConstants.ACCESS_TOKEN, facebookAccessToken);

    Gamebase.addMapping(activity, credentialInfo, new GamebaseDataCallback<AuthToken>() {
        @Override
        public void onCallback(AuthToken result, GamebaseException exception) {
            if (Gamebase.isSuccess(exception)) {
                // 매핑 추가 성공
                Log.d(TAG, "Add Mapping successful");
                String userId = Gamebase.getUserID();
                return;
            }

            // 우선 addMapping API 호출 및, 이미 연동되어있는 계정으로 매핑을 시도하여, 다음과 같이, ForcingMappingTicket을 얻을 수 있습니다.
            if (exception.getCode() == GamebaseError.AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER) {
                // ForcingMappingTicket 클래스의 from() 메소드를 이용하여 ForcingMappingTicket 인스턴스를 얻습니다.
                final ForcingMappingTicket ticket = ForcingMappingTicket.from(exception);
                final String forcingMappingKey = ticket.forcingMappingKey;

                // 강제 매핑을 시도합니다.
                Gamebase.addMappingForcibly(activity, credentialInfo, forcingMappingKey, null, new GamebaseDataCallback<AuthToken>() {
                    @Override
                    public void onCallback(AuthToken data, GamebaseException addMappingForciblyException) {
                        if (Gamebase.isSuccess(addMappingForciblyException)) {
                            // 강제 매핑 추가 성공
                            Log.d(TAG, "Add Mapping Forcibly successful");
                            String userId = Gamebase.getUserID();
                            return;
                        }           

                        // 강제 매핑 추가 실패
                        // 에러 코드를 확인하고 적절한 처리를 진행합니다.
                    }
                }
            } else {
                ...
            }
        }
    });
}
```



### Remove Mapping

특정 IdP에 대한 연동을 해제합니다. 만약, 현재 로그인 중인 계정을 해제하려고 하면 실패를 반환합니다.<br/>
解除关联之后，Gamebase将该IdP退出登录。

**API**

```java
+ (void)Gamebase.removeMapping(Activity activity, String providerName, null, GamebaseDataCallback<AuthToken> callback);
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


## Gamebase User`s Information
在使用Gamebase完成认证过程后，制作App时可获取到所需的信息。

> <font color="red">[注意]</font><br/>
>
> 如果您使用"Gamebase.loginForLastLoggedInProvider()" API登录，则无法获取认证信息。
>
> 如果需要认证信息，代替“Gamebase.loginForLastLoggedInProvider”使用IDPCode和同一个{IDP_CODE}作为参数，来使用“Gamebase.login(activity, IDP_CODE, callback)” API登录，才可以获取到正常的认证信息。

### Get Authentication Information for Gamebase
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


### Get Authentication Information for External IdP

从外部认证SDK，可获取访问令牌、用户ID、Profile等信息。

**API**

```java
+ (String)Gamebase.getAuthProviderUserID(String providerName);
+ (String)Gamebase.getAuthProviderAccessToken(String providerName);
+ (AuthProviderProfile)Gamebase.getAuthProviderProfile(String providerName);
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

### Get Banned User Information

如果在Gamebase Console中登记为受到制裁的游戏用户，当该用户尝试登录时，
可能会看到以下限制信息代码。您可以使用**Gamebase.getBanInfo()**方法，确认制裁信息。

* BANNED_MEMBER(7)

## TransferAccount
게스트 계정을 다른 단말기로 이전하기 위해 계정 이전을 위한 키를 발급받는 기능입니다.

이 키를 **TransferAccountInfo** 라고 부릅니다.
발급받은 TransferAccountInfo는 다른 기기에서 **requestTransferAccount** API를 호출하여 계정 이전을 할 수 있습니다.

> `주의`
> TransferAccountInfo의 발급은 게스트 로그인 상태에서만 발급이 가능합니다.
> TransferAccountInfo를 이용한 계정 이전은 게스트 로그인 상태 또는 로그인되어 있지 않은 상태에서만 가능합니다.
> 로그인한 게스트 계정이 이미 다른 외부 IdP (Google, Facebook, Payco 등) 계정과 매핑이 되어 있다면 계정 이전이 지원되지 않습니다.

### Issue TransferAccount
게스트 계정 이전을 위한 TransferAccountInfo를 발급합니다.

**API**

```java
+ (void)Gamebase.issueTransferAccount(final GamebaseDataCallback<TransferAccountInfo> callback);
```

**示例**

```java
Gamebase.issueTransferAccount(new GamebaseDataCallback<TransferAccountInfo>() {
    @Override
    public void onCallback(final TransferAccountInfo transferAccount, final GamebaseException exception) {
        if (!Gamebase.isSuccess(exception)) {
            // Issuing TransferAccount failed.
            return;
        }

        // Issuing TransferAccount success.
        final String account = transferAccount.account.id;
        final String password = transferAccount.account.password;
    }
});
```

### Query TransferAccount
게스트 계정 이전을 위해 이미 발급받은 TransferAccountInfo 정보를 게임베이스 서버에 질의합니다.

**API**

```java
+ (void)Gamebase.queryTransferAccount(final GamebaseDataCallback<TransferAccountInfo> callback);
```

**Example**

```java
Gamebase.queryTransferAccount(new GamebaseDataCallback<TransferAccountInfo>() {
    @Override
    public void onCallback(final TransferAccountInfo transferAccount, final GamebaseException exception) {
        if (!Gamebase.isSuccess(exception)) {
            // Querying TransferAccount failed.
            return;
        }

        // Querying TransferAccount success.
        final String account = transferAccount.account.id;
        final String password = transferAccount.account.password;
    }
});
```


### Renew TransferAccount
이미 발급받은 TransferAccountInfo 정보를 갱신합니다.
"자동 갱신", "수동 갱신"의 방법이 있으며, "Password만 갱신", "ID와 Password 모두 갱신" 등의 설정을 통해
TransferAccountInfo 정보를 갱신 할 수 있습니다.

```java
+ (void)Gamebase.renewTransferAccount(final TransferAccountRenewConfiguration config, final GamebaseDataCallback<TransferAccountInfo> callback);
```

**Example**

```java
// If you want renew the account automatically, use this config.
final RenewalTargetType renewalTargetType = RenewalTargetType.ID_PASSWORD; // RenewalTargetType.PASSWORD
final TransferAccountRenewConfiguration autoConfig = TransferAccountRenewConfiguration.newAutoRenewConfiguration(renewalTargetType);

// If you want renew the account manually, use this config.
final TransferAccountRenewConfiguration manualConfig = TransferAccountRenewConfiguration.newManualRenewConfiguration("id", "password");
Gamebase.renewTransferAccount(autoConfig, new GamebaseDataCallback<TransferAccountInfo>() {
    @Override
    public void onCallback(final TransferAccountInfo transferAccountInfo, final GamebaseException exception) {
        if (!Gamebase.isSuccess(exception)) {
            // Renewing TransferAccount failed.
            return;
        }

        // Renewing TransferAccount success.
        final String renewedAccount = transferAccount.account.id;
        final String renewedPassword = transferAccount.account.password;
    }
});
```




### Transfer Guest Account to Another Device
**issueTransfer** API로 발급받은 TransferAccount를 통해 계정을 이전하는 기능입니다.
계정 이전 성공 시 TransferAccount를 발급받은 단말기에서 이전 완료 메시지가 표시될 수 있고, Guest 로그인 시 새로운 계정이 생성됩니다.
계정 이전이 성공한 단말기에서는 TransferAccount를 발급받았던 단말기의 게스트 계정을 계속해서 사용할 수 있습니다.

> `注意`
> 如果在游客登录状态下转移成功，原设备中登录过的游客信息将丢失。

**API**

```java
+ (void)Gamebase.transferAccountWithIdPLogin(String accountId, String accountPassword, GamebaseDataCallback<AuthToken> callback);
```

**示例**

```java
Gamebase.transferAccountWithIdPLogin(accountId, accountPassword, new GamebaseDataCallback<AuthToken>() {
    @Override
    public void onCallback(final AuthToken authToken, final GamebaseException exception) {
        if (!Gamebase.isSuccess(exception)) {
            // Transfering Account failed.
            return;
        }
        // Transfering Account success.
        // TODO: implements post login process
    }
});
```

## Error Handling

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
|                | AUTH_TRANSFERACCOUNT_EXPIRED             | 3041       | TransferAccount의 유효기간이 만료됐습니다. |
|                | AUTH_TRANSFERACCOUNT_BLOCK               | 3042       | 잘못된 TransferAccount를 여러번 입력하여 계정 이전 기능이 잠겼습니다. |
|                | AUTH_TRANSFERACCOUNT_INVALID_ID          | 3043       | TransferAccount의 Id가 유효하지 않습니다. |
|                | AUTH_TRANSFERACCOUNT_INVALID_PASSWORD    | 3044       | TransferAccount의 Password가 유효하지 않습니다. |
|                | AUTH_TRANSFERACCOUNT_CONSOLE_NO_CONDITION | 3045      | TransferAccount 설정이 되어있지 않습니다. <br/> TOAST Gamebase Console에서 먼저 설정해주세요. |
|                | AUTH_TRANSFERACCOUNT_NOT_EXIST           | 3046       | TransferAccount가 존재하지 않습니다. TransferAccount를 먼저 발급받아주세요. |
|                | AUTH_TRANSFERACCOUNT_ALREADY_EXIST_ID    | 3047       | TransferAccount가 이미 존재합니다. |
|                | AUTH_TRANSFERACCOUNT_ALREADY_USED        | 3048       | TransferAccount가 이미 사용되었습니다. |
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
| Add Mapping Forcibly | AUTH_ADD_MAPPING_FORCIBLY_NOT_EXIST_KEY         | 3311       | 강제매핑키(ForcingMappingKey)가 존재하지 않습니다. <br/>ForcingMappingTicket을 다시 한번 확인해주세요. |
|                      | AUTH_ADD_MAPPING_FORCIBLY_ALREADY_USED_KEY      | 3312       | 강제매핑키(ForcingMappingKey)가 이미 사용되었습니다. |
|                      | AUTH_ADD_MAPPING_FORCIBLY_EXPIRED_KEY           | 3313       | 강제매핑키(ForcingMappingKey)의 유효기간이 만료되었습니다. |
|                      | AUTH_ADD_MAPPING_FORCIBLY_DIFFERENT_IDP         | 3314       | 강제매핑키(ForcingMappingKey)가 다른 IdP에 사용되었습니다. <br/>발급받은 ForcingMappingKey는 같은 IdP에 강제 매핑을 시도 하는데 사용됩니다. |
|                      | AUTH_ADD_MAPPING_FORCIBLY_DIFFERENT_AUTHKEY     | 3315       | 강제매핑키(ForcingMappingKey)가 다른 계정에 사용되었습니다. <br/>발급받은 ForcingMappingKey는 같은 IdP 및 계정에 강제 매핑을 시도 하는데 사용됩니다. |
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
