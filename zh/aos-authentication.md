## Game > Gamebase > Android SDK使用指南 > 认证

## Login

Gamebase默认支持Guest登录。

* 使用游客以外的Provider登录，需要Provider AuthAdapter。
* AuthAdapter和第三方提供的SDK设置，请参考以下内容。
    * [Game > Gamebase > Android SDK 使用指南 > 开始 > Setting > Gradle](./aos-started/#gradle)
    * [Game > Gamebase > Android SDK 使用指南 > 开始 > Setting > Console > 第三方提供的SDK指南](./aos-started/#console)

### Login Flow

多数游戏在标题页上实现登录。
* 当App首次安装和启动时，游戏用户可以在标题页选择要进行的IdP(identity provider)类型。
* 登录过一次后，您将不会看到IdP选择画面，将使用之前登录的IdP类型进行认证。

上述逻辑可以按以下顺序实现。

![last provider login flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/login_for_last_logged_in_provider_flow_2.19.0.png)
![idp login flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/idp_login_flow_2.19.0.png)

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
    * 请使用**BanInfo.from(exception)**确认制裁信息，并告知游戏用户无法进行游戏的原因。
    * 初始化Gamebase 时调用**GamebaseConfiguration.Builder.enablePopup(true)** 和**enableBanPopup(true)**，Gamebase会自动弹出禁用的窗口。
* 其他错误
    * 因为使用上一次的登录类型认证失败，请进行**2.使用指定的IdP进行认证**。

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
    * 请使用**BanInfo.from(exception)**确认制裁信息，并告知游戏用户无法进行游戏的原因。
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
                // 如果要根据游戏UI实现禁用弹出窗口，请使用BanInfo.from(exception)
                // 确认制裁信息并告知游戏用户无法进行游戏的原因。
                BanInfo banInfo = BanInfo.from(exception);
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
                    //如果要根据游戏UI实现禁用弹出窗口，请使用BanInfo.from(exception)
                    // 确认制裁信息并告知游戏用户无法进行游戏的原因。
                    BanInfo banInfo = BanInfo.from(exception);
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

 {@line:180}
### Login with IdP

以下是允许您使用特定IdP登录的示例代码。<br/>
您可以在** AuthProvider **类中确认可以登录的IdP类型。

> <font color="red">[注意]</font><br/>
>
> 因PAYCO IdP（为认证模块）在检测中出现错误，常被误认为第三方结算模块，iOS审核总被拒，
> 不再提供AuthProvider.PAYCO的常数。
> 您要将"payco"字符串作为参数传送。 

**API**

```java
+ (void)Gamebase.login(Activity activity, AuthProvider provider, GamebaseDataCallback<AuthToken> callback);
+ (void)Gamebase.login(Activity activity, AuthProvider provider, Map<String, Object> additionalInfo, GamebaseDataCallback<AuthToken> callback);
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
                    //如果要根据游戏UI实现禁用弹出窗口，请使用BanInfo.from(exception)
                    // 确认制裁信息并告知游戏用户无法进行游戏的原因。
                    BanInfo banInfo = BanInfo.from(exception);
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
| AuthProviderCredentialConstants.PROVIDER_NAME | 设定IdP 类型                               | AuthProvider.GOOGLE<br> AuthProvider.FACEBOOK<br>AuthProvider.NAVER<br>AuthProvider.TWITTER<br>AuthProvider.LINE<br>AuthProvider.HANGAME<br>AuthProvider.APPLEID<br>AuthProvider.WEIBO<br>"payco" |
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
                    // 如果要根据游戏UI实现禁用弹出窗口，请使用BanInfo.from(exception)
                    // 确认制裁信息并告知游戏用户无法进行游戏的原因
                    BanInfo banInfo = BanInfo.from(exception);
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
    * AppleID ID: cc
    * Twitter ID: dd
* Gamebase 用户 ID : 456abcabc
    * Google ID: ee
    * Google ID: ff **-> 由于您已关联Google ee帐户，因此无法添加其他Google帐户。**

Mapping API中有添加映射和解除映射的功能。

> <font color="red">[注意]</font><br/>
>
> Guest登录中若成功Mapping，则Guest IdP消失。
>

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
+ (void)Gamebase.addMapping(Activity activity, String providerName, GamebaseDataCallback<AuthToken> callback);
+ (void)Gamebase.addMapping(Activity activity, String providerName, Map<String, Object> additionalInfo, GamebaseDataCallback<AuthToken> callback);
```

**示例**

```java
private static void addMappingForFacebook(final Activity activity) {
    String mappingProvider = AuthProvider.FACEBOOK;
    Gamebase.addMapping(activity, mappingProvider, new GamebaseDataCallback<AuthToken>() {
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
                // 若欲强制解除关联，注销该账户或解除映射(mapping)。或如下
                // 获得ForcingMappingTicket后利用addMappingForcibly()方法尝试强制映射。
                Log.e(TAG, "Add Mapping failed- ALREADY_MAPPED_TO_OTHER_MEMBER");
                final ForcingMappingTicket ticket = ForcingMappingTicket.from(exception);
                final String forcingMappingKey = ticket.forcingMappingKey;

                Gamebase.addMappingForcibly(activity, mappingProvider, forcingMappingKey, new GamebaseDataCallback<AuthToken>() {
                    @Override
                    public void onCallback(AuthToken data, GamebaseException exception) {
                        ...
                        // 详细内容请参考addMappingForcibly API文件。
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
| AuthProviderCredentialConstants.PROVIDER_NAME | 设定IdP类型                                | AuthProvider.GOOGLE<br> AuthProvider.FACEBOOK<br>AuthProvider.PAYCO<br>AuthProvider.NAVER<br>AuthProvider.TWITTER<br>AuthProvider.LINE |
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
                // 若欲强制解除关联，注销该账户或解除映射(mapping)。或如下
                // 获得ForcingMappingTicket后利用addMappingForcibly()方法尝试强制映射。
                Log.e(TAG, "Add Mapping failed- ALREADY_MAPPED_TO_OTHER_MEMBER");
                final ForcingMappingTicket ticket = ForcingMappingTicket.from(exception);
                final String forcingMappingKey = ticket.forcingMappingKey;

                Gamebase.addMappingForcibly(activity, credentialInfo, forcingMappingKey, new GamebaseDataCallback<AuthToken>() {
                    @Override
                    public void onCallback(AuthToken data, GamebaseException exception) {
                        ...
                        // 详细内容请参考addMappingForcibly API文件。
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
若特定IdP有已映射的账户，尝试**强制**映射。
尝试**强制映射**时需要从AddMapping API获得的`ForcingMappingTicket`。

如下为对Facebook尝试强制映射的范例。

**API**

```java
+ (void)Gamebase.addMappingForcibly(Activity activity, String providerName, String forcingMappingKey, GamebaseDataCallback<AuthToken> callback);
+ (void)Gamebase.addMappingForcibly(Activity activity, String providerName, String forcingMappingKey, Map<String, Object> additionalInfo, GamebaseDataCallback<AuthToken> callback);
```

**Example**

```java
private static void addMappingForciblyFacebook(final Activity activity) {
    String mappingProvider = AuthProvider.FACEBOOK;
    Gamebase.addMapping(activity, mappingProvider, new GamebaseDataCallback<AuthToken>() {
        @Override
        public void onCallback(AuthToken result, GamebaseException exception) {
            if (Gamebase.isSuccess(exception)) {
                // 成功添加Mapping
                Log.d(TAG, "Add Mapping successful");
                String userId = Gamebase.getUserID();
                return;
            }

            // 首先调用addMapping API，尝试以已关联的账户映射，如下可获得ForcingMappingTicket。
            if (exception.getCode() == GamebaseError.AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER) {
                // 利用ForcingMappingTicket类的from()方法获得ForcingMappingTicket实例。
                final ForcingMappingTicket ticket = ForcingMappingTicket.from(exception);
                final String forcingMappingKey = ticket.forcingMappingKey;

                // 尝试强制映射。
                Gamebase.addMappingForcibly(activity, mappingProvider, forcingMappingKey, null, new GamebaseDataCallback<AuthToken>() {
                    @Override
                    public void onCallback(AuthToken data, GamebaseException addMappingForciblyException) {
                        if (Gamebase.isSuccess(addMappingForciblyException)) {
                            // 添加强制映射成功
                            Log.d(TAG, "Add Mapping Forcibly successful");
                            String userId = Gamebase.getUserID();
                            return;
                        }

                        // 添加强制映射失败
                        // 确认错误代码并解决错误。
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
若特定IdP有已映射的账户，尝试**强制**映射。
尝试**强制映射**时需要从AddMapping API获得的`ForcingMappingTicket`。

游戏中先直接以IdP提供的SDK进行验证，并可利用发放的访问令牌等调用Gamebase AddMappingForcibly的接口。

* Credential参数设置方法

| keyname                                            | a use                                                        | 值类型                                                      |
| -------------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| AuthProviderCredentialConstants.PROVIDER_NAME      | 设置IdP类型                                                | AuthProvider.GOOGLE<br> AuthProvider.FACEBOOK<br>AuthProvider.NAVER<br>AuthProvider.TWITTER<br>AuthProvider.LINE<br>"payco" |
| AuthProviderCredentialConstants.ACCESS_TOKEN       | 设置登录IdP后获得的验证信息（访问令牌）<br/>在Google验证时不使用 |                                                              |
| AuthProviderCredentialConstants.AUTHORIZATION_CODE | 输入登录Google后可获得的OTOC(one time authorization code) |                                                              |

> [参考]
>
> 游戏中欲使用外部服务（Facebook等）的固有功能时可能需要。
>

<br/>

> <font color="red">[注意]</font><br/>
>
> 外部SDK要求支持的开发事项应使用外部SDK的API实现，Gamebase不支持。
>

如下为对Facebook尝试强制映射的范例。

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
                // 添加映射成功
                Log.d(TAG, "Add Mapping successful");
                String userId = Gamebase.getUserID();
                return;
            }

            // 首先调用addMapping API，尝试以已关联的账户映射，如下可获得ForcingMappingTicket。
            if (exception.getCode() == GamebaseError.AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER) {
                // 利用ForcingMappingTicket类的from()方法获得ForcingMappingTicket实例。
                final ForcingMappingTicket ticket = ForcingMappingTicket.from(exception);
                final String forcingMappingKey = ticket.forcingMappingKey;

                // 尝试强制映射。
                Gamebase.addMappingForcibly(activity, credentialInfo, forcingMappingKey, null, new GamebaseDataCallback<AuthToken>() {
                    @Override
                    public void onCallback(AuthToken data, GamebaseException addMappingForciblyException) {
                        if (Gamebase.isSuccess(addMappingForciblyException)) {
                            // 添加强制映射成功
                            Log.d(TAG, "Add Mapping Forcibly successful");
                            String userId = Gamebase.getUserID();
                            return;
                        }

                        // 添加强制映射失败
                        // 确认错误代码并解决错误。
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

解除特定IdP的关联。若欲解除当前登录的账户，将返回失败。<br/>
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

### Get Authentication Information for Gamebase
可以获取Gamebase发行的认证信息。

**API**

```java
+ (String)Gamebase.getUserID();
+ (String)Gamebase.getAccessToken();
+ (String)Gamebase.getLastLoggedInProvider();
```

**示例**

```java

// Obtaining Gamebase UserID
String userId = Gamebase.getUserID();

// Obtaining Gamebase AccessToken
String accessToken = Gamebase.getAccessToken();

// Obtaining Last Logged In Provider
String lastLoggedInProvider = Gamebase.getLastLoggedInProvider();
```

### Get Authentication Information for External IdP

从外部认证SDK，可获取访问令牌、用户ID、Profile等信息。

> <font color="red">[注意]</font><br/>
>
> * 如果您使用"Gamebase.loginForLastLoggedInProvider()" API登录，则无法获取认证信息。
>     * 如果需要认证信息，代替“Gamebase.loginForLastLoggedInProvider”使用IDPCode和同一个{IDP_CODE}作为参数，来使用“Gamebase.login(activity, IDP_CODE, callback)” API登录，才可以获取到正常的认证信息。

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
可能会看到以下限制信息代码。您可以使用**BanInfo.from(exception)**方法，确认制裁信息。

* BANNED_MEMBER(7)

## TransferAccount
获得将访客账户转移至其他终端机的密钥的功能。

该密钥称为**TransferAccountInfo**。
获得的TransferAccountInfo可从其他终端机调用**requestTransferAccount** API，并转移账户。

> `注意`
> TransferAccountInfo仅在访客登录状态下可获得。
> 使用TransferAccountInfo的账户转移仅可在访客登录状态或未登录状态下实现。
> 若登录的访客账户已与其他外部IdP（Google、Facebook、PAYCO等）映射，则不支持账户转移。

### Issue TransferAccount
为转移访客账户，发放TransferAccountInfo。

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
为转移访客账户，向Gamebase服务器查询已获得的TransferAccountInfo信息。

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
更新已获得的TransferAccountInfo信息。
更新方法有**自动更新**与**手动更新**，可选择**仅更新密码**、**同时更新ID与密码**更新TransferAccountInfo信息。

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
向从**issueTransfer** API获得的TransferAccount转移账户的功能。
账户转移成功时获得TransferAccount的终端机可显示转移完成信息，访客登录时创建新的账户。
在账户转移成功的终端机中可继续使用获得TransferAccount的终端机的账户信息。

> <font color="red">[注意]</font><br/>
>
> * 如果在游客登录状态下转移成功，原设备中登录过的游客信息将丢失。
> * 如果连续输入不正确的id/password，则出现**AUTH_TRANSFERACCOUNT_BLOCK(3042)**错误，暂时封禁账号转移。
> 在上述情况下，根据以下示例，可通过使用TransferAccountFailInfo值，提示用户账号被封，过多久才能再转移账号。

**API**

```java
+ (void)Gamebase.transferAccountWithIdPLogin(String accountId, String accountPassword, GamebaseDataCallback<AuthToken> callback);
```

**示例**

```java
Gamebase.transferAccountWithIdPLogin(accountId, accountPassword, new GamebaseDataCallback<AuthToken>() {
    @Override
    public void onCallback(AuthToken authToken, GamebaseException exception) {
        if (!Gamebase.isSuccess(exception)) {
            // Transfering Account failed.
            TransferAccountFailInfo failInfo = TransferAccountFailInfo.from(exception);
            if (failInfo != null) {
                // Transfering Account failed by entering the wrong id / pw multiple times.
                // You can tell when the account transfer is blocked by the TransferAccountFailInfo.
                String failedId = failInfo.id;
                int failCount = failInfo.failCount;
                Date blockedDate = new Date(failInfo.blockEndDate);
                return;
            }

            // Transfering Account failed by another reason.
            return;
        }

        // Transfering Account success.
        // TODO: implements post login process
    }
});
```

### Withdraw Immediately

无论预约退出时间的设置状态如何，立即退出。 
实际内置运行与Gamebase.withdraw() API相同。

立即退出后无法取消，因此需要提示用户是否继续执行。

## Error Handling

| Category       | Error                                    | Error Code | Description                              |
| -------------- | ---------------------------------------- | ---------- | ---------------------------------------- |
|                | AUTH\_EXTERNAL\_LIBRARY\_INITIALIZATION\_ERROR | 3006 | 第三方认证库初始化失败 |
|                | AUTH\_WITHDRAW\_ALREADY\_TEMPORARY\_WITHDRAW | 3602   | 用户已临时退出                    |
|                | AUTH\_WITHDRAW\_NOT\_TEMPORARY\_WITHDRAW | 3603       | 用户未临时退出                     |
{@line:end}

## TemporaryWithdrawal

为”预约退出”功能。
由于请求了临时退出，不立即退出，预约时期过后退出。 
可以在控制台中修改预约时间。

> ”注意”
>
> 使用预约退出功能时，不应调用**Gamebase.withdraw()** API。
> 通过调用**Gamebase.withdraw()** API，可立即退出。

登录成功后可通过调用AuthToken.getTemporaryWithdrawalInfo() API判断是否是预约退出的用户。 

### Request TemporaryWithdrawal

请求预约退出。
在控制台中指定的预约时间过后，自动完成退出处理。

**API**

```java
+ (void)Gamebase.TemporaryWithdrawal.requestWithdrawal(@NonNull Activity activity,
                                                       @Nullable GamebaseDataCallback<TemporaryWithdrawalInfo> callback);
```

**ErrorCode**

|Error Code | Description |
| --- | --- |
| AUTH\_WITHDRAW\_ALREADY\_TEMPORARY\_WITHDRAW(3602) | 用户已临时退出 |

**Example**

```java
public static void testRequestWithdraw() {
    Gamebase.TemporaryWithdrawal.requestWithdrawal(new GamebaseCallback() {
        @Override
        public void onCallback(TemporaryWithdrawalInfo data GamebaseException exception) {
            if (!Gamebase.isSuccess(exception)) {
                if (exception.getCode() == GamebaseError.AUTH_WITHDRAW_ALREADY_TEMPORARY_WITHDRAW) {
                    // Already requested temporary withdrawal before.
                } else {
                    // Request temporary withdrawal failed.
                    return;
                }
            }

            // Request temporary withdrawal success.
        }
    });
}
```
### Check TemporaryWithdrawal User

如果用户登录使用预约退出功能的游戏，您需要调用**TCGBAuthToken.tcgbMember.temporaryWithdrawal**API，若返还的结果不为null，为有效的TemporaryWithdrawalInfo对象时，则需通知用户正在进行退出。

**Example**

```java
public static void testLogin() {
    Gamebase.login(activity, provider, new GamebaseDataCallback<AuthToken>() {
        @Override
        public void onCallback(AuthToken data, GamebaseException exception) {
            if (!Gamebase.isSuccess(exception)) {
                // Login failed
                return;
            }

            // Check if user is requesting withdrawal
            if (data.getTemporaryWithdrawalInfo() != null) {
                // User is under temporary withdrawal
                long gracePeriodDate = data.getTemporaryWithdrawalInfo().getGracePeriodDate();
            } else {
                // Login success.
            }
        }
    });
}
```

### Cancel TemporaryWithdrawal

取消退出请求。
若预约退出时间到期，则无法退出。

**API**

```java
+ (void)Gamebase.TemporaryWithdrawal.cancelWithdrawal(@NonNull Activity activity,
                                                      @Nullable GamebaseCallback callback);
```

**ErrorCode**

|Error Code | Description |
| --- | --- |
| AUTH\_WITHDRAW\_NOT\_TEMPORARY\_WITHDRAW(3603) | 用户未临时退出 |

**Example**

```java
public static void testCancelWithdraw() {
    Gamebase.TemporaryWithdrawal.cancelWithdrawal(new GamebaseCallback() {
        @Override
        public void onCallback(final GamebaseException exception) {
            if (!Gamebase.isSuccess(exception)) {
                if (exception.getCode() == GamebaseError.AUTH_WITHDRAW_NOT_TEMPORARY_WITHDRAW) {
                    // Never requested temporary withdrawal before.
                } else {
                    // Cancel temporary withdrawal failed.
                    return;
                }
            }

            // Cancel temporary withdrawal success.
        }
    });
}
```

### Withdraw Immediately

无论预约退出时期的设置状态如何，都立即退出。 
实际内置运行与Gamebase.withdraw() API相同。

立即退出后无法取消，因此需要提示用户是否继续执行。

**API**

```java
+ (void)Gamebase.TemporaryWithdrawal.withdrawImmediately(@NonNull Activity activity,
                                                         @Nullable GamebaseCallback callback);
```

**Example**

```java
public static void testWithdrawImmediately() {
    Gamebase.TemporaryWithdrawal.withdrawImmediately(activity, new GamebaseCallback() {
        @Override
        public void onCallback(final GamebaseException exception) {
            if (!Gamebase.isSuccess(exception)) {
                // Withdraw failed.
                return;
            }

            // Withdraw success.
        }
    });
}
```

## Error Handling

| Category       | Error                                    | Error Code | Description                              |
| -------------- | ---------------------------------------- | ---------- | ---------------------------------------- |
| Auth           | INVALID\_MEMBER                          | 6          | 无效的用户请求                   |
|                | BANNED\_MEMBER                           | 7          | 被制裁的用户                               |
|                | AUTH\_USER\_CANCELED                     | 3001       | 登录信息已被取消 。                           |
|                | AUTH\_NOT\_SUPPORTED\_PROVIDER           | 3002       | 是不支持的认证方式 。                  |
|                | AUTH\_NOT\_EXIST\_MEMBER                 | 3003       | 是不存在或已退出（删除数据）的用户。               |
|                | AUTH\_EXTERNAL\_LIBRARY\_INITIALIZATION\_ERROR | 3006 | 第三方认证库初始化失败 |
|                | AUTH\_EXTERNAL\_LIBRARY\_ERROR           | 3009       | 外部认证库错误<br/>请确认DetailCode和DetailMessage。  |
|                | AUTH_ALREADY_IN_PROGRESS_ERROR           | 3010       | 未完成之前的认证过程。 |
| TransferKey    | SAME\_REQUESTOR                          | 8          | 在同一台设备上使用了相同的TransferKey。 |
|                | NOT\_GUEST\_OR\_HAS\_OTHERS              | 9          | 非游客帐户尝试了转移或帐户已关联了游客以外的IDP。|
|                | AUTH_TRANSFERACCOUNT_EXPIRED             | 3041       | TransferAccount的有效期已结束。|
|                | AUTH_TRANSFERACCOUNT_BLOCK               | 3042       | 多次输入错误TransferAccount，账户转移功能锁定。|
|                | AUTH_TRANSFERACCOUNT_INVALID_ID          | 3043       | TransferAccount的ID无效。|
|                | AUTH_TRANSFERACCOUNT_INVALID_PASSWORD    | 3044       | TransferAccount的密码无效。|
|                | AUTH_TRANSFERACCOUNT_CONSOLE_NO_CONDITION | 3045      | 未设置TransferAccount。<br/> 请先在TOAST Gamebase控制台中设置。|
|                | AUTH_TRANSFERACCOUNT_NOT_EXIST           | 3046       | 无TransferAccount。请先获得TransferAccount。|
|                | AUTH_TRANSFERACCOUNT_ALREADY_EXIST_ID    | 3047       | 已有TransferAccount。|
|                | AUTH_TRANSFERACCOUNT_ALREADY_USED        | 3048       | TransferAccount已使用。|
| Auth (Login)   | AUTH\_TOKEN\_LOGIN\_FAILED               | 3101       | 令牌登录失败                          |
|                | AUTH\_TOKEN\_LOGIN\_INVALID\_TOKEN\_INFO | 3102       | 无效的令牌信息                      |
|                | AUTH\_TOKEN\_LOGIN\_INVALID\_LAST\_LOGGED\_IN\_IDP | 3103       | 无近期登录的IdP信息                   |
| IdP Login      | AUTH\_IDP\_LOGIN\_FAILED                 | 3201       | IdP登录失败                         |
|                | AUTH\_IDP\_LOGIN\_INVALID\_IDP\_INFO     | 3202       | 无效的IdP信息（Console中没有此IdP信息） |
| Add Mapping    | AUTH\_ADD\_MAPPING\_FAILED               | 3301       | 添加映射（Mapping）失败                           |
|                | AUTH\_ADD\_MAPPING\_ALREADY\_MAPPED\_TO\_OTHER\_MEMBER | 3302       | 已经与其他帐户映射（Mapping）。                    |
|                | AUTH\_ADD\_MAPPING\_ALREADY\_HAS\_SAME\_IDP | 3303       |  已映射（Mapping）到相同的IdP。                   |
|                | AUTH\_ADD\_MAPPING\_INVALID\_IDP\_INFO   | 3304       | 无效的IdP信息（Console中没有此IdP信息） |
|                | AUTH_ADD_MAPPING_CANNOT_ADD_GUEST_IDP    | 3305       | 游客IDP无法进行AddMapping。 |
| Add Mapping Forcibly | AUTH_ADD_MAPPING_FORCIBLY_NOT_EXIST_KEY         | 3311       | 无强制映射密钥(ForcingMappingKey)<br/>请再次确认ForcingMappingTicket。|
|                      | AUTH_ADD_MAPPING_FORCIBLY_ALREADY_USED_KEY      | 3312       | 已使用强制映射密钥(ForcingMappingKey)。|
|                      | AUTH_ADD_MAPPING_FORCIBLY_EXPIRED_KEY           | 3313       | 强制映射密钥(ForcingMappingKey)的有效期已结束。|
|                      | AUTH_ADD_MAPPING_FORCIBLY_DIFFERENT_IDP         | 3314       | 强制映射密钥(ForcingMappingKey)已在其他IdP中使用。<br/>获得的ForcingMappingKey用于尝试相同IdP强制映射。|
|                      | AUTH_ADD_MAPPING_FORCIBLY_DIFFERENT_AUTHKEY     | 3315       | 强制映射密钥(ForcingMappingKey)已用于其他账户。<br/>获得的ForcingMappingKey用于尝试相同IdP及账户强制映射。|
| Remove Mapping | AUTH\_REMOVE\_MAPPING\_FAILED            | 3401       | 解除映射（Mapping）失败                           |
|                | AUTH\_REMOVE\_MAPPING\_LAST\_MAPPED\_IDP | 3402       | 无法解除最后映射（Mapping）的IdP。                |
|                | AUTH\_REMOVE\_MAPPING\_LOGGED\_IN\_IDP   | 3403       | 当前登录的IdP                      |
| Logout         | AUTH\_LOGOUT\_FAILED                     | 3501       | 退出登录失败                           |
| Withdrawal     | AUTH\_WITHDRAW\_FAILED                   | 3601       | 退出（删除数据）失败                 |
|                | AUTH\_WITHDRAW\_ALREADY\_TEMPORARY\_WITHDRAW | 3602   | 用户已临时退出                     |
|                | AUTH\_WITHDRAW\_NOT\_TEMPORARY\_WITHDRAW | 3603       | 用户未临时退出                     |
| Not Playable   | AUTH\_NOT\_PLAYABLE                      | 3701       | 是无法玩游戏的状态(维护或已下线等)。         |
| Auth(Unknown)  | AUTH\_UNKNOWN\_ERROR                     | 3999       | 未知错误(未定义的错误)             |

* 所有错误代码，请参考以下文档。
    * [Entire Error Codes](./error-code/#client-sdk)

**AUTH_EXTERNAL_LIBRARY_ERROR**

* 该错误为各IdP的SDK中发生的错误。
* 确认错误代码方式如下。

```java
Gamebase.login(activity, provider, new GamebaseDataCallback<AuthToken>() {
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
