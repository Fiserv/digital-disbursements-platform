# What is Hosted Pages

<img title="icon" alt="hosted pages icon" src="/assets/images/Picture25.png" width="40" height="30"> 

Hosted payment pages is a Fiserv offering for secure card collection via webview or iFrame in order to avoid PCI Compliance exposure. We have a native API (client to server), server to server, and Cross Origin Resource (CORS) APIs to provide our clients a variety of integration options.

The goal of this guide is to describe how to implement and use Hosted Pages. It is not intended to describe all the interactions between App, Mobile Application Server, and Connected Commerce (uCom) Server.

## Components  

Hosted Pages has 4 main Components, the Mobile Application, the Mobile Application Server, Hosted Page and the Connected Commerce (uCom) Sever/API.  

<center><img src="assets/images/HostedPages (1).jpeg" alt="HP Diagram" class="center"></center>

1. **Mobile Application (App)** - This could be a native mobile application running on IOS or Android devices. This could also be a web application running on a browser.

2. **Mobile Application Server (MAS)** - This is a server which is a communication bridge between the App and Connected Commerce (uCom). It also stores Connected Commerce (uCom) developer account information like app id, app secret, and encryption key securely.

3. **Hosted Page (HP)** - This is a web page rendered on native webview or browser which will display the UI to let user enter their account information like Credit card, debit card, gift card etc. This page securely captures the account information from user and passes it to Connected Commerce (uCom) server. When finished, the page sends the result to redirectUrl.

4. **Connected Commerce (uCom) Server** - This is the Fiserv solution server which provides all the APIs. This server sits behind Apigee.

## Integration Prerequisites

The following parameters are needed to access the Hosted Pages:

| Parameter| Description|
|---|---|
| **Api Key**  | This will be generated for you when you create an Apigee account. This should be saved securely on MAS and shared with App. Api-key is fixed for a merchant.|
| **Api Secret**  | This will be generated for you when you create an Apigee account. This should be saved securely on the application server. This should NOT be shared with app. Api-key is fixed for a merchant. |
| **Redirect Url/MAS Url (Asynchronous)** | All the Hosted pages responses(error/success) will be responded back to JavaScript main return callback only. Responses should be parsed and handled from JavaScript callback. Additionally same responses will be delivered to your MAS URL by Hosted pages via HTTP POST (Ajax Call) asynchronously. This API should be provided by MAS. MAS has to enable CORS for Fiserv origin “int.api.firstdata.com", "cat.api.firstdata.com", and "prod.api.firstdata.com”. This can be used for auditing purposes when a web browser or an app crash accidentally.|
| **FDCustomerId**  | This must be obtained using other Connected Commerce (uCom) apis. This is optional when you initiate SDK with guest checkout option. |
| **PageLink (url and relation)** | This is the unique page which is going to display the use case. Url is the address where page is hosted, and Relation is the name of the use case. PageLink can be retrieved run time via the api (ucom/v1/hosted - pages/pages) and can be cached. We prefer that PageLink should be freshly fetched. The page contents are configured offline. |

## Steps to Integrate Hosted Pages

### Architecture Flow

<center><img src="/assets/images/HostedPages (2).png" alt="HP Diagram" class="center"></center>

### Step 1: Start a New Session  

App calls MAS to get a token ID, encryption Key and page Link. token ID and encryption Key should not be cached or stored on the app and should be fetched from MAS. The token ID and encryption Key expires after 20 minutes and therefore this step should be done every time user starts the flow.

<center><img src="/assets/images/HostedPages (3).png" alt="HP Diagram" class="center"></center>

### Step 2: App calls MAS

The api between app and MAS is not part of this document. It’s up to the merchant to decide this part of the transaction.

### Step 3: MAS calls Connected Commerce (uCom) to Get Token

MAS has to call Connected Commerce (uCom) to get a tokenId. MAS should not cache the tokenId. The get Token call will provide the one time session token and public key which needs to be passed to the SDK to launch Hosted Pages.

[Click here for instructions on Creating Security Access Token](../../ConnectedCommerce/api/?type=post&path=/v1/tokens)

### Step 4: MAS calls Connected Commerce (uCom) to get page link

MAS can cache the page link for future reference to ensure better performance. Merchant may have configured multiple pages and therefore this api will return all of them. Each page can be identified by the relation.

**Endpoint URL**
<https://int.api.firstdata.com/ucom/v1/hosted-pages/pages>

