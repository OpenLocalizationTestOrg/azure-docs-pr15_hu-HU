<properties
    pageTitle=".NET több szálon alkalmazás |} Microsoft Azure"
    description="A .NET oktatóprogram, amely segít a több szálon alkalmazást használó szolgáltatás Bus sorban várakozó rétegek közötti kommunikációt Azure-ban fejlesztése."
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="09/01/2016"
    ms.author="sethm"/>

# <a name="net-multi-tier-application-using-azure-service-bus-queues"></a>.NET több szálon alkalmazást Azure Service Bus sorban várakozó

## <a name="introduction"></a>– Bevezetés

Microsoft Azure fejlesztésének gyerekjáték a Visual Studio és a szabad Azure SDK használata a .NET rendszerhez. Ebben az oktatóanyagban végigvezeti a hozhat létre új alkalmazást, amely több Azure erőforrásokat a helyi környezetben futó használ. A lépések feltételezik, hogy nincs semmilyen előzetes használatában Azure.

Megtanulhatja, hogy a következőket:

-   Hogyan engedélyezhető az Azure fejlesztési egyetlen letöltése és telepítése a számítógépen.
-   Hogyan lehet az Azure fejlesztése Visual Studio segítségével.
-   Bemutatja, hogyan hozhat létre a több szálon-alkalmazások használata a webes és dolgozó szerepkörök Azure-ban.
-   Hogyan lehet rétegek használ szolgáltatási Bus sorok közötti kommunikációt.

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

