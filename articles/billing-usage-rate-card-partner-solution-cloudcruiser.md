<properties
   pageTitle="A felhő Cruiser és számlázási API-integráció a Microsoft Azure |} Microsoft Azure"
   description="Microsoft Azure számlázási partner felhő Cruiser, a szolgáltatások az Azure számlázási API-k integrálása a termék egyedi perspektíva ismertetése  Az Azure és a felhő Cruiser ügyfelek, amely érdekli használatával/történt a felhőben Cruiser a Microsoft Azure Pack különösen hasznos."
   services=""
   documentationCenter=""
   authors="BryanLa"
   manager="mbaldwin"
   editor=""
   tags="billing"
   />

<tags
   ms.service="billing"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="billing"
   ms.date="09/08/2016"
   ms.author="mobandyo;sirishap;bryanla"/>

# <a name="cloud-cruiser-and-microsoft-azure-billing-api-integration"></a>A felhő Cruiser és a Microsoft Azure számlázási API-integráció

Ez a cikk azt ismerteti, hogyan az új Microsoft Azure számlázási API-gyűjtött adatok használható felhő Cruiser munkafolyamat költség szimulációk és elemzés.

## <a name="azure-ratecard-api"></a>Azure RateCard API
A RateCard API az Azure ráta információk. Miután hitelesítése a megfelelő hitelesítő adatokkal, lekérdezheti a API a rendelkezésre álló szolgáltatásokat metaadatait összegyűjtéséhez Azure-együtt a mértékek társított kínálnak azonosítójával.

Az alábbi minta választ vár, a megjelenítő árak esetében a A0 API (Windows) példány:

    {
        "MeterId": "0e59ad56-03e5-4c3d-90d4-6670874d7e29",
        "MeterName": "Compute Hours",
        "MeterCategory": "Virtual Machines",
        "MeterSubCategory": "A0 VM (Windows)",
        "Unit": "Hours",
        "MeterRates":
        {
            "0": 0.029
        },
        "EffectiveDate": "2014-08-01T00:00:00Z",
        "IncludedQuantity": 0.0,
        "MeterStatus": "Active"
    },

### <a name="cloud-cruisers-interface-to-azure-ratecard-api"></a>A felhő Cruiser's felületet Azure RateCard API
Felhőalapú Cruiser többféleképpen is kihasználhatja a RateCard API információk. Ez a cikk az bemutatjuk, hogyan használható, hogy IaaS terhelést költség szimulációk és elemzés.

A használati eset szemléltetésére Képzelje el, a terhelést több példányok futtatása a Microsoft Azure-csomag (WAP). A cél, hasonlóan ez a Azure azonos terhelést, és a Költségbecslés az ilyen áttelepítési módon. Létrehozásához a szimulációk, létezik a végrehajtani kívánt két fő feladatok:

1. **Importálás és a folyamat a mezőt a RateCard API gyűjtött.** Ebben a feladatban is történik a munkafüzetben, ahol a kivonat az RateCard API a transzformált és új ráta csomagra közzé. A ráta új csomag a szimulációk az Azure árak becsléséhez lesz.

2. **A normalizálandó WAP szolgáltatások és IaaS Azure szolgáltatásait.** Alapértelmezés szerint WAP szolgáltatások alapul egyes erőforrások (Processzor, memóriaméret, méretét, stb.) Azure közben a szolgáltatások alapján példány méret (A0, A1, A2 stb.). Ez az első tevékenység felhő Cruiser ETL motor által elvégzett, nevű munkafüzet, amennyiben ezek az erőforrások is kapcsolt példány méretű, papírlapnak megfelelő Azure példány szolgáltatások.

### <a name="import-data-from-the-ratecard-api"></a>Adatok importálása a RateCard API

Felhőalapú Cruiser munkafüzetek összegyűjtése és feldolgozása az RateCard API adatait automatikus módszert nyújt.  ETL (kivonat, átalakítás, betöltés) munkafüzetek konfigurálása a webhelycsoport, átalakítási és a felhő Cruiser adatbázisba adatok közzététele teszi lehetővé.

Minden olyan munkafüzet beállíthatja, hogy egy vagy több gyűjtemények, amivel összehangolására kiegészítése vagy kiegészítheti, a használati adatok különböző forrásból származó információkat. A következő két képernyőképek bemutatják, hogyan új *webhelycsoport* létrehozása egy meglévő munkafüzetet és a *webhelycsoport* a RateCard API importáló információt:

![Ábra 1 – olyan új webhelycsoportok létrehozása][1]