HTTP Method: GET

**Headers**
Content-Type = application/json

Authorization = Bearer {{tokenId}}

Api-Key = {api-key}

Timestamp = {time UTC in milliseconds}

Sample Response:

```json

{
    "_links": [
        {
            "href": "https://int.api.firstdata.com/ucom/v1/hostedpages/pages/f74237ec-112a-4204-a37b-2cfe9daa904f",
            "rel": " account.add",
            "method": "GET",
            "version": "v2"
        }
    ]
}

```

### Step 5: Load Hosted Page

There are two ways to load hosted Pages: Webview  or iFrame.To start the hosted page, the app needs the api-key, pageLink, tokenId, fdCustomerId, encryptionKey and redirectUrl. Please ensure that the Mobile app has disabled webview caching and enabled loading javascript in webview.

<!--
type: tab
titles: WebView, iFrame 
-->

#### Mobile Webview Integration Steps

<details>
<summary><b>A) Load the URL in WebView.</b></summary>

CAT: <https://int.api.firstdata.com/ucom/v2/static/v2/mobile/int/ucom-sdk.html>

PRE-PROD: <https://cat.api.firstdata.com/ucom/v2/static/v2/mobile/cat/ucomsdk.html>

PROD: <https://prod.api.firstdata.com/ucom/v2/static/v2/mobile/prod/ucomsdk.html>

</details>

<details>
<summary><b>B) Call ucomClient.init() javascript method after WebView is loaded with configuration objects.</b></summary>

```javascript

ComClient.init({
   "accessToken": "<token-id>>",
   "apiKey": "<API-key>",
   //If guest checkout then do not pass fdCustomerId 
   "fdCustomerId": "<fdCustomerId>",
   //Get it from pageLink 
   "pageId": "<page-id>>",
   "encryptionKey": "<Encryption-Key>",
   "redirectUrl": "<MAS-URL>"
});

```

>Note: Please refer to the **SDK Configuration Property Values** section for additional params.

</details>

<details>
<summary><b>C) Set local redirection listener.<b/></summary>

A redirection listener should be set on the webview to catch the event that Hosted Pages has finished its work. Hosted Pages will call this redirection in case of permanent failures and final success(nonce). A permanent failure is if js fails to load or Ajax call fails or tokenId has expired or encryptionKey invalid.

Once Hosted Pages get response from Connected Commerce (uCom) then it will do URL redirect with encoded URI.

**Redirection Listener URL:** ucom://finish?response=response-payload-object-string

>***Note:** Webview redirection listener URL should be decoded before handle it.*

##### IOS Sample Code Snippets

