<properties 
    pageTitle="Adatok áthelyezése méretezett kivételének felhő adatbázisok között |} Microsoft Azure" 
    description="Megtudhatja, hogyan kezelheti shards, és helyezze át az adatokat rugalmas adatbázis API-k használata önálló szolgáltatott szolgáltatáson keresztül." 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="ddove"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="ddove" />

# <a name="moving-data-between-scaled-out-cloud-databases"></a>Adatok áthelyezése méretezett kivételének felhő adatbázisok között

Ha Ön a szoftver szolgáltatás fejlesztői, és hogy az alkalmazás váratlanul megy keresztül nagy igény szerinti, kiterjesztve azt a NÖV szeretne. Úgy adhat további adatbázisok (shards). Hogyan az adatokat az új adatbázisok újraterjeszteni a az adatok integritását megszakítása nélkül A **felosztása és egyesítése eszköz** használatával korlátozott adatbázisból származó adatok áthelyezése az új adatbázisok.  

Az felosztása és egyesítése eszköz futása, mint az Azure webszolgáltatás. Egy rendszergazda vagy Fejlesztőeszközök shardlets (egy shard adatainak) áthelyezése másik adatbázis (shards) közötti eszközt használja. A shard térkép management használja a szolgáltatás metaadat-alapú adatbázis, valamint konzisztens hozzárendelések biztosítása.

![– Áttekintés][1]

## <a name="download"></a>Letöltés
[Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge/)


## <a name="documentation"></a>Dokumentáció
1. [Rugalmas adatbázis felosztása és egyesítése eszköz oktatóprogram](sql-database-elastic-scale-configure-deploy-split-and-merge.md)
* [Felosztása és egyesítése biztonsági beállításai](sql-database-elastic-scale-split-merge-security-configuration.md)
* [Felosztása és egyesítése biztonsági kérdései](sql-database-elastic-scale-split-merge-security-configuration.md)
* [Shard térkép kezelése](sql-database-elastic-scale-shard-map-management.md)
* [Méretezési meglévő adatbázis áttelepítése](sql-database-elastic-convert-to-use-elastic-tools.md)
* [Rugalmas Adatbáziseszközök](sql-database-elastic-scale-introduction.md)
* [Rugalmas adatbázis eszközök szószedet](sql-database-elastic-scale-glossary.md)

## <a name="why-use-the-split-merge-tool"></a>Miért érdemes használni a felosztása és egyesítése eszköz?

**Rugalmas**

Alkalmazások kell egy Azure SQL-adatbázis-adatbázis határain rugalmasan egymástól. Ha az adatokat az új adatbázisok integritását megtartásával szükség szerint eszközzel.

**Projektvezetői felosztása** 

Általános kapacitás robbanásveszélyes NÖV kezelése növelése szüksége. Ehhez létrehozása további kapacitás sharding az adatokat, és terjesztése fokozatosan további adatbázisok között, amíg kapacitás igényeinek teljesülése esetén. Ez a legjobb példa az "felosztása" funkció. 

**A Lekicsinyítve, hogy egyesítése**

Kapacitás van szüksége a Lekicsinyítve, üzleti idényjellegű miatt. Az eszköz lehetővé teszi kevesebb skála egységek csökkenti a vállalati verzió esetén. A "egyesítés" funkció a skála rugalmas felosztása és egyesítése szolgáltatásban e követelmény foglalkozik. 

**Interaktív kezelése shardlets áthelyezésével**

Több-ös bérlői webhelyeit adatbázisonként a shards shardlets felosztását vezethet kapacitás szűk néhány shards a. Ehhez a shardlets ismételt hozzárendelése vagy elfoglalt shardlets áthelyezése új vagy kisebb területekre shards. 

## <a name="concepts--key-features"></a>Fogalmak és a fontosabb funkciói

**Ügyfél által üzemeltetett szolgáltatások**

