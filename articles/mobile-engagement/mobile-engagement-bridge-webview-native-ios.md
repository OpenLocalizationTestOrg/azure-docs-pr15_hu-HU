<properties 
    pageTitle="Híd iOS webnézet natív Mobile tetszés szerint elmélyedhet iOS operációs rendszerrel SDK" 
    description="Megtudhatja, hogy miként közötti Javascript és a natív SDK Mobile tetszés szerint elmélyedhet iOS rendszerű webnézet híd létrehozása"      
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="erikre" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-ios" 
    ms.devlang="objective-c" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="bridge-ios-webview-with-native-mobile-engagement-ios-sdk"></a>Híd iOS webnézet natív Mobile tetszés szerint elmélyedhet iOS operációs rendszerrel SDK

> [AZURE.SELECTOR]
- [Android-híd](mobile-engagement-bridge-webview-native-android.md)
- [iOS-híd](mobile-engagement-bridge-webview-native-ios.md)

Néhány mobilalkalmazások hibrid alkalmazásként ahol magát az alkalmazás fejlesztett natív iOS C-cél fejlesztés csak néhányat vagy akár az összeset a képernyőn való megjelenítését az iOS webnézet belül készültek. Továbbra is használhatja az Mobile tetszés szerint elmélyedhet iOS SDK ilyen-alkalmazásokból, és ebben az oktatóanyagban ismerteti, hogyan érhetők el az ezzel kapcsolatos. 

Kétféleképpen azonban nem dokumentált mindkettő cél:

