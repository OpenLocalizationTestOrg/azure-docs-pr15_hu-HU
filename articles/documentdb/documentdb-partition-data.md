<properties 
    pageTitle="Szétválasztás és Azure DocumentDB méretezési |} Microsoft Azure"      
    description="További tudnivalók: hogyan particionáló működik, de Azure DocumentDB beállítása a szétválasztás, és a billentyűk partition és hogyan válassza ki a megfelelő partíciót billentyűt az alkalmazás."         
    services="documentdb" 
    authors="arramac" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter=""/> 

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/20/2016" 
    ms.author="arramac"/> 

# <a name="partitioning-and-scaling-in-azure-documentdb"></a>Szétválasztás és az Azure DocumentDB méretezése
[Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) segít elérése a gyors, kiszámítható a teljesítmény és a méretezés zökkenőmentes együtt az alkalmazás növekedésével meg lett tervezve. Ez a cikk áttekintést nyújt az hogyan particionáló működik, de DocumentDB, és ismerteti, hogy miként konfigurálható az alkalmazások hatékonyan átméretezheti a gyűjtemények DocumentDB.

Ez a cikk elolvasása, után fogja tudni az alábbi kérdésekre választ:   

- Honnan tudja az Azure DocumentDB particionáló munkát?
- Hogyan állítható be a DocumentDB a szétválasztás
- Mik azok a termékkulcsok, és hogyan végezze el tudom válassza ki a megfelelő partíciót billentyűt az alkalmazás?

