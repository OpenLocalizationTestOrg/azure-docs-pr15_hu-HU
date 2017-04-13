<properties 
    pageTitle="Híd Android webnézet Android SDK natív Mobile tetszés szerint elmélyedhet a" 
    description="Megtudhatja, hogy miként közötti Javascript és a natív Mobile tetszés szerint elmélyedhet Android SDK webnézet híd létrehozása"      
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="erikre" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-android" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="bridge-android-webview-with-native-mobile-engagement-android-sdk"></a>Híd Android webnézet Android SDK natív Mobile tetszés szerint elmélyedhet a

> [AZURE.SELECTOR]
- [Android-híd](mobile-engagement-bridge-webview-native-android.md)
- [iOS-híd](mobile-engagement-bridge-webview-native-ios.md)

Néhány mobilalkalmazások hibrid alkalmazásként ahol magát az alkalmazás fejlesztett natív Android fejlesztési csak néhányat vagy akár az összeset a képernyőn való megjelenítését belül az Android webnézet készültek. Mobil tetszés szerint elmélyedhet Android SDK ilyen alkalmazásokból továbbra is használhatja, és ebben az oktatóanyagban ismerteti, hogyan érhetők el az ezzel kapcsolatos. Az alábbi példakódot alapuló Android át [az alábbi](https://developer.android.com/guide/webapps/webview.html#BindingJavaScript). Azt ismerteti, hogyan dokumentált módon, hogy egy hibrid alkalmazásból webnézet gázvezetékrendszer őket az Android SDK keresztül közben is kezdeményezhet kérések nyomon követheti az eseményeket, a feladatok, a hibák, a app – útmutatók végrehajtásához azonos a Mobile tetszés szerint elmélyedhet Android SDK a gyakran használt módszerek felhasználható. 

1. Első lépésként meg kell győződjön meg arról, hogy telt-keresztül az [oktatóanyag első lépések](mobile-engagement-android-get-started.md) a hibrid alkalmazásban SDK Android Mobile tetszés szerint elmélyedhet integrálása. Az ehhez szükséges lépéseket, amikor a `OnCreate` módszer az alábbiakhoz hasonlóan fog kinézni.  
    
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
    
            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("<Mobile Engagement Conn String>");
            EngagementAgent.getInstance(this).init(engagementConfiguration);
        }

2. Most már győződjön meg arról, hogy a hibrid alkalmazást a képernyő egy webnézet rajta. A kódját hasonló lesz a következő, ahol betöltése azt egy helyi HTML fájl **Sample.html** a webnézetben a `onCreate` módszer a képernyőt. 

        private void SetWebView() {
            WebView myWebView = (WebView) findViewById(R.id.webview);
            myWebView.loadUrl("file:///android_asset/Sample.html");
        }

        protected void onCreate(Bundle savedInstanceState) {
            ...
            ...
            SetWebView();
        }

