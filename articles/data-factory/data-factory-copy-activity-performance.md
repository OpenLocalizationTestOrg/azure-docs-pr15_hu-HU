<properties
    pageTitle="Másolja a tevékenység teljesítmény és a beállítási útmutatója |} Microsoft Azure"
    description="Tudjon meg többet a Másolás tevékenység használatakor a az adatok mozgás az Azure Data Factory teljesítményt befolyásoló főbb tényezők."
    services="data-factory"
    documentationCenter=""
    authors="linda33wj"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="jingwang"/>


# <a name="copy-activity-performance-and-tuning-guide"></a>Másolja a tevékenység teljesítményét és beállítási útmutatója
Azure adatok gyári másolás tevékenység megoldás betöltése osztályú biztonságos, megbízható és nagy teljesítményű adatok biztosítja. Lehetővé teszi az adatok terabájt tízesre példányát mindennap felhő gazdag különböző és a helyszíni adatok tárolja. Blazing gyors teljesítmény betöltése adatai annak érdekében, hogy a core "nagy-adatok" probléma koncentrálhat kulcs: fejlett analitikai megoldások fejlesztésére és ahhoz, hogy, hogy az adatok mélyebb az összefüggéseket.

Azure biztosít egy vállalati szintű adatok tárolására és adatok raktári megoldások, és a Másolás tevékenység kínál erősen optimalizált adatok betöltése eszköz, amely egyszerűen konfigurálása és állíthatja be. Csak egyetlen példányt tevékenységgel érhet el:

- Adatok betöltése az **Azure SQL-adatraktár** **1.2-es GB/s**
- Adatok betöltése az **1,0 GB/s** **Azure Blob-tárolóhoz**
- Adatok betöltése **Azure tó adattár** a **1,0 GB/s**


Ez a cikk ismerteti:

