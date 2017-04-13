<properties
 pageTitle="A Feladatütemező fogalmak, a kifejezések és a szervezetek |} Microsoft Azure"
 description="Azure ütemező fogalmak, kifejezések és entitás hierarchián belül, beleértve a feladatok és a feladat gyűjtemények.  A teljes ütemezett feladat példa látható."
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="get-started-article"
 ms.date="08/18/2016"
 ms.author="deli"/>

# <a name="scheduler-concepts-terminology--entity-hierarchy"></a>A Feladatütemező fogalmak, terminológia + entitás hierarchia

## <a name="scheduler-entity-hierarchy"></a>Ütemező személy hierarchia

Az alábbi táblázat ismerteti a fő erőforrások elérhetővé tett vagy a Feladatütemező API által használt:

|Erőforrás | Leírás |
|---|---|
|**Feladat gyűjtemény**|Egy feladat webhelycsoport feladatok csoport tartalmaz, és beállításai, a kvóták és a feladatok webhelycsoporton belüli által megosztott szabályozás kezeli. Egy feladat webhelycsoport egy előfizetés tulajdonosa hozza létre, és a csoportok feladatok kihasználtsága vagy az alkalmazás határai közös alapján. Van egy régió korlátozódik. Azt is lehetővé teszi, hogy kvóták korlátozhatja a használatát, hogy a webhelycsoport összes feladat végrehajtását. A kvóták MaxJobs és MaxRecurrence tartalmazza.|
|**Feladat**|A feladat egyetlen ismétlődő művelet végrehajtása egyszerű vagy összetett stratégiák és határozza meg. Műveletek a HTTP, tároló várólista, szolgáltatás bus várólista vagy bus témakör szolgáltatáskérések közé.|
|**Korábbi**|Egy korábbi helyén egy feladat végrehajtását részletes adatait. Sikeres és sikertelen, valamint a bármely válasz adatai összehasonlítása tartalmaz.|

## <a name="scheduler-entity-management"></a>Ütemező személy kezelése

Magas szintű az ütemező és a Szolgáltatáskezelés API jelenítik meg az erőforrások az alábbi műveleteket:

|Lehetőség|Leírás és URI-cím|
|---|---|
|**A webhelycsoport feladatkezelés**|JUTHAT, HELYEZI, és törölje a létrehozó és módosító felhasználói feladat webhelycsoportok és a bennük található feladatok támogatása. Egy feladat gyűjteménye tároló feladatok és térképek kvótáinak és közös beállításokat. Kvótákat, később ismertetett példák feladatok és a legkisebb ismétlődési gyakoriságát maximális száma. <p>HELYEZI, és törlése:`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}`</p><p>JELENIK MEG:`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}`</p>
|**Feladatkezelés**|JUTHAT, HELYEZZE, közzé, javítása és törlése a létrehozó és módosító felhasználói feladatok támogatása. Az összes feladat, amely már létezik, feladat gyűjtemény szándékos, nincs implicit létrehozás kell tartoznia. <p>`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}/jobs/{jobName}`</p>|
|**Előzmények feladatkezelés**|Technikai támogatás kérése az adatvégrehajtás korábbi, például a feladat eltelt idő és a feladat-végrehajtás eredmények 60 nap beolvasható. Összeadja a lekérdezési karakterlánc paraméter támogatása szűrési állapot alapján. <P>`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}/jobs/{jobName}/history`</p>|

## <a name="job-types"></a>Projekt típusa

Feladatok több típusba sorolhatók: HTTP feladatok (beleértve a HTTPS-feladatok, amely támogatja az SSL), a tárhely várólistás feladatok, a szolgáltatás bus várólistás feladatok és a szolgáltatás bus tematikus feladatot. HTTP-feladatok ideálisak, ha egy meglévő terhelést vagy szolgáltatás zárólap van. Tárterület várólistás feladatok tehet közzé tároló sorok, üzenetek, így ezeket a feladatokat tároló sorban várakozó használó munkaterhelésekből ideális is használhatja. Hasonlóképpen szolgáltatás bus feladatok ideálisak a munkaterhelésekből szolgáltatás bus sorban várakozó és témaköröket.

## <a name="the-job-entity-in-detail"></a>A "feladat" entitás részletesen

Ütemezett feladat alapszinten, több részből foglalja magában:

- Ha akkor indul el, a feladat időzítő végrehajtandó művelet  

- (Nem kötelező) A time to a feladat futtatása  

- (Nem kötelező) Mikor és hogyan gyakran ismételje meg a feladat  

- (Nem kötelező) Minden olyan esetben, ha az elsődleges művelet sikertelen művelet  

Belső ütemezett feladat adatokat is tartalmaz, rendszer által biztosított például a következő ütemezett végrehajtási ideje.

