<properties
   pageTitle="Mi az Azure SQL-adatraktár? | Microsoft Azure"
   description="Nagyvállalati adatbázis alkalmas petabyte mennyiségű relációs és nem relációs adatok feldolgozásának meghatározva. Az üzleti első felhő adatraktár a megnövesztés, a kisebb, és mutasson az egérrel a másodpercben megadott."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/27/2016"
   ms.author="lodipalm;barbkess;mausher;jrj;sonyama;kevin"/>


# <a name="what-is-azure-sql-data-warehouse"></a>Mi az Azure SQL-adatraktár?

Azure SQL-adatraktár alkalmas nagy mennyiségű relációs és a nem relációs adatok feldolgozásának felhőalapú, méretezési adatbázis. A nagymértékben párhuzamos feldolgozási (MPP) architektúrára épül, SQL adatraktár kezelhető a vállalati terhelést.

SQL-adatraktár:

- Az SQL Server relációs adatbázis Azure felhő méretezési lehetőségek egyesíti. Növelése, csökkentése, mutasson az egérrel, vagy számítási folytathatja a másodpercben megadott. Költségek takaríthat méretezés Processzor kifelé, szükség esetén, és vissza használatát nem csúcs időszakokban vágás.
- Az Azure platform használja. Azt egyszerűen üzembe zökkenőmentes tartani és teljesen hiba miatt automatikus vissza – a jelen a alternatív.
- Az SQL Server ökológiai kiegészíti. A jól ismert SQL Server Transact-SQL nyelvben (T-SQL), és eszközökkel fejleszthet.

Ez a cikk ismerteti a fontosabb funkciói SQL adatraktár.

## <a name="massively-parallel-processing-architecture"></a>Nagymértékben párhuzamos feldolgozási architektúra

SQL-adatraktár egy nagymértékben párhuzamos feldolgozási (MPP) adatbázis rendszer meghatározva. Adatok osztani és videofunkcióinak feldolgozása több csomópontok keresztül, SQL adatraktár is kínálhatnak, nagyon nagy méretezhetőség - bármely egyetlen rendszer túl távol.  Bepillantás a színfalak mögé SQL adatraktár oldalpárokat az adatok sok megosztott semmi tárolási és egység Az adatok prémium helyileg felesleges tárolója tárolja, és kapcsolódó lekérdezés-végrehajtási csomópontok számítja ki. E architektúra SQL adatraktár megnyitja a "osztása és meghódítása" megközelítés terhelések és komplex lekérdezések futtatásához. Kérések megkapta a vezérlő csomópontot, optimalizált és a számítási csomópontok párhuzamosan munkájuk elvégzéséhez majd átadott.

MPP-architektúra és Azure tárolási lehetőségeket kombinálásával SQL adatraktár végezheti el:

- A nagyobb vagy kisebb számítási független tároló.
- A nagyobb vagy kisebb számítási adatok áthelyezése nélkül.
- Szünet kapacitás miközben adatok sértetlen számítja ki.
- Önéletrajz egy kicsit értesítés minőségben számítja ki.

A következő ábrán a architektúra részletesebben.

![SQL-adatok raktári architektúra][1]


**Szabályozási csomópont:** A vezérlő csomópont kezeli, és lekérdezések optimalizálja. Az alkalmazások és a kapcsolatokkal szoftverekkel előtérrészét. Az SQL-adatraktár a vezérlő csomópont SQL-adatbázis úgy van-e kapcsolva, és csatlakozott formátumban és érzi azonos. A felület, csoportban a vezérlő csomópontot koordinálja minden adat mozgását és kiszámítása az elosztott adatokon párhuzamos lekérdezések futtatásához szükséges. Az SQL-T lekérdezés SQL adatraktár elküldésekor a vezérlő csomópontot átalakítások külön lekérdezések minden számítási csomóponton párhuzamosan futó be.