- [Teljesítmény számokat](#performance-reference) a támogatott adatforrások és gyűjtő adatokat tárolja segít; projektek tervezése
- Funkciók, amelyek a különböző forgatókönyvekben, többek között a [párhuzamos másolás](#parallel-copy), a [felhőben adatok mozgását mennyiség](#cloud-data-movement-units)és a [Másolás előkészített](#staged-copy); másolás átviteli is növelése
- Hogyan javítható teljesítményének és a főbb tényezők, amely hatással lehet a teljesítmény Másolás [Teljesítmény eszközbeállítási útmutatást](#performance-tuning-steps) .

> [AZURE.NOTE] Ha nem ismeri a Másolás tevékenység általános, előtt olvassa el [az adatok másolása tevékenység használatával áthelyezése](data-factory-data-movement-activities.md) a cikket elolvasva.

## <a name="performance-reference"></a>Hivatkozás a teljesítmény

![Teljesítmény mátrix](./media/data-factory-copy-activity-performance/CopyPerfRef.png)

> [AZURE.NOTE] Nagyobb teljesítmény érhet el további adatok mozgását egységek (DMUs) az alapértelmezett vizsgál maximális DMUs, amely 8 futtatása felhő a felhőbe másolás tevékenységhez. Például a 100 DMUs, másolhatja adatok az Azure Blob Azure adatok tó áruház másodpercenként 1 GB-os. Részletes tudnivalók erről a szolgáltatásról a [felhőben adatok mozgását egységek](#cloud-data-movement-units) szakaszban olvashat. Lépjen kapcsolatba a [támogatási Azure](https://azure.microsoft.com/support/) kérhet további DMUs.

Megjegyzés: a címzett pontok:

- Átviteli kiszámítása a következő képlet használatával: [forrásból származó adatok mérete olvasható] / [másolás tevékenység időtartamának futtatása].
- A teljesítmény hivatkozás a táblázatban szereplő számok voltak mérni [TPC-H](http://www.tpc.org/tpch/) adatkészlet futtatása egyetlen példányt tevékenységet.
- Másolás között felhő adatokat tárolja, állítsa összehasonlításra **cloudDataMovementUnits** 1 és 4-es (vagy 8). **parallelCopies** nincs megadva. Ezek a funkciók részleteket a [párhuzamos példányt](#parallel-copy) szakaszban olvashat.
- Azure adatokat tárolja a forrás- és gyűjtő is ugyanabban a Azure régióban.
- Hibrid (helyszíni felhő vagy a helyszíni felhő) adatok mozgását, átjáró egy példányát, amely a helyszíni adatok áruházból külön volt a számítógépen futó volt. A konfigurációt a következő táblázatban szerepel. Egy egyetlen tevékenységet a átjáró lett fut, a másolás csak egy kisebb részét a próba-gép Processzor, memória vagy hálózati sávszélesség elfogyasztott mennyiség megjelenítésére.
    <table>
    <tr>
        <td>PROCESSZOR</td>
        <td>32 magmintákat 2,20 GHz -es Intel Xeon E5-2660 v2</td>
    </tr>
    <tr>
        <td>A memória</td>
        <td>128 GB</td>
    </tr>
    <tr>
        <td>Hálózati</td>
        <td>Internetes kapcsolat: 10 GB/s; intranet interface: 40 GB/s</td>
    </tr>
    </table>

## <a name="parallel-copy"></a>A párhuzamos másolása
A forrásból származó adatokat olvasni, vagy adatok írja be a cél **egy másolatot a tevékenységet futtatása párhuzamosan**. Ez a funkció teszi fejlettebbé a Másolás a kapacitásának, és csökkenti a ideig tart, ha az adatokat.

Ez a beállítás eltér a tevékenység-definícióban az **feldolgozási** tulajdonságot. A **párhuzamos** tulajdonság meghatározza, hogy hány **egyidejű másolás tevékenység fut** másik tevékenységeket windows (1 óra szeretne 2 AM, 2: AM 3: AM, de 3-4-es AM és stb.) adatainak feldolgozása. Ez a lehetőség akkor hasznos, ha a korábbi terhelést elkészíti. A párhuzamos Másolás lehetőséget a **Futtatás egyetlen tevékenységet**érinti.

Tekintsük át a minta példa. A következő példában a múltbeli több szeletek kell dolgozható fel. Adatok gyári másolás tevékenység (tevékenység Futtatás) az egyes szeletek egy példánya fut:

- Az adatok szelet az első tevékenység ablakból (1 óra szeretne 2 AM) == > tevékenység 1 futtatása
- A második tevékenység ablak (2, de a 3-as AM) az adatok Szelet == > tevékenység 2 futtatása
- A második tevékenység ablakból (3 de a 4-es AM) a szeletre == > tevékenység 3 futtatása

És így tovább.

Ebben a példában a **feldolgozási** értéke 2, amikor **tevékenység 1** és **tevékenység végre 2** adatok másolása két tevékenység a windows **egyidejűleg** adatok mozgás a teljesítmény javítása érdekében. Azonban több fájl futtatása 1 tevékenységeket használó vannak rendelve, az adatok mozgását szolgáltatás fájlokat másolja a forrásból származó a célfájl egy egyszerre.

### <a name="parallelcopies"></a>parallelCopies
A **parallelCopies** tulajdonsággal a párhuzamos másolás tevékenység használni kívánt jelzik. Érdemes elképzelnie ezt a tulajdonságot másolása a tevékenységet, amely a forrásból származó olvashatják vagy a gyűjtő adatokat tárolja párhuzamosan írási szálak maximális számát.

Futtatása másolás tevékenységeknél Data Factory határozza meg, hogy a példányszámot a párhuzamos adatok tárolására és a cél adatokat tárolja a forrásból származó adatok másolása használatával. Alapértelmezett példányszámot párhuzamos használt forrás- és a használni kívánt gyűjtő típusától függ.  

Forrás- és gyűjtő |   Alapértelmezett párhuzamos példányszám szolgáltatás által meghatározott
------------- | -------------------------------------------------
Adatok másolása (Blob-tárolóhoz; üzletek fájlalapú között Adatok tó Store; Amazon S3; a helyszíni fájlrendszerben; egy helyszíni) Fájlrendszerhez | 1 és között 32. Adatok között két felhő adatokat tárolja, vagy a fizikai konfigurációja miatt az átjárót futtató számítógépen használt hibrid másolatának (másolja az adatokat, vagy a helyszíni adatok áruházból) másolásához használja a fájlokat és a felhő adatok mozgását mennyiségek (DMUs) méretének függ.
Adatok másolása **bármely forrásadatok tárolása Azure táblatárolóhoz** | 4
Más adatforrás és gyűjtő pár | 1

Általában az alapértelmezett működés meg kell adnia a legjobb teljesítmény. Azonban kattintva állítsa be a betöltés az adatokat szolgáltató gépeken tárolja, vagy a Másolás teljesítményének finomhangolása, akkor előfordulhat, hogy válassza bírálja felül az alapértelmezett érték, és adja meg a **parallelCopies** tulajdonság értékét. Az érték 1 és 32 (mindkettő végpontokat is beleértve) közé kell lennie. Futásidőben a legjobb teljesítmény elérése érdekében másolás tevékenységet használ, amelynek az értéke kisebb vagy egyenlő beállított értékére.

    "activities":[  
        {
            "name": "Sample copy activity",
            "description": "",
            "type": "Copy",
            "inputs": [{ "name": "InputDataset" }],
            "outputs": [{ "name": "OutputDataset" }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource",
                },
                "sink": {
                    "type": "AzureDataLakeStoreSink"
                },
                "parallelCopies": 8
            }
        }
    ]

Megjegyzés: a címzett pontok:

- Fájlalapú üzletek adatainak másolásakor a párhuzamos történik a fájlok szintjén. Nem nincs chunking belül egy fájlt. A tényleges példányszámot párhuzamos az adatok mozgását szolgáltatás használja a Másolás futásidőben nincs több, mint a fájlok van számát. Ha másolatot viselkedését **mergeFile**, Másolás tevékenység nem kihasználhatja párhuzamos.
- Miután megadta a **parallelCopies** tulajdonság értékét, fontolja meg a betöltés növekedés a forrás- és gyűjtő adatokat tárolja, és az átjáró hibrid másolatának esetén. Ez történik, különösen ha van több tevékenységeket vagy tevékenységének futtassa a azonos adatokat tároló egyidejű lefut. Ha azt észleli, hogy az adatokat tároló vagy az átjáró túl-e sok, és a betöltés, csökkentése a betöltés enyhítése **parallelCopies** értéket.
- Adatok másolása, amelyek nem szeretne tárolja, amelyek a fájlalapú fájlalapú tárolja, ha az adatok mozgását szolgáltatás figyelmen kívül hagyja a **parallelCopies** tulajdonság. Akkor is, ha a párhuzamos van megadva, akkor a program nem állítja ebben az esetben.

> [AZURE.NOTE] Az adatkezelési átjáró 1.11 vagy újabb verzióját kell használnia a **parallelCopies** funkcióval, amikor egy hibrid lemásolni.

### <a name="cloud-data-movement-units"></a>Felhőalapú adatok mozgását mennyiség
**Felhőalapú adatok mozgását egység (DMU)** azt méri, amely a power (kombinációi Processzor, a memória és a hálózati erőforrás-hozzárendelés) az adatok gyári egyetlen egység. Egy DMU megszokhatta a felhőbe a felhőbe fájlmásolás, de nem hibrid másolatot.

Alapértelmezés szerint a Data Factory egyetlen felhő DMU használja egy futtatása egyetlen példányt tevékenység elvégzéséhez. Bírálja felül az alapértelmezett értéket, adja meg az alábbi képlettel történik a **cloudDataMovementUnits** tulajdonság értékét. Tudni a teljesítmény erősítési szintjét jelenhet meg beállíthatja, hogy egy adott másolás forrás- és gyűjtő további egységek lásd: a [Teljesítmény hivatkozást](#performance-reference).

    "activities":[  
        {
            "name": "Sample copy activity",
            "description": "",
            "type": "Copy",
            "inputs": [{ "name": "InputDataset" }],
            "outputs": [{ "name": "OutputDataset" }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource",
                },
                "sink": {
                    "type": "AzureDataLakeStoreSink"
                },
                "cloudDataMovementUnits": 4
            }
        }
    ]

A **megengedett értékeket** **cloudDataMovementUnits** tulajdonság is (alapértelmezett) 1, 2, 4-es és 8. A **tényleges száma felhő DMUs** futásidőben használja a másolás, a beállított érték, attól függően, hogy az adatok mintát kisebb vagy egyenlő. 

> [AZURE.NOTE] Ha több DMUs egy újabb átviteli az a felhő, forduljon a [támogatási Azure](https://azure.microsoft.com/support/)van szüksége. Csak akkor, ha a másolt több fájl Blob-tárolóhoz Blob-tárolóhoz tó adattár vagy Azure SQL-adatbázissal, és a fájl mérete nem nagyobb, mint 16 MB egyenként 8 és a fenti jelenleg works beállításához.

Jobban használja az alábbi két tulajdonságait, és a mozgás fájlmegosztásra erősítése, olvassa el a a [minta használjon esetekben](#case-study-use-parallel-copy)című témakört. Nem kell **parallelCopies** kihasználhatja az alapértelmezett működés konfigurálása. Ha letiltja és **parallelCopies** túl kicsi, több felhő DMUs előfordulhat, hogy nem kell teljesen használni.  

A Másolás teljes időtartama annak **fontos** tudni fogja, hogy-előfizetést terhelő alapján. Ha a projekt másolása magával szeretné vinni egy órával egy felhőalapú egység használt, és most négy felhő egységekkel 15 percet vesz igénybe, a teljes számla szinte változatlan marad. Például a négy felhő egységek használhatja. Az első felhő egység tölt 10 perc alatt, a másodikat pedig 10 perc, a harmadik, 5 percig tart, és a negyedik befogadó, 5 percig tart, mind az egyik példány tevékenység futtatása. A teljes másolása (adatok mozgás) idejét, amely a 10 + 10 + 5 + 5 = 30 perc az előfizetést terhelő. **ParallelCopies** használ, a számlázási nem befolyásolja.

## <a name="staged-copy"></a>Szakaszos másolása
Ha egy adatforrás adattár adatok másolása a egy gyűjtő adattár, előfordulhat, hogy kíván használni Blob-tárolóhoz egy ideiglenes átmeneti tárolásra szolgáló tároló. Átmeneti különösen akkor hasznos, az alábbi esetekben:

1.  **Különféle adatok tárolja az SQL-adatraktár keresztül PolyBase adatainak ingest szeretne**. SQL-adatraktár PolyBase egy nagy átviteli mechanizmusként nagy mennyiségű adat betöltése SQL adatraktár használja. Azonban a forrásadatok Blob-tárolóhoz kell lennie, és annak meg kell felelnie a további feltételeket. Adatok egy adatok áruházból eltérő Blob-tárolóhoz betöltéssel, adatok másolása köztes átmeneti tárolásra szolgáló Blob-tárolóhoz keresztül is aktiválhatja. Ebben az esetben Data Factory hajtja végre a szükséges adatok átalakításokat győződjön meg arról, hogy megfelel-e PolyBase követelményeinek. Kattintson az PolyBase adatok betöltése SQL adatraktár segítségével. További információra kíváncsi olvassa el a [Használata PolyBase adatok az Azure SQL-adatraktár betöltése](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse)című témakört.
2.  **Előfordul, hogy végezze el a hibrid adat mozgását igénybe vesz igénybe (Ez azt jelenti, hogy egy helyszíni adatok között másolása áruház és a felhő adatok tárolására) lassú hálózati kapcsolaton keresztül**. A teljesítmény javítása érdekében, csökkentheti az adatok helyszíni, hogy az adatok áthelyezése a felhőben az átmeneti tárolásra szolgáló adatok Store kevesebb időt vesz igénybe. Kattintson az adatokat az átmeneti tárolásra szolgáló áruházban kibontásához, mielőtt a céltár adatok betöltése.
3.  A **nem kívánt nyissa meg a portokon kívül 80 és 443-as, a tűzfal vállalati informatikai házirendek miatt portot**. Ha például másolásakor adatok a helyszíni adatok áruházból egy SQL Azure-adatbázis gyűjtő vagy egy Azure SQL-adatraktár gyűjtő, aktiválnia kell kimenő TCP kommunikáció port 1433 a Windows tűzfal és a vállalati tűzfal. Ebben az esetben kihasználhatja az adatok másolása első átjárót egy Blob-tároló átmeneti tárolásra szolgáló példányhoz HTTP vagy HTTPS a 443-as port. Ezután betöltése az adatokat egy SQL-adatbázis vagy SQL adatraktár Blob-tároló átmeneti. Ez a folyamat nem kell port 1433 engedélyezése.

### <a name="how-staged-copy-works"></a>Hogyan szakaszos másolás működése
Az átmeneti tárolásra szolgáló szolgáltatás aktiválása után, először az adatokat másolja a forrás adatainak áruházból az átmeneti tárolásra szolgáló adatok Store (hozása saját). Ezután az adatokat másolja az átmeneti tárolásra szolgáló adatok áruházból a gyűjtő adatokat tároló. Adatok gyári automatikusan kezeli a két lépésből álló folyamat. Adatok gyári is törli a köteggel átmeneti tárolására ideiglenes adatainak adatok mozgását befejeződése után.

Átjáró a felhőbeli másolás esetben (forrás- és gyűjtő adatokat tárolja a felhőben vannak) nem használható. Az adatok gyári szolgáltatás másolás műveleteket hajtja végre.

![Másolás előkészített: Cloud eset](media/data-factory-copy-activity-performance/staged-copy-cloud-scenario.png)

A hibrid másolás helyzetben (forrás helyszíni pedig gyűjtő a felhőben), az átjáró adatokat helyezi át a forrás adatainak áruházból egy átmeneti tárolásra szolgáló adatok áruházból. Adatok gyári szolgáltatás adatokat mozgatja a gyűjtő adatokat tároló az átmeneti tárolásra szolgáló adatok áruházból. Az adatok másolása egy felhőalapú adatokat tároló helyszíni adatok tárolóhoz átmeneti keresztül is támogatott, a fordított folyamat.

![Másolás előkészített: hibrid eset](media/data-factory-copy-activity-performance/staged-copy-hybrid-scenario.png)

Adatok mozgás egy átmeneti tárolásra szolgáló tároló használatával aktiválása után, megadhatja, hogy szeretné-e az adatokat kell előtt adatok áthelyezése a forrás adatainak áruházból ideiglenes vagy átmeneti tárolásra szolgáló adattár tömörített, és kattintson a adatok áthelyezése a ideiglenes vagy a gyűjtő adatok áruház adattár átmeneti előtt kibontandó.

Jelenleg nem másolhat adatok között két a helyszíni adatok üzlet átmeneti tárolásra szolgáló üzlet használatával. Megakadályozási ezt a beállítást, amint elérhetővé tenni.

### <a name="configuration"></a>Konfiguráció
A **enableStaging** beállításáról másolás tevékenység adja meg, hogy az adatokat kell helyezni a tárban Blob-tárolóban lévő adatok céltárat betöltése előtt. Ha **enableStaging** igaz értékre állítja, a következő táblázatban található további tulajdonságok megadása Ha nincs telepítve egyik, az Azure tároló létrehozásához szükséges, vagy tároló megosztott access aláírás csatolt szolgáltatás átmeneti.

A tulajdonság | Leírás | Alapérték | Szükséges
--------- | ----------- | ------------ | --------
**enableStaging** | Adja meg, hogy szeretné-e adatokat tároló átmeneti ideiglenes keresztül. | Hamis | nem
**linkedServiceName** | Adja meg, hogy egy [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service) vagy [AzureStorageSas](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service) csatolt szolgáltatása, amelynek tároló példányának hivatkozik, amikor használni egy ideiglenes átmeneti tárolásra szolgáló tár nevére. <br/><br/> Adatok betöltése SQL adatraktár PolyBase keresztül a tárhely átengedése aláírással rendelkező nem használható. Minden más esetben használható. | A #HIÁNYZIK | Igen, ha **enableStaging** értéke igaz
**elérési út** | Adja meg a szakaszos adatokat tartalmazza, amelyet Blob tároló elérési. Ha nem ad meg egy elérési utat, a szolgáltatás a ideiglenes adatok tárolására tároló hoz létre. <br/><br/> Adjon meg egy elérési utat, csak akkor, ha tárterületet használ a átengedése aláírás, vagy a ideiglenes az adatokat egy adott helyen van szüksége. | A #HIÁNYZIK | nem
**enableCompression** | Itt adhatja meg, hogy adatokat tömöríti-e, mielőtt másolása a megadott helyre. Ez a beállítás csökkenti az átvitt adatok mennyisége. | Hamis | nem

Az alábbiakban a fenti táblázatban ismertetett Tulajdonságok másolása tevékenység minta meghatározása:

    "activities":[  
    {
        "name": "Sample copy activity",
        "type": "Copy",
        "inputs": [{ "name": "OnpremisesSQLServerInput" }],
        "outputs": [{ "name": "AzureSQLDBOutput" }],
        "typeProperties": {
            "source": {
                "type": "SqlSource",
            },
            "sink": {
                "type": "SqlSink"
            },
            "enableStaging": true,
            "stagingSettings": {
                "linkedServiceName": "MyStagingBlob",
                "path": "stagingcontainer/path",
                "enableCompression": true
            }
        }
    }
    ]


### <a name="billing-impact"></a>Számlázási hatás
-Előfizetést terhelő alapján két lépésből áll: időtartam másolása, és másolja a típus. 

- Használatakor előkészítése során (adatok másolása egy felhőalapú adatok áruházból egy másik felhő adattár) felhő másolatának-előfizetést terhelő [összegét lépés: 1 és lépés: 2 másolás időtartam] [felhő másolás Egységár] x.
- Ha előkészítése során hibrid másolatának (adatok másolása a helyszíni adatok áruházból egy felhőalapú adattár), az előfizetést terhelő [hibrid másolás időtartam] x [hibrid másolás Egységár] + [cloud másolás időtartam] [felhő másolás Egységár] x.


## <a name="performance-tuning-steps"></a>Teljesítmény beállítási lépések
Javasoljuk, hogy az Ön által ezeket a lépéseket az adatok gyári szolgáltatás másolás tevékenységeket használó teljesítményének finomhangolása:

1.  **Alapterv létrehozása**. A fejlesztői fázisban tesztelje a folyamat másolás tevékenység ellen képviselő adatok minta használatával. A [modell szeletelés](data-factory-scheduling-and-execution.md#time-series-datasets-and-data-slices) Data Factory segítségével korlátozza az adattal dolgozik.

    Végrehajtási idő és a teljesítmény jellemzők gyűjt a **Figyelés és felügyeleti alkalmazás**használatával. A Data Factory kezdőlapján válassza a **Monitor és kezelése** . A fastruktúrájú nézetben válassza ki a **Kimenet adatkészlet**. A **Tevékenység Windows** listában válassza a Másolás tevékenység futtatni. **Tevékenység Windows** sorolja fel, a Másolás tevékenység időtartama és a másolt adatok méretét. A teljesítmény **Tevékenység ablak Explorer**szerepel. Ha többet szeretne megtudni az alkalmazásról, lásd: [Monitor és Azure Data Factory folyamatok kezelése a figyelő és felügyeleti alkalmazás](data-factory-monitor-manage-app.md).

    ![A tevékenység részleteit futtatása](./media/data-factory-copy-activity-performance/mmapp-activity-run-details.png)

    Az cikk későbbi részében összehasonlíthatja a teljesítmény és konfigurálása az igényektől [Teljesítmény hivatkozás](#performance-reference) másolása tevékenység a teszteket a.
2. **Diagnosztizálása és a teljesítmény optimalizálása**. Ha a teljesítmény erőforrásigények nem felel meg a várakozásoknak, Önnek kell azonosítania a teljesítmény szűk. Ezután optimalizálása teljesítmény távolítsa el vagy csökkentése szűk hatását. Teljesítmény megállapítása teljes körű leírását a jelen cikk túlra, de az alábbiakban néhány gyakori megfontolások:
    - Teljesítménnyel kapcsolatos szolgáltatások:
        - [A párhuzamos másolása](#parallel-copy)
        - [Felhőalapú adatok mozgását mennyiség](#cloud-data-movement-units)
        - [Szakaszos másolása](#staged-copy)   
    - [Forrás](#considerations-for-the-source)
    - [Gyűjtése](#considerations-for-the-sink)
    - [Szerializálási és deszerializálás](#considerations-for-serialization-and-deserialization)
    - [Tömörítés](#considerations-for-compression)
    - [Oszlopok megfeleltetése](#considerations-for-column-mapping)
    - [Az adatkezelési átjáró](#considerations-for-data-management-gateway)
    - [Egyéb szempontok](#other-considerations)

3. **Kibontás a beállításait, a teljes adatkészletet**. Ha elégedett a végrehajtás eredmények és a teljesítmény, kibonthatja a definition és a folyamat aktív időszak, hogy pontosan illeszkedjen a teljes adatkészletet.

## <a name="considerations-for-the-source"></a>A forrás megfontolandó szempontok
### <a name="general"></a>Általános
Győződjön meg arról, hogy a mögöttes adatok tároló nem túl sok más be- és szemben futó feladatok szerint. 

Microsoft adatokat tárolja olvassa el a [figyelése és témakörök beállítása](#performance-reference) jellemző adatokat tárolja, és a Súgó adatok tárolására teljesítményt nyújt, válaszidő kisméretűvé alakítása és átviteli maximalizálása tisztában című témakört.

Ha Blob-tárolóhoz adatok másolása a SQL adatraktár, fontolja meg **PolyBase** teljesítmény. [Adatok az Azure SQL-adatraktár betöltése használata PolyBase](data-factory-azure-sql-data-warehouse-connector.md###use-polybase-to-load-data-into-azure-sql-data-warehouse) talál részleteket.


### <a name="file-based-data-stores"></a>Fájlalapú adatokat tárolja.
*(Blob tárhely, tó adattár, Amazon S3, a helyszíni fájlrendszer és a helyszíni Fájlrendszerhez tartalmazza)*

- **Átlagos méretét és fájlszám**: másolás tevékenység továbbítja egy adatfájl egyszerre. Az adatok áthelyezésére ugyanannyi a teljes kapacitásának az alsó, ha az adatok áll a sok kisméretű fájlt, hanem a minden fájl betöltő fázis miatt néhány nagyméretű fájlok. Ezért ha lehetséges, egyesítése kisméretű fájlok nagyobb méretű fájlok nagyobb teljesítmény eléréséhez.
- **Fájlformátum és tömörítés**: további lehetőségek a teljesítmény javítása érdekében, [szerializálási és deszerializálás kérdései](#considerations-for-serialization-and-deserialization) és [tömörítés szempontjai](#considerations-for-compression) foglalása című szakaszban találhatók.
- A **helyszíni fájlrendszer** eset, amelyben **Az adatkezelési átjáró** szükség, [az adatkezelési átjáró kapcsolatos szempontok](#considerations-for-data-management-gateway) című.

### <a name="relational-data-stores"></a>Relációs adatok tárolja.
*(Ideértendők a; SQL-adatbázis SQL-adatraktár; Amazon Redshift; SQL Server-adatbázisok; és az Oracle, MySQL, DB2, Teradata, Sybase és PostgreSQL-adatbázis stb.)*

- **Adatok minta**: A táblázat séma hatása a Másolás kapacitása. A nagy sor méretet lehetővé teszi a jobb teljesítmény kis sor méreténél, ugyanannyi adatok másolásához. A hiba oka, hogy az adatbázis hatékonyabban meghallgathatja kevesebb kötegenként kevesebb sort tartalmazó adat.
- **Lekérdezés vagy a tárolt eljárás**: optimalizálása a logika a lekérdezés vagy a tárolt eljárás a Másolás tevékenység forrás-ból az adatok hatékonyabb ad meg.
- **A helyszíni relációs adatbázisok**, például SQL Server vagy Oracle-alapú, amely kell az **Adatkezelési átjáró**használni, az [adatkezelési átjáró előtt megfontolandó kérdések](#considerations-on-data-management-gateway) szakaszban.

## <a name="considerations-for-the-sink"></a>A gyűjtő megfontolandó szempontok

### <a name="general"></a>Általános
Győződjön meg arról, hogy a mögöttes adatok tároló nem túl sok más be- és szemben futó feladatok szerint. 

A Microsoft adatokat tárolja tekintse át a [figyelése és témakörök beállítása](#performance-reference) jellemző adatokat tárolja. Az alábbi témakörök segítséget nyújtanak az megértéséhez adatokat tároló teljesítményt nyújt, és hogyan válaszidő kisméretűvé alakítása és maximalizálja az átviteli.

Ha a másolandó adatok **Blob-tárolóhoz** az **SQL adatraktár**, fontolja meg **PolyBase** teljesítmény. [Adatok az Azure SQL-adatraktár betöltése használata PolyBase](data-factory-azure-sql-data-warehouse-connector.md###use-polybase-to-load-data-into-azure-sql-data-warehouse) talál részleteket.


### <a name="file-based-data-stores"></a>Fájlalapú adatokat tárolja.
*(Blob tárhely, tó adattár, Amazon S3, a helyszíni fájlrendszer és a helyszíni Fájlrendszerhez tartalmazza)*

- **Másolás viselkedését**: adatok másolása egy másik fájlalapú adattár, ha másolás tevékenység van-e a **copyBehavior** tulajdonság keresztül három lehetőség közül választhat. Hierarchia megőrzi, hierarchia összeolvasztja, illetve fájlok egyesíti. Megőrzése mellett vagy a hierarchia összeolvasztása vannak vagy kis teljesítmény terhelést, de a fájlok egyesítése okoz a teljesítmény terhelést növeléséhez.
- **Fájlformátum és tömörítés**: lásd: további lehetőségek a teljesítmény javítása érdekében [szerializálási és deszerializálás kérdései](#considerations-for-serialization-and-deserialization) és [tömörítés szempontjai](#considerations-for-compression) szakaszokban.
- **BLOB-tárolóhoz**: jelenleg Blob-tároló támogatja csak blokkolja BLOB optimalizált adatátvitel és kapacitása.
- **A helyszíni fájlrendszer** esetben **az adatkezelési**átjáró használatát igénylő című [szempontjait az adatkezelési átjáró](#considerations-for-data-management-gateway) .

### <a name="relational-data-stores"></a>Relációs adatok tárolja.
*(SQL-adatbázissal, SQL adatraktár, SQL Server-adatbázisok és az Oracle-adatbázisok befoglalása)*

- **Másolja a jelenség**: attól függően, hogy a beállított **sqlSink**tulajdonságait, Másolás tevékenység adatot ír a céladatbázist különböző módokon.
    - Alapértelmezés szerint a adatok mozgását szolgáltatás által használt a tömeges másolás API szeretne beszúrni az adatok hozzáfűzése mód, amely magában foglalja a legjobb teljesítmény elérése érdekében.
    - Ha a gyűjtő az adja meg a tárolt eljárás, az adatbázis az egyik adatsorban tömeges terhelést, hanem egyszerre vonatkozik. Teljesítmény jelentősen csökken. Ha az adatkészletet nagy, ha szükséges, fontolja meg, hogy a **sqlWriterCleanupScript** tulajdonság segítségével.
    - Ha úgy állítja be a **sqlWriterCleanupScript** tulajdonság futtatása tevékenységeknél másolás, a szolgáltatás elindítja a parancsfájlt, és majd illessze be az adatokat a tömeges másolás API-t használja. Például a teljes táblázatot felülírja a legújabb adatokkal, megadhatja az új adatokat a forrásból tömeges – betöltés előtt először törölje az összes rekordot parancsfájl.
- **Adatok minta és a Köteg mérete**:
    - A táblázat séma másolás átviteli hatással van. Adatok ugyanannyi másolásához egy sor nagy méretű lépve egy kis sor méretnél nagyobb teljesítmény, mert az adatbázis hatékonyabban is véglegesítése kevesebb kötegenként adatokat.
    - Másolás tevékenység adatokat tételek sorozat szúrja be. Beállíthatja, hogy a sorok száma egy köteg **writeBatchSize** tulajdonságával. Ha adatainak van a kis sorok, beállíthatja, hogy a **writeBatchSize** tulajdonság alsó köteg terhelést és magasabb átviteli összekapcsolhatók nagyobb értékű. Ha az adatok sor mérete túl nagy, ügyeljen arra, hogy **writeBatchSize**növelésével. A magas érték okozta az adatbázis túlterhelés másolás hiba vezethet.
- **A helyszíni relációs adatbázisok** , például SQL Server és a Oracle, **az adatkezelési**átjáró használatát igénylő [szempontjait az adatkezelési átjáró](#considerations-for-data-management-gateway) című.


### <a name="nosql-stores"></a>NoSQL tárolja.
*(Ideértendők a táblatároló és Azure DocumentDB)*

- A **tábla tárolása**:
    - **Partition**: adatok írása időosztásos partíciók jelentősen csökkenti a teljesítményét. A forrásadatok rendezéshez partíciót billentyűt, hogy az adatok beilleszti hatékony egy partíciót egymás után, vagy módosítsa az adatok írásához egyetlen partíciók logika.
- A **DocumentDB**:
    - **Köteg méret**: A **writeBatchSize** tulajdonság állítja be a párhuzamos kérések száma a DocumentDB szolgáltatás a dokumentumok létrehozására. Számíthat jobb teljesítményt **writeBatchSize** növelése, mert az több párhuzamos összehívásokat, DocumentDB. Jó helyen jár, nézze meg írásakor DocumentDB szabályozásának (a "Mérték nagy kérése" hibaüzenet). Különböző tényezők okozhatják szabályozása dokumentum mérete, beleértve az dokumentumokat és a cél gyűjteményben az indexelő házirend számát. Újabb példány átviteli elérése érdekében fontolja meg jobban gyűjteménye, például S3.

## <a name="considerations-for-serialization-and-deserialization"></a>A szerializálási és deszerializálás kapcsolatos szempontok
Szerializálási és deszerializálás akkor fordulhat elő, ha a bemeneti adatok beállítása vagy a kimeneti adatkészlet egy fájlt. Jelenleg, a Másolás tevékenység támogatja Avro és a szöveget (például CSV és TSV) adatformátumok.

A **viselkedés másolja**.

-   Fájlok másolása fájlalapú adatokat tárolja között:
    - Amikor bemeneti és kimeneti adatok mindkét halmazhoz azonos vagy nincs fájlbeállításai formátumot, az adatok mozgását szolgáltatás végrehajtja a bináris másolatának szerializálási és deszerializálás nélkül. Megjelenik egy újabb átviteli és összehasonlítása az alkalmazási példát, amelyben a forrás- és gyűjtő formátum beállításai különböznek egymástól.
    - Amikor bemeneti és kimeneti adatkészletek mindkét szöveges formátumban, és csak a kódolást különböző típusú, az adatok mozgását szolgáltatás csak jelent kódolási átalakítás. Azt nem minden szerializálási és deszerializálás néhány teljesítményt bináris másolatának felsőbb képest.
    - Forrásadatok adatfolyam, átalakítás, és majd szerializálni azt be a kimeneti formátum jelzi, amikor a bemeneti és kimeneti adatkészletek mindkét van más fájlformátumokban vagy más beállításokat, például a határoló, az adatok mozgását szolgáltatás deserializes. Ez a művelet felsőbb és összehasonlítása más esetben sokkal több teljesítménybeli eredményez.
- Egy adattár, amely nem fájlalapú (például a tárolóból fájlalapú relációs tárolóval) / származó fájlok másolásakor a szerializálási vagy deszerializálás lépés szükség. Ebben a lépésben általános teljesítménybeli eredményez.

**Fájlformátum**: A választott fájlformátum hatással lehetnek a Másolás teljesítményre. Például a Avro tárolja a metaadatokat az adatok kompakt binárissá is. A Hadoop ökológiai feldolgozás és lekérdezése széles körű támogatása rendelkezik. Avro azonban több drága szerializálási és deszerializálás összehasonlítva a szöveg formázása alsó másolás átviteli eredményez. Adja meg a példányszámot, a feldolgozás folyamat során fájlformátum holistically. Először milyen formában az adatokat tárolja az adatforrás adatait tárolja, vagy olvassa be a külső rendszerek; listát tároló, analitikus feldolgozási és lekérdezése; és milyen formában az adatok exportálja az adatokat marts, a jelentéskészítő és a képi megjelenítések eszközök. Előfordul, hogy olvassa el a optimálisnál fájlformátumban, és írási teljesítmény lehet hasznos, amikor a akkor érdemes megfontolni, hogy a teljes elemzési folyamatot.

## <a name="considerations-for-compression"></a>A tömörítési megfontolandó szempontok
Ha a bemeneti és kimeneti adatkészlet egy fájlt, beállíthatja, hogy másolás tevékenység tömörítés vagy kibontási végrehajtásához, a cél adatokat ír. Amikor úgy dönt, hogy a tömörítési, elérhetővé teszi-e egy útján között (I/O) bemeneti és Processzor. Tömöríti a számítási erőforrásokat a felesleges adatok költségeket. De cserébe csökkenti a hálózati I/O és tárolási. Attól függően, hogy az adatok a teljes másolás adatátvitel egy erősítése jelenhet meg.

**A kodek**: másolás tevékenység gzip bzip2 és Deflate tömörítési típusok támogatja. Azure hdinsight szolgáltatáshoz igénybe vehet, feldolgozásra három diagramtípusokat. Minden egyes tömörítés kodek előnyökkel jár. Például bzip2 a legalacsonyabb másolás átviteli van, de a legjobb struktúra lekérdezési teljesítményt bzip2 kap, mert feldolgozásra felosztása. Gzip a legkiegyensúlyozottabb lehetőséget, és a leggyakrabban használatos. Válassza ki a kodek, amely a leginkább illik a végpontok közötti forgatókönyv.

**Szint**: az egyes tömörítés kodek két lehetőség közül választhat: leggyorsabb tömörített és optimálisan tömörített. A leggyorsabb tömörített lehetőséget, ha az eredményül kapott fájl optimálisan nem tömöríti, minél gyorsabban tömöríti az adatokat. A optimálisan tömörített több időt tölt a tömörítést és együttesen minimális mennyiségű adat. Ellenőrizheti, hogy mindkét beállításai lehetőséget választva megtekintheti, amely biztosítja, hogy jobban általános teljesítményére abban az esetben.

**A figyelmet**: másolni szeretné a nagy mennyiségű adatot egy helyszíni tár és a felhő között, akkor fontolja meg köztes blob-tárolóhoz tömörítést. Köztes tároló használatával akkor hasznos, ha a sávszélesség a vállalati hálózathoz és a Azure szolgáltatások korlátozó tényező, és azt szeretné, hogy a bemeneti adatok megadása és a kimeneti adatkészlet is nem tömörített formában. Pontosabban egy egyetlen példányt tevékenységet is szétválasztani két másolás tevékenységeket. Az első példányt tevékenységhez másolja a forrásból származó egy ideiglenes vagy átmeneti tárolásra szolgáló blob tömörített formában. A második másolás tevékenység másolja át a tömörített adatok átmeneti, és közben a gyűjtő ír majd kibontja.

## <a name="considerations-for-column-mapping"></a>Oszlopok megfeleltetése megfontolandó szempontok
Beállíthatja, hogy a **columnMappings** tulajdonság másolása tevékenység térkép összes vagy egy részét a bemeneti oszlopot a kimeneti oszlopokra. Az adatok mozgását szolgáltatás beolvassa az adatokat a forrás, miután kell leképezése oszlop az adatokon az adatokat ír a gyűjtő előtt. A további feldolgozásra csökkenti a Másolás kapacitása.

A forrásadatok tárolja esetén queryable, például egy relációs tárol, például SQL-adatbázis vagy SQL Server vagy táblatároló vagy DocumentDB, például egy NoSQL tár esetén, fontolja meg az oszlop szűrése terjesztése, és átrendezése logika a **lekérdezés** tulajdonság Oszlopok megfeleltetése használata helyett. Ezzel a módszerrel a kivetítés fordul elő, miközben az adatok mozgását szolgáltatás adatokat olvas be a forrás adatainak áruházból, ahol még sokkal hatékonyabban.

## <a name="considerations-for-data-management-gateway"></a>Az adatkezelési átjáró megfontolandó szempontok
Az átjáró beállítása javaslatok olvassa el a [az adatkezelési átjáró használatával kapcsolatos szempontok](data-factory-move-data-between-onprem-and-cloud.md#Considerations-for-using-Data-Management-Gateway)című témakört.

**Átjáró-számítógép környezet**: azt javasoljuk, hogy használja-e az adatkezelési átjáró állomáshoz egy dedikált számítógépre. Eszközök, mint PerfMon Processzor, a memória és a sávszélesség használatának megvizsgálni az átjárót futtató számítógépen másolás művelet során. Ha Processzor, memória vagy hálózati sávszélesség egy szűk váltson egy nagyobb teljesítményű számítógépre.

**Egyidejű másolás tevékenység fut**: az adatkezelési átjáró egyetlen példány szolgálhat egyszerre, akár egyidejűleg több másolatot tevékenység lefut. Egyidejű feladatok maximális száma számítása az átjáró-számítógép hardverkonfigurációja alapján történik. További másolási feladatok sorban állnak, amíg az üzeneteket fel nem átjáró, illetve amíg állást változtat időtúllépés történik. Erőforrás kérelem az átjárót futtató számítógépen elkerülése érdekében a várakozási sorban található feladatok másolása a számának csökkentése egy időben másolás tevékenység időbeosztását szakasz, vagy inkább a felosztása több átjáró-számítógép alakzatot a betöltés.


## <a name="other-considerations"></a>Egyéb szempontok
Ha a másolni kívánt adatok mérete túl nagy, beállíthatja, hogy további partíciót a vállalati logika a az adatoknak a slicing mechanizmusa Data Factory. Ezután ütemezése másolás tevékenység gyakrabban futtatásához futtatása másolás tevékenységeknél adatok méretének csökkentése érdekében.

Legyen óvatos adatkészletek és adatok gyári igénylő az azonos adatokat tárolón összekötőre egyszerre másolás tevékenységek száma. Előfordulhat, hogy sok egyidejű másolási feladatok egy adattár szabályozása, és érdeklődő csökkentett teljesítmény elérése érdekében másolás belső újrapróbálkozások feladatot, és bizonyos esetekben végrehajtási hibákat tapasztal.

## <a name="sample-scenario-copy-from-an-on-premises-sql-server-to-blob-storage"></a>Példa: egy helyszíni SQL Server másolás Blob-tárolóhoz
**Példa**: egy folyamat épül adatok másolása egy helyszíni SQL Server Blob-tárolóhoz CSV-formátumban. Ha a másolási feladatot gyorsabb, a CSV-fájlok bzip2 formátumba lehet tömöríteni.

**Próba- és analysis**: A másolás tevékenység kapacitásának érték kevesebb mint 2 MB, amely a teljesítmény viszonyítási alap sokkal lassabb.

**Teljesítmény elemzése és beállítása**: a probléma elhárításához tekintse meg a adatainak feldolgozása és áthelyezve.

1.  **Adatok olvasása**: átjáró megnyit egy SQL Server kapcsolatot, és elküldi a lekérdezést. Az SQL Server válaszol a adatfolyam küldése az átjáró az intraneten keresztül.
2.  **Adatok szerializált és tömörítés**: átjáró serializes az adatok adatfolyam az a CSV formátum, és az adatok bzip2 adatfolyam tömöríti.
3.  **Adatok írásához**: az átjáró hatékonyan feltölti az bzip2 adatfolyam Blob-tárolóhoz az interneten keresztül.

Amint látható, az adatok folyamatban van feldolgozott, és a továbbított egymás után következő módon került: az SQL Server > LAN > átjáró > WAN > Blob-tároló. **Az általános teljesítményére engedi át a minimális átviteli végig a folyamat**.

![Adatfolyam](./media/data-factory-copy-activity-performance/case-study-pic-1.png)

Az alábbi tényezőket közül egy vagy több miatt megtelhet teljesítményét szűk:

-   **Forrás**: magát az SQL Server rendelkezik alacsony átviteli nagy terhelés miatt.
-   **Az adatkezelési átjáró**:
    -   **LAN**: átjáró távol az SQL Server számítógépen található, és még egy kis sávszélességű kapcsolat.
    -   **Átjáró**: átjáró megérkezett a betöltés korlátozások a következő műveleteket:
        -   **Szerializálási**: az adatok az a CSV formátum adatfolyam szerializálása van lassú a teljesítmény.
        -   **A tömörítési**: úgy döntött, hogy a lassú tömörítés kodek (például bzip2, amely az alapvető i7 2,8 MB).
    -   **WAN**: A sávszélesség a vállalati hálózathoz és a Azure szolgáltatások között halkítva (például T1 = 1,544 KB; T2 6,312 KB =).
-   **Gyűjtése**: Blob-tárolóhoz alacsony átviteli rendelkezik. (Ebben az esetben a valószínű, mert annak SLA garantálja 60 MB legalább.)

Ebben az esetben bzip2 adatok tömörítés előfordulhat, hogy is lassíthatják a teljes folyamat. A gzip tömörítés kodek átirányítása, előfordulhat, hogy a szűk megkönnyítése érdekében.


## <a name="sample-scenarios-use-parallel-copy"></a>Lehetséges eset: párhuzamos másolással  

**Forgatókönyv I:** A helyszíni fájlrendszer Blob-tárolóhoz 1000 1 MB-os fájlok másolása

**Elemzés és teljesítményének javítása**: példát, ha olyan a négyágú core gépen telepítette az átjáró Data Factory használ 16 párhuzamos másolatok fájlok áthelyezéséhez a fájlrendszerből Blob-tárolóhoz egyidejűleg. A párhuzamos végrehajtás magas átviteli kell eredményez. Is pontosan megadhatja a párhuzamos másolatok számát. Sok kisméretű fájlt másolásakor a párhuzamos másolatok jelentősen súgó átviteli erőforrások hatékonyabban használatával.

![Forgatókönyv 1](./media/data-factory-copy-activity-performance/scenario-1.png)

**Forgatókönyv II**: 500 MB-os 20 BLOB másolja Blob-tárolóhoz az adatok tó áruházból Analytics, és kattintson a teljesítményének finomhangolása.

**Elemzés és a teljesítményhangolás**: Ebben az esetben Data Factory átmásolja az adatokat Blob-tárolóhoz tó adattár egyetlen-példányt (**parallelCopies** értéke 1) és az adatok egyetlen-felhő mozgását egységek használatával. A erőforrásigények átviteli közelébe, amely ismerteti a [teljesítményét hivatkozás csoportban](#performance-reference).   

![Forgatókönyv 2](./media/data-factory-copy-activity-performance/scenario-2.png)

**Forgatókönyv III**: egyes a fájl mérete tucatnyi MB-nál nagyobb, és a teljes hangereje nagy.

**Elemzés és bekapcsolásával teljesítmény**: **parallelCopies** növelésével nem oszt hatékonyabb másolása egy egyetlen-felhő DMU erőforrás korlátai miatt. Ehelyett adjon meg további felhő DMUs megszerezni a további erőforrások végezze el az adatok mozgását. Nem adja meg a **parallelCopies** tulajdonság értékét. Adatok gyári kezeli a a párhuzamos meg. Ebben az esetben ha a **cloudDataMovementUnits** 4, egy átviteli a körülbelül négyszer fordul elő.

![Forgatókönyv 3](./media/data-factory-copy-activity-performance/scenario-3.png)

## <a name="reference"></a>Hivatkozás
Az alábbiakban a teljesítmény figyelése és egyes a támogatott adatokat tárolja a hivatkozások beállítása:

- Azure tároló (beleértve Blob-tárolóhoz és táblatároló): [Azure tároló méretezhetőség tárolók](../storage/storage-scalability-targets.md) és [Azure tároló teljesítmény és méretezhetőség feladatlista](../storage//storage-performance-checklist.md)
- Azure SQL-adatbázis: [A teljesítmény monitor](../sql-database/sql-database-service-tiers.md#monitoring-performance) is, és jelölje be az adatbázis tranzakció egység (DTU) százalékban
- Azure SQL-adatraktár: A képesség mérése adatok raktári egységek (DWUs); Lásd: a [kezelése power az Azure SQL-adatraktár (áttekintés) számítja ki.](../sql-data-warehouse/sql-data-warehouse-manage-compute-overview.md)
- [Teljesítményszint DocumentDB az](../documentdb/documentdb-performance-levels.md) Azure DocumentDB:
- A helyszíni SQL Server: [Monitorára és a teljesítmény elérése érdekében finomításához](https://msdn.microsoft.com/library/ms189081.aspx)
- A helyszíni a fájl kiszolgálói: [teljesítményhangolás az fájlkiszolgálókhoz](https://msdn.microsoft.com/library/dn567661.aspx)
