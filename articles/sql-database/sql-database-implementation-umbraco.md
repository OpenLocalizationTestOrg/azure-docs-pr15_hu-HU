<properties
   pageTitle="Azure SQL-adatbázis Azure esettanulmány - Umbraco |} Microsoft Azure"
   description="Hogyan Umbraco használja gyorsan hozhatók létre SQL-adatbázis és a felhőben bérlők ezer skála szolgáltatások ismertetése"
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
   ms.workload="NA"
   ms.date="09/22/2016"
   ms.author="carlrab"/>

# <a name="umbraco-uses-azure-sql-database-to-quickly-provision-and-scale-services-for-thousands-of-tenants-in-the-cloud"></a>Umbraco Azure SQL-adatbázis segítségével gyorsan a felhőben bérlők ezer rendelkezés és arányának szolgáltatások

![Umbraco embléma](./media/sql-database-implementation-umbraco/umbracologo.png)

Umbraco rendszer népszerű Megnyitás-adatforrás-Tartalomkezelés (cm-es) futtatható bármi kis marketingkampány vagy brosúra webhelyekről összetett alkalmazások Fortune 500 vállalatok és a globális médiaklipek webhely számára. 

> "Van legfeljebb 100 000 fejlesztők a fórumban és élő, több, mint 350,000 webhelyek Umbraco fut, a rendszer a használó fejlesztőknek igazán nagy közössége."

> – Péter sem Christensen, technikai utasításnak Umbraco

> [AZURE.VIDEO azure-sql-database-case-study-umbraco]

Ügyfél-telepítések leegyszerűsítése Umbraco hozzáadott Umbraco mint-a-szolgáltatási (UaaS): a szoftverek,-a-szolgáltatási (szoftver) ajánlja fel, hogy a helyszíni telepítésekhez szükségtelenné biztosít a beépített méretezés és kezelés felsőbb eltávolítja a fejlesztők számára a termékek megoldást, hanem innováció kezelése koncentráljon engedélyezésével. Umbraco el tudja előnyökkel összes ezeket a Microsoft Azure által kínált rugalmas platform-mint-a-szolgáltatás (PaaS) modellben megtalálható.

UaaS lehetővé teszi, hogy a szoftver az ügyfelek számára Umbraco CMS lehetőségeket, amelyek korábban már ki a gyermekektől. Ezek a felhasználók, amely tartalmazza a gyártási adatbázis használata CMS környezettel rendelkező kiépített környezetet. Ügyfelek fejlesztés és átmeneti tárolásra szolgáló környezetben, attól függően, hogy követelményeik legfeljebb két további adatbázisok adhat hozzá. Egy új környezet kérésekor az automatizált eljárással oszt ki, hogy az ügyfél adatbázis automatikusan. Az új adatbázis másodpercben, készen áll, mivel az adatbázis van már előre kiépítve szerint (lásd az ábrát 1) elérhető adatbázisok Azure rugalmas készletből Umbraco.


![Umbraco kiépítési életciklus](./media/sql-database-implementation-umbraco/figure1.png)

Ábra 1. A kiépítési életciklusának Umbraco szolgáltatásként (UaaS)
 
##<a name="azure-elastic-pools-and-automation-simplify-deployments"></a>Azure rugalmas készletek telepített és automatizálási egyszerűsítése telepítések

Azure SQL-adatbázis és más Azure szolgáltatásokhoz Umbraco ügyfelek önálló is kiépítése környezetükben, és Umbraco egyszerűen figyelésére és egy ötletes munkafolyamatot részeként az adatbázisok kezelése:

1.  Kiépítése

    Umbraco akiknek 200 elérhető kiépített előre létrehozott adatbázisok rugalmas készletek kapacitása. Új ügyfél regisztrál az UaaS, Umbraco az ügyfél közeli valós idejű új CMS-környezettel rendelkező társításával egy adatbázist az elérhetőség készletből biztosít.

    Amikor egy elérhetősége készlet eléri a küszöbértéket, rugalmas új készlet létrehozása, és új adatbázisok előre kiépített az egyes ügyfelek szükség szerint.

    Végrehajtási teljesen automatizált C# management tárak és Azure Service Bus sorban várakozó használatával.