**Csomópontok kiszámítása:** A számítási csomópontok a power mögött SQL adatraktár szolgál. Elérik azt, hogy tárolja az adatokat, és a folyamat a lekérdezés SQL-adatbázisait. Adatok hozzáadásakor a SQL adatraktár elosztja a számítási csomópontok a sorok. A számítási csomópontok a dolgozók, amely a párhuzamos lekérdezések futtathat adatain. Feldolgozás után azok át az eredményeket vissza a vezérlő csomópontot. A lekérdezés befejezéséhez a vezérlő csomópontot összesíti az eredmények, és a végeredmény adja eredményül.

**Tároló:** Az adatok Azure Blob-tárolóhoz vannak tárolva. Amikor számítási csomópontok interaktív módon az adatokat, írja be, és olvassa el a közvetlenül és blob-tárolóból. Azure tároló kibővíti átlátszó és nagyban, mivel a SQL adatraktár azonos hajthat végre. Mivel a számítási és tárolási független, SQL adatraktár automatikusan méretezheti tároló elkülönítve méretezés számítási és fordítva. Azure Blob-tárolóhoz is teljesen alternatív hibafa, és a biztonsági mentési és visszaállítási folyamatok egyszerűsítik.

**Mozgását adatszolgáltatás:** Adatok mozgását szolgáltatás (Dokumentumkezelő) adatokat mozgatja a csomópontok között. Dokumentumkezelő szükségük van az illesztés és összesítések adatok a számítási csomópontok hozzáférést biztosít. Dokumentumkezelő nem egy Azure szolgáltatás. A Windows szolgáltatás, amely az SQL-adatbázis mellett a csomóponton. Dokumentumkezelő a háttérben fut, mert nem közvetlenül kapcsolatba vele. Jó helyen jár ha megnézi lekérdezés tervek, megfigyelheti néhány Dokumentumkezelő művelet tartoznak, mivel az adatok mozgását párhuzamosan mindkét lekérdezés futtatásához szükséges.


## <a name="optimized-for-data-warehouse-workloads"></a>Adatok raktári munkaterhelésekből optimalizálva

A MPP-megközelítés adattárolási adott teljesítmény optimalizálásokat, beleértve a körében van támogatott:

- Megosztott lekérdezés-optimalizáló és az összetett statisztikák át az összes adat. Adatok méretét és a terjesztési adatokat használ, a szolgáltatás nem tudja optimalizálni lekérdezések adott elosztott lekérdezési műveletek a költségére felmérése.

- Speciális algoritmusok és módszerek az adatok mozgását folyamat hatékony az adatok között, a lekérdezés végrehajtásához szükséges a források számítások áthelyezése integrálva. Ezek adatműveleteket mozgás kialakításának, és az adatok mozgását szolgáltatás összes optimalizálásokat automatikusan fordulhat elő.

- Csoportosított **columnstore** indexek alapértelmezés szerint. Oszlop-alapú tároló használatával a SQL adatraktár átlagosan 5 x tömörítés nyereség hagyományos sorirányú tároló fölé, valamint legfeljebb 10 x vagy több lekérdezési teljesítmény nyereség kap. Lekérdezések analitikájának szeretne képet beolvasni a sorok munka columnstore indexek a sok van szükség.


## <a name="predictable-and-scalable-performance"></a>Kiszámíthatóvá és méretezhető teljesítmény

SQL-adatraktár elválasztja a tárhely és a számítási, amely lehetővé teszi, hogy minden, ha át kívánja méretezni, egymástól függetlenül. SQL-adatraktár gyorsan és egyszerűen méretezheti további számítási erőforrások hozzáadása a egy kicsit értesítés. A kiegészítő Azure Blob-tárolóhoz használatát. BLOB a legalacsonyabb költség mellett egyszerű kiterjesztett nemcsak stabil, replikált tárhely, de a infrastruktúra is szükséges. Nagyméretű felhőalapú tárolási és Azure számítási kombinációval használ, a SQL adatraktár lehetővé teszi a lekérdezési teljesítmény és tárolására szolgáló fizet, szükség esetén. Számítási számának módosítása, egyszerűen balra vagy jobbra az Azure-portálon csúszka, vagy is ütemezhető legyen az SQL-T és a PowerShell használatával.

