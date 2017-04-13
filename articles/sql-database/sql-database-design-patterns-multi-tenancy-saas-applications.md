<properties
   pageTitle="Multitenant szoftver-alkalmazások és Azure SQL-adatbázis mintázatok tervezése |} Microsoft Azure"
   description="Ez a cikk azt ismerteti, hogy a követelmények és az általános adatok architektúra mintázatok multitenant adatbázis felhőalapú környezetben futtatott alkalmazások figyelembe kell vennie és ezek a minták társított különböző kompromisszumok. Azt is elmagyarázza, hogy Azure SQL-adatbázis rugalmas eszközök és rugalmas készletek ezeknek a követelményeknek, nincs – nem biztonságos módon a cím."
   keywords=""
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="sqldb-design"
   ms.date="08/24/2016"
   ms.author="carlrab"/>

# <a name="design-patterns-for-multitenant-saas-applications-and-azure-sql-database"></a>Multitenant szoftver-alkalmazások és Azure SQL-adatbázis mintázatok tervezése

Ebben a cikkben talál kapcsolatos követelményeket és a hasonló adatokat architektúra mintázatok multitenant szoftver, a (szoftver) adatbázis szolgáltatásalkalmazások felhőalapú környezetben futtatott. Azt is bemutatja az tényezőket kell vennie és különböző tervezési mintázatok kompromisszumok. Rugalmas készletek és Azure SQL-adatbázisban rugalmas eszközök segíthetnek igényeknek egyéb célok veszélyeztesse.

A fejlesztők néha döntéseket azok az adatok rétegek multitenant alkalmazások bérleti modellekkel tervezésekor szemben a hosszú távú legjobb érdeklődési használható. Első lépésként legalább egy fejlesztő előfordulhat, hogy perceive, fejlesztés és alsó felhőalapú szolgáltatás szolgáltató költségeket, mint a fontosabb, mint a bérlői elkülönítési Kezeléstechnikai vagy az alkalmazás a méretezhetőség. Ez a beállítás később felhasználói elégedettséget aggályokat és költséges tanfolyam korrekciós vezethet.

Egy multitenant alkalmazást egy olyan alkalmazás, felhőalapú környezetben is, és ugyanazokat a szolgáltatásokat száz vagy bérlők, akik nem megosztása, vagy másik fél adatokat ezer biztosítja. Példa egy olyan szoftver alkalmazás, a felhőben tárolt környezet bérlőkhöz szolgáltatásokat nyújtó.

## <a name="multitenant-applications"></a>Multitenant alkalmazások

Multitenant alkalmazások, az adatok és a terhelést lehet egyszerűen particionálni. Szintén szétválaszthatók adatok és a terhelést, például mentén bérlői határai, mert a legtöbb kérelmek egy bérlői határain belül fordul elő. Ez a tulajdonság az adatok és a terhelést a járó, és részesítése a az alkalmazás mintázatok, a jelen cikkben tárgyalt szemben.

A fejlesztők felhőalapú alkalmazásairól – többek között a teljes dokumentumhasználat ilyen típusú alkalmazás használata:

- Partner adatbázis-alkalmazásokat, hogy a program éppen-re áttelepített a felhőbe szoftver alkalmazásként
- Az alapoktól kezdve a felhőbe épített szoftver-alkalmazások
- Közvetlen, ügyfélkapcsolati alkalmazások
- Alkalmazotti elérésű vállalati alkalmazások

Az a felhő és gyök rendelkezők tervezett szoftver alkalmazásokat, a partner általában az adatbázis-alkalmazások multitenant alkalmazás is. Ezeket az alkalmazásokat szoftver egy speciális szoftveralkalmazás szolgáltatásként kézbesítése a bérlők. Bérlők az alkalmazás szolgáltatás érhetik el és van társítva, az alkalmazás részét képező tárolt adatok teljes tulajdonjogát. De szoftver előnyei kihasználhatja a bérlők lemondást kell néhány szabályozható saját adataikat. A szoftver szolgáltatót, hogy a adataik biztonságos és csatlakoztathatók más bérlők adatok megbízható őket. Ez a fajta multitenant szoftver alkalmazás példák MYOB SnelStart és Salesforce.com-hoz. Ezeket az alkalmazásokat, mindegyik kell partícionálni bérlői határai és az alkalmazás tervezése a mintázatok, a jelen cikkben ölelik támogatási mentén.