Ügyfél által üzemeltetett szolgáltatásként felosztása és egyesítése érkeznek. Telepítése, és a szolgáltatás üzemelteti a Microsoft Azure-előfizetésben. A csomag, letölthet NuGet befejezéséhez az adott telepítéshez információkkal konfigurációs sablont tartalmaz. Lásd: az [felosztása és egyesítése oktatóanyag](sql-database-elastic-scale-configure-deploy-split-and-merge.md) további információt. Mivel az Azure előfizetés fut a szolgáltatás, ellenőrizheti, és állítsa be a szolgáltatás a legtöbb biztonsági szempontjait. Az alapértelmezett sablont a beállítások konfigurálása a SSL, az ügyfél tanúsítvány-alapú hitelesítés, a titkosítási tárolt hitelesítő adatait, esetlegesen korán DoS és IP-korlátozások tartalmazza. A következő dokumentum [felosztása és egyesítése biztonsági beállításai](sql-database-elastic-scale-split-merge-security-configuration.md)talál további információt a biztonsági szempontjait.

Az alapértelmezett szolgáltatás fut egy dolgozói és egy webes szerepkör telepítését. Az Azure Cloud Services A1 virtuális méretének minden használja. Ezeket a beállításokat nem módosítja, a csomag telepítésekor, miközben a futó felhőalapú szolgáltatást (keresztül az Azure-portálra) a sikeres telepítését követően módosíthatja őket. Figyelje meg, hogy a dolgozó szerepkör nem konfigurálni kell több, mint egy példányát technikai okok miatt. 

**Shard térkép integrációja**

Az alkalmazás shard térképe hogyan kommunikáljon a felosztása és egyesítése szolgáltatást. Amikor a felosztása és egyesítése szolgáltatás felosztása és egyesítése tartományok vagy shardlets közötti mozgás shards, a szolgáltatás automatikusan megőrzi a shard térkép naprakész. Ehhez a szolgáltatást az alkalmazás a shard manager adatbázis csatlakozik, és kezeli a tartomány és a hozzárendelések felosztása és egyesítése/áthelyezések előrehaladását. Ezzel biztosíthatja, hogy a shard térkép mindig megjeleníti az aktuális nézet, felosztása és egyesítése műveletek vannak megy. Felosztásához egyesítés és shardlet elmozdulásnak műveletek a forrás shard nagyobb számú shardlets áthelyezése a cél shard alapján történik. A shardlet során mozgását művelet a shardlets fizetnie az aktuális köteg offline shard térképen megjelölve, és nem érhetők el az adatok függő útválasztási kapcsolatot **OpenConnectionForKey** API segítségével. 

**Egységes shardlet kapcsolatok**

Adatok mozgását indításakor a shardlets az új munkalap bármely shard térkép megadott adatok függő útválasztási való csatlakozás a tárolja a shardlet shard történik módon és későbbi kapcsolatok shard leképezés API-hoz a fenti shardlets blokkolt alatt az adatok mozgását következetlenségekre elkerülése érdekében. Az azonos shard a többi shardlets kapcsolatok is első levágni, de ismét fog sikerülni közvetlenül az Ismét gombra. A köteg helyez át, a shardlets ismét a cél shard online vannak megjelölve, és a forrásadatokat a forrás shard törlődik. A szolgáltatás halad át ezeket a lépéseket minden köteg, mindaddig, amíg az összes shardlets át lett helyezve. Több kapcsolat kill műveletek ez lesz vezethet, a művelet befejezéséhez felosztása és egyesítése és áthelyezése során.  

**Shardlet elérhetőségének kezelése**

A shardlets egy köteg elérhetetlenség hatókörének egyszerre korlátozása a kapcsolatot, közvetlenül a fent ismertetett módon shardlets, az aktuális köteghez korlátozza. Az előnyben részesített fölé megközelítés hol a teljes shard marad az összes annak shardlets offline felosztása és egyesítése művelet során. A köteg egyszerre áthelyezése különböző shardlets számát jelenti mérete konfigurációs paraméter. Az egyes felosztása és egyesítése művelet attól függően, hogy az alkalmazás elérhetőségét, és a teljesítmény igényeinek definiálható. Előfordulhat, hogy a tartományban lévő shard térképen zárolt nagyobb, mint a köteg méretet. Ennek oka az, a szolgáltatás a tartomány mérete választja ki, hogy a tényleges sharding kulcs adat értékeinek száma körülbelül megfelelő köteg méretet. Ne feledje, különösen ritkán feltöltött sharding kulcsok: az. 

