<properties
    pageTitle="Azure függvények értesítés központi kötés |} Microsoft Azure"
    description="Megtudhatja, hogyan Azure értesítés központi kötés Azure függvények használata."
    services="functions"
    documentationCenter="na"
    authors="wesmc7777"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure működik, függvények, esemény feldolgozása, dinamikus számítási, kiszolgáló nélküli architektúra"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="10/27/2016"
    ms.author="wesmc"/>

# <a name="azure-functions-notification-hub-output-binding"></a>Azure függvények értesítés központi kimeneti kötése

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Ez a cikk ismerteti, hogy konfigurálása és a kód Azure értesítés központi kötések Azure-függvényekkel. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

A függvények elküldheti a leküldéses értesítéseket, és néhány kódsorokat konfigurált Azure értesítés elosztót használ. Azonban az Azure-értesítés központban kell beállítania az a Platform értesítések szolgáltatások (PNS) szeretné használni. További információt az Azure értesítés központi konfigurálása és egy értesítést szeretne kapni a regisztráció ügyfél-alkalmazások fejlesztése megtekintéséhez, [értesítés hubok – első lépések](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) , és kattintson a felső részén található ügyfél céltábladiagram illő.

Az értesítést küld a natív értesítések vagy sablon értesítések lehet. Konfigurált target natív értesítések egy adott értesítés platform a `platform` a kimeneti kötelező tulajdonsága. Sablon értesítés több érintettek használható.   

## <a name="notification-hub-output-binding-properties"></a>Értesítés központi kimeneti kötési tulajdonságok

A function.json fájlt tartalmazza a következő tulajdonságokat:

