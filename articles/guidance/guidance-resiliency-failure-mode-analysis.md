<properties
   pageTitle="Hiba mód analysis |} Microsoft Azure"
   description="Útmutató hiba mód elemzését felhő megoldások Azure alapján."
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="mwasson"/>

# <a name="azure-resiliency-guidance-failure-mode-analysis"></a>Azure tűrőképessége útmutatást: hiba mód elemzése

A következő készítéséhez tűrőképessége rendszerbe, a rendszer a hiba lehetséges pontok azonosításával áll hiba mód analysis (FMA). A FMA a architektúra és tervezés fázisok részének kell lennie, úgy, hogy az elejétől a rendszerbe hibajavítás készíthet.

Az alábbiakban az általános folyamat egy FMA elvégzésére: 

1. Azonosítsa a rendszer minden összetevő. Szerepeltetni külső függőségeket, például Identitásszolgáltatók, külső szolgáltatásokra támaszkodik és így tovább.   

2. Minden összetevő azonosítsa a fellépő esetleges hibák. Előfordulhat, hogy egyetlen összetevővel egynél több hiba módban. Például érdemes olvasási hibák fontolja meg, és hibák külön-külön írni, mert a hatás és a lehetséges megoldásokkal kapcsolatban különböző lesz.

3. Minden hiba üzemmódban aszerint, hogy a teljes kockázat ráta. Vegye figyelembe a következőket:  

    - Mi az a hiba valószínűsége. Az általában viszonylag közös? A Extrememly ritka? Nincs szükség a pontos számok a célja, hogy a prioritás rangsorolása súgó.

    - Mi az, hogy az alkalmazás elérhetőségét, az adatok elvesztését, a költség és üzleti zavarok hatással? 

4. Minden egyes hiba üzemmódban határozza meg, hogyan válaszol és helyreállítása az alkalmazás. Fontolja meg a kompromisszumok a költség és alkalmazások összetettsége.   

Kiindulási pontként a FMA folyamatot Ez a cikk a potenciális hiba módok katalógushoz, és azok megoldásokkal kapcsolatban tartalmaz. A katalógus technológia vagy Azure szolgáltatás, valamint az alkalmazás szintű tervezésekor általános kategória szerint vannak rendezve. A katalógus nem teljes, de magában foglalja a fő Azure számos szolgáltatások. 


## <a name="app-service"></a>Alkalmazás szolgáltatás

### <a name="app-service-app-shuts-down"></a>Alkalmazás szolgáltatás alkalmazás leáll.

Az **észlelési**. Lehetséges okai:

- Várható leállítása
    
    - Az alkalmazás; leállítja operátor az Azure portál például használatával.

    - Az alkalmazás nem betöltetlen, mert az üresjárati volt. (Csak ha az `Always On` beállítás le van tiltva.)

- Váratlan leállítása

    - Az alkalmazás összeomlik.

    - Az alkalmazás szolgáltatás virtuális példány elérhetetlenné válik.


Application_End naplózás fog tájékozódást segíti a alkalmazás tartomány leállítása (finom folyamat lefagyhat), és az alkalmazás tartomány leállás tájékozódást segíti, hogy csak úgy.

**Helyreállítási**

- Várt a Leállítás, ha az alkalmazás leállás segítségével leállítása. Ha például az ASP.NET, az a `Application_End` módot.

- Ha az alkalmazás volt betöltetlen, miközben az üresjárati, automatikusan indítani a következő kérésének megfelelően. A "hideg indításkor" költség azonban fog merülnek fel. 

- Megakadályozhatja, hogy az alkalmazás éppen törölt, miközben üresjárati, engedélyezze a `Always On` beállítása a web App alkalmazásban. Lásd: [Azure alkalmazás szolgáltatás konfigurálása web Apps alkalmazások][app-service-configure].

- Operátor megakadályozása az alkalmazás leáll, állítsa be az erőforrás lakat `ReadOnly` szintjét. [Az Azure erőforrás-kezelő zárolása forrásokban]talál[rm-locks].

- Ha az alkalmazás összeomlik, vagy egy alkalmazás szolgáltatás virtuális nem érhető el, a alkalmazás szolgáltatás automatikusan újraindítja az alkalmazást. 


**Diagnosztikai**. Alkalmazás naplókról és a webhely kiszolgálói naplók. Lásd: a [diagnosztikai naplózás web Apps alkalmazások Azure alkalmazás szolgáltatás engedélyezése][app-service-logging].


### <a name="a-particular-user-repeatedly-makes-bad-requests-or-overloads-the-system"></a>Egy felhasználó többször lehetővé teszi a hibás kérések vagy a rendszer túlterhelések. 

Az **észlelési**. Felhasználók hitelesítő és felhasználói azonosító szerepeltetni alkalmazás naplók.

**Helyreállítási**

- [Azure API] -kezelési[ api-management] szabályozási kérelmek a felhasználók elől. [Azure API kezelésével szabályozásának Speciális kérelem] látható[api-management-throttling]

- A felhasználónak letiltja.

**Diagnosztikai**. Jelentkezzen be az összes hitelesítési kérések.


### <a name="a-bad-update-was-deployed"></a>A hibás frissítése telepítették.

Az **észlelési**. Az Azure-portálon keresztül alkalmazás állapotának figyelése (lásd: [Monitor Azure web app teljesítmény][app-insights-web-apps]), illetve végrehajtja a [állapot végpont felügyeleti mintát][health-endpoint-monitoring-pattern].