- Először egy leírása a [hivatkozás](http://stackoverflow.com/questions/9826792/how-to-invoke-objective-c-method-from-javascript-and-send-back-data-to-javascrip) , amely magában foglalja a Regisztrálás egy `UIWebViewDelegate` a webes nézet és a tényleges és-azonnal-megszakítás Javascript a munkavégzés helye módosítás. 
- Második egyik alapján a [WWDC 2013 munkamenet](https://developer.apple.com/videos/play/wwdc2013/615)megközelítés, amely olyan, mint az első tisztább, és amely azt az Ez az útmutató követi. Figyelje meg, hogy ez a módszer csak akkor működik, a ios7 rendszerhez és a fenti. 

Az iOS egymással, áthidalhatják a minta esetében, kövesse az alábbi lépéseket:

1. Első lépésként meg kell győződjön meg arról, hogy telt-keresztül az [oktatóanyag első lépések](mobile-engagement-ios-get-started.md) a hibrid alkalmazásban Mobile tetszés szerint elmélyedhet iOS SDK integrálása. Tetszés szerint is engedélyezheti, hogy megtekintése a SDK módszerek azt indítja el őket a webnézet a próba naplózás az alábbi képlettel történik. 
    
        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
           ....
            [EngagementAgent setTestLogEnabled:YES];
           ....
        }

2. Most már győződjön meg arról, hogy a hibrid alkalmazást a képernyő egy webnézet rajta. Kattintva felveheti a `Main.storyboard` az alkalmazás. 

3. A webnézet társítani a **ViewController** kattintva, majd húzza a webnézet való nézet vezérlő jelenetről a `ViewController.h` képernyőn helyezze szerkesztése csak az alábbi a `@interface` sor. 

4. Miután ezt megtette, egy párbeszédpanel jeleníthető meg fog felugró kérő nevét. Adja meg nevét, **webnézet**. A `ViewController.h` fájl az alábbiakhoz hasonlóan kell kinéznie:

        #import <UIKit/UIKit.h>
        #import "EngagementViewController.h"
        
        @interface ViewController : EngagementViewController
        @property (strong, nonatomic) IBOutlet UIWebView *webView;
        
        @end

5. Frissíteni fogjuk a `ViewController.m` fájlt később, de először azt hozza létre a híd fájlt, amely egy bizonyos leggyakrabban használt mobil tetszés szerint elmélyedhet iOS fölé hoz létre a SDK módszereket. Élőfej használja **EngagementJsExports.h** nevű új fájl létrehozása a `JSExport` kattintva jelenítse meg az iOS natív módszerek [munkamenet](https://developer.apple.com/videos/play/wwdc2013/615) fent leírt eljárást. 

        #import <Foundation/Foundation.h>
        #import <JavaScriptCore/JavascriptCore.h>
        
        @protocol EngagementJsExports <JSExport>
        
        + (void) sendEngagementEvent:(NSString*) name :(NSString*)extras;
        + (void) startEngagementJob:(NSString*) name :(NSString*)extras;
        + (void) endEngagementJob:(NSString*) name;
        + (void) sendEngagementError:(NSString*) name :(NSString*)extras;
        + (void) sendEngagementAppInfo:(NSString*) appInfo;
        
        @end

        @interface EngagementJs : NSObject <EngagementJsExports>

        @end

6. A második rész a híd fájl ezután azt hoz létre. Hozzon létre egy fogja tartalmazni a tényleges csomagolást létrehozása hívja fel a Mobile tetszés szerint elmélyedhet iOS SDK módszerek végrehajtása **EngagementJsExports.m** nevű fájlt. Is vegye figyelembe, hogy azt is elemzése a `extras` a webnézet javascript az átadott és üzembe be, hogy egy `NSMutableDictionary` a tetszés szerint elmélyedhet SDK módszer hívások átadandó objektumot.  

        #import <UIKit/UIKit.h>
        #import "EngagementAgent.h"
        #import "EngagementJsExports.h"
        
        @implementation EngagementJs
        
        +(void) sendEngagementEvent:(NSString*)name :(NSString*)extras {
           NSMutableDictionary* extrasInput = [self ParseExtras:extras];
           [[EngagementAgent shared] sendEvent:name extras:extrasInput];
        }
        
        + (void) startEngagementJob:(NSString*) name :(NSString*)extras {
           NSMutableDictionary* extrasInput = [self ParseExtras:extras];
           [[EngagementAgent shared] startJob:name extras:extrasInput];
        }
        
        + (void) endEngagementJob:(NSString*) name {
           [[EngagementAgent shared] endJob:name];
        }
        
        + (void) sendEngagementError:(NSString*) name :(NSString*)extras {
           NSMutableDictionary* extrasInput = [self ParseExtras:extras];
           [[EngagementAgent shared] sendError:name extras:extrasInput];
        }
        
        + (void) sendEngagementAppInfo:(NSString*) appInfo {
           NSMutableDictionary* appInfoInput = [self ParseExtras:appInfo];
           [[EngagementAgent shared] sendAppInfo:appInfoInput];
        }
        
        + (NSMutableDictionary*) ParseExtras:(NSString*) input {
           NSData *data = [input dataUsingEncoding:NSUTF8StringEncoding];
           NSError* error = nil;
           NSMutableDictionary* extras = [NSJSONSerialization JSONObjectWithData:data options:0 error:&error];
           
           return extras;
        }
        
        @end

5. Most már azt térjen vissza az a **ViewController.m** , és frissítse azt a következő kódot: 
        
        #import <JavaScriptCore/JavaScriptCore.h>
        #import "ViewController.h"
        #import "EngagementJsExports.h"
        
        @interface ViewController ()
        
        @end
        
        @implementation ViewController
        
        - (void)viewDidLoad
        {
           self.webView.delegate = self;
           [super viewDidLoad];
           [self loadWebView];
        }
        
        - (void)loadWebView {
           NSBundle* mainBundle = [NSBundle mainBundle];
           NSURL* htmlPage = [mainBundle URLForResource:@"LocalPage" withExtension:@"html"];
           
           NSURLRequest* urlReq = [NSURLRequest requestWithURL:htmlPage];
           [self.webView loadRequest:urlReq];
        }
        
        - (void)webViewDidFinishLoad:(UIWebView*)wv
        {
           JSContext* context = [wv valueForKeyPath:@"documentView.webView.mainFrame.javaScriptContext"];
           
           context[@"EngagementJs"] = [EngagementJs class];
        }
        
        - (void)webView:(UIWebView*)wv didFailLoadWithError:(NSError*)error
        {
           NSLog(@"Error for WEBVIEW: %@", [error description]);
        }
        
        - (void)didReceiveMemoryWarning {
           [super didReceiveMemoryWarning];
           // Dispose of any resources that can be recreated.
        }
        
        @end

6. Megjegyzés: a **ViewController.m** fájllal kapcsolatos, a következőket:

    - Az a `loadWebView` módszer, hogy tölt **LocalPage.html** következő azt fogja megvizsgálni azonosítójú nevű helyi HTML-fájl. 
    - Az a `webViewDidFinishLoad` módszer, hogy vannak grabbing a `JsContext` és a csomagolópapír osztály társítása. Hívja fel a csomagolópapír SDK módszerek a webnézet a **EngagementJs** -leíró használatával lehetővé teszi a. 

7. A következő kódot **LocalPage.html** nevű fájl létrehozása:

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
                   alert('window.onerror: ' + err);
               }
               
               function send(inputId)
               {
                   var input = document.getElementById(inputId);
                   if(input)
                   {
                       var value = input.value;
                       // Example of how extras info can be passed with the Engagement logs
                       var extras = '{"CustomerId":"MS290011"}';
                   }
                   
                   if(value && value.length > 0)
                   {
                       switch(inputId)
                       {
                           case "event":
                           EngagementJs.sendEngagementEvent(value, extras);
                           break;
                           
                           case "job":
                           EngagementJs.startEngagementJob(value, extras);
                           window.setTimeout(
                           function(){
                               EngagementJs.endEngagementJob(value);
                           }, 10000 );
                           break;
        
                           case "error":
                           EngagementJs.sendEngagementError(value, extras);
                           break;
                           
                           case "appInfo":
                           var appInfo = '{"customer_name":"' + value + '"}';
                           EngagementJs.sendEngagementAppInfo(appInfo);
                           break;
                       }
                   }
               }
               </script>
               
           </head>
           <body>
               <h1>Bridge Tester</h1>
               
               <div id='engagement'>
                   
                   <br/>
                   <h2>Event</h2>
                   <input type="text" id="event" size="35">
                   <button onclick="send('event')">Send</button>
                   
                   <br/>
                   <h2>Job</h2>
                   <input type="text" id="job" size="35">
                   <button onclick="send('job')">Send</button>
                   
                   <br/>
                   <h2>Error</h2>
                   <input type="text" id="error" size="35">
                   <button onclick="send('error')">Send</button
                   
                   <br/>
                   <h2>AppInfo</h2>
                   <input type="text" id="appInfo" size="35">
                   <button onclick="send('appInfo')">Send</button>
               
               </div>
           </body>
        </html>

8. Megjegyzés: a fenti HTML-fájllal kapcsolatos, a következőket:

    -   Egy sor olyan beviteli mezők, ahol megadhatja az esemény, a projekt, a hiba, a AppInfo névként használandó adatokat tartalmazza. Ha a mellette gombra kattint, a hívás a Javascript, amely végül hívja fel a módszerek a híd fájl átadni a hívást Mobile tetszés szerint elmélyedhet IOS-en SDK csomagjában talál. 
    -   Címkézés azt is, néhány statikus további információ az események, a feladatok és a páros hibák bemutatják, hogyan ezt megteheti a. A további információ küldi el, a JSON karakterlánc, amely jelenik meg a `EngagementJsExports.m` fájl, elemzése és átadott küldése, feladatok, események hibák együtt. 
    -   A mobil tetszés szerint elmélyedhet feladat futtatása az 10 másodperc, majd állítsa le a beviteli mezőben megadott nevű van megrúgni. 
    -   A mobil tetszés szerint elmélyedhet appinfo vagy címke átadott "customer_name", az érték, amely a megadott a szövegbeviteli értékként a címke, valamint a statikus billentyűt. 
 
9. Futtassa az alkalmazást, és a következő jelenik meg. Most adjon meg egy próba eseményt, az alábbihoz hasonló néhány nevet, és mellett kattintson a **Küldés** gombra. 

    ![][1]

10. Most ha megnyitja a **Monitor** lapján az alkalmazást, és keresse meg, az **események részleteinek ->**, látni fogja az esemény jelenik meg a statikus app – útmutatók, amely azt küldi együtt. 

    ![][2]

<!-- Images. -->
[1]: ./media/mobile-engagement-bridge-webview-native-ios/sending-event.png
[2]: ./media/mobile-engagement-bridge-webview-native-ios/event-output.png