Az első lépések a kódot, töltse le a projekt a [DocumentDB teljesítmény tesztelése illesztő minta](https://github.com/Azure/azure-documentdb-dotnet/tree/a2d61ddb53f8ab2a23d3ce323c77afcf5a608f52/samples/documentdb-benchmark). 

## <a name="partitioning-in-documentdb"></a>A DocumentDB szétválasztás

A DocumentDB tárolhatja és séma nélküli JSON dokumentumok a sorrend a ezredmásodperc a válaszidő bármely skála a lekérdezés. DocumentDB tárolók nyújt a **webhelycsoportok**nevű adatokat tároljon. Webhelycsoportok logikai erőforrásokat, és egy vagy több fizikai partíciót vagy a kiszolgálón is kiterjedhet. A partíciók száma DocumentDB tárhely méretét és a gyűjtemény a kiépített átviteli alapján határozza meg. DocumentDB az összes partíciót van társítva SSD biztonsági tároló rögzített méretű, és az elérhetőség magas replikált van. Partition management Azure DocumentDB teljesen kezeli, és nem rendelkezik összetett kódíráshoz, és kezelheti a partíciók. Webhelycsoportok DocumentDB **gyakorlatilag korlátlan** tárhely és a teljesítmény. 

Az alkalmazás teljesen átlátszó szétválasztás. DocumentDB támogatja a gyors olvasása és írása, SQL és LINQ lekérdezések, JavaScript-alapú tranzakció alapú logika, konzisztencia szintek és külön hozzáférés REST API hívások egyetlen webhelycsoport erőforrás keresztül. A szolgáltatás terjesztésével adatkezelési partíciót és a megfelelő partíciót útválasztási lekérdezés kérelmeket. 

Hogyan működik ez a funkció? Amikor egy webhelycsoport DocumentDB hoz létre, észre fogja, hogy van-e **partíciót kulcs** konfigurációs tulajdonságérték adhatja meg. Ez a JSON tulajdonság (vagy az elérési út) belül a az adatok között több kiszolgálók vagy partíciót terjesztheti DocumentDB által használható dokumentumok. DocumentDB kivonat az partíciót kulcs értékét, és a kivonatolt eredménye segítségével határozza meg a partíciót, amelyben a JSON dokumentumot szeretne tárolni. Az azonos partíciót a ugyanazzal a partíciót kulccsal összes dokumentumot szeretne tárolni. 

Például érdemes megvizsgálni tárolja az adatokat, alkalmazottak és osztályaik DocumentDB alkalmazást. Vegyük válassza a `"department"` annak érdekében, hogy az adatok részlegenként méretezése a partíciót kulcs tulajdonság. Minden dokumentum DocumentDB kell tartalmaznia egy kötelező `"id"` tulajdonságot, amelyet egyedinek kell lennie minden dokumentum azonos értékű partíciót kulcs, például `"Marketing`". A webhelycsoport összes dokumentumot pl. rendelkeznie partíciót billentyűt és id, egyedi kombinációját `{ "Department": "Marketing", "id": "0001" }`, `{ "Department": "Marketing", "id": "0002" }`, és `{ "Department": "Sales", "id": "0001" }`. Más szóval, az összetett tulajdonsága (partíciót esetén azonosító) a webhelycsoport elsődleges kulcsa.

### <a name="partition-keys"></a>Partition kulcsok
A választási lehetőségek, a partíciót kulcs egy fontos döntés, amely a tervezéskor ellenőrizze, hogy. Válasszon ki egy széles értékek és valószínű, hogy rendelkezik egyenletesen elosztott access mintázatok JSON-tulajdonság neve. A partíciók kulcs meg van adva a JSON elérési útját, mint például `/department` a tulajdonság részleg jelöli. 

A következő táblázat mutatja a példák partíciót kulcs definíciók és a megfelelő JSON értékeket.

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><strong>Partition kulcs elérési útja</strong></p></td>
            <td valign="top"><p><strong>Leírás</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>/Department</p></td>
            <td valign="top"><p>Hol található a dokumentumot a a dokumentum doc.department JSON értékének felel meg.</p></td>
        </tr>
        <tr>
            <td valign="top"><p>/ tulajdonságok/neve</p></td>
            <td valign="top"><p>Hol található a dokumentumot a a dokumentumot (beágyazott tulajdonság) doc.properties.name JSON értékének felel meg.</p></td>
        </tr>
        <tr>
            <td valign="top"><p>/ID</p></td>
            <td valign="top"><p>Doc.id JSON értékének felel meg (azonosítóját és partíciót kulcs azok az azonos tulajdonság).</p></td>
        </tr>
        <tr>
            <td valign="top"><p>/ "részleg neve"</p></td>
            <td valign="top"><p>Megfelel a JSON értékre a dokumentum ["név" részleg] hol a dokumentumot a dokumentum található.</p></td>
        </tr>
    </tbody>
</table>

> [AZURE.NOTE] Partition kulcs elérési útja szintaxisa hasonlít az elérési utat, amely megfelel az elérési utat a tulajdonság értékét helyett fő különbséget házirend útneveket indexelés, azaz és nincs helyettesítő végén. Ha például szeretné megadhatja/részleg /? indexelést állít be az értékeket a részleg, de adjon meg /department a partíciót-definíció. A partíciók kulcs elérési implicit módon indexelt, és használja az indexelő házirend érvényteleníti indexelésből nem kizárt.

Most tekintsünk át egy milyen hatással van a választható partíciót billentyűt az alkalmazás teljesítményét.

### <a name="partitioning-and-provisioned-throughput"></a>Particionáló és kiépített átviteli
DocumentDB készült kiszámíthatóan teljesítményét. Egy webhelycsoport létrehozásakor, lefoglalása **: [kérelem egységek](documentdb-request-units.md) (Licencelési)-másodpercenként**számát tekintve kapacitása. Minden egyes kérelmet, hogy a rendszer-erőforrásokat, például a Processzor és művelethez IO mennyiségét arányos kérelem egység díjat van-e hozzárendelve. A munkamenet konzisztencia 1 kB dokumentum olvasási 1 kérelem egység fogyaszt. Olvasás: 1 Licencelési tárolt elemek számát vagy a számát rendszert futtató azonos egyidejű kérelmek függetlenül. Nagyobb dokumentumoknál magasabb kérelem egységek méretétől függően. Ha tudja, hogy a személyek és az olvasás kell az alkalmazás támogatja a méretét, akkor is kiépítése az alkalmazás van szüksége a további szükséges átviteli pontos összegét. 

DocumentDB dokumentumokat tárolja, amikor elérhetővé teszi őket egyenletesen között partíciót partíciót kulcs értéke alapján. A teljesítmény akkor is egyenletesen elosztott között a rendelkezésre álló partíciók tehát a partíciót egy átviteli = (a webhelycsoport összes teljesítmény) / (partíciót száma). 

>[AZURE.NOTE] A gyűjtemény a teljes kapacitásának elérése érdekében ki kell választania egy partíciót kulcsot, amely lehetővé teszi, hogy egyenletes elosztása kérések számos különböző partíciót fő érték között.

## <a name="single-partition-and-partitioned-collections"></a>Egyetlen partíciót és particionált gyűjtemények
DocumentDB egyetlen-partíciót és is particionált gyűjtemények létrehozását támogatja. 

- **Partitioned gyűjtemények** több partíciót időtartomány és a nagyon nagy mennyiségű tárterület és a teljesítmény támogatja. Meg kell adnia a webhelycsoport partíciót kulcsot.
- **Egyetlen-partíciót gyűjtemények** alsó ár beállítások és az azt jelenti, hogy a lekérdezés, és hajtsa végre a tranzakciók az összes webhelycsoport adatok között van. Egy olyan partíciót méretezhetőség és tároló határain rendelkeznek. Nem kell adnia egy ezek a gyűjtemények partíciót kulcsot. 

![A DocumentDB particionált gyűjtemények][2] 

Ha nagy mennyiségű tároló vagy átviteli nem szükséges esetben partíciót gyűjtemények közelítését. Ne feledje, hogy egyetlen-partíciót gyűjtemények a méretezhetőség és tárterületkorlátok egyetlen partíciók, azaz tárterület és legfeljebb 10 000 kérelem mennyiség / szekundum akár 10 GB. 

Particionált gyűjtemények támogatniuk kell a nagyon nagy mennyiségű tárterület és a teljesítmény. Az alapértelmezett ajánlatok azonban vannak beállítva, hogy legfeljebb 250 GB-nyi tárhelyet tárolhatja, és legfeljebb 250 000 kérelem mennyiség / szekundum méretezni. Magasabb tároló vagy a webhelycsoport használati átviteli van szükség, ha kérjük, forduljon az [Azure-támogatási](documentdb-increase-limits.md) , hogy az alábbi nagyobb a fiókjához.

A következő táblázat felsorolja az egy egyetlen partíciót használata és particionált gyűjtemények közötti különbségeket:

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p></p></td>
            <td valign="top"><p><strong>Egyetlen partíciót gyűjtemény</strong></p></td>
            <td valign="top"><p><strong>A webhelycsoport particionált</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>Partition billentyűt</p></td>
            <td valign="top"><p>Nincs lehetőség</p></td>
            <td valign="top"><p>Szükséges</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Elsődleges kulcs dokumentum</p></td>
            <td valign="top"><p>"azonosító"</p></td>
            <td valign="top"><p>összetett kulcs &lt;partíciót kulcs&gt; és "azonosító"</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Minimális tárhely</p></td>
            <td valign="top"><p>0 GB</p></td>
            <td valign="top"><p>0 GB</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Maximális tárhely</p></td>
            <td valign="top"><p>10 GB</p></td>
            <td valign="top"><p>Korlátlan (250 GB alapértelmezés szerint)</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Minimális átviteli</p></td>
            <td valign="top"><p>400 kérelem mennyiség / szekundum</p></td>
            <td valign="top"><p>10 000 kérelem mennyiség / szekundum</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Maximális sebesség</p></td>
            <td valign="top"><p>10 000 kérelem mennyiség / szekundum</p></td>
            <td valign="top"><p>Korlátlan (kérés 250 000 egység másodpercenként alapértelmezés szerint)</p></td>
        </tr>
        <tr>
            <td valign="top"><p>API-verziókkal</p></td>
            <td valign="top"><p>Az összes</p></td>
            <td valign="top"><p>A Skype 2015 API-12-16 és újabb</p></td>
        </tr>
    </tbody>
</table>

## <a name="working-with-the-sdks"></a>A SDK használata

Azure DocumentDB hozzáadott Automatikus szétválasztás [Verzió 2015-12-16 REST API](https://msdn.microsoft.com/library/azure/dn781481.aspx)-val támogatása. Particionált gyűjtemények létrehozásához, le kell töltenie SDK verziók 1.6.0 vagy újabb verzió, az egyik a támogatott SDK platformon (.NET, Node.js, Java, Python). 

### <a name="creating-partitioned-collections"></a>Particionált webhelycsoportok létrehozása

A következő példában a .NET kódtöredékének eszköz telemetriai adatokat az átviteli másodpercenként 20 000 kérelem egységek tárolni gyűjtemény létrehozásához. A SDK OfferThroughput értékre állítja (amely a állítja be a `x-ms-offer-throughput` kérés fejléce a REST API). Itt be van állítva a `/deviceId` partíciót kulcsként. Partition kulcs megválasztása a webhelycsoport metaadatokat, például a név és az indexelő házirend a többi együtt menti a program.

Az Ez a példa azt kiválasztott `deviceId` tudjuk, amely (a) mivel eszközök nagyszámú találhatók, mivel írások is lehet partíciót keresztül egyenletesen elosztott és lehetővé teszi, hogy mi az adatbázist, nagy mennyiségű adattal ingest méretezése és (b) számos a kérelmeket, például a legújabb olvasási eszköz beolvasása az egy egyetlen deviceId korlátozódik egyetlen partíciót a tudja visszaszerezni.

    DocumentClient client = new DocumentClient(new Uri(endpoint), authKey);
    await client.CreateDatabaseAsync(new Database { Id = "db" });

    // Collection for device telemetry. Here the JSON property deviceId will be used as the partition key to 
    // spread across partitions. Configured for 10K RU/s throughput and an indexing policy that supports 
    // sorting against any number or string property.
    DocumentCollection myCollection = new DocumentCollection();
    myCollection.Id = "coll";
    myCollection.PartitionKey.Paths.Add("/deviceId");

    await client.CreateDocumentCollectionAsync(
        UriFactory.CreateDatabaseUri("db"),
        myCollection,
        new RequestOptions { OfferThroughput = 20000 });
        

> [AZURE.NOTE] Particionált webhelycsoportok létrehozása, meg kell adnia az > 10 000 kérelem erőforrás-mennyiség / szekundum átviteli értéket. Mivel az átviteli Többszörösök 100, azt 10,100 vagy újabb verziójában kell.

Ez a módszer teszi a REST API DocumentDB hívása, majd a szolgáltatás partíciót a kért átviteli alapján számos fog rendelkezni. A teljesítmény igények is alapkoncepciójára módosíthatja a gyűjtemény kapacitása. [Teljesítményszint](documentdb-performance-levels.md) talál további információt.

### <a name="reading-and-writing-documents"></a>Olvasásra és írásra dokumentumok

Most vegyük beszúrni DocumentDB adatokat. Az alábbiakban egy minta osztály tartalmazó olvasási eszközt, és hívást kezdeményez CreateDocumentAsync szeretne beszúrni olvasási gyűjteménybe új eszköz.

    public class DeviceReading
    {
        [JsonProperty("id")]
        public string Id;

        [JsonProperty("deviceId")]
        public string DeviceId;

        [JsonConverter(typeof(IsoDateTimeConverter))]
        [JsonProperty("readingTime")]
        public DateTime ReadingTime;

        [JsonProperty("metricType")]
        public string MetricType;

        [JsonProperty("unit")]
        public string Unit;

        [JsonProperty("metricValue")]
        public double MetricValue;
      }

    // Create a document. Here the partition key is extracted as "XMS-0001" based on the collection definition
    await client.CreateDocumentAsync(
        UriFactory.CreateDocumentCollectionUri("db", "coll"),
        new DeviceReading
        {
            Id = "XMS-001-FE24C",
            DeviceId = "XMS-0001",
            MetricType = "Temperature",
            MetricValue = 105.00,
            Unit = "Fahrenheit",
            ReadingTime = DateTime.UtcNow
        });


Vegyük olvasni partíciót billentyűt, és azonosítója, a dokumentum frissítése, és, utolsó lépésként törölje partíciót billentyűt, és azonosító. Jegyzet, hogy az olvasás tartalmazzák PartitionKey érték (megfelelő a `x-ms-documentdb-partitionkey` kérés fejléce a REST API).

    // Read document. Needs the partition key and the ID to be specified
    Document result = await client.ReadDocumentAsync(
      UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
      new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });

    DeviceReading reading = (DeviceReading)(dynamic)result;

    // Update the document. Partition key is not required, again extracted from the document
    reading.MetricValue = 104;
    reading.ReadingTime = DateTime.UtcNow;

    await client.ReplaceDocumentAsync(
      UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
      reading);

    // Delete document. Needs partition key
    await client.DeleteDocumentAsync(
      UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
      new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });



### <a name="querying-partitioned-collections"></a>Particionált gyűjtemények lekérdezése

Ha particionált tartozó adatok lekérdezéséhez DocumentDB automatikusan irányítja a lekérdezést a partíciók megfelelő partíciót legfontosabb értékeit a szűrőben megadott (ha vannak). A lekérdezés például csak az a partíciót billentyűt "XMS-0001" tartalmazó partíciót rendszer irányítja.

    // Query using partition key
    IQueryable<DeviceReading> query = client.CreateDocumentQuery<DeviceReading>(
        UriFactory.CreateDocumentCollectionUri("db", "coll"))
        .Where(m => m.MetricType == "Temperature" && m.DeviceId == "XMS-0001");

Az alábbi lekérdezés nincs szűrő a partíciót kulcs (DeviceId), és az összes partíciót hol végre szemben a partíciót index van rendezve. Megjegyzés: a EnableCrossPartitionQuery megadhatja, hogy (`x-ms-documentdb-query-enablecrosspartition` a REST API), hogy a lekérdezés végrehajtása keresztül partíciót SDK csomagjában talál.

    // Query across partition keys
    IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
        UriFactory.CreateDocumentCollectionUri("db", "coll"), 
        new FeedOptions { EnableCrossPartitionQuery = true })
        .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100);

