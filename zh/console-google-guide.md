 Mobile Service > IAP > Google Console Guide

Google Play Billing is a service that lets you sell consumable product or subscription from inside an Android app, or in-app.
Google Play Billing requires some keys created in Google Play Console and Google API Console. 
Also you should set Google Real-time developer notifications for subscription.

## Google Application Key

| Key | Description                                             |
| ---------------------------------- | ---------------------------------------------- |
| Google In App Purchase License Key | App Public KEY(RSA)       |
| Google API Client ID               | OAuth Client ID            |
| Google API Client Secret           | OAuth Client Secret        |
| Refresh Token For Google OAuth     | Refresh Token |

## Google Console
| Console        | URL                              |
| -------------- | ------------------------------- |
| Google Play Console | https://developer.android.com/distribute/console |
| Google API Console | https://console.developers.google.com/apis/dashboard |

## Google Play Console

### Google In App Purchase License Key
```
Google Play Console > select App> (left panel) Development tools > Services & APIs > Licensing & in-app billing
```
![](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap_google_license_en.png)


## Google API Console

* Android Developer Guide
	* [Android Developers - Google Play Billing](http://developer.android.com/google/play/billing/billing_admin.html)
	* [Android Developers - Authorization](https://developers.google.com/identity/protocols/OAuth2WebServer)

### OAuth 2.0 Client ID
```
Create a project in the Google APIs Console with the same account as the Google Play Developer Console.
Use the links below to generate the following information required for OAuth authentication.  
1) Client ID  
2) Client Secret  
3) Refresh Token  
```

##### 1. Create an OAuth client at https://console.developers.google.com/apis/credentials (web application)
![[]](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap_google_credentials_en.png)

##### 2. Enter https://developers.google.com/oauthplayground in the Authorized redirection urls
![[]](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap_google_Oauth_en.png)

##### 3. Copy client ID / client secret from the popup window after creating
![[]](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap_google_Oauth_clientSecret_en.png)

##### 4. [OAuth Playground](https://developers.google.com/oauthplayground/) > oauthplayground Setting > Use your own OAuth credentials
![[]](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap_g_03.png)

##### 5. Enter Authorization code code by entering https://www.googleapis.com/auth/androidpublisher in Step 1
![[]](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap_g_04.png)

##### 6. In Step 2, click the Exchange authorization code for tokens button to issue a token.
![[]](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap_g_05.png)


## Google Play Additional Settings

After creating the OAuth credentials, proceed with the project setup by referring to the guide below.

> [Reference]
> Google Guide : https://developers.google.com/android-publisher/getting_started

#### 1. Google Play Android Developer API should be enabled.
```
  - https://console.developers.google.com > APIs & Services > Dashboard
```
![[]](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap-console-google-console-1.png)


#### 2. Check your project is linked in Google Play Developer Console
```
  - https://play.google.com/apps/publish > Settings > Developer account > API access
```
![[]](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap-console-google-console-2.png)

#### 3. Make sure that the Linked Project in the Google Play Developer Console is the same as the OAuth Client creation project in GoogleAPIs.
![[]](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap_google_linked_en.png)

## Google real-time developer notification 

> [Reference]
> You should use Google Cloud (https://cloud.google.com) Platform. 

### Google Cloud > Pub/Sub

At publication/subscription console, (https://console.cloud.google.com/cloudpubsub) do follwing jobs.
https://cloud.google.com/pubsub/docs/quickstart-console 

#### Create a topic (Products > Pub/Sub)

```
1. After creating topic, move to detail page by clicking option or topic name.
2. Choose publicator by clicking Permissions at topic detail page.
3. Fill google-play-developer-notifications@system.gserviceaccount.com in member form.
4. Click Add button.
```
![[] Topic 만들기](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap_google_createTopic_en.png)
![[] Topic 수정하기](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap_google_addMember_en.png)

#### Add a subscription
```
1. Display the menu for the topic you just created, and click New subscription.
2. Type a name for the subscription, such as MySub.
- Choose a Push Delivery Type
- endpoint url :  https://api-iap.cloud.toast.com/callback/subscription/{YOUR_PACKAGE_NAME}/GG
- {YOUR_PACKAGE_NAME} : google package name
```
![[] Subscription 만들기](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap_google_new_subscirption_en.png)
![[] Subscription 만들기](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap_google_create_subscription_en.png)

#### Verifying IAP domain

See more documents here.(https://cloud.google.com/pubsub/docs/push)

```

1. Go to https://console.cloud.google.com/apis/credentials/domainverification.
2. At [Domain] tab, click [Add domain].
3. Fill https://api-iap.cloud.toast.com in form and [Add domain]
4. Click [TAKE ME THERE] button,  and go to web master center.
5. Click [Add property]
6. At this point, the Google Cloud Platform Console checks your domain against the ones that you’ve verified in Search Console.
 Assuming that you’ve properly verified the domain, the page updates to show your new list of allowed domains.

```
![[] domain verification](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap-console-domain-verification_en_1.png)
![[] domain verification](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap_google_add_domain_en.png)
![[] domain verification](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap-console-domain-verification_en_3.png)
![[] domain verification](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/google_domain_auth.png)
![[] domain verification](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap-console-domain-verification_en_4.png)
![[] domain verification](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap-console-domain-verification_en_5.png)



