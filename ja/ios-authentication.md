## Game > Gamebase > iOS SDK ご利用ガイド > 認証


## Login

Gamebaseでは基本的にゲストログインに対応しています。

- ゲスト以外のProviderにログインするには該当のProvider AuthAdapterが必要です。
- AuthAdapterおよび3rd-Party Provider SDKの設定は、次を参照してください。
    - [Game > Gamebase > iOS SDK使用ガイド > 始める > 3rd-Party Provider SDK Guide](ios-started#3rd-party-provider-sdk-guide)

ログインを試みるIdPごとにadditionalInfoのパラメーターを入力しなければならない場合があります。<br/>
AdditionalInfoに対する説明は下の**Gamebaseで対応しているIdP**の説明をご参考ください。


### Import Header File

ログインを設計するViewControllerに次のヘッダーファイルを持ってきます。

```objectivec
#import <Gamebase/Gamebase.h>
```

### Login Flow

多くのゲームが、タイトル画面でログインを実装します。

* アプリをインストールし、最初に起動した時、タイトル画面でゲームユーザーがどのIdP(identity provider)で認証するかを選択できるようにします。
* 一度ログインした後は、IdP選択画面を表示せず、以前にログインしたIdPタイプで認証します。

上述したロジックは、次のような手順で設計することができます。

![last provider login flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/login_for_last_logged_in_provider_flow_2.19.0.png)
![idp login flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/idp_login_flow_2.19.0.png)

#### 1. 前回のログインタイプで認証

* 前回の認証記録がある場合、IDとパスワードを入力させずに認証を試みます。
* **[TCGBGamebase loginForLastLoggedInProviderWithViewController:completion:]**を呼び出します。

#### 1-1. 認証に成功した場合

* おめでとうございます！認証に成功しました。
* **[TCGBGamebase userID]**でユーザーIDを取得し、ゲームロジックを設計してください。

#### 1-2. 認証に失敗した場合

* ネットワークエラー
    * エラーコードが **TCGB_ERROR_SOCKET_ERROR(110)**または **TCGB_ERROR_SOCKET_RESPONSE_TIMEOUT(101)**の場合、一時的なネットワークの問題で認証が失敗したということなので**[TCGBGamebase loginForLastLoggedInProviderWithViewController:completion:]**を再度呼び出すか、しばらくしてから再度試行します。
* 利用停止ゲームユーザー
    * エラーコードが**TCGB_ERROR_BANNED_MEMBER(7)**の場合、利用停止ゲームユーザーのため認証に失敗したということです。
    * **[TCGBBanInfo banInfoFromError:error]**で制裁情報を確認してゲームユーザーにゲームをプレイできない理由を伝えてください。
    * Gamebaseの初期化時、**[TCGBConfiguration enablePopup:YES]** および**[TCGBConfiguration enableBanPopup:YES]**を呼び出すと、Gamebaseが利用停止に関するポップアップを自動的に表示します。
* その他のエラー
    * 以前のログインタイプでの認証が失敗しました。**「2. 指定されたIdPで認証」**を行います。

#### 2. 指定されたIdPで認証

* IdPのタイプを直接指定して認証を試みます。
    * 認証可能なタイプは、**TCGBConstants.h**ファイルの**TCGBAuthIdPs**に宣言されています。
* **[TCGBGamebase loginWithType:viewController:completion:]**APIを呼び出します。

#### 2-1. 認証に成功した場合

* おめでとうございます！認証に成功しました。
* **[TCGBGamebase userID]**でユーザーIDを取得し、ゲームロジックを設計してください。

#### 2-2. 認証に失敗した場合

* ネットワークエラー
    * エラーコードが**TCGB_ERROR_SOCKET_ERROR(110)**または **TCGB_ERROR_SOCKET_RESPONSE_TIMEOUT(101)**の場合、一時的なネットワークの問題で認証に失敗したということです。**[TCGBGamebase loginWithType:viewController:completion:]**をもう一度呼び出すか、しばらくしてから再試行します。
* 利用停止ゲームユーザー
    * エラーコードが**TCGB_ERROR_BANNED_MEMBER(7)**の場合、利用停止ゲームユーザーのため認証に失敗したということです。
    * **[TCGBBanInfo banInfoFromError:error]**で制裁情報を確認して、ゲームユーザーにゲームをプレイできない理由を伝えてください。
    * Gamebase初期化時に **[TCGBConfiguration enablePopup:YES]**および **[TCGBConfiguration enableBanPopup:YES]**を呼び出すと、Gamebaseが利用停止に関するポップアップを自動的に表示します。
* その他のエラー
    * エラーが発生したことをゲームユーザーに伝え、ゲームユーザーが認証IdPタイプを選択できる状態(主にタイトル画面またはログイン画面)へ戻ります。

### Login as the Latest Login IdP

最後にログインしたIdPでログインを試みます。該当するログイントークンの期限が切れていたり、
トークン検証などに失敗した場合、失敗を返します。<br/>
この場合、該当するIdPに対するログインを設計する必要があります。




```objectivec
- (void)automaticLogin {
    [TCGBGamebase loginForLastLoggedInProviderWithViewController:topViewController completion:^(TCGBAuthToken *authToken, TCGBError *error){
        if ([TCGBGamebase isSuccessWithError:error] == YES) {
            NSLog(@"Login is succeeded.");
            //TODO: 1. Do you want.
        }
        else {
            if (error.code == TCGB_ERROR_SOCKET_ERROR || error.code == TCGB_ERROR_SOCKET_RESPONSE_TIMEOUT) {
                NSLog(@"Retry loginForLastLoggedInProviderWithViewController:completion: or Notify to user -\n\terror[%@]", [error description]);
                //TODO: 1. If the error had occured by network problem, you can retry by loginForLastLoggedInProviderWithViewController:completion:
            }
            else {
                NSLog(@"Try to login with loginWithType:viewController:completion:");
                // Last Logged In Provider Name
    			NSString *lastLoggedInProvider = [TCGBGamebase lastLoggedInProvider];
    			if (lastLoggedInProvider == nil || lastLoggedInProvider <= 0) {
                	//TODO: 1. Show your UI what user want to sign in.
                    //2. If the user has selected IdP, set lastLoggedInProvider to it.
                    //3. Invoke loginWithType:viewController:completion: method to try login.
                }

                // Try to login with IdP authentication
                //Warning: If you receive an event asynchronously from async handler(callback), you can use codes below in the async handler.
                [TCGBGamebase loginWithType:lastLoggedInProvider viewController:topViewController completion:^(TCGBAuthToken *authToken, TCGBError *error) {
                    if ([TCGBGamebase isSuccessWithError:error] == YES) {
                        NSLog(@"Login is succeeded.");
                    }
                    else {
                        NSLog(@"Login is failed.");
                    }
                }];
            }
        }
    }];
}
```

### Login with IdP

特定のIdPログインを呼び出すために**[TCGBGamebase loginWithType:viewController:completion:]**メソッドを呼び出します。<br/>
Gamebaseを通じてログインを初めて試みたり、ログイン情報(アクセストークン)の期限などが切れている場合、このAPIを使用してログインを試みる必要があります。<br/>
ログイン結果により**(TCGBError *)error**の客体を利用して成功したかどうかを判断することができます。<br/>
また、**TCGBAuthToken**の客体を利用してユーザーIDなどのユーザー情報及びトークン情報を取得することができます。<br/>
ログインに成功した場合、GamebaseのアクセストークンがLocal Storageに保存され、その後、loginForLastLoggedInProviderWithViewController:completion:のメソッドを使用する際に、保存されているアクセストークンを使用することになります。<br/>
ただし、IdPのアクセストークンは、各IdPが提供するSDKが管理します。<br/>

> [参考]
>
> iOSでサポートするIdPは **TCGBConstants.h**のTCGBAuthIDPs領域の**kTCGBAuthXXXXXX**に定義されています。
>

> [参考]
>
> ログインする時に追加情報を必要とするIdPもあります。
> このような追加情報を設定できるように**[TCGBGamebase loginWithType:additionalInfo:viewController:completion:]**APIを提供します。
> additionalInfoパラメータに必須情報をdictionary形式で入力してください。
> additionalInfo値がある場合にはその値を使用し、nullの場合には[NHN Cloud Console](./oper-app/#authentication-information)に登録された値を使用します。

> [参考]
>
> LINE IdPはGamebase SDK 2.43.0からLINEサービス提供地域を設定できます。
> 当該地域はadditionalInfoに設定できます。 

* additionalInfoパラメータの設定方法

| keyname                                  | a use                          | 値の種類                         |
| ---------------------------------------- | ------------------------------ | ------------------------------ |
| kTCGBAuthLoginWithCredentialLineChannelRegionKeyname | LINEサービス提供地域設定 | "japan"<br/>"thailand"<br/>"taiwan"<br/>"indonesia" |

**API**

```objectivec
+ (void)loginWithType:(NSString *)type viewController:(UIViewController *)viewController completion:(LoginCompletion)completion;
+ (void)loginWithType:(NSString *)type additionalInfo:(nullable NSDictionary<NSString *, id> *)additionalInfo viewController:(UIViewController *)viewController completion:(LoginCompletion)completion;
```

**Example**

```objectivec
- (void)loginFacebookButtonClick {
    [TCGBGamebase loginWithType:kTCGBAuthFacebook viewController:topViewController completion:^(TCGBAuthToken *authToken, TCGBError *error) {
        if ([TCGBGamebase isSuccessWithError:error] == YES) {
            // To Login Succeeded
            NSString *userId = [authToken.tcgbMember userId];
        } else {
            // To Login Failed
        }
    }];
}
```

```objectivec
- (void)loginLineButtonClick {
    NSDictionary *additionalInfo = @{ 
        @"key" : @"value" 
    };

    [TCGBGamebase loginWithType:kTCGBAuthLine additionalInfo:additionalInfo viewController:viewController completion:^(TCGBAuthToken *authToken, TCGBError *error) {    
       if ([TCGBGamebase isSuccessWithError:error] == YES) {
            // To Login Succeeded
            NSString *userId = [authToken.tcgbMember userId];
        } else {
            // To Login Failed
        }
    }];
}
```

### Login with Credential

IdPが提供するSDKを使ってゲームで直接認証した後、発行されたアクセストークンなどを利用してGamebaseにログインできるインターフェースです。


* Credentialパラメーターの設定方法

| keyname                                  | a use                          | 値の種類                           |
| ---------------------------------------- | ------------------------------ | ------------------------------ |
| kTCGBAuthLoginWithCredentialProviderNameKeyname | IdPタイプの設定                    | facebook, iosgamecenter, naver, google, twitter, line, appleid, hangame, weibo, kakaogame |
| kTCGBAuthLoginWithCredentialAccessTokenKeyname | IdPログイン後に取得した認証情報(アクセストークン)設定 |                                |
| kTCGBAuthLoginWithCredentialIgnoreAlreadyLoggedInKeyname | Gamebaseログイン状態でログアウトを行わなくても、他のアカウントログイン試行を許可する | **BOOL** |
|kTCGBAuthLoginWithCredentialLineChannelRegionKeyname | LINEサービス提供地域のうち、ログインを行う1つの地域 | [Login with IdP参考](./ios-authentication/#login-with-idp)|


> [参考]
>
> ゲーム内で外部サービス(Facebookなど)の固有機能を使用しなければならないとき、必要になることがあります。
>
<br/>

> <font color="red">[注意]</font><br/>
>
> 外部のSDKで対応を求める開発事項は、外部SDKのAPIを使用して設計する必要があり、Gamebaseでは対応しておりません。
>

```objectivec
#import "TCGBConstants.h"

- (void)authLoginWithCredential {
    NSDictionary *credentialDic = @{ kTCGBAuthLoginWithCredentialProviderNameKeyname: kTCGBAuthFacebook, kTCGBAuthLoginWithCredentialAccessTokenKeyname:@"ここにfacebook SDKで発行したAccess Tokenを入力してください" };
    [TCGBGamebase loginWithCredential:credentialDic viewController:parentViewController completion:^(TCGBAuthToken *authToken, TCGBError *error) {
        NSLog([authToken description]);
    }];
}
```

### Authentication Additional Information Settings

[Console Guide](./oper-app/#authentication-information)

## Logout

#### Import Header File

ログアウトを設計するViewControllerに次のヘッダーファイルを持ってきます。

```objectivec
#import <Gamebase/Gamebase.h>
```

#### Logout API

ログインされたIdPからのログアウトを試みます。主にゲームの設定画面にログアウトボタンを設け、ボタンをクリックすると実行されるように設計するケースが多いです。
ログアウトに成功してもゲームユーザーのデータは維持されます。
ログアウトに成功した場合、該当するIdPで認証を行った記録が削除されるため、次回ログインする時にID・パスワードの入力ウィンドウが表示されます。<br/><br/>

次は、ログアウトボタンをクリックするとログアウトされるコード例です。

```objectivec
- (void)authLogout {
    [TCGBGamebase logoutWithViewController:topViewController completion:^(TCGBError *error) {
        if ([TCGBGamebase isSuccessWithError:error] == YES) {
            // To Logout Succeeded
        } else {
            // To Logout Failed
        }
    }];
}
```




## Withdraw

### Import Header File

退会を設計するViewControllerに次のヘッダーファイルを持ってきます。

```objectivec
#import <Gamebase/Gamebase.h>
```

### Withdraw API

ログイン状態で退会を試行します。

* 退会成功時
  * ログインしていたIdPのゲーム利用者データは削除されます。
  * 該当IdPで再度ログインできます。その場合は新しいゲーム利用者データを作成します。
  * 連携中のすべてのIdPがログアウト処理されます。
* Gamebaseからの退会を意味するもので、IdPアカウントからの退会を意味するものではありません。

> <font color="red">[注意]</font><br/>
>
> 複数のIdPを連携中の場合、すべてのIdP連携が解除され、Gamebaseユーザーデータが削除されます。
>

次は、退会ボタンをクリックすると退会されるコード例です。

```objectivec
- (void)authWithdrawal {
    [TCGBGamebase withdrawWithViewController:topViewController completion:^(TCGBError *error) {
        if ([TCGBGamebase isSuccessWithError:error] == YES) {
            // To Withdrawal Succeeded
        } else {
            // To Withdrawal Failed
        }
    }];
}
```

## Mapping

マッピングは、既にログインされているアカウントに他のIdPアカウントを連携させたり、解除する機能です。

ほとんどのゲームにおいて、一つのゲームユーザーアカウントに複数のIdPを連携(マッピング)させることができます。<br/>GamebaseのマッピングAPIを使用すれば、既にログインされているアカウントに他のIdPアカウントを連携させたり、解除することができます。

つまり、連携中のIdPアカウントでログインを試みる場合、常に同じユーザーIDでログインされることになります。<br/><br/>

注意すべき点は、各IdPは一つのアカウントにのみ連携させることができるという点です。<br/>
例えば、Googleアカウントに連携中の場合、他のGoogleアカウントを追加で連携させることができません。<br/>
アカウント連携の例は、次の通りです。<br/><br/>

* GamebaseユーザーID:123bcabca
    * Google ID:aa
    * Facebook ID:bb
    * AppleGameCenter ID:cc
* GamebaseユーザーID:456abcabc
    * Google ID:ee
    * Google ID:ff **-> すでにGoogleのeeアカウントに連携されているため、Googleアカウントを追加で連携させることができません。**

マッピングAPIにはマッピング追加APIとマッピング解除APIがあります。

> <font color="red">[注意]</font><br/>
>
> Guestログイン中にマッピングに成功すると、Guest IdPは消えます。
>

### Add Mapping Flow

マッピングは、次の手順で設計することができます。

![add mapping flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_add_mapping_flow_2.30.0.png)

#### 1. ログイン
マッピングは、現在のアカウントにIdPアカウントの連携を追加する機能であるため、ログインされた状態でなければなりません。
まず、ログインAPIを呼び出してログインします。

#### 2. マッピング

**[TCGBGamebase addMappingWithType:viewController:completion:]**を呼び出してマッピングを試みます。

#### 2-1. マッピングに成功した場合

* おめでとうございます！現在のアカウントと連携しているIdPアカウントが追加されました。
* マッピングに成功しても、「現在ログイン中のIdP」は変わりません。つまり、Gamecenterアカウントでログインした後、Facebookアカウントのマッピングを試み、それが成功したからといって「現在ログイン中のIdP」がGamecenterからFacebookに変更されるわけではありません。Gamecenterのままで維持されます。
    * <font color="red">[注意]</font><br/>：Guestアカウントは例外です。Guestアカウントでログインした状態で試行したマッピングが成功した場合、Guest IdPは**削除**され、「現在ログイン中のIdP」もマッピングされたIdPに変更されます。
* マッピングは、単にIdP連携だけを追加する機能です。

#### 2-2. マッピングに失敗した場合

* ネットワークエラー
    * エラーコードが**TCGB_ERROR_SOCKET_ERROR(110)**または **TCGB_ERROR_SOCKET_RESPONSE_TIMEOUT(101)**の場合、一時的なネットワークの問題で認証が失敗したということなので、**[TCGBGamebase addMappingWithType:viewController:completion:]**を再度呼び出すか、しばらくしてから再度試行します。
* すでに他のアカウントに連携中の時に発生するエラー
    * エラーコードが**TCGB_ERROR_AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER(3302)**の場合、マッピングしようとしているIdPのアカウントがすでに他のアカウントに連動中という意味です。この時取得した**ForcingMappingTicket**で強制マッピング(**[TCGBGamebase addMappingForciblyWithTicket:viewController:completion:]**)またはログインアカウント変更(**[TCGBGamebase changeLoginWithForcingMappingTicket:viewController:completion:]**)を試行できます。
* すでに同じIdPアカウントに連携している時に発生するエラー
	* エラーコードが**TCGB_ERROR_AUTH_ADD_MAPPING_ALREADY_HAS_SAME_IDP(3303)**の場合、マッピングしようとしているIdPと同じ種類のアカウントがすでに連携中という意味です。
	* Gamebaseマッピングは、1つのIdPにつき1つのアカウントのみ連携可能です。例えばGoogleアカウントにすでに連携中の場合は、Googleアカウントを追加できません。
	* 同じIdPの他のアカウントを連携するには、**[TCGBGamebase removeMappingWithType:viewController:completion:]**を呼び出して連携を解除した後、再度マッピングを行ってください。
* その他のエラー
    * マッピングが失敗しました。

### Import Header file into ViewController

マッピングを設計するViewControllerに次のヘッダーファイルを持ってきます。

```objectivec
#import <Gamebase/Gamebase.h>
```



### Add Mapping API

特定のIdPにログインされた状態で他のIdPへのマッピングを試みます。<br/>

* additionalInfoパラメータ設定方法

| keyname                                  | a use                          | 値種類                         |
| ---------------------------------------- | ------------------------------ | ------------------------------ |
|kTCGBAuthLoginWithCredentialLineChannelRegionKeyname | LINEサービス提供地域のうち、ログインを行う1つの地域 | [Login with IdP参考](./ios-authentication/#login-with-idp)|

**API**

```objectivec
+ (void)addMappingWithType:(NSString *)type viewController:(UIViewController *)viewController completion:(LoginCompletion)completion;
'+ (void)addMappingWithType:(NSString *)type additionalInfo:(nullable NSDictionary<NSString *, id> *)additionalInfo viewController:(UIViewController *)viewController completion:(LoginCompletion)completion;
```

**Example**

次は、Facebookにマッピングを試みる例です。

```objectivec
- (void)authAddMapping {
    [TCGBGamebase addMappingWithType:kTCGBAuthFacebook viewController:parentViewController completion:^(TCGBAuthToken *authToken, TCGBError *error) {
        if ([TCGBGamebase isSuccessWithError:error] == YES) {
            NSLog(@"AddMapping is succeeded.");
        } else if (error.code == TCGB_ERROR_SOCKET_ERROR || error.code == TCGB_ERROR_SOCKET_RESPONSE_TIMEOUT) {
            NSLog(@"Retry addMapping");
        } else if (error.code == TCGB_ERROR_AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER) {
            NSLog(@"Already mapped to other member");
        } else {
            NSLog(@"AddMapping Error - %@", [error description]);
        }
    }];
}
```

### AddMapping with Credential

ゲームで直接IdPに提供するSDKで、予め認証を行いアクセストークンなどを利用してGamebase AddMappingをすることができるインターフェースです。




* Credentialパラメーターの設定方法



| keyname                                  | a use                          | 値の種類                           |
| ---------------------------------------- | ------------------------------ | ------------------------------ |
| kTCGBAuthLoginWithCredentialProviderNameKeyname | IdPタイプの設定                      | facebook, iosgamecenter, naver, google, twitter, line, appleid |
| kTCGBAuthLoginWithCredentialAccessTokenKeyname | IdPログイン後に取得した認証情報(アクセストークン)設定 |                                |



> [参考]
>
> ゲーム内で外部サービス(Facebookなど)の固有機能を使用しなければならないとき、必要になることがあります。
>

<br/>


> <font color="red">[注意]</font><br/>
>
> 外部のSDKで対応を求める開発事項は、外部SDKのAPIを使用して設計する必要があり、Gamebaseでは対応しておりません。
>

**API**

```objectivec
+ (void)addMappingWithCredential:(NSDictionary *)credentialInfo viewController:(UIViewController *)viewcontroller completion:(LoginCompletion)completion;
```

**Example**

```objectivec
- (void)authAddMappingCredential {
    UIViewController* topViewController = nil;
    NSString* facebookAccessToken = @"FACEBOOK_ACCESS_TOKEN";
    NSMutableDictionary* credentialInfo = [NSMutableDictionary dictionary];
    credentialInfo[kTCGBAuthLoginWithCredentialProviderNameKeyname] = kTCGBAuthFacebook;
    credentialInfo[kTCGBAuthLoginWithCredentialAccessTokenKeyname] = facebookAccessToken;
    [TCGBGamebase addMappingWithCredential:credentialInfo viewController:topViewController completion:^(TCGBAuthToken *authToken, TCGBError *error) {
        if ([TCGBGamebase isSuccessWithError:error] == YES) {
            NSLog(@"AddMapping is succeeded.");
        }
        else if (error.code == TCGB_ERROR_SOCKET_ERROR || error.code == TCGB_ERROR_SOCKET_RESPONSE_TIMEOUT) {
            NSLog(@"Retry addMapping");
        }
        else if (error.code == TCGB_ERROR_AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER) {
            NSLog(@"Already mapped to other member");
        }
        else {
            NSLog(@"AddMapping Error - %@", [error description]);
        }
    }];
}
```

### Add Mapping Forcibly
特定IdPにすでにマッピングされているアカウントがある時、**強制的に**マッピングを試行します。
**強制マッピング**を試行する時は、AddMapping APIで取得した`ForcingMappingTicket`が必要です。

**API**

```objectivec
+ (void)addMappingForciblyWithTicket:(TCGBForcingMappingTicket *)ticket viewController:(nullable UIViewController *)viewController completion:(LoginCompletion)completion;
```

**Example**

次はFacebookに強制マッピングを試行する例です。

```objectivec
- (void)authAddMappingForcibly {
    [TCGBGamebase addMappingWithType:kTCGBAuthFacebook viewController:parentViewController completion:^(TCGBAuthToken *authToken, TCGBError *error) {
        if ([TCGBGamebase isSuccessWithError:error] == YES) {
            NSLog(@"AddMapping is succeeded.");
        } else if (error.code == TCGB_ERROR_SOCKET_ERROR || error.code == TCGB_ERROR_SOCKET_RESPONSE_TIMEOUT) {
            NSLog(@"Retry addMapping");
        } else if (error.code == TCGB_ERROR_AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER) {
            NSLog(@"Already mapped to other member");
            TCGBForcingMappingTicket* ticket = [TCGBForcingMappingTicket forcingMappingTicketFromError:error];
            [TCGBGamebase addMappingForciblyWithTicket:ticket viewController:self completion:^(TCGBAuthToken *authToken, TCGBError *error) {
                if ([TCGBGamebase isSuccessWithError:error]) {
                    // Mapping success.
                }
                else {
                    // Mapping failed.
                }
            }];
        } else {
            NSLog(@"AddMapping Error - %@", [error description]);
        }
    }];
}
```

### Change Login with ForcingMappingTicket

特定IdPにすでにマッピングされているアカウントがある時、**ログインアカウントを変更**します。
**ログインアカウントを変更**する時はAddMapping APIで取得した`ForcingMappingTicket`が必要です。

Change Login APIの呼び出しが失敗した場合、以前のアカウントのログイン状態が維持されます。

**API**

```objectivec
+ (void)changeLoginWithForcingMappingTicket:(TCGBForcingMappingTicket *)ticket viewController:(nullable UIViewController *)viewController completion:(LoginCompletion)completion;
```

**Example**

次はFacebookにログインしているアカウントの変更を試行する例です。

```objectivec
- (void)authChangeLogin {
    [TCGBGamebase addMappingWithType:kTCGBAuthFacebook viewController:parentViewController completion:^(TCGBAuthToken *authToken, TCGBError *error) {
        if ([TCGBGamebase isSuccessWithError:error] == YES) {
            NSLog(@"AddMapping is succeeded.");
        } else if (error.code == TCGB_ERROR_SOCKET_ERROR || error.code == TCGB_ERROR_SOCKET_RESPONSE_TIMEOUT) {
            NSLog(@"Retry addMapping");
        } else if (error.code == TCGB_ERROR_AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER) {
            NSLog(@"Already mapped to other member");
            TCGBForcingMappingTicket* ticket = [TCGBForcingMappingTicket forcingMappingTicketFromError:error];
            [TCGBGamebase changeLoginWithForcingMappingTicket:ticket viewController:self completion:^(TCGBAuthToken *authToken, TCGBError *error) {
                if ([TCGBGamebase isSuccessWithError:error]) {
                    // Change login successed.
                }
                else {
                    // Change login failed.
                    // The login status of the previous account is maintained.
                }
            }];
        } else {
            NSLog(@"AddMapping Error - %@", [error description]);
        }
    }];
}
```

### Remove Mapping API

特定のIDPに対する連携を解除します。<br/>
解除しようとしているIdP以外に**IdPがない**場合、失敗を返します。<br/>
連携を解除した後は、Gamebase内部で該当するIdPに対するログアウト処理を行います。

```objectivec
[TCGBGamebase removeMappingWithType:kTCGBAuthFacebook viewController:topViewController completion:^(TCGBError *error) {
    if ([TCGBGamebase isSuccessWithError:error] == YES) {
        // To Remove Mapping Succeeded
    } else {
        // To Remove Mapping Failed cause of the error
    }
}];
```

### Get IdP Mapping List
現在のアカウントがどんなIdPとマッピングされているか、リストを確認することができます。

```objectivec
// Obtaining Names of Mapping IdPs
NSArray* authMappingList = [TCGBGamebase authMappingList];
```


## Gamebase User`s Information
Gamebaseで認証フローを進めた後、アプリを制作する際に必要な情報を得ることができます。

> <font color="red">[注意]</font><br/>
>
> "[TCGBGamebase loginForLastLoggedInProvider]"APIでログインした場合、認証情報を取得することができません。
>
> 認証情報が必要な場合、"[TCGBGamebase loginForLastLoggedInProvider]"の代わりに、使用したいIDPCodeと同じ{IDP_CODE}をパラメータにして"[TCGBGamebase loginWithType:IDP_CODE viewController:topViewController completion:completion];" APIでログインすると、正常に認証情報を取得できます。

### Get Authentication Information for Gamebase
Gamebaseから発行された認証情報を取得することができます。

```objectivec
// Obtaining Gamebase UserID
NSString* gamebaseUserID = [TCGBGamebase userID];

// Obtaining Gamebase AccessToken
NSString* gamebaseAccessToken = [TCGBGamebase accessToken];

// Obtaining Last Logged In Provider
NSString* lastProviderName = [TCGBGamebase lastLoggedInProvider];
```


### Get Authentication Information for External IdP

* 外部認証IdPのアクセストークン、ユーザーID、Profileなどの情報はログイン後、ゲームサーバーでGamebase Server APIを呼び出して取得できます。
    * [Game > Gamebase > APIガイド > Authentication > Get IdP Token and Profiles](./api-guide/#get-idp-token-and-profiles)

> <font color="red">[注意]</font><br/>
>
> * 外部IdPの認証情報はセキュリティのためにゲームサーバーで呼び出すことを推奨します。
> * IdPによってはアクセストークンの有効期限が短い場合があります。
>     * 例えばGoogleは、ログインしてから2時間後にはアクセストークンの有効期限が切れます。
>     * ユーザー情報が必要な場合は、ログイン後すぐにGamebase Server APIを呼び出してください。
> * "[TCGBGamebase loginForLastLoggedInProviderWithViewController:completion:]" APIでログインした場合は認証情報を取得できません。
>     * ユーザー情報が必要な場合は、"[TCGBGamebase loginForLastLoggedInProviderWithViewController:completion:]"の代わりに使用したいIDPCodeと同じ{IDP_CODE}をパラメータにして"[TCGBGamebase loginWithType:viewController:completion:]"APIでログインする必要があります。

> <font color="red">[注意]</font><br/>
>
> iOS 12以下のappleidログインの場合、認証情報を照会できません。
>

### Get Banned User Information

Gamebase Consoleに制裁されたゲームユーザーとして登録されている場合、
ログインを試行すると、以下のような利用制限情報コードが表示されることがあります。**[TCGBBanInfo banInfoFromError:error]**メソッドを利用して制裁情報を確認できます。

* TCGB_ERROR_BANNED_MEMBER




## TransferAccount
ゲストアカウントを他の端末に移行するためのキーを発行する機能です。

このキーを**TransferAccountInfo**と呼びます。
発行されたTransferAccountInfoは、他の端末で**requestTransferAccount**APIを呼び出してアカウントを移行できます。

> <font color="red">[注意]</font><br/>
> TransferAccountInfoは、ゲストログイン状態でのみ発行できます。
> TransferAccountInfoを利用したアカウント移行は、ゲストログイン状態またはログインされていない状態でのみ可能です。
> ログインしたゲストアカウントがすでに他の外部IdP(Google、Facebook など)アカウントとマッピングされている場合、アカウント移行がサポートされません。

### Issue TransferAccount
ゲストアカウントを移行するためにTransferAccountInfoを発行します。

**API**

```objectivec
+ (void)issueTransferAccountWithCompletion:(TransferAccountCompletion)completion;
```

**Example**

```objectivec
 - (void)issueTransferAccount {
     [TCGBGamebase issueTransferAccountWithCompletion:^(TCGBTransferAccountInfo* transferAccount, TCGBError *error) {
        NSLog(@"Issued TransferAccount => %@, error => %@", [transferAccount description], [error description]);
     }];
 }
```

### Query TransferAccount
ゲストアカウントを移行するために、すでに発行されているTransferAccountInfo情報をGamebaseサーバーに問い合わせます。

**API**

```objectivec
+ (void)queryTransferAccountWithCompletion:(TransferAccountCompletion)completion;
```

**Example**

```objectivec
 - (void)queryTransferAccount {
     [TCGBGamebase queryTransferAccountWithCompletion:^(TCGBTransferAccountInfo* transferAccount, TCGBError *error) {
        NSLog(@"Published TransferAccount => %@, error => %@", [transferAccount description], [error description]);
     }];
 }
```


### Renew TransferAccount
すでに発行されたTransferAccountInfo情報を更新します。
更新方法には**自動更新**と**手動更新**があり、**パスワードのみ更新**、**IDとパスワードを更新**を選択してTransferAccountInfo情報を更新できます。

```objectivec
+ (void)renewTransferAccountWithConfiguration:(TCGBTransferAccountRenewConfiguration *)config completion:(TransferAccountCompletion)completion;
```

**Example**

```objectivec
- (void)renewTransferAccount {
    // If you want renew the account automatically, use this config.
    TCGBTransferAccountRenewalTargetType renewalTargetType = TCGBTransferAccountRenewalTargetTypeIdPassword;
    TCGBTransferAccountRenewConfiguration* autoConfig = [TCGBTransferAccountRenewConfiguration autoRenewConfigurationWithRenewalTarget:renewalTargetType];

    // If you want renew the account manually, use this config.
    TCGBTransferAccountRenewConfiguration* manualConfig = [TCGBTransferAccountRenewConfiguration manualRenewConfigurationWithAccountId:@"ID" accountPassword:@"PASSWORD"];
    [TCGBGamebase renewTransferAccountWithConfiguration:autoConfig completion:^(TCGBTransferAccountInfo *transferAccount, TCGBError *error) {
        if ([TCGBGamebase isSuccessWithError:error]) {
            // Renewing TransferAccount success.
            NSString* accountId = transferAccount.account.accountId;
            NSString* accountPw = transferAccount.account.accountPassword;
            return;
        }
        else {
            // Renewing TransferAccount failed.
        }
    }];
}
```




### Transfer Guest Account to Another Device
**issueTransfer**APIで発行したTransferAccountにアカウントを移行する機能です。
アカウントの移行に成功した時、TransferAccountを発行した端末から移行完了メッセージが表示される場合があり、ゲストログインすると新規のアカウントが作成されます。
アカウント移行が成功した端末では、TransferAccountを発行した端末のゲストアカウントを継続して使用できます。

> <font color="red">[注意]</font><br/>
>
> すでにゲストログインしている状態で移行が成功すると、端末にログインしていたゲストアカウントは消失します。
> 間違ったid/passwordを連続して入力した場合、**AUTH_TRANSFERACCOUNT_BLOCK(3042)**エラーが発生して、アカウント移行が一定時間できなくなります。
> この場合は、以下の例のように、TCGBTransferAccountFailInfo値を利用して、いつまでアカウント移行ができないのかをユーザーに伝えることができます。



**API**

```objectivec
+ (void)transferAccountWithIdPLoginWithAccountId:(NSString *)accountId accountPassword:(NSString *)accountPassword completion:(void(^)(TCGBAuthToken* authToken, TCGBError* error))completion;
```

**Example**

```objectivec
 - (void)transferOtherDevice {
    [TCGBGamebase transferAccountWithIdPLoginWithAccountId:@"1Aie0198" accountPassword:@"1Aie0199" completion:^(TCGBAuthToken* authToken, TCGBError* error) {
       if (error.code == TCGB_ERROR_AUTH_TRANSFERACCOUNT_BLOCK) {
            // Transfering Account failed.
            TCGBTransferAccountFailInfo* failInfo = [TCGBTransferAccountFailInfo transferAccountFailInfoFrom:error];
            if (failInfo == nil) {
                // Transfering Account failed by entering the wrong id / pw multiple times.
                // You can tell when the account transfer is blocked by the TransferAccountFailInfo.

                NSString *failedId = failInfo.accountId;
                NSInteger failCount = failInfo.failCount;
                NSDate *blockedDate = [NSDate dateTimeIntervalSince1970:(failInfo.blockEndDate / 1000.0)];
                return;
            }
            // Transfering Account failed by another reason.
            return;  
        }
        // Transfering Account success.
        // TODO: implements post login process
    }];
 }
```



## TemporaryWithdrawal

「退会猶予」機能です。
一時退会をリクエストして即時に退会が行われずに一定期間の猶予期間が過ぎると、退会が行われます。
猶予期間はコンソールで変更できます。

> <font color="red">[注意]</font><br/>
>
> 退会猶予機能を使用する場合には**[TCGBGamebase withdrawWithViewController:completion:]**APIを使用しないでください。
> **[TCGBGamebase withdrawWithViewController:completion:]**APIは即時にアカウントを退会します。

ログインが成功すると、AuthToken.getTemporaryWithdrawalInfo() APIを呼び出して退会猶予状態のユーザーかを判断できます。

### Request TemporaryWithdrawal

一時退会をリクエストします。
コンソールに指定した期間が過ぎると自動的に退会進行が完了します。

**API**

```objectivec
'+ (void)requestTemporaryWithdrawalWithViewController:(nullable UIViewController *)viewController completion:(nullable TemporaryWithdrawCompletion)completion;
```

**ErrorCode**

|Error Code | Description |
| --- | --- |
| TCGB\_ERROR\_AUTH\_WITHDRAW\_ALREADY\_TEMPORARY\_WITHDRAW(3602) | すでに一時退会中のユーザーです。 |

**Example**

```objectivec
- (void)testRequestWithdraw {
    [TCGBGamebase requestTemporaryWithdrawalWithViewController:parentViewController completion:^(TCGBTemporaryWithdrawalInfo *info, TCGBError *error) {
        if ([TCGBGamebase isSuccessWithError:error] == NO) {
            if (error.code == TCGB_ERROR_AUTH_WITHDRAW_ALREADY_TEMPORARY_WITHDRAW) {
                // Already requested temporary withdrawal before.
            }
            else {
                // Request temporary withdrawal failed.
                return;
            }
        }

        // Request temporary withdrawal success.
    }];
}
```

### Check TemporaryWithdrawal User

退会猶予を使用するゲームはログイン後に常に**TCGBAuthToken.tcgbMember.temporaryWithdrawal**を使用し、結果がnullではない有効なTemporaryWithdrawalInfoオブジェクトを返す場合、該当ユーザーに退会進行中であることを伝える必要があります。

**Example**


```objectivec
- (void)testLogin {
    [TCGBGamebase loginWithType:@"appleid" viewController:parentViewController completion:^(TCGBAuthToken *authToken, TCGBError *error) {
        if ([TCGBGamebase isSuccessWithError:error] == NO) {
            // Login failed
            return;
        }

        // Check if user is requesting withdrawal
        if (authToken.tcgbMember.temporaryWithdrawal != nil) {
            // User is under temporary withdrawal
            long gradePeriod = authToken.tcgbMember.temporaryWithdrawal.gracePeriodDate;
        }
        else {
            // Login Success
        }
    }];
}
```


### Cancel TemporaryWithdrawal

退会リクエストをキャンセルします。
退会リクエストした後、期間が満了して退会が完了すると、キャンセルができません。

**API**

```objectivec
+ (void)cancelTemporaryWithdrawalWithViewController:(UIViewController *)viewController completion:(WithdrawCompletion)completion;
```

**ErrorCode**

|Error Code | Description |
| --- | --- |
| TCGB\_ERROR\_AUTH\_WITHDRAW\_NOT\_TEMPORARY\_WITHDRAW(3603) | 一時退会中のユーザーではありません。 |

**Example**

```objectivec
- (void)testCancelWithdraw {
    [TCGBGamebase cancelTemporaryWithdrawalWithViewController:parentViewController completion:^(TCGBError *error) {
        if ([TCGBGamebase isSuccessWithError:error] == NO) {
            if (error.code == TCGB_ERROR_AUTH_WITHDRAW_NOT_TEMPORARY_WITHDRAW) {
                // Never requested temporary withdrawal before.
            }
            else {
                // Cancel temporary withdrawal failed.
                return
            }
        }

        // Cancel temporary withdrawal success.
    }];
}
```

### Withdraw Immediately

退会猶予期間を無視して、即時退会を進行します。
実際の内部動作は**[TCGBGamebase withdrawWithViewController:completion:]**APIと同じです。

即時退会はキャンセルできないため、実行するかどうかをユーザーによく確認してください。

**API**

```objectivec
+ (void)withdrawImmediatelyWithViewController:(UIViewController *)viewController completion:(WithdrawCompletion)completion;
```


**Example**

```objectivec
- (void)testWithdrawImmediately {
    [TCGBGamebase withdrawImmediatelyWithViewController:parentViewController completion:^(TCGBError *error) {
        if ([TCGBGamebase isSuccessWithError:error] == NO) {
            // withdraw failed.
            return;
        }

        // Withdraw success.
    }];
}
```

## GraceBan

* 「決済アビューズ自動解除」機能です。
    * 決済アビューズ自動解除機能は、決済アビューズ自動制裁で利用停止にならなければいけないユーザーが利用停止猶予状態後、利用停止になるようにします。
    * 利用停止猶予状態の場合、設定した期間内に解除条件を全て満たすと正常にプレイが可能になります。
    * 期間内に条件を満たせなかった場合、利用停止になります。
* 決済アビューズ自動解除機能を使用するゲームはログイン後、常にTCGBAuthToken.tcgbMember.graceBanInfo値を確認し、nullではない有効なTCGBGraceBanInfoオブジェクトを返した場合、該当ユーザーに利用停止解除条件、期間などを案内する必要があります。
    * 利用停止猶予状態のユーザーのゲーム内アクセス制御はゲームで処理する必要があります。

**Example**

```objectivec
- (void)testGraceBanInfo {
    [TCGBGamebase loginWithType:kTCGBAuthAppleID viewController:viewController completion:^(TCGBAuthToken *authToken, TCGBError *error) {
        if ([TCGBGamebase isSuccessWithError:error] == NO) {
            // Login failed
            return;
        }
        
        // Check if user is under grace ban
        if (authToken.tcgbMember.graceBanInfo != nil) {
            TCGBGraceBanInfo *graceBanInfo = authToken.tcgbMember.graceBanInfo;
            // gracePeriodDate : epoch time in milliseconds
            long long gracePeriodDate = graceBanInfo.gracePeriodDate;
            NSString *message = [graceBanInfo.message stringByRemovingPercentEncoding];
            if (graceBanInfo.paymentStatus != nil) {
                TCGBPaymentStatus *paymentStatus = graceBanInfo.paymentStatus;
                double paymentStatusAmount = paymentStatus.amount;
                int paymentStatusCount = paymentStatus.count;
            }
            if (graceBanInfo.releaseRuleCondition != nil) {
                TCGBReleaseRuleCondition *releaseRuleCondition = graceBanInfo.releaseRuleCondition;
                double releaseRuleConditionAmount = releaseRuleCondition.amount;
                int releaseRuleConditionCount = releaseRuleCondition.count;
                NSString *releaseRuleConditionCurrency = releaseRuleCondition.currency;
                // condition type : "AND", "OR"
                NSString *releaseRuleConditionType = releaseRuleCondition.conditionType;
            }
            // Guide the user through the UI how to finish the grace ban status.
        }
        else {
            // Login Success
        }
    }];
}
```

## Error Handling

| Category       | Error                                    | Error Code | Description                              |
| -------------- | ---------------------------------------- | ---------- | ---------------------------------------- |
| Auth           | TCGB\_ERROR\_INVALID\_MEMBER             | 6          | 無効な会員へのリクエストです。                        |
|                | TCGB\_ERROR\_BANNED\_MEMBER              | 7          | 制裁中の会員です。                               |
|                | TCGB\_ERROR\_AUTH\_USER\_CANCELED        | 3001       | ログインがキャンセルされました。                            |
|                | TCGB\_ERROR\_AUTH\_NOT\_SUPPORTED\_PROVIDER | 3002       | サポートしていない認証方式です。                        |
|                | TCGB\_ERROR\_AUTH\_NOT\_EXIST\_MEMBER    | 3003       | 存在しないか、退会した会員です。                      |
|                | TCGB\_ERROR\_AUTH\_EXTERNAL\_LIBRARY\_INITIALIZATION\_ERROR    | 3006       | 外部認証ライブラリの初期化に失敗しました。                      |
|                | TCGB\_ERROR\_AUTH\_EXTERNAL\_LIBRARY\_ERROR | 3009       | 外部認証ライブラリエラーです。<br/>詳細エラーを確認してください。  |
|                | TCGB\_ERROR\_AUTH\_INVALID\_GAMEBASE\_TOKEN | 3011       | Gamebase Access Tokenが有効ではないためログアウトしました。<br/>ログインを再試行してください。 |
| Auth (Login)   | TCGB\_ERROR\_AUTH\_TOKEN\_LOGIN\_FAILED  | 3101       | トークンのログインに失敗しました。                          |
|                | TCGB\_ERROR\_AUTH\_TOKEN\_LOGIN\_INVALID\_TOKEN\_INFO | 3102       | トークン情報が有効ではありません。                        |
|                | TCGB\_ERROR\_AUTH\_TOKEN\_LOGIN\_INVALID\_LAST\_LOGGED\_IN\_IDP | 3103       | 最近ログインしたIdP情報がありません。                   |
| IdP Login      | TCGB\_ERROR\_AUTH\_IDP\_LOGIN\_FAILED    | 3201       | IdPログインに失敗しました。                        |
|                | TCGB\_ERROR\_AUTH\_IDP\_LOGIN\_INVALID\_IDP\_INFO | 3202       | IdP情報が有効ではありません。 (Consoleに該当IdP情報がありません。) |
|                | TCGB\_ERROR\_AUTH\_IDP\_LOGIN\_EXTERNAL\_AUTHENTICATION\_REQUIRED | 3203       | Gamebaseログインを要求する前に、まず IdP ログインしている必要があります。
| Add Mapping    | TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_FAILED  | 3301       | マッピングの追加に失敗しました。                           |
|                | TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_ALREADY\_MAPPED\_TO\_OTHER\_MEMBER | 3302       | すでに他のメンバーにマッピングされています。                      |
|                | TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_ALREADY\_HAS\_SAME\_IDP | 3303       | すでに同じIdPにマッピングされています。                     |
|                | TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_INVALID\_IDP\_INFO | 3304       | IdP情報が有効ではありません。 (Consoleに該当IdP情報がありません。) |
|                | TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_CANNOT\_ADD\_GUEST\_IDP | 3305  | ゲストIdPではAddMappingができません。 |
| Remove Mapping | TCGB\_ERROR\_AUTH\_REMOVE\_MAPPING\_FAILED | 3401       | マッピングの削除に失敗しました。                           |
|                | TCGB\_ERROR\_AUTH\_REMOVE\_MAPPING\_LAST\_MAPPED\_IDP | 3402       | 最後にマッピングされたIdPは削除できません。                |
|                | TCGB\_ERROR\_AUTH\_REMOVE\_MAPPING\_LOGGED\_IN\_IDP | 3403       | 現在ログインしているIdPです。                     |
| Logout         | TCGB\_ERROR\_AUTH\_LOGOUT\_FAILED        | 3501       | ログアウトに失敗しました。                            |
| Withdrawal     | TCGB\_ERROR\_AUTH\_WITHDRAW\_FAILED      | 3601       | 退会に失敗しました。                              |
|                | TCGB\_ERROR\_AUTH\_WITHDRAW\_ALREADY\_TEMPORARY\_WITHDRAW | 3602   | すでに一時退会中のユーザーです。                    |
|                | TCGB\_ERROR\_AUTH\_WITHDRAW\_NOT\_TEMPORARY\_WITHDRAW | 3603       | 一時退会中のユーザーではありません。                     |
| Not Playable   | TCGB\_ERROR\_AUTH\_NOT\_PLAYABLE         | 3701       | プレイできない状態です(メンテナンスまたはサービス終了など)。        |
| Auth(Unknown)  | TCGB\_ERROR\_AUTH\_UNKNOWN\_ERROR        | 3999       | 不明なエラーです。 (定義されていないエラーです。)            |




* エラーコードは次の文書を参照してください。
    - [エラーコード](./error-code/#client-sdk)


**TCGB\_ERROR\_AUTH\_EXTERNAL\_LIBRARY\_ERROR**

* このエラーは外部認証ライブラリでエラーが発生した時に返されます。
* 外部認証ライブラリで発生したエラー情報は詳細エラーに含まれており、詳細なエラーコードおよびメッセージは次のように確認できます。 

```objectivec
TCGBError *tcgbError = error; // TCGBError object via callback

NSInteger detailErrorCode = [error detailErrorCode];
NSString *detailErrorMessage = [error detailErrorMessage];
// If you use **description** method, you can get entire information of this object by JSON Format
NSLog(@"TCGBError: %@", [tcgbError description]);
```

* 詳細エラーコードは、それぞれの外部認証ライブラリのDeveloperページを参照してください。
