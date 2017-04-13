<properties 
    pageTitle="Mi az az Azure WebJobs SDK" 
    description="Az Azure WebJobs SDK bemutatása. Ebből a cikkből megtudhatja, mi a SDK csomagjában talál az, akkor hasznos, ha, és a minták kód tipikus helyzetekben." 
    services="app-service\web, storage" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/01/2016" 
    ms.author="tdykstra"/>

# <a name="what-is-the-azure-webjobs-sdk"></a>Mi az az Azure WebJobs SDK

## <a id="overview"></a>– Áttekintés

Ez a cikk bemutatja, hogy mi az a WebJobs SDK csomagjában talál, áttekinti a néhány gyakori alkalmazási területek esetében hasznos lehet, és hogyan használható a kódban áttekintést.

[WebJobs](websites-webjobs-resources.md) lehetővé teszi az Azure alkalmazás szolgáltatás, amely lehetővé teszi, hogy a program vagy parancsfájl futtatása web App alkalmazásban, API-alkalmazást vagy mobilalkalmazásban az azonos környezetben. A [WebJobs SDK](websites-webjobs-resources.md) célja, a kód írhat a gyakori feladatok, hogy egy WebJob is végezhet, például kép feldolgozás, várólista feldolgozása, RSS összesítési, fájl karbantartási és e-mailek küldése egyszerűsítése érdekében. A WebJobs SDK csomagjában talál Azure-tárhely és a szolgáltatás Bus kezelése, a feladatok ütemezése és kezelése a hibák és sok más tipikus esetei beépített funkciókat tartalmaz. Ezeken kívül célja bővíthető lesz. A [WebJobs SDK Megnyitás](https://github.com/Azure/azure-webjobs-sdk/), és egy [Nyissa meg a bővítmények forrás tárháza](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview).

A WebJobs SDK tartalmazza az alábbiakat:

* **NuGet csomagok**. A Visual Studio New project hozzáadni kívánt NuGet csomagok keretet használó a kód dekoráció WebJobs SDK attribútumokkal rendelkező a módszereket.
  
* **Az irányítópult**. A WebJobs SDK része Azure alkalmazás szolgáltatás, és multimédiás figyelés és diagnosztika nyújt a NuGet csomagok használó programok. Nem kell a figyelés és diagnosztika szolgáltatások használatának kódírás.

## <a id="scenarios"></a>Felhasználási területei

Íme néhány tipikus forgatókönyvet az Azure WebJobs SDK a könnyebben kezelhető:

* Kép: feldolgozás vagy más Processzor-igényes munka. A webhelyek közös funkció képes feltölteni a képeket vagy videókat. Gyakran szeretné módosítani a tartalmat, feltöltésük, de Ön nem szeretné, hogy a felhasználó várja meg, amíg az ehhez szükséges lépéseket.

* Várólista feldolgozása. Gyakori módja a webes időtúllépést kódmentes szolgáltatás kommunikáció sorban várakozó környezetbe. A webhely meg kell kapnia végzett munkát, ha egy alakzatot egy várólista üzenet verembe küldi. Kódmentes szolgáltatás gyűjti össze a várólista származó üzeneteket, és a munka nem. Sorok használhatja is kép feldolgozásra: például után a felhasználó egy számot a fájlok feltöltését, helyezi el a fájl nevét a kódmentes feldolgozásra általi felvételre várólista üzenetet. Vagy, ha sorban várakozó segítségével javítható a webhely növeléséhez. Például helyett közvetlenül egy SQL-adatbázis ír, írni egy sorban van a felhasználó már nincs szüksége, és mondja el szolgáltatás fogópont magas-késés relációs adatbázisszerű, kódmentes működik. Egy példa feldolgozása folyamatát, kép várólista című témakörben az [oktatóprogram WebJobs SDK kezdéshez](websites-dotnet-webjobs-sdk-get-started.md).

* Az RSS-összesítést. Ha egy webhelyet, amely az RSS-hírcsatornák összeállított egy tipplistát, akkor az összes a cikkek a háttér folyamatban adatcsatornák sikerült olvashat.

* A fájl karbantartás, például összesítése vagy naplófájlok törlése.  Lehet, hogy több webhelyek vagy annak érdekében, hogy a feladatok futtathatnak egyesítésben használni kívánt külön időtartamának az éppen létrehozott naplófájlok. Vagy lehet, hogy futtatni szeretné üríteni a régi naplófájlok heti feladat ütemezése.

* Bejövő adatok Azure táblákba. Előfordulhat, hogy tárolt fájlok és BLOB rendelkezik, és szeretné őket elemezni, és tárolja az adatokat a táblázatokban. A bejövő adatok függvény sikerült írása sok sort (bizonyos esetekben, több millió), és a WebJobs SDK lehetővé teszi, hogy ez a funkció egyszerűen végrehajtásához. A SDK is tartalmaz, például a táblázat írt sorok számát folyamatjelzők valós idejű ellenőrzése.

* Egyéb futtatása a háttérben egy szál, például [e-mailben elküldeni](https://github.com/victorhurdugaci/AzureWebJobsSamples/tree/master/SendEmailOnFailure)kívánt hosszabb ideig futó feladatokat. 

* Minden olyan tevékenységek szeretné futtatni időközönként, például minden éjszakai mentéséről művelet végrehajtása.

Sok fel az alábbiakat érdemes webalkalmazást több VMs, amely több WebJobs egyidejű futtatni szeretné futtatni szeretne méretezni. Bizonyos esetekben ez eredményezhet első feldolgozott többször ugyanazokat az adatokat, de ez nem probléma a beépített várólista blob és a WebJobs SDK szolgáltatás Bus indítók használatakor. A SDK biztosítja, hogy a függvények kell dolgozza csak egyszer az egyes üzenetek vagy blob.

A WebJobs SDK is egyszerűen esetek a gyakori hibakezelési kezelheti. Beállíthatja a riasztások függvény nem sikerült, és beállíthatja, hogy időtúllépései, hogy a függvény automatikusan megszakad, ha nem megadott határidőre egészíti ki az értesítések elküldéséhez.

## <a id="code"></a>Mintakódok

A leggyakoribb feladatok együttműködik az Azure-tároló kezelése kódját egyszerű. Az konzol alkalmazásban `Main` módszerrel hoz létre egy `JobHost` objektumra, amely a koordináták a hívások módszerek írhat. Az WebJobs SDK keretrendszer tudja, hogy mikor hívja fel a módokat, és mi paraméterértékeket használandó WebJobs SDK attribútumain alapuló őket a használhatja. A SDK *indítók* adja meg, hogy milyen feltételek okozó hívható függvény, és megadhatja, hogy milyen információk be- és kijelentkezés a metódus paraméterei *kötőanyagok* biztosít.

Például a [QueueTrigger](websites-dotnet-webjobs-sdk-storage-queues-how-to.md) attribútum hívható, amikor üzenet érkezik, a várólista, és ha az üzenet formátuma JSON egy bájt tömb vagy egy egyéni típus, egy üzenet meg automatikusan deszerializált függvény hibát okoz. A [BlobTrigger](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md) attribútum egy folyamat eseményindítók, amikor egy új blob Azure tároló fiók jön létre.

Íme egy egyszerű program, amely egy várólista lekérdezi hoz létre az egyes várólista kapott üzenet blob:

        public static void Main()
        {
            JobHost host = new JobHost();
            host.RunAndBlock();
        }

        public static void ProcessQueueMessage([QueueTrigger("webjobsqueue")] string inputText, 
            [Blob("containername/blobname")]TextWriter writer)
        {
            writer.WriteLine(inputText);
        }

A `JobHost` objektum a beállított háttérkép függvények tároló. A `JobHost` objektum funkciók, a velük kiváltó események órák figyeli és végrehajtja a függvény a kiváltó események végrehajtásakor. Hívja a `JobHost` használatával is jelezheti, hogy szeretné-e a tároló folyamat az aktuális szál vagy a háttérben egy szál futtatásához. A példában a `RunAndBlock` metódus futtatja a folyamat folyamatosan a jelenlegi szálon.

Mivel a `ProcessQueueMessage` metódus ebben a példában egy `QueueTrigger` attribútum, az eseményindító, a függvény nem várólista új üzenet fogadását. A `JobHost` objektum se nézze, amikor az új várólista üzenetek a megadott várólista (Ez a példa az "webjobsqueue" jelöli), és ha ilyet, hívja `ProcessQueueMessage`. 

A `QueueTrigger` kötések attribútum a `inputText` az várólista üzenet az érték paraméter. És a `Blob` kötések attribútum egy `TextWriter` "containername" nevű tároló "blobname" nevű blob-objektumot.  

        public static void ProcessQueueMessage([QueueTrigger("webjobsqueue")]] string inputText, 
            [Blob("containername/blobname")]TextWriter writer)

A függvény ezek a paraméterek majd használja a várólista üzenet értékének írni a blob:

        writer.WriteLine(inputText);

Az eseményindító boríték és funkciók WebJobs SDK nagyban leegyszerűsíti a kódot kell írni. A szükséges sorok, BLOB vagy fájlok feldolgozása, vagy ütemezett tevékenységek, kezdeményezhet követő kódot a WebJobs SDK keretrendszer történik meg. Például keretében létrehozza, amelyek nem szerepelnek még, megnyílik a várólista olvasás várólista üzeneteket, törli várólista üzeneteket, amikor feldolgozás befejeződik, hoz létre, amelyek nem szerepelnek blob-tárolók még, BLOB, és így tovább ír.

Az alábbi példa mutatja egy WebJob indítók számos: `QueueTrigger`, `FileTrigger`, `WebHookTrigger`, és `ErrorTrigger`. 

```
    public class Functions
    {
        public static void ProcessQueueMessage([QueueTrigger("queue")] string message,
        TextWriter log)
        {
            log.WriteLine(message);
        }

        public static void ProcessFileAndUploadToBlob(
            [FileTrigger(@"import\{name}", "*.*", autoDelete: true)] Stream file,
            [Blob(@"processed/{name}", FileAccess.Write)] Stream output,
            string name,
            TextWriter log)
        {
            output = file;
            file.Close();
            log.WriteLine(string.Format("Processed input file '{0}'!", name));
        }

        [Singleton]
        public static void ProcessWebHookA([WebHookTrigger] string body, TextWriter log)
        {
            log.WriteLine(string.Format("WebHookA invoked! Body: {0}", body));
        }

        public static void ProcessGitHubWebHook([WebHookTrigger] string body, TextWriter log)
        {
            dynamic issueEvent = JObject.Parse(body);
            log.WriteLine(string.Format("GitHub WebHook invoked! ('{0}', '{1}')",
                issueEvent.issue.title, issueEvent.action));
        }

        public static void ErrorMonitor(
        [ErrorTrigger("00:01:00", 1)] TraceFilter filter, TextWriter log,
        [SendGrid(
            To = "admin@emailaddress.com",
            Subject = "Error!")]
         SendGridMessage message)
        {
            // log last 5 detailed errors to the Dashboard
            log.WriteLine(filter.GetDetailedMessage(5));
            message.Text = filter.GetDetailedMessage(1);
        }
    }
```

## <a id="schedule"></a>Ütemezés

A `TimerTrigger` attribútum lehetővé teszi az eseményindító funkciók időközönként futtatásához. Azure vagy az ütemezés egyes funkciók a WebJobs SDK használatával egy WebJob keresztül egészének ütemezhet egy WebJob `TimerTrigger`. Az alábbiakban a minta kódot.

```
public class Functions
{
    public static void ProcessTimer([TimerTrigger("*/15 * * * * *", RunOnStartup = true)]
    TimerInfo info, [Queue("queue")] out string message)
    {
        message = info.FormatNextOccurrences(1);
    }
}
```

A további példakódot GitHub.com [TimerSamples.cs](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/ExtensionsSample/Samples/TimerSamples.cs) az azure-webjobs-sdk-bővítmények adattárban kapcsolatban.

## <a name="extensibility"></a>Bővítési

Ön nem korlátozódik beépített funkciók – a WebJobs SDK lehetővé teszi, hogy szeretne írni, egyéni eseményindítók és kötőanyagok.  Ha például gyorsítótár-események és periodikus ütemezést eseményindítók is írhat. Egy [Nyissa meg a forrás tárházba](https://github.com/Azure/azure-webjobs-sdk-extensions) való használatának megkezdéséhez, a saját eseményindítók és kötőanyagok írása [WebJobs SDK bővítési részletes útmutató,](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview) és a minta kódot tartalmazza.

## <a id="workerrole"></a>A WebJobs SDK kívüli WebJobs használatával

A program, amelyet használ a a WebJobs SDK csomagjában talál egy szabványos konzol alkalmazás, és futtatását is lehetővé teszi tetszőleges helyre – a futtathat egy WebJob nincsenek. Helyi meghajtóra tesztelheti a a program a fejlesztés a számítógépen, és a gyártási futtathatja azt egy felhőalapú szolgáltatásba dolgozó szerepkör vagy Windows-szolgáltatás Ha inkább egy adott környezetben. 

Az irányítópult azonban csak egy Azure alkalmazás szolgáltatás webalkalmazás kiterjesztése érhető el. Szeretne egy WebJob kívüli futtatása és az irányítópult továbbra is használható, ha beállíthatja az azonos tárterület-fiók, amelyre hivatkozik a WebJobs SDK irányítópult kapcsolati karakterláncot, és hogy web app WebJobs irányítópult fog majd adatokat jelenítenek meg a programból, máshová futtató függvény végrehajtása webalkalmazást. Az URL-cím https://*{webappname}*.scm.azurewebsites.net/azurejobs/#/functions használatával érheti el az irányítópulton. További tudnivalókért lásd: az [első helyi fejlesztési a WebJobs SDK csomagjában talál az irányítópult](http://blogs.msdn.com/b/jmstall/archive/2014/01/27/getting-a-dashboard-for-local-development-with-the-webjobs-sdk.aspx), de ne feledje, hogy a blogbejegyzés látható egy régi csatlakozási karakterlánc nevét. 

## <a id="nostorage"></a>Irányítópult-szolgáltatások

A WebJobs SDK előnyökkel jár több még akkor is, ha Ön nem használja az WebJobs SDK indítók vagy kötőanyagok:

* Az irányítópult függvények hívhatók.
* Az irányítópult függvények is ismétlődő lejátszásra van állítva.
* Az irányítópult által generált őket egy függvényben meghívását csatolt vagy az adott WebJob (alkalmazás a naplók írt használatával Console.Out, Console.Error, nyomkövetési, stb.) kapcsolódó naplók megtekintése (naplók használatával írt egy `TextWriter` objektumra, amely a SDK átadja a függvénynek paraméterként). 

További tudnivalókért lásd: [hogyan manuálisan indítsa függvény](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#manual) és a [hatékony Szövegalkotás naplók](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#logs) 

## <a id="nextsteps"></a>Következő lépések

A WebJobs SDK csomagjában talál további információkért olvassa el a [Azure WebJobs ajánlott erőforrások](http://go.microsoft.com/fwlink/?linkid=390226)című témakört.

A legújabb továbbfejlesztett a WebJobs SDK csomagjában talál információt olvassa el a [Kibocsátási megjegyzések](https://github.com/Azure/azure-webjobs-sdk/wiki/Release-Notes)című témakört.
 