Ügyfelek számára nyújtott közvetlen szolgáltatáshoz, illetve alkalmazottakra (gyakran néven bérlők helyett a felhasználók) a szervezeten belül a multitenant alkalmazás dokumentumhasználat másik kategóriához alkalmazásokat. Ügyfelek fizessen elő a szolgáltatás, és nem rendelkezik az a szolgáltató által gyűjtött, és tárolja az adatokat. Szolgáltatók túl kormányzati utasított adatvédelmi előírásokat egymástól elszigetelt ügyfeleik adatok megtartása kevésbé szigorú követelményeik vannak. Ez a fajta ügyfélkapcsolati multitenant alkalmazás példák multimédiás tartalom szolgáltatók például Netflix, illetve a Spotify és Xbox LIVE. További példákat könnyen particionálható alkalmazások ügyfélkapcsolati, Internet-skála alkalmazások, és dolog Internet (IoT) mely egyes ügyfelek vagy eszközön lehet partíciót lesz. Partition határai is elválaszthatja egymástól a felhasználó és eszköz számára.

Nem minden alkalmazások partíciót egyszerűen egy-egy tulajdonság, például a bérlői, az ügyfél, a felhasználó vagy a eszköz mentén. Egy összetett vállalatirányítási (ERP)-alkalmazást, például a termékek, megrendelések és ügyfelek tartalmaz. Általában egy összetett séma erősen egymáshoz kapcsolódó táblákat ezer rendelkezik.

Nincs olyan partíciót stratégia is alkalmazása az összes tábla, és az alkalmazások terhelést belül tevékenységekhez. Ez a cikk multitenant alkalmazásokat, amelyekre egyszerűen particionálható adatok és a feladatok koncentrál.

## <a name="multitenant-application-design-trade-offs"></a>A kompromisszumok Tervező multitenant alkalmazás

A tervezés minta, amely egy multitenant alkalmazásfejlesztő úgy dönt, általában az alábbi tényezőket vizsgálata alapján történik:

-   **Bérlői elkülönítési**. A Fejlesztőeszközök kell gondoskodjon arról, hogy nincs bérlői nem kívánt, más bérlők adatokhoz való hozzáférés. Más tulajdonságait, például a zajos szomszédok védelme is visszaállíthatja egy bérlői adatokat és bérlői-specifikus testreszabásokat elkülönítési kötelező kiterjed.
-   **Felhőalapú erőforrásköltség**. Egy szoftver alkalmazást kell lennie a költség versenytársak. Egy multitenant alkalmazásfejlesztő előfordulhat, hogy dönt, hogy az a felhő erőforrások, például tárhely használatának alsó költség optimalizálása, és költségek számítja ki.
-   **Kezeléstechnikai DevOps**. Egy multitenant alkalmazásfejlesztő elkülönítési védelem beépítése és, és a Lync-állapota az alkalmazás és az adatbázis sémája, valamint bérlői kapcsolatos problémák megoldása kell. Az alkalmazások fejlesztése, és a művelet összetettsége közvetlenül megnövelt költség megfelelője, és lekérdezi a bérlői elégedettséget.
-   **Méretezhetőség**. Szúrhatók adásának a további bérlők és bérlők, akiknek nincs szükségük kapacitással, a szoftver sikeres művelet feltétlenül szükséges.

Ezek a tényezők mindegyikének kompromisszumok egy másik képest. A legalacsonyabb költségét a felhőben, előfordulhat, hogy nem felületet kínálják, az legkényelmesebb fejlesztési kínáló. Fontos a fejlesztő döntéseket tájékoztatni ezeket a beállításokat, és a kompromisszumok az alkalmazás tervezési folyamat során.

Népszerű fejlesztési mintát is több bérlők csomag néhány adatbázisba. Az eljárás előnye a alacsonyabb költséget, mert a néhány, és az adatbázisok korlátozott számú való használatáról a relatív egyszerűség fizet. De az idő múlásával szoftver multitenant alkalmazásfejlesztő fog figyelmen kívül hagyja, hogy ez a beállítás van-e a bérlői elkülönítési és méretezhetőség jelentős downsides. Ha bérlői elkülönítési fontos, további erőfeszítéssel megosztott tárolási bérlői adatok védelme ezzel az illetéktelen hozzáférést vagy zajos szomszédok. A további munkamennyiség jelentősen növeli a előfordulhat, hogy a fejlesztéshez és elkülönítési a karbantartási költségek. Hasonlóképpen bérlők hozzáadása szükség, ha a tervezés mintát általában szükséges szakértelemmel bérlői adatok újraterjeszteni az adatbázisok megfelelően méretezni a adatok réteg-alkalmazás között.  

