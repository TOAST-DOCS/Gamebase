## Game > Gamebase > iOS SDK ご利用ガイド > 認証


## Login

Gamebaseでは基本的にゲストログインに対応しています。

- ゲスト以外のProviderでログインするためには、該当するProvider AuthAdapterが必要です。
- AuthAdapter及び3rd-Party Provider SDKの設定は、次をご参考ください。
    - [3rd-Party Provider SDK Guide](ios-started#3rd-party-provider-sdk-guide)

ログインを試みるIdPごとにadditionalInfoのパラメーターを入力しなければならない場合があります。<br/>
AdditionalInfoに対する説明は下の**Gamebaseで対応しているIdP**の説明をご参考ください。


### Import Header File

ログインを設計するViewControllerに次のヘッダーファイルを持ってきます。

```objectivec
#import <Gamebase/Gamebase.h>
```

### Login Flow

多くのゲームがタイトル画面にログインを設計しています。
* アプリをインストールして初めて起動したとき、タイトル画面からゲームユーザーがどのIdP(identity provider)で認証するか選択できるようにします。
* 一度ログインした後は、IdP選択画面を表示せずに、前回ログインしたIdPタイプで認証します。

上述したロジックは、次のような手順で設計することができます。

![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_001_2.6.0.png)
![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_002_1.10.0.png)
![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_003_1.10.0.png)
![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_004_1.10.0.png)
![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_005_1.10.0.png)

#### 1. 前回のログインタイプで認証

* 前回の認証記録がある場合、IDとパスワードを入力させずに認証を試みます。
* **[TCGBGamebase loginForLastLoggedInProviderWithViewController:completion:]**を呼び出します。

#### 1-1. 認証に成功した場合

* おめでとうございます！認証に成功しました。
* **[TCGBGamebase userID]**でユーザーIDを取得し、ゲームロジックを設計してください。

#### 1-2. 認証に失敗した場合

* ネットワークエラー
    * エラーコードが**TCGB_ERROR_SOCKET_ERROR(110)**または**TCGB_ERROR_SOCKET_RESPONSE_TIMEOUT(101)**の場合、一時的なネットワーク問題により認証に失敗したケースであるため、**[TCGBGamebase loginForLastLoggedInProviderWithViewController:completion:]**をもう一度呼び出したり、しばらくしてからもう一度試します。
* 利用停止中のゲームユーザー
    * エラーコードが**TCGB_ERROR_AUTH_BANNED_MEMBER(3005)**の場合、利用停止中のゲームユーザーであるため認証に失敗したケースです。
    * **[TCGBGamebase banInfo]**で利用制限情報を確認し、ゲームユーザーに対しゲームプレイができない理由について知らせてください。
    * Gamebaseを初期化する際に**[TCGBConfiguration enablePopup:YES]**及び**[TCGBConfiguration enableBanPopup:YES]**を呼び出せば、Gamebaseが利用停止に関するポップアップを自動で表示します。
* その他のエラー
    * 前回のログインタイプで認証に失敗しました。**3. 指定されたIdPで認証**を進めます。

#### 2. 指定されたIdPで認証

* IdPのタイプを直接指定して認証を試みます。
    * 認証可能なタイプは、**TCGBConstants.h**ファイルの**TCGBAuthIdPs**に宣言されています。
* **[TCGBGamebase loginWithType:viewController:completion:]**APIを呼び出します。

#### 2-1. 認証に成功した場合

* おめでとうございます！認証に成功しました。
* **[TCGBGamebase userID]**でユーザーIDを取得し、ゲームロジックを設計してください。

#### 2-2. 認証に失敗した場合

* ネットワークエラー
    * エラーコードが**TCGB_ERROR_SOCKET_ERROR(110)**または**TCGB_ERROR_SOCKET_RESPONSE_TIMEOUT(101)**の場合、一時的なネットワーク問題により認証に失敗したケースであるため、**[TCGBGamebase loginWithType:viewController:completion:]**をもう一度呼び出したり、しばらくしてからもう一度試します。
* 利用停止中のゲームユーザー
    * エラーコードが**TCGB_ERROR_AUTH_BANNED_MEMBER(3005)**の場合、利用停止中のゲームユーザーであるため認証に失敗したケースです。
    * **[TCGBGamebase banInfo]** で利用制限情報を確認し、ゲームユーザーに対しゲームプレイができない理由について知らせてください。
    * Gamebaseを初期化する際に**[TCGBConfiguration enablePopup:YES]**及び**[TCGBConfiguration enableBanPopup:YES]**を呼び出せば、Gamebaseが利用停止に関するポップアップを自動で表示します。
* その他のエラー
    * エラーが発生したことをゲームユーザーに知らせ、ゲームユーザーが認証IdPのタイプを選択できる状態(主にタイトル画面またはログイン画面)に戻ります。

### Login as the Latest Login IdP

特定のIDPに対するログインボタンをクリックしたとき、次のログインAPIを設計します。<br/>
最後にログインしたIdPでログインを試みます。該当するログイントークンの期限が切れていたり、
トークン検証などに失敗した場合、失敗を返します。<br/>
この場合、該当するIdPに対するログインを設計する必要があります。




```objectivec
- (void)automaticLogin {
    [TCGBGamebase loginForLastLoggedInProviderWithViewController:self completion:^(TCGBAuthToken *authToken, TCGBError *error){
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

<br/><br/>
IdPの中には、ログインする際に必ず必要な情報があるものがあります。<br/>
例えば、Facebookログインを設計する場合、scopeなどを設定する必要があります。<br/>
このような必須情報を設定することができるように**[TCGBGamebase loginWithType:additionalInfo:viewController:completion:]**APIを提供します。<br/>
パラメーターのadditionalInfoに必須情報をdictionary形式で入力してください。<br/>
(パラメーター値がnilの場合、NHN Cloud Consoleに登録したadditionalInfoの値が埋められます。パラメーター値がある場合、Consoleに登録してある値よりもこちらを優先してその値を上書きします。)


> [参考]
>
> iOSで対応しているIdPは、**TCGBConstants.h**のTCGBAuthIDPs領域の**kTCGBAuthXXXXXX**で定義されています。
>

```objectivec
- (void)loginPaycoButtonClick {
    [TCGBGamebase loginWithType:kTCGBAuthPayco viewController:self completion:^(TCGBAuthToken *authToken, TCGBError *error) {
        if ([TCGBGamebase isSuccessWithError:error] == YES) {
            // To Login Succeeded
            NSString *userId = [authToken.tcgbMember.userId];
        } else {
            // To Login Failed
        }
    }];
}
```

#### Gamebaseでサポート中のIdP
[Console Guide](./oper-app/#authentication-information)を参照してください。

### Login with Credential

IdPが提供するSDKを使ってゲームで直接認証した後、発行されたアクセストークンなどを利用してGamebaseにログインできるインターフェースです。




* Credentialパラメーターの設定方法



| keyname                                  | a use                          | 値の種類                           |
| ---------------------------------------- | ------------------------------ | ------------------------------ |
| kTCGBAuthLoginWithCredentialProviderNameKeyname | IdPタイプの設定                      | facebook, payco, iosgamecenter, naver, google, twitter, line, appleid |
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

```objectivec
#import "TCGBConstants.h"

- (void)authLoginWithCredential {
    NSDictionary *credentialDic = @{ kTCGBAuthLoginWithCredentialProviderNameKeyname: @"facebook", kTCGBAuthLoginWithCredentialAccessTokenKeyname:@"ここにfacebook SDKで発行したAccess Tokenを入力してください" };
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
    [TCGBGamebase logoutWithViewController:self completion:^(TCGBError *error) {
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

### Widthdraw API

次は、ログイン状態でのゲームユーザー退会を設計するコード例です。<br/><br/>

* 退会に成功した場合、ログインしたIdPに紐づいていたゲームユーザーデータは削除されます。
* 該当するIdPでもう一度ログインすることができ、新しいゲームユーザーデータを作成します。
* Gamebaseからの退会を意味するもので、IdPアカウントからの退会を意味するものではありません。
* 退会に成功すると、IdPログアウトを試みます。

次は、退会ボタンをクリックすると退会されるコード例です。

```objectivec
- (void)authWithdrawal {
    [TCGBGamebase withdrawWithViewController:self completion:^(TCGBError *error) {
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
    * Payco ID:dd
* GamebaseユーザーID:456abcabc
    * Google ID:ee
    * Google ID:ff **-> すでにGoogleのeeアカウントに連携されているため、Googleアカウントを追加で連携させることができません。**

マッピングAPIにはマッピング追加APIとマッピング解除APIがあります。

### Add Mapping Flow

マッピングは、次の手順で設計することができます。

#### 1. ログイン
マッピングは、現在のアカウントにIdPアカウントの連携を追加する機能であるため、ログインされた状態でなければなりません。
まず、ログインAPIを呼び出してログインします。

#### 2. マッピング

**[TCGBGamebase addMappingWithType:viewController:completion:]**を呼び出してマッピングを試みます。

#### 2-1. マッピングに成功した場合

* おめでとうございます！現在のアカウントと連携しているIdPアカウントが追加されました。
* マッピングに成功しても、「現在ログイン中のIdP」は変わりません。つまり、Gamecenterアカウントでログインした後、Facebookアカウントのマッピングを試み、それが成功したからといって「現在ログイン中のIdP」がGamecenterからFacebookに変更されるわけではありません。Gamecenterのままで維持されます。
* マッピングは、単にIdP連携だけを追加する機能です。

#### 2-2. マッピングに失敗した場合

* ネットワークエラー
    * エラーコードが**TCGB_ERROR_SOCKET_ERROR(110)**または**TCGB_ERROR_SOCKET_RESPONSE_TIMEOUT(101)**の場合、一時的なネットワーク問題により認証に失敗したケースであるため、**[TCGBGamebase addMappingWithType:viewController:completion:]**をもう一度呼び出したり、しばらくしてからもう一度試します。
* 既に他のアカウントに連携している場合に発生するエラー
    * エラーコードが**TCGB_ERROR_AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER(3302)**の場合、マッピングしようとしているIdPのアカウントが既に他のアカウントに連携しているという意味です。連携済みのアカウントを解除したい場合、該当するアカウントでログインしてから**[TCGBGamebase withdrawWithViewController:completion:]**を呼び出して退会したり、**[TCGBGamebase removeMappingWithType:viewController:completion:]**を呼び出して連携を解除した後、もう一度マッピングを試みてください。
* 既に同じIdPアカウントに連携されている場合に発生するエラー
	* エラーコードが**TCGB_ERROR_AUTH_ADD_MAPPING_ALREADY_HAS_SAME_IDP(3303)**の場合、マッピングしようとしているIdPと同じ種類のアカウントが既に連携しているという意味です。
	* Gamebaseのマッピングは、IdP一つにつき一つのアカウントのみ連携させることができます。例えば、既にPAYCOアカウントに連携している場合は、これ以上PAYCOアカウントを追加することができません。
	* 同じIdPの他のアカウントを連携させるためには、**[TCGBGamebase removeMappingWithType:viewController:completion:]**を呼び出して連携を解除してからもう一度マッピングを試みてください。
* その他のエラー
    * マッピングに失敗しました。

### Import Header file into View Controller

マッピングを設計するViewControllerに次のヘッダーファイルを持ってきます。

```objectivec
#import <Gamebase/Gamebase.h>
```



### Add Mapping API

特定のIdPにログインされた状態で他のIdPへのマッピングを試みます。<br/>

次は、Facebookにマッピングを試みる例です。

```objectivec
- (void)authAddMapping {
    [TCGBGamebase addMappingWithType:@"facebook" viewController:parentViewController completion:^(TCGBAuthToken *authToken, TCGBError *error) {
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
            }
        }
    }];
}
```

### AddMapping with Credential

ゲームで直接IdPに提供するSDKで、予め認証を行いアクセストークンなどを利用してGamebase AddMappingをすることができるインターフェースです。




* Credentialパラメーターの設定方法



| keyname                                  | a use                          | 値の種類                           |
| ---------------------------------------- | ------------------------------ | ------------------------------ |
| kTCGBAuthLoginWithCredentialProviderNameKeyname | IdPタイプの設定                      | facebook, payco, iosgamecenter, naver, google, twitter, line, appleid |
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


```objectivec
     - (void)onButtonLogin {
         UIViewController* topViewController = nil;
 
         NSString* facebookAccessToken = @"feijla;feij;fdklvda;hfihsdfeuipivaipef/131fcusp";
         NSMutableDictionary* credentialInfo = [NSMutableDictionary dictionary];
         credentialInfo[kTCGBAuthLoginWithCredentialProviderNameKeyname] = @"facebook";
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

次はFacebookに強制マッピングを試行する例です。

```objectivec
- (void)authAddMapping {
    [TCGBGamebase addMappingWithType:@"facebook" viewController:parentViewController completion:^(TCGBAuthToken *authToken, TCGBError *error) {
        if ([TCGBGamebase isSuccessWithError:error] == YES) {
            NSLog(@"AddMapping is succeeded.");
        }
        else if (error.code == TCGB_ERROR_SOCKET_ERROR || error.code == TCGB_ERROR_SOCKET_RESPONSE_TIMEOUT) {
            NSLog(@"Retry addMapping");
        }
        else if (error.code == TCGB_ERROR_AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER) {
            NSLog(@"Already mapped to other member");
            TCGBForcingMappingTicket* ticket = [TCGBForcingMappingTicket forcingMappingTicketWithError:error];
            [TCGBGamebase addMappingForciblyWithType:ticket.idPCode forcingMappingKey:ticket.forcingMappingKey viewController:parentViewController completion:^(TCGBAuthToken *authToken, TCGBError *error) {
                if ([TCGBGamebase isSuccessWithError:error]) {
                    // Mapping success.
                }
                else {
                    // Mapping failed.
                }
            }];
        }
        else {
            NSLog(@"AddMapping Error - %@", [error description]);
        }
    }];
}
```


### Add Mapping Forcibly with Credential
特定IdPにすでにマッピングされているアカウントがある時、**強制的に**マッピングを試行します。
**強制マッピング**を試行する時は、AddMapping APIで取得した`ForcingMappingTicket`が必要です。

ゲームで直接IdPが提供するSDKにより先に認証して発行されたアクセストークンなどを利用して、Gamebase AddMappingForciblyを呼び出すことができるインターフェイスです。

* Credentialパラメータ設定方法


| keyname                                  | a use                          | 値種類                       |
| ---------------------------------------- | ------------------------------ | ------------------------------ |
| kTCGBAuthLoginWithCredentialProviderNameKeyname | IdPタイプ設定                  | facebook, payco, iosgamecenter, naver, google, twitter |
| kTCGBAuthLoginWithCredentialAccessTokenKeyname | IdPログイン後に取得した認証情報(アクセストークン)設定 |                                           |

> [参考]
>
> ゲーム内から外部サービス(Facebookなど)の固有機能を使用する際に、必要な場合があります。
>

<br/>


> <font color="red">[注意]</font><br/>
>
> 外部SDKで要求する開発事項は外部SDKのAPIを使用して実装する必要があり、Gamebaseではサポートしません。
>

次はFacebookに強制マッピングを試行する例です。

```objc
- (void)onButtonLogin {
    UIViewController* topViewController = nil;
    
    NSString* facebookAccessToken = @"feijla;feij;fdklvda;hfihsdfeuipivaipef/131fcusp";
    NSMutableDictionary* credentialInfo = [NSMutableDictionary dictionary];
    credentialInfo[kTCGBAuthLoginWithCredentialProviderNameKeyname] = @"facebook";
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
            TCGBForcingMappingTicket* ticket = [TCGBForcingMappingTicket forcingMappingTicketWithError:error];
            [TCGBGamebase addMappingWithCredential:credentialInfo forcingMappingKey:ticket.forcingMappingKey viewController:topViewController completion:^(TCGBAuthToken *authToken, TCGBError *error) {
                if ([TCGBGamebase isSuccessWithError:error]) {
                    // Mapping success.
                }
                else {
                    // Mapping failed.
                }
            }];
        }
        else {
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
[TCGBGamebase removeMappingWithType:@"facebook" viewController:self completion:^(TCGBError *error) {
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
> 認証情報が必要な場合、"[TCGBGamebase loginForLastLoggedInProvider]"の代わりに使用したいIDPCodeと同じ{IDP_CODE}をパラメーターとし、"[TCGBGamebase loginWithType:IDP_CODE viewController:self completion:completion];"APIでログインしないと認証情報を正常に取得することができません。

### Get Authentication Information for Gamebase
Gamebaseから発行された認証情報を取得することができます。

```objectivec
// Obtaining Gamebase UserID
NSString* gamebaseUserID = [TCGBGamebase userID];

// Obtaining Gamebase AccessToken
NSString* gamebaseAccessToken = [TCGBGamebase accessToken];

// Obtaining Last Logged In Provider
NSString* lastProviderName = [TCGBGamebase lastLoggedInProvider];

// Obtaining Ban Information
TCGBBanInfo* banInfo = [TCGBGamebase banInfo];
```


### Get Authentication Information for External IdP

外部の認証SDKからアクセストークン、ユーザーID、プロファイルなどの情報を取得することができます。

```objectivec
// Example for obtaining ID Provider's Authentication Information

// Obtaining Facebook UserID
NSString *userID = [TCGBGamebase authProviderUserIDWithIDPCode:@"facebook"];

// Obtaining Facebook AccessToken
NSString *accessTokenOfIDP = [TCGBGamebase authProviderAccessTokenWithIDPCode:@"facebook"];

// Obtaining Facebook Profile
TCGBAuthProviderProfile *providerProfile = [TCGBGamebase authProviderProfileWithIDPCode:@"facebook"];
```

### Get Banned User Information

Gamebase Consoleで利用制限対象のゲームユーザーに登録された場合、
ログインを試みると、次のような利用制限情報コードが表示されることがあります。**[TCGBGamebase banInfo]**メソッドを利用して利用制限情報を確認することができます。

* TCGB_ERROR_BANNED_MEMBER




## TransferAccount
ゲストアカウントを他の端末に移行するためのキーを発行する機能です。

このキーを**TransferAccountInfo**と呼びます。
発行されたTransferAccountInfoは、他の端末で**requestTransferAccount**APIを呼び出してアカウントを移行できます。

> `注意`
> TransferAccountInfoは、ゲストログイン状態でのみ発行できます。
> TransferAccountInfoを利用したアカウント移行は、ゲストログイン状態またはログインされていない状態でのみ可能です。
> ログインしたゲストアカウントがすでに他の外部IdP(Google、Facebook、PAYCOなど)アカウントとマッピングされている場合、アカウント移行がサポートされません。

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

> `注意`
> ゲストでログインした状態でアカウントを移行すると、ゲストアカウントは消滅します。

**API**

```objectivec
+ (void)transferAccountWithIdPLoginWithAccountId:(NSString *)accountId accountPassword:(NSString *)accountPassword completion:(void(^)(TCGBAuthToken* authToken, TCGBError* error))completion;
```

**Example**

```objectivec
 - (void)transferOtherDevice {
    [TCGBGamebase transferAccountWithIdPLoginWithAccountId:@"1Aie0198" accountPassword:@"1Aie0199" completion:^(TCGBAuthToken* authToken, TCGBError* error) {
        NSLog(@"Transfered => %@,\nerror => %@", [authToken description], [error description]);
    }];
 }
```


## Error Handling

| Category       | Error                                    | Error Code | Description                              |
| -------------- | ---------------------------------------- | ---------- | ---------------------------------------- |
| Auth           | TCGB\_ERROR\_INVALID\_MEMBER             | 6          | 正しくない会員に対するリクエストです。                        |
|                | TCGB\_ERROR\_BANNED\_MEMBER              | 7          | 利用制限対象の会員です。                               |
|                | TCGB\_ERROR\_AUTH\_USER\_CANCELED        | 3001       | ログインがキャンセルされました。                           |
|                | TCGB\_ERROR\_AUTH\_NOT\_SUPPORTED\_PROVIDER | 3002       | この認証方式には対応しておりません。                      |
|                | TCGB\_ERROR\_AUTH\_NOT\_EXIST\_MEMBER    | 3003       | 退会されているか、存在しない会員です。                    |
|                | TCGB\_ERROR\_AUTH\_EXTERNAL\_LIBRARY\_ERROR | 3009       | 外部認証ライブラリーエラーです。 <br/> DetailCodeおよびDetailMessageを確認してください。 |
| Auth (Login)   | TCGB\_ERROR\_AUTH\_TOKEN\_LOGIN\_FAILED  | 3101       |トークンログインに失敗しました。                         |
|                | TCGB\_ERROR\_AUTH\_TOKEN\_LOGIN\_INVALID\_TOKEN\_INFO | 3102       |トークン情報が有効ではありません。                       |
|                | TCGB\_ERROR\_AUTH\_TOKEN\_LOGIN\_INVALID\_LAST\_LOGGED\_IN\_IDP | 3103       | 最近ログインしたIdPの情報がありません。                  |
| IdP Login      | TCGB\_ERROR\_AUTH\_IDP\_LOGIN\_FAILED    | 3201       | IdPログインに失敗しました。                       |
|                | TCGB\_ERROR\_AUTH\_IDP\_LOGIN\_INVALID\_IDP\_INFO | 3202       | IdP情報が有効ではありません。(Consoleに該当するIdPの情報がありません。) |
| Add Mapping    | TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_FAILED  | 3301       | マッピング追加に失敗しました。                          |
|                | TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_ALREADY\_MAPPED\_TO\_OTHER\_MEMBER | 3302       | 既に他のメンバーにマッピングされています。                     |
|                | TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_ALREADY\_HAS\_SAME\_IDP | 3303       | 既に同じIdPにマッピングされています。                    |
|                | TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_INVALID\_IDP\_INFO | 3304       | IdP情報が有効ではありません。(Consoleに該当するIdPの情報がありません。) |
|                | TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_CANNOT\_ADD\_GUEST\_IDP | 3305  | ゲストIdPではAddMappingができません。 |
| Remove Mapping | TCGB\_ERROR\_AUTH\_REMOVE\_MAPPING\_FAILED | 3401       | マッピング削除に失敗しました。                          |
|                | TCGB\_ERROR\_AUTH\_REMOVE\_MAPPING\_LAST\_MAPPED\_IDP | 3402       | 最後にマッピングされたIdPは、削除することができません。              |
|                | TCGB\_ERROR\_AUTH\_REMOVE\_MAPPING\_LOGGED\_IN\_IDP | 3403       | 現在ログイン中のIdP です。                   |
| Logout         | TCGB\_ERROR\_AUTH\_LOGOUT\_FAILED        | 3501       | ログアウトに失敗しました。                           |
| Withdrawal     | TCGB\_ERROR\_AUTH\_WITHDRAW\_FAILED      | 3601       | 退会に失敗しました。                             |
| Not Playable   | TCGB\_ERROR\_AUTH\_NOT\_PLAYABLE         | 3701       | プレイできない状態です(メンテナンスまたはサービス終了など)。       |
| Auth(Unknown)  | TCGB\_ERROR\_AUTH\_UNKNOWN\_ERROR        | 3999       | 不明なエラーです。(定義されていないエラーです。)            |





* 全体のエラーコードは、次のドキュメントをご参考ください。
    - [エラーコード](./error-code/#client-sdk)


**TCGB\_ERROR\_AUTH\_EXTERNAL\_LIBRARY\_ERROR**
* このエラーは、各IdPのSDKで発生したエラーです。
* エラーコードの確認は、次の通りです。

* IdP SDKのエラーコードは、各Developerページをご参考ください。

```objectivec
TCGBError *tcgbError = error; // TCGBError object via callback
NSError *moduleError = [tcgbError.userInfo objectForKey:NSUnderlyingErrorKey]; // NSError object from external module
NSInteger moduleErrorCode = moduleError.code;
NSString *moduleErrorMessage = moduleError.message;

// If you use **description** method, you can get entire information of this object by JSON Format
NSLog(@"TCGBError:%@", [tcgbError description]);
```