**Metaadat-tároló**

A felosztása és egyesítése szolgáltatás használja az adatbázis állapota megtartásához és naplók tartása közben összehívás feldolgozása. A felhasználó az adatbázist hoz létre az előfizetése, és biztosít a kapcsolati karakterlánc azt a szolgáltatást telepítéshez a konfigurációs fájl. A felhasználó szervezettől rendszergazdák is csatlakozhat az adatbázis kérelem előrehaladását és vizsgálja meg a lehetséges hibák részletes tájékoztatást.

**Sharding-tájékoztatás**

A felosztása és egyesítése szolgáltatás (1) sharded táblázatok, (2) hivatkozási táblák és (3) normál táblák közötti különbséget tesz. Felosztása és egyesítése vagy áthelyezés szemantikáját használt táblát típusától függ, és a következők: 

* **Sharded táblák**: osztott, egyesítés és áthelyezés műveletek áthelyezése shardlets forrásból származó cél shard. A teljes kérelem sikeres befejezését követően ezeket shardlets már nem bemutató a forrásra. Ne feledje, hogy a célhely táblák kell a cél shard található adatok előtt a művelet feldolgozásához cél tartomány nem tartalmazhat. 

* **A hivatkozási táblák**: hivatkozási táblák, felosztása, egyesítése és áthelyezése a műveletek másolja az adatokat a forrásból származó a cél shard. Ne feledje, hogy nincs módosítás jelentkezhet be a cél shard egy adott tábla minden sor már létezik, az alábbi táblázatban a célhelyen. A tábla nem lehet üres minden olyan tábla fájlmásolás hivatkozás feldolgozása.

* **További táblák**: a forrás- vagy a Céllista felosztása és egyesítése művelet a többi táblázat használható. A felosztása és egyesítése szolgáltatás nem veszi figyelembe az alábbi táblázat bármely adatok mozgását vagy másolási műveleteket. Ne feledje, is zavarja kényszerek esetén ezeket a műveleteket.

Hivatkozás sharded táblák és összehasonlítása az információkat a shard térképen **SchemaInfo** API-k által biztosított. A következő példa bemutatja egy adott shard térkép manager objektum smm a következő API-k használata: 

    // Create the schema annotations 
    SchemaInfo schemaInfo = new SchemaInfo(); 

    // Reference tables 
    schemaInfo.Add(new ReferenceTableInfo("dbo", "region")); 
    schemaInfo.Add(new ReferenceTableInfo("dbo", "nation")); 

    // Sharded tables 
    schemaInfo.Add(new ShardedTableInfo("dbo", "customer", "C_CUSTKEY")); 
    schemaInfo.Add(new ShardedTableInfo("dbo", "orders", "O_CUSTKEY")); 

    // Publish 
    smm.GetSchemaInfoCollection().Add(Configuration.ShardMapName, schemaInfo); 

A táblázatok "régió" és "nemzeti" meghatározásuk szerint hivatkozási táblát, és felosztása és egyesítése/áthelyezés műveletekkel másolja. "vevő" és "rendelések" viszont meghatározásuk szerint sharded táblákat. C_CUSTKEY és O_CUSTKEY lesz a sharding billentyűt. 

**Hivatkozási integritás megőrzése**

A felosztása és egyesítése szolgáltatás elemzi a táblák közötti függőségek, és használja az elsődleges kulcs külső kulcsú kapcsolatok szakaszban a hivatkozási táblák és shardlets áthelyezése műveleteket. Az általános hivatkozás táblák másolása először függőség sorrendben, majd az belül minden tétel a függőségeket sorrendjének shardlets másolt a vágólapra. Ez a szükséges, hogy a célhely shard idegen kulcsok-Elsődlegeskulcs korlátozó vannak fogadja, az új adatok megérkezik. 