```code

 #Load HP static URL in webview 
 - (void) successfulMASRequestWithResponseDictionary:(NSDictionary *)response { 
 // Step 2 - Load Hosted Pages 
 NSString *tokenId = response[@"tokenId"]; 
 NSString *apiKey = response[@"apiKey"]; 
 NSString *fdCustomerId = response[@"fdCustomerId"]; 
 NSString *url = response[@"href"]; 
 NSString *requestURL = [NSString stringWithFormat:@"https://%@.api.firstdata.com/u
com/v2/static/v2/ucom-sdk.html", MAS.masSharedManager.environment]; 
 hostedPageRequest = [[NSMutableURLRequest alloc] initWithURL:[NSURL URLWithString:
requestURL]]; 
 // Load 
 dispatch_async(dispatch_get_main_queue(), ^{ 
 [self->_webView loadRequest:hostedPageRequest]; 
 [self->_webView reloadFromOrigin]; 
 }); 
 } 

 #Once webview loaded then pass configuration object and call HP javascript method 
 - (void) webView:(WKWebView *)webView didFinishNavigation:(WKNavigation *)navigatio
n { 
 MAS *sharedInstance = [MAS masSharedManager]; 
 NSString *execTemplate = 
 @"uComClient.init({ 
 \"apiKey\":\"%@\", 
 \"redirectUrl\":\"%@\", 
 \"fdCustomerId\":\"%@\", 
 \"pageId\":\"%@\", 
 \"accessToken\":\"%@\", 
 \"encryptionKey\":\"%@\", 
 \"env\":\"%@\"});"; 
 NSString *exec = [NSString stringWithFormat:execTemplate,sharedInstance.apiKey,
_redirectUrl, 
 sharedInstance.fdCustomerId, sharedInstance.pageId, sharedInstance.bearer, 
 sharedInstance.encryptionKey, sharedInstance.environment]; 
 [_webView evaluateJavaScript:exec completionHandler:nil]; 
 if ([_activityIndicatorView isAnimating]) { 
 [_activityIndicatorView stopAnimating]; 
 } 
 } 

 #pragma mark WKNavigationDelegate methods (Redirection Listener Methods) 
 - (void) webView:(WKWebView *)webView decidePolicyForNavigationAction:(WKNavigation
Action *)navigationAction decisionHandler:(void (^)(WKNavigationActionPolicy))decisionH
andler { 
 //Step 4 - Handle finish redirection 
 NSString *requestURL = navigationAction.request.URL.absoluteString; 
 NSLog(@"Redirect has occurred ..."); 
 NSLog(@"Request Url: %@", requestURL); 
 if ([requestURL hasPrefix:@"ucom://finish"]) { 
 // This is the clue that Hosted Posted is finished 
 NSLog(@" --------------------------------------------------------------
"); 
 NSLog(@"| REQUEST URL: %@", requestURL); 
 NSLog(@" --------------------------------------------------------------
"); 
 decisionHandler(WKNavigationActionPolicyCancel); 
 NSString *resultStr = [[requestURL componentsSeparatedByString:@"ucom://fin
ish?response="].lastObject stringByRemovingPercentEncoding]; 
 UIAlertController *alertController = [UIAlertController alertControllerWith
Title:@"Result" message:resultStr preferredStyle:UIAlertControllerStyleAlert]; 
 UIAlertAction *okButton = [UIAlertAction actionWithTitle:@"OK" style:UIAler
tActionStyleDefault handler:^(UIAlertAction * _Nonnull action) { 
 [self.navigationController popViewControllerAnimated:YES]; 
 }]; 
 [alertController addAction:okButton]; 
 [self presentViewController:alertController animated:YES completion:nil]; 
 } else { 
 // Otherwise don't intercept 
 decisionHandler(WKNavigationActionPolicyAllow); 
 } } 


```

##### Android Sample Code Snippets**

```code

@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_web);
    // Load Hosted Pages
    String url = "";
    url =
        String.format("https://%s.api.firstdata.com/ucom/v2/static/v2/mobile/%s/ucomsdk.html", getEnvironmentString(env), getEnvironmentString(env));
    FDLogger.INSTANCE.d(TAG, url);
    HashMap < String, String > headers = new HashMap < > ();
    headers.put(Constants.Header.API_KEY, apiKey);
    headers.put(Constants.Header.AUTHORIZATION, Constants.Header.BEARER +
        tokenId);
    headers.put(Constants.Header.TIMESTAMP, String.valueOf(new Date().getTime()));
    FDLogger.INSTANCE.map(TAG, headers);
    webView.loadUrl(url, headers);
    webView.setWebViewClient(new WebViewClient() {
        @Override
        public void onPageFinished(WebView view, String url) {
            super.onPageFinished(view, url);
            progressBar.setVisibility(View.GONE);
            String javascript = "";
            //Initialize the SDK with SDK configuration params by calling this 
            ucomSDK.init() method and pass required parameters
            javascript = String.format(
                "uComClient.init({" +
                "\"%s\":\"%s\"," +
                "\"%s\":\"%s\"," +
                "\"%s\":\"%s\"," +
                "\"%s\":\"%s\"," +
                "\"%s\":\"%s\"," +
                "\"%s\":\"%s\"," +
                "\"%s\":\"%s\"" +
                "});",
                Constants.BundleKey.API_KEY, apiKey,
                Constants.BundleKey.REDIRECT_URL, redirectUrl,
                Constants.BundleKey.FD_CUSTOMER_ID, fdCustomerId,
                Constants.BundleKey.PAGE_ID, pageId,
                Constants.BundleKey.ACCESS_TOKEN, tokenId,
                Constants.BundleKey.ENCRYPTION_KEY, encryptionKey,
                Constants.BundleKey.ENV, getEnvironmentString(env));
            FDLogger.INSTANCE.d(TAG, javascript);
            webView.evaluateJavascript(javascript, null);
        }
        @RequiresApi(api = Build.VERSION_CODES.LOLLIPOP)
        @Override
        public boolean shouldOverrideUrlLoading(WebView view, WebResourceRequest request) {
            Uri uri = request.getUrl();
            FDLogger.INSTANCE.d(TAG, uri.getScheme());
            FDLogger.INSTANCE.d(TAG, uri.getPath());
            // Handle finish redirection
            String resultString = request.getUrl().toString();
            String decodedstr = null;
            if (resultString.startsWith("ucom://finish")) {
                // This is the clue that Hosted Posted is finished
                Log.i(TAG, "REQUEST URL: " + resultString);
                try {
                    decodedstr = URLDecoder.decode(resultString, "UTF-8");
                } catch (UnsupportedEncodingException e) {
                    e.printStackTrace();
                }
                Log.i(TAG, "Decoded String: " + decodedstr);
                if (!resultString.startsWith("ucom://finish?result=failure")) {
                    finish();
                    return true;
                }
            } else {
                // Otherwise don't intercept
                Log.i(TAG, "REQUEST URL: " + resultString);
            }
            return super.shouldOverrideUrlLoading(view, request);
        }
    });
}

```