Bérlői elkülönítési gyakran alapvető követelmények szoftver multitenant alkalmazások gondoskodjon arról, hogy vállalkozások és szervezetek között. A fejlesztő által észlelt előnye az egyszerűség és a költség bérlői elkülönítési és méretezhetőség: Előfordulhat, hogy kell tempted. Ez a csökkentés is szemléltetéséhez összetett és drága a szolgáltatás megnő, és a bérlői elkülönítési követelmények válnak fontos, és az alkalmazás rétegben felügyelt. Az ügyfelek számára nyújtott közvetlen, fogyasztói elérésű szolgáltatáshoz multitenant alkalmazások, azonban bérlői elkülönítési lehet felhő erőforrásköltség optimalizálás alacsonyabb prioritású.

## <a name="multitenant-data-models"></a>Multitenant adatmodellek

Általános tervezési eljárások úgy, hogy a bérlői adatok hajtsa végre az három különálló modell, az 1.


  ![Adatmodellek multitenant alkalmazás](./media/sql-database-design-patterns-multi-tenancy-saas-applications/sql-database-multi-tenant-data-models.png)
 ábra 1: multitenant adatmodellek közös tervezés tanácsok

-   **Adatbázis-használati bérlői**. Minden egyes bérlői van a saját adatbázis. Összes bérlői adatokat a bérlő adatbázis korlátozódik, és a többi bérlők csoportja és adataik elszigetelt.
-   **A megosztott adatbázis sharded**. Több bérlők megosztása több adatbázisok valamelyikével. Eltérők halmazának bérlők adatbázisonként például kivonat, a tartomány vagy a lista szétválasztás particionáló stratégia használatával van rendelve. Ez adatokat a terjesztési stratégia gyakran nevezik sharding.
-   **A megosztott adatbázis-egyetlen**. Egyetlen néha nagy adatbázisban vannak használatát a bérlői azonosító oszlop összes bérlőkhöz adatait tartalmazza.

> [AZURE.NOTE] Előfordulhat, hogy az alkalmazás fejlesztője válassza a különböző bérlők elhelyezése másik adatbázist a sémák, és a séma neve segítségével a különböző bérlők értelmezni. Ezt a megközelítést nem azt javasoljuk, mert a dinamikus SQL használata általában szükséges, és nem lehet hatékony a gyorsítótár-csomagot. Ez a cikk további részében akkor kiemelése a megosztott táblázat megközelítés multitenant alkalmazás kategóriára.

## <a name="popular-multitenant-data-models"></a>Népszerű multitenant adatmodellek

Fontos, az alkalmazás tervezés kompromisszumok azt már azonosítva értelmez multitenant adatmodellek különböző típusú ki szeretné számítani. Ezek a tényezők súgó szabadságfokának a három leggyakoribb multitenant adatmodellek korábbi leírt és az adatbázis használatát 2 ábrán látható módon.

-   **Elkülönítési**. A bérlők közötti elkülönítési fokát egy mértéket mennyi bérlői elkülönítési adatmodell éri el is lehet.
-   **Felhőalapú erőforrásköltség**. Az összeg, az erőforrás-megosztás bérlők között tudja optimalizálni a felhőben erőforrásköltség. Erőforrás a számítási és tárolási költség definiálható.
-   **DevOps költség**. Alkalmazások fejlesztése, telepítés és kezelhetőséget Kezeléstechnikai csökkenti a teljes költsége a szoftver művelet.  

A 2 az Y tengely bérlői elkülönítési szintjét mutatja be. Az X tengely erőforrás-megosztás szintjét mutatja be. A szürke, a középső átlós nyíl irányát DevOps költségek, általános szabályok növeléséhez vagy csökkentéséhez.

![Népszerű multitenant alkalmazás tervezés mintázatok](./media/sql-database-design-patterns-multi-tenancy-saas-applications/sql-database-popular-application-patterns.png) szám 2: népszerű multitenant adatmodellek

A jobb alsó részre osztott a 2-alkalmazás mintát használó a potenciálisan nagy, jeleníti meg egy megosztott adatbázishoz, és a megosztott táblát (vagy külön séma) megközelítés. Célszerű az erőforrás-megosztás, mert az összes bérlők ugyanazokat az adatbázis erőforrásokat (Processzor, a memóriahasználat, a bemeneti és kimeneti.) egyetlen adatbázisban. De bérlői elkülönítési korlátozott. Szükség lehet a bérlők védelme egymástól az alkalmazási réteg, további lépéseket. További lépések jelentősen növelheti a DevOps költség fejlesztése, és a kezelése. Méretezhetőség a hardver az adatbázist tároló beosztásának korlátozza.