2.  Csatlakozást

    Ügyfelek egy-három környezetben (a termelési, előkészítése és/vagy fejlesztési), minden egyes használata a saját adatbázist. Ügyfél-adatbázisok rugalmas adatbázis-készletek amely lehetővé teszi a hatékony anélkül, hogy túlságosan kiépítése méretezés megadására Umbraco szerepelnek.

    ![Umbraco projektek áttekintése](./media/sql-database-implementation-umbraco/figure2.png)

    ![Umbraco projekt részletei](./media/sql-database-implementation-umbraco/figure3.png)

    Ábra 2. Umbraco mint-a-szolgáltatási (UaaS) ügyfél webhelyén, a projektek áttekintése és a részletek megjelenítése

    Azure SQL-adatbázis adatbázis-tranzakción egységek (DTUs) használja a való életben adatbázis-tranzakciók szükséges relatív power képviseli. UaaS ügyfelek adatbázisok általában 10 DTUs működnek, hanem minden legyen a rugalmasság, ha át kívánja méretezni igény. Ez azt jelenti, hogy UaaS is gondoskodhat arról, hogy ügyfelei mindig szükséges forrásokat, még akkor is csúcs idők során. Például a legutóbbi vasárnap Sportesemény során egy UaaS ügyfél tapasztalt adatbázis csúcsaira legfeljebb 100 DTUs játék időtartamára. Azure rugalmas készletek végzett a teljesítmény csökkenése nélkül magas igények támogatásához Umbraco.

3.  Monitor

    Az adatbázis-Umbraco monitorok tevékenység belül az Azure portálon, egyéni e-mail értesítések és irányítópultok használata.

4.  Vészhelyreállítás

    Azure a két vészhelyreállítás (DR) lehetőséget nyújt: aktív Geo replikációs és Geo-visszaállítása. A DR lehetőséget, jelölje be a vállalat attól függ, hogy a [üzemkészségének célokat](sql-database-business-continuity.md).

    Aktív Geo replikációs megelőzve legrövidebb leállás válasz a leggyorsabb szintű nyújt. Aktív Geo replikációs olvasható formátumú másodlagos zónák négy különböző régiókban kiszolgálókon hozhat létre, és a formátumú másodlagos zónák közül való áttérés hiba esetén majd kezdeményezhet.

    Umbraco használatához nincs szükség a replikáció Geo, de azt kihasználhatja az Azure Geo-visszaállítás egy üzemszünetek megelőzve a minimális legrövidebb leállás biztosítása érdekében. GEO-visszaállítási adatbázis biztonsági másolatok geo felesleges Azure tároló támaszkodik. Amely lehetővé teszi, hogy a felhasználók visszaállítása egy biztonsági másolatból, ha az elsődleges régió egy üzemszünetek.

5.  A rendelkezés megszüntetése

    A project online-környezetben törlésekor a bármely társított adatbázisok (fejlesztés, átmeneti tárolásra szolgáló, vagy élő) Azure Service Bus várólista karbantartása alatt törlődnek. A automatizált eljárással visszaállítja a nem használt adatbázisok Umbraco's rugalmas adatbázis-elérhetősége készlet, hatékonyabbá tehetők a jövőbeli kiépítéséhez maximális kihasználtsági megőrzésével.

##<a name="elastic-pools-allow-uaas-to-scale-with-ease"></a>Rugalmas készletek UaaS méretezése a könnyű kezelés engedélyezése

Azure-adatbázis rugalmas készletek kihasználása a Umbraco anélkül, hogy felett vagy hiány rendelkezést tudja optimalizálni a teljesítmény felelősséget ügyfelei számára. Umbraco által jelzett majdnem 3000 adatbázisok 19 rugalmas adatbázis-készletek, az azt jelenti, hogy könnyen méretezni igazodik, hogy 325,000 ügyfelek meglévő vagy új felhasználók, akiket a felhőben egy CMS telepíthető bármelyik szükség szerint a keresztül.

