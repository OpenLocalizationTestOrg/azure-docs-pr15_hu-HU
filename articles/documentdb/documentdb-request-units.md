<properties 
    pageTitle="DocumentDB egységek kérése |} Microsoft Azure" 
    description="További tudnivalók, adja meg, és kérelem egység követelményei DocumentDB becslése." 
    services="documentdb" 
    authors="syamkmsft" 
    manager="jhubbard" 
    editor="mimig" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/29/2016" 
    ms.author="syamk"/>

#<a name="request-units-in-documentdb"></a>DocumentDB egységek kérése
Most már elérhető: DocumentDB [kérelem egység Számológép](https://www.documentdb.com/capacityplanner). További [Estimating a átviteli van szüksége](documentdb-request-units.md#estimating-throughput-needs).

![Átviteli Számológép][5]

##<a name="introduction"></a>– Bevezetés
Ez a cikk áttekintést nyújt a [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/)kérelem egységek. 

Ez a cikk elolvasása, után is az alábbi kérdésekre választ:  

-   Mik azok a mennyiség kérni, és kérelmezze az díjak?
-   Hogyan adja meg a kérelem egység kapacitás gyűjtemény?
-   Hogyan becslése, az alkalmazás kérelem egységhez van szüksége?
-   Mi történik, ha lehet nagyobb, mint kérelem egység kapacitás gyűjtemény?


##<a name="request-units-and-request-charges"></a>Erőforrás-mennyiség kérése, és kérje a költségek
DocumentDB gyors, előre látható teljesítményt nyújt az erőforrások *lefoglalására* igényeinek kielégítéséhez, az alkalmazás átviteli van szüksége.  Alkalmazás betöltése és mintázatok időbeli változás elérni, mert a DocumentDB lehetővé teszi, egyszerűen növeléséhez vagy csökkentéséhez fenntartott átviteli érhető el az alkalmazás.

A DocumentDB fenntartott átviteli másodpercenként feldolgozása kérelem egységek számát tekintve van megadva.  Érdemes elképzelnie kérelem egységek átviteli pénznemként, amellyel garantált kérelem egységek érhető el az alkalmazás másodpercenként alapon *lefoglalása* az összeget.  Minden egyes művelet DocumentDB - dokumentum írása, kérdezni, egy dokumentum frissítése – a Processzor, a memória és IOPS fogyaszt.  Ez azt jelenti, hogy az egyes műveletek vonz *kérelem ingyenesen*, *kérelem*egységekben kifejezett.  Ismertetése a tényezők, amelyek befolyásolhatja a kérelem egység díjakat, az alkalmazás átviteli követelmények, valamint lehetővé teszi, hogy az alkalmazás futtatásához a lehető leghatékonyabban költség. 

##<a name="specifying-request-unit-capacity"></a>Kérés egység kapacitás meghatározása
DocumentDB gyűjtemény létrehozásakor másodpercenként (RUs), amelyet a webhelycsoport számára fenntartott kérelem egységek számát adja meg.  A webhelycsoport létrehozása után a megadott RUs teljes felosztását számára a webhelycsoport van fenntartva.  Minden egyes webhelycsoport garantált dedikált és elszigetelt átviteli jellemzőiket befolyásolják.  

Fontos tudni, hogy működik-e DocumentDB foglalás modell; Ez azt jelenti, hogy akkor is számlát kapni ennyi átviteli *fenntartott* a gyűjteményben, függetlenül attól, mennyi adott átviteli aktívan *használja*.  Felhívjuk a figyelmét arra, akkor jó helyen jár, az alkalmazás betöltése, az adatok és könnyen méretezheti mezőjében a mennyiségét használatát mintázatok módosítása, amely fenntartott RUs DocumentDB SDK vagy az [Azure-portálon](https://portal.azure.com)keresztül.  Fel és le skála átviteli alkalmazásba című témakör tartalmaz további tudnivalókért lásd: [DocumentDB teljesítményszint](documentdb-performance-levels.md).

##<a name="request-unit-considerations"></a>Kérés egység kapcsolatos szempontok
A DocumentDB webhelycsoport foglalása kérelem mennyiségek becslése, amikor célszerű figyelembe az alábbi változók:

- **Dokumentum mérete**. Mivel a dokumentum méretének növelése a olvasása, és írja be az adatokat is növelik elfogyasztott mennyiség.
- **Dokumentumok tulajdonság számát**. Feltételezve alapértelmezett indexelés minden tulajdonság, a dokumentum írni elfogyasztott mennyiség növelik a tulajdonság száma növekszik.
- **Adatok konzisztencia**. Erős vagy határolt Staleness konzisztencia szintű adatok használata esetén a dokumentumok olvasásához további egységek fogyni fog.
- **Indexelt tulajdonságait**. Az index házirend a egyes határozza meg, hogy mely tulajdonságokat indexelve vannak alapértelmezés szerint. A kérelem egység felhasználási csökkentheti az indexelt tulajdonságok számának korlátozásával vagy Lusta indexelés engedélyezésével.
- A **dokumentum indexelés**. Minden dokumentum automatikusan indexelt alapértelmezés szerint kevesebb kérelem egységek, ha úgy dönt, hogy nem index néhány dokumentum felhasználja meg.
- **Lekérdezés mintázatok**. A lekérdezés komplexitását hatással van, hány kérése egységek felhasznált művelet. Predikátumok számú, a predikátumok, előrejelzések, UDF számát és az összes adatforrás adatkészlet méretének jellegét befolyásolhatja a lekérdezési műveletek a költségét.
- A **parancsfájlok használatát**.  Lekérdezés, a tárolt eljárások és indítók mobilalkalmazásokban kérelem egységek a folyamatban lévő művelet komplexitását alapján Az alkalmazásokat fejleszt, mint a vizsgálat gombra a kérés ingyenesen fejléce megértéséhez, hogyan az egyes műveletek használata más kérelem egység kapacitása.

##<a name="estimating-throughput-needs"></a>Átviteli igényeinek becslése
A kérelem egység normalizált méri-összehívás feldolgozása költség. Egyetlen kérelem időegység jelöli a feldolgozás kapacitás (keresztül önkiszolgáló hivatkozás vagy azonosító) olvasásához (kivéve a Rendszertulajdonságok) 10 tulajdonság egyedi értékeket tartalmazó egyetlen 1KB JSON dokumentum szükséges. Kérelmének (Beszúrás) létrehozása, cseréje vagy törlése a dokumentumon felhasználja további feldolgozása a szolgáltatást, és egyúttal további kérelem egységek.   

> [AZURE.NOTE] Az Alapterv 1 kérelem egység 1KB dokumentum megfelel egy egyszerű GET önkiszolgáló hivatkozás vagy a dokumentum azonosítója.

###<a name="use-the-request-unit-calculator"></a>A kérés egység Számológép használata
A Súgó aggódnia ügyfelek finomhangolása a átviteli módszerekkel becsült lép, és webalapú [kérelem egység Számológép](https://www.documentdb.com/capacityplanner) segíti a szokásos műveletek, beleértve a kérelem egység követelményei becslése:

- Dokumentum létrehozása (írások)
- Dokumentumok olvasása
- Dokumentum törlése
- Dokumentum-frissítések

Az eszköz is a minta dokumentumok kiszámlázásakor alapján adatokat tároló igényeinek becslése támogatása.

Az eszköz használata egyszerű:

1. Egy vagy több képviselő JSON dokumentumok feltöltése.

    ![Dokumentumok feltöltése a kérelem egység Számológép][2]

2. Adatok tárhelyre becsléséhez, adja meg a dokumentumok tárolásához várt száma.

3. Írja be a dokumentum létrehozása, olvasása, frissítése és törlése (másodpercenként alapon) szükséges műveletek. Átböngészve felmérheti a kérelem egység költségek dokumentum frissítés műveletek, töltse fel a minta lépés: 1, a fenti tipikus mező frissítéseket tartalmazó dokumentumok másolatát.  Például módosításakor dokumentum frissítéseiről rendszerint két tulajdonságok lastLogin és userVisits nevű fájlt, majd egyszerűen minta dokumentum másolása a két tulajdonságokat értékének módosítása és a másolt dokumentum feltöltése.

    ![Írja be a kérelem egység Számológép átviteli követelmények][3]

4. Kattintson a calculate, és ellenőrizze az eredményt.

    ![Egység Számológép eredményei kérése][4]

>[AZURE.NOTE]Ha dokumentumtípusok, amelyek értelmez a méret és tulajdonságok indexelt száma jelentősen eltérnek, töltse egy mintája szerepel az egyes tipikus dokumentum *típusát* a eszköz, és jelölje be az eredmények számítása.

###<a name="use-the-documentdb-request-charge-response-header"></a>A DocumentDB kérelem ingyenesen válasz fejlécének használata
A DocumentDB szolgáltatásból származó minden válasza egy egyéni élőfejet (x ms-kérés-ingyenes), amely tartalmazza az elfogyasztott mennyiség a kérelem kérelem egységek tartalmazza. Ezt a fejlécet a DocumentDB SDK keresztül is érhető el. A .NET SDK RequestCharge a ResourceResponse objektum tulajdonsága.  Lekérdezések esetén a DocumentDB lekérdezés Intéző az Azure-portálon információk kérelem ingyenesen végrehajtott lekérdezések.

![A lekérdezés Explorer Licencelési díjak vizsgálata][1]

Ezzel a szem előtt, módja az alkalmazáshoz szükséges fenntartott átviteli összeg rögzítése a kérelem egység díjat tevékenységével szokásos műveletek az alkalmazás által használt képviselő dokumentum ellen becslése, és majd becslése műveletek száma várhatóan sok minden második hajt végre.  Győződjön meg arról, mérésére és tipikus lekérdezések és DocumentDB parancsfájl, valamint használatát.

>[AZURE.NOTE]Ha dokumentumtípusok, amelyek értelmez a méret és tulajdonságok indexelt száma jelentősen eltérnek, jegyezze fel a megfelelő műveletet kérelem egység díjat társított egyes tipikus dokumentum *típusát* .

Példa:

1. A kérelem egység díjat (Beszúrás) létrehozásának rögzítése egy tipikus dokumentumot. 
2. A kérés egység díjat dokumentum tipikus beolvasásának rekord.
3. A kérés egység díjat a szokásos dokumentumok frissítés rekord.
3. A kérés egység díjat tipikus, közös dokumentumok lekérdezések rekord.
4. A kérés egység költséget bármilyen egyéni parancsfájlok (tárolt eljárások, eseményindítók, felhasználó által definiált függvények) alkalmazásával kapcsolatos károkozásra rögzítése
5. A becsült számmal műveletek várhatóan sok minden második futtatásához szükséges kérés egységek kiszámítása.

##<a name="a-request-unit-estimation-example"></a>A kérés egység becslés példa
Fontolja meg a következő ~ 1KB dokumentumot:

    {
     "id": "08259",
    "description": "Cereals ready-to-eat, KELLOGG, KELLOGG'S CRISPIX",
    "tags": [
        {
        "name": "cereals ready-to-eat"
        },
        {
        "name": "kellogg"
        },
        {
        "name": "kellogg's crispix"
        }
    ],
    "version": 1,
    "commonName": "Includes USDA Commodity B855",
    "manufacturerName": "Kellogg, Co.",
    "isFromSurvey": false,
    "foodGroup": "Breakfast Cereals",
    "nutrients": [
        {
        "id": "262",
        "description": "Caffeine",
        "nutritionValue": 0,
        "units": "mg"
        },
        {
        "id": "307",
        "description": "Sodium, Na",
        "nutritionValue": 611,
        "units": "mg"
        },
        {
        "id": "309",
        "description": "Zinc, Zn",
        "nutritionValue": 5.2,
        "units": "mg"
        }
    ],
    "servings": [
        {
        "amount": 1,
        "description": "cup (1 NLEA serving)",
        "weightInGrams": 29
        }
    ]
    }

>[AZURE.NOTE]Dokumentumok DocumentDB, a rendszer minified szűrést, a rendszer a fenti dokumentum mérete kicsit kisebb, mint 1KB.


A következő táblázat mutatja közelítő kérelem ehhez a dokumentumhoz (a közelítő kérelem egység díjat feltételezi, hogy a "Munkamenet" a fiók konzisztencia szint van beállítva, és minden dokumentum automatikusan indexelve vannak) tipikus műveletekhez egység díjak:

Művelet|Egység ingyenesen kérése 
---|---
Dokumentum létrehozása|~ 15 LICENCELÉSI 
Dokumentumok olvasása|~ 1 LICENCELÉSI
Lekérdezés dokumentumok azonosító alapján|LICENCELÉSI ~2.5

Emellett az alábbi táblázat tartalmazza közelítő kérelem egység díjak alkalmazásban használt tipikus lekérdezések:

Lekérdezés|Egység ingyenesen kérése|# a visszaadott dokumentumok
---|---|--- 
Jelölje ki a élelmiszer azonosító szerint|LICENCELÉSI ~2.5|1 
Jelölje ki a gyártójuk élelmiszerek|~ 7 LICENCELÉSI|7
Ételek csoport és a sorrend szerint vastagság kijelöléséhez|~ 70 LICENCELÉSI|100
Jelölje ki a felső 10 élelmiszerek élelmiszer csoport|~ 10 LICENCELÉSI|10

>[AZURE.NOTE]A visszaadott dokumentumok számára függően változnak, Licencelési díjak.

Ezekkel az információkkal azt is becsüljük meg az alkalmazást, műveletek és másodpercenként megakadályozási lekérdezések száma megadott Licencelési követelményei:

A művelet vagy lekérdezés|Becsült szám / szekundum|Szükséges RUs 
---|---|--- 
Dokumentum létrehozása|10|150 
Dokumentumok olvasása|100|100 
Jelölje ki a gyártójuk élelmiszerek|25|175 
Ételek csoport kijelölése|10|700 
Jelölje ki a felső 10|15|150 összesen|155|1275

Ebben az esetben egy átlagos átvitt követelmény 1,275 Licencelési/s megakadályozási.  Kerekítés a legközelebbi 100 felfelé azt az alkalmazást a webhelycsoport 1,300 Licencelési/s volna kiépítése.

##<a id="RequestRateTooLarge"></a>Meghaladó fenntartott átviteli korlátai
A visszahívási, hogy kérelem egység felhasználás kiértékelése másodpercenként egy díjértéket. Amelyekre a kiépített kérelem egység ráta gyűjtemény alkalmazásokhoz kéréseit az adott webhelycsoport fog kell szabályozott mindaddig, amíg a ráta fenntartott szint alá esik. A szabályozási fordul elő, ha a kiszolgáló preemptively befejezheti a kérelmet, RequestRateTooLargeException (HTTP állapotkód 429), és lépjen vissza az x-ms-újrapróbálkozási-után-az ms fejléc, az időtartamot ezredmásodpercben, a felhasználónak kell várniuk, mielőtt a kérelem megoldódhat jelző.

    HTTP Status 429
    Status Line: RequestRateTooLarge
    x-ms-retry-after-ms :100

A .NET-ügyfél SDK és LINQ lekérdezést, akkor Önnek nem kell kezelni az Ez a kivétel, az aktuális verziója az ügyfél .NET SDK implicit módon kezeli a válasz legtöbbször használja, ha megőrzi a kiszolgáló által meghatározott újrapróbálkozási után fejléc, és próbálkozások a kérést. A fiók egy időben használatban van több ügyfél által, kivéve a következő újrapróbálkozási fog sikerülni.

Ha egynél több ügyfél összesítve felett a kérelem ráta működő újrapróbálkozási alapértelmezés esetleg nem elegendő, és az ügyfél throw fog egy DocumentClientException 429 állapotkód, az alkalmazás. Például ez esetben akkor fontolja meg ismét viselkedés és az alkalmazás hibát eljárások kezelésének vagy a gyűjtemény a fenntartott átviteli növekvő logika kezelése.

##<a name="next-steps"></a>Következő lépések

Többet szeretne tudni az Azure DocumentDB adatbázisokkal fenntartott átviteli, hogy az alábbi források:
 
- [DocumentDB árak](https://azure.microsoft.com/pricing/details/documentdb/)
- [DocumentDB kapacitás kezelése](documentdb-manage.md) 
- [Modellezési DocumentDB az adatok](documentdb-modeling-data.md)
- [Teljesítményszint DocumentDB](documentdb-partition-data.md)

DocumentDB kapcsolatos további tudnivalókért lásd: az Azure DocumentDB [dokumentációt](https://azure.microsoft.com/documentation/services/documentdb/). 

A méretezés és teljesítményének a DocumentDB tesztelésekor, című cikkben ismerkedhet [a teljesítmény és a méretezés az Azure DocumentDB tesztelése](documentdb-performance-testing.md).


[1]: ./media/documentdb-request-units/queryexplorer.png 
[2]: ./media/documentdb-request-units/RUEstimatorUpload.png
[3]: ./media/documentdb-request-units/RUEstimatorDocuments.png
[4]: ./media/documentdb-request-units/RUEstimatorResults.png
[5]: ./media/documentdb-request-units/RUCalculator2.png
