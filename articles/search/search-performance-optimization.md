<properties 
    pageTitle="Azure keresési teljesítmény és optimalizálási szempontok |} Microsoft Azure" 
    description="Azure keresési teljesítményének finomhangolása, és állítsa be az optimális skála" 
    services="search" 
    documentationCenter="" 
    authors="LiamCavanagh" 
    manager="pablocas" 
    editor=""/>

<tags 
    ms.service="search" 
    ms.devlang="rest-api" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="10/17/2016" 
    ms.author="liamca"/>

# <a name="azure-search-performance-and-optimization-considerations"></a>Azure keresési teljesítmény és optimalizálási kapcsolatos szempontok

Egy nagy keresőmoduljának egy kulcsot sikeres sok Mobile és a webalkalmazások. Az ingatlan autós piactér online katalógusok, amely a fast search és a megfelelő eredményeket hatással van a felhasználói felület. A dokumentum célja, hogy megtalálja gyakorlati tanácsok a ki a legtöbbet Azure keres, különösen az összetett követelményei méretezhetőség, speciális esetek több nyelv támogatása súgó vagy egyéni rangsorolást.  Emellett a dokumentum ismertet a belső, és ismerteti azokat a való életben ügyfél alkalmazások hatékony működnek az alábbi módszerek.

## <a name="performance-and-scale-tuning-for-search-services"></a>A teljesítmény és a méret beállítása a keresési szolgáltatás

Azt az összes használt keresőprogramok, például a Bing és a Google és a nagy teljesítményű kínálnak.  Eredményt adja amikor az ügyfelek a keresési engedélyező webhelyen vagy a mobilalkalmazás használja, elvárt lesz hasonló teljesítményt nyújt.  Amikor keresés teljesítményével optimalizálás, a legjobb megközelítés egyik szűkítheti az időtartam, amelyek a lekérdezés végrehajtása és a találatokat is visszaadjon szükséges időt.  Amikor keresés késés fontos, hogy optimalizálás:

1. Válassza ki a befejezéséhez egy tipikus keresés kérő cél időtartama (vagy maximális időtartama) kell vennie.

2. Hozzon létre, és tesztelje a valós terhelési szemben a keresési szolgáltatás a felmérni, ezek a késés kamatlábak reálisan adatkészletet.

3. Kevés lekérdezések / szekundum (QPS) kezdődik, és továbbra is a próba végrehajtott mindaddig, amíg a lekérdezési időtartam a definiált célértékhez várakozási alá esik számát növelni.  Ez a skála tervezéséhez az alkalmazás használatát a növekedésével egy fontos javasolt.