</details>
<!--
type: tab
-->

#### Website Integration Steps

<details>

<summary><b>A) Include the Connected Commerce (uCom) SDK library below on head tag of the html page.</b></summary>

CAT: <https://int.api.firstdata.com/ucom/v2/static/v2/js/ucom-sdk.js>  

PRE-PROD: <https://cat.api.firstdata.com/ucom/v2/static/v2/js/ucom-sdk.js>

PROD: <https://prod.api.firstdata.com/ucom/v2/static/v2/js/ucom-sdk.js>

</details>

<details>
<summary><b>B) Initialize the SDK with SDK configuration params by calling this ucomSDK.init() method.</b></summary>
    1. Pass access token
    2. Pass API Key
    3. Pass fdCustomerId
    4: Pass page URL
    5: Pass Mount Id where SDK needs to be mounted on the screen
    6. Pass EncryptionKey
    7. Pass RedirectURL
</details>

<details>

<summary><b>C) Call uComSDK.start() method to render SDK on mounted element on the page.</b></summary>

</details>

<details>

<summary><b>D) Call uComSDK.stop() to kill the SDK from the page.</b></summary>

The javascript method should be called after the web content is loaded. Refer to the sample code below:

##### Web Sample Code

```code

<html>
   <head>
      <!-- uCom SDK -->
      <script src="https://int.api.firstdata.com/ucom/v2/static/v2/js/ucomsdk.js"></script>
   </head>
   <body>
      <!-- uCom SDK container -->
      <div id="ucom-container"></div>
      <script>
         /* 
         // return Callback method
         // Hosted Pages Response (Error/Success) will be returned to this callback 
         once the hosted page actions get completed.
         */
         function returnCallBack(response) {
         console.log('UCOM response:', response);
         /* Kill the uCom UI */
         ucomSDK.stop();
         /* Do rest of the flow here */
         }
         //Configuration object
         var configs = {
         "accessToken": "<access-token>",
         "apiKey": "<API-KEY>",
         "fdCustomerId": "<transaction-id>",
         "pageUrl": "https://int.api.firstdata.com/ucom/v1/hostedpages/pages/<your-page-id>",
         "mountId": "ucom-container",
         "encryptionKey": "<encryption-key>",
         "redirectUrl": "<redirect-url>",
         /*Below attribute should be enabled when you integrate and test it on 
         http://localhist in lower (CAT) environment. This attribute should be removed in 
         higher environment*/
         /* "debug": true */
         }
         /*
         ucomSDK.init(<configs>, <callbackMethod>, <isLoggingEnabled>);
         @params 
         configs - configurations object,
         callbackMethod - callback method,
         isLoggingEnabled - pass true to enable console debug logging(true/false)
         */
         //Initialize the uCom SDK with configurations
         ucomSDK.init(configs, returnCallBack);
         //Event Subscribe
         ucomSDK.on('ready', function() {
         //Handle ready event
         });
         ucomSDK.on('change', function(event) {
         if (event.formValid) {
         //Enable pay button
         } else {
         //Disable pay button
         }
         });
         ucomSDK.on('error', function(response) {
         //Handle Error Response
         });
         ucomSDK.on('success', function(response) {
         //Handle nonce
         });
         //Start the uCom UI
         ucomSDK.start();
      </script>
   </body>
</html>

```

</details>

<!-- type: tab-end -->

---

#### SDK Configuration Property Values

Please refer to the table below for additional SDK configuration properties:

>Please note that the SDK configurations below are applicable for both Hosted Page implementations: webview and iFrame.

