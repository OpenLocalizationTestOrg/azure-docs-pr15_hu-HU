<properties
    pageTitle="Adatkapcsolat: adatfolyam-esemény adatfolyam bemenetben |} Microsoft Azure"
    description="Megtudhatja, hogyan állíthatja be a "ráfordítások" nevű adatfolyam Analytics származó adatok. Bemenetben események adatfolyam tartalmazza, és adatokat is hivatkozhat."
    keywords="adatfolyam, adatkapcsolatot, esemény-adatfolyam"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>

# <a name="data-connection-learn-about-data-stream-inputs-from-events-to-stream-analytics"></a>Adatkapcsolat: További tudnivalók: adatok adatfolyam forráson események Értékáram-elemzés

Értékáram-elemzés adatkapcsolat események adatforrásból származó adatok adatfolyam. Link-"bemenet." Értékáram-elemzés osztályú funkciók integrálása az Azure adatfolyam források esemény központi IoT központi és Blob adattárolás, amely a analytics feladat ugyanazon vagy másik Azure előfizetésről tartalmaz.

## <a name="data-input-types-data-stream-and-reference-data"></a>Adatok bevitele típusok: adatok adatfolyam és a hivatkozási adatok
Adatok adatforrás tolódik, mint azt a Értékáram-elemzés feladat elfogyasztott és valós idejű feldolgozás. Bemenetben két különböző típusú oszthatók: adatok adatfolyam-ráfordítások és hivatkozási adatok bemeneti adatok alapján.

### <a name="data-stream-inputs"></a>Adatfolyam-bemeneti adatok alapján
Adatfolyam határolatlan események várhatók időbeli sorrendjét. Adatfolyam-elemző feladatok tartalmaznia kell legalább egy adatfolyam adatbevitel elfogyasztott és a transzformált által a feladatot. Adatforrások adatfolyam beviteli BLOB-tárolóhoz, esemény hubok és IoT hubok támogatottak. Esemény hubok gyűjthetők össze az esemény adatfolyamok több eszközök és szolgáltatások, például a tevékenység-hírcsatornák közösségi hálózat, tőzsdei kereskedelmi információk vagy érzékelők adatainak használják. IoT hubok optimalizált adatokat gyűjt a csatlakoztatott eszközök dolog Internet (IoT) helyzetekben.  Tömeges adatok adatfolyamként ingesting BLOB-tárolóhoz használható az beviteli forrásaként.  

### <a name="reference-data"></a>Hivatkozási adatok
Értékáram-elemzés egy második típusú beviteli hivatkozási adatok neve támogatja. Ez a külső adatokat, amelyek statikus vagy lassan módosítása adott idő alatt, és általában használatos korrelációs és look-ups hajt végre. Azure Blob-tárolóhoz jelenleg hivatkozási adatok egyetlen támogatott beviteli forrását. Hivatkozási adatok forrása BLOB korlátozódik 100 MB méretű.
Útmutató: adatok bemenetben hivatkozást hoz létre, lásd: [Használata hivatkozási adatok](stream-analytics-use-reference-data.md)  

## <a name="create-a-data-stream-input-with-an-event-hub"></a>Hozzon létre egy esemény hubhoz adatfolyam adatbevitel