4. Amikor csak lehetséges, használja a HTTP-kapcsolatok újra.  Ha használja az Azure keresési .NET SDK, ez azt jelenti, példány vagy [SearchIndexClient](https://msdn.microsoft.com/library/azure/microsoft.azure.search.searchindexclient.aspx) példány szabad újra, és a REST API-t használja, ha egy egyetlen HttpClient szabad újra.
 
Ezek létrehozásakor tesztelése munkaterhelésekből, létezik néhány jellemzői Azure keresési, amelyre érdemes figyelni:

1. Ajánlatos leküldéses, hány keresési lekérdezést egyszerre, hogy az Azure keresési szolgáltatás elérhető erőforrások túl sok a program.  Amikor ez történik, látni fogja a HTTP 503 válasz kódok.  Emiatt célszerű különböző tartománnyal keresési kérelem megjelenítéséhez a késés díjak közötti különbségek felvételekor további keresési kérelem indítása.

2. Azure keresés tartalmak feltöltése lesz hatással lehet a általános teljesítményt és az Azure keresési szolgáltatás késés.  Ha várhatóan elküldhesse az adatokat, miközben a felhasználók keresést végez, fontos vesz figyelembe a terhelést a vizsgálatok.

3. Nem minden keresési lekérdezés az azonos szinten teljesítmény hajtja végre.  Ha például egy dokumentumot a Keresés vagy a keresési javaslatok általában végez gyorsabban lekérdezés nagyszámú metszettel és a szűrők az.  Célszerű a különböző lekérdezések megtekintéséhez figyelembe a vizsgálatok készítésekor várt állapotba.  

4. Keresési kérelem változata, fontos, mert a folyamatosan hajtsa végre az azonos keresési kérések, ha az adatok gyorsítótárba indul el, hogy a teljesítmény szebben, mint azt valószínűleg több elemezve lekérdezéssel van beállítva.

> [AZURE.NOTE] [Visual Studio betöltés tesztelése](https://www.visualstudio.com/docs/test/performance-testing/run-performance-tests-app-before-release) valójában jó módszer szerint lehetővé teszi, hogy hajtsa végre a HTTP-kérések Azure keresési lekérdezések végrehajtásának lenne szükség szerint a teljesítménytesztjeinek végrehajtásához, és lehetővé teszi a kérelmek parallelization.

## <a name="scaling-azure-search-for-high-query-rates-and-throttled-requests"></a>Azure keresési magas lekérdezés egységdíjakat méretezése és szabályozott kérések

Ha túl sok csökkentett kérések kapnak vagy haladja meg a cél késés díjak egy nagyobb lekérdezés betöltése az, keresse meg csökkentése késés díjak két módszer egyikével:

1. **Kópiák növelése:**  Kópia hasonlít az adatok Azure keresési betöltése szemben a példányok egyenleg kérések engedélyezése egy példányát.  Összes az terheléselosztás, és adatok keresztül kópiák replikációs Azure keresési kezeli, és megváltoztathatja a szolgáltatás bármikor kiosztott kópiák számát.  Szabványos keresési szolgáltatásának 12 kópiájába és egy egyszerű keresési szolgáltatás 3 kópiájába oszthat fel.  Kópiák módosítható az [Azure portál](search-create-service-portal.md) vagy az [Azure Keresés kezelése API](search-get-started-management-api.md)segítségével.

2. **Keresési réteg növelése:**  Azure keresési megtalálható [rétegek számát](https://azure.microsoft.com/pricing/details/search/) , és minden ezeket a meghatározási kínál, teljesítmény különböző szintjeit.  Egyes esetekben előfordulhat, hogy a réteg éppen nem adja meg eléggé kis késés díjak, akkor is, ha a kópiák vannak maximumot sok lekérdezések.  Ebben az esetben is érdemes feljebb helyezése a Azure keresési S3 réteg alkalmas esetek nagyszámú dokumentumokat és a nagyon magas lekérdezés munkaterhelésekből, például az újabb keresés rétegek közül.

## <a name="scaling-azure-search-for-slow-individual-queries"></a>Méretezési Azure keresés lassú egyedi lekérdezésekhez

Egy másik miért késés díjak lassú lehet oka befejezéséhez túl sok időbe telik egyetlen lekérdezésből.  Ebben az esetben kópiák hozzáadása nem növeli a késés díjak.  Ebben az esetben a két lehetőség érhető el:

1. **Partíciót növelése** Egy partíciót a kisalkalmazások az adatok felosztása a további erőforrások között.  Emiatt amikor felvesz egy második partíciót az adatok kap felosztása két.  A harmadik partíciót a tárgymutatóhoz osztja három stb.  Ez az, hogy bizonyos esetekben lassú lekérdezések gyorsabban fog miatt a számítási parallelization effektus tartalmaz.  Néhány példával, ahol azt van ez nagyon jól alacsony szelektivitás lekérdezéseket tartalmazó lekérdezések használata parallelization láthatók.  Ez a lekérdezéseket, amelyek megfelelnek a sok dokumentumot, vagy ha faceting kell adnia az megszámolja fölé nagyszámú dokumentum áll.  Mivel nincs szükség pontszám az alkalmazhatóságát a dokumentumok vagy a dokumentumok a számokat tartalmazó cellákat számlálnia kiszámítása sok, további partíciót hozzáadása segítséget nyújt további kiszámítása.  

   A szabványos keresési szolgáltatás 12 partíciót legfeljebb és 1 partíciót az egyszerű keresési szolgáltatás lehet.  Partíciót módosítható az [Azure portál](search-create-service-portal.md) vagy az [Azure Keresés kezelése API](search-get-started-management-api.md)segítségével.

2. **Magas hivatkozás mezők korlátozni:** Egy nagy hivatkozás mezőt egy facetable vagy szűrhető mezőre, amely egyedi értékek jelentős szám, sok erőforrást eredmények kiszámítására fölé eredményeként megnyitja áll.   Például facetable szűrhető terméket azonosító vagy leírás mező beállítása tenné magas hivatkozás esetében, mert a dokumentumot a dokumentum értékek közül a legtöbb egyediek. Lehetőség szerint a magas hivatkozás mezők számának korlátozása.

3. **Keresési réteg növelése:**  Legfeljebb áthelyezése egy újabb Azure keresési réteg lehet úgy a lekérdezések lassú a teljesítmény javítása érdekében.  Minden újabb réteg a gyorsabb Processzor- és több memóriát, amely pozitív hatással lehet a lekérdezési teljesítmény is tartalmaz.

## <a name="scaling-for-availability"></a>Az elérhetőség méretezése

Kópiák nemcsak lekérdezési időtartam csökkentése érdekében, de lehetővé teszi a magas elérhetőség.  Egyetlen replikáról a szoftverfrissítések után, illetve más karbantartási eseményeket, amely akkor fordul elő, a várt kell periodikus legrövidebb leállás kiszolgáló újraindul miatt.  Fontos eredményt adja, fontolja meg, ha az alkalmazáshoz szükséges keresések (lekérdezést) magas rendelkezésre állásának és írások (indexelési esemény).  Azure keresési kínál az alábbi attribútumokkal rendelkező fizetett keresési ajánlataiban SLA beállításai:

- a csak olvasható munkaterhelésekből (lekérdezést) magas elérhetősége 2 kópiák
- 3 vagy több kópiák az írási és olvasási munkaterhelésekből (lekérdezések és indexelés) magas elérhetősége

További információt a kérjük, keresse fel az [Azure keresési szolgáltatás szint szerződés](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

Mivel kópiák az adatok másolatait, több kópiák lehetővé, hogy a számítógép újraindítása és karbantartás több kópia ellen miközben továbbra is a többi replikához futtatható lekérdezések egyszerre Azure keresés.  Éppen ezért is szüksége lesz figyelembe, hogy milyen hatással van a legrövidebb leállás előfordulhat, hogy most már az adatokat egy kisebb példányát futtatható lekérdezéseket.

## <a name="scaling-geo-distributed-workloads-and-provide-geo-redundancy"></a>Méretezés geo elosztott munkaterhelésekből és geo redundancia

A geo elosztott munkaterhelésekből találja, hogy a felhasználók távol az adatközpont, ahol az Azure keresési szolgáltatás üzemelteti, amely magasabb késés díjak lesz.  Ezért érdemes gyakran fontos, hogy több keresési szolgáltatás, amely közelebb közelében ezeket a felhasználókat a régióban.  Azure keresési jelenleg nem biztosít egy automatikus módszerrel geo replikálása Azure keresési indexek különböző régiók, de néhány módszer használható fel, hogy ezt a folyamatot egyszerű végrehajtása, illetve kezelhet. Ezek a következő néhány szakaszokban mutatja.

A keresési szolgáltatás geo elosztott halmazának célja, hogy van elérhető két vagy több indexek két vagy több régióban, ahol a felhasználó átirányítja az Azure keresési szolgáltatás, ahol a legalacsonyabb időtartam, ebben a példában látható módon:

   ![Régió szerint szolgáltatás Keresztté lapja][1]

### <a name="keeping-data-in-sync-across-multiple-azure-search-services"></a>Adatok szinkronizált megőrzési több Azure Search szolgáltatásban

Kétféleképpen lehet megőrzési a elosztott keresés szolgáltatások szinkronizálja az [Azure keresési indexelő](search-indexer-overview.md) vagy a leküldéses API (más néven az [Azure keresési REST API -val](https://msdn.microsoft.com/library/dn798935.aspx)) segítségével olyan áll.  

### <a name="azure-search-indexers"></a>Azure keresési indexelő

Az Azure keresési indexelő használja, ha már importál adatmódosítás az Azure SQL-adatbázis vagy a DocumentDB például egy központi adattárhoz. Amikor létrehoz egy új keresés szolgáltatást, akkor egyszerűen is létrehozhat egy új Azure keresési indexelő szolgáltatás, amely a azonos adattárhoz mutat. Ebben az esetben, amikor új változások történnek az adatok tárolóba azok fog majd indexelni a különböző indexelő.  

Íme egy példa milyen lenne ki az architektúra.

   ![Elosztott indexelő és szolgáltatás kombinációk adatforrással][2]


### <a name="push-api"></a>Leküldéses API 
Az Azure keresési leküldéses API [frissíti a Azure keresési index](https://msdn.microsoft.com/library/dn798930.aspx)használatakor tárolhatja a különböző keresési szolgáltatások szinkronizálja közvetítheti módosításokat a Keresés az összes szolgáltatáshoz, amikor frissítésre szükség.  Amikor ezzel fontos, hogy miként kezelje a esetekben, amikor egy keresési szolgáltatás frissítésére sikertelen, és egy vagy több frissítés sikeres.

## <a name="leveraging-azure-traffic-manager"></a>Azure forgalom Manager feljebb helyezése

[Azure forgalom Manager](../traffic-manager/traffic-manager-overview.md) kérelmek több geo található webhelyeket, majd készül több Azure keresési szolgáltatás lehetővé teszi.  Egy, a forgalom Manager előnye, hogy akkor is probe Azure keresési győződjön meg róla, hogy ez elérhető, és a felhasználók átirányítása az alternatív keresési szolgáltatás megelőzve legrövidebb leállás.  Ezeken kívül keresési kérelem keresztül az Azure webhelyek vannak továbbításában szerepet Azure forgalom Manager lehetővé betöltését egyenleg esetben, ha a webhely mentése, de nem az Azure-keresés.  Íme egy példa milyen architektúrája, amelyek a forgalom Manager használja.

   ![Idegen – lap szolgáltatások régió, a központi forgalom-kezelővel][3]

## <a name="monitoring-performance"></a>A teljesítmény figyelése

Azure keresési elemzése és [Keresési forgalom Analytics (STA)](search-traffic-analytics.md)– a szolgáltatás a teljesítmény figyelését kínál. Keresztül STA tetszés szerint jelentkezhet az egyéni keresési műveletek, valamint a összesített mértékek Azure tárterület-fiókjába, majd elemzéshez feldolgozott vagy a Power BI formájában jelenik meg.  STA mértékek használata esetén áttekintheti Teljesítménystatisztikák, például a lekérdezések vagy lekérdezés válaszidő átlagos száma.  Ezenkívül a művelet naplózás lehetővé teszi speciális keresési műveletek részleteit lehatolhatnak.

STA egy értékes eszköz, az adott Azure keresés szempontjából késés díjak megértéséhez.  A lekérdezés teljesítménymutatók bejelentkezett alapuló lekérdezés (az azt küldésekor szükséges időt) Azure keresés teljesen feldolgoztatni idejét, mivel tudunk ezzel a paranccsal annak eldöntésében, késés problémák Azure keresési szolgáltatás oldaláról vagy kívül a szolgáltatás, például a hálózati késés.  

## <a name="next-steps"></a>Következő lépések

Ha többet szeretne tudni a árak rétegek és szolgáltatások korlátozások, egyesével, című cikkben találhat [szolgáltatás Azure keresés](search-limits-quotas-capacity.md).

Látogasson el a [kapacitás tervezés](search-capacity-planning.md) bővebb információkat partíciót és replika kombinációk.

További részletezés teljesítményre gyakorolt, és bemutatja, hogyan tudnak megvalósítani a jelen cikkben tárgyalt optimalizálásokat néhány bemutatókat nézhet nézze meg a következő videót:

> [AZURE.VIDEO azurecon-2015-azure-search-best-practices-for-web-and-mobile-applications]

<!--Image references-->
[1]: ./media/search-performance-optimization/geo-redundancy.png
[2]: ./media/search-performance-optimization/scale-indexers.png
[3]: ./media/search-performance-optimization/geo-search-traffic-mgr.png