**Shard térkép egységesebb és az esetleges befejezése**

Jelenlétében hibák a felosztása és egyesítése szolgáltatás után bármilyen üzemszünetek folytatja a műveletek, és célja, hogy a haladás összehívásokban el. Azonban lehetnek helyrehozhatatlan azokról a helyzetekről, például amikor a cél shard elveszett vagy túl javítás sérül. Adott körülmények között bizonyos shardlets-be áthelyezendő amelyeknek továbbra is a forrás shard állnak. A szolgáltatás biztosítja, hogy shardlet hozzárendelések csak frissülnek a szükséges adatok másolása sikeresen megtörtént a célba. Shardlets csak törlődnek a forrás, ha minden adat átmásolni a cél, és a megfelelő megfeleltetések frissítése sikerült. A törlési művelet történik a háttérben, miközben a tartomány már kapcsolódik be a cél shard. A felosztása és egyesítése szolgáltatás mindig biztosítja a shard térkép tárolt megfeleltetést helyességét.


## <a name="the-split-merge-user-interface"></a>A felosztása és egyesítése felhasználói felület

A szolgáltatás felosztása és egyesítése csomag dolgozó szerepkört és a webes szerepkört tartalmazza. A webes szerepkört felosztása és egyesítése kérések küldhetik interaktív módon használják. A felhasználói felület fő összetevői a következők:

-    A művelet típusa: Művelet típusa: a szolgáltatás a kérelem végrehajtott művelet típusú vezérlők választógomb. A felosztott, választhat egyesítése és esetek áthelyezése. Egy korábban benyújtott műveletet is vonhatja vissza. Használjon felosztott, egyesítése és áthelyezések tartomány shard térképekhez. Lista shard leképezi csak a támogatási áthelyezés műveleteket.

-    Shard térkép: A következő szakaszban kérelem paraméterek információkat szeretne kapni a shard térkép és az adatbázis-szolgáltatója a shard térképre terjed ki. Mindenekelőtt meg kell adnia a nevét az Azure SQL-adatbázis kiszolgálója és az adatbázis szolgáltatója a shardmap, hitelesítő adatait, és csatlakozás a shard adatbázist, és végül a shard térkép nevét. A művelet jelenleg csak egy adott hitelesítő adatok csoportját fogad el. A hitelesítő adatokat kell rendelkezik a szükséges engedélyekkel végre szeretne hajtani a shards módosításokat a shard térképre, valamint a felhasználói adatok mód lehetőség használatával.

-    Forrástartományban (felosztása és egyesítése): egy felosztása és egyesítése művelet dolgozza fel a tartományt az alsó és felső billentyűt. Magas határolatlan kulcs értékkel művelet megadásához jelölje be a "magas kulcsa max" jelölőnégyzetet, és hagyja üresen a magas kulcsmező. Tartomány-értékeket megadott nem kell pontosan egyezik a megfeleltetés és a határokat a shard térképen. Ha egyáltalán nincs megadva bármelyik tartományban határai a szolgáltatás fog előállítani alapján a legközelebbi tartomány meg automatikusan. Az GetMappings.ps1 PowerShell parancsprogramot segítségével beolvashatja a megadott shard térképen aktuális megfeleltetések.

-    A felosztott forrás viselkedés (megosztott): osztott műveletekhez meghatározása a tartományának felosztása pontjára. Művelet, mert a sharding billentyűt, amelyre a felosztás fordul elő. Használja a választógomb adja meg a szeretné-e a tartomány (kivéve a felosztott billentyűt) alsó részén áthelyezése vagy szeretné-e a felső részen áthelyezése (beleértve a felosztott billentyűt).

