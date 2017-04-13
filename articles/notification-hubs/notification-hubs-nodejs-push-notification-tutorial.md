<properties
    pageTitle="Leküldéses értesítések Azure értesítés hubok és Node.js küldése"
    description="Megtudhatja, hogy miként küldhet értesítést hubok leküldéses értesítéseket Node.js alkalmazásból."
    keywords="leküldéses értesítések leküldéses notifications,node.js leküldéses, ios leküldéses"
    services="notification-hubs"
    documentationCenter="nodejs"
    authors="ysxu"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="yuaxu"/>

# <a name="sending-push-notifications-with-azure-notification-hubs-and-nodejs"></a>Leküldéses értesítések Azure értesítés hubok és Node.js küldése
[AZURE.INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

##<a name="overview"></a>– Áttekintés

> [AZURE.IMPORTANT] Ebben az oktatóanyagban befejezéséhez, az aktív Azure fiókkal kell rendelkezniük. Nem rendelkeznek fiókkal, ha mindössze néhány perc is létrehozhat ingyenes próba-fiók. A részletekért lásd: [Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-nodejs-how-to-use-notification-hubs).

Ez az útmutató kell követnie az Azure értesítés hubok segítségével a leküldéses értesítések küldésére közvetlenül egy Node.js alkalmazásból. 

Az érintett esetek küld a leküldéses értesítések alkalmazásokat a következő platformokon:

* Android-alapú
* iOS
* Windows Phone
* Univerzális Windows platformra 

Értesítés hubok kapcsolatos további tudnivalókért című [Következő lépések](#next) .

##<a name="what-are-notification-hubs"></a>Mik azok a értesítés hubok?

Azure értesítés hubok a leküldéses értesítést küld a mobileszközök egy könnyen használható, több platformot, méretezhető infrastruktúra szükséges. A szolgáltatás infrastruktúra a részletekért olvassa el az [Azure értesítés hubok](http://msdn.microsoft.com/library/windowsazure/jj927170.aspx) lapon.

##<a name="create-a-nodejs-application"></a>Node.js-alkalmazás létrehozása

Az első lépés az oktatóprogram hoz létre egy új, üres Node.js alkalmazást. Egy Node.js-alkalmazás létrehozása, tanulmányozza [létrehozása és helyezhetnek üzembe a Node.js alkalmazások Azure webhelyére mutató][nodejswebsite], [Node.js Felhőszolgáltatásba] [ Node.js Cloud Service] használatáról a Windows PowerShell és [WebMatrix webhelyet].

##<a name="configure-your-application-to-use-notification-hubs"></a>Az alkalmazást, hogy az értesítési hubok konfigurálása

Azure értesítés hubok használata esetén szüksége letöltéséről és használatáról a Node.js [azure csomag](https://www.npmjs.com/package/azure), amelyek tartalmazzák még a segítő tárakról, hogy a leküldéses értesítést többi szolgáltatások kommunikálni egy beépített készletre.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>A csomag beszerzése csomópont csomag Manager (NPM) használatával

1.  A parancssor, például **PowerShell** (Windows), a **terminált** (Mac) vagy a **Bash** (Linux) használja, és nyissa meg azt a mappát, amelyen létrehozta az üres alkalmazást.

2.  A parancssorablakban írja a **npm telepítse az azure-sb** be.

3.  Manuális futtatását is lehetővé teszi annak ellenőrzésére, hogy a **ls** vagy **dir** parancs a **csomópont\_modulok** mappa létrehozásának. Egy adott mappába keresse meg az **azure** -csomagra, mely tartalmazza a el szeretné érni a értesítés központi tárakat.

>[AZURE.NOTE] Ismerje meg a hivatalos [NPM blog](http://blog.npmjs.org/post/85484771375/how-to-install-npm)NPM telepítéséről. 

### <a name="import-the-module"></a>A modul importálása

A **server.js** fájlt az alkalmazás tetején szövegszerkesztőben használ, vegye fel a következőt:

    var azure = require('azure');

### <a name="setup-an-azure-notification-hub-connection"></a>Az Azure értesítés központi kapcsolat beállítása

A **NotificationHubService** objektum lehetővé teszi a értesítés hubok használata. A következő kódot létrehoz egy **NotificationHubService** a nofication központi **hubname**nevű objektumot. Hozzá felső részén a **server.js** -fájl importálása az azure modul-utasítás után:

    var notificationHubService = azure.createNotificationHubService('hubname','connectionstring');

A kapcsolat **connectionstring** értéket szerezhető be az [Azure-portálon] az alábbi lépések végrehajtásával:

1. A bal oldali navigációs ablaktáblában kattintson a **Tallózás gombra**.

2. Jelölje ki a **Értesítés hubok**, és keresse meg a központi, hogy a minta használni kíván. Is hivatkozhat a [Windows áruház első lépések oktatóprogram](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) , ha egy új értesítés hubhoz segítséget nyújt.

3. Válassza a **Beállítások**.

4. Kattintson a **hozzáférési**. Mindkét megosztott és a teljes hozzáférés a csatlakozási_karakterlánc jelenik meg.

![Azure Portal - értesítés hubok](./media/notification-hubs-nodejs-how-to-use-notification-hubs/notification-hubs-portal.png)

> [AZURE.NOTE] A kapcsolati karakterláncot az [Azure parancssori kezelőfelületről Azure](../xplat-cli-install.md) [Azure PowerShell](../powershell-install-configure.md) vagy a az **azure sb névtér megjelenítése** parancs által biztosított **Get-AzureSbNamespace** parancsmaggal is meghallgathatja.

##<a name="general-architecture"></a>Általános architektúra

A **NotificationHubService** vezérlőnek küld a leküldéses értesítések bizonyos eszközöket és alkalmazásokat a következő objektum példányok:

* **Android** - az **GcmService** objektumra, amely **notificationHubService.gcm** a használata
* **iOS** - használja az **ApnsService** objektumra, amely **notificationHubService.apns** címen érhető el
* **Windows Phone** - az **MpnsService** objektumra, amely **notificationHubService.mpns** a használata
* **Univerzális Windows platformra** – az **WnsService** objektumra, amely **notificationHubService.wns** a használata

### <a name="how-to-send-push-notifications-to-android-applications"></a>Útmutató: küldés leküldéses értesítéseket, az Android-alkalmazáshoz

A **GcmService** objektum leküldéses értesítéseket küldeni az Android-alkalmazáshoz használható **küldése** módszert kínál. A **Küldés** módszer fogadja el a következő paraméterek:

* **Címkék** - címke azonosítója. Ha nincs címke van megadva, az értesítés al ügyfelek küld.
* **Tartalom** - az üzenet JSON vagy nyers karakterlánc tartalom.
* A **visszahívási** – a visszahívás függvény.

A tartalom formázása a további tudnivalókért olvassa el a a a [Végrehajtási GCM kiszolgálói](http://developer.android.com/google/gcm/server.html#payload) dokumentum **tartalom** szakaszát.

A következő kódot a **NotificationHubService** által elérhetővé tett **GcmService** példány használja a leküldéses értesítést küld az összes regisztrált ügyfeleknek.

    var payload = {
      data: {
        message: 'Hello!'
      }
    };
    notificationHubService.gcm.send(null, payload, function(error){
      if(!error){
        //notification sent
      }
    });

### <a name="how-to-send-push-notifications-to-ios-applications"></a>Útmutató: küldés leküldéses értesítéseket, az iOS-alkalmazáshoz

Ugyanaz, Android-alkalmazásokkal való használatára fentebb ismertetett,: a **ApnsService** objektum leküldéses értesítéseket küldeni az iOS-alkalmazáshoz használható **küldése** módszert kínál. A **Küldés** módszer fogadja el a következő paraméterek:

* **Címkék** - címke azonosítója. Ha nincs címke van megadva, az értesítés összes ügyfelet küld.
* **Tartalom** - az üzenet JSON vagy a karakterlánc-tartalom.
* A **visszahívási** – a visszahívás függvény.

További információ a tartalom formázása című **Értesítés tartalom** a [helyi és a leküldéses értesítést programozási útmutató a](http://developer.apple.com/library/ios/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/ApplePushService/ApplePushService.html) dokumentum.

A következő kódot a **NotificationHubService** által elérhetővé tett **ApnsService** példány egy figyelmeztető üzenet küldése az összes ügyfelet használja:

    var payload={
        alert: 'Hello!'
      };
    notificationHubService.apns.send(null, payload, function(error){
      if(!error){
        // notification sent
      }
    });

### <a name="how-to-send-push-notifications-to-windows-phone-applications"></a>Útmutató: küldés leküldéses értesítések Windows Phone-alkalmazások

A **MpnsService** objektum használt küldje el a leküldéses értesítések Windows Phone-alkalmazások **küldése** módszert kínál. A **Küldés** módszer fogadja el a következő paraméterek:

* **Címkék** - címke azonosítója. Ha nincs címke van megadva, az értesítés összes ügyfelet küld.
* **Tartalom** - XML-tartalom az üzenetet.
* **Cél_neve**  -  `toast` bejelentési értesítéseket. `token`a mozaik értesítések.
* **NotificationClass** - prioritás az értesítésre. Olvassa el a [leküldéses értesítések kiszolgálóról](http://msdn.microsoft.com/library/hh221551.aspx) dokumentumot az érvényes értékeket **HTTP Élőfejelemek** szakaszát.
* **Beállítások** – nem kötelező kérelem fejlécek.
* A **visszahívási** – a visszahívás függvény.

Érvényes **Cél_neve**, **NotificationClass** és fejléc beállítások listáját jelölje ki a [leküldéses értesítések kiszolgálóról](http://msdn.microsoft.com/library/hh221551.aspx) lapot.

A következő példa kódot leküldéses bejelentési értesítés küldése a **NotificationHubService** által elérhetővé tett **MpnsService** példány használja:

    var payload = '<?xml version="1.0" encoding="utf-8"?><wp:Notification xmlns:wp="WPNotification"><wp:Toast><wp:Text1>string</wp:Text1><wp:Text2>string</wp:Text2></wp:Toast></wp:Notification>';
    notificationHubService.mpns.send(null, payload, 'toast', 22, function(error){
      if(!error){
        //notification sent
      }
    });

### <a name="how-to-send-push-notifications-to-universal-windows-platform-uwp-applications"></a>Útmutató: küldés leküldéses értesítéseket univerzális Windows platformon (UWP) alkalmazások

A **WnsService** objektum leküldéses értesítéseket küldeni univerzális Windows platformra alkalmazások használható **küldése** módszert kínál.  A **Küldés** módszer fogadja el a következő paraméterek:

* **Címkék** - címke azonosítója. Ha nincs címke van megadva, az értesítés küld összes regisztrált ügyfeleknek.
* **Tartalom** - az XML-üzenet tartalom.
* **Típus** - értesítés típusát.
* **Beállítások** – nem kötelező kérelem fejlécek.
* A **visszahívási** – a visszahívás függvény.

Érvényes típusok és a kérelem fejlécek listáját lásd: a [leküldéses értesítést szolgáltatási kérelem és válasz fejlécek](http://msdn.microsoft.com/library/windows/apps/hh465435.aspx).

A következő kódot a **NotificationHubService** által elérhetővé tett **WnsService** példány bejelentési leküldéses értesítést küld a UWP-at használja:

    var payload = '<toast><visual><binding template="ToastText01"><text id="1">Hello!</text></binding></visual></toast>';
    notificationHubService.wns.send(null, payload , 'wns/toast', function(error){
      if(!error){
        // notification sent
      }
    });

## <a name="next-steps"></a>Következő lépések

A fenti minta kódrészletek könnyen hozhat létre a szolgáltatás infrastruktúra végzi a leküldéses értesítések számos különböző eszközök teszi lehetővé. Most, hogy már megtanulta értesítés hubok használata node.js alapjait, kövesse ezeket a hivatkozásokat, ha többet szeretne tudni, hogyan bővítheti a további e ezekre a lehetőségekre.

-   [Azure értesítés hubok](https://msdn.microsoft.com/library/azure/jj927170.aspx)is találhat az MSDN hivatkozás.
-   Keresse fel az [Azure SDK csomópont] tárházba GitHub további minták és végrehajtása részletek.

  [Azure SDK csomóponthoz]: https://github.com/WindowsAzure/azure-sdk-for-node
  [Next Steps]: #nextsteps
  [What are Service Bus Topics and Subscriptions?]: #what-are-service-bus-topics
  [Create a Service Namespace]: #create-a-service-namespace
  [Obtain the Default Management Credentials for the Namespace]: #obtain-default-credentials
  [Create a Node.js Application]: #Create_a_Nodejs_Application
  [Configure Your Application to Use Service Bus]: #Configure_Your_Application_to_Use_Service_Bus
  [How to: Create a Topic]: #How_to_Create_a_Topic
  [How to: Create Subscriptions]: #How_to_Create_Subscriptions
  [How to: Send Messages to a Topic]: #How_to_Send_Messages_to_a_Topic
  [How to: Receive Messages from a Subscription]: #How_to_Receive_Messages_from_a_Subscription
  [How to: Handle Application Crashes and Unreadable Messages]: #How_to_Handle_Application_Crashes_and_Unreadable_Messages
  [How to: Delete Topics and Subscriptions]: #How_to_Delete_Topics_and_Subscriptions
  [1]: #Next_Steps
  [Topic Concepts]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-topics-01.png
  [Azure Classic Portal]: http://manage.windowsazure.com
  [image]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-03.png
  [2]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-04.png
  [3]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-05.png
  [4]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-06.png
  [5]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-07.png
  [SqlFilter.SqlExpression]: http://msdn.microsoft.com/library/windowsazure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
  [Azure Service Bus Notification Hubs]: http://msdn.microsoft.com/library/windowsazure/jj927170.aspx
  [SqlFilter]: http://msdn.microsoft.com/library/windowsazure/microsoft.servicebus.messaging.sqlfilter.aspx
  [Webhelyek létrehozása WebMatrix]: /develop/nodejs/tutorials/web-site-with-webmatrix/
  [Node.js Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Previous Management Portal]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/previous-portal.png
  [nodejswebsite]: /develop/nodejs/tutorials/create-a-website-(mac)/
  [Node.js Cloud Service with Storage]: /develop/nodejs/tutorials/web-app-with-storage/
  [Node.js Web Application with Storage]: /develop/nodejs/tutorials/web-site-with-storage/
  [Azure portál]: https://portal.azure.com