**Helyreállítási**. Több [telepítési helyek] használata[ app-service-slots] és visszaállíthatja a legutolsó helyes példányban. További tudnivalókért lásd: [egyszerű webes alkalmazás hivatkozás architektúra][ra-web-apps-basic].


## <a name="azure-active-directory"></a>Azure Active Directory

### <a name="openid-connect-oidc-authentication-fails"></a>OpenID csatlakozás (OIDC) hitelesítési sikertelen lesz.

Az **észlelési**. A hiba lehetséges módok a következők:

1. Azure Active Directory nem érhető el, vagy egy hálózati probléma miatt nem érhető el. A hitelesítés végpontra átirányítási és a OIDC köztes kivételt okoz.
2. Azure AD-bérlő nem létezik. Átirányítás a hitelesítési végpont HTTP hibakód adja eredményül, és a OIDC köztes kivételt okoz.
3. Felhasználó nem tud hitelesítést végezni. Nincs észlelési stratégia nem szükséges; Azure AD a bejelentkezési hibák kezeli.

**Helyreállítási**

1. A köztes a kezelt kivételek elfog.
2. Kezelni `AuthenticationFailed` eseményeket.
3. Irányítsa át a felhasználó egy hiba lapot.
4. Felhasználói újrapróbálkozások.


## <a name="azure-search"></a>Azure keresés

### <a name="writing-data-to-azure-search-fails"></a>Sikertelen az Azure keresési írni az adatokat.

Az **észlelési**. A tényleges `Microsoft.Rest.Azure.CloudException` hibákat.

**Helyreállítási**

A [Keresés .NET SDK] [ search-sdk] tranziens hiba után automatikusan próbálkozások. Az ügyfél SDK bármely kivételek nem tranziens hibák kell kezelni.

Az alapértelmezett újrapróbálkozási házirend exponenciális vissza kikapcsolása használja. Különböző újrapróbálkozási-házirend használata, hívja a `SetRetryPolicy` a a `SearchIndexClient` vagy `SearchServiceClient` osztály. További tudnivalókért lásd: a [Automatikus próbálkozások][auto-rest-client-retry].

**Diagnosztikai**. Használja a [keresési forgalom Analytics][search-analytics].


### <a name="reading-data-from-azure-search-fails"></a>Adatok beolvasása az Azure kereséséből származó sikertelen lesz.

Az **észlelési**. A tényleges `Microsoft.Rest.Azure.CloudException` hibákat.

**Helyreállítási**

A [Keresés .NET SDK] [ search-sdk] tranziens hiba után automatikusan próbálkozások. Az ügyfél SDK bármely kivételek nem tranziens hibák kell kezelni.

Az alapértelmezett újrapróbálkozási házirend exponenciális vissza kikapcsolása használja. Különböző újrapróbálkozási-házirend használata, hívja a `SetRetryPolicy` a a `SearchIndexClient` vagy `SearchServiceClient` osztály. További tudnivalókért lásd: a [Automatikus próbálkozások][auto-rest-client-retry].

**Diagnosztikai**. Használja a [keresési forgalom Analytics][search-analytics].



## <a name="cassandra"></a>Cassandra


### <a name="reading-or-writing-to-a-node-fails"></a>Sikertelen az csomópontra írásakor vagy olvasásakor.