![Szám 2 - adatok importálása az új webhelycsoport][2]

Után importálja az adatokat a munkafüzetben, akkor hozhat létre több lépéseket és átalakítási folyamatokat, módosíthatja, és az adatok. Ez a példa azt érdeklik csak infrastruktúra-mint-a-szolgáltatás azt is lépésekkel hozhatja létre átalakítása szükségtelen sorok eltávolítása (IaaS) vagy a rekordok, mivel kapcsolatos szolgáltatások IaaS kívül.

Az alábbi képernyőképen a bemutatja átalakítása segítségével dolgozza fel a RateCard API gyűjtött adatokat:

![Ábra 3 - transzformáció lépéseket RateCard API összegyűjtött adatainak feldolgozása][3]

### <a name="defining-new-services-and-rate-plans"></a>Új szolgáltatások és ráta definiálása csomagok

Eltérő módon szolgáltatások definiálása a felhőben Cruiser. A lehetőségek közül, ha a szolgáltatások importálása a használati adatok. Ez a módszer általában arra használják, amikor nyilvános felhőket, ahol a szolgáltatásokkal már határozzák meg a szolgáltató használata.

A ráta megtervezése olyan díjat vagy árak esetében, amelyek különböző szolgáltatások változási dátumokat vagy egyéb lehetőségek közül a vevők csoportja alapján alkalmazhatók. Ráta csomagok is használható a felhőben Cruiser szimulációk vagy "Mi lenne, ha" forgatókönyvek, hogyan befolyásolják a változások a szolgáltatások is terhelési teljes költségét.

Ebben a példában fogjuk használni a mezőt a RateCard API az új szolgáltatások definiálhat a felhőben Cruiser. Ugyanúgy hogy segítségével a mértékek, a szolgáltatásokhoz kapcsolódó hozzon létre egy új ráta megtervezése felhő Cruiser.

Az átalakítás folyamat végén hozzon létre egy új lépést, és a RateCard API az adatok közzététele például új szolgáltatások és mértékek.

![Ábra 4 – a RateCard API adatainak közzétételi új szolgáltatások és kamatlábak szerint][4]

### <a name="verify-azure-services-and-rates"></a>Azure-szolgáltatások és Mértékek ellenőrzése

A szolgáltatások és -mértékek közzététele, után ellenőrizheti a felhőben Cruiser *szolgáltatások* lap importált szolgáltatások listában:

![Ábra 5 – az új szolgáltatások ellenőrzése][5]

Az *Arány tervek* lapon jelölje be az új ráta tervet, úgynevezett "AzureSimulation" a mértékek az importált RateCard API-val.

![Ábra 6 – az új ráta megtervezése és a kapcsolódó díjak ellenőrzése][6]

### <a name="normalize-wap-and-azure-services"></a>A normalizálandó WAP és Azure szolgáltatások

Alapértelmezés szerint WAP információk használatát számítási, a memória és a hálózati erőforrások felhasználása alapján. A felhőben Cruiser, határozhatja meg a szolgáltatások alapú közvetlenül a felosztás vagy ezek az erőforrások forgalmi díjas használatát. Például egyszerű díjának megadása processzorhasználata az órát, vagy a GB memóriát egy példány rendelt díjat.

Ebben a példában a költségek WAP és Azure-között összehasonlítása érdekében szükség összesítéséhez az Erőforrás kihasználtsága a WAP kötegeket, amely ezután csatolható Azure szolgáltatás be. Ez a transzformáció könnyen végrehajtható a munkafüzetekben:

![Ábra 7 - szolgáltatások normalizálandó WAP adatok átalakítása][7]

A munkafüzetben az utolsó lépésben, hogy az adatok közzététele a felhőben Cruiser adatbázisba. Során ebben a lépésben a használatát adatainak most kapcsolt szolgáltatások (feleltesse meg az Azure-szolgáltatások) be és alapértelmezett díjak hozhat létre a költségek területhez tartozik.

Befejezése a munkafüzetet, után automatizálhatja a adatok feldolgozásának tevékenység hozzáadása a ütemezőt, és adja meg a gyakoriság és az idő futtatása a munkafüzet.

![Ábra 8 - munkafüzet ütemezése][8]

### <a name="create-reports-for-workload-cost-simulation-analysis"></a>Jelentések létrehozása a terhelést a költség szimulációk elemzéshez

Után a használatát gyűjtött, és a költségek töltődnek be a felhőben Cruiser adatbázisba, azt is kihasználhatja a felhőben Cruiser háttérismeretek modul hozhat létre a terhelést a költség szimulációk, amely azt kívánja.

