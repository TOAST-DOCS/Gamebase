## Game > Gamebase > Unity SDK ご利用ガイド > 決済

ここではUnityでアプリ内決済機能を使用するために必要な設定方法についてご案内いたします。
Gamebaseは、一つの統合された決済APIを提供することで、ゲームで簡単に各ストアのアプリ内決済を連携することができるようサポートします。

> <font color="red">[注意]</font><br/>
>
> 外部パッケージで決済関連処理がある場合、 Gamebase決済機能が正常に動作しない可能性があります。

### Settings

AndroidやiOSでアプリ内決済機能を設定する方法は、次のドキュメントをご参考ください。<br/>

* [Android Purchase Settings](aos-purchase#settings)<br/>
* [iOS Purchase Settings](ios-purchase#settings)

### Purchase Flow

アイテムの購入は大きく分けて決済フロー、消費フロー、再処理フローの3つがあります。
決済フローは、次のような順序で実装してください。

![purchase flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_001_2.10.0.png)

1. 以前の決済が正常に終了せず、再処理が動作しない場合、決済が失敗します。そのため決済前に**RequestItemListOfNotConsumed**を呼び出して再処理を行い、未支給のアイテムがある場合はConsume Flowを進行します。
2. ゲームクライアントではGamebase SDKの**RequestPurchase**を呼び出して決済を試行します。
3. 決済が成功すると**RequestItemListOfNotConsumed**を呼び出して未消費決済履歴を確認した後、支給するアイテムが存在場合、Consume Flowを進行します。

### Consume Flow

未消費決済履歴リストに値がある場合、次のような順序でConsume Flowを進行してください。

> <font color="red">[注意]</font><br/>
>
> アイテムの重複支給が発生しないように、ゲームサーバーで必ず重複支給の有無をチェックしてください。
>

![consume flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_002_2.40.1.png)

1. ゲームクライアントがゲームサーバーに決済アイテムのconsume(消費)をリクエストします。
    * UserID, gamebaseProductId, paymentSeq, purchaseTokenを伝達します。
2. ゲームサーバーは、ゲームDBにすでに同じpaymentSeqでアイテムを支給した履歴があるかを確認します。
    * 2-1. まだアイテムを支給していなければGamebaseサーバーのPayment Transaction APIを呼び出ししてpaymentSeq、purchaseToken値が有効か検証します。
        * [Game > Gamebase > APIガイド > Purchase(IAP) > Get Payment Transaction](./api-guide/#get-payment-transaction)
    * 2-2. purchaseTokenが正常な値の場合はUserIDにgamebaseProductIdに該当するアイテムを支給します。
    * 2-3. アイテム支給後、ゲームDBにUserID、gamebaseProductId、paymentSeq、purchaseTokenを保存して重複支給防止または再支給ができるようにします。
3. アイテム支給有無に関係なく、ゲームサーバーはGamebaseサーバーのconsume(消費) APIを呼び出してアイテムの支給を完了します。
    * [Game > Gamebase > APIガイド > Purchase(IAP) > Consume](./api-guide/#consume)

### Retry Transaction Flow

![retry transaction flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_retry_transaction_flow_2.19.0.png)

* ストア決済には成功したがエラーが発生して正常に終了しなかった場合があります。
* **RequestItemListOfNotConsumed**を呼び出して再処理を行い、未支給のアイテムがある場合、Consume Flowを進行してください。
* 再処理は次の時点で呼び出すことを推奨します。
    * ログイン完了後
    * 決済前
    * ゲーム内ショップ(またはロビー)に移動した時
    * ユーザープロフィールまたはメールボックスを確認した時

### Purchase Item

購入したいアイテムのitemSeqを利用して次のAPIを呼び出し、購入をリクエストします。
ゲームユーザーが購入をキャンセルする場合、**PURCHASE_USER_CANCELED**エラーが返されます。


**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static void RequestPurchase(string gamebaseProductId, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Purchase.PurchasableReceipt> callback)
static void RequestPurchase(string gamebaseProductId, string payload, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Purchase.PurchasableReceipt> callback)
```

**Example**
```cs
public void RequestPurchase(string gamebaseProductId)
{
    Gamebase.Purchase.RequestPurchase(gamebaseProductId, (purchasableReceipt, error) =>
    {
        if (Gamebase.IsSuccess(error))
        {
            Debug.Log("Purchase succeeded.");
        }
        else
        {
        	if (error.code == (int)GamebaseErrorCode.PURCHASE_USER_CANCELED)
            {
                Debug.Log("User canceled purchase.");
            }
            else
            {
            	Debug.Log(string.Format("Purchase failed. error is {0}", error));
            }
        }
    });
}


public void RequestPurchase(string gamebaseProductId)
{
    string userPayload = "{\"description\":\"This is example\",\"channelId\":\"delta\",\"characterId\":\"abc\"}";
    Gamebase.Purchase.RequestPurchase(gamebaseProductId, userPayload, (purchasableReceipt, error) =>
    {
        if (Gamebase.IsSuccess(error))
        {
            Debug.Log("Purchase succeeded.");
            // userPayload value entered when calling API
            string payload = purchasableReceipt.payload
        }
        else
        {
        	if (error.code == (int)GamebaseErrorCode.PURCHASE_USER_CANCELED)
            {
                Debug.Log("User canceled purchase.");
            }
            else
            {
            	Debug.Log(string.Format("Purchase failed. error is {0}", error));
            }
        }
    });
}  
```

**VO**
```cs
public class PurchasableReceipt
{
    /// <summary>
    /// 購入したアイテムの商品IDです。
    /// </summary>
    public string gamebaseProductId;

    /// <summary>
    /// itemSeqで商品を購入するLegacy API用の識別子です。
    /// </summary>
    public long itemSeq;

    /// <summary>
    /// 購入した商品の価格です。
    /// </summary>
    public float price;

    /// <summary>
    /// 通貨コードです。
    /// </summary>
    public string currency;

    /// <summary>
    /// 決済識別子です。
    /// purchaseTokenと一緒にConsumeサーバーAPIを呼び出すのに使用する重要な情報です。
    ///    
    /// 注意：Consume APIは、ゲームサーバーで呼び出してください！
    /// <para/><see href="https://docs.toast.com/en/Game/Gamebase/en/api-guide/#purchase-iap">Consume API</see>
    /// </summary>
    public string paymentSeq;

    /// <summary>
    /// 決済識別子です。
    /// paymentSeqと一緒にConsumeサーバーAPIを呼び出すために使用する重要な情報です。
    /// Consume APIでは「accessToken」という名前のパラメータで渡す必要があります。
    ///    
    /// 注意：Consume APIは、ゲームサーバーで呼び出してください！
    /// <para/><see href="https://docs.toast.com/en/Game/Gamebase/en/api-guide/#purchase-iap">Consume API</see>
    /// </summary>
    public string purchaseToken;

    /// <summary>
    /// Google、Appleのようにストアコンソールに登録された商品IDです。
    /// </summary>
    public string marketItemId;

    /// <summary>
    /// 商品タイプです。次の値を使用できます。
    /// * UNKNOWN：認識できないタイプ。Gamebase SDKをアップデートするか、Gamebaseサポートへお問い合わせください。
    /// * CONSUMABLE：消費性商品。
    /// * AUTO_RENEWABLE：購読型商品。
    /// * CONSUMABLE_AUTO_RENEWABLE：購読型商品を購入したユーザーに、定期的に消費が可能な商品を支給したい場合に使われる「消費が可能な購読商品」。
    /// <para/><see cref="GamebasePurchase.ProductType"/>
    /// </summary>
    public string productType;

    /// <summary>
    /// 商品を購入したUser ID。
    /// 商品を購入していないUser IDでログインした場合、購入したアイテムを獲得できません。
    /// </summary>
    public string userId;

    /// <summary>
    /// ストアの決済識別子です。
    /// </summary>
    public string paymentId;

    /// <summary>
    /// 購読商品は更新されるごとにpaymentIdが変更されます。
    /// このフィールドは最初に購読商品を決済した時のpaymentIdを伝えます。
    /// ストアによっては、決済サーバーの状態に応じた値が存在しない場合があるため
    /// 常に有効な値を保障するわけではありません。
    /// </summary>
    public string originalPaymentId;

    /// <summary>
    /// 商品を購入した時刻です。(epoch time)
    /// </summary>
    public long purchaseTime;

    /// <summary>
    /// 購読が終了する時刻です。(epoch time)
    /// </summary>
    public long expiryTime;

    /// <summary>
    /// 決済したストアコードです。
    /// GamebaseStoreCodeクラスでストアコードリストを確認できます。
    /// </summary>
    public string storeCode;

    /// <summary>
    /// Gamebase.Purchase.requestPurchase API呼び出し時にpayloadで渡された値です。
    ///
    /// スストアサーバーの状態によっては情報が失われる場合があるため、使用を推奨しません。
    /// </summary>
    public string payload;

    /// <summary>
    /// プロモーション決済かどうか
    /// - (Android) Gamebase決済サーバーで一時的に検証ロジックをオフにする場合にはfalseのみ出力されるため、常に有効な値が保障されません。
    /// </summary>
    public bool isPromotion;
    
    /// <summary>
    /// テスト決済かどうか
    /// - (Android) Gamebase決済サーバーで一時的に検証ロジックをオフにする場合にはfalseのみ出力されるため、常に有効な値が保障されません。
    /// </summary>
    public bool isTestPurchase;
}
```

### List Purchasable Items

アイテムリストを照会したい場合、次のAPIを呼び出します。
コールバックで返されるリストの中にはそれぞれ各アイテムの情報が含まれています。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static void RequestItemListPurchasable(GamebaseCallback.GamebaseDelegate<List<GamebaseResponse.Purchase.PurchasableItem>> callback)
```

**Example**
```cs
public void RequestItemListPurchasable()
{
    Gamebase.Purchase.RequestItemListPurchasable((purchasableItemList, error) =>
    {
        if (Gamebase.IsSuccess(error))
        {
            Debug.Log("Get list succeeded.");
        }
        else
        {
            Debug.Log(string.Format("Get list failed. error is {0}", error));
        }
    });
}
```

**VO**
```cs
public class PurchasableItem
{
    /// <summary>
    /// Gamebaseコンソールに登録された商品IDです。
    /// Gamebase.Purchase.requestPurchase APIで商品を購入する時に使用されます。
    /// </summary>
    public string gamebaseProductId;

    /// <summary>
    /// itemSeqで商品を購入するLegacy API用の識別子です。
    /// </summary>
    public long itemSeq;

    /// <summary>
    /// 商品の価格です。
    /// </summary>
    public float price;

    /// <summary>
    /// 通貨コードです。
    /// </summary>
    public string currency;

    /// <summary>
    /// Gamebaseコンソールに登録されている商品名です。
    /// </summary>
    public string itemName;

    /// <summary>
    /// Google、Appleのようにストアコンソールに登録された商品IDです。
    /// </summary>
    public string marketItemId;

    /// <summary>
    /// 商品タイプです。次の値を使用できます。
    /// * UNKNOWN：認識できないタイプ。Gamebase SDKをアップデートするか、Gamebaseサポートへお問い合わせください。
    /// * CONSUMABLE：消費性商品。
    /// * AUTORENEWABLE：購読型商品。
    /// * CONSUMABLE_AUTO_RENEWABLE：購読型商品を購入したユーザーに、定期的に消費が可能な商品を支給したい場合に使われる「消費が可能な購読商品」。
    /// <para/><see cref="GamebasePurchase.ProductType"/>
    /// </summary>
    public string productType;

    /// <summary>
    /// 通貨記号が含まれるローカライズされた価格情報です。
    /// </summary>
    public string localizedPrice;

    /// <summary>
    /// ストアコンソールに登録されているローカライズされた商品名です。
    /// </summary>
    public string localizedTitle;

    /// <summary>
    /// ストアコンソールに登録されているローカライズされた商品説明です。
    /// </summary>
    public string localizedDescription;

    /// <summary>
    /// Gamebaseコンソールで該当商品の「使用有無」を表します。
    /// </summary>
    public bool isActive;
}
```

### List Non-Consumed Items

アイテムを購入したが、正常にアイテムが消費(配送、支給)されなかった未消費決済履歴をリクエストします。
未消費決済履歴があある場合はゲームサーバー(アイテムサーバー)にリクエストして、アイテムを配送(支給)するように処理する必要があります。
正常に決済が完了しなかった場合、再処理の役割も担うため、次の状況で呼び出してください。

* ゲームユーザーに支給されなかったアイテムが残っているかを確認
    * ログイン完了後
    * ゲーム内ショップ(またはロビー)に移動した時
    * ユーザープロフィールまたはメールボックスを確認した時
* 再処理が必要なアイテムがあるかを確認
    * 決済前
    * 決済失敗後

**GamebaseRequest.Purchase.PurchasableConfiguration**

| API                             | Mandatory(M) / Optional(O) | Description                                                                    |
| ------------------------------- | -------------------------- | ------------------------------------------------------------------------------ |
| allStores                       | O                          | 同じUserIDで他のストアにて購入した未消費履歴も変換します。<br/>デフォルト値は**false**です。 |

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static void RequestItemListOfNotConsumed(GamebaseRequest.Purchase.PurchasableConfiguration configuration, GamebaseCallback.GamebaseDelegate<List<GamebaseResponse.Purchase.PurchasableReceipt>> callback)
```

**Example**
```cs
public void RequestItemListOfNotConsumedSample(bool allStores)
{
    var configuration = new GamebaseRequest.Purchase.PurchasableConfiguration
    {
        allStores = allStores
    };
    Gamebase.Purchase.RequestItemListOfNotConsumed(configuration, (purchasableReceiptList, error) =>
    {
        if (Gamebase.IsSuccess(error))
        {
            Debug.Log("Get list succeeded.");

            // Should Deal With This non-consumed Items.
            // Send this item list to the game(item) server for consuming item.
        }
        else
        {
            Debug.Log(string.Format("RequestItemListOfNotConsumed failed. error is {0}", error));
        }
    });
}
```

### List Actived Subscriptions

現在のユーザーIDで有効になっている定期購入リストを照会します。
決済が完了した定期購入商品(自動更新型定期購入、自動更新型消費性定期購入商品)は、期間が終了するまで照会できます。 

> <font color="red">[注意]</font><br/>
>
> 現在AndroidではGoogle Playストアでのみサブスクリプション商品をサポートしています。

**GamebaseRequest.Purchase.PurchasableConfiguration**

| API                             | Mandatory(M) / Optional(O) | Description                                                                    |
| ------------------------------- | -------------------------- | ------------------------------------------------------------------------------ |
| allStores                       | O                          | 同じUserIDで他のストアにて購入した未消費履歴も変換します。<br/>デフォルト値は**false**です。 |

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static void RequestActivatedPurchases(GamebaseRequest.Purchase.PurchasableConfiguration configuration, GamebaseCallback.GamebaseDelegate<List<GamebaseResponse.Purchase.PurchasableReceipt>> callback)
```

**Example**
```cs
public void RequestActivatedPurchasesSample(bool allStores)
{
    var configuration = new GamebaseRequest.Purchase.PurchasableConfiguration
    {
        allStores = allStores
    };
    
    Gamebase.Purchase.RequestActivatedPurchases(configuration, (purchasableReceiptList, error) =>
    {
        if (Gamebase.IsSuccess(error) == true)
        {
            Debug.Log("RequestItemListPurchasable succeeded");

            foreach (GamebaseResponse.Purchase.PurchasableReceipt purchasableReceipt in purchasableReceiptList)
            {
                var message = new StringBuilder();
                message.AppendLine(string.Format("gamebaseProductId:{0}", purchasableReceipt.gamebaseProductId));
                message.AppendLine(string.Format("price:{0}", purchasableReceipt.price));
                message.AppendLine(string.Format("currency:{0}", purchasableReceipt.currency));
                
                // You will need paymentSeq and purchaseToken when calling the Consume API.
                // Refer to the following document for the Consume API.
                // https://docs.toast.com/en/Game/Gamebase/en/api-guide/#purchaseiap
                message.AppendLine(string.Format("paymentSeq:{0}", purchasableReceipt.paymentSeq));
                message.AppendLine(string.Format("purchaseToken:{0}", purchasableReceipt.purchaseToken));
                message.AppendLine(string.Format("marketItemId:{0}", purchasableReceipt.marketItemId));
                Debug.Log(message);
            }
        }
        else
        {
            // Check the error code and handle the error appropriately.
            Debug.Log(string.Format("RequestActivatedPurchases failed. error is {0}", error));
        }
    });
}
```

### List Subscriptions status

現在のユーザーID基準でサブスクリプション商品の状態を照会します。
コールバックで返されるリストにはサブスクリプション商品の情報が含まれています。

> <font color="red">[注意]</font><br/>
>
> * 以下のガイドに従って購読イベントを設定すると、購読ステータスコードが正常に返されます。
>     * [Game > Gamebase > ストアコンソールガイド > Googleコンソールガイド > Googleシステム内リアルタイム購読情報イベント配信設定](./console-google-guide/#google_1)
>     * イベント設定を行っていない状態で購入したサブスクリプション商品のステータスコードは常に0(PURCHASED)が返されます。
> * 現在、サブスクリプション商品はGoogle Playストアのみサポートします。


**GamebaseRequest.Purchase.PurchasableConfiguration**

| API                             | Mandatory(M) / Optional(O) | Description                                                 |
| ------------------------------- | -------------------------- | ----------------------------------------------------------- |
|  includeExpiredSubscriptions    | O                          | 期限切れのサブスクリプション商品まで含めて照会します。<br/>デフォルト値は**false**です。   |

**API**

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static void RequestSubscriptionsStatus(GamebaseRequest.Purchase.PurchasableConfiguration configuration, GamebaseCallback.GamebaseDelegate<List<GamebaseResponse.Purchase.PurchasableReceipt>> callback)
```

**Example**
```cs
public void RequestSubscriptionsStatusSample(bool includeExpiredSubscriptions)
{
    var configuration = new GamebaseRequest.Purchase.PurchasableConfiguration
    {
        includeExpiredSubscriptions = includeExpiredSubscriptions
    };
    Gamebase.Purchase.RequestSubscriptionsStatus(configuration, (subscriptionStatusList, error) =>
    {
        if (Gamebase.IsSuccess(error) == true)
        {
            Debug.Log("RequestSubscriptionsStatus succeeded");

            foreach (GamebaseResponse.Purchase.PurchasableSubscriptionStatus subscriptionStatus in subscriptionStatusList)
            {
                var message = new StringBuilder();
                message.AppendLine(string.Format("storeCode:{0}", subscriptionStatus.storeCode));
                message.AppendLine(string.Format("itemSeq:{0}", subscriptionStatus.itemSeq));
                message.AppendLine(string.Format("price:{0}", subscriptionStatus.price));

                // Subscription status
                // Refer to the following document for the entire status code.
                // https://docs.nhncloud.com/en/TOAST/en/toast-sdk/iap-unity/#iapsubscriptionstatusstatus
                message.AppendLine(string.Format("statusCode:{0}", subscriptionStatus.statusCode));
                message.AppendLine(string.Format("gamebaseProductId:{0}", subscriptionStatus.gamebaseProductId));
                Debug.Log(message);
            }
        }
        else
        {
            // Check the error code and handle the error appropriately.
            Debug.Log(string.Format("RequestSubscriptionsStatus failed. error is {0}", error));
        }
    });
}
```

**VO**
```cs
public class PurchasableSubscriptionStatus
{
    /// <summary>
    /// アプリがインストールされたストアに対してGamebaseで内部的に定義したコードです。
    /// </summary>
    public string storeCode;

    /// <summary>
    /// ストアの決済識別子です。
    /// </summary>
    public string paymentId;

    /// <summary>
    /// サブスクリプション商品は更新されるたびにpaymentIdが変更されます。
    /// このフィールドはサブスクリプション商品を初めて決済したときのpaymentIdを示します。
    /// ストアや決済サーバーの状態によっては値が存在しない場合があるため
    /// 常に有効な値を保障するわけではありません。
    /// </summary>
    public string originalPaymentId;

    /// 決済識別子です。
    /// purchaseTokenと一緒に「Consume」サーバーAPIを呼び出すために使用する重要な情報です。
    ///    
    /// 注意：Consume APIはゲームサーバーで呼び出してください！
    /// <para/><see href="https://docs.toast.com/en/Game/Gamebase/en/api-guide/#purchase-iap">Consume API</see>
    public string paymentSeq;

    /// <summary>
    /// 購入した商品の商品IDです。
    /// </summary>
    public string marketItemId;

    /// <summary>
    /// IAP Webコンソールの項目固有識別子
    /// </summary>
    public long itemSeq;

    /// <summary>
    /// 次の値のいずれかを持ちます。
    /// * UNKNOWN：不明なタイプです。 Gamebase SDKをアップデートするか、Gamebaseサポートにお問い合わせください。
    /// * CONSUMABLE：消耗品です。
    /// * AUTO_RENEWABLE：サブスクリプション商品です。
    /// </summary>
    public string productType;

    /// <summary>
    /// 商品を購入したユーザーIDです。
    /// 商品の購入に使用していないユーザーIDでログインすると購入した商品を受け取れません。
    /// </summary>
    public string userId;

    /// <summary>
    /// 商品の価格です。
    /// </summary>
    public float price;

    /// <summary>
    /// 通貨情報です。
    /// </summary>
    public string currency;

    /// <summary>
    /// Payment識別子。
    /// paymentSeqで「Consume」サーバーAPIを呼び出すために使用される重要な情報です。
    /// Consume APIで引数名を「accessToken」に指定すると渡されます。
    ///
    /// <para/><see href="https://docs.toast.com/ko/Game/Gamebase/ko/api-guide/#purchase-iap">Purchase IAP</see>
    /// </summary>
    public string purchaseToken;

    /// <summary>
    /// この値はGoogleで購入する時に使用され、次の値を持つことができます。
    /// ただし、GoogleサーバーのエラーによりGamebase決済サーバーで一時的に認証ロジックが無効になっている場合、
    /// nullが返されるため常に有効な値を保障するわけではありません。
    /// * null：正常決済
    /// * テスト：テスト決済
    /// * プロモーション：プロモーション決済
    /// </summary>
    public string purchaseType;

    /// <summary>
    /// 商品を購入した時間。(epoch time)
    /// </summary>
    public long purchaseTime;

    /// <summary>
    /// 購読の期限が切れる時間。(epoch time)
    /// </summary>
    public long expiryTime;

    /// <summary>
    /// Gamebase.Purchase.requestPurchase API呼び出し時にペイロードに渡される値です。
    ///
    /// ストアサーバー状態によって情報が流出する場合があるため、使用を推奨しません。
    /// </summary>
    public string payload;

    /// <summary>
    /// 購読状態
    /// 全体ステータスコードは次の文書を参照してください。
    /// <para/><see href="https://docs.nhncloud.com/en/TOAST/en/toast-sdk/iap-unity/#iapsubscriptionstatus">IAP Subscription Status</see>
    /// </summary>
    public int statusCode;

    /// <summary>
    /// 購読状態の説明です。
    /// </summary>
    public string statusDescription;

    /// <summary>
    /// Gamebaseコンソールに登録された商品IDです。
    /// Gamebase.Purchase.requestPurchase APIで商品を購入する時に使用されます。
    /// </summary>
    public string gamebaseProductId;
}
```

### Event by Promotion

プロモーション決済が完了すると、GamebaseEventHandlerを通してイベントを取得して処理できます。
GamebaseEventHandlerでプロモーション決済イベントを処理する方法は、下記のガイドを参照してください。
[Game > Gamebase > Unity SDK使用ガイド > ETC > Gamebase Event Handler](./unity-etc/#purchase-updated)

Supported Platforms
<span style="color:#1D76DB; font-size：10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size：10pt">■</span> UNITY_ANDROID

> <font color="red">[注意]</font><br/>
>
> iOSプロモーション決済を行うには、必ず下記のガイドに沿って設定してください。
> [Game > Gamebase > iOS SDK使用ガイド > 決済 > Event by Promotion](./ios-purchase/#event-by-promotion)

### Error Handling

| Error                                     | Error Code | Description                              |
| ----------------------------------------- | ---------- | ---------------------------------------- |
| PURCHASE_NOT_INITIALIZED                  | 4001       | Purchaseモジュールが初期化されていません。<br>gamebase-adapter-purchase-IAPモジュールをプロジェクトに追加したか確認してください。|
| PURCHASE_USER_CANCELED                    | 4002       | ゲームユーザーがアイテムの購入をキャンセルしました。                 |
| PURCHASE_NOT_FINISHED_PREVIOUS_PURCHASING | 4003       | 購入ロジックが完了していない状態でAPIが呼び出されました。|
| PURCHASE_NOT_ENOUGH_CASH                  | 4004       | 該当するストアのcashが足りないため決済することができません。            |
| PURCHASE_INACTIVE_PRODUCT_ID              | 4005       | 該当商品が有効な状態ではありません。  |
| PURCHASE_NOT_EXIST_PRODUCT_ID             | 4006       | 存在しないGamebaseProductIDで決済をリクエストしました。 |
| PURCHASE_LIMIT_EXCEEDED                   | 4007       | 月の購入限度を超過しました。             |
| PURCHASE_NOT_SUPPORTED_MARKET             | 4010       | このストアには対応しておりません。<br>選択可能なストアは、GG(Google)、TS(ONE store)、GALAXY、AMAZON、HUAWEI、MYCARDです。 |
| PURCHASE_EXTERNAL_LIBRARY_ERROR           | 4201       | NHN Cloud IAPライブラリエラーです。<br/>詳細エラーを確認してください。 |
| PURCHASE_UNKNOWN_ERROR                    | 4999       | 定義されていない購入エラーです。<br>ログ全体を[カスタマーセンター](https://toast.com/support/inquiry)にアップロードしてください。なるべく早くお答えいたします。|

* 全体のエラーコードは、次のドキュメントをご参考ください。
    * [エラーコード](./error-code/#client-sdk)

**PURCHASE_EXTERNAL_LIBRARY_ERROR**

* このエラーはNHN Cloud IAPライブラリでエラーが発生した時に返されます。
* NHN Cloud IAPライブラリで発生したエラー情報は詳細エラーに含まれており、詳細なエラーコードおよびメッセージは次のように確認できます。 

```cs
GamebaseError gamebaseError = error; // GamebaseError object via callback

if (Gamebase.IsSuccess(gamebaseError))
{
    // succeeded
}
else
{
    Debug.Log(string.Format("code:{0}, message:{1}", gamebaseError.code, gamebaseError.message));

    GamebaseError moduleError = gamebaseError.error; // GamebaseError.error object from external module
    if (null != moduleError)
    {
        int moduleErrorCode = moduleError.code;
        string moduleErrorMessage = moduleError.message;

        Debug.Log(string.Format("moduleErrorCode:{0}, moduleErrorMessage:{1}", moduleErrorCode, moduleErrorMessage));
    }
}
```

* NHN Cloud IAPのエラーコードは、次のドキュメントをご参考ください。
    * [NHN Cloud > NHN Cloud SDK使用ガイド > NHN Cloud IAP > Unity > エラーコード](https://docs.toast.com/en/TOAST/en/toast-sdk/iap-unity/#error-code)