Együtt az azt jelenti, hogy teljesen szabályozhatják a számítási függetlenül tárhely SQL adatraktár teszi teljesen mutasson a adatraktár, ami azt jelenti, hogy nem fizet a számítási nincs szükség esetén. Helyen tartásával a tárhely, az összes számítási megjelenik az Azure-pénzt mentése, a fő készletbe van. Szükség esetén, egyszerűen folytathatja a számítási és nem az adatok és érhető el a terhelést kiszámítására.

## <a name="data-warehouse-units"></a>Adatok raktári mennyiség

Hozzárendelés az erőforrások az SQL adatraktár mértékegysége adatok raktári egységek (DWUs). DWUs az alapul szolgáló erőforrások, mint a Processzor, a memóriahasználat, IOPS, amely a SQL adatraktár felosztott történő változásának mértéke. DWUs számának növelése növekszik, erőforrások és a teljesítmény. Pontosabban DWUs biztosíthatja, hogy:

- Alkalmasak a adatraktár könnyen méretezhető, anélkül, hogy a mögöttes hardver vagy szoftver.

- A teljesítmény javítása DWU szintet is előrejelzésére, mielőtt a adatraktár méretének módosítása.

- Az alapul szolgáló hardver- és a példányok megváltoztathatja, vagy helyezze át a terhelést teljesítmény megtartásával.

- A Microsoft bármikor módosíthatja az alapul szolgáló architektúra a szolgáltatás a terhelést teljesítményének megtartásával.

- A Microsoft is gyorsan javítja a SQL adatraktár, oly módon, hogy méretezhető, és a rendszer egyenletesen effektusok.

Adatok raktári egységek nyújt, amelyek rendkívül közötti kapcsolatot a terhelést teljesítmény adattárolási három pontos mértékek történő változásának mértéke. A célja, hogy az alábbi főbb terhelést mértékek fog méretezése lineárisan az Ön által választott a adatraktár az DWUs.

**Beolvasás/összesítési:** A terhelést mérőszám megnyitja a lekérdezést, amely ellenőrzi a sok sort, és végrehajtja a egy összetett összesítési szabványos adattárolási. Ez az adatátviteli, valamint a Processzor időigényes művelet.

**Betöltés:** Ez a mérőszám azt méri, az azt jelenti, hogy adatokat ingest az üzembe. Az Azure Blob-tárolóhoz képviselő adatkészlet betöltése PolyBase terhelések végrehajtható. Ez a mérőszám terhelési hálózati és a szolgáltatás Processzor szempontok szolgál.

**Létrehozása a táblázat, jelölje be (CTAS):** CTAS azt méri, az azt jelenti, hogy másolja a táblázatot. Ez magában foglalja a adatok beolvasása tárhelyről, terjesztése meg a készülék csomópontok közötti, és újból írni tároló. A Processzor IO, és a hálózati ilyen művelet adatbázis.

## <a name="pause-and-scale-on-demand"></a>Szünet és igény skála

Gyorsabb eredmények van szüksége, a DWUs növelése és fizet, a nagyobb teljesítmény elérése érdekében. Ha szükséges kisebb számítja ki a power, csökkentheti a DWUs és fizetni csak akkor van szüksége. Lehet megfontolni a következő helyzetekben DWUs módosítása:

- Ha nem kell lekérdezések futtatása, talán a esti időszakok vagy hétvégék, leválasztása a lekérdezések. Majd mutasson a számítási erőforrások fizet a DWUs, nincs szükség szerint csoportokat elkerülése érdekében.

- Amikor a rendszer alacsony igény szerint, fontolja meg DWU csökkentése egy kis méretűre. Hozzá tud férni az adatokat, de egy jelentős költség megtakarítási.

- A nehéz adatok betöltésének vagy a átalakítási műveletet végrehajtásakor érdemes méretezni, hogy az adatok érhető el gyorsan.