A bal alsó részre osztott a 2 több bérlők sharded szemlélteti több adatbázisok között (általában különböző hardveres méretezni egységek). Minden egyes adatbázis-bérlőkhöz egy részét, amely megszünteti a méretezhetőség kérdés más mintázatok tárolja. További kapacitás szükség a bérlők további, ha egyszerűen helyezhetők el a bérlők új hardver skála egységek rendelt új adatbázisok. Erőforrás-megosztás mennyiségét azonban csökken. Csak a bérlők ugyanazt a méretezés mértékegységet az erőforrások megosztása helyezett el. Ezt a megközelítést ritkán továbbfejlesztésére bérlői elkülönítési, mert sok bérlők továbbra is collocated anélkül, hogy automatikusan védeni egymás műveletek biztosít. Alkalmazás összetettsége magas marad.

A bal felső részre osztott a 2, a harmadik megközelítés. A saját adatbázis minden bérlői adatok helyezi. Ezt a megközelítést jó bérlői leválasztó tulajdonságok rendelkezik, de korlátozza az erőforrások megosztása, amikor adatbázisonként van a saját dedikált erőforrásait. Ez a módszer akkor hasznos, ha minden bérlők kiszámíthatóan munkaterhelésekből van. Bérlői munkaterhelésekből váló kisebb kiszámítható, a szolgáltató nem tudja optimalizálni a erőforrások megosztása. Gyakori szoftver alkalmazások által fut. A szolgáltató igényeinek kielégítéséhez túlzott rendelkezést vagy az alsó erőforrásokat kell. Magasabb költségek vagy az alsó bérlői elégedettséget vagy művelet eredményezi. Erőforrás megosztását bérlők között magasabb fokú válik, hogy a megoldás költséghatékonyabb célszerű. DevOps költség üzembe helyezését és az alkalmazás karbantartását is adatbázisok számának növelése nő. Ellentétben ezeket a problémákat ezt a módszert nyújt a legjobb és legegyszerűbb elkülönítve bérlők.

Ezek a tényezők is befolyásolja a tervezés minta egy ügyfél:

-   **Bérlői adatok tulajdonjogát**. Egy alkalmazást, amelyben a bérlők megőrzése a saját adatok tulajdonjoga bérlőnként egy adatbázist, a mintázat részesítése szemben.
-   **Méretezés**. Az alkalmazások ezres vagy bérlők milliónyi több száz függvénynél adatbázis megosztása sharding például az alábbi módszerek részesítése szemben. Elkülönítési követelmények továbbra is jelenthet kihívásokkal kapcsolatban.
-   **Érték és üzleti modell**. Ha egy alkalmazás bérlői-bevétel per Ha kis (kisebb, mint dollár), a válik a kevésbé fontos elkülönítési követelményeket és a megosztott adatbázis van ilyesmire lehetőség. Ha a bérlői bevétel néhány dollárban vagy egyéb, egy adatbázis-használati bérlői modell több lehetséges. Érdemes fejlesztési költségek csökkentése.

A tervezés kompromisszumok a 2 adni, egy ideális multitenant modellnek kell jó bérlői elkülönítési tulajdonságok beépítése az optimális erőforráshoz megosztása bérlők között. A kategória, a jobb felső sarkában részre osztott szám 2 ismertetett elférjen a modellt.

## <a name="multitenancy-support-in-azure-sql-database"></a>Azure SQL-adatbázisban multitenancy támogatás

Azure SQL-adatbázis támogatja a 2 tagolt mintázatokat multitenant alkalmazást. Rugalmas készletek egy jó erőforrás-megosztás kombináló alkalmazás mintát is támogat, és az adatbázis-használati-bérlői elkülönítési előnyei megközelíthető (lásd az ábrát 3 a jobb felső sarkában részre osztott). Rugalmas Adatbáziseszközök és funkciók az SQL-adatbázis csökkentésére a költség, valamint működtetéséhez egy alkalmazást, amelynek sok adatbázis (a 3 árnyékolt területén látható). Ezek az eszközök segíthetnek létrehozása és kezelése, így a több elem adatbázis mintázatok közül.