| SDK Params     | Required/Optional | Description|
|----------------|-------------------|----------- |
| accessToken    | Required       | Session Token Id     |
| apiKey    | Required       | API Key     |
| fdCustomerId    | Required       | First data Customer Id     |
| PageURL    | Required       | Page link url of the page     |
| mountId    | Required       | Mount Id where Hosted Pages should be rendered     |
| encryptionKey    | Required       | Public key from session token response to encrypt the data     |
| redirectUrl    | Optional       | MAS URL to capture all the hosted pages response for auditing purpose.     |
| orgId    | Optional       | Org id should be passed if Threatmetrix should be enabled on Hosted Pages     |
| sessionId    | Optional       | Session id should be passed if Threatmetrix should be enabled on Hosted Pages     |
| extraObject    | Optional       | Additional details can be passed to Hosted Pages to generate nonce as part of card detail. Example is below     |
| debug    | Optional       | Eg: debug: true This attribute should be passed if you develop and integrate it on localhost (<http://localhost>) and bypass the https error on CAT environment. Note: This attribute should be removed in higher environment.     |

#### ExtraObject Sample Payload

```json
          
"billingAddress": {
    "type": "work",
    "streetAddress": "100 universal city plaza",
    "locality": "Hollywood",
    "region": "CA",
    "postalCode": "98290",
    "country": "US",
    "formatted": "100 universal city plaza,Hollywood, CA 98290 USA",
    "primary": true
}          
          
```

#### Handle POST from Hosted Pages

>Once Hosted Pages is finished, it will send the result app callback URL and redirectUrl (POST URL). MAS must implement this api to receive the result body. MAS will have to enable CORS on their end to allow access from javascript originating from “int.api.firstdata.com" and "prod.api.firstdata.com”.

<center><img src="/assets/images/HostedPages (4).png" alt="HP Diagram" class="center"></center>

## Hosted Pages Response Payloads

Below are possible responses from Hosted Pages that must be handled accordingly. The responses will differ based on the flow used, whether it's a new card, or vaulted card.

<details>
<summary><b>New Card Sample Responses</b></summary>

>**Success response payload from Connected Commerce (uCom) API**  

```json

{
   "type":"CREDIT",
   "token":{
      "tokenType":"CLAIM_CHECK_NONCE",
      "tokenProvider":"UCOM",
      "tokenId":"4f0dd98e-bf56-499c-b562-7936ca20964c"
   },
   "isSaveCard":true,
   "credit":{
      "nameOnCard":"Michael John",
      "cardType":"VISA",
      "alias":"2345"
   }
}

```

>**Success Response with SDK Error**

This is the success response with sdk errors payload. Sometimes card will be enrolled successfully but the SDK will fail to post the response into redirect URL due to some reason. In this case SDK will send back with success response with SDK errors.

```json

{
    "type": "CREDIT",
    "token": {
        "tokenType": "CLAIM_CHECK_NONCE",
        "tokenProvider": "UCOM",
        "tokenId": "4f0dd98e-bf56-499c-b562-7936ca20964c"
    },
    "isSaveCard": true,
    "credit": {
        "nameOnCard": "Michael John",
        "cardType": "VISA",
        "alias": "2345"
    },
    "developerInfo": {
        "developerMessage": "Redirect URl is failure",
        "fieldError": [
            {
                "code": 1013,
                "field": "Redirect URL",
                "message": "Redirect URL is failure"
            }
        ]
    }
} 
          
```

>**Success Response with Threatmetrix Details**

This is the enrollment response with TM(Threatmetrix) payload from Connected Commerce (uCom) API.

<!-- theme: danger -->
>Please note that this only applies if **Threatmetrix** is enabled as part of the hosted pages configurations on the back end.

```json
    
{
    "type": "CREDIT",
    "token": {
        "tokenType": "CLAIM CHECK NONCE",
        "tokenProvider": "UCOM",
        "tokenId": "4f0dd98e-bf56-499c-b562-7936ca20964c"
    },
    "isSaveCard": true,
    "credit": {
        "nameOnCard": "Michael John",
        "cardType": "VISA",
        "alias": "2345"
    },
    "userSysDetails": {
        "orgId": "8cz43sdv",
        "sessionId": "2fb1a98a-7182-497f-bc2a-79c37e556cb2"
    }
}
    
```

>**Success with Extra Params Details**

Merchant has the ability to pass the billing address into SDK. If they inject the billing address into SDK then that information will be part of the response.

```json

{
    "type": "CREDIT",
    "token": {
        "tokenType": "CLAIM_CHECK_NONCE",
        "tokenProvider": "UCOM",
        "tokenId": "4f0dd98e-bf56-499c-b562-7936ca20964c"
    },
    "isSaveCard": true,
    "credit": {
        "nameOnCard": "Michael John",
        "cardType": "VISA",
        "alias": "2345",
        "billingAddress": {
            "type": "work",
            "streetAddress": "100 universal city plaza",
            "locality": "Hollywood",
            "region": "CA",
            "postalCode": "98290",
            "country": "US",
            "formatted": "100 universal city plaza, Hollywood, CA 98290 USA",
            "primary": true
        }
    }
}
          
```

<!-- theme: danger -->
>**Failure response payload from Connected Commerce (uCom) API**

```json

{
    "code": "279912",
    "message": "Decryption failed.",
    "category": "common",
    "developerInfo": {
        "developerMessage": "Decryption failed due to invalid/expired keyId.",
        "fieldError": [
            {
                "field": "KeyId/Algorithm error.",
                "message": "crypto-service: Single use key has already been used (CR008): Key#9bff66d610b48efb829a409c8d619f1dc8306da8fb10d997abbed44fc353fa21"
            }
        ]
    }
}

```

</details>

<details>
<summary><b>Vaulted Card Sample Responses</b></summary>

>**Success**  

This is the success response payload from Connected Commerce (uCom) API

```json

{
    "type": "VAULTED_ACCOUNT",
    "token": {
        "tokenType": "CLAIM_CHECK_NONCE",
        "tokenProvider": "UCOM",
        "tokenId": "4f0dd98e-bf56-499c-b562-7936ca20964c"
    },
    "credit": {
        "cardType": "VISA",
        "alias": "2345"
    }
}
```

>**Success Response with SDK Error**

This is the success response with sdk errors payload. Sometimes card will be enrolled successfully but SDK will fail to post the response into redirect URL due to some reason. In this case SDK will send back with success response with SDK errors.

```json

{
    "type": "VAULTED_ACCOUNT",
    "token": {
        "tokenType": "CLAIM_CHECK NONCE",
        "tokenProvider": "UCOM",
        "tokenId": "4f0dd98e-bf56-499c-b562-7936ca20964c"
    },
    "credit": {
        "cardType": "VISA",
        "alias": "2345"
    },
    "developerInfo": {
        "developerMessage": "Redirect URl is failure",
        "fieldError": [
            {
                "code": 1013,
                "field": "Redirect URL",
                "message": "Redirect URL is failure"
            }
        ]
    }
}

```

>**Success with Threatmetrix Details**

This is the enrollment response with TM(Threatmetrix) payload from Connected Commerce (uCom) API.

<!-- theme: danger -->
>Please note that this only applies if **Threatmetrix** is enabled as part of the hosted pages configurations on the back end.

```json

{
    "type": "VAULTED_ACCOUNT",
    "token": {
        "tokenType": "CLAIM_CHECK_NONCE",
        "tokenProvider": "UCOM",
        "tokenId": "4f0dd98e-bf56-499c-b562-7936ca20964c"
    },
    "credit": {
        "cardType": "VISA",
        "alias": "2345"
    },
    "userSysDetails": {
        "orgId": "8cz43sdv",
        "sessionId": "2fb1a98a-7182-497f-bc2a-79c37e556cb2"
    }
}
```

>**Success with Extra Params Details**

Merchant has the ability to pass the billing address into SDK. If they inject the billing address into SDK then that information will be part of the response.

```json

{
    "type": "VAULTED_ACCOUNT",
    "token": {
        "tokenType": "CLAIM_CHECK_NONCE",
        "tokenProvider": "UCOM",
        "tokenId": "4f0dd98e-bf56-499c-b562-7936ca20964c"
    },
    "credit": {
        "cardType": "VISA",
        "alias": "2345",
        "billingAddress": {
            "type": "work",
            "streetAddress": "100 universal city plaza",
            "locality": "Hollywood",
            "region": "CA",
            "postalCode": "98290",
            "country": "US",
            "formatted": "100 universal city plaza, Hollywood, CA 98290 USA",
            "primary": true
        }
    }
}
          
```

<!-- theme: danger -->
>**Failure**  

This is the failure response payload from Connected Commerce (uCom) API

```json

{
    "response": {
        "code": "279912",
        "message": "Decryption failed.",
        "category": "common",
        "developerInfo": {
            "developerMessage": "Decryption failed due to invalid/expired keyId.",
            "fieldError": [
                {
                    "field": "KeyId/Algorithm error.",
                    "message": "crypto-service: Single use key has already been used (CR008): Key#9bff66d610b48efb829a409c8d619f1dc8306da8fb10d997abbed44fc353fa21"
                }
            ]
        }
    }
}
          
```

</details>
## Events

>Please note that this section is applicable for both Hosted Page implementations: webview and iFrame.

The only way to communicate with Hosted Pages is by listening to an event. Hosted Pages will emit and communicate back if you are subscribed with those events.

```code
          
ucomSDK.on(event, handler);   
          
```

1. **onReady**

    Triggered when iFrame is fully rendered and can accept user’s inputs.

    ```code
            
    ucomSDK.on('ready', function () { //Handle ready event
    });
            
    ```

2. **onChange**

    Triggered when form value is changed. The event payload always contains object with form valid status.

    ```code
            
    ucomSDK.on('change', function () {
    if (event.formValid) {
        //Enable pay button
    } else {
        //Disable pay button
    }
    });          

    ```

    Handler Event Object

    ```code
    {
    "elementType": "payment",
    "formValid": false
    }
            
    ```

3. **onError**

    Triggered when the Hosted Pages API has errors. The event payload object contains API error which needs to be handled on app.

    ```code
            
    ucomSDK.on('error', function (response) { //Handle Error Response
    });
    ```

4. **onSuccess**

    Triggered when Hosted Pages generates a nonce. The event payload object contains nonce which needs to be handled on app.

    ```code
            
    ucomSDK.on('success', function (response) { //Handle Nonce 
    });
            
    ```

## API Error Status Codes

Below error status code needs to be handled from client side. These API error responses will be communicated back to App JavaScript main callback to handle the errors and show the appropriate error dialog.

### Error Responses Before Screen Renders

Following errors will be thrown before hosted pages screen render.

| Status Code     | Transaction Status Desc | Comments |
|----------------|-------------------|----------- |
| 269904    | Configuration record(s) not found.       | When an invalid page id is passed, then the API will throw this error     |
| 401    | Unauthorized       | This error will occur when when an invalid access token id is passed.     |

#### Example Response Payload

```json

{
    "response": {
        "code": "269904",
        "message": "Configuration record(s) not found.",
        "category": "common",
        "developerInfo": {
            "developerMessage": "Configuration record(s) not found."
        }
    }
}

```

```json

{
    "response": {
        "code": 401,
        "message": "Unauthorized",
        "category": "common",
        "developerInfo": {
            "developerMessage": "Unauthorized"
        }
    }
}
          
```

#### Error Responses After Form Submission

Following errors will be thrown after hosted pages screen render

| Status Code     | Transaction Status Desc | Comments |
|----------------|-------------------|----------- |
| 279912    | Decryption failed.        | When we pass an invalid or an already used public key into SDK then API will throw this error.     |
| 269901    | Unable to process your request, please try again later, if problem persist, contact sys admin.       | This error occurs when a communications problem occurs between the frontend and backend.    |
| 401    | Access Token expired or Unauthorized       | This error will occur when user idle and try to submit the rendered form with expired access token.     |

Example response payload:  

```json
          
{
    "response": {
        "code": "279912",
        "message": "Decryption failed.",
        "category": "common",
        "developerInfo": {
            "developerMessage": "Decryption failed due to invalid/expiredkeyId.",
            "fieldError": [
                {
                    "field": "KeyId/Algorithm error.",
                    "message": "crypto-service: Single use key has already been used (CR008): Key#9bff66d610b48efb829a409c8d619f1dc8306da8fb10d997abbed44fc353fa21"
                }
            ]
        }
    }
}
         
```

Example response payload:

```json

{
    "response": {
        "code": 401,
        "message": "Unauthorized",
        "category": "common",
        "developerInfo": {
            "developerMessage": "Unauthorized"
        }
    }
}
          
```

## Native/Web Submit Button

If needed, Hosted Pages has the ability to submit the form through the mobile app native button or website button from outside the iFrame. Following command will trigger the save action:

```code

ucomSDK.triggerSaveAction(); 

```

>Please note: this has to be triggered from outside the iFrame or web view.