### <a name="parallel-query-execution"></a>A párhuzamos lekérdezés végrehajtása

A DocumentDB SDK 1.9.0 és hajthat végre particionált gyűjtemények, kis késés lekérdezéseket, akkor is, ha az érintéssel működő partíciót nagyszámú szükségük támogatási párhuzamos lekérdezés végrehajtása lehetőségek fölé. A következő lekérdezés például keresztül partíciót párhuzamosan futó van beállítva.

    // Cross-partition Order By Queries
    IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
        UriFactory.CreateDocumentCollectionUri("db", "coll"), 
        new FeedOptions { EnableCrossPartitionQuery = true, MaxDegreeOfParallelism = 10, MaxBufferedItemCount = 100})
        .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100)
        .OrderBy(m => m.MetricValue);

A párhuzamos lekérdezés-végrehajtási kezelheti a következő paraméterek beállítása:

- Megadásával `MaxDegreeOfParallelism`, megadhatja, hogy a párhuzamos, azaz egyidejű hálózati kapcsolatok, a webhelycsoport partíciók maximális száma mértékét. Ha a -1, hogy a párhuzamos fokú kezeli a SDK csomagjában talál. Ha a `MaxDegreeOfParallelism` nem megadott vagy állítsa 0, amely az alapértelmezett érték, a webhelycsoport partíciót egy hálózati kapcsolat lesz.
- Megadásával `MaxBufferedItemCount`, akkor is kereskedelmi lekérdezési időtartama és az ügyfél egymás memória kihasználtsági ki. Ha ezt a paramétert elhagyja, vagy állítsa be ezt a -1, párhuzamos lekérdezés végrehajtása során pufferelt elemek számát a SDK kezeli.