Az **észlelési**. A kivétel elfog. .NET-ügyfelek esetén ez általában lesz `System.Web.HttpException`. Előfordulhat, hogy a többi ügyfél kivétel másfajta.  További tudnivalókért lásd: [munkavégzés jobbra Cassandra hibakezelés](http://www.datastax.com/dev/blog/cassandra-error-handling-done-right).

**Helyreállítási**

- Minden egyes [Cassandra ügyfél](https://wiki.apache.org/cassandra/ClientOptions) rendelkezik-e saját újrapróbálkozási házirendek és funkciók. További tudnivalókért lásd: [munkavégzés jobbra Cassandra hibakezelés][cassandra-error-handling].
- Egy állvány-et telepítés használata adatok csomópontok hibafa tartományok elosztva.
- A helyi kvórum konzisztencia üzembe több területre. Nem tranziens hiba történik, ha nem sikerül fölé egy másik területére.

**Diagnosztikai**. Alkalmazás naplók


## <a name="cloud-service"></a>Felhőalapú szolgáltatás

### <a name="web-or-worker-roles-are-unexpectedlybeing-shut-down"></a>Webhely vagy dolgozó szerepkörök vannak váratlanul leáll.

Az **észlelési**. A [RoleEnvironment.Stopping] [ RoleEnvironment.Stopping] esemény el van indítva.

**Helyreállítási**. A [RoleEntryPoint.OnStop] felülbírálása[ RoleEntryPoint.OnStop] módszert biztonságosan jelenjenek meg. További tudnivalókért lásd: [Azure OnStop események kezelni az jobb módszer] [ onstop-events] (blog).


## <a name="documentdb"></a>DocumentDB

### <a name="reading-data-from-documentdb-fails"></a>Adatok beolvasása a DocumentDB sikertelen lesz.

Az **észlelési**. A tényleges `System.Net.Http.HttpRequestException` vagy `Microsoft.Azure.Documents.DocumentClientException`. 

**Helyreállítási**

- A SDK automatikusan próbálkozások sikertelen kísérletek. A újrapróbálkozások száma és a maximális várakozási idő megadásához konfigurálása `ConnectionPolicy.RetryOptions`. Az ügyfél hatványra kivételek vagy túl a újrapróbálkozási házirend vagy nem tranziens hibákat. 

- Ha DocumentDB lehetővé az ügyfél, a HTTP 429 hibát ad vissza. A állapotkód beadása a `DocumentClientException`. Ha hiba 429 egységes, érdemes megfontolni a DocumentDB gyűjtemény átviteli értékét.

- A DocumentDB adatbázis bizonyos két vagy több területek között. Az összes kópiák olvasható. Az ügyfél SDK használata esetén adja meg a `PreferredLocations` paraméter. Ez az Azure régiók olyan rendezett listát. A lista első elérhető terület összes olvasás küld. Ha nem sikerül a kérést, az ügyfél megpróbálja a többi régiók listájában a sorrendben. További tudnivalókért lásd: a [több területre DocumentDB fiókoknál fejlődő][docdb-multi-region].

**Diagnosztikai**. Jelentkezzen be az összes hibát ügyféloldali.


### <a name="writing-data-to-documentdb-fails"></a>Írás a DocumentDB sikertelen lesz.

Az **észlelési**. A tényleges `System.Net.Http.HttpRequestException` vagy `Microsoft.Azure.Documents.DocumentClientException`. 

**Helyreállítási**

- A SDK automatikusan próbálkozások sikertelen kísérletek. A újrapróbálkozások száma és a maximális várakozási idő megadásához konfigurálása `ConnectionPolicy.RetryOptions`. Az ügyfél hatványra kivételek vagy túl a újrapróbálkozási házirend vagy nem tranziens hibákat. 

- Ha DocumentDB lehetővé az ügyfél, a HTTP 429 hibát ad vissza. A állapotkód beadása a `DocumentClientException`. Ha hiba 429 egységes, érdemes megfontolni a DocumentDB gyűjtemény átviteli értékét.

- A DocumentDB adatbázis bizonyos két vagy több területek között. Ha nem sikerül az elsődleges régió, a kiemelt írni egy másik terület kell. Egy feladatátvételt manuálisan is válthat. A SDK alkalmazás kódja is működő feladatátvevő után az automatikus észlelés és a továbbítás, tesz. Időszakban feladatátvevő (általában perc) az írási műveletek fog rendelkezni magasabb késés, mint a SDK megtalálja az új írási régió. További tudnivalókért lásd: a [több területre DocumentDB fiókoknál fejlődő][docdb-multi-region].

- Egy Visszalépés továbbra is fennáll a biztonsági másolat várólista a dokumentumot, és később folyamat a sor.

**Diagnosztikai**. Jelentkezzen be az összes hibát ügyféloldali.


## <a name="elasticsearch"></a>Elasticsearch

### <a name="reading-data-from-elasticsearch-fails"></a>Adatok beolvasása a Elasticsearch sikertelen lesz.

Az **észlelési**. A megfelelő kivételt, az adott [Elasticsearch ügyfél] Elfog[ elasticsearch-client] használatban. 

**Helyreállítási**

- Egy újrapróbálkozási mechanizmusa használja. Minden egyes ügyfél rendelkezik-e saját újrapróbálkozási házirendeket. 

- Több Elasticsearch csomópontok telepítése, és a replikáció használata az elérhetőség magas.

További tudnivalókért lásd: az [Operációs rendszert futtató Elasticsearch a Azure][elasticsearch-azure].

**Diagnosztikai**. Ellenőrző eszközök segítségével Elasticsearch, vagy jelentkezzen a tartalom ügyféloldali összes hibát. A "Figyelés" című [Elasticsearch fut a Azure][elasticsearch-azure].


### <a name="writing-data-to-elasticsearch-fails"></a>Írás a Elasticsearch sikertelen lesz.

Az **észlelési**. A megfelelő kivételt, az adott [Elasticsearch ügyfél] Elfog[ elasticsearch-client] használatban.  

**Helyreállítási**

- Egy újrapróbálkozási mechanizmusa használja. Minden egyes ügyfél rendelkezik-e saját újrapróbálkozási házirendeket. 
 
- Ha az alkalmazás elviseli csökkentett konzisztencia szintet, fontolja meg az írás `write_consistency` beállítása `quorum`.

További tudnivalókért lásd: az [Operációs rendszert futtató Elasticsearch a Azure][elasticsearch-azure].

**Diagnosztikai**. Ellenőrző eszközök segítségével Elasticsearch, vagy jelentkezzen a tartalom ügyféloldali összes hibát. A "Figyelés" című [Elasticsearch fut a Azure][elasticsearch-azure].


## <a name="queue-storage"></a>A várakozási tárhely

### <a name="writing-a-message-to-azure-queue-storage-fails-consistently"></a>Egységes írása üzenet Azure várólista tároló sikertelen lesz.

Az **észlelési**. Miután *N* újrapróbálkozások, az írást továbbra is sikertelen lesz.

**Helyreállítási**

- Tárolja az adatokat a helyi gyorsítótár és a továbbítás tároló írás újabb verzióiban, a szolgáltatás elérhetővé.
- Hozzon létre egy másodlagos várólista, és ha az elsődleges várólista nem érhető el, hogy a várakozási írni.

**Diagnosztikai**. Használja a [tárolási mértékek][storage-metrics].


### <a name="the-application-cannot-process-a-particular-message-from-the-queue"></a>Az alkalmazás nem lehet feldolgozni azokat egy adott üzenet a sorból. 

Az **észlelési**. Adott alkalmazást. Ha például az üzenet érvénytelen adatot tartalmaz, vagy üzleti logikai funkcióinak valamiért nem sikerül. 

**Helyreállítási**

Az üzenet áthelyezése egy külön sorba. Futtassa a vizsgálja meg, hogy sorban az üzenetek egy külön folyamatot.

Fontolja meg inkább Azure Bus üzenetküldés sorok, amely tartalmaz egy [kézbesítetlen] [ sb-dead-letter-queue] erre a célra használható funkciók körét.

Megjegyzés: Ha a WebJobs tároló sorban várakozó használja, a WebJobs SDK tartalmaz a beépített elhalt üzenetek kezelésének. Megtudhatja, [hogy miként használja a WebJobs SDK csomagjában talál az Azure várólista tároló][sb-poison-message].

**Diagnosztikai**. Naplózás alkalmazást használja.


## <a name="redis-cache"></a>Gyorsítótár vgx.dll

### <a name="reading-from-the-cache-fails"></a>A gyorsítótárból olvasási sikertelen lesz.

Az **észlelési**. A tényleges `StackExchange.Redis.RedisConnectionException`.

**Helyreállítási**

1. Ismételje meg a tranziens hibák. Azure vgx.dll gyorsítótár támogatja a beépített újrapróbálkozási – lásd: [Vgx.dll gyorsítótár újrapróbálkozási irányelvek][redis-retry].
2. A gyorsítótár mért sikertelen találatok nem tranziens hibák tekinti, és visszatérés az eredeti adatforráshoz.

**Diagnosztikai**. Használja a [gyorsítótár vgx.dll diagnosztika][redis-monitor].


### <a name="writing-to-the-cache-fails"></a>Írás a gyorsítótár sikertelen lesz.

Az **észlelési**. A tényleges `StackExchange.Redis.RedisConnectionException`.

**Helyreállítási**

1. Ismételje meg a tranziens hibák. Azure vgx.dll gyorsítótár támogatja a beépített újrapróbálkozási – lásd: [Vgx.dll gyorsítótár újrapróbálkozási irányelvek][redis-retry].
2. Ha a hiba nem tranziens, hagyja figyelmen kívül, és mondja el egyéb később írni a gyorsítótár tranzakciók.

**Diagnosztikai**. Használja a [gyorsítótár vgx.dll diagnosztika][redis-monitor].


## <a name="sql-database"></a>SQL-adatbázis

### <a name="cannot-connect-to-the-database-in-the-primary-region"></a>Nem tud csatlakozni az elsődleges területen az adatbázist.

Az **észlelési**. Csatlakozás sikertelen lesz.

**Helyreállítási**

Előfeltétel: Aktív geo-replikáció úgy kell konfigurálni az adatbázist. Lásd: [Az SQL adatbázis aktív Geo-replikáció][sql-db-replication].

- Lekérdezések olvassa el a másodlagos kópiából. 
- Beszúrása és a frissítések manuális átadni a másodlagos kópia. Lásd: az [SQL Azure-adatbázis tervezett vagy nem tervezett feladatátvevő kezdeményezzen][sql-db-failover].

A replika különböző kapcsolati karakterlánc, használ, így a program módosítania kell a kapcsolati karakterláncot, az alkalmazás.


### <a name="client-runs-out-of-connections-in-the-connection-pool"></a>Ügyfél fut, a kapcsolat készletben kapcsolatok ki.

Az **észlelési**. A tényleges `System.InvalidOperationException` hibákat. 

**Helyreállítási**

- Ismételje meg a műveletet.
- A kezelési terv szerint, hogy egy használatieset uralja nem az összes kapcsolathoz elkülönítése az egyes használatieset-kapcsolat készleteket.
- A maximális kapcsolat készletek növelése

**Diagnosztikai**. Alkalmazás naplók.


### <a name="database-connection-limit-is-reached"></a>Adatbázis kapcsolat letelik. 

Az **észlelési**. Az Azure SQL-adatbázis egyidejű dolgozók, a bejelentkezési és a munkamenetek korlátozza. A korlátok attól függenek, hogy a szolgáltatási réteg. További információ című cikkben találhat [Azure SQL-adatbázis erőforrás][sql-db-limits].

A hibák feltárása, Elfog `System.Data.SqlClient.SqlException` , és jelölje be a értékének `SqlException.Number` az SQL hibakódú. Megfelelő hibakódok listájáért lásd: [SQL-hibakódok az SQL-adatbázis-ügyfélalkalmazásokban: adatbázis-kapcsolati hiba és egyéb problémák][sql-db-errors].

**Helyreállítási**. A hibák számít ideiglenes (tranziens), hogy újra próbálkozik megoldhatja a problémát. Egységes gombra kattint a hibák, ha fontolja meg az adatbázis méretezését.

**Diagnosztikai**. -A [sys.event_log] [ sys.event_log] lekérdezés sikeres az adatbázis-kapcsolatokat, a csatlakozási hibák és a kölcsönös kizárások adja vissza.

- Hozzon létre egy [szabályt] [ azure-alerts] esetén sikertelen a kapcsolatokat.

- [SQL-adatbázis a naplózás] engedélyezése[ sql-db-audit] és a sikertelen bejelentkezések ellenőrzése.


## <a name="service-bus-messaging"></a>Bus üzenetküldési szolgáltatás

### <a name="reading-a-message-from-a-service-bus-queue-fails"></a>Üzenet olvasása a szolgáltatás Bus sorból sikertelen lesz.

Az **észlelési**. Az ügyfél SDK alóli kivételek elfog. Az alap osztálya szolgáltatás Bus kivételek [MessagingException][sb-messagingexception-class]. Ha a hiba ideiglenes (tranziens), a `IsTransient` tulajdonság értéke igaz. 

További tudnivalókért lásd: a [szolgáltatás Bus üzenetküldés kivételek][sb-messaging-exceptions].

**Helyreállítási**

1. Ismételje meg a tranziens hibák. Lásd: a [Szolgáltatás Bus irányelvek újra][sb-retry].

2. Üzenetek, amelyek a nem minden címzett *kézbesítetlen*kerülnek. Ez a várólista használatával megtekintheti, hogy nem sikerült mely üzeneteket fogadni. Nincs automatikus karbantartása: a halottlevél nem. Az üzenetek van maradnak mindaddig, amíg kifejezetten beolvasásához. Lásd: [a szolgáltatás Bus áttekintése kézbesítetlen sorok][sb-dead-letter-queue].


### <a name="writing-a-message-to-a-service-bus-queue-fails"></a>Üzenet írása a szolgáltatás Bus várólista sikertelen lesz.

Az **észlelési**. Az ügyfél SDK alóli kivételek elfog. Az alap osztálya szolgáltatás Bus kivételek [MessagingException][sb-messagingexception-class]. Ha a hiba ideiglenes (tranziens), a `IsTransient` tulajdonság értéke igaz. 

További tudnivalókért lásd: a [szolgáltatás Bus üzenetküldés kivételek][sb-messaging-exceptions].

**Helyreállítási**

1. A szolgáltatás Bus ügyfél automatikusan próbálkozások tranziens hibák után. Alapértelmezés szerint akkor exponenciális vissza ki. Újrapróbálkozások maximális száma vagy a legnagyobb határidő után az ügyfél kivételt okoz. További tudnivalókért lásd: a [Szolgáltatás Bus irányelvek újra][sb-retry].

2. Ha várólista kvóta kimerítve van, az ügyfél okoz [QuotaExceededException][QuotaExceededException]. A kivételhiba üzenete további részleteket adja vissza. A sorból néhányüzenetet drain előtt, hogy újra próbálkozik, és fontolja meg inkább a megszakító minta folyamatos újrapróbálkozások elkerülése érdekében, miközben a kvóta kimerítve van. Győződjön meg arról, hogy a [BrokeredMessage.TimeToLive] tulajdonság nincs beállítva túl nagy. 

3. Terület belül tűrőképessége [particionált sorban várakozó]vagy témakörök továbbfejlesztett[sb-partition]. Egy másik várólista vagy témakör van hozzárendelve egy üzenetben áruházból. Az üzenetben üzlet nem érhető el, ha a várólista vagy a témakör az összes művelet sikertelen lesz. A particionált várólista vagy témát particionált keresztül több üzenetben tárolja. 

4. További tűrőképessége hozzon létre két szolgáltatás Bus névtér különböző régiókban, és az üzenetek replikáció. Aktív replikációs és a passzív replikációs is használhatja.

    - Aktív replikáció: az ügyfél mindkét sorban várakozó minden üzenetet küld. A címzett mindkét sorban várakozó figyeli. Nyomon követése egy egyedi azonosítót, üzenetek, így az ügyfél ismétlődő üzenetet elvetheti.

    - Passive replikáció: az ügyfél az üzenetet küld egy sorba. Ha hiba lép fel, az ügyfél visszavált a többi sor. A címzett mindkét sorban várakozó figyeli. Ezt a megközelítést csökkenti az ismétlődő küldött üzenetek számát. A címzett azonban továbbra is kell kezelni az ismétlődő üzenetek.

    További tudnivalókért lásd: [GeoReplication minta] [ sb-georeplication-sample] és a [Gyakorlati tanácsok a szolgáltatás Bus kimaradások és katasztrófák elleni szigetelő alkalmazások][sb-outages].


### <a name="duplicate-message"></a>Ismétlődő üzenet.

Az **észlelési**. Vizsgálja meg a `MessageId` és `DeliveryCount` az üzenet tulajdonságait.

**Helyreállítási**

- Ha lehetséges tervezése idempotent kell feldolgozási műveletek az üzenetet. Az üzenetek már feldolgozása, és jelölje be az azonosító üzenet feldolgozása előtt üzenet azonosítók ellenkező esetben tárolni.

- Ismétlődő észlelése, engedélyezze a rendelkező várólista létrehozásával `RequiresDuplicateDetection` igaz értékre kell állítani. Ezzel a beállítással szolgáltatás Bus automatikusan törli az ugyanazon küldött üzenetek `MessageId` előző üzenetként.  Vegye figyelembe az alábbiakat:

    -  Ez a beállítás megakadályozza, hogy az ismétlődő üzenetek a várólista üzembe. A többször feldolgozása ugyanazt az üzenetet fogadó nem akadályozza meg.

    - Ismétlődő észlelési egy időkeret tartalmaz. Ha túl az ablak duplikált küldi el, nem észlelhető.

**Diagnosztikai**. Jelentkezzen be az ismétlődő üzeneteket.


### <a name="the-application-cannot-process-a-particular-message-from-the-queue"></a>Az alkalmazás nem lehet feldolgozni azokat egy adott üzenet a sorból. 

Az **észlelési**. Adott alkalmazást. Ha például az üzenet érvénytelen adatot tartalmaz, vagy üzleti logikai funkcióinak valamiért nem sikerül. 

**Helyreállítási**

Vannak olyan két hiba módot is célszerű figyelembe venni. 

- A címzett a hibát észlel. Ebben az esetben az üzenet áthelyezése a halottlevél. Egy külön folyamatot szemügyre veszi a halottlevél az üzenetek később futtatni.

- A címzett sikertelen az üzenet közepén &mdash; például esetén nem kezelt kivétel következtében. Ebben az esetben kezelendő használata `PeekLock` módban. Ebben a módban a zárolás lejár, ha az üzenet elérhetővé válik a többi címzett. Ha az üzenet mérete meghaladja a kézbesítési maximális számát vagy a time-to-live, az üzenet a halottlevél automatikusan került.

További tudnivalókért lásd: a [szolgáltatás-áttekintés Bus kézbesítetlen sorok][sb-dead-letter-queue].

**Diagnosztikai**. Az alkalmazás egy üzenetet a halottlevél helyezi át, amikor az alkalmazás naplók írási esemény.


## <a name="service-fabric"></a>Szolgáltatás háló

### <a name="a-request-to-a-service-fails"></a>Nem sikerül egy szolgáltatási kérelmet.

Az **észlelési**. A szolgáltatás hibát ad vissza.

**Helyreállítási**

- Keresse meg újra a proxy (`ServiceProxy` vagy `ActorProxy`), és hívja meg a szolgáltatás/szereplő módszer újra. 

- **Állapot-nyilvántartó szolgáltatás**. A megbízható gyűjtemények műveletek tördelése tranzakció. Ha hiba lép fel, a tranzakció fog állítható vissza. A kérelmet, ha egy sorban lekért fog dolgozza fel újra. 
 
- **Állapot nélküli szolgáltatást**. Ha a szolgáltatás külső tárolóhoz adatokat is fennáll, összes művelet kell idempotent.


**Diagnosztikai**. Alkalmazás napló

### <a name="service-fabric-node-is-shut-down"></a>Szolgáltatás háló csomópont leállt.

Az **észlelési**. A szolgáltatás a lemondás jogkivonat átadott `RunAsync` módot. Háló szolgáltatás leállítása a csomópont előtt lemondja a a tevékenység.

**Helyreállítási**. A lemondás jogkivonat használja a Leállítás feltárása. Amikor szolgáltatás háló lemondási kér, Befejezés munkáját, és kilépés azokból `RunAsync` minél gyorsabban.

**Diagnosztikai**. Alkalmazás naplók


## <a name="storage"></a>Tárhely

### <a name="writing-data-to-azure-storage-fails"></a>Sikertelen az írás adatok Azure-tárolóhoz

Az **észlelési**. Az ügyfél megkapja a hibák írásakor.

**Helyreállítási**

1. Ismételje meg a műveletet, ideiglenes (tranziens) hibák lévő visszaállításához. [Ismételje meg a házirend] [ Storage.RetryPolicies] az ügyfél SDK kezeli meg automatikusan.
2. Végrehajtja a megszakító minta ellenőrizni tároló elkerülése érdekében.
3. Ha az N újrapróbálkozási kísérletek sikertelenek, hajtsa végre a egy biztonságos Visszalépés. Példa:

    - Tárolja az adatokat a helyi gyorsítótár és a továbbítás tároló írás újabb verzióiban, a szolgáltatás elérhetővé.
    - Ha az írási művelet tranzakció alapú hatókör volt, egyenlíti a tranzakciók.

**Diagnosztikai**. Használja a [tárolási mértékek][storage-metrics].


### <a name="reading-data-from-azure-storage-fails"></a>Adatok beolvasása az Azure-tárhelyről sikertelen lesz.

Az **észlelési**. Az ügyfél megkapja a hibák olvasásakor.

**Helyreállítási**

1. Ismételje meg a műveletet, ideiglenes (tranziens) hibák lévő visszaállításához. [Ismételje meg a házirend] [ Storage.RetryPolicies] az ügyfél SDK kezeli meg automatikusan.
2. TS-GRS tárolására Ha nem sikerül az elsődleges végpontot olvasása, próbálja meg a másodlagos végpont olvasása. Az ügyfél SDK oldhatja meg automatikusan. Lásd: [Azure tároló replikáció][storage-replication].
3. Ha *N* újrapróbálkozási kísérletek sikertelenek, egy alaplekérdezések lépéseket biztonságosan csökkentheti. Ha nem lehet beolvasni a termék képét tárhely, például egy általános helyőrző képe megjelenítése.

**Diagnosztikai**. Használja a [tárolási mértékek][storage-metrics].


## <a name="virtual-machine"></a>Virtuális gépen

### <a name="connection-to-a-backend-vm-fails"></a>Nem sikerül kapcsolatot egy virtuális kódmentes.

Az **észlelési**. A hálózati kapcsolat hibákat.

**Helyreállítási**

- Az elérhetőség beállítása egy terheléselosztó mögött legalább két kódmentes VMs üzembe.

- A kapcsolati hiba esetén tranziens néha TCP sikeresen újra próbálkozik az üzenetet küldő. 

- Ismét házirend végrehajtása az alkalmazásban. 

- Állandó vagy nem tranziens hibák, a végrehajtja a [megszakító] [ circuit-breaker] mintát.

- Ha a hívó virtuális meghaladja a hálózati kilépési, a kimenő várólista megtelik. Ha a kimenő várólista egységes teljes, fontolja meg a méretezés kifelé. 

**Diagnosztikai**. A szolgáltatás határai naplózása.

### <a name="vm-instance-becomes-unavailable-or-unhealthy"></a>Virtuális példány lesz, nem érhető el vagy megsérült.

Az **észlelési**. Állítsa be a terheléselosztó [állapot vizsgálati] [ lb-probe] , amely jelzi, hogy megfelelő-e a virtuális példány. A vizsgálati ellenőriznie kell, hogy kritikus függvények válaszol megfelelően. 

**Helyreállítási**. A minden alkalmazás réteg több virtuális példányok üzembe elérhetősége ugyanazok, és helyezze el egy terheléselosztó a VMs elé. Ha nem sikerül az állapot vizsgálati, a terheléselosztó megszakítja a küld a sérült példány új kapcsolatokat.

**Diagnosztikai**. -Terheléselosztó [napló analytics]használata[lb-monitor].
- Állítsa be a figyelni a rendszerállapot figyelése a végpontok összes felügyeleti rendszert.


### <a name="operator-accidentally-shuts-down-a-vm"></a>Operátor véletlenül egy virtuális leáll.

Az **észlelési**. A #HIÁNYZIK

**Helyreállítási**. Az erőforrás lakat beállítása `ReadOnly` szintjét. [Az Azure erőforrás-kezelő zárolása forrásokban]talál[rm-locks].

**Diagnosztikai**. [Azure tevékenység napló]használata[azure-activity-logs].


## <a name="webjobs"></a>WebJobs

### <a name="continuous-job-stops-running-when-the-scm-host-is-idle"></a>Folytonos feladat SCM állomás tétlen állapotában leáll.

Az **észlelési**. A lemondás jogkivonat átadni a WebJob függvényt. További tudnivalókért lásd: a [sikeres-e leállás][web-jobs-shutdown]. 

**Helyreállítási**. Engedélyezze a `Always On` beállítása a web App alkalmazásban. További tudnivalókért lásd: [WebJobs futtatása a háttérben feladatok][web-jobs].


## <a name="application-design"></a>Alkalmazás tervezése

### <a name="application-cant-handle-a-spike-in-incoming-requests"></a>Alkalmazás nem tudják kezelni a Nyárs a bejövő felkérést.

Az **észlelési**. Az alkalmazás függ. A jelenség tipikus:

- A webhely elindítja a HTTP 5xx hibakódok visszaadása.
- Függő szolgáltatások, például adatbázisból vagy tárolására, indítsa el a kérések szabályozása. Keresse meg a HTTP-hibák HTTP 429 (túl sok kérelmek), például attól függően, hogy a szolgáltatás.
- HTTP várólista hossza nő.

**Helyreállítási**

- Nagyobb terhelés kezelésére méretezése. 

- Kaszkádolt hibák megszakíthatja a teljes alkalmazás zsugorítása hibák csökkentésében. Kezelési stratégiák a következők:

    - A [Minta szabályozásának] megvalósítása[ throttling-pattern] ellenőrizni kódmentes rendszerek elkerülése érdekében.
    - [Simítás várólista alapú] terhelést[ queue-based-load-leveling] puffer kérelmeket, és egy megfelelő tempójában feldolgozni azokat.
    - Egyes ügyfelek sorrendet. Például ha az alkalmazás ingyenes és a díjköteles rétegek, szabályozása ingyenes rétegben ügyfelek, de nem fizetett ügyfelek. Lásd: a [Prioritás várólista mintát][priority-queue-pattern].

**Diagnosztikai**. Használja az [alkalmazás szolgáltatás és a diagnosztikai naplózás][app-service-logging]. Használja egy szolgáltatásba, például [Azure napló Analytics][azure-log-analytics], [Alkalmazás háttérismeretek][app-insights], vagy [Új Relic] [ new-relic] annak a diagnosztikai naplók megértésében.


### <a name="one-of-the-operations-in-a-workflow-or-distributed-transaction-fails"></a>Sikertelen a munkafolyamat vagy elosztott tranzakciók műveletek közül.

Az **észlelési**. Miután *N* újrapróbálkozások, még nem.

**Helyreállítási**

- A kezelési terv hajtja végre a [Feladatütemező ügynök felügyelő] [ scheduler-agent-supervisor] minta a teljes munkafolyamat kezeléséhez. 
- A időtúllépései nem próbálkozzon újra. Van egy kis sikeres kamatlábát ezt a hibát. 
- A várakozási munkát, annak érdekében, hogy később újra.

**Diagnosztikai**. Jelentkezzen be az összes művelet (sikeres és sikertelen), beleértve a végtermék műveletek. Korrelációs azonosítót, használja, így nyomon követheti összes művelet ugyanazon tranzakción belül.


### <a name="a-call-to-a-remote-service-fails"></a>Nem sikerül hívást kezdeményez egy távoli szolgáltatás.

Az **észlelési**. HTTP hiba jelenik meg.

**Helyreállítási**

1. Ismételje meg a tranziens hibák. 
2. Ha a hívás után sikertelen lesz *N* kísérletek, a visszalépési művelet végrehajtása. (Alkalmazás bizonyos.)
3. A [megszakító mintát] megvalósítása[ circuit-breaker] kaszkádolt hibák elkerülése érdekében. 

**Diagnosztikai**. Jelentkezzen be az összes távoli hívásvezérlés hibák.


## <a name="next-steps"></a>Következő lépések

FMA folyamattal kapcsolatos további tudnivalókért lásd: [cloud Services szándékosan címtárfrissítések] [ resilience-by-design-pdf] (PDF-fájl letöltése).

<!-- links -->

[api-management]: https://azure.microsoft.com/documentation/services/api-management/
[api-management-throttling]: ../api-management/api-management-sample-flexible-throttling.md
[app-insights]: ../application-insights/app-insights-overview.md
[app-insights-web-apps]: ../application-insights/app-insights-azure-web-apps.md
[app-service-configure]: ../app-service-web/web-sites-configure.md
[app-service-logging]: ../app-service-web/web-sites-enable-diagnostic-log.md
[app-service-slots]: ../app-service-web/web-sites-staged-publishing.md
[auto-rest-client-retry]: https://github.com/Azure/autorest/blob/master/Documentation/clients-retry.md
[azure-activity-logs]: ../monitoring-and-diagnostics/monitoring-overview-activity-logs.md
[azure-alerts]: ../monitoring-and-diagnostics/insights-alerts-portal.md
[azure-log-analytics]: ../log-analytics/log-analytics-overview.md
[BrokeredMessage.TimeToLive]: https://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx
[cassandra-error-handling]: http://www.datastax.com/dev/blog/cassandra-error-handling-done-right
[circuit-breaker]: https://msdn.microsoft.com/library/dn589784.aspx
[docdb-multi-region]: ../documentdb/documentdb-developing-with-multiple-regions.md
[elasticsearch-azure]: guidance-elasticsearch-running-on-azure.md
[elasticsearch-client]: https://www.elastic.co/guide/en/elasticsearch/client/index.html
[health-endpoint-monitoring-pattern]: https://msdn.microsoft.com/library/dn589789.aspx
[onstop-events]: https://azure.microsoft.com/blog/the-right-way-to-handle-azure-onstop-events/
[lb-monitor]: ../load-balancer/load-balancer-monitor-log.md
[lb-probe]: ../load-balancer/load-balancer-custom-probe-overview.md#learn-about-the-types-of-probes
[new-relic]: https://newrelic.com/
[priority-queue-pattern]: https://msdn.microsoft.com/library/dn589794.aspx
[queue-based-load-leveling]: https://msdn.microsoft.com/library/dn589783.aspx
[QuotaExceededException]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.quotaexceededexception.aspx
[ra-web-apps-basic]: guidance-web-apps-basic.md
[redis-monitor]: ../redis-cache/cache-how-to-monitor.md
[redis-retry]: ../best-practices-retry-service-specific.md#cache-redis-retry-guidelines
[resilience-by-design-pdf]: http://download.microsoft.com/download/D/8/C/D8C599A4-4E8A-49BF-80EE-FE35F49B914D/Resilience_by_Design_for_Cloud_Services_White_Paper.pdf
[RoleEntryPoint.OnStop]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx
[RoleEnvironment.Stopping]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.stopping.aspx
[rm-locks]: ../resource-group-lock-resources.md
[sb-dead-letter-queue]: ../service-bus-messaging/service-bus-dead-letter-queues.md
[sb-georeplication-sample]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/GeoReplication
[sb-messagingexception-class]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingexception.aspx
[sb-messaging-exceptions]: ../service-bus-messaging/service-bus-messaging-exceptions.md
[sb-outages]: ../service-bus/service-bus-outages-disasters.md#protecting-queues-and-topics-against-datacenter-outages-or-disasters
[sb-partition]: ../service-bus-messaging/service-bus-partitioning.md
[sb-poison-message]: ../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#poison
[sb-retry]: ../best-practices-retry-service-specific.md#service-bus-retry-guidelines
[search-sdk]: https://msdn.microsoft.com/library/dn951165.aspx
[scheduler-agent-supervisor]: https://msdn.microsoft.com/library/dn589780.aspx
[search-analytics]: ../search/search-traffic-analytics.md
[sql-db-audit]: ../sql-database/sql-database-auditing-get-started.md
[sql-db-errors]: ../sql-database/sql-database-develop-error-messages.md#resource-governanance-errors
[sql-db-failover]: ../sql-database/sql-database-geo-replication-failover-portal.md
[sql-db-limits]: ../sql-database/sql-database-resource-limits.md
[sql-db-replication]: ../sql-database/sql-database-geo-replication-overview.md
[storage-metrics]: https://msdn.microsoft.com/library/dn782843.aspx
[storage-replication]: ../storage/storage-redundancy.md
[Storage.RetryPolicies]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.retrypolicies.aspx
[sys.event_log]: https://msdn.microsoft.com/library/dn270018.aspx
[throttling-pattern]: https://msdn.microsoft.com/library/dn589798.aspx
[web-jobs]: ../app-service-web/web-sites-create-web-jobs.md
[web-jobs-shutdown]: ../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#graceful