-    A forrás Shardlet (áthelyezése): áthelyezése a műveletek azonosak a különböző felosztása és egyesítése műveletek nem igényel ahhoz a forrás tartomány. Áthelyezés forrás egyszerűen azonosítjuk a sharding kulcs értékét, amelyet használni szeretne helyezni.

-    TARGET (megosztott) Shard: Miután megadta az adatokat a felosztott tevékenység a forráson, meg kell határoznia, amelyhez, mert a cél az Azure SQL-adatbázis kiszolgáló és az adatbázis nevét kell másolni az adatokat.

-    Cél tartományban (egyesített): egyesítés művelet shardlets áthelyezése egy meglévő shard. A meglévő shard azáltal, hogy a meglévő tartomány, amely az egyesíteni kívánt tartomány határainak azonosította.

-    Köteg mérete: A Köteg mérete szabályozza, hogy a program elküldi az offline az adatok mozgás közben egyszerre shardlets számát. Ez a egy egész értéket hol használhatók a kisebb értékek érzékeny shardlets az állásidőt időszakok hosszú közben. Nagyobb értékekre növeli az idő, amely egy adott shardlet offline de javíthatja a teljesítményt.

-    Művelet azonosítója (Mégse): Ha egy folyamatban lévő műveletet, amely már nincs szükség, megszakíthatja a műveletet, mert a művelet Azonosítójával ebben a mezőben. A kérelem állapot táblából meghallgathatja a művelet Azonosítót (lásd: a szakasz 8.1) vagy a kimenet a webböngészőben, ahol benyújtott-e a kérést.


## <a name="requirements-and-limitations"></a>Követelmények és korlátai 

Az aktuális végrehajtása a felosztása és egyesítése szolgáltatás van vonatkoznak az alábbi követelmények és korlátozások: 

* A shards léteznek és regisztrálását shard térképen, a következő shards felosztása és egyesítése művelet végrehajtása előtt kell. 

* A szolgáltatás nem hoz létre a táblák vagy bármely más adatbázis-objektumok automatikusan műveleteihez részeként. Ez azt jelenti, hogy az összes sharded táblázatok és hivatkozás a séma kell-e a cél shard bármely felosztása és egyesítése/áthelyezés előtt található. Sharded táblák különösen van szükség lehet a tartomány, ahol új shardlets felosztása és egyesítése vagy áthelyezés hozzá az üres. Egyéb esetben a művelet sikertelen lesz a cél shard kezdeti konzisztencia ellenőrzése. Tartsa szem előtt, hogy adatokat csak másolja, ha a hivatkozás üres táblázatot, és, hogy nincsenek-e más egyidejű kapcsolatban konzisztencia garanciáit írja be a hivatkozás táblák műveletek hivatkozást. Azt javasoljuk, hogy ez: felosztása és egyesítése művelet futtatásakor más írási műveletek módosíthatja a hivatkozási táblák.

* A szolgáltatás egy egyedi index vagy kulcs, amely tartalmazza a sharding billentyűt javítható teljesítményének és megbízhatóságára vonatkozó nagy shardlets által létrehozott sor identitás támaszkodik. Adatok áthelyezése egyszerre egy értéket, mint csak sharding kulcs még finomabb granuláltságú a szolgáltatás lehetővé teszi. Ezzel az elemzéssel csökkentheti a legnagyobb mennyiségű naplózási hely és zárolások, amely a művelet során szükségesek. Fontolja meg egy egyedi index vagy egy elsődleges kulcsot, például egy adott tábla sharding billentyűjét, ha azt szeretné, hogy a táblázat használata felosztása és egyesítése/áthelyezések létrehozását. Teljesítmény okokból a sharding billentyűt az első oszlop az a billentyűt, vagy az index kell lennie.