Ezt az alkalmazási helyzetet mutatja be, hogy a következő jelentés létrehozott:

![A költség összehasonlítása][9]

A felső grafikonon költség összehasonlítása szolgáltatásai, összehasonlítása az ár, a terhelést a az egyes bizonyos szolgáltatásokhoz futó WAP (sötétkék) és Azure (világoskék) között.

Az alsó diagram ábrázolja ugyanazokat az adatokat, de részleg szerinti bontásban. Ez a az egyes részlegek futtatni szeretne a terhelést a WAP és Azure-együtt őket a megtakarítási sáv (zöld) közötti különbség költségét jeleníti meg.

## <a name="azure-usage-api"></a>Azure használatát API


### <a name="introduction"></a>– Bevezetés

A Microsoft nemrégiben lehetővé tevő programozás útján lekérje a használati adatok eléréséhez az összefüggéseket a felhasználás az előfizetői Azure használatát API jelent meg. Ez a felhő Cruiser ügyfeleknek, hogy kihasználhassa az Ez az API elérhető sokoldalúbb adatkészlet remek híreket.

Felhőalapú Cruiser is kihasználhatja a használatát API többféleképpen integrációja. A granularitása (óránként kihasználtsági információk), és az API keresztül érhető el az erőforrás-metaadatok adatai biztosít a szükséges adatkészlet rugalmas Showback vagy visszaterhelési az adatmodellek támogatása. 

Ebben az oktatóanyagban hogyan felhő Cruiser használatát API információk előnyeivel egy példa bemutatja azt. Pontosabban azt fogja erőforrás csoport létrehozása az Azure a, a fiók szerkezet címkék társítani, majd ismertetik az adatok és a felhő Cruiser be a címke adatai feldolgozása folyamatát.
 
A végleges célja, hogy is olyanra, mint az alábbi jelentések létrehozása, és tudja elemezheti költség és a fiók struktúra, a címkék szerint töltve alapján felhasználás.

![Szám 10 - címkék használata részletezést jelentés][10]

### <a name="microsoft-azure-tags"></a>Microsoft Azure címkék

Az Azure használatát API keresztül érhető el az adatok nem csak fogyasztási adatai, de az erőforrás-metaadatait bármely, társított címkék együtt is tartalmaz. Címkék segítségével egyszerűen, de ahhoz, hogy a hatékonyabb rendszerezéséhez az erőforrások, szükséges ahhoz, hogy:

- Címkék megfelelően veszi át az erőforrások rendelkezést időben
- Címkék megfelelően szolgálnak a Showback/visszaterhelési folyamat kötik a használatát a szervezeti fiók struktúra.

Ezekről a követelményekről a nehéz, lehet, különösen akkor, ha a kézi végrehajtás nyújtására vagy töltődik, oldal. Elgépelt, nem a megfelelő, vagy még hiányzó címkék közös panaszok ügyfelek esetén a címkék és a hibák is megnehezítik leírási_idő feltöltés szélén nagyon.

Az új Azure használatát API-val felhő Cruiser adhat lekérés erőforrás címkézés információinak és keresztül egy összetett ETL nevű eszközt munkafüzetek, címkézési gyakori hibák kijavítása. Reguláris kifejezések és az adatok korrelációs átalakítás keresztül felhő Cruiser helytelen címkével ellátott erőforrások azonosítása és címkékkel a helyes, az erőforrások megfelelő társítás a fogyasztói biztosítása.

A feltöltés szélén a felhő Cruiser automatizálja Showback/visszaterhelési folyamatát, és is kihasználhatja a címkével kötik a használatát, hogy a megfelelő fogyasztói (részleg, divízió, a Project stb.) információkat. Ez az automatizálási egy nagyon nagy fokozása tartalmaz, és egy következetes és naplózható feltöltés folyamat gondoskodhat arról.
 

### <a name="creating-a-resource-group-with-tags-on-microsoft-azure"></a>Erőforrás a csoport létrehozása a Microsoft Azure címkékkel
Ebben az oktatóanyagban első lépésként a erőforrás csoport létrehozása az Azure-portálon kattintson a címkék az erőforrások társítani kívánt létrehozása. Ez a példa azt fog szervezni az alábbi címkéket: részleg, környezetben, tulajdonosának, a Project.

Az alábbi képernyőképen egy minta erőforráscsoport a társított címkék.

![Ábra 11 - erőforráscsoport Azure portálon társított címkékkel][11]