A webhelycsoport állapotának azonos adni, párhuzamos lekérdezés ad vissza eredmények ugyanabban a sorrendben, ahogy a soros végrehajtása. Idegen-partíciót kérdezni, amely tartalmazza a rendezés (ORDER BY és/vagy TOP), amikor a DocumentDB SDK csomagjában talál problémák a lekérdezés párhuzamosan át partíciók, és egyesíti a globális rendelt találatok összegyűjtéséhez ügyféloldali részben rendezett eredményez.

### <a name="executing-stored-procedures"></a>Tárolt eljárások végrehajtása

Dokumentumok az azonos Eszközazonosítót atomi tranzakciókat is futtathat, például ha éppen fenntartása, összegzések vagy egy eszközt egy dokumentumban legújabb állapotát. 

    await client.ExecuteStoredProcedureAsync<DeviceReading>(
        UriFactory.CreateStoredProcedureUri("db", "coll", "SetLatestStateAcrossReadings"),
        new RequestOptions { PartitionKey = new PartitionKey("XMS-001") }, 
        "XMS-001-FE24C");

A következő szakaszban akkor tekintse meg hogyan léphet particionált gyűjtemények egyetlen-partíciót gyűjtemények.

<a name="migrating-from-single-partition"></a>
### <a name="migrating-from-single-partition-to-partitioned-collections"></a>Áttérés a egyetlen-partíciót particionált felhasználócsoportoknak
Ha van szüksége a egyetlen-partíciót gyűjtemény az alkalmazás újabb átviteli (> 10 000 Licencelési/s) vagy nagyobb adattárolás (> 10 GB-os), a [DocumentDB adatok áttelepítési eszköz](http://www.microsoft.com/downloads/details.aspx?FamilyID=cda7703a-2774-4c07-adcc-ad02ddc1a44d) használatával az adatokat az egyetlen-partíciót gyűjteményből áttelepítése particionált gyűjteménye. 

Egy egyetlen-partíciót gyűjteményből áttelepítése particionált gyűjteménye

1. Adatok exportálása az egyetlen-partíciót gyűjteményből JSON. További információt a [JSON-fájl exportálása](documentdb-import-data.md#export-to-json-file) talál.
2. Adatokat importál egy partíciót-definíciót és több mint 10 000 kérelem mennyiség / második átviteli, létrehozott particionált gyűjteménybe az alábbi példában látható módon. [Importálás DocumentDB](documentdb-import-data.md#DocumentDBSeqTarget) talál további információt.

![Adatok áttelepítése DocumentDB Partitioned gyűjteménye][3]  

>[AZURE.TIP] Gyorsabb importálása időpontok érdemes megfontolni a számot a párhuzamos kérések 100 vagy annál kihasználhatja a magasabb átviteli particionált webhelycsoportok számára érhető el. 

Most, hogy végzett alapvető, nézzük meg néhány fontos tervezési szempontjait DocumentDB a termékkulcsok való munkavégzés során.

## <a name="designing-for-partitioning"></a>Szétválasztás megtervezése
A választási lehetőségek, a partíciót kulcs egy fontos döntés, amely a tervezéskor ellenőrizze, hogy. Ebben a részben néhány kijelölése a webhelycsoport partíciót kulcsot részt kompromisszumok mutatja be.

### <a name="partition-key-as-the-transaction-boundary"></a>Partition billentyűt, a tranzakció határ
Partition kulcs választási kell egyenleg szükséges a tranzakciókat több termékkulcsok méretezhető megoldást a biztosítása érdekében a szervezetek elosztása követelménye használatának engedélyezése. Egy rendkívüli ugyanazt a partíciót kulcsot minden dokumentum sikerült beállítani, de ez korlátozhatják a méretezhetőség a megoldás. A rendkívüli, a minden dokumentum esetén erősen méretezhető lenne, de szeretné megakadályozzák a több dokumentum tranzakciók tárolt eljárások és indítók segítségével egy egyedi partíciót kulcs rendelheti. Az ideális partíciót billentyűt az egyik a, amely lehetővé teszi, hogy hatékony lekérdezésekkel és annak érdekében, hogy a megoldás méretezhető elegendő hivatkozás van. 

### <a name="avoiding-storage-and-performance-bottlenecks"></a>Tárterület és a teljesítmény szűk elkerülése 
Fontos a Válasszon egy tulajdonságot, amely lehetővé teszi, hogy legyen a különböző értékek száma elosztva írások is. Partition ugyanazt a kulcsot kérelmek nem haladhatja meg a kapacitásának egyetlen partíciót, és fog kell szabályozott. Ezért fontos, hogy válasszon egy partíciót kulcsot, amely nem vezet **"interaktív területek"** , az alkalmazáson belül. A dokumentumok ugyanazt a partíciót kulcsot a teljes tárterület méretét is is legfeljebb 10 GB-os tároló. 

### <a name="examples-of-good-partition-keys"></a>Példák a helyes kulcsok
Íme néhány példa, hogy hogyan válassza ki a partíciót billentyűt az alkalmazás:

* A felhasználói profil kódmentes esetén végrehajtási, ha a felhasználói azonosító partíciót kulcs kinek ajánljuk.
* Korlátot IoT adatok, például az eszköz állapotát, ha olyan Eszközazonosítót érdemes partíciót billentyű.
* Ha DocumentDB naplózás idősorok adatok használata esetén, a hostname (állomásnév) vagy a folyamat Azonosítóra partíciót kulcs kinek ajánljuk.
* Ha a több elem bérlői architektúrája, a a bérlői Azonosítóra partíciót kulcs kinek ajánljuk.

Ne feledje, hogy bizonyos esetekben használja (például a fentebb ismertetett IoT és a felhasználói profil) a partíciót kulcs lehet ugyanaz, mint az azonosító (dokumentum billentyűt). Más, például az időt adatsor lehet, hogy egy partíciót kulcs eltérő azonosítója.

### <a name="partitioning-and-loggingtime-series-data"></a>Particionáló és naplózás/idősorok adatok
A leggyakoribb használati eset DocumentDB az egyik és a telemetriai naplózás. Fontos, hogy mivel nagy mennyiségű adattal olvasási/írási lehet szükség, válassza ki egy jó partíciót billentyűt. A választási lehetőségek attól függenek, az Olvasás és írja be a díjértékeket és a lekérdezések futtatásához várt típusú. Az alábbiakban néhány tipp a válassza ki a helyes billentyűt.

- Ha a használati eset jár kisebb mértékű acculumating átírva hosszabb idő, és szükség szerint időbélyegeket cellatartományok lekérdezés és más szűrők, akkor használja az például egy összegző az időbélyegző dátum partíciót kulcs egy jó módszer szerint. Így lekérdezés összes adatot felett egy dátumra a egyetlen partíciót. 
- Ha a terhelést a írási nehéz, amely általában gyakrabban, egy nem alapuló időbélyeg, hogy DocumentDB is írások egyenletes elosztása partíciót számos különböző partíciót kulcsot kell használnia. A hostname (állomásnév), a folyamat azonosítója, a Tevékenységazonosító vagy a nagy hivatkozás egy másik tulajdonságot az alábbiakban kinek ajánljuk. 
- A harmadik megközelítés, egy hibrid egyik ahol több gyűjtemények, egy minden nap/hónap és a partíciót kulcs egy részletes tulajdonság, például a hostname (állomásnév). Beállíthatja, hogy a időkeret alapján különböző teljesítményszint segítsége azt, például az aktuális hónap azon webhelycsoportjának van kiépítve magasabb átviteli óta szolgál, olvasása és írása, mivel az előző hónapok alsó átviteli, mivel ezek csak Olvasás szolgálnak.

### <a name="partitioning-and-multi-tenancy"></a>Szétválasztás és a több elem bérleti
Több elem bérlői alkalmazást DocumentDB vannak végrehajtási, esetén két fő mintázatok DocumentDB – a bérleti végrehajtásához bérlőnként egy partíciót billentyűt, és egy webhelycsoport bérlőnként. Az alábbiakban a szakemberek és a negatívumokat, minden egyes:

* Bérlőnként egy partíciót kulcs: Ebben a modellben bérlők collocated egyetlen gyűjtemény belül. De lekérdezések és beszúrása a dokumentuma egy bérlői webhelyre ellen egyetlen partíciót hajtható végre. A bérlői webhely összes dokumentuma keresztül tranzakció alapú logika is alkalmazhat. Mivel több bérlők gyűjtemény megosztásához tárhely és a teljesítmény költségek bérlők egyetlen gyűjtemény belül megosztott erőforrásokat, ahelyett hogy kiépítési további belmagassága minden bérlő mentheti. Hátránya, hogy nincs teljesítmény elkülönítési bérlőnként. Teljesítmény/átviteli nő alkalmazása a teljes webhelycsoport viewben célzott növeli a bérlők.
* Bérlőnként egy webhelycsoport: minden bérlői van a saját gyűjteményéhez. Ebben a modellben foglalhat bérlőnként teljesítményét. Modellel DocumentDB meg új alapú felhasználási árak a modell költséghatékonyabb, amelyben kisszámú bérlők több bérlői alkalmazások.

Kis bérlők collocates és saját webhelycsoport nagyobb bérlők áttelepíti a kombinált/többszintű megközelítés is használhatja.

## <a name="next-steps"></a>Következő lépések
Ez a cikk azt már leírása hogyan particionáló működik, de Azure DocumentDB, hogyan hozhat létre particionált webhelycsoportok és hogyan is választhat egy jó partíciót billentyűt az alkalmazás. 

-   Hajtsa végre a méretezés és teljesítményének a DocumentDB tesztelésekor. [A teljesítmény és a méretezés az Azure DocumentDB tesztelése](documentdb-performance-testing.md) minta talál.
-   Első lépések a [SDK](documentdb-sdk-dotnet.md) vagy a [REST API](https://msdn.microsoft.com/library/azure/dn781481.aspx) coding
-   További tudnivalók [a DocumentDB kiépített átviteli](documentdb-performance-levels.md) :
-   Ha azt szeretné, ha testre szeretné szabni, hogy hogyan az alkalmazás végrehajt, szétválasztás, csatlakoztathatja a saját ügyféloldali particionáló végrehajtása. Lásd: az [ügyféloldali szétválasztás támogatás](documentdb-sharding.md).

[1]: ./media/documentdb-partition-data/partitioning.png
[2]: ./media/documentdb-partition-data/single-and-partitioned.png
[3]: ./media/documentdb-partition-data/documentdb-migration-partitioned-collection.png  

 