* Összehívás feldolgozása során bizonyos shardlet adatok jelen, mind a forrás-és a cél shard lehetnek. Az hibák vírusvédelmet a shardlet mozgás során. Felosztása és egyesítése shard térképpel integrációja biztosítja, hogy kapcsolatok függő API-hoz a **OpenConnectionForKey** módszerrel a shard térképen útválasztás adatai között nem látható minden nem következetes köztes államok. Azonban való csatlakozáskor a forrás- vagy a Céllista shards nélkül a **OpenConnectionForKey** módszerrel, nem következetes köztes államok lehet látható felosztása és egyesítése/áthelyezések vannak megy. Ezek a kapcsolatok igazolhatja, attól függően, hogy az időzítés vagy a mögöttes a kapcsolat shard részleges vagy ismétlődő eredmények. A korlátozás jelenleg tartalmazza a skála rugalmas többfázisú-Shard-lekérdezések létrehozott kapcsolatok.

* A metaadat-alapú adatbázis felosztása és egyesítése szolgáltatás különböző szerepeket között nem kell megosztani. A szerepkör a fejlesztői futó felosztása és egyesítése szolgáltatás például kell mutatnia, mint a termelési szerepkör másik metaadat-adatbázishoz.
 

## <a name="billing"></a>A számlázás 

A felosztása és egyesítése szolgáltatás fut egy felhőalapú szolgáltatásba, a Microsoft Azure-előfizetésben. Ezért a szolgáltatás a példánya felhőszolgáltatások díjai alkalmazni. Csak a gyakori műveleteket hajt végre felosztása és egyesítése/áthelyezés, javasoljuk, hogy törli a felosztása és egyesítése felhőszolgáltatásba. Futó költségének menti vagy felhőalapú szolgáltatás példányaiban rendszerbe. Telepítheti újra és könnyen runnable beállításoktól induljon felosztása és egyesítése hajtanak végre kell. 
 
## <a name="monitoring"></a>Figyelése 
### <a name="status-tables"></a>Állapot-táblázatokban 

A felosztása és egyesítése szolgáltatás biztosítja, hogy a **RequestStatus** táblázatot a metaadat-alapú adatbázis kész vagy folyamatban lévő kérelmek figyelemmel kísérésére tárolására. A táblázat minden egyes felosztása és egyesítése kérelme a felosztása és egyesítése szolgáltatás példány küldött sor. Az alábbi adatok lekérése lehetőséget kínál:

* **Időbélyegző**: dátumát és idejét, mikor lett elindítva a a kérést.

* **OperationId**: egy globálisan egyedi azonosítója, amely egyedileg kell azonosítania a kérést. A kérés is használható megszakíthatja a műveletet, miközben még mindig folyamatban.

* **Állapot**: az aktuális állapotát a kérést. Folyamatban lévő kérelmekhez is jeleníti meg az aktuális fázis, amelyben a kérelem van.

* **CancelRequest**: jelző, amely azt jelzi, hogy a kérelmet megszakították.

* **Folyamatban**: a művelet befejezése A százalékos becslése. 50 érték azt jelzi, hogy a művelet körülbelül 50 % kész.

* **Részletek**: An XML érték, amely a részletes jelentések biztosít. A jelentések sorok csoportját forrásból származó másolandó cél rendszeresen frissül. Hibák vagy kivételeket ebben az oszlopban a hiba részletes információt is tartalmaz.


### <a name="azure-diagnostics"></a>Azure diagnosztika