A következő kódot biztosít a teljes ütemezett feladat példa. Részletek későbbi szakaszában találhatók.

    {
        "startTime": "2012-08-04T00:00Z",               // optional
        "action":
        {
            "type": "http",
            "retryPolicy": { "retryType":"none" },
            "request":
            {
                "uri": "http://contoso.com/foo",        // required
                "method": "PUT",                        // required
                "body": "Posting from a timer",         // optional
                "headers":                              // optional

                {
                    "Content-Type": "application/json"
                },
            },
           "errorAction":
           {
               "type": "http",
               "request":
               {
                   "uri": "http://contoso.com/notifyError",
                   "method": "POST",
               },
           },
        },
        "recurrence":                                   // optional
        {
            "frequency": "week",                        // can be "year" "month" "day" "week" "minute"
            "interval": 1,                              // optional, how often to fire (default to 1)
            "schedule":                                 // optional (advanced scheduling specifics)
            {
                "weekDays": ["monday", "wednesday", "friday"],
                "hours": [10, 22]
            },
            "count": 10,                                 // optional (default to recur infinitely)
            "endTime": "2012-11-04",                     // optional (default to recur infinitely)
        },
        "state": "disabled",                           // enabled or disabled
        "status":                                       // controlled by Scheduler service
        {
            "lastExecutionTime": "2007-03-01T13:00:00Z",
            "nextExecutionTime": "2007-03-01T14:00:00Z ",
            "executionCount": 3,
                                                "failureCount": 0,
                                                "faultedCount": 0
        },
    }

A fenti minta ütemezett feladat naplójában egy feladat definíciója részekből áll több:

- Kezdés ("kezdő")  

- Hiba műveletet ("errorAction") tartalmazó műveletet ("művelet")

- Ismétlődés ("ismétlődés")  

- Állapot ("állapot")  

- Állapot ("állapot")  

- Ismételje meg a házirend ("retryPolicy")  

Érdemes megvizsgálni mindegyike részletesen:

## <a name="starttime"></a>Kezdés időpontja