- `name`: Változó nevét a kód függvény az értesítési központi üzenet.
- `type`: *"notificationHub"*értékre kell állítani.
- `tagExpression`: Címke a kifejezések megadhatja, hogy értesítéseket, kézbesítése egy sor olyan eszközök, amelyek megegyeznek a címke kifejezés értesítést szeretne kapni a regisztrált teszi lehetővé.  További tudnivalókért lásd: [Útválasztás és a címkét](../notification-hubs/notification-hubs-tags-segment-push-message.md).
- `hubName`: Az Azure-portálon értesítés központi erőforrás neve.
- `connection`: Ez a kapcsolati karakterlánc- **Alkalmazásbeállítással** kapcsolati karakterláncot az értesítési központi *DefaultFullSharedAccessSignature* értékét meg kell lennie.
- `direction`: *","*értékre kell állítani. 
- `platform`: A platform tulajdonság jelzi az értesítési platform az értesítési célokat. A következő értékek egyike lehet:
    - `template`: Ez esetén az alapértelmezett platform a platform tulajdonság nem szerepel a kimeneti kötés. Sablon értesítések bármely az Azure-értesítés központban konfigurált platform célba használható. Sablonok használata az általános közötti platform értesítések és az Azure értesítés központi elküldéséhez a további tudnivalókért lásd: [sablonok](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md).
    - `apns`: Az Apple leküldéses értesítéseket kezelő szolgáltatás. Beállításáról további információt az értesítés-központban, az APN és a értesítést kap egy ügyfél-alkalmazásban, [értesítés](../notification-hubs/notification-hubs-ios-apple-push-notification-apns-get-started.md) jelenik meg, küldése leküldéses Azure értesítés hubok az IOS-en 
    - `adm`: [Amazon eszköz üzenetküldés](https://developer.amazon.com/device-messaging). Az értesítési-központban ADM konfigurálása és a értesítést kap egy Kindle alkalmazásban a további tudnivalókért lásd: [Értesítés veszik fel a szükséges Kindle alkalmazások – első lépések](../notification-hubs/notification-hubs-kindle-amazon-adm-push-notification.md) 
    - `gcm`: [A Google Cloud üzenetküldés](https://developers.google.com/cloud-messaging/). Firebase felhő üzenetküldés, amely az új verzió GCM, is támogatott. Beállításáról további információt az értesítés-központban GCM/FCM és a értesítést kap egy Android ügyfél alkalmazásban, [értesítés](../notification-hubs/notification-hubs-android-push-notification-google-fcm-get-started.md) jelenik meg, küldése leküldéses az Azure értesítés hubok androidon
    - `wns`: [Windows leküldéses értesítést szolgáltatások](https://msdn.microsoft.com/en-us/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview) Windows platformon kiválasztásával. Windows Phone 8.1-es vagy újabb verzió WNS is támogatott. Beállításáról további információt az értesítés-központban WNS és a értesítést kap egy univerzális Windows platformon (UWP)-alkalmazásban, lásd: [értesítés hubok for Windows univerzális Platform alkalmazások – első lépések](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md)
    - `mpns`: [A Microsoft leküldéses értesítéseket kezelő szolgáltatás](https://msdn.microsoft.com/en-us/library/windows/apps/ff402558.aspx). A platform támogatja a Windows Phone 8 és Windows Phone-platformokon korábbi. Beállításáról további információt az értesítés-központban MPNS és a értesítést kap a Windows Phone-alkalmazásban, [küldése leküldéses értesítéseket, az Azure értesítés hubok a Windows Phone](../notification-hubs/notification-hubs-windows-mobile-push-notifications-mpns.md) lásd:
 
Példa function.json:

    {
      "bindings": [
        {
          "name": "notification",
          "type": "notificationHub",
          "tagExpression": "",
          "hubName": "my-notification-hub",
          "connection": "MyHubConnectionString",
          "platform": "gcm",
          "direction": "out"
        }
      ],
      "disabled": false
    }

## <a name="notification-hub-connection-string-setup"></a>Értesítés központi csatlakozási karakterlánc beállítása

Egy értesítés központi kimenet kötelező használatához be kell állítania a kapcsolati karakterlánc számára a központi. Ez történik a *Integrate* lapon az értesítési központi bejelölésével, illetve újat hozna létre. 

Manuálisan is felveheti egy meglévő központi a kapcsolati karakterlánc hozzáadásával egy kapcsolati karakterláncot az *DefaultFullSharedAccessSignature* az az értesítési hubon keresztül csatlakozott. A kapcsolati karakterláncot az értesítő üzenet küldése függvény hozzáférési engedélyt biztosít. A *DefaultFullSharedAccessSignature* kapcsolat karakterláncérték webböngészőn keresztül elérhetők a **billentyűk** a fő a az Azure-portálra értesítési központi erőforrás lap gombra. A központi a kapcsolati karakterlánc manuális hozzáadásához kövesse az alábbi lépéseket: 

1. Kattintson a Azure portál **függvény alkalmazás** lap, **függvény alkalmazás Beállítások > alkalmazás szolgáltatás beállítások**.

2. Kattintson a **Beállítások** lap **Beállításai**.

3. Görgessen lefelé a **kapcsolati karakterláncot** szakaszig, és vegyen fel egy névvel ellátott *DefaultFullSharedAccessSignature* értékét a értesítés-központját. A típus módosítása **egyéni**.
4. A kapcsolati karakterlánc neve a kimeneti kötések hivatkozást. A fenti példában **MyHubConnectionString** hasonló.

## <a name="native-notification-examples"></a>Natív értesítés – példa

#### <a name="apns-native-notifications-with-c-queue-triggers"></a>APN a és C# várólista indítók natív értesítések

Ebben a példában a [Microsoft Azure értesítés hubok tár](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) definiált típusok használatához natív APN értesítés küldése. 

    #r "Microsoft.Azure.NotificationHubs"
    #r "Newtonsoft.Json"
    
    using System;
    using Microsoft.Azure.NotificationHubs;
    using Newtonsoft.Json;
    
    public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
    
        // In this example the queue item is a new user to be processed in the form of a JSON string with 
        // a "name" value.
        //
        // The JSON format for a native APNS notification is ...
        // { "aps": { "alert": "notification message" }}  

        log.Info($"Sending APNS notification of a new user");   
        dynamic user = JsonConvert.DeserializeObject(myQueueItem);  
        string apnsNotificationPayload = "{\"aps\": {\"alert\": \"A new user wants to be added (" + 
                                            user.name + ")\" }}";
        log.Info($"{apnsNotificationPayload}");
        await notification.AddAsync(new AppleNotification(apnsNotificationPayload));        
    }

#### <a name="gcm-native-notifications-with-c-queue-triggers"></a>GCM a és C# várólista indítók natív értesítések

Ebben a példában a [Microsoft Azure értesítés hubok tár](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) definiált típusok használatához natív GCM értesítés küldése. 

    #r "Microsoft.Azure.NotificationHubs"
    #r "Newtonsoft.Json"
    
    using System;
    using Microsoft.Azure.NotificationHubs;
    using Newtonsoft.Json;
    
    public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
    
        // In this example the queue item is a new user to be processed in the form of a JSON string with 
        // a "name" value.
        //
        // The JSON format for a native GCM notification is ...
        // { "data": { "message": "notification message" }}  

        log.Info($"Sending GCM notification of a new user");    
        dynamic user = JsonConvert.DeserializeObject(myQueueItem);  
        string gcmNotificationPayload = "{\"data\": {\"message\": \"A new user wants to be added (" + 
                                            user.name + ")\" }}";
        log.Info($"{gcmNotificationPayload}");
        await notification.AddAsync(new GcmNotification(gcmNotificationPayload));       
    }

#### <a name="wns-native-notifications-with-c-queue-triggers"></a>WNS a és C# várólista indítók natív értesítések

Ebben a példában a [Microsoft Azure értesítés hubok tár](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) definiált típusok használatához natív WNS bejelentési értesítés küldése. 

    #r "Microsoft.Azure.NotificationHubs"
    #r "Newtonsoft.Json"
    
    using System;
    using Microsoft.Azure.NotificationHubs;
    using Newtonsoft.Json;
    
    public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
    
        // In this example the queue item is a new user to be processed in the form of a JSON string with 
        // a "name" value.
        //
        // The XML format for a native WNS toast notification is ...
        // <?xml version="1.0" encoding="utf-8"?>
        // <toast>
        //   <visual>
        //     <binding template="ToastText01">
        //       <text id="1">notification message</text>
        //     </binding>
        //   </visual>
        // </toast>

        log.Info($"Sending WNS toast notification of a new user");  
        dynamic user = JsonConvert.DeserializeObject(myQueueItem);  
        string wnsNotificationPayload = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                                        "<toast><visual><binding template=\"ToastText01\">" +
                                            "<text id=\"1\">" + 
                                                "A new user wants to be added (" + user.name + ")" + 
                                            "</text>" +
                                        "</binding></visual></toast>";

        log.Info($"{wnsNotificationPayload}");
        await notification.AddAsync(new WindowsNotification(wnsNotificationPayload));       
    }


## <a name="template-notification-examples"></a>Sablon értesítés példák

#### <a name="template-example-for-nodejs-timer-triggers"></a>Sablon például Node.js időzítő eseményindítók 

Ez a példa tartalmazó [sablon regisztrációs](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) értesítést küld `location` és `message`.

    module.exports = function (context, myTimer) {
        var timeStamp = new Date().toISOString();
       
        if(myTimer.isPastDue)
        {
            context.log('Node.js is running late!');
        }
        context.log('Node.js timer trigger function ran!', timeStamp);  
        context.bindings.notification = {
            location: "Redmond",
            message: "Hello from Node!"
        };
        context.done();
    };

#### <a name="template-example-for-f-timer-triggers"></a>Az F # időzítő eseményindítók sablon példa

Ez a példa tartalmazó [sablon regisztrációs](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) értesítést küld `location` és `message`.

    let Run(myTimer: TimerInfo, notification: byref<IDictionary<string, string>>) =
        notification = dict [("location", "Redmond"); ("message", "Hello from F#!")]


#### <a name="template-example-using-an-out-parameter"></a>Sablon például egy kimenő paraméter használatával 

Ez a példa tartalmazó [sablon regisztrációs](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) értesítést küld egy `message` a sablon helyőrző.

    using System;
    using System.Threading.Tasks;
    using System.Collections.Generic;
     
    public static void Run(string myQueueItem,  out IDictionary<string, string> notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
        notification = GetTemplateProperties(myQueueItem);
    }
     
    private static IDictionary<string, string> GetTemplateProperties(string message)
    {
        Dictionary<string, string> templateProperties = new Dictionary<string, string>();
        templateProperties["message"] = message;
        return templateProperties;
    }

#### <a name="template-example-with-asynchronous-function"></a>Példa sablon aszinkron függvény

Ha aszinkron kód használata esetén meg a paraméterek nem használhatók. Ebben az esetben használható `IAsyncCollector` való visszatéréshez a sablon értesítés. A következő kódot példája aszinkron a fenti kódot. 

    using System;
    using System.Threading.Tasks;
    using System.Collections.Generic;
    
    public static async Task Run(string myQueueItem, IAsyncCollector<IDictionary<string,string>> notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
    
        log.Info($"Sending Template Notification to Notification Hub");
        await notification.AddAsync(GetTemplateProperties(myQueueItem));    
    }
    
    private static IDictionary<string, string> GetTemplateProperties(string message)
    {
        Dictionary<string, string> templateProperties = new Dictionary<string, string>();
        templateProperties["user"] = "A new user wants to be added : " + message;
        return templateProperties;
    }

#### <a name="template-example-using-json"></a>Sablon például JSON használatával 

Ez a példa tartalmazó [sablon regisztrációs](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) értesítést küld egy `message` helyőrző a sablon használatával JSON érvényes karakterlánc.

    using System;
     
    public static void Run(string myQueueItem,  out string notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
        notification = "{\"message\":\"Hello from C#. Processed a queue item!\"}";
    }


#### <a name="template-example-using-notification-hubs-library-types"></a>Sablon például értesítés hubok dokumentumtár-típus használatával

Ebben a példában a [Microsoft Azure értesítés hubok tár](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)definiált típusok használatához. 


    #r "Microsoft.Azure.NotificationHubs"

    using System;
    using System.Threading.Tasks;
    using Microsoft.Azure.NotificationHubs;
     
    public static void Run(string myQueueItem,  out Notification notification, TraceWriter log)
    {
       log.Info($"C# Queue trigger function processed: {myQueueItem}");
       notification = GetTemplateNotification(myQueueItem);
    }

    private static TemplateNotification GetTemplateNotification(string message)
    {
        Dictionary<string, string> templateProperties = new Dictionary<string, string>();
        templateProperties["message"] = message;
        return new TemplateNotification(templateProperties);
    }

## <a name="next-steps"></a>Következő lépések

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