Erre valójában szerint Péter sem Christensen, technikai érdeklődő Umbraco, a "UaaS most tapasztal naponta körülbelül 30 új vevők értéknövekedésével. Ügyfeleink nagyon az új projektek kiépítése másodpercben, azonnali közzététele frissítések élő webhelyükön a fejlesztői környezet használatával a "telepítés egyetlen kattintással"-ból való kényelme mellett, és végezze el a módosításokat, hasonlóan a gyors, ha az azok a hibák keresése."

Egy ügyfél nem kell-e a második és/vagy harmadik környezet bezárhatja, egyszerűen eltávolíthatja azokat a környezetben. Amely szabadítja erőforrásokat, más felhasználók számára a Umbraco rugalmas adatbázis-elérhetősége készlet részeként használható.

![Umbraco telepítési architektúra](./media/sql-database-implementation-umbraco/figure4.png)

Ábra 3. A Microsoft Azure UaaS telepítési-architektúra

##<a name="the-path-from-datacenter-to-cloud"></a>Az elérési út a felhőbe adatközponthoz

A Umbraco fejlesztők kezdetben a döntést szoftver modell áthelyezése, amikor azok tudomása volt, hogy a szolgáltatás kiépítésekor költséghatékony és méretezhető úgy kimutatásadatokat.

> "Rugalmas adatbázis készletek dolgoznak a tökéletes-e a szoftver kínáló, mert azt felfelé és lefelé szükség szerint én kapacitása. Kiépítési egyszerű feladat, és a telepítés, azt is őrizni kihasználtsági legfeljebb."

> – Péter sem Christensen, technikai utasításnak Umbraco

"Azt szeretett volna meg időt, hogy ügyfelei problémák megoldásával kapcsolatos infrastruktúra nem kezelése. Niels Hartvig, a Umbraco alapító szeretett volna megkönnyítik a legtöbb érték ügyfeleink"szerint. "Az eredetileg tekintett üzemeltető kiszolgálókon ragozott formáival, de kapacitás tervezés volna egy nightmare." Coincidentally Umbraco nem alkalmazhat bármilyen adatbázis-rendszergazdák számára, amely aláhúzás, a fő értékajánlatát UaaS használatához.

A Umbraco fejlesztők számára egy fontos cél volt UaaS az ügyfelek számára kiépítése környezetben, gyorsan és kapacitás korlátozások nélkül ad lehetőséget. De Umbraco adatközpont esetén egy dedikált szolgáltatott szolgáltatást igényelt volna feldolgozása felszakadásáig kezelheti a felesleges kapacitás rengeteg. Amely volna van azt jelenti, amely rendszeresen underutilized jelentős számítási infrastruktúra hozzáadása.

Ezeken kívül az Umbraco fejlesztőcsapatához megy végbe, amely lehetővé teszi őket újra felhasználhatja a lehető meglévő kódjukat annyi megoldást. Mikkel Madsen állapotok mint Umbraco fejlesztőeszközök, "azt elégedett volt a Microsoft fejlesztőeszközök, amely azt volt ismert, például a Microsoft SQL Server, a Microsoft Azure SQL-adatbázissal, ASP.net és az Internet Information Services (IIS). Mielőtt egy IaaS vagy egy PaaS kapcsolatos cloud megoldást, akkor győződjön meg arról, hogy meg kellene támogatja a Microsoft-eszközök és platformok, hogy nem kell módosításokat tömeges a kód alap szeretett volna. "

Összes a feltétel teljesítéséhez Umbraco keresi az alábbi képesítési információival a felhőben partner:

-   Elegendő kapacitása és megbízhatóság
-   Támogatása a Microsoft fejlesztőeszközök, így nem kényszerített Umbraco mérnökök volna való teljesen reinvent a fejlesztői környezet
-   Jelenléti adatok az összes a földrajzi piacokon, amelyben UaaS hozzáférésért (Győződjön meg arról, hogy azok is hozzáférhetnek az adataikhoz gyorsan és, hogy azok adatai egy helyet, amely megfelel a regionális szabályozói követelmények vállalkozások szükség)

##<a name="why-umbraco-chose-azure-for-uaas"></a>Miért Umbraco kiválasztott UaaS az Azure