3. Most az úgynevezett hoz létre egy bizonyos gyakran használt Mobile tetszés szerint elmélyedhet Android SDK módszerek segítségével fölé **WebAppInterface** híd fájl létrehozása a `@JavascriptInterface` [Android dokumentációjában](https://developer.android.com/guide/webapps/webview.html#BindingJavaScript)leírt módon:

        import android.content.Context;
        import android.os.Bundle;
        import android.util.Log;
        import android.webkit.JavascriptInterface;
        
        import com.microsoft.azure.engagement.EngagementAgent;
        
        import org.json.JSONArray;
        import org.json.JSONObject;
        
        public class WebAppInterface {
            Context mContext;
        
            /** Instantiate the interface and set the context */
            WebAppInterface(Context c) {
                mContext = c;
            }
        
            @JavascriptInterface
            public void sendEngagementEvent(String name, String extras ){
                EngagementAgent.getInstance(mContext).sendEvent(name, ParseExtras(extras));
            }
        
            @JavascriptInterface
            public void startEngagementJob(String name, String extras ){
                EngagementAgent.getInstance(mContext).startJob(name, ParseExtras(extras));
            }
        
            @JavascriptInterface
            public void endEngagementJob(String name){
                EngagementAgent.getInstance(mContext).endJob(name);
            }
        
            @JavascriptInterface
            public void sendEngagementError(String name, String extras ){
                EngagementAgent.getInstance(mContext).sendError(name, ParseExtras(extras));
            }
        
            @JavascriptInterface
            public void sendEngagementAppInfo(String appInfo){
                EngagementAgent.getInstance(mContext).sendAppInfo(ParseExtras(appInfo));
            }
        
            public Bundle ParseExtras(String input) {
                Bundle extras = new Bundle();
        
                try {
                    JSONObject jObject = new JSONObject(input);
                    extras.putString(jObject.names().getString(0),
                            jObject.get(jObject.names().getString(0)).toString());
                } catch (Exception e) {
                    e.printStackTrace();
                }
                return extras;
            }
        }  

4. A fenti híd-fájl létrehozása azt után szükség érdekében, hogy a webnézet társítva. Ehhez fordulhat elő, akkor módosíthatók a `SetWebview` módszer, hogy néz ki a következőket:

        private void SetWebView() {
            WebView myWebView = (WebView) findViewById(R.id.webview);
            myWebView.loadUrl("file:///android_asset/Sample.html");
            WebSettings webSettings = myWebView.getSettings();
            webSettings.setJavaScriptEnabled(true);
            myWebView.addJavascriptInterface(new WebAppInterface(this), "EngagementJs");
        }

5. A fenti kódtöredékének azt nevű `addJavascriptInterface` társíthatja az híd osztály a webnézet és is létrehozott egy úgynevezett **EngagementJs** hívja fel a módszerek a híd fájlból fogópontot. 

6. A következő fájl neve **Sample.html** a projekt, amely a webnézet betöltött, és hol azt visszahívja módszerek a híd fájlból **eszközök** nevű mappában létrehozhat.

        <!doctype html>
        <html>
            <head>
                <style type='text/css'>
                    html { font-family:Helvetica; color:#222; }
                    h1 { color:steelblue; font-size:22px; margin-top:16px; }
                    h2 { color:grey; font-size:14px; margin-top:18px; margin-bottom:0px; }
                </style>
        
                <script type="text/javascript">
        
                    window.onerror = function(err)
                    {
                      log('window.onerror: ' + err);
                    }
        
                    send = function(inputId)
                    {
                        var input = document.getElementById(inputId);
                        if(input)
                        {
                            var value = input.value;
                            // Example of how extras info can be passed with the Engagement logs
                            var extras = '{"CustomerId":"MS290011"}';
        
                            if(value && value.length > 0)
                            {
                                switch(inputId)
                                {
                                    case "event":
                                    EngagementJs.sendEngagementEvent(value, extras);
                                    break;
        
                                    case "job":
                                    EngagementJs.startEngagementJob(value, extras);
                                    window.setTimeout( function()
                                    {
                                      EngagementJs.endEngagementJob(value);
                                    }, 10000 );
                                    break;
        
                                    case "error":
                                    EngagementJs.sendEngagementError(value, extras);
                                    break;
        
                                    case "appInfo":
                                    EngagementJs.sendEngagementAppInfo({"customer_name":value});
                                    break;
                                }
                            }
                        }
                    }
                </script>
            </head>
            <body>
                <h1>Bridge Tester</h1>
                <div id='engagement'>
                    <h2>Event</h2>
                    <input type="text" id="event" size="35">
                    <button onclick="send('event')">Send</button>
        
                    <h2>Job</h2>
                    <input type="text" id="job" size="35">
                    <button onclick="send('job')">Send</button>
        
                    <h2>Error</h2>
                    <input type="text" id="error" size="35">
                    <button onclick="send('error')">Send</button>
        
                    <h2>AppInfo</h2>
                    <input type="text" id="appInfo" size="35">
                    <button onclick="send('appInfo')">Send</button>
                </div>
            </body>
        </html>

8. Megjegyzés: a fenti HTML-fájllal kapcsolatos, a következőket:

    -   Egy sor olyan beviteli mezők, ahol megadhatja az esemény, a projekt, a hiba, a AppInfo névként használandó adatokat tartalmazza. Ha a mellette gombra kattint, a hívás a Javascript, amely végül hívja fel a módszerek a híd fájl átadni a hívást a Mobile tetszés szerint elmélyedhet Android SDK csomagjában talál. 
    -   Címkézés azt is, néhány statikus további információ az események, a feladatok és a páros hibák bemutatják, hogyan ezt megteheti a. A további információ küldi el, a JSON karakterlánc, amely jelenik meg a `WebAppInterface` fájl, elemzett, és helyezze az Android `Bundle` és a feladatok, események hibák küldése együtt átadott. 
    -   A mobil tetszés szerint elmélyedhet feladat futtatása az 10 másodperc, majd állítsa le a beviteli mezőben megadott nevű van megrúgni. 
    -   A mobil tetszés szerint elmélyedhet appinfo vagy címke átadott "customer_name", az érték, amely a megadott a szövegbeviteli értékként a címke, valamint a statikus billentyűt. 
 
9. Futtassa az alkalmazást, és a következő jelenik meg. Most adjon meg egy próba eseményt, az alábbihoz hasonló néhány nevet, és, kattintson a **Küldés** gombra. 

    ![][1]

10. Most ha megnyitja a **Monitor** lapján az alkalmazást, és keresse meg, az **események részleteinek ->**, látni fogja az esemény jelenik meg a statikus app – útmutatók, amely azt küldi együtt. 

    ![][2]

<!-- Images. -->
[1]: ./media/mobile-engagement-bridge-webview-native-android/sending-event.png
[2]: ./media/mobile-engagement-bridge-webview-native-android/event-output.png