Megtudhatja, hogy mi az a ideális DWU érték, próbálja meg méretezés felfelé és lefelé, és néhány lekérdezések futtatása az adatok betöltése után. Mivel a méretezés gyors, megpróbálhatja különböző szintjeit, teljesítmény egy óra vagy annál kisebb szám.  Tartsa szem előtt, hogy SQL adatraktár lett tervezve nagy mennyiségű adatot feldolgozása, illetve láthatja a méretezés igaz képességei, különösen a nagyobb értékekhez kínálunk, a fogja használni kívánt közelít, illetve meghaladja az 1 TB nagy mennyiségű adathalmaz.


## <a name="built-on-sql-server"></a>Az SQL Server-ra épülő

SQL-adatraktár az SQL Server relációs adatbázismotor alapul, és egy vállalati adatraktár a várt szolgáltatásainak nagy része tartalmaz. Ha már tudja az SQL-T, érdemes egyszerűen a Tudásbázis SQL adatraktár nem ruházhatja át. E speciális, vagy csak most ismerkedik, a példák a dokumentáció keresztül segítséget nyújt megkezdéséhez. Általános is figyelni a úgy, hogy azt már kialakítani, adatraktár SQL nyelv elemei az alábbi képlettel történik:

- SQL-adatraktár T-SQL-szintaxisa sok művelet használja. Támogatja a hagyományos SQL konstrukciók, például a tárolt eljárások, felhasználó által definiált függvények, táblázat szétválasztás, indexek és rendezési sorrendek széles körét is.

- SQL-adatraktár is megtalálható számos SQL Server újabb funkcióiról, például: csoportosított **columnstore** indexek, PolyBase integráció és naplózás (teljes veszélyt értékelési) adatok.

- Bizonyos az SQL-T nyelvi elemek, amely kevésbé gyakori adattárolási munkaterhelésekből- vagy SQL Server, újabbak jelenleg nem lehet. További tudnivalókért lásd: az [áttelepítési dokumentációt][].

Az SQL Server, SQL adatraktár, SQL-adatbázis és elemzési Platform rendszer között a Transact-SQL nyelvben és szolgáltatás szimpátiát fejleszthet az adatok igényeinek leginkább megfelelő megoldást. Döntse el, hova szeretné tartani az adatokat a teljesítményt, biztonságot és méret követelményeit, alapján, és majd átadásához adatok szükség szerint más rendszerek között.

## <a name="data-protection"></a>Adatok védelme

Adatraktár SQL Azure prémium helyileg felesleges tároló összes adatokat tárolja. Az adatok szinkronizált példányok karbantartott honosított hibák esetén átlátszóvá adatvédelem biztosítása érdekében a helyi adatok központban. Ezeken kívül SQL adatraktár automatikusan biztonsági másolatot készít az aktív (nem felfüggesztett) adatbázisok rendszeres időközönként Azure tároló pillanatképek használata. További tudnivalókat biztonsági mentése és visszaállítása works című témakörben a [biztonsági mentés és visszaállítás áttekintése][].

## <a name="integrated-with-microsoft-tools"></a>Microsoft-eszközök integrálva

SQL adatraktár is magába foglalja a számos eszközt, hogy az SQL Server, lehet, hogy a felhasználók ismert. Ezek a következők:

**Hagyományos SQL Server-eszközök:** SQL adatraktár teljes mértékben integrálva van az SQL Server Analysis Services, az Integration Services és a Reporting Services.

**Felhőalapú eszközök:** Adatraktár SQL Azure-adatok gyári, a megjelenítő Értékáram-elemzés, a gépi tanulási és a Power BI új eszközöket a szám mellett is használható. További teljes listáját a [beépített eszközök áttekintése][]című témakörben találhat.

**Külső eszközök:** A külső eszköz szolgáltatók sok van tanúsítvánnyal integráció a SQL adatraktár az eszközök. Egy teljes listát az [SQL adatraktár megoldás partnerek][]megtekintése

## <a name="hybrid-data-sources-scenarios"></a>Hibrid adatforrások felhasználási területei

SQL adatraktár használata PolyBase eddiginél azt jelenti, hogy az adatok áthelyezése át a ökológiai, az azt jelenti, hogy a telepítő nem relációs és a speciális hibrid környezetek kialakításához feloldása segítségével a felhasználók és a helyszíni adatforrások.