![Azure SQL-adatbázisban mintázatok](./media/sql-database-design-patterns-multi-tenancy-saas-applications/sql-database-patterns-sqldb.png) ábra 3: Multitenant alkalmazás mintázatok Azure SQL-adatbázisban

## <a name="database-per-tenant-model-with-elastic-pools-and-tools"></a>Adatbázis-használati bérlői modell rugalmas készletek és eszközökkel

SQL-adatbázisban rugalmas készletek bérlői elkülönítési kombinálhatják hatékonyabb támogatása bérlői adatbázisok között megosztása az adatbázis-használati bérlői megközelítés erőforrás. SQL-adatbázis használata az adatok réteg megoldás, akik multitenant alkalmazások szoftver szolgáltatók. Az erőforrás-megosztás bérlők között terhet az adatbázis-szolgáltatás réteg értékeként a az alkalmazási réteg. Rugalmas feladatok, rugalmas lekérdezés, rugalmas tranzakciók és a rugalmas adatbázis ügyfél tár egyszerűsített kezelése, és különböző adatbázisok skála a lekérdezés komplexitását.

| Alkalmazás vonatkozó követelmények | SQL-adatbázis funkciók |
| ------------------------ | ------------------------- |
| Bérlői elkülönítési és erőforrás-megosztás | [Rugalmas készletek](sql-database-elastic-pool.md): rendelése SQL-adatbázis erőforráskészlethez tartozik, és az erőforrások megosztása a különböző adatbázisok között. Egyes adatbázisokat is rajzolhat annyi erőforrások a készletből, a bérlői munkaterhelésekből módosításainak köszönhetően kapacitás igény szerinti kiugrásainak megfelelő igazodik szükség szerint. Szükség szerint a rugalmas készlet magát felfelé vagy lefelé kell átméretezi. Rugalmas készletek is biztosít a könnyű kezelhetőség, és figyelemmel kísérésére és a hibaelhárítás a készlet szintjén. |
| DevOps Kezeléstechnikai adatbázisok között | [Rugalmas készletek](sql-database-elastic-pool.md): korábbi leírt módon.|
||[Rugalmas lekérdezés](sql-database-elastic-query-horizontal-partitioning.md): lekérdezési jelentéskészítéshez adatbázisok közötti határokon bérlői webhelyen elemzés.|
||[Rugalmas feladatok](sql-database-elastic-jobs-overview.md): csomag és az megbízható telepítése több adatbázisok adatbázis-karbantartási műveletek vagy az adatbázis sémája módosításokat.|
||[Rugalmas tranzakcióinak](sql-database-elastic-transactions-overview.md): folyamat több adatbázisok atomi és elkülöníteni módon változik. Rugalmas tranzakciók van szükség, ha alkalmazások felett több adatbázis-műveletek "mind vagy semmi" garanciákkal van szükségük. |
||[Rugalmas adatbázis ügyfél tár](sql-database-elastic-database-client-library.md): adatok terjesztését kezelése és a bérlők megfeleltetése adatbázisok. |

## <a name="shared-models"></a>Megosztott modellek

Ismertetett módon, a legtöbb szoftver szolgáltatók, a megosztott modell megközelítés jelenthet a bérlői elkülönítési problémákkal kapcsolatos problémák és az alkalmazások fejlesztése és karbantartás bonyodalmainak. Jó helyen jár adja meg a szolgáltatás közvetlenül az fogyasztói multitenant alkalmazások, bérlői elkülönítési követelmények nem lehet magas, a prioritás szerint kis méretre költség. Előfordulhat, hogy tudnak bérlők csomag egy vagy több adatbázisokban a magas sűrűség költségek csökkentése érdekében. A megosztott adatbázis modellek egy adatbázist, vagy több sharded adatbázisok használata az erőforrás-megosztás és a teljes költség további hatékonyságot eredményezhet. Azure SQL-adatbázis itt bizonyos szolgáltatások, amely segít a felhasználóknak a nagyobb biztonság és az adatok réteg skála management elkülönítési összeállítása.

