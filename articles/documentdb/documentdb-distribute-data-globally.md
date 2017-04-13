<properties
   pageTitle="Adatok globálisan DocumentDB terjesztése |} Microsoft Azure"
   description="Tudjon meg többet bolygónk nagyméretű geo replikációs, feladatátvevő és adatok helyreállítás globális létrehozott adatbázisok Azure DocumentDB, teljes körű felügyelt NoSQL adatbázis-szolgáltatás használatával."
   services="documentdb"
   documentationCenter=""
   authors="kiratp"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="documentdb"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="kipandya"/>
   
   
# <a name="distribute-data-globally-with-documentdb"></a>Adatok globálisan DocumentDB terjesztése

> [AZURE.NOTE] Globális DocumentDB adatbázisok eloszlása általában elérhető, és minden újonnan létrehozott DocumentDB fiókok automatikusan engedélyezett. Ahhoz, hogy minden meglévő fiókok globális terjesztési dolgozunk, de időközben, ha azt szeretné, hogy engedélyezve van a fiók globális terjesztési [ügyfélszolgálatát](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) , és azt fogja engedélyezze azt meg most.

Azure DocumentDB az igényeknek megfelelően IoT alkalmazások, több millió eszközökre globálisan elosztott és internetes skála alkalmazásokat, hogy a felhasználók világszerte erősen válaszol elemek kézbesítése álló lett tervezve. Ezek az adatbázis rendszerek arcra elérésének kis késés access alkalmazás adatokhoz több földrajzi régiók pontosan meghatározott adatok egységesebb és elérhetőség garanciákkal a kérdés. Egy globálisan elosztott adatbázis rendszer DocumentDB egyszerűbbé teszi a globális adatok eloszlását, amelyekkel egyértelműen kompromisszumok konzisztencia, az elérhetőség és a teljesítmény elérése érdekében az összes megfelelő garanciákkal között teljes körű felügyelt és a több elem terület adatbázis-fiókok biztosításával. Adatbázis-fiókok DocumentDB kínálják magas elérhetőségét, egyetlen számjegy ms késések, több [pontosan meghatározott konzisztencia szintek] [consistency], átlátszó területi feladatátvevő a több elem webhelykezelési API-k és az azt jelenti, hogy átviteli és tárolása elastically méretezheti át a földgömb. 

  
## <a name="configuring-multi-region-accounts"></a>Több elem régió fiókok konfigurálása

Méretezze át a földgömb DocumentDB fiókját beállítása egy perc alatt az [Azure portálon](documentdb-portal-global-replication.md)keresztül lehet végrehajtani. Végezze el kell, jelölje ki a megfelelő konzisztencia szintet szintekre támogatott pontosan meghatározott konzisztencia között Azure régiók tetszőleges számú társítása az adatbázis-fiókot. DocumentDB konzisztencia szintek törlése kompromisszumok adott konzisztencia jótállási és a teljesítmény között adja meg. 

![Jól DocumentDB kínál több, meghatározott (mérsékelten) konzisztencia modellek közül választhat][1]

Jól DocumentDB kínál több, meghatározott (mérsékelten) konzisztencia modellek közül választhat.

Jelölje ki a megfelelő konzisztencia szint függ, hogy adatokat konzisztencia garantálja az alkalmazás igényeinek. DocumentDB automatikusan replikálja az adatok minden megadott területek között, és garantálja az adatbázis-fiók kiválasztása után következetességét. 


## <a name="using-multi-region-failover"></a>Több elem régió feladatátvevő használatával 

Azure DocumentDB alkalmas átlátszó feladatátvevő adatbázis fiókok különböző több Azure régiók – az új [több webhelykezelési API-khoz] [ developingwithmultipleregions] garancia, hogy az alkalmazás továbbra is használhatja a logikai végpont és zavartalan a feladatátvevő. Feladatátvevő azt szabályozzák, nyújtó számla a lehetősége, hogy az adatbázis áthelyezése az eseményt a tartomány esetleg nem sikerül feltételek bármelyike fordul elő, beleértve az alkalmazás, infrastruktúra, szolgáltatás vagy területi hibák (valós vagy szimulált). DocumentDB területi hiba esetén a szolgáltatás átlátszó sikertelen lesz az adatbázis-fiókot fölé, és az alkalmazás továbbra is hozzáférhetnek az adataikhoz elérhetősége megtartásával. Miközben DocumentDB kínál [99.99 % elérhetősége SLA][sla], végét az alkalmazás elérhetősége tulajdonságai tesztelése területi hiba mindkét [programozás útján] amelyek[ arm] és az Azure-portálon keresztül.


## <a name="scaling-across-the-planet"></a>A bolygónk keresztül méretezése
DocumentDB lehetővé teszi, hogy egymástól függetlenül átviteli kiépítése, és minden egyes DocumentDB webhelycsoport bármely méretarány a tárterület globálisan felhasználása végig a adatbázis-fiókjához tartozó egyes területeire. Automatikusan globálisan elosztott és azon részeire lépkedhet a adatbázis-fiókjához tartozó összes felügyelt DocumentDB gyűjteménye. Webhelycsoportok belül az adatbázis-fiókot is elosztva a bármelyik, amelyben a Azure régiók [DocumentDB szolgáltatás érhető el][serviceregions]. 

A megvásárolt átviteli és az egyes DocumentDB webhelycsoport felhasznált tárhely már automatikusan kiépítve át az összes terület egyaránt. Ez lehetővé teszi, hogy az alkalmazás zökkenőmentes méretezze át a földgömb [fizet csak a átviteli és az egyes órán belül tárterület a][pricing]. Például 2 millió RUs DocumentDB gyűjtemény van kiépítve, ha majd az adatbázis-fiókjához tartozó régióban kap 2 millió RUs, hogy a webhelycsoporthoz. Ez bemutat.

![Méretezési átviteli egy DocumentDB a webhelycsoporthoz négy területek között][2]

DocumentDB garantálja < 10 ms és < 15 ms olvasási P99 a késések. Az olvasási kérelmek soha nem időtartomány adatközponthoz oszlopazonosító [kiválasztása után konzisztencia követelmények]zökkenőmentes[consistency]. Az írások mindig előtt ezeket az ügyfelek nem nyugtázza helyileg elkövetett kvórum. Minden adatbázis fiók írási terület prioritású-e beállítva. A legnagyobb prioritású kijelölt terület jár a fiókhoz, az aktuális írási területen. Az összes SDK írási aktuális terület adatbázis fiók írások átlátszó irányítja. 

Végül mivel a DocumentDB teljesen [séma-felismerése nélkül] [ vldb] -Önnek nem kell aggódnia több adatközpontokkal keresztül sémák kezelése/frissítésekor, vagy a másodlagos indexek. Az [SQL-lekérdezések] [ sqlqueries] továbbra is működnek, miközben az alkalmazás és az adatmodellek továbbra is alapkoncepciójára. 


## <a name="enabling-global-distribution"></a>Globális terjesztési engedélyezése 

Eldöntheti, hogy az adatok mindkét társításával elosztott helyileg vagy globálisan egy DocumentDB egy vagy több Azure régiókat adatbázis-fiókot. Adja hozzá, vagy bármikor eltávolíthatja a régiók adatbázis-fiókjába. Megtudhatja, [hogy miként DocumentDB globális adatbázis-replikáció: az Azure portálon végezze el](documentdb-portal-global-replication.md)a portál használatával globális terjesztési engedélyezéséhez. Globális terjesztési programozottan engedélyezi, olvassa el a [több területre DocumentDB fiókoknál fejlődő](documentdb-developing-with-multiple-regions.md).

## <a name="next-steps"></a>Következő lépések

További információ a terjesztésével adatait globálisan DocumentDB az alábbi cikkekben:

* [Átviteli és a webhelycsoport-tároló kiépítése] [throughputandstorage]
* [Több elem webhelykezelési API-khoz többi keresztül. .NET, Java, Python és csomópont SDK] [developingwithmultipleregions]
* [DocumentDB konzisztencia szintjén] [consistency]
* [Elérhetőség SLA] [sla]
* [Adatbázis-fiók kezelése] [manageaccount]

[1]: ./media/documentdb-distribute-data-globally/consistency-tradeoffs.png
[2]: ./media/documentdb-distribute-data-globally/collection-regions.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[pcolls]: documentdb-partition-data.md
[consistency]: documentdb-consistency-levels.md
[consistencytradeooffs]: ./documentdb-consistency-levels/#consistency-levels-and-tradeoffs
[developingwithmultipleregions]: documentdb-developing-with-multiple-regions.md
[createaccount]: documentdb-create-account.md
[manageaccount]: documentdb-manage-account.md
[manageaccount-consistency]: documentdb-manage-account.md#consistency
[throughputandstorage]: documentdb-manage.md
[arm]: documentdb-automation-resource-manager-cli.md
[regions]: https://azure.microsoft.com/regions/
[serviceregions]: https://azure.microsoft.com/en-us/regions/#services 
[pricing]: https://azure.microsoft.com/pricing/details/documentdb/
[sla]: https://azure.microsoft.com/support/legal/sla/documentdb/ 
[vldb]: http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf
[sqlqueries]: documentdb-sql-query.md