Ebben az oktatóanyagban összeállítása és a több szálon alkalmazás futtatásához az Azure felhőszolgáltatásában fogja. Az előtér-ASP.NET MVC webes szerepkör lesz, és a háttér, amelyet használ szolgáltatási Bus várólista dolgozó-szerepkörbe. Az előtér telepíti az Azure webhely helyett egy felhőalapú szolgáltatásba, webes projektként hozhat létre ugyanezt a több szálon kérelmet. Mi az Azure webhely előtér a máshogyan kell elvégezni útmutatásért lásd: a [következő lépésekkel](#nextsteps) szakaszban. A [.NET-a-helyszíni és felhőbeli hibrid alkalmazás](../service-bus-relay/service-bus-dotnet-hybrid-app-using-service-bus-relay.md) oktatóprogram is próbálkozhat.

Az alábbi képernyőfelvételen látható a teljes alkalmazást.

![][0]

## <a name="scenario-overview-inter-role-communication"></a>Forgatókönyv áttekintése: a szerepkör közötti kommunikáció

Egy feldolgozási sorrendben elküldéséhez az előtér-felhasználói felület összetevő fut a webes szerepkört, a középső logika a dolgozó szerepkör futó kell használata. Ebben a példában a réteg közötti kommunikációhoz üzenetküldés brokered szolgáltatás Bus.

A két összetevő használata a webes és a középső név kezdőbetűjét rétegek között brokered üzenetküldés különválasztja. Közvetlen üzenetküldés (Ez azt jelenti, hogy a TCP- vagy a HTTP), szemben a webes réteg nem csatlakozik a középső közvetlenül; inkább azt verembe küldi munka, az erőforrás-mennyiség, az üzenetek szolgáltatás Bus, amely biztos, hogy megtartja őket mindaddig, amíg a középső felhasználása és feldolgozni azokat készen áll az.

Szolgáltatás Bus biztosít a támogatási brokered üzenetküldés két entitás: sorok és témaköröket. A sorok a sor küldött üzeneteket egy egyetlen címzett elfogyasztott mennyiség megjelenítésére. Témakörök, amelyben minden egyes közzétett üzenet elérhetővé válik a előfizetéséhez regisztrált a témakör a közzététel/előfizetés minta támogatja. Egyes előfizetések logikailag kezeli saját várólista üzenetek. Az előfizetések is beállítható úgy, hogy korlátozhatja az üzenetek át az előfizetés várólista azokat, amelyek megfelelnek a szűrő a szűrési szabályokkal. Az alábbi példában a szolgáltatás Bus sorban várakozó.

![][1]

A kommunikációt közvetlen üzenetküldés számos előnye van:

-   **Időbeli szétválasztás.** Az aszinkron üzenetben mintázattal gyártók és a fogyasztói nem kell online egy időben. Szolgáltatás Bus üzeneteket biztos, hogy tárolja, addig, amíg a igénybe fél kész fogadására őket. Az elosztott alkalmazás megszakítását, vagy önkéntes, például karbantartási, illetve egy összetevő összeomlik miatt anélkül, hogy a rendszer egészének érintő összetevői lehetővé teszi. Ezenkívül a igénybe alkalmazás előfordulhat, hogy csak kell bizonyos időszakokban napjának online állapotba.

-   **Töltse be a simítás.** Sok alkalmazásban rendszer betöltés változik az idő múlásával általában állandó szükséges munka minden egyes mértékegység feldolgozás idő állapotában. Intermediating üzenet gyártók és a fogyasztói várólista az azt jelenti, hogy, hogy a igénybe alkalmazást (dolgozó) csak kell kiterjesztve azt a maximális betöltése, hanem átlagos terhelés kell építenie. A várakozási sorban található mélységét megnő, és szerződést, a bejövő betöltés változik. Ez a pénzt értelmez infrastruktúra-szolgáltatási alkalmazás terhelés szükséges mennyiségét közvetlenül menti.

-   **Terheléselosztás.** Betöltése nő, mint további dolgozó folyamatok felvehetők a sorból olvasható. Minden üzenet a dolgozó folyamatok közül csak akkor dolgozza fel. Ezenkívül a leküldéses-alapú terheléselosztási lehetővé teszi, hogy a dolgozó gépek optimális használata, ha a dolgozó gépek különböznek egymástól értelmez feldolgozás power, szerint a saját maximális sebesség azok fog lekérés üzenetek. A minta gyakran adja meg a *fogyasztói gyorskorcsolyázásban* minta.

    ![][2]

Az alábbi szakaszokból megtudhatja, hogy a kód, amely a architektúra.

## <a name="set-up-the-development-environment"></a>A fejlesztői környezet beállítása

Mielőtt elkezdené Azure-alkalmazások fejlesztésével, eszközökhöz, és állítsa be a fejlesztői környezet.

1.  Telepítse az Azure SDK a .NET rendszerhez, [Eszközök és SDK csomagjában talál][].

2.  Kattintson a **telepítse a SDK** használja a Visual Studio verzióját. A lépéseket, ebben az oktatóanyagban Visual Studio 2015 használja.

4.  Amikor futtatni vagy mentené a telepítő kéri, kattintson a **Futtatás**parancsra.

5.  A **Webes Platform telepítő**kattintson a **telepítés** gombra, és a telepítés folytatásához.

6.  A telepítés befejezése után, lehetősége van a szolgáltatás fejlesztését az alkalmazás indításához szükséges. A SDK tartalmaz, amelyekkel egyszerűen a Visual Studio Azure alkalmazások fejlesztői eszközökkel. Ha nincs telepítve a Visual Studio, a SDK telepíti az ingyenes Visual Studio Express.

## <a name="create-a-namespace"></a>Névtér létrehozása

A következő lépésben, hogy szolgáltatás névtér létrehozása, szerezze be a megosztott Access-aláírás (Társítások) billentyűt. Névtér az alkalmazás oszlopazonosító nyújt a minden alkalmazás szolgáltatás Bus keresztül elérhetővé tett. Névtér létrehozásakor generálja a rendszer egy Társítások billentyűt. Névtér, ha Társítások kulcs itt szolgáltatás Bus hitelesítést végezni az access, az alkalmazás hitelesítő adatait.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="create-a-web-role"></a>Hozzon létre egy webes szerepkör

Ebben a részben generál az alkalmazás első oldala. Első lépésként hozzon létre a lapokat, az alkalmazás jeleníti meg.
Ezt követően adja hozzá a kódot, amely elküld egy szolgáltatás Bus várólista elemeket, és megjeleníti a sor állapot adatait.

### <a name="create-the-project"></a>A projekt létrehozása

1.  Rendszergazdai jogosultságokkal használatával indítsa el a Microsoft Visual Studio. Rendszergazdai jogosultságokkal a Visual Studio indításához kattintson a jobb gombbal a **Visual Studio** program ikonjára, és kattintson a **Futtatás rendszergazdaként**. Az Azure számítási irányító, ebben a cikkben ismertetett igényel, hogy a Visual Studio indítható el a rendszergazdai jogokkal rendelkező.

    A Visual Studióban, a **fájl** menüben kattintson az **Új**gombra, és kattintson a **Projekt**.

2.  **Telepített sablonok** **Visual C#**kattintson a **felhőben** , és válassza a **Azure Felhőszolgáltatásba**. A projekt **MultiTierApp**neve. Kattintson **az OK**gombra.

    ![][9]

3.  A **.NET-keretrendszer 4.5** szerepkörök kattintson duplán a **ASP.NET webes szerepkör**.

    ![][10]

4.  Vigye az egérmutatót fölé **WebRole1** a **Felhőszolgáltatásba Azure megoldást**, kattintson a ceruza ikonra, és nevezze át a webes szerepkör **FrontendWebRole**. Kattintson **az OK**gombra. (Győződjön meg arról, hogy a egy kisbetűk "e," nem "FrontEnd" a "Frontend" adja meg.)

    ![][11]

5.  **Új ASP.NET projekt** párbeszédpanelen a **sablon kiválasztása** listában kattintson a **MVC**elemre.

    ![][12]

6. Továbbra is a **Új ASP.NET** párbeszédpanel, kattintson a **Módosítás hitelesítési** gombra. **Hitelesítés módosítása** párbeszédpanelen válassza a **Nincs hitelesítés**, és kattintson **az OK**gombra. Ebben az oktatóprogramban az alkalmazás, nem egy felhasználónak bejelentkezés esetén üzembe helyezése.

    ![][16]

7. Vissza a **Új ASP.NET** párbeszédpanel, kattintson **az OK** gombra a projekt létrehozásához.

6.  **Megoldás Explorer**, a **FrontendWebRole** projektben kattintson a jobb gombbal a **hivatkozást**, majd **NuGet csomagok kezelése**gombra.

7.  Kattintson a **Tallózás** fülre, majd keressen `Microsoft Azure Service Bus`. Kattintson a **telepítés**gombra, és fogadja el a használati feltételeket.

    ![][13]

    Ne feledje, hogy a szükséges ügyfélprogram szerelvények most hivatkozunk néhány új kód fájlok van hozzáadva.

9.  A **Megoldás Explorer**kattintson a jobb gombbal a **modellek** kattintson a **Hozzáadás**gombra, majd kattintson az **Osztályjegyzetfüzet**. A **név** mezőbe írja be a nevét, **OnlineOrder.cs**. Kattintson a **Hozzáadás**gombra.

### <a name="write-the-code-for-your-web-role"></a>Írja be a kódot a webes szerepkör

Ebben a részben hoz létre, amely megjeleníti az alkalmazás a különböző oldalakat.

1.  Visual Studio OnlineOrder.cs fájlban cserélje ki a meglévő névtér meghatározása a következő kódot:

    ```
    namespace FrontendWebRole.Models
    {
        public class OnlineOrder
        {
            public string Customer { get; set; }
            public string Product { get; set; }
        }
    }
    ```

2.  Kattintson duplán a **Megoldást Explorer** **Controllers\HomeController.cs**. Adja hozzá a következő **használata** kimutatásokban a fájlt, amelyet fel szeretne venni a névtér az imént létrehozott, illetve szolgáltatás Bus modell tetején.

    ```
    using FrontendWebRole.Models;
    using Microsoft.ServiceBus.Messaging;
    using Microsoft.ServiceBus;
    ```

3.  Is Visual Studio HomeController.cs fájlban cserélje névtér definíciója a következő kódot. Ez a kód a beküldött elemek a sorba kezelésének módszerei tartalmazza.

    ```
    namespace FrontendWebRole.Controllers
    {
        public class HomeController : Controller
        {
            public ActionResult Index()
            {
                // Simply redirect to Submit, since Submit will serve as the
                // front page of this application.
                return RedirectToAction("Submit");
            }
    
            public ActionResult About()
            {
                return View();
            }
    
            // GET: /Home/Submit.
            // Controller method for a view you will create for the submission
            // form.
            public ActionResult Submit()
            {
                // Will put code for displaying queue message count here.
    
                return View();
            }
    
            // POST: /Home/Submit.
            // Controller method for handling submissions from the submission
            // form.
            [HttpPost]
            // Attribute to help prevent cross-site scripting attacks and
            // cross-site request forgery.  
            [ValidateAntiForgeryToken]
            public ActionResult Submit(OnlineOrder order)
            {
                if (ModelState.IsValid)
                {
                    // Will put code for submitting to queue here.
    
                    return RedirectToAction("Submit");
                }
                else
                {
                    return View(order);
                }
            }
        }
    }
    ```

4.  A **Szerkesztés** menüben kattintson a **Megoldás létrehozása** , amennyiben teszteléséhez munkája pontosságát.

5.  A nézet most létrehozása a `Submit()` korábban létrehozott módot. Kattintson a jobb gombbal a `Submit()` módszer (a túlterhelés a `Submit()` , amely bemutatja, hogy nincsenek paraméterek), és válassza a **Nézet hozzáadása**.

    ![][14]

6.  A nézet létrehozása megjelenik egy párbeszédpanel. A **sablon** listában válassza a **Létrehozás**lehetőséget. A **modell osztályához** listában kattintson a **OnlineOrder** osztály.

    ![][15]

7.  Kattintson a **hozzáadása**gombra.

8.  Most módosítsa az alkalmazás megjelenített nevét. A **Megoldás Intézőben**duplán a **Views\Shared\\_Layout.cshtml** kattintással nyissa meg a Visual Studio-szerkesztőben.

9.  **Saját ASP.NET-alkalmazás** minden előfordulását cserélje **BÁRDOS a termékek**.

10. A **Kezdőlap**, **kapcsolatos**és **partner** eltávolítása. A kiemelt kód törlése:

    ![][28]

11. Végül módosítsa a beküldési lapot, amelyet fel szeretne venni a várakozási sorban található információkat. A **Megoldás Explorer**, a **Views\Home\Submit.cshtml** fájlra duplán kattintva nyissa meg a Visual Studio-szerkesztőben. A hozzáadása után a következő sort `<h2>Submit</h2>`. Most a `ViewBag.MessageCount` üres. Később fogja adataival.

    ```
    <p>Current number of orders in queue waiting to be processed: @ViewBag.MessageCount</p>
    ```

12. A felhasználói felület most már végrehajtotta. Használhatja a futtassa az alkalmazást, és győződjön meg arról, hogy ez úgy néz ki vártnak **F5** billentyűkombinációt.

    ![][17]

### <a name="write-the-code-for-submitting-items-to-a-service-bus-queue"></a>Írja be a kódot szolgáltatás Bus várólista elemek küldéséhez

Most adja hozzá elemeket egy sorba elküldése kódját. Első lépésként hozzon létre egy osztály, amely a szolgáltatás Bus várólista kapcsolat adatait tartalmazza. Ezután inicializálni Global.aspx.cs a kapcsolatot. Végezetül frissítése a korábban létrehozott a beküldött kódot HomeController.cs ténylegesen küldhetik szolgáltatás Bus várólista elemek.

1.  **Megoldás Explorer**, a jobb gombbal a **FrontendWebRole** (kattintson a jobb gombbal a projektet, nem a szerepkör). Kattintson a **Hozzáadás**gombra, és kattintson az **Osztályjegyzetfüzet**.

2.  Az osztály **QueueConnector.cs**neve. Kattintson a **Hozzáadás** az osztály létrehozásához.

3.  Most vegye fel a kód, amely a kapcsolat adatait beágyazza és előkészíti a kapcsolatot egy szolgáltatás Bus sorba. QueueConnector.cs teljes tartalmának cserélje ki a következő kódot, és írja be az értékeket `your Service Bus namespace` (a névtér neve) és `yourKey`, amely olyan, az **elsődleges kulcs** az Azure portálról kapott.

    ```
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using Microsoft.ServiceBus.Messaging;
    using Microsoft.ServiceBus;
    
    namespace FrontendWebRole
    {
        public static class QueueConnector
        {
            // Thread-safe. Recommended that you cache rather than recreating it
            // on every request.
            public static QueueClient OrdersQueueClient;
    
            // Obtain these values from the portal.
            public const string Namespace = "your Service Bus namespace";
    
            // The name of your queue.
            public const string QueueName = "OrdersQueue";
    
            public static NamespaceManager CreateNamespaceManager()
            {
                // Create the namespace manager which gives you access to
                // management operations.
                var uri = ServiceBusEnvironment.CreateServiceUri(
                    "sb", Namespace, String.Empty);
                var tP = TokenProvider.CreateSharedAccessSignatureTokenProvider(
                    "RootManageSharedAccessKey", "yourKey");
                return new NamespaceManager(uri, tP);
            }
    
            public static void Initialize()
            {
                // Using Http to be friendly with outbound firewalls.
                ServiceBusEnvironment.SystemConnectivity.Mode =
                    ConnectivityMode.Http;
    
                // Create the namespace manager which gives you access to
                // management operations.
                var namespaceManager = CreateNamespaceManager();
    
                // Create the queue if it does not exist already.
                if (!namespaceManager.QueueExists(QueueName))
                {
                    namespaceManager.CreateQueue(QueueName);
                }
    
                // Get a client to the queue.
                var messagingFactory = MessagingFactory.Create(
                    namespaceManager.Address,
                    namespaceManager.Settings.TokenProvider);
                OrdersQueueClient = messagingFactory.CreateQueueClient(
                    "OrdersQueue");
            }
        }
    }
    ```

4.  Ezután győződjön meg arról, hogy a **inicializálni** mód kap-e hívni. Kattintson duplán a **Megoldást Explorer** **Global.asax\Global.asax.cs**.

5.  A következő sort a kód hozzáadása a **Application_Start** módszer végén.

    ```
    FrontendWebRole.QueueConnector.Initialize();
    ```

6.  Végül frissítse a korábban létrehozott, a várakozási sorban található elemek küldhetik webes kódját. Kattintson duplán a **Megoldást Explorer** **Controllers\HomeController.cs**.

7.  Frissítés a `Submit()` módszer (a nincs paraméterei túlterhelés) az alábbi képlettel történik, hogy az üzenet megszámlálása a várólista.

    ```
    public ActionResult Submit()
    {
        // Get a NamespaceManager which allows you to perform management and
        // diagnostic operations on your Service Bus queues.
        var namespaceManager = QueueConnector.CreateNamespaceManager();
    
        // Get the queue, and obtain the message count.
        var queue = namespaceManager.GetQueue(QueueConnector.QueueName);
        ViewBag.MessageCount = queue.MessageCount;
    
        return View();
    }
    ```

8.  Frissítés a `Submit(OnlineOrder order)` módszer (a egy paramétert túlterhelés) az alábbi képlettel történik, hogy továbbítsák a várólista rendelési adatokat.

    ```
    public ActionResult Submit(OnlineOrder order)
    {
        if (ModelState.IsValid)
        {
            // Create a message from the order.
            var message = new BrokeredMessage(order);
    
            // Submit the order.
            QueueConnector.OrdersQueueClient.Send(message);
            return RedirectToAction("Submit");
        }
        else
        {
            return View(order);
        }
    }
    ```

9.  Most már az alkalmazás ismételt futtathatja. Minden alkalommal, amikor a megrendelés leadásakor az üzenet száma növekszik.

    ![][18]

## <a name="create-the-worker-role"></a>A dolgozó szerepkör létrehozása

A sorrend beküldött elemek feldolgozásával dolgozó szerepkör most hoz létre. Ebben a példában a **Szolgáltatás Bus várólista dolgozó szerepkört** Visual Studio project-sablon. Már szerezte be a szükséges hitelesítő adatokat a portálon.

1. Ellenőrizze, hogy a Visual Studio kapcsolt az Azure-fiókjába.

2.  A Visual Studióban a **Megoldást Intézőben** kattintson a jobb gombbal a **MultiTierApp** projekt **szerepkörök** mappájában.

3.  Kattintson a **Hozzáadás**gombra, és kattintson az **Új dolgozó szerepkör projekt**. Megjelenik az **Új szerepkör projekt hozzáadása** párbeszédpanel.

    ![][26]

4.  **Új szerepkör projekt hozzáadása** párbeszédpanelen kattintson a **Szolgáltatás Bus várólista dolgozó szerepkört**.

    ![][23]

5.  A projekt **OrderProcessingRole**nevet a **név** mezőbe. Kattintson a **Hozzáadás**gombra.

6.  A kapcsolati karakterlánc, amely szerezte be az "A szolgáltatás Bus névtér létrehozása" szakasz a 9-es lépésben a vágólapra másolása

7.  A **Megoldás Explorer**, kattintson a jobb gombbal az 5 lépésben létrehozott **OrderProcessingRole** (Győződjön meg arról, hogy a jobb gombbal a **OrderProcessingRole** a **szerepkörök**, és nem az osztály). Válassza a **Tulajdonságok parancsot**.

8.  A **Tulajdonságok** párbeszédpanel **Beállítások** lapján kattintson az **érték** mezőbe a **Microsoft.ServiceBus.ConnectionString**, és illessze be a 6 lépésben a vágólapra másolt végpont értéket.

    ![][25]

9.  Hozzon létre egy **OnlineOrder** osztály a rendelések jelölik a sorból feldolgozása. A már létrehozott osztályjegyzetfüzet újra felhasználhatja. A **Megoldás Explorer**kattintson a jobb gombbal a **OrderProcessingRole** osztály (kattintson az osztályjegyzetfüzet ikonra, és nem a szerepkör a jobb gombbal). Kattintson a **Hozzáadás**gombra, majd kattintson a **Meglévő elemet**.

10. Tallózással keresse meg azt az almappát **FrontendWebRole\Models**, és kattintson duplán a **OnlineOrder.cs** ehhez a projekthez hozzáadni.

11. **WorkerRole.cs**, módosítsa a **Várólistanév** változó értékének `"ProcessingQueue"` való `"OrdersQueue"` ahogy a következő kódot.

    ```
    // The name of your queue.
    const string QueueName = "OrdersQueue";
    ```

12. Adja hozzá a következő utasítással WorkerRole.cs fájl tetején.

    ```
    using FrontendWebRole.Models;
    ```

13. Az a `Run()` működik, benne a `OnMessage()` hívás, tartalmának cseréje a `try` záradék a következő kódot.

    ```
    Trace.WriteLine("Processing", receivedMessage.SequenceNumber.ToString());
    // View the message as an OnlineOrder.
    OnlineOrder order = receivedMessage.GetBody<OnlineOrder>();
    Trace.WriteLine(order.Customer + ": " + order.Product, "ProcessingMessage");
    receivedMessage.Complete();
    ```

14. Az alkalmazás befejezése. A teljes alkalmazás tesztelheti a jobb gombbal a megoldást Explorer MultiTierApp projekt, **indítási projektként beállítása**kijelölése, nyomja le az F5 billentyűt. Figyelje meg, hogy az üzenet száma nem növeli, mert a dolgozó szerepkör dolgozza fel a sor-elemek és megjelölése befejezettként jelöli meg őket. A Kimenet nyomon követése a dolgozó szerepkör Azure kiszámítania irányító a felhasználói felület hatására megjelenik. Kattintson a jobb gombbal a irányító ikonjára a tálca értesítési területén, és válassza a **Kiszámítania irányító UI megjelenítése**ehhez.

    ![][19]

    ![][20]

## <a name="next-steps"></a>Következő lépések  

További tudnivalók a szolgáltatás Bus, az alábbi forrásokban talál:  

* [Azure Service Bus][sbmsdn]  
* [Szolgáltatás Bus szolgáltatás lapon][sbwacom]  
* [Szolgáltatás Bus sorban várakozó használata][sbwacomqhowto]  

További információ a több szálon esetek című témakörben talál:  

* [Tárterület-táblázatok, a sorok és BLOB .NET több szálon alkalmazás][mutitierstorage]  

  [0]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-01.png
  [1]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-100.png
  [2]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-101.png
  [Eszközök és SDK]: http://go.microsoft.com/fwlink/?LinkId=271920


  [GetSetting]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.getsetting.aspx
  [Microsoft.WindowsAzure.Configuration.CloudConfigurationManager]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx
  [NamespaceMananger]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx

  [QueueClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx

  [TopicClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx

  [EventHubClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.aspx

  [9]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-10.png
  [10]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-11.png
  [11]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-02.png
  [12]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-12.png
  [13]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-13.png
  [14]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-33.png
  [15]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-34.png
  [16]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-14.png
  [17]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-36.png
  [18]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-37.png

  [19]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-38.png
  [20]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-39.png
  [23]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBWorkerRole1.png
  [25]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBWorkerRoleProperties.png
  [26]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBNewWorkerRole.png
  [28]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-40.png

  [sbmsdn]: http://msdn.microsoft.com/library/azure/ee732537.aspx  
  [sbwacom]: /documentation/services/service-bus/  
  [sbwacomqhowto]: service-bus-dotnet-get-started-with-queues.md  
  [mutitierstorage]: https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36
  