A "kezdő" a kezdési időpontot, és lehetővé teszi a felhasználóknak, hogy egy időzóna eltolás adja meg a vezetékes [ISO-8601](http://en.wikipedia.org/wiki/ISO_8601)formátumban.

## <a name="action-and-erroraction"></a>művelet és errorAction

A "művelet" a hívjon meg minden előfordulást műveletével, és egy adott típusú szolgáltatás meghívása ismerteti. A művelet pedig a megadott időközönként mit fog futtatható. Ütemező támogatja a HTTP, tároló várólista, szolgáltatás bus témakör és szolgáltatás bus várólista műveletek.

A művelet a fenti példában egy olyan HTTP-művelet. Az alábbi képen egy tároló várólista műveletet:

    {
            "type": "storageQueue",
            "queueMessage":
            {
                "storageAccount": "myStorageAccount",  // required
                "queueName": "myqueue",                // required
                "sasToken": "TOKEN",                   // required
                "message":                             // required
                    "My message body",
            },
    }

Az alábbi képen egy példa egy szolgáltatási bus témakör művelet.

  "művelet": {"típus": "serviceBusTopic", "serviceBusTopicMessage": {"topicPath": "t1",  
      "névtér": "mySBNamespace", "transportType": "netMessaging", / / lehet netMessaging vagy AMQP "hitelesítés": {"sasKeyName": "QPolicy", "típus": "sharedAccessKey"}, "üzenet": "Üzenet", "brokeredMessageProperties": {}, "customMessageProperties": {"alkalmazásnév": "FromScheduler"}},}

Az alábbi képen egy szolgáltatási bus várólista művelet példa:


  "művelet": {"serviceBusQueueMessage": {"Várólistanév": 1. "kérdés",  
      "névtér": "mySBNamespace", "transportType": "netMessaging", / / lehet netMessaging vagy AMQP "hitelesítés": {  
        "sasKeyName": "QPolicy", "típus": "sharedAccessKey"}, "üzenet": "Üzenet",  
      "brokeredMessageProperties": {}, "customMessageProperties": {"alkalmazásnév": "FromScheduler"}}, "típus": "serviceBusQueue"}

A "errorAction" a meghívott, ha az elsődleges művelet nem sikerül a művelet hibakezelő. Ez a változó hibakezelő zárólap hívás vagy felhasználói értesítés küldése is használhatja. Egy másodlagos végpontot abban az esetben, hogy az elsődleges nem érhető el (például az esetében a végpont webhelyén katasztrófa) alkalmazáson keresztül is használható, vagy a értesítésével arról, hogy egy hibakezelési végpont használhatók. Az elsődleges műveletet, hasonlóan a művelet a hiba esetén lehet egyszerű vagy összetett logika egyéb műveletek alapján. Megtudhatja, hogy miként hozhat létre a biztonsági jogkivonat, olvassa el a hozhat [létre és használhat megosztott Access aláírás](https://msdn.microsoft.com/library/azure/jj721951.aspx).

## <a name="recurrence"></a>Ismétlődés

Ismétlődés több részből áll:

- Gyakoriság: A perc, óra, nap, hét, hónap, év egyik  

- Időköz: Az ismétlődés az adott gyakorisággal intervallum  

- Előírt ütemezés: Adja meg, perc, óra, a hétköznapokat, hónapok és az ismétlődés monthdays  

- Száma: Előfordulásainak száma  

- Záró időpont: nincs feladatok hajtja végre a megadott befejezési idő után  

Ha egy ismétlődő objektum JSON definíciója megadott egy feladat ismétlődő. Ha nincs megadva darab és a Befejezés időpontja, a befejezési szabályt, amely akkor következik be, először fogadja van.

## <a name="state"></a>állam

A feladat állapota négy értékek egyike: engedélyezve, letiltva, Befejezve vagy hibás. ELHELYEZHETI vagy az engedélyezett vagy letiltott állapotba frissíteni őket, hogy a javítás feladatokat. Ha a feladat befejezése, vagy a hibás, azaz olyan végleges állapot, amely nem frissülnek (azonban továbbra is törölheti a feladatot). Példa a state tulajdonság értéke az alábbi képlettel történik:


        "state": "disabled", // enabled, disabled, completed, or faulted
Befejezett és hibás feladatok 60 nap után törlődnek.

## <a name="status"></a>állapot

Miután egy ütemezési feladat elindult, információk függvény a feladat aktuális állapotáról ad vissza. Az objektum nem állítható be a felhasználó által – a rendszer által van beállítva. Jó helyen jár azt is tartalmazni fogja a feladat objektum (nem pedig egy külön csatolt erőforrás), hogy egy könnyen beszerzése a feladat állapotát.

Feladat állapotának tartalmaz, az idő az előző végrehajtás (ha van ilyen), a következő ütemezett végrehajtása (a folyamatban lévő feladatok), és a feladat végrehajtását számát.

## <a name="retrypolicy"></a>retryPolicy

Ha nem sikerül egy ütemezési feladat, akkor lehet határozza meg, a művelet megismétlése, és hogy hogyan újrapróbálkozási házirend megadásához. Ez határozza meg a **retryType** objektum – **akkor nincs** értékre van állítva egy újrapróbálkozási házirend sem, ha felett látható módon. Állítsa be **rögzített** újrapróbálkozási házirend esetén.

Ismét házirendjének két további beállításokat kell megadni: a kísérletek időköze (**retryInterval**) és az (**retryCount**) ismétlések számát.

Az Ismét gombra a **retryInterval** objektummal megadott időközönként., azaz a újrapróbálkozások között. Az alapértelmezett érték 30 másodperc minimális konfigurálható értéke 15 másodperc és a maximális értéke a 18 hónap. Ingyenes feladat gyűjtemények feladatok van konfigurálható minimális érték 1 óra értéket.  Azt határozza meg az ISO 8601 formátumban. Hasonlóképpen az ismétlések számát értékének van megadva, az **retryCount** objektummal egy újrapróbálkozási pró ismétlődésének száma. Az alapértelmezett érték 4, és ennek maximális értéke 20\. mindkét **retryInterval** és **retryCount** nem kötelező megadni. Az alapértelmezett **rögzített** értéke **retryType** és nem értékek explicit módon megadva felsorolásukat.

## <a name="see-also"></a>Lásd még:

 [Mi az a Feladatütemező?](scheduler-intro.md)

 [Az Azure-portálon a Feladatütemező használatának első lépései](scheduler-get-started-portal.md)

 [Tervek és a számlázásra az Azure ütemező](scheduler-plans-billing.md)

 [Összetett ütemterveket és a Azure ütemezővel speciális ismétlődés létrehozásának](scheduler-advanced-complexity.md)

 [Azure ütemező REST API-hivatkozás](https://msdn.microsoft.com/library/mt629143)

 [Azure ütemező PowerShell-parancsmagok hivatkozás](scheduler-powershell-reference.md)

 [Azure ütemező magas rendelkezésre állás és megbízhatóság](scheduler-high-availability-reliability.md)

 [Azure ütemező korlátozások, alapértelmezett és hibakódok esetén](scheduler-limits-defaults-errors.md)

 [Azure ütemező kimenő hitelesítést](scheduler-outbound-authentication.md)