A következő lépésként az információkat olvashat a használatát API a be a felhőben Cruiser. A használati API jelenleg biztosít a válaszokat JSON formátumban. Íme egy mintája szerepel az adatforrásból:


    {
      "id": "/subscriptions/bb678b04-0e48-4b44-XXXX-XXXXXXX/providers/Microsoft.Commerce/UsageAggregates/Daily_BRSDT_20150623_0000",
      "name": "Daily_BRSDT_20150623_0000",
      "type": "Microsoft.Commerce/UsageAggregate",
      "properties":
      {
        "subscriptionId": "bb678b04-0e48-4b44-XXXX-XXXXXXXXX",
        "usageStartTime": "2015-06-22T00:00:00+00:00",
        "usageEndTime": "2015-06-23T00:00:00+00:00",
        "meterName": "Compute Hours",
        "meterRegion": "",
        "meterCategory": "Virtual Machines",
        "meterSubCategory": "Standard_D1 VM (Non-Windows)",
        "unit": "Hours",
        "instanceData": "{\"Microsoft.Resources\":{\"resourceUri\":\"/subscriptions/bb678b04-0e48-4b44-XXXX-XXXXXXXX/resourceGroups/DEMOUSAGEAPI/providers/Microsoft.Compute/virtualMachines/MyDockerVM\",\"location\":\"eastus\",\"tags\":{\"Department\":\"Sales\",\"Project\":\"Demo Usage API\",\"Environment\":\"Test\",\"Owner\":\"RSE\"},\"additionalInfo\":{\"ImageType\":\"Canonical\",\"ServiceType\":\"Standard_D1\"}}}",
        "meterId": "e60caf26-9ba0-413d-a422-6141f58081d6",
        "infoFields": {},
        "quantity": 8

      },
    },


### <a name="import-data-from-the-usage-api-into-cloud-cruiser"></a>Adatokat importálhat az használatát API felhő Cruiser

Felhőalapú Cruiser munkafüzetek összegyűjtése és feldolgozása az használatát API adatait automatikus módszert nyújt. Egy ETL (kivonat, átalakítás, betöltés) a munkafüzet konfigurálása a webhelycsoport, átalakítási és a felhő Cruiser adatbázisba adatok közzététele teszi lehetővé.

Minden olyan munkafüzet beállíthatja, hogy egy vagy több lekérdezésére. Ez lehetővé teszi, hogy összehangolására kiegészítése vagy kiegészítheti, a használati adatok különböző forrásból származó információkat. Ebben a példában azt hoz létre egy új lapon az Azure-sablon munkafüzet (_UsageAPI)_ és be egy új _webhelycsoport_ adatok importálása a használatát API-t.

![Ábra: 3 - importálja a UsageAPI lap használatát API-adatok][12]

Figyelje meg, hogy a munkafüzet már van-e más munkalapok szolgáltatások az Azure (_ImportServices_), és a számlázás API (_PublishData_) felhasználási adatainak feldolgozása.

Ezután azt használja a használatát API _UsageAPI_ lap feltöltéséhez, és az információkat a _PublishData_ -lapon a számlázási API felhasználási adataival összehangolására.

### <a name="processing-the-tag-information-from-the-usage-api"></a>A használati API címke adatainak feldolgozása

Miután importálja az adatokat a munkafüzetben, azt hoz létre átalakítása lépéseket _UsageAPI_ tartalmazó lapon annak érdekében, hogy az API adatainak feldolgozása. Első lépésként egy "JSON felosztása" processzor segítségével bontsa ki a címkék mezőre, majd a mezők létrehozása minden az egyik (részleg, Project, tulajdonos és környezet).

![Ábra 4 – a címke információt új mezők létrehozása][13]

Figyelje meg a "Networking" szolgáltatás hiányzik a címke információkat (sárga be), de azt is ellenőrizze, hogy az a _ResourceGroupName_ mező megjeleníti az azonos erőforráscsoport részét. Van még az erőforrások erőforráscsoport a címkéket, mert azt ezen információk segítségével a hiányzó címkék alkalmazása az erőforrás később.

A következő lépésként társítása az adatokat a _ResourceGroupName_a címkéket a keresési tábla létrehozása. A keresési tábla használandó a következő lépés a címke információkkal a felhasználási adatok kiegészítése.

### <a name="adding-the-tag-information-to-the-consumption-data"></a>A címke információ hozzáadása a felhasználási adatok

Most azt ugorhat a _PublishData_ lapra, amely a dolgozza fel a számlázási API felhasználási adatait, és vegye fel a mezőket kiolvasott címkéket. Ez a folyamat megjeleníti a keresési tábla a _ResourceGroupName_ a használják a keresések a billentyűjét, az előző lépésben létrehozott történik.

![Ábra 5 – a fiók-struktúra a keresések adataival feltöltése][14]

Figyelje meg, hogy a megfelelő fiókot struktúra mezők a "Networking" szolgáltatás a volt alkalmazott, a hiányzó címkéket a problémák orvoslására: a probléma. Azt is töltve a fiók struktúra mezők az erőforrások nem a cél erőforráscsoport "Egyéb", annak érdekében, hogy elkülönüljön őket a jelentésekben.

Most már csak szükség hozzáadásához a használati adatok közzététele. Ez a lépés során az egyes szolgáltatásokhoz definiált a ráta tervezése a megfelelő díjak fog vonatkozni a használatát adatait, és az eredményül kapott kamat töltődik be az adatbázis.

A legjobb része csak hogy ezt a folyamatot egyszer folyamatát. A munkafüzet befejezése után fel szeretne venni az ütemező kell, és az ütemezett időpontban fog futni óránkénti vagy a napi. Új kimutatások létrehozása és testreszabása a meglévőket, annak érdekében, hogy értelmes háttérismeretek beolvasni a felhőben használatát az adatok elemzése a témát csak akkor.

### <a name="next-steps"></a>Következő lépések

+ Részletes útmutató felhő Cruiser munkafüzetek és jelentések létrehozása olvassa el felhő Cruiser online [dokumentációt](http://docs.cloudcruiser.com/) (érvényes bejelentkezés szükséges).  További információt a felhőben Cruiser, kérjük, forduljon az [info@cloudcruiser.com](mailto:info@cloudcruiser.com).
+ Lásd: [szerezhet be a Microsoft Azure erőforrás-felhasználás mélyebb](billing-usage-rate-card-overview.md) az Azure Erőforrás kihasználtsága és RateCard API-khoz áttekintése.
+ Nézze meg az [Azure számlázási REST API -val hivatkozás](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) mindkét API-hoz, az Azure-kezelő által biztosított API-khoz halmazára részét képező további információt.
+ Ha a kívánt kördiagramjaira jobbra a minta kódot be, olvassa el a Microsoft Azure számlázási API mintakódok a [Azure mintakódok](https://azure.microsoft.com/documentation/samples/?term=billing).

### <a name="learn-more"></a>tudj meg többet
+ Ha többet szeretne tudni az Azure erőforrás-kezelő [Azure erőforrás szolgáltatásának áttekintése](azure-resource-manager/resource-group-overview.md) témakörben.

<!--Image references-->
 
[1]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Create-New-Workbook-Collection.png "Ábra 1 – olyan új webhelycsoportok létrehozása"
[2]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Import-Data-From-RateCard.png "Szám 2 - adatok importálása az új webhelycsoport"
[3]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Transformation-Steps-Process-RateCard-Data.png "Ábra 3 - transzformáció lépéseket RateCard API összegyűjtött adatainak feldolgozása"
[4]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Publish-RateCard-Data-New-Services-Rates.png "Ábra 4 – a RateCard API adatainak közzétételi új szolgáltatások és kamatlábak szerint"
[5]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Verify-Azure-Services-And-Pricing1.png "Ábra 5 – az új szolgáltatások ellenőrzése"
[6]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Verify-Azure-Services-And-Pricing2.png "Ábra 6 – az új ráta megtervezése és a kapcsolódó díjak ellenőrzése"
[7]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Transforming-WAP-Normalize-Services.png "Ábra 7 - szolgáltatások normalizálandó WAP adatok átalakítása"
[8]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Workbook-Scheduling.png "Ábra 8 - munkafüzet ütemezése"
[9]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Workload-Cost-Simulation-Report.png "Ábra 9 - mintajelentés a terhelést költség összehasonlító eset"
[10]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/1_ReportWithTags.png "Szám 10 - címkék használata részletezést jelentés"
[11]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/2_ResourceGroupsWithTags.png "Ábra 11 - erőforráscsoport Azure portálon társított címkékkel"
[12]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/3_ImportIntoUsageAPISheet.png "Ábra 12 - importálja a UsageAPI lap használatát API-adatok"
[13]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/4_NewTagField.png "Ábra 13 - hozhatók létre új mezők a címke-információ"
[14]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/5_PopulateAccountStructure.png "Ábra 14 - feltöltése a fiók-struktúra a keresések adataival"