Polybase lehetővé teszi, a különböző forrásokból származó adatok használhatja az SQL-T a videóban ismerős parancsokat használatával. Polybase lehetővé teszi, a lekérdezés nem relációs adatok Azure Blob-tárolóban lévő tartani, mintha egy normál táblázat. Lekérdezés nem relációs adatok, illetve nem relációs adatok importálása az SQL adatraktár Polybase használata

- PolyBase külső táblákat használ, nem relációs adatok eléréséhez. A tábla SQL adatraktár, tárolja, és mentheti, elérheti őket SQL használatával, és eszközök, például normál relációs adatok szeretné elérni.

- Polybase való integráció a agnostic. Az azonos és funkciók, amely támogatja az összes adatforrásához azt közzététele. Az adatok Polybase értelmezése formátumokba, többek között a tagolt vagy ORC fájlokat különböző lehet.

- PolyBase is használják tárolására egy HDInsight fürthöz blob-tárolóhoz eléréséhez használható. Ki férhet hozzá relációs és nem relációs eszközökkel ugyanazokat az adatokat.

## <a name="sla"></a>SLA

SQL adatraktár felajánlja a termék szintű szolgáltatásiszint-szerződés (SLA) Microsoft Online Services SLA részeként. További információért látogasson el [az SQL adatraktár SLA][]. Minden más termék SLA információt látogasson el a [Szolgáltatás szint rendelkezést] Azure lapon, vagy töltse le őket a [Mennyiségi licencelési][] lapon. 

## <a name="next-steps"></a>Következő lépések

Most egy kicsit tudni SQL adatraktár, ismerkedjen meg gyorsan a [Hozzon létre egy SQL adatraktár][] , és [Töltse be a mintaadatokat][]. Ha még kezdő az Azure, akkor az [Azure szószedet][] hasznosnak fogja találni, fel, amelyekkel új terminológia. Vagy, ajánljuk figyelmébe az alábbi egyéb SQL adatok raktári forrásokhoz.  

- [Ügyfél sikertörténetek]
- [Blogok]
- [Frissítéseiről]
- [Videók]
- [Ügyfél tanácsadó csoport blogok]
- [Támogatási jegyek létrehozása]
- [Fórum az MSDN webhelyen]
- [Egymást fedő túlcsordulás fórum]
- [Twitter]


<!--Image references-->
[1]: ./media/sql-data-warehouse-overview-what-is/dwarchitecture.png

<!--Article references-->
[Támogatási jegyek létrehozása]: ./sql-data-warehouse-get-started-create-support-ticket.md
[Példaadatok betöltése]: ./sql-data-warehouse-load-sample-databases.md
[Hozzon létre egy SQL adatraktár]: ./sql-data-warehouse-get-started-provision.md
[Áttelepítési dokumentáció]: ./sql-data-warehouse-overview-migrate.md
[SQL-adatraktár megoldás partnerek]: ./sql-data-warehouse-partner-business-intelligence.md
[Integrált eszközök – áttekintés]: ./sql-data-warehouse-overview-integrate.md
[Biztonsági mentési és visszaállítási – áttekintés]: ./sql-data-warehouse-restore-database-overview.md
[Azure szószedet]: ../azure-glossary-cloud-terminology.md

<!--MSDN references-->

<!--Other Web references-->
[Ügyfél sikertörténetek]: https://customers.microsoft.com/search?sq=&ff=story_products_services%26%3EAzure%2FAzure%2FAzure%20SQL%20Data%20Warehouse%26%26story_product_families%26%3EAzure%2FAzure%26%26story_product_categories%26%3EAzure&p=0
[Blogok]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Ügyfél tanácsadó csoport blogok]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Frissítéseiről]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Fórum az MSDN webhelyen]: https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=AzureSQLDataWarehouse
[Egymást fedő túlcsordulás fórum]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Videók]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
[Az SQL adatraktár SLA]: https://azure.microsoft.com/en-us/support/legal/sla/sql-data-warehouse/v1_0/
[Mennyiségi licencelés]: http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=37
[Szolgáltatás szintű rendelkezést]: https://azure.microsoft.com/en-us/support/legal/sla/