[Azure esemény hubok](https://azure.microsoft.com/services/event-hubs/) erősen méretezhető esemény ingestor közzététel-előfizetés. Azt, hogy a folyamat, és elemzése a nagy mennyiségű adatot a csatlakoztatott eszközök és alkalmazások által gyártott gyűjt események másodpercenként, több millió. Értékáram-elemzés a leggyakrabban használt felhasználandó egyike. Esemény elosztók és a megjelenítő Értékáram-elemzés közös ügyfelek-végpont megoldást nyújt valós idejű analytics. Esemény hubok segítségével a felhasználók valós idejű Azure-hírcsatorna eseményeket, és Értékáram-elemzés feladatok is feldolgozni azokat valós időben. Például ügyfelek webes gombra kattint, érzékelő kiejtés, online naplóbeli események küldése esemény hubok, és a bemeneti adatok adatfolyamként esemény hubok használható valós idejű szűrés, összesítése és korrelációs Értékáram-elemzés feladatok létrehozása.

Fontos, hogy ne feledje, hogy az alapértelmezett időbélyeg Értékáram-elemzés az esemény hubok érkező események az időbélyegző, amely az eseményre az esemény-központban, amely EventEnqueuedUtcTime érkeztek. Az adatok feldolgozása időbélyeg használata a rendezvény tartalom adatfolyamként, az [Időbélyegző által](https://msdn.microsoft.com/library/azure/dn834998.aspx) kulcsszó kell használni.

### <a name="consumer-groups"></a>Fogyasztói csoportok

Minden adatfolyam Analytics esemény központi beviteli kell beállítania, hogy saját fogyasztói csoport. A feladat tartalmazó saját illesztés vagy több bemenetben esetén néhány beviteli egynél több olvasó után, amely hatással van az olvasóknak egyetlen fogyasztói csoport száma szerint olvasható. Meghaladja az 5 olvasók fogyasztói csoportonként partíciót egy esemény központi korlát elkerülése végett érdemes egyes megjelenítő Értékáram-elemzés feladat fogyasztói csoport kijelölése. Figyelje meg, hogy van-e még legfeljebb 20 fogyasztói csoportok használati események központi. A részletekért lásd: az [Esemény hubok programozási útmutató](../event-hubs/event-hubs-programming-guide.md).

### <a name="configure-event-hub-as-an-input-data-stream"></a>Állítsa be az esemény központi egy bemeneti adatok adatfolyamként

Az alábbi táblázat ismerteti a minden tulajdonság abban az esetben, ha központi figyelmeztető leírását tartalmazó lap:

| TULAJDONSÁG NEVE | LEÍRÁS |
|------|------|
| Beviteli Alias | Egy rövid nevet, a bemeneti hivatkozni szeretne a feladat lekérdezésben használt |
| Szolgáltatás Bus Namespace | A szolgáltatás Bus névtere üzenetküldés személyek halmaza tárolója. Ha létrehozott egy új esemény hubon keresztül csatlakozott, szolgáltatás Bus névteret is létrehozott. |
| Esemény központi | A beviteli esemény központi neve. |
| Esemény központi házirend neve | A megosztott access szabályt, amely az esemény központi konfigurálása lapon létrehozhatók. Minden megosztott hozzáférési házirend beállított engedélyek, nevet, és a hívóbillentyűk. |
| Esemény központi házirend kulcs | A megosztott hívóbetű hitelesíti a szolgáltatás Bus névtér való hozzáférést. |
| Esemény központi fogyasztói csoport (nem kötelező) | A fogyasztói csoport ingest adatokat a rendezvény központból. Ha nincs megadva, Értékáram-elemzés feladatok fogja használni a fogyasztói alapértelmezett csoport ingest adatokat a rendezvény központból. Egy egyedi fogyasztói csoport minden Értékáram-elemzés feladat használata ajánlott. |
| Esemény szerializálási formátum | A lekérdezések működjenek a kívánt módon, Értékáram-elemzés kell, hogy melyik szerializálási formátum (JSON, CSV vagy Avro) használata esetén a bejövő adatfolyamok. |
| Kódolás | UTF-8 jelenleg csak az támogatott kódolási formátumot. |

Ha az adatok érkezik egy esemény központi forrás, érheti el néhány metaadatok mezők Értékáram-elemzés lekérdezésbe. Az alábbi táblázat felsorolja a mezők és azok leírását.

| A TULAJDONSÁG | LEÍRÁS |
|------|------|
| EventProcessedUtcTime | Dátum és idő, amely az esemény szerint megjelenítő Értékáram-elemzés feldolgozása. |
| EventEnqueuedUtcTime | Dátum és idő, amely az esemény megkapta az esemény hubok. |
| PartitionId | A beviteli kártya nulla alapú partíciót azonosítója. |

Előfordulhat, hogy például írhat lekérdezés például a következőket:

````
SELECT
    EventProcessedUtcTime,
    EventEnqueuedUtcTime,
    PartitionId
FROM Input
````

## <a name="create-an-iot-hub-data-stream-input"></a>Hozzon létre egy központi IoT adatbevitel adatfolyam

Azure Iot is méretezhető erősen közzététel előfizetés esemény ingestor IoT esetek optimalizálva.
Fontos, hogy ne feledje, hogy az alapértelmezett időbélyeg IoT hubok adatfolyam Analytics-ból érkező események az időbélyegző, amely az esemény érkezett, amely EventEnqueuedUtcTime IoT-központban. Az adatok feldolgozása időbélyeg használata a rendezvény tartalom adatfolyamként, az [Időbélyegző által](https://msdn.microsoft.com/library/azure/dn834998.aspx) kulcsszó kell használni.

> [AZURE.NOTE] Csak egy DeviceClient tulajdonság küldött üzenetek is dolgozható fel.

### <a name="consumer-groups"></a>Fogyasztói csoportok

Minden adatfolyam Analytics IoT központi beviteli kell beállítania, hogy saját fogyasztói csoport. A feladat tartalmazó saját illesztés vagy több bemenetben esetén néhány beviteli egynél több olvasó után, amely hatással van az olvasóknak egyetlen fogyasztói csoport száma szerint olvasható. Meghaladja az 5 olvasók fogyasztói csoportonként partíciót egy központi IoT határértékén elkerülése végett érdemes egyes megjelenítő Értékáram-elemzés feladat fogyasztói csoport kijelölése.

### <a name="configure-iot-hub-as-an-data-stream-input"></a>Állítsa be a IoT központi egy bemeneti adatok adatfolyamként

Az alábbi táblázat ismerteti, minden egyes tulajdonság leírását a bemeneti IoT központi lapon:

| TULAJDONSÁG NEVE | LEÍRÁS |
|------|------|
| Beviteli Alias | Egy rövid nevet, a bemeneti hivatkozni szeretne a feladat lekérdezésben használt. |
| IoT központi | Egy IoT tároló egy üzenetben szervezetek számára is. |
| Végpont | A központi IoT végpontjának neve. |
| Megosztott hozzáférési házirend neve | A megosztott hozzáférési házirend adhat hozzáférést az IoT-központban. Minden megosztott hozzáférési házirend beállított engedélyek, nevet, és a hívóbillentyűk. |
| Megosztott házirend hívóbetű | A megosztott hívóbetű hitelesíti IoT-hubon keresztül csatlakozott az access. |
| (Nem kötelező) a fogyasztói csoport | Az adatok a IoT központból ingest fogyasztói csoport. Ha nincs megadva, Értékáram-elemzés feladatok fogja használni a fogyasztói alapértelmezett csoport ingest adatok a IoT központból. Egy egyedi fogyasztói csoport minden Értékáram-elemzés feladat használata ajánlott. |
| Esemény szerializálási formátum | A lekérdezések működjenek a kívánt módon, Értékáram-elemzés kell, hogy melyik szerializálási formátum (JSON, CSV vagy Avro) használata esetén a bejövő adatfolyamok. |
| Kódolás | UTF-8 jelenleg csak az támogatott kódolási formátumot. |

Ha az adatok IoT központi listában elérhető lesz, érheti el néhány metaadatok mezők Értékáram-elemzés lekérdezésbe. Az alábbi táblázat felsorolja a mezők és azok leírását.

| A TULAJDONSÁG | LEÍRÁS |
|------|------|
| EventProcessedUtcTime | Dátum és idő, amely az esemény feldolgozása. |
| EventEnqueuedUtcTime | Dátum és idő, amely az esemény megkapta az IoT-központban. |
| PartitionId | A beviteli kártya nulla alapú partíciót azonosítója. |
| IoTHub.MessageId | Használja összehangolására kétirányú kommunikációt IoT-központban. |
| IoTHub.CorrelationId | Üzenet a válaszok és IoT központi visszajelzést használt. |
| IoTHub.ConnectionDeviceId | Az üzenet által IoT központi servicebound üzenetekre ellátott küldi hitelesített azonosítója. |
| IoTHub.ConnectionDeviceGenerationId | A hitelesített eszköz lehet küldeni az üzenetet, a generationId által IoT központi servicebound üzenetekre ellátott. |
| IoTHub.EnqueuedTime | Ha az üzenetet megkapta IoT központi ideje. |
| IoTHub.StreamId | A feladó eszköz által hozzáadott egyéni eseménytulajdonság. |

## <a name="create-a-blob-storage-data-stream-input"></a>Hozzon létre egy Blob tároló adatfolyam adatbevitel

Ha nagy mennyiségű strukturálatlan adatokat a felhőben tárolhatja esetben Blob-tárolóhoz kínál költséghatékony és méretezhető megoldást. [Blob-tárolóban](https://azure.microsoft.com/services/storage/blobs/) lévő adatok általában számít "a többi" adatokat, de azt is feldolgoztatni adatok adatfolyamként Értékáram-elemzés. Egy szokták Blob tároló bemeneti adatok alapján, a megjelenítő Értékáram-elemzés napló feldolgozása, ahol telemetriai rendszerből rögzíthető, és elemzése és bontsa ki az adatokat értelmes feldolgozott kell.

Fontos, hogy ne feledje, hogy az alapértelmezett időbélyeg Blob tároló események Értékáram-elemzés az időbélyegző, hogy a blob utolsó módosítás dátuma mely *isBlobLastModifiedUtcTime*. Az adatok feldolgozása időbélyeg használata a rendezvény tartalom adatfolyamként, az [Időbélyegző által](https://msdn.microsoft.com/library/azure/dn834998.aspx) kulcsszó kell használni.

Tartsa szem előtt, hogy a vesszőkkel bemenetben **megkövetelése** meghatározása az adatkészlet mezőinek fejlécsor. További sor fejlécmezők összes **egyedinek**kell lennie.

> [AZURE.NOTE] Értékáram-elemzés nem támogatja a tartalmak hozzáadása egy meglévő blob. Értékáram-elemzés csak megtekinthetik a blob, miután, illetve a módosításokat végrehajtani, miután ez olvasható nem lehet feldolgozni. Az ajánlott egyszer tölthet fel az összes adatot, és bármilyen további eseményeket nem adhat hozzá a blob-tárolóban.

Az alábbi táblázat minden egyes tulajdonság leírását tartalmazó lapon beviteli Blob tároló ismerteti:

<table>
<tbody>
<tr>
<td>TULAJDONSÁG NEVE</td>
<td>LEÍRÁS</td>
</tr>
<tr>
<td>Beviteli Alias</td>
<td>Egy rövid nevet, a bemeneti hivatkozni szeretne a feladat lekérdezésben használt.</td>
</tr>
<tr>
<td>Tárterület-fiók</td>
<td>Hol találhatók a blob-fájlok a tárterület-fiók neve.</td>
</tr>
<tr>
<td>Tárterület Fiókkulcs</td>
<td>A titkos kulcs a tárhely partnernél.</td>
</tr>
<tr>
<td>Tárterület-tárolóhoz
</td>
<td>Tárolók adja meg a Microsoft Azure Blob-szolgáltatásban tárolt BLOB-logikai csoportja. Amikor blob feltöltése a Blob-szolgáltatás, meg kell adnia, hogy a blob-tároló.</td>
</tr>
<tr>
<td>Elérési út előtag mintát [választható]</td>
<td>A fájl elérési útja, a megadott tárolóban a BLOB megkeresésére.
A PATH tudja kiválasztani az alábbi 3 változót egy vagy több példány megadása:<BR>{date}, {idő}<BR>{partíciót}<BR>Példa: 1: cluster1/naplók / {date} / {time} / {partition}<BR>Példa 2: cluster1/naplók / {date}<P>Megjegyzendő, hogy "*" nem engedélyezett pathprefix érték. Csak érvényes <a HREF="https://msdn.microsoft.com/library/azure/dd135715.aspx">Azure blob-karakterek</a> használhatók.</td>
</tr>
<tr>
<td>A dátumformátum [választható]</td>
<td>Ha a dátum jogkivonat az előtag elérési utat, választhat a dátumformátumot, amelyben a fájlok vannak rendezve. Példa: YYYY/hh/nn</td>
</tr>
<tr>
<td>Időformátum [választható]</td>
<td>Ha az időt jogkivonat az előtag elérési utat, adja meg az időformátumot, ahol a fájlok vannak rendezve. Jelenleg az egyetlen támogatott érték HH.</td>
</tr>
<tr>
<td>Esemény szerializálási formátum</td>
<td>A lekérdezések működjenek a kívánt módon, Értékáram-elemzés kell, hogy melyik szerializálási formátum (JSON, CSV vagy Avro) használata esetén a bejövő adatfolyamok.</td>
</tr>
<tr>
<td>Kódolás</td>
<td>Az CSV- és JSON UTF-8 jelenleg az egyetlen támogatott kódolási formátumot.</td>
</tr>
<tr>
<td>Elválasztó karakterrel</td>
<td>Értékáram-elemzés közös határolójel számos CSV-formátumban szerializálási adatok használatát támogatja. Támogatott értékek vesszővel, pontosvesszővel, hely, lapon, és függőleges vonás:.</td>
</tr>
</tbody>
</table>

Ha az adatok Blob-tároló forrásból származó elérhető lesz, a megjelenítő Értékáram-elemzés lekérdezés érheti el a néhány metaadatok mezőit. Az alábbi táblázat felsorolja a mezők és azok leírását.

| A TULAJDONSÁG | LEÍRÁS |
|------|------|
| BlobName | A beviteli blob a kapott az esemény neve. |
| EventProcessedUtcTime | Dátum és idő, amely az esemény szerint megjelenítő Értékáram-elemzés feldolgozása. |
| BlobLastModifiedUtcTime | Dátum és idő, amely a blob utolsó módosítás dátuma. |
| PartitionId | A beviteli kártya nulla alapú partíciót azonosítója. |

Előfordulhat, hogy például írhat lekérdezés például a következőket:

````
SELECT
    BlobName,
    EventProcessedUtcTime,
    BlobLastModifiedUtcTime
FROM Input
````


## <a name="get-help"></a>Segítség kérése
További segítségért próbálja meg az [Azure-adatfolyam Analytics-fórum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Következő lépések
Már megtanulta kapcsolatos adatok csatlakozási beállítások Azure-ban a Értékáram-elemzés feladatokhoz. Értékáram-elemzés kapcsolatos további tudnivalókért lásd:

- [Értékáram-elemzés Azure használatának első lépései](stream-analytics-get-started.md)
- [Azure Értékáram-elemzés feladatok méretezése](stream-analytics-scale-jobs.md)
- [Azure adatfolyam Analytics lekérdezés nyelvi hivatkozása](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure adatfolyam Analytics kezelése REST API-útmutató](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
