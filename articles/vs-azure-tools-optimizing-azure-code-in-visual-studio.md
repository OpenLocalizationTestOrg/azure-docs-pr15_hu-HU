<properties
   pageTitle="A Visual Studio Azure kód optimalizálása |} Microsoft Azure"
   description="Megtudhatja, hogyan Azure kód optimalizálási eszközök a Visual Studióban megtudhatja, hogy a kód, megbízhatóbb és jobban végrehajtása."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="optimizing-your-azure-code"></a>Optimalizálás a Azure kód

Microsoft Azure használó alkalmazások esetén programozási, amikor nincsenek coding eljárásokat az alkalmazás méretezhetőség, a működését és a teljesítmény felhőalapú környezetben kapcsolatos problémák elkerülése érdekében célszerű követnie. A Microsoft-Azure kód elemzőeszköz, felismeri és azonosítja számos gyakran észlelt problémák és megoldási lehetőséget nyújt segítséget nyújt. Letöltheti az eszköz a Visual Studióban NuGet keresztül.

## <a name="azure-code-analysis-rules"></a>Azure kód Analysis szabályok

A Azure kód Analysis használja a következő szabályok az automatikusan ikonra az Azure kódot, amikor teljesítményét érintő ismert problémákat talált. Észlelt problémák egy figyelmeztetések jelennek meg vagy fordító hibákat. Kód javítások és a figyelmeztetés vagy a hiba megoldásához javaslatok gyakran szolgáltatáson keresztül egy lámpa ikonra.

## <a name="avoid-using-default-in-process-session-state-mode"></a>Alapértelmezett (folyamaton) munkamenet-állapot módja használatának kerülése

### <a name="id"></a>AZONOSÍTÓ

AP0000

### <a name="description"></a>Leírás

Ha az alapértelmezett (folyamaton) munkamenet-állapot módja felhő alkalmazást használ, elvesznek a munkamenet-állapotot.