| Alkalmazás vonatkozó követelmények | SQL-adatbázis funkciók |
| ------------------------ | ------------------------- |
| Biztonsági elkülönítési szolgáltatásai | [Sor szintű biztonság](https://msdn.microsoft.com/library/dn765131.aspx) |
|| [Adatbázis sémája](https://msdn.microsoft.com/library/dd207005.aspx) |
| DevOps Kezeléstechnikai adatbázisok között | [Rugalmas lekérdezés](sql-database-elastic-query-horizontal-partitioning.md) |
|| [Rugalmas feladatok](sql-database-elastic-jobs-overview.md) |
|| [Rugalmas tranzakciók](sql-database-elastic-transactions-overview.md) |
|| [Rugalmas adatbázis ügyfél-tárban](sql-database-elastic-database-client-library.md) |
|| [Rugalmas adatbázis felosztása és egyesítése](sql-database-elastic-scale-overview-split-and-merge.md) |

## <a name="summary"></a>Összefoglalás

A legtöbb szoftver multitenant alkalmazások fontosak a bérlői elkülönítési követelményeknek. A legjobb lehetőséget, ha elkülönítési erősen felé az adatbázist a bérlő használati megközelítés leans. Két megközelítés összetett alkalmazás rétegek képzett fejlesztési oktatói elkülönítési, amelyek jelentősen növeli a költség és a kockázat megadására igénylő befektetések szükség. Elkülönítési követelmények számításból korai a szolgáltatás fejlesztését, ha az első két modellekben még jobban költséges lehet átalakítások őket. A fő hátrányai az adatbázist a bérlő használati modell társított kapcsolódó megnövelt felhő erőforrásköltségek miatt csökkentett megosztása és fenntartása sok adatbázisok kezelésében. Szoftver alkalmazásfejlesztő gyakran elvégezzen őket, hogy a következő kompromisszumok.

Bár kompromisszumok lehet, hogy a legtöbb felhő adatbázis szolgáltatókkal fő korlátok, a Azure SQL-adatbázis megszünteti a korlátok rugalmas adatbázis funkciók és rugalmas készlet. Szoftver fejlesztők egyesítheti egy adatbázis-használati bérlői modell elkülönítési jellemzői és erőforrás-megosztás és kezelhetőség fejlődött Ez számos adatbázisok optimalizálása rugalmas készletek és a kapcsolódó eszközök segítségével.

Multitenant alkalmazás szolgáltatók bérlői elkülönítési követelmények nem rendelkező, és ki lehet egy nagy sűrűségfüggvény eredménye az adatbázisban bérlők csomag Észreveheti, hogy megosztott adatok modellek eredmény az erőforrás-megosztás további hatékonyság, és teljes költsége csökkentése. Azure SQL-adatbázis rugalmas Adatbáziseszközök, sharding tárak és biztonsági funkciók szoftver szolgáltatók létrehozása és kezelése a multitenant alkalmazások súgója

## <a name="next-steps"></a>Következő lépések

[Első lépések a rugalmas Adatbáziseszközök](sql-database-elastic-scale-get-started.md) egy minta alkalmazás, amely bemutatja az ügyfél-tár.

Hozzon létre egy [egyéni irányítópulttá rugalmas készlet szoftver](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools-custom-dashboard) költséghatékony, méretezhető adatbázis-megoldás rugalmas készletek használó minta alkalmazással.

[Ha át kívánja méretezni létező adatbázisokat](sql-database-elastic-convert-to-use-elastic-tools.md)az áttelepítendő Azure SQL-adatbázis eszközeivel.

[Létrehozhat egy rugalmas](sql-database-elastic-pool-create-portal.md)meg meg.  

Megtudhatja, hogyan [figyelésére](sql-database-elastic-pool-manage-portal.md)és kezelése egy rugalmas készlet.

## <a name="additional-resources"></a>További források

- [Mi az az Azure rugalmas készlet?](sql-database-elastic-pool.md)
- [Méretezés kifelé az Azure SQL-adatbázis](sql-database-elastic-scale-introduction.md)
- [Rugalmas Adatbáziseszközök és sor szintű biztonsági multitenant alkalmazások](sql-database-elastic-tools-multi-tenant-row-level-security.md)
- [Az Azure Active Directory és a csatlakozás OpenID multitenant alkalmazások hitelesítés](../guidance/guidance-multitenant-identity-authenticate.md)
- [Dejójáték Kft felmérések alkalmazás](../guidance/guidance-multitenant-identity-tailspin.md)
- [Gyors megoldást indítása](sql-database-solution-quick-starts.md)

## <a name="questions-and-feature-requests"></a>Kérdések és frissítéseiről

A kérdések keresse meg az us [SQL-adatbázis fórumán](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted). Adja hozzá a szolgáltatás kérelmének [SQL-adatbázis Visszajelzési fórum](https://feedback.azure.com/forums/217321-sql-database/).