A felosztása és egyesítése szolgáltatás Azure SDK 2,5 alapján figyelemmel kísérésére és diagnosztika Azure diagnosztika használja. Itt című cikkben ismertetett módon szabályozhatja a diagnosztikai konfigurációja: [Diagnosztika segítségével, így az Azure Felhőszolgáltatások és a virtuális gépeken futó](../cloud-services/cloud-services-dotnet-diagnostics.md). A letöltött csomag két diagnosztika konfiguráció – egy, a webes szerep és egy, a dolgozók szerep tartalmazza. A szolgáltatás diagnosztika őket a [Microsoft Azure-ban felhőalapú szolgáltatás kapcsolatos Alapismeretekről](https://code.msdn.microsoft.com/windowsazure/Cloud-Service-Fundamentals-4ca72649)hajtsa végre a lépéseket. A teljesítmény számláló, az IIS naplók, a Windows-eseménynaplók és felosztása és egyesítése alkalmazás eseménynaplók bejelentkezni definíciók tartalmazza. 

## <a name="deploy-diagnostics"></a>Diagnosztikai terjesztése 

Ahhoz, hogy figyelemmel kísérésére és a diagnosztikai konfiguráció használata a webes és dolgozó szerepkörök, a NuGet csomag által biztosított diagnosztika, a következő parancsokat Azure PowerShell használatával: 

    $storage_name = "<YourAzureStorageAccount>" 
    
    $key = "<YourAzureStorageAccountKey" 
    
    $storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key  
    
    
    $config_path = "<YourFilePath>\SplitMergeWebContent.diagnostics.xml" 
    
    $service_name = "<YourCloudServiceName>" 
    
    Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Production -Role "SplitMergeWeb" 
    
    
    $config_path = "<YourFilePath>\SplitMergeWorkerContent.diagnostics.xml" 
    
    $service_name = "<YourCloudServiceName>" 
    
    Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Production -Role "SplitMergeWorker" 

A beállítása és diagnosztikai beállítások telepítése További információt találhat: [Diagnosztika segítségével, így az Azure Felhőszolgáltatások és a virtuális gépeken futó](../cloud-services/cloud-services-dotnet-diagnostics.md). 

## <a name="retrieve-diagnostics"></a>Diagnosztikai beolvasása 

A diagnosztikai könnyen hozzáférhető Explorerből a Visual Studio kiszolgáló a kiszolgáló Explorer fa Azure részét. Nyissa meg a Visual Studio-példányt, és a menüsávon kattintson a nézet, majd a kiszolgáló Explorer. Kattintson az Azure előfizetés csatlakozhat az Azure ikonra. Nyissa meg Azure tárterület -> -> <your storage account> -> táblázatok -> WADLogsTable. További információért témakörökben [böngészési tároló kiszolgáló Intézővel](http://msdn.microsoft.com/library/azure/ff683677.aspx). 

![WADLogsTable][2]

A kiemelt a fenti ábrán WADLogsTable tartalmazza a felosztása és egyesítése szolgáltatás alkalmazás naplófájl részletes eseményeit. Figyelje meg, hogy az alapértelmezett beállítás a letöltött csomag egy éles üzemi részesíti előnyben. Az intervallumra, amelynél naplókról és a számláló kikerülnek az szolgáltatás példányainak ezért nagy (5 perc). Teszteléshez és a telepítéshez kisebb, az intervallum az internetről vagy a dolgozó szerepkör igény diagnosztika beállításainak módosításával. Kattintson a jobb gombbal a szerepkör a Visual Studio kiszolgáló Intéző (lásd fent), és adja meg a diagnosztika konfigurációs beállítások párbeszédpanel átadása időszak: 

![Konfiguráció][3]


## <a name="performance"></a>Teljesítmény

Az általános a teljesítmény növelése érdekében, hogy minél nagyobb további performant szolgáltatási rétegek Azure SQL-adatbázisban várható. A nagyobb szolgáltatási rétegek magasabb IO, a Processzor és a memória hozzárendelések indexeinek előnyös, és törölje a a felosztása és egyesítése szolgáltatás által használt műveletek. Emiatt növelje meg a szolgáltatási réteg csak az adott adatbázisok egy definiált, korlátozott ideig.

A szolgáltatás is a rendes működését részeként érvényességi lekérdezések hajt végre. Adatérvényesítési lekérdezések ellenőrizze a váratlan jelenléti adatok a cél tartományban található, és győződjön meg arról, hogy bármely felosztása és egyesítése/áthelyezés elindítja a egységes. Ezek a lekérdezések minden sharding határozza meg a műveletet hatókörének kulcs tartományok és a tétel méretét, a kérelem definíció részeként működik. Ezek a lekérdezések végrehajtása legjobb bemutató, amely tartalmazza a sharding billentyűt, mint az első oszlop esetén a tárgymutató 

Ezeken kívül az első oszlop sharding kulccsal egyediségét tulajdonság lehetővé teszi a szolgáltatást használatára optimalizált megközelítés, amely korlátozza az erőforrás-felhasználás naplózási hely és memóriahasználat szempontjából. Ez a tulajdonság egyediségét (általában fölött 1GB) nagyobb adatmennyiség áthelyezése van szükség. 

## <a name="how-to-upgrade"></a>A frissítés menete

1. Kövesse a [Deploy felosztása és egyesítése szolgáltatás](sql-database-elastic-scale-configure-deploy-split-and-merge.md).
2. A felhőalapú szolgáltatás konfigurációs fájl a felosztása és egyesítése telepítéshez megadott új konfigurációs paramétereknek megfelelően módosíthatja. Egy új szükséges paraméter titkosításhoz használt tanúsítvány adatait. Ehhez legegyszerűbben, összehasonlítása konfigurációs új sablonfájlt szemben a meglévő konfigurációs le. Ellenőrizze, hogy a "DataEncryptionPrimaryCertificateThumbprint" és "DataEncryptionPrimary" beállításait a webes és a dolgozó szerepkör hozzáadása.
3. Mielőtt a frissítés Azure győződjön meg arról, hogy az összes folyamatban felosztása és egyesítése művelet befejezése után. Is egyszerűen ehhez a RequestStatus és PendingWorkflows folyamatban lévő kérések felosztása és egyesítése metaadatok adatbázisból származó táblázatot lekérdezésével.
4. Az új csomag és a frissített szolgáltatás konfigurációs fájl frissítse a meglévő felhőalapú szolgáltatás üzembe felosztása és egyesítése az Azure-előfizetésben.

Szükségtelen hozhatók létre új metaadat-adatbázis felosztása és egyesítése frissíteni. Az új verzió automatikusan frissíti a meglévő metaadat-adatbázist az új verzióra. 

## <a name="best-practices--troubleshooting"></a>Ajánlott eljárások és hibaelhárítás
 
-    Adja meg a próba bérlői webhelyet, és gyakorolni a legfontosabb felosztása és egyesítése/áthelyezés műveletek a próba-bérlő több shards keresztül. Győződjön meg arról, hogy az összes metaadatok meg van adva megfelelően a shard map és, hogy a műveletek nem nem sértheti meg a kényszerek, és idegen kulcsok.
-    A próba-bérlő megtartása adatok méretét annak érdekében, hogy Ön nem tapasztalja adatok mérete a legnagyobb bérlői adatok maximális mérete feletti kapcsolatos problémákat. Ezzel az elemzéssel egy felső határ felmérheti a Navigálás egyetlen bérlő szükséges időt. 
-    Győződjön meg arról, hogy a séma lehetővé teszi, hogy törlését. A felosztása és egyesítése szolgáltatás megbizonyosodhat adat törlése a forrás shard, ha az adatok másolása sikeresen megtörtént a célba. Például **indítók törlése** megakadályozhatja, hogy a szolgáltatás a forrás lévő adatok törlése és vezethet művelet sikertelen lesz.
-    A sharding billentyűt kell lennie az első oszlop az elsődleges kulcsnak vagy egyedi index definíciója. Biztosítható, hogy a legjobb teljesítmény elérése érdekében a felosztása és egyesítése érvényességi lekérdezések és mindig működésbe sharding kulcs tartományok, amelyek tényleges adatok mozgását és törlési műveletekre vonatkozó.
-    A terület és adatok központban, az adatbázisok tároló a felosztása és egyesítése szolgáltatásban helymegosztást. 

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]



<!--Anchors-->
<!--Image references-->
[1]:./media/sql-database-elastic-scale-overview-split-and-merge/split-merge-overview.png
[2]:./media/sql-database-elastic-scale-overview-split-and-merge/diagnostics.png
[3]:./media/sql-database-elastic-scale-overview-split-and-merge/diagnostics-config.png
 