Péter sem Christensen szerint "követően, hogy az összes beállítást, hogy kijelölt Azure, mert azt minden a feltétel teljesül, a kezelhetőség és méretezhetőség ismerete és költséghatékonyság. Azt a környezetekben a Azure VMs beállítása, és az egyes környezetekben saját Azure SQL-adatbázis példány a rugalmas adatbázis készletek példányait. Adatbázisok fejlesztése, előkészítése és élő környezetekben között elválasztva kínálunk is nagy teljesítmény elkülönítési egyező méretarányra ügyfeleink – a nagyon nagy win. "

Péter sem továbbra is fennáll, "előtt manuálisan kiépítése webes adatbázisok kiszolgálóinak volt. Most hogy nem kell foglalkoznia. Minden automatikus – kiépítési a karbantartáshoz a. "

Péter sem akkor is Boldog az Azure méretezési szolgáltatását. "Rugalmas adatbázis készletek dolgoznak a tökéletes-e a szoftver kínáló, mert azt felfelé és lefelé szükség szerint én kapacitása. Kiépítési egyszerű feladat, és a telepítés, azt is őrizni kihasználtsági legfeljebb." Péter sem állapotok, "az egyszerű, a rugalmas készletek, a szolgáltatás réteg-alapú DTUs biztosítékot együtt kínálja velünk a hozhatók létre új erőforráskészletek igény. A legutóbb egy nagyobb ügyfeleink csúcsos 100 DTUs az élő környezetben való. Azure használ, a rugalmas készletek adni az ügyfél-adatbázisok valós idejű szükség rájuk anélkül, hogy előre DTU követelmények erőforrásokkal. Egyszerűen helyezi, ügyfeleink beszerzése a válaszidő, amely elvárt, és azt is megfelelnek a teljesítmény szolgáltatói rendelkezést."

Mikkel Madsen összegzi meg: "Azt már elfogadott kapcsolódó közös szoftver példa (bevezetési vonatkozó új vevők a méretezés valós időben) a hatékony Azure algoritmus az alkalmazás mintát (előre kiépítési adatbázisok, mind a fejlesztés és live) fölött (Azure Service Bus sorban várakozó használatával Azure SQL-adatbázis együtt) alaptechnológiát."

##<a name="with-azure-uaas-is-exceeding-customer-expectations"></a>Azure-UaaS értéke meghaladja a felhasználói elvárásainak

A cloud partnerként Azure kiválasztása esetén Umbraco óta tudja UaaS ügyfelek biztosítson optimalizált-Tartalomkezelés teljesítmény elérése érdekében az IT-erőforrás befektetés szükséges egy saját szolgáltatott megoldás nélkül. Péter sem felirat látható, mint "azt közkedvelt a fejlesztői kényelmesebbé és méretezhetőség: Azure us kapja, és ügyfeleink thrilled szolgáltatásokkal és megbízhatóságára. Általános Várjon egy nagy win nekünk!"
 
## <a name="more-information"></a>További információ

- Azure-adatbázis rugalmas készletek kapcsolatos további információért olvassa el a [Rugalmas adatbázis készletek](sql-database-elastic-pool.md)című témakört.

- Azure Service Bus kapcsolatos további tudnivalókért lásd: [Azure Service Bus](https://azure.microsoft.com/services/service-bus/).

- További tudnivalók a webes és dolgozó szerepeket, a [dolgozó szerepkörök](../fundamentals-introduction-to-azure.md#compute)című témakör tartalmaz. 

- Virtuális hálózatokkal kapcsolatos további tudnivalókért lásd: a [virtuális hálózat](https://azure.microsoft.com/documentation/services/virtual-network/).    

- Biztonsági mentési és helyreállítási kapcsolatos további tudnivalókért lásd: [üzleti folytonosságot](sql-database-business-continuity.md).  

- Figyelés ppols kapcsolatos további információért olvassa el a [készletek figyelése](sql-database-elastic-pool-manage-portal.md)című témakört. 

- Szolgáltatásként Umbraco kapcsolatos további információért olvassa el a [Umbraco](https://umbraco.com/cloud)című témakört.