Ossza meg az ötletek és [Azure kód elemzése visszajelzését](http://go.microsoft.com/fwlink/?LinkId=403771)a visszajelzést.

### <a name="reason"></a>Oka

Alapértelmezés szerint ki a fájlt a megadott munkamenet-állapot módja a folyamatban. Is ha nincs a konfigurációs fájl a megadott bejegyzés, a munkamenet-állapot mód alapértelmezés szerint a folyamat. A folyamaton mód munkamenet-állapot memóriahasználat, a webkiszolgáló tárol. Ha egy példány újraindítása, vagy egy új példányát használt terheléselosztási vagy feladatátvevő támogatási, a munkamenet-állapot a memóriában a webes kiszolgálón tárolt nem menti a rendszer. Ez a helyzet megakadályozza, hogy az alkalmazás a felhő nem méretezhető.

ASP.NET-munkamenet állapot számos különböző tárolási lehetőségek támogatja a munkamenet-állapot adatai: InProc, StateServer, SQL Server, egyéni, és ki. Egyéni üzemmód adatokhoz host egy külső munkamenet-állapot mentése, például a [munkamenet-állapot Azure-szolgáltató az vgx.dll](http://go.microsoft.com/fwlink/?LinkId=401521)használata ajánlott.

### <a name="solution"></a>Megoldás

Egy javasolt megoldás, ha egy felügyelt gyorsítótár-szolgáltatás a munkamenet-állapot tárolja. Megtudhatja, hogy miként [Azure munkamenet-állapot-szolgáltató az vgx.dll](http://go.microsoft.com/fwlink/?LinkId=401521) használva a munkamenet állapotát. A munkamenet-állapot más helyekről annak biztosítására, az alkalmazás méretezhető a felhő is tárolhat. Ha többet szeretne tudni az alternatív megoldások olvassa el a [Munkamenet állam módban](https://msdn.microsoft.com/library/ms178586).

## <a name="run-method-should-not-be-async"></a>Futási mód nem kell aszinkron

### <a name="id"></a>AZONOSÍTÓ

AP1000

### <a name="description"></a>Leírás

A [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) módszer kívüli (például [kerülve](https://msdn.microsoft.com/library/hh156528.aspx)) aszinkron módszerek létrehozása, és kattintson az aszinkron módszerek hívhat [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx). Írja be az újraindítás ismétlődve dolgozó szerepkörét deklarálása a [[Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) módszer szerint aszinkron okoz.

Ossza meg az ötletek és [Azure kód elemzése visszajelzését](http://go.microsoft.com/fwlink/?LinkId=403771)a visszajelzést.

### <a name="reason"></a>Oka

Hívja fel a [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) módszer belül aszinkron módszerek hatására a felhőalapú szolgáltatás futtatási a dolgozó szerepkör Lomtár. Dolgozó szerepkörbe indításakor minden program végrehajtása megy végbe az [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) módszer belül. Kilépés a [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) módszer hatására a dolgozó szerepkör újraindításához. A dolgozó szerepkört futtatókörnyezet Találatok aszinkron módszer, ha kiszállítja az összes művelet után a aszinkron módszer, és adja vissza. Ennek hatására a [[[[Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) módszer onnan, és indítsa újra a dolgozó szerepkört. A következő közelítését végrehajtás a dolgozó szerepkör újra Találatok aszinkron módszer, és újraindul, újra is látható Lomtár dolgozó szerepkörét okoz.

### <a name="solution"></a>Megoldás

Helyezze a [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) módszer kívül az összes aszinkron művelet. Ezután hívja fel a refactored aszinkron módszer a [[Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) módszer, például RunAsync () .wait belül. Az Azure kód elemzőeszköz segítséget nyújtanak a probléma megoldásához.

A következő kódrészletet mutatja be, a kód fix erre a problémára:

```
public override void Run()
{
    RunAsync().Wait();
}

public async Task RunAsync()
{
    //Asynchronous operations code logic
    // This is a sample worker implementation. Replace with your logic.

    Trace.TraceInformation("WorkerRole1 entry point called");

    HttpClient client = new HttpClient();

    Task<string> urlString = client.GetStringAsync("http://msdn.microsoft.com");

    while (true)
    {
        Thread.Sleep(10000);
        Trace.TraceInformation("Working");

        string stream = await urlString;
    }

}
```

## <a name="use-service-bus-shared-access-signature-authentication"></a>A hitelesítési szolgáltatás Bus megosztott Access aláírás használata

### <a name="id"></a>AZONOSÍTÓ

AP2000

### <a name="description"></a>Leírás

Használhatja a megosztott Access-aláírás (Társítások) hitelesítéshez. Access-vezérlő szolgáltatást (ACS) a bus hitelesítéshez elavult alatt.

Ossza meg az ötletek és [Azure kód elemzése visszajelzését](http://go.microsoft.com/fwlink/?LinkId=403771)a visszajelzést.

### <a name="reason"></a>Oka

A nagyobb biztonság érdekében Azure Active Directory cseréje Társítások hitelesítéssel ACS hitelesítési. Lásd: az [Azure Active Directory nem ACS a tervek](http://blogs.technet.com/b/ad/archive/2013/06/22/azure-active-directory-is-the-future-of-acs.aspx) az áttűnés terv olvashat.

### <a name="solution"></a>Megoldás

Az alkalmazások használata Társítások hitelesítés. A következő példa bemutatja egy meglévő Társítások jogkivonat használata szolgáltatás bus névtere vagy entitás eléréséhez.

```
MessagingFactory listenMF = MessagingFactory.Create(endpoints, new StaticSASTokenProvider(subscriptionToken));
SubscriptionClient sc = listenMF.CreateSubscriptionClient(topicPath, subscriptionName);
BrokeredMessage receivedMessage = sc.Receive();
```

További információt a következő témakörökben talál.

- Áttekintéséhez tanulmányozza a [Megosztott Access aláírás hitelesítési szolgáltatás Bus](https://msdn.microsoft.com/library/dn170477.aspx) című témakört.

- [Szolgáltatás Bus megosztott hitelesítés aláírás használata](https://msdn.microsoft.com/library/dn205161.aspx)

- Minta projekt olvassa el a [használatával megosztott Access aláírás (Társítások) hitelesítési Bus előfizetéssel](http://code.msdn.microsoft.com/windowsazure/Using-Shared-Access-e605b37c) című témakört.

## <a name="consider-using-onmessage-method-to-avoid-receive-loop"></a>Fontolja meg inkább OnMessage módszer "hurok fogadása" elkerülése

### <a name="id"></a>AZONOSÍTÓ

AP2002

### <a name="description"></a>Leírás

Elkerülése érdekében jobb megoldás, mint a **Receive** metódus hívása üzenetek fogadására érkeznek "fogadása hurok", hívja fel a **OnMessage** módszer. Jó helyen jár Ha a **fogadás** módszert kell használnia, és megadhatja, hogy egy alapértelmezettől server várakozási idő, ellenőrizze, hogy a server várakozási idő egy percnél hosszabb.

Ossza meg az ötletek és [Azure kód elemzése visszajelzését](http://go.microsoft.com/fwlink/?LinkId=403771)a visszajelzést.

### <a name="reason"></a>Oka

Az ügyfél **OnMessage**hívásakor elindítja az egy belső üzenet szivattyú, amely folyamatosan lekérdezi a várólista vagy -előfizetésre. Ez az üzenet szivattyú üzeneteket fogadni a hívást alkotta végtelen ciklust tartalmazza. A hívás időtúllépés történik, ha azt egy új hívás hibák. Az időkorlát értékét az éppen használt [MessagingFactory](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactory.aspx) [így időtúllépés történt](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx) tulajdonsága határozza meg.

A használatának **OnMessage** **fogadási** összehasonlítva előnye, hogy a felhasználók manuálisan lekérdezik az üzenetek, kezelje a kivételek, folyamat párhuzamosan több üzenetet, és fejezze be az üzenetek nincs-e.

Ha **fogadási** nélkül, az alapértelmezett érték hív meg, ügyeljen arra, *ServerWaitTime* értéke nagyobb, mint egy perc alatt. *ServerWaitTime* beállítása több, mint egy perc megakadályozza, hogy a kiszolgáló időzítését mielőtt az üzenet jelenik meg teljes mértékben.

### <a name="solution"></a>Megoldás

Tanulmányozza az alábbi kód példák az ajánlott típusú helytelen szóhasználat. További információra kíváncsi olvassa el a [QueueClient.OnMessage módszer (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.onmessage.aspx)és [QueueClient.Receive módszer (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.receive.aspx)című témakört.

Az Azure üzenetben infrastruktúra a teljesítmény javítása érdekében olvassa el a tervezési minta [Aszinkron üzenetküldés alapozó](https://msdn.microsoft.com/library/dn589781.aspx).

Az alábbi képen **OnMessage** használó üzeneteket fogadni.

```
void ReceiveMessages()
{
    // Initialize message pump options.
    OnMessageOptions options = new OnMessageOptions();
    options.AutoComplete = true; // Indicates if the message-pump should call complete on messages after the callback has completed processing.
    options.MaxConcurrentCalls = 1; // Indicates the maximum number of concurrent calls to the callback the pump should initiate.
    options.ExceptionReceived += LogErrors; // Enables you to get notified of any errors encountered by the message pump.

    // Start receiving messages.
    QueueClient client = QueueClient.Create("myQueue");
    client.OnMessage((receivedMessage) => // Initiates the message pump and callback is invoked for each message that is recieved, calling close on the client will stop the pump.
    {
        // Process the message.
    }, options);
    Console.WriteLine("Press any key to exit.");
    Console.ReadKey();
```

Az alábbi képen az alapértelmezett server várakozási idő a **fogadás** használja.

```
string connectionString =  
CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

QueueClient Client =  
    QueueClient.CreateFromConnectionString(connectionString, "TestQueue");

while (true)  
{   
   BrokeredMessage message = Client.Receive();
   if (message != null)
   {
      try  
      {
         Console.WriteLine("Body: " + message.GetBody<string>());
         Console.WriteLine("MessageID: " + message.MessageId);
         Console.WriteLine("Test Property: " +  
            message.Properties["TestProperty"]);

         // Remove message from queue
         message.Complete();
      }

      catch (Exception)
      {
         // Indicate a problem, unlock message in queue
         message.Abandon();
      }
   }
```

Az alábbi képen egy alapértelmezettől server várakozási idő a **fogadás** használja.

```
while (true)  
{   
   BrokeredMessage message = Client.Receive(new TimeSpan(0,1,0));

   if (message != null)
   {
      try  
      {
         Console.WriteLine("Body: " + message.GetBody<string>());
         Console.WriteLine("MessageID: " + message.MessageId);
         Console.WriteLine("Test Property: " +  
            message.Properties["TestProperty"]);

         // Remove message from queue
         message.Complete();
      }

      catch (Exception)
      {
         // Indicate a problem, unlock message in queue
         message.Abandon();
      }
   }
}
```
## <a name="consider-using-asynchronous-service-bus-methods"></a>Fontolja meg inkább aszinkron szolgáltatás Bus módszerek

### <a name="id"></a>AZONOSÍTÓ

AP2003

### <a name="description"></a>Leírás

Aszinkron szolgáltatás Bus módszerek használata brokered csevegést a teljesítmény javítása érdekében.

Ossza meg az ötletek és [Azure kód elemzése visszajelzését](http://go.microsoft.com/fwlink/?LinkId=403771)a visszajelzést.

### <a name="reason"></a>Oka

Alkalmazás program feldolgozási aszinkron módszerekkel lehetővé teszi, mivel minden hívás végrehajtásakor nem blokkolja a fő szál. Szolgáltatás Bus módszerek üzenetküldés, műveletet használata esetén (küldése, fogadása, törlés, stb) időt vesz igénybe. Ez esetben a művelet feldolgozásával szolgáltatás Bus szolgáltatás mellett a késés a kérelem és a válasz is tartalmaz. Ha növelni szeretné az idő / műveletek számát, műveletek végre kell hajtania egyidejűleg. További információt találhat [Gyakorlati tanácsok a teljesítmény javítása használatával szolgáltatás Bus Brokered üzenetküldés](https://msdn.microsoft.com/library/azure/hh528527.aspx).

### <a name="solution"></a>Megoldás

Információ arról, hogy miként használhatja az ajánlott aszinkron módszer is találhat [QueueClient Class (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.aspx) .

Az Azure üzenetben infrastruktúra a teljesítmény javítása érdekében olvassa el a tervezési minta [Aszinkron üzenetküldés alapozó](https://msdn.microsoft.com/library/dn589781.aspx).

## <a name="consider-partitioning-service-bus-queues-and-topics"></a>Fontolja meg a particionáló szolgáltatás Bus sorban várakozó és témakörök

### <a name="id"></a>AZONOSÍTÓ

AP2004

### <a name="description"></a>Leírás

Partition szolgáltatás Bus sorban várakozó és a jobb teljesítmény szolgáltatás Bus csevegést témaköröket.

Ossza meg az ötletek és [Azure kód elemzése visszajelzését](http://go.microsoft.com/fwlink/?LinkId=403771)a visszajelzést.

### <a name="reason"></a>Oka

Szolgáltatás Bus sorban várakozó és témakörök szétválasztás növeli a teljesítmény átviteli és szolgáltatás elérhetőségét, mivel a particionált várólista vagy a témakör a teljes kapacitásának már nem korlátozza a egy adott üzenet közvetítő vagy üzenetben áruházból teljesítményét. Ezenkívül átmenetileg nem működik egy üzenetben áruház nem egy particionált várólista vagy teheti a témakör nem érhető el. További tudnivalókért lásd: a [Szétválasztás üzenetküldés szervezetek](https://msdn.microsoft.com/library/azure/dn520246.aspx).

### <a name="solution"></a>Megoldás

A következő kódrészletet üzenetben szervezetek partition mutatja.

```
// Create partitioned topic.
NamespaceManager ns = NamespaceManager.CreateFromConnectionString(myConnectionString);
TopicDescription td = new TopicDescription(TopicName);
td.EnablePartitioning = true;
ns.CreateTopic(td);
```

További tudnivalókért lásd: a [particionálva szolgáltatás Bus sorban várakozó és témakörök |} Microsoft Azure-Blog](https://azure.microsoft.com/blog/2013/10/29/partitioned-service-bus-queues-and-topics/) , és nézze meg a [Microsoft Azure Service Bus particionálva várólista](https://code.msdn.microsoft.com/windowsazure/Service-Bus-Partitioned-7dfd3f1f) minta.

## <a name="do-not-set-sharedaccessstarttime"></a>Nincs beállítva a SharedAccessStartTime

### <a name="id"></a>AZONOSÍTÓ

AP3001

### <a name="description"></a>Leírás

Kerülje a pontos idő a megosztott hozzáférési házirend azonnal indításához SharedAccessStartTimeset használatával. Csak kell beállítania a ezt a tulajdonságot, ha azt szeretné, később a megosztott hozzáférési házirend indításához.

Ossza meg az ötletek és [Azure kód elemzése visszajelzését](http://go.microsoft.com/fwlink/?LinkId=403771)a visszajelzést.

### <a name="reason"></a>Oka

Óra szinkronizálása hatására enyhe időkülönbség adatközpontokkal között. Például úgy szeretné logikailag gondolja, hogy a kezdési időpontot a tárolás Társítások házirend beállítása az aktuális idő DateTime.Now használatával, vagy hasonló módon okoz a Társítások házirend módosítása azonnal érvénybe lép. Azonban adatközpontokkal kis időt eltérések problémákat okozhat a mivel lehet, hogy bizonyos adatközponthoz időpontok kissé későbbi, mint a kezdési időpontot, míg mások előtt azt. Emiatt a Társítások házirend is jár le gyorsan (vagy akár közvetlenül) a házirend-élettartam túl rövid értékűre.

További megosztott Access-aláírás használatáról az Azure tárolón című témakörben olvashat [Bemutatása táblázat Társítások (megosztott Access-aláírás), várólista Társítások és Blob-Társítások - MSDN-blogok a Microsoft Azure tároló csapatának blogja – kezdőlap webhely - frissítéseket](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx).

### <a name="solution"></a>Megoldás

Távolítsa el a kimutatás, amely a kezdési időpontot, a megosztott hozzáférési házirend állítja be. Az Azure kód elemzőeszköz egy fix biztosít a probléma. Biztonsági kezelése lásd a tervezés minta [Valet kulcs mintát](https://msdn.microsoft.com/library/dn568102.aspx).

A következő kódrészletet mutatja be, a kód fix erre a problémára.

```
// The shared access policy provides  
// read/write access to the container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // To ensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
   SharedAccessExpiryTime = DateTime.UtcNow.AddHours(10),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

## <a name="shared-access-policy-expiry-time-must-be-more-than-five-minutes"></a>A megosztott hozzáférési házirend lejárati idő öt percnél hosszabb kell lennie.

### <a name="id"></a>AZONOSÍTÓ

AP3002

### <a name="description"></a>Leírás

Is lehetnek, mint amennyit egy öt perc az órákat között egy úgynevezett "óra ferdeség." feltétel miatt különböző helyeken adatközpontokkal eltérés A Társítások megakadályozására házirend jogkivonat ne járjon le tervezettnél korábbi beállítása a lejárati idő öt percnél hosszabb.

Ossza meg az ötletek és [Azure kód elemzése visszajelzését](http://go.microsoft.com/fwlink/?LinkId=403771)a visszajelzést.

### <a name="reason"></a>Oka

A világ különböző helyeken Adatközpontokkal szinkronizálása egy óra jel. Óra jel más helyekre utazási időt vesz igénybe, mivel lehet egy idő eltérése adatközpontokkal a különböző földrajzi helyek közötti Bár mindent miatt szinkronizálja a rendszer. Az időkülönbség hatással lehet a megosztott Access házirend kezdési idő és a lejárat időköz. Ezért annak érdekében, hogy megosztott hozzáférési házirend azonnal érvénybe lép, nem adja meg a kezdési időpontot. Ezeken kívül legyen a lejárati idő, hogy korai időtúllépés 5 percnél hosszabb.

Azure tároló megosztott Access-aláírás használatával kapcsolatos további tudnivalókért lásd: [Bemutatkozik a táblázat Társítások (megosztott Access-aláírás), várólista Társítások és Blob-Társítások - MSDN-blogok a Microsoft Azure tároló csapatának blogja – kezdőlap webhely - frissítéseket](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx).

### <a name="solution"></a>Megoldás

Biztonsági kezelése a további tudnivalókért lásd: a Tervező minta [Valet kulcs mintát](https://msdn.microsoft.com/library/dn568102.aspx).

Az alábbi képen a nem adja meg a megosztott hozzáférési házirend kezdés.

```
// The shared access policy provides  
// read/write access to the container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // To ensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
   SharedAccessExpiryTime = DateTime.UtcNow.AddHours(10),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

Az alábbi képen egy megosztott hozzáférési házirend kezdetének megadása a nagyobb, mint az 5 perc házirend lejárati időtartamot.

```
// The shared access policy provides  
// read/write access to the container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // To ensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
  SharedAccessStartTime = new DateTime(2014,1,20),   
 SharedAccessExpiryTime = new DateTime(2014, 1, 21),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

További tudnivalókért lásd: [létrehozása és használata az Access megosztott aláírás](https://msdn.microsoft.com/library/azure/jj721951.aspx).

## <a name="use-cloudconfigurationmanager"></a>CloudConfigurationManager használata

### <a name="id"></a>AZONOSÍTÓ

AP4000

### <a name="description"></a>Leírás

A [ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager(v=vs.110).aspx) osztály használatával projektekhez, például az Azure-webhely és Azure mobil szolgáltatások futtatókörnyezet problémák nem vezet be. Legjobb módszer azonban érdemes célszerű használni a felhőben[ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager(v=vs.110).aspx) egy egyesített mód, ha az összes Azure felhő alkalmazáshoz konfigurációk kezelése.

Ossza meg az ötletek és [Azure kód elemzése visszajelzését](http://go.microsoft.com/fwlink/?LinkId=403771)a visszajelzést.

### <a name="reason"></a>Oka

CloudConfigurationManager beolvassa a konfigurációs fájl az alkalmazás-környezet megfelelő.

[CloudConfigurationManager](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx)

### <a name="solution"></a>Megoldás

A kód használata az [CloudConfigurationManager osztály](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx)refactor. A kód javítás erre a problémára az Azure kód elemzőeszköz által biztosított.

A következő kódrészletet mutatja be, a kód fix erre a problémára. Csere

`var settings = ConfigurationManager.AppSettings["mySettings"];`

a

`var settings = CloudConfigurationManager.GetSetting("mySettings");`

Íme egy példa bemutatja, hogyan tárolja a konfigurációs beállítások App.config vagy Web.config fájl. A beállítások hozzáadása a konfigurációs fájl appSettings szakaszához. Az alábbiakban megtalálja az előző példa-a fájlt.

```
<appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="mySettings" value="[put_your_setting_here]"/>
  </appSettings>  
```

## <a name="avoid-using-hard-coded-connection-strings"></a>Csomagolásukkor a csatlakozási_karakterlánc használatának kerülése

### <a name="id"></a>AZONOSÍTÓ

AP4001

### <a name="description"></a>Leírás

Ha csomagolásukkor a csatlakozási_karakterlánc használja, és később frissíti kell, módosítsa a forráskód vagy fordítsa újra az alkalmazást, be kell. Jó helyen jár Ha a konfigurációs fájl tárolja a kapcsolati karakterláncot, módosíthatja őket később a konfigurációs fájl egyszerűen frissítésével.

Ossza meg az ötletek és [Azure kód elemzése visszajelzését](http://go.microsoft.com/fwlink/?LinkId=403771)a visszajelzést.

### <a name="reason"></a>Oka

Merevlemez kódolása a csatlakozási_karakterlánc oka az, a hibás gyakorlat mostantól problémák esetén gyorsan módosítani kell a csatlakozási_karakterlánc. Ezenkívül ha a projekt adatforrás-vezérlő ellenőrizni kívánt, csomagolásukkor a csatlakozási_karakterlánc mutassa be biztonsági rés, mivel a karakterláncok megtekintheti a forráskód.

### <a name="solution"></a>Megoldás

Kapcsolati karakterlánc tárolni a konfigurációs fájl vagy Azure környezetben.

- Különálló alkalmazásokhoz használva app.config csatlakozási karakterlánc beállításait.

- IIS által üzemeltetett webalkalmazások esetében használva web.config kapcsolati karakterláncot.

- ASP.NET vNext alkalmazásokhoz használva configuration.json kapcsolati karakterláncot.

Konfigurációk fájlokat, például web.config vagy app.config használatával kapcsolatos további tudnivalókért lásd [ASP.NET konfigurációs útmutatója](https://msdn.microsoft.com/library/vstudio/ff400235(v=vs.100).aspx). Hogyan Azure környezetben változók munkát információ [Azure webhelyek: hogyan alkalmazás karakterláncok és a kapcsolati karakterláncot munka](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/). A kapcsolati karakterlánc tárolása a verziókövetés további tudnivalókért lásd [elkerülése érdekében a bizalmas információkat, például a forrás kód adattárban tárolt fájlok kapcsolat karakterláncok üzembe](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control).

## <a name="use-diagnostics-configuration-file"></a>Diagnosztikai konfigurációs fájl használata

### <a name="id"></a>AZONOSÍTÓ

AP5000

### <a name="description"></a>Leírás

Helyett diagnosztika beállításainak konfigurálása a kód, mint például az API programozási Microsoft.WindowsAzure.Diagnostics használatával, a diagnostics.wadcfg fájlban konfigurálja a diagnosztika beállításainak. (Vagy ha Azure SDK 2,5 diagnostics.wadcfgx). Ezzel az eljárással módosíthatja diagnosztika beállításainak anélkül, hogy fordítani a kódot.

Ossza meg az ötletek és [Azure kód elemzése visszajelzését](http://go.microsoft.com/fwlink/?LinkId=403771)a visszajelzést.

### <a name="reason"></a>Oka

Mielőtt Azure SDK 2,5 (amely Azure diagnosztika 1.3 használ), az Azure diagnosztika (ÜVEGVATTA) számos különböző módszerekkel konfigurálható: feltétlenül szükséges kódot, deklaráció konfigurációs vagy az alapértelmezett beállítás használatával felveszi a konfigurációs blob a tároló területen. Azonban az előnyben részesített diagnosztika konfigurálása módja az XML konfigurációs fájl (diagnostics.wadcfg vagy diagnositcs.wadcfgx SDK 2.5-ös és újabb) az alkalmazás projektben. Ezt a megközelítést a diagnostics.wadcfg fájl teljesen határozza meg a konfiguráció és frissíthető és üzlethez címen lesz. A diagnostics.wadcfg konfigurációs fájl e keverése beállítás konfigurációk programozott módszerek a [DiagnosticMonitor](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.diagnosticmonitor.aspx)vagy [RoleInstanceDiagnosticManager](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.management.roleinstancediagnosticmanager.aspx)osztályok használatával fejetlenséget vezethet. [Initialize vagy módosítása Azure diagnosztika konfigurációs](https://msdn.microsoft.com/library/azure/hh411537.aspx) talál további információt.

ÜVEGVATTA 1.3-as (az Azure SDK 2,5 része) kezdődik, akkor már nem lehetséges diagnosztika konfigurálása a kód segítségével. Emiatt csak biztosíthat a konfiguráció alkalmazásával, vagy a diagnosztika bővítmény frissítésekor.

### <a name="solution"></a>Megoldás

Konfigurációs diagnosztika designerrel diagnosztikai beállítások áthelyezése a diagnosztika konfigurációs fájl (diagnositcs.wadcfg vagy diagnositcs.wadcfgx SDK 2.5-ös és újabb). Ajánlott, telepítse az [Azure SDK 2,5](http://go.microsoft.com/fwlink/?LinkId=513188) és a legújabb diagnosztika funkcióval is.

1. A konfigurálni kívánt szerepkört helyi menüben válassza a Tulajdonságok parancsot, és válassza a beállítás fülre.

1. A **diagnosztikai** csoportban győződjön meg arról, hogy a **Diagnosztika engedélyezése** jelölőnégyzet be van-e jelölve.

1. Válassza a **beállítás** gombra.

  ![A diagnosztikai engedélyezése beállítás elérése](./media/vs-azure-tools-optimizing-azure-code-in-visual-studio/IC796660.png)

  További információt talál [Az Azure Felhőszolgáltatások és a virtuális gépeken futó konfigurálása diagnosztika](vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md) .


## <a name="avoid-declaring-dbcontext-objects-as-static"></a>Statikus DbContext objektumokat deklarálása elkerülése

### <a name="id"></a>AZONOSÍTÓ

AP6000

### <a name="description"></a>Leírás

A memóriahasználat, ne a deklarálása statikus DBContext objektumokat.

Ossza meg az ötletek és [Azure kód elemzése visszajelzését](http://go.microsoft.com/fwlink/?LinkId=403771)a visszajelzést.

### <a name="reason"></a>Oka

DBContext objektumok tartsa lenyomva az ujját a lekérdezés eredményében a minden hívásból. Statikus DBContext objektumok nem értékesítik addig, amíg az alkalmazás tartomány betöltetlen. Emiatt a statikus DBContext objektumot is mobilalkalmazásokban nagy mennyiségű memóriát

### <a name="solution"></a>Megoldás

DBContext deklarálhatnak helyi változójának vagy nem statikus példány mező használata feladatok és majd várja meg a használat után kell értékesített.

A következő példa MVC vezérlő osztály használatát a DBContext objektum mutatja.

```
public class BlogsController : Controller
    {
        //BloggingContext is a subclass to DbContext        
        private BloggingContext db = new BloggingContext();
        // GET: Blogs
        public ActionResult Index()
        {
            //business logics…
            return View();
        }
        protected override void Dispose(bool disposing)
        {
            if (disposing)
            {
                db.Dispose();
            }
            base.Dispose(disposing);
        }
    }
```

## <a name="next-steps"></a>Következő lépések

Optimzing és Azure alkalmazások hibaelhárítási kapcsolatos további információért olvassa el a [webalkalmazást Azure App szolgáltatásban fellépő Visual Studio segítségével](./app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md)című témakört.
