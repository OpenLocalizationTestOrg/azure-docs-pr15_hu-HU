<properties
    pageTitle="Az SQL Server alkalmazás mintázatok a VMs |} Microsoft Azure"
    description="Ez a cikk bemutatja a alkalmazás mintázatok az SQL Server Azure VMs. Biztosít megoldás fejlesztője és a fejlesztők a alap jó alkalmazás architektúrája és a tervezéshez."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="luisherring"
    manager="jhubbard"
    editor=""
    tags="azure-service-management,azure-resource-manager" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="08/19/2016"
    ms.author="lvargas" />

# <a name="application-patterns-and-development-strategies-for-sql-server-in-azure-virtual-machines"></a>Alkalmazás mintázatok és Azure virtuális gépeken futó SQL Server fejlesztési stratégiák

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


## <a name="summary"></a>Összefoglalás:
Annak megállapítása, melyik alkalmazás mintázat vagy a minta használni az SQL Server-alapú alkalmazások Azure-ban környezet egy fontos tervezési döntés, és azt csak egy tömör megértése hogyan SQL Server Azure infrastruktúra összetevőkhöz működik együtt. Az SQL Server Azure infrastruktúrájának szolgáltatásai áttelepítéséhez, karbantartása,-egyszerűen figyelheti a meglévő, beépített – a Windows Server az Azure virtuális gépeken futó SQL Server-alkalmazások.

Ez a cikk a cél, a szükséges megoldás fejlesztője és a fejlesztők a alap jó alkalmazás architektúra és a tervezés, követik áttelepítéséhez Azure, valamint az Azure új alkalmazások fejlesztéséhez meglévő alkalmazást.

A minden alkalmazás minta egy helyszíni forgatókönyv, a megfelelő felhő engedélyezni megoldás és a kapcsolódó technikai javaslatok kifejezésre keresve. Ezenkívül a cikk azt ismerteti, hogy Azure-specifikus fejlesztési stratégiák, hogy megfelelően az alkalmazások tervezhet. A sok lehetséges alkalmazás mintázatok miatt célszerű fejlesztője és a fejlesztők az alkalmazások és a felhasználók leginkább megfelelő mintájának kell választania.

**Technikai munkatársak:** Péter Carlos Vargas hering, Madhan Arumugam Ramakrishnan

**Technikai véleményezők:** Corey Sanders, Drew McDaniel, Narayan Annamalai, Nir Mashkowski, Sanjay Mishra, Silvano Coriani, lengyel Schackow, Tim Hickey, Tim Wieman, Xin Jin

## <a name="introduction"></a>– Bevezetés

N szintű alkalmazások számos különböző típusú fejleszthet elválasztva a különböző számítógépeken, valamint ahogy külön összetevőket a különböző alkalmazás rétegek összetevői. Például az ügyfélalkalmazás is elhelyezhet, és üzleti szabály az egyik gépen, előtér webes réteg és adat-hozzáférési szint összetevőket egy másik számítógépen, és egy másik számítógépen lévő háttéradatbázis réteg összetevőket. Az ilyen típusú tagolásának segítséget nyújt a elkülöníteni minden réteg egymástól. Honnan származnak az adatok módosításakor, nem kell az ügyfél vagy a webes alkalmazás, de csak az adat-hozzáférési szint összetevőket módosítása.

Egy tipikus *n szintű* alkalmazást tartalmazza a bemutató réteg, a vállalati réteg és az adatok réteg:


| Réteg              | Leírás                                                                                                                                                                     |
|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Bemutató** | A *bemutató réteg* (webes szint, előtér-réteg), ahol felhasználók az alkalmazások használata a réteg.                                                                      |
| **Üzleti**     | Az *üzleti első csoportba tartozó* (középső) a bemutató első csoportba tartozó réteghez és az adatok réteg egymással használja, és a rendszer az alapvető funkcionalitást tartalmazza. |
| **Adatok**         | Az *adatok réteg* tulajdonképpen a kiszolgáló, amely az alkalmazások adatok (például egy SQL Servert futtató kiszolgáló) tárolja.                                                             |

Alkalmazás rétegek ismertetik a logikai csoportosítását a funkciók és -összetevők az alkalmazásokban; mivel a réteg a funkciók és -összetevők tényleges eloszlását külön fizikai kiszolgálók, a számítógépek, a hálózatok vagy a távoli helyek ismertetik. A rétegeket, az alkalmazás előfordulhat, hogy állnak a tényleges számítógépen (az egy réteg), vagy előfordulhat, hogy a különböző számítógépeken (n szintű) kell elosztva, és más rétegek pontosan meghatározott felületeken keresztül összetevőket kommunikálni rétegekre-összetevők. A szerződési időszak réteg fizikai terjesztési mintákat például kétszintű, a harmadik szintű és az n szintű hivatkozásként érdemes elképzelnie. **2 szintű alkalmazás mintát** tartalmazza a két alkalmazás rétegek: application server és az adatbázis-kiszolgálóval. A közvetlen kommunikáció az application server és az adatbázis-kiszolgáló közötti történik. Az application server web-szint és a vállalati szintű összetevőit tartalmazza. **3 szintű alkalmazás mintát**, három alkalmazás réteg sorolhatók: webes server, az üzleti logikai réteg és/vagy a vállalati réteg adat-hozzáférési összetevőket tartalmazó alkalmazáskiszolgáló és az adatbázis-kiszolgálóval. Az érintett webkiszolgálóra, és az adatbázis-kiszolgáló közötti kommunikáció történik át az application server. Alkalmazás rétegek és rétegek részletes információkat a [Microsoft alkalmazás architektúra útmutató](https://msdn.microsoft.com/library/ff650706.aspx)című témakörben.

A cikket elolvasva indítása előtt rendelkeznie kell az SQL Server és Azure fogalmak alapvető ismeretek. Tudnivalókért lásd: az [SQL Server könyvek Online](https://msdn.microsoft.com/library/bb545450.aspx), [Az Azure virtuális gépeken futó SQL Server](virtual-machines-windows-sql-server-iaas-overview.md) és [Azure.com](https://azure.microsoft.com/).

Ez a cikk ismerteti a több alkalmazás mintázatok, lehet, hogy csoporttagokkal tartott egyszerű alkalmazásai, valamint a nagyon összetett vállalati alkalmazások. Mielőtt részletező minden mintát, azt javasoljuk, hogy meg kell megismerkedhet a rendelkezésre álló adatokat tároló szolgáltatások, például [Azure tárhely](../storage/storage-introduction.md), [Azure SQL-adatbázis](../sql-database/sql-database-technical-overview.md)és [Az Azure virtuális gép az SQL Server](virtual-machines-windows-sql-server-iaas-overview.md)Azure-ban. Döntések a legjobb tervezés az alkalmazások, ismerje meg, milyen adatokat tároló szolgáltatás egyértelműen használata.

### <a name="choose-sql-server-in-an-azure-virtual-machine-when"></a>Válassza az SQL Server Azure virtuális gépen, ha:

- Az SQL Server és a Windows vezérlő van szüksége. Ez lehet, hogy például az SQL Server-verzióra, speciális gyorsjavítások, teljesítmény konfigurációs stb.

- Egy helyszíni SQL Server teljes kompatibilitásának van szükség, és át szeretné helyezni a meglévő alkalmazást, az Azure-van.

- Kihasználhatja az Azure környezetben a funkcióinak szeretne, de Azure SQL-adatbázis nem támogatja az alkalmazáshoz szükséges összes funkcióját. Ez lehet például a az alábbi kérdésekre:

    - **Az adatbázis mérete**: Ez a cikk frissült időben SQL-adatbázis támogatja az adatok legfeljebb 1 TB adatbázis. Ha az alkalmazáshoz szükséges legfeljebb 1 TB-nyi adatokat, és nem szeretne egyéni sharding megoldások megvalósítása, ajánlott használata SQL Server-Azure virtuális gépen. A legfrissebb információkat lásd: a [Méretezés ki Azure SQL-adatbázisok](https://msdn.microsoft.com/library/azure/dn495641.aspx) és [Azure SQL-adatbázis szolgáltatási rétegek teljesítményszint](../sql-database/sql-database-service-tiers.md).
    - **HIPAA megfelelőségi**: egészségügyi ügyfelek és független szoftver keresve előfordulhat, hogy válassza [Az Azure virtuális gépeken futó SQL Server](virtual-machines-windows-sql-server-iaas-overview.md) [Azure SQL-adatbázis](../sql-database/sql-database-technical-overview.md) helyett, mert az SQL Server-Azure virtuális gép szerint HIPAA üzleti társítása szerződés (BAA) vonatkozik. Információ a megfelelőségi [Microsoft Azure Trust Center: megfelelőség](https://azure.microsoft.com/support/trust-center/compliance/).
    - **Példány szintű szolgáltatások**: Ebben az esetben a SQL-adatbázis nem támogat az adatbázis-Ön kívüli live szolgáltatásokat (például a csatolt kiszolgáló ügynök feladatok, FileStream, szolgáltatás ügynök stb.). További információért témakörben [Azure SQL-adatbázis irányelveket](https://msdn.microsoft.com/library/azure/ff394102.aspx).

## <a name="1-tier-simple-single-virtual-machine"></a>1 – réteg (egyszerű): egyetlen virtuális gépen

A alkalmazás mintázat telepíteni az SQL Server-alkalmazás és az adatbázis egy különálló virtuális géphez Azure-ban. Virtuális ugyanarra a gépre tartalmazza az ügyfél-és webalkalmazás, üzleti összetevők, adat-hozzáférési réteg és az adatbázis-kiszolgálóval. A bemutatók, a vállalati verzió és az adat-hozzáférési kódot logikailag választják el, de egy kiszolgáló gépen fizikailag található. A legtöbb ügyfelek indítsa el a alkalmazás mintázattal és ezt követően ezeket méretezése a rendszer további webes szerepkörök vagy virtuális gépeken futó hozzáadásával.

Ez az alkalmazás mintát a következő esetekben hasznos:

- Egyszerű áttelepítése az Azure platform dönti el, hogy a platform-e választ ad az alkalmazás követelmények szeretne.

- Szeretne tartani, a rétegek között a késés csökkentheti az azonos Azure adatközpontban azonos virtuális gépen futó alkalmazás rétegek.

- Szeretne gyorsan kiépítése fejlesztés és tesztelje a környezetek rövid ideig.

- Változó terhelést szintek vizsgálata terhelési végrehajtandó műveletet, de egyszerre nem szeretné, hogy saját és sok fizikai gépek mindig karbantartása.

Az alábbi ábra bemutatja, hogyan egy egyszerű helyszíni a helyzet, és hogyan akkor is megoldást üzembe helyez a felhő engedélyezni az Azure virtuális egyetlen gépen.

![1 – réteg alkalmazás mintával](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728008.png)

Az üzleti réteg (üzleti logikai funkcióinak és -összetevők használatát adatok) telepítése a bemutató réteget, ugyanabban a fizikai rétegben teljes méretűre alkalmazás teljesítményének, kivéve, ha egy külön réteg miatt méretezhetőség vagy biztonsági aggályokat kell használnia.

Mivel ez nagyon közös mintát induljon, hasznosak lehetnek az alábbi cikkben a áttelepítési az adatok áthelyezése az SQL Server virtuális: [SQL Server-Azure virtuális az adatbázis áttelepítése](virtual-machines-windows-migrate-sql.md).

## <a name="3-tier-simple-multiple-virtual-machines"></a>3 – réteg (egyszerű): több virtuális gépeken futó

A alkalmazás mintázat Azure-ban a 3-as szintű alkalmazások helyezze a minden alkalmazás réteg egy másik virtuális gépen rendszerbe. Ez egy egyszerű skála felfelé, és a méretezési forgatókönyvek rugalmas környezetet nyújt. Amikor egy virtuális gép az ügyfél-és webalkalmazás tartalmaz, a többi egyik tárolja, az üzleti összetevők, és a többi egyik tárolja, az adatbázis-kiszolgáló.

Ez az alkalmazás mintát a következő esetekben hasznos:

- Áttelepítése az összetett adatbázis-alkalmazások Azure virtuális gépeken futó szeretne.

- Azt szeretné, hogy a különböző régiókban tárolandó másik alkalmazás rétegek. Például, előfordulhat, hogy megosztott adatbázisokat jelentési célokra több területre telepített.

- Vállalati alkalmazások helyezze át a helyszíni virtualizálva platformokon Azure virtuális gépeken futó szeretne. A vállalati alkalmazások részletes vitafórum olvassa el a [Vállalati alkalmazás újdonságai](https://msdn.microsoft.com/library/aa267045.aspx)című témakört.

- Szeretne gyorsan kiépítése fejlesztés és tesztelje a környezetek rövid ideig.

- Változó terhelést szintek vizsgálata terhelési végrehajtandó műveletet, de egyszerre nem szeretné, hogy saját és sok fizikai gépek mindig karbantartása.

Az alábbi ábra bemutatja, hogyan is elhelyezhet egy egyszerű 3 szintű alkalmazás Azure virtuális másik gépre minden alkalmazás réteg bejelölésével.

![3 – réteg alkalmazás mintával](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728009.png)

A alkalmazás mintázat van csak egy virtuális gép (virtuális) minden réteg. Ha több VMs Azure-ban, azt javasoljuk, egy virtuális hálózat beállítása. [Azure virtuális hálózati](../virtual-network/virtual-networks-overview.md) hoz létre egy megbízható biztonsági oszlopazonosító és is lehetővé teszi, hogy VMs kommunikációhoz egymással a magánjellegű IP-címet. Ezeken kívül mindig győződjön meg arról, hogy az összes internetes kapcsolatok csak nyissa meg a bemutató réteg. Ha ez az alkalmazás minta követően kezelése a hálózati biztonsági csoport szabályokat, a hozzáférés vezérléséhez. További tudnivalókért lásd: a [külső hozzáférés engedélyezése a virtuális az Azure portálon](virtual-machines-windows-nsg-quickstart-portal.md).

Kattintson a diagramon internetes protokollok lehet TCP, UDP, HTTP vagy HTTPS.

>[AZURE.NOTE] Az Azure virtuális hálózat beállítása az ingyenes. Jó helyen jár az előfizetést terhelő a virtuális Magánhálózati átjáró a helyszíni kapcsolódó. Ez a díj alapul, amely a kapcsolat kiépített és elérhető idő mennyiségét.

## <a name="2-tier-and-3-tier-with-presentation-tier-scale-out"></a>egységes 2 és 3 szintű bemutató első csoportba tartozó méretezési

A alkalmazás mintázat 2 szintű vagy 3 szintű adatbázis-alkalmazás Azure virtuális gépeken futó helyezze a minden alkalmazás réteg egy másik virtuális gépen rendszerbe. Ezeken kívül méretezni el a bemutató réteg ügyfelek bejövő kéréseit megnövelt mennyisége miatt.

Ez az alkalmazás mintát a következő esetekben hasznos:

- Vállalati alkalmazások helyezze át a helyszíni virtualizálva platformokon Azure virtuális gépeken futó szeretne.

- A bemutató réteg ügyfelek bejövő kéréseit megnövelt mennyisége miatt méretezése szeretne.

- Szeretne gyorsan kiépítése fejlesztés és tesztelje a környezetek rövid ideig.

- Változó terhelést szintek vizsgálata terhelési végrehajtandó műveletet, de egyszerre nem szeretné, hogy saját és sok fizikai gépek mindig karbantartása.

- Saját maga igény felfelé és lefelé méretezheti infrastruktúra környezetben szeretne.

Az alábbi ábra bemutatja, hogyan helyezheti el az alkalmazás rétegek több virtuális gépeken futó Azure-ban méretezéssel el a bemutató réteg ügyfelek bejövő kéréseit megnövelt mennyisége miatt. A diagram naplójában Azure terheléselosztó a felelős a forgalom terjesztésére több virtuális gépeken futó keresztül, és is annak megállapítása, hogy melyik webkiszolgáló való csatlakozáshoz. Webkiszolgálón mögött egy terheléselosztó több példányon biztosítja a bemutató réteg a magas elérhetőségét.

![Alkalmazás mintát - bemutató réteg skála meg](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728010.png)

### <a name="best-practices-for-2-tier-3-tier-or-n-tier-patterns-that-have-multiple-vms-in-one-tier"></a>Gyakorlati tanácsok az egy réteg több VMs rendelkező egységes 2, 3 szintű vagy n szintű mintázatok

Azt ajánljuk, hogy helyezni, amely a virtuális gépeken futó tartoznak ugyanabban a rétegben ugyanazt a felhőalapú szolgáltatást, és ugyanolyan elérhetőségének beállítása. Például a set-webkiszolgálón helyezze **CloudService1** és **AvailabilitySet1** és egy adatbázis-kiszolgálók **CloudService2** és **AvailabilitySet2**. Az elérhetőség beállítása a Azure lehetővé teszi, hogy a magas elérhetősége csomópontok helyezhet külön hibafa tartományok, és frissítse a tartomány.

Kihasználhatja az egy réteg több virtuális példányát, szüksége Azure terheléselosztó konfigurálása rétegek alkalmazás között. Minden réteg betöltése terheléselosztó konfigurálásához terheléselosztás végpont létrehozásához a Minden réteg VMs külön-külön. Az egy adott réteg előbb létre VMs ugyanazt a felhőalapú szolgáltatást. Ez biztosítja, hogy biztosítanak-e az azonos nyilvános virtuális IP-címét. Ezután hozzon létre egy adott rétegben a virtuális gépeken futó zárólap. Az azonos végpontot ezután rendelhet a többi virtuális gépeken futó terheléselosztás, hogy rétegben. Terheléselosztás meg létrehozásával forgalom elosztása több virtuális gépeken futó, és azt is, hogy melyik csomópontot, ha egy kódmentes virtuális csomópont nem sikerül kapcsolódni megállapításához betöltése terheléselosztó. Például a webkiszolgálón mögött egy terheléselosztó több példányon biztosítja a bemutató réteg a magas elérhetőségét.

A legjobb, mint mindig győződjön meg arról, hogy minden internetes kapcsolat először nyissa meg a bemutató réteg. A bemutató réteg fér hozzá a vállalati réteg, és kattintson a vállalati réteg az adatok réteg segítségével érheti el. További tájékoztatást a bemutató réteg való hozzáférés engedélyezése olvassa el a [külső hozzáférés engedélyezése a virtuális az Azure portál használatával](virtual-machines-windows-nsg-quickstart-portal.md)című témakört.

Figyelje meg, hogy a helyszíni környezetben terheléselosztóként hasonlít működik-e a betöltése terheléselosztó Azure-ban. További tudnivalókért olvassa el a [terheléselosztás az Azure infrastruktúrájának szolgáltatásai](virtual-machines-windows-load-balance.md)című témakört.

Ezenkívül azt javasoljuk, hogy Ön egy személyes hálózat beállítása a virtuális gépeken futó Azure virtuális hálózaton keresztül. Ebben a csoportban adhatja át a személyes IP-cím egymás között kommunikáció őket. További tudnivalókért lásd: az [Azure virtuális hálózat](../virtual-network/virtual-networks-overview.md).

## <a name="2-tier-and-3-tier-with-business-tier-scale-out"></a>egységes 2 és 3 szintű az üzleti réteg skála meg

A alkalmazás mintázat 2 szintű vagy 3 szintű adatbázis-alkalmazás Azure virtuális gépeken futó helyezze a minden alkalmazás réteg egy másik virtuális gépen rendszerbe. Emellett célszerű az alkalmazás kiszolgáló összetevők több virtuális gépeken futó az alkalmazás bonyolultsága miatt terjesztése.

Ez az alkalmazás mintát a következő esetekben hasznos:

- Vállalati alkalmazások helyezze át a helyszíni virtualizálva platformokon Azure virtuális gépeken futó szeretne.

- Szeretné terjeszteni a alkalmazás kiszolgálói összetevőket több virtuális gépeken futó alkalmazás bonyolultsága miatt.

- Üzleti logikai nehéz helyszíni üzleti (sor üzleti) alkalmazások Azure virtuális gépeken futó áthelyezni kívánt. ÜZLETI alkalmazások olyan elengedhetetlen egy vállalati, például a könyvelési, az emberi erőforrások (ó), bérszámfejtő, ellátási lánc kezelése és erőforrás-alkalmazások tervezési futó kritikus számítógép alkalmazásokat.

- Szeretne gyorsan kiépítése fejlesztés és tesztelje a környezetek rövid ideig.

- Változó terhelést szintek vizsgálata terhelési végrehajtandó műveletet, de egyszerre nem szeretné, hogy saját és sok fizikai gépek mindig karbantartása.

- Saját maga igény felfelé és lefelé méretezheti infrastruktúra környezetben szeretne.

Az alábbi ábra bemutatja, hogyan egy helyszíni eset és a felhő engedélyezni megoldás. Ebben az esetben az alkalmazás rétegek több virtuális gépeken futó Azure-ban, a vállalati réteg, mely tartalmazza az üzleti logikai réteg és adat-hozzáférési összetevőket méretezéssel helyezze. A diagram naplójában Azure terheléselosztó a felelős a forgalom terjesztésére több virtuális gépeken futó keresztül, és is annak megállapítása, hogy melyik webkiszolgáló való csatlakozáshoz. Az alkalmazás-kiszolgálók mögött egy terheléselosztó több példányával biztosítja a vállalati réteg a magas elérhetőségét. További tudnivalókért lásd: [Gyakorlati tanácsok a egységes 2, 3 szintű vagy n szintű alkalmazás mintázatok, amelyek több virtuális gépeken futó az egy réteg](#best-practices-for-2-tier-3-tier-or-n-tier-patterns-that-have-multiple-vms-in-one-tier).

![Üzleti réteg skála meg az alkalmazás mintával](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728011.png)

## <a name="2-tier-and-3-tier-with-presentation-and-business-tiers-scale-out-and-hadr"></a>egységes 2 és 3 szintű bemutató és üzleti rétegek skála meg és HADR

A alkalmazás mintázat 2 szintű vagy 3 szintű adatbázis-alkalmazás Azure virtuális gépeken futó azáltal, hogy a bemutató réteg (webkiszolgáló) és az üzleti réteg (alkalmazáskiszolgáló) összetevők több virtuális gépeken futó rendszerbe. Nagyfokú rendelkezésre állásának és katasztrófa helyreállítási megoldások ezeken kívül az adatbázisok Azure virtuális gépeken futó végrehajtása.

Ez az alkalmazás mintát a következő esetekben hasznos:

- Szeretne áttérni vállalati alkalmazások helyszíni virtualizált platformokon Azure SQL Server magas rendelkezésre állásának és katasztrófa helyreállítási lehetőségeket alkalmazásával.

- Szeretné méretezni meg a bemutató réteg és a vállalati réteg ügyfelek bejövő kéréseit nagyobb mennyiségű és az alkalmazás komplexitását.

- Szeretne gyorsan kiépítése fejlesztés és tesztelje a környezetek rövid ideig.

- Változó terhelést szintek vizsgálata terhelési végrehajtandó műveletet, de egyszerre nem szeretné, hogy saját és sok fizikai gépek mindig karbantartása.

- Saját maga igény felfelé és lefelé méretezheti infrastruktúra környezetben szeretne.

Az alábbi ábra bemutatja, hogyan egy helyszíni eset és a felhő engedélyezni megoldás. Ebben az esetben, méretezze át a bemutató réteg és az üzleti réteg összetevők több virtuális gépeken futó Azure-ban. Ezeken kívül végrehajtja a magas rendelkezésre állásának és katasztrófa helyreállítási (HADR) technikákat az SQL Server-adatbázishoz Azure-ban.

Futó példányok az alkalmazás különböző VMs ellenőrizze, hogy Ön-e terheléselosztási kérések át őket. Ha több virtuális gépeken futó, győződjön meg arról, hogy a VMs elérhető és futó, amely az idő van szükség. Ha letiltja a terheléselosztás, Azure betöltése terheléselosztó VMs állapotának nyomon követi, és arra utasítja megfelelően a megfelelő működő virtuális csomópont a bejövő hívásokat. Tájékoztatást arról, hogyan állíthatja be a virtuális gépeken futó a terheléselosztás olvassa el a [terheléselosztás Azure infrastruktúrájának szolgáltatásai](virtual-machines-windows-load-balance.md)című témakört. Webes és az alkalmazás kiszolgálók mögött egy terheléselosztó több példányával biztosítja a bemutatót, és az üzleti meghatározási magas elérhetőségét.

![Méretezési és a felső elérhetősége](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728012.png)

### <a name="best-practices-for-application-patterns-requiring-sql-hadr"></a>Gyakorlati tanácsok a SQL HADR igénylő alkalmazás mintázatok

Az SQL Server magas rendelkezésre állásának és katasztrófa helyreállítási megoldások az Azure virtuális gépeken futó beállításakor az [Azure virtuális hálózaton](../virtual-network/virtual-networks-overview.md) keresztül, a virtuális gépeken futó virtuális hálózat beállítása során kötelező.  Virtuális gépeken futó virtuális hálózaton belül lesz egy stabil magánjellegű IP-címet a szolgáltatás legrövidebb leállás után is, így elkerülheti a szükséges DNS-névfeloldás frissítésének időpontja. Emellett a virtuális hálózat lehetővé teszi, hogy a helyszíni hálózaton kiterjesztése Azure, és létrehoz egy megbízható biztonsági határérték. Ha például ha az alkalmazás a vállalati tartomány korlátozások (többek között a Windows-hitelesítés, az Active Directory) van, [Azure virtuális hálózat](../virtual-network/virtual-networks-overview.md) beállítása szükség.

A legtöbb futó gyártási kód Azure, felhasználókat elsődleges és másodlagos kópiák megakadályozzák Azure-ban.

Átfogó információt és oktatóanyagok a magas rendelkezésre állásának és katasztrófa helyreállítási technikákat című témakörben talál [magas rendelkezésre állásának és a helyreállítás az Azure virtuális gépeken futó SQL Server](virtual-machines-windows-sql-high-availability-dr.md).

## <a name="2-tier-and-3-tier-using-azure-vms-and-cloud-services"></a>2 egységes és a 3-as réteg Azure VMs és Cloud Services használatával

A alkalmazás mintázat rendszerbe Azure 2 szintű vagy 3 szintű alkalmazást mindkét [Azure Cloud Services](../cloud-services/cloud-services-choose-me.md#tellmecs) (webes és dolgozó szerepkörök - szolgáltatásként (PaaS) Platform) használatával és [Azure virtuális gépeken futó](virtual-machines-windows-about.md) (infrastruktúra-szolgáltatásként (IaaS)). A bemutató szint/üzleti réteg és [Azure virtuális](virtual-machines-windows-about.md) gépeken futó SQL Server [Azure Cloud Services](https://azure.microsoft.com/documentation/services/cloud-services/) használata a adatok réteg akkor előnyös Azure futó legtöbb alkalmazás. A hiba oka, hogy egy számítási példány Cloud Services futó összevonása egy könnyű kezelés, telepítési, figyelésére és méretezési.

Felhőbeli szolgáltatásokkal Azure akiknek az infrastruktúra-, rutinszerű karbantartása hajt végre, összekapcsolja az operációs rendszerek és megpróbálja a szolgáltatásra és a hardver hibák helyreállítása. Amikor az alkalmazás méretezési van szüksége, automatikus és manuális méretezési lehetőségek érhetők el a felhőalapú szolgáltatás projekthez növelésével vagy csökkentésével példányok vagy az alkalmazás által használt virtuális gépeken futó számát. Ezeken kívül az Azure-ban egy felhőalapú szolgáltatás a project alkalmazás telepítése a helyszíni Visual Studio is használhatja.

Összefoglalásként tehát elmondhatjuk Ha nem szeretné saját teljes körű bemutató/vállalati verzió felügyeleti feladatait, az első csoportba tartozó és az alkalmazás bármely összetett konfigurációs szoftver vagy az operációs rendszer szükséges, nem Azure Cloud Services használata. Ha Azure SQL-adatbázis nem támogatja az összes funkcióját keres, használata SQL Server-Azure virtuális gép az adatok réteg. Az alkalmazás forgalmi Azure Cloud Services és tárolja az adatokat az Azure virtuális gépeken futó egyesíti mindkét szolgáltatások előnyeit. Részletes összehasonlítása érdekében című témakör a [Comparing fejlesztési stratégiák Azure-ban](#comparing-web-development-strategies-in-azure).

A bemutató réteg a alkalmazás mintát tartalmaz egy webes szerepkört, amelynek Cloud Services összetevő a végrehajtás Azure környezetben működik, és azt testre szabott web alkalmazásprogramozási az IIS és ASP.NET által támogatott. A vállalati vagy kódmentes réteg egy dolgozó szerepkört, amely Cloud Services összetevő az adatvégrehajtás Azure környezetben fut, és akkor hasznos, ha általános fejlesztés és feldolgozása háttér végezhetnek egy webes szerepkör tartalmazza. Az adatbázis réteg virtuális gépen az SQL Server Azure-ban található. A bemutató réteg és az adatbázis réteg közötti kommunikáció történik, vagy közvetlenül a vállalati réteg – dolgozó szerepkör összetevők fölé.

Ez az alkalmazás mintát a következő esetekben hasznos:

- Szeretne áttérni vállalati alkalmazások helyszíni virtualizált platformokon Azure SQL Server magas rendelkezésre állásának és katasztrófa helyreállítási lehetőségeket alkalmazásával.

- Saját maga igény felfelé és lefelé méretezheti infrastruktúra környezetben szeretne.

- Azure SQL-adatbázis összes funkcióját igénylő adatbázis vagy az alkalmazás nem támogatja.

- Változó terhelést szintek vizsgálata terhelési végrehajtandó műveletet, de egyszerre nem szeretné, hogy saját és sok fizikai gépek mindig karbantartása.

Az alábbi ábra bemutatja, hogyan egy helyszíni eset és a felhő engedélyezni megoldás. Ebben az esetben a bemutató réteg webes szerepkörök, a vállalati réteg szerepkörök dolgozó, de az adatok réteg az Azure virtuális gépeken futó helyezze. A bemutató réteg több példányban futó különböző webes szerepkörök biztosítja, hogy egyenleg kérések betöltése át őket. Azure Cloud Services Azure virtuális gépeken futó egyesítésekor azt javasoljuk [, Hogy Ön Azure virtuális hálózat](../virtual-network/virtual-networks-overview.md) beállítása, valamint. [Azure virtuális hálózati](../virtual-network/virtual-networks-overview.md)akkor a felhőben stabil és állandó saját IP-címek belül ugyanazt a felhőalapú szolgáltatást. Miután egy virtuális hálózati definiálni a virtuális gépeken futó, és a felhőszolgáltatásokba, azok is kezdeményezhessen egymás között a magánjellegű IP-címe alatt. Ezeken kívül virtuális gépeken futó és Azure webes/dolgozó szerepkörök ugyanabba a [Azure virtuális hálózati](../virtual-network/virtual-networks-overview.md) összevonása kis késés és biztonságosabbá kapcsolódási. További tudnivalókért olvassa el a [Mi az, hogy egy felhőalapú szolgáltatásba](../cloud-services/cloud-services-choose-me.md)című témakört.

A diagram naplójában Azure terheléselosztó több virtuális gépeken futó forgalom elosztja, és is határozza meg, hogy melyik webkiszolgáló vagy az application server kapcsolódni. A webes és az alkalmazás kiszolgálók mögött egy terheléselosztó több példányával biztosítja a bemutató réteg és a vállalati réteg magas elérhetőségét. További tudnivalókért lásd: [Gyakorlati tanácsok a SQL HADR igénylő alkalmazás mintázatok](#best-practices-for-application-patterns-requiring-sql-hadr).

![A Cloud Services alkalmazás mintázatok](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728013.png)

Ez az alkalmazás minta végrehajtása egy másik megoldást környezetbe egy összesített webes szerepkört, amelyben a bemutató réteg és üzleti réteg összetevők, a következő ábrán látható módon. Ez az alkalmazás minta akkor lehet hasznos, hogy az állapot-nyilvántartó tervezés alkalmazásokhoz. Azure állapot nélküli számítási csomópontok kínál a webes és dolgozó szerepkörök, mivel a azt javasoljuk, hogy alkalmazhat egy logikai tárolja az egyik a következő technológiák munkamenet-állapot: [Azure gyorsítótárazás](https://azure.microsoft.com/documentation/services/redis-cache/), [Azure Táblatárolóhoz](../storage/storage-dotnet-how-to-use-tables.md) vagy [Azure SQL-adatbázis](../sql-database/sql-database-technical-overview.md).

![A Cloud Services alkalmazás mintázatok](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728014.png)

## <a name="pattern-with-azure-vms-azure-sql-database-and-azure-app-service-web-apps"></a>Minta Azure VMs, Azure SQL-adatbázis és Azure alkalmazás Service (a Web Apps alkalmazások)

Ez az alkalmazás minta elsődleges célja követnie Azure infrastruktúra kombinálja a service (IaaS) összetevők Azure platform-mint-a-szolgáltatás összetevőket (PaaS) a megoldás. A minta relációs adatok tárolására Azure SQL-adatbázis összpontosít. Nem tartalmazza az SQL Server Azure virtuális gépen, amely szerint a szolgáltatás egyesíti az Azure infrastruktúra része.

A alkalmazás mintázat egy adatbázis alkalmazás Azure virtuális ugyanarra a gépre a bemutatót, és az üzleti rétegek elhelyezése és Azure SQL-adatbázis (SQL-adatbázis) Servers adatbázis elérése rendszerbe. A bemutató réteg alkalmazhat hagyományos IIS-alapú webes megoldások használatával. Vagy a kombinált bemutató és üzleti réteg alkalmazhat az [Azure Web Apps](https://azure.microsoft.com/documentation/services/app-service/web/)használatával.

Ez az alkalmazás mintát a következő esetekben hasznos:

- Már rendelkezik egy meglévő SQL-adatbázis kiszolgálója Azure-ban beállított, és az alkalmazás gyorsan vizsgálni kívánt.

- Az eszköz képességeit Azure környezetben vizsgálni kívánt.

- Szeretne gyorsan kiépítése fejlesztés és tesztelje a környezetek rövid ideig.

- Lehet, hogy az üzleti logikát és adatokat az access összetevők önálló webalkalmazás belül.

Az alábbi ábra bemutatja, hogyan egy helyszíni eset és a felhő engedélyezni megoldás. Ebben az esetben helyezze az alkalmazás rétegek az Azure SQL-adatbázis adatainak Azure és az access egy virtuális gépen.

![Vegyes alkalmazás mintával](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728015.png)

Ha úgy dönt, hogy egy kombinált webes és az alkalmazás réteg megvalósítása Azure Web Apps alkalmazások használatával, azt javasoljuk, hogy őrizze meg a középső szintű vagy alkalmazás réteg, dinamikus csatolású kódtár () a webalkalmazás környezetében.

Ezenkívül olvassa el a javaslatok, ha többet szeretne tudni a technikákat programozási Ez a cikk végén [Comparing webes fejlesztési stratégiák Azure-ban](#comparing-web-development-strategies-in-azure) szakaszában megadott.

## <a name="n-tier-hybrid-application-pattern"></a>N szintű hibrid alkalmazás mintával

Az n szintű hibrid alkalmazás mintát az alkalmazás között a helyszíni és Azure kanadai többszintű hajtja végre. Emiatt létrehozhat egy rugalmas, és újból felhasználható hibrid rendszer, amely módosíthatja, illetve egy adott réteg hozzáadása a többi rétegek módosítása nélkül. Ha ki szeretné terjeszteni a felhőbe a vállalati hálózathoz, [Azure virtuális hálózati](../virtual-network/virtual-networks-overview.md) szolgáltatást használhatja.

A hibrid alkalmazás mintát a következő esetekben hasznos:

- Szeretné, hogy futtassa a részben a felhőben, és a részben a helyszíni alkalmazásait.

- Egy meglévő helyszíni alkalmazást a felhőalapú néhány vagy összes elemei át szeretné.

- Szeretne áttérni vállalati alkalmazások helyszíni virtualizálva platformokon Azure.

- Saját maga igény felfelé és lefelé méretezheti infrastruktúra környezetben szeretne.

- Szeretne gyorsan kiépítése fejlesztés és tesztelje a környezetek rövid ideig.

- Azt szeretné, hogy az adatbázis-alkalmazások vállalati biztonsági másolatok készítése a tényleges költség lehetőséget.

Az alábbi ábra bemutatja, hogyan-n szintű hibrid alkalmazás minta, amely a helyszíni és Azure terjed ki. Ahogy a diagramot, a helyszíni infrastruktúrát [Active Directory tartományi szolgáltatások](https://technet.microsoft.com/library/hh831484.aspx) tartományvezérlőnek a felhasználói hitelesítés és hitelesítés támogatására. Figyelje meg, hogy az ábra bemutatja az esetben, ha az adatok réteg egyes részeinek élő egy helyszíni adatközpont, mivel az adatok réteg egyes részeinek live Azure-ban. Attól függően, hogy az alkalmazás igényeinek számos más hibrid esetben hajtja végre. Például, előfordulhat, hogy ne a bemutató réteg és a vállalati réteg helyszíni környezetben, de az adatok réteg Azure-ban.

![N szintű alkalmazás mintával](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728016.png)

Azure-is használhatja az Active Directory különálló felhő könyvtár a szervezet számára, vagy integrálhatja meglévő helyszíni Active Directory és az [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/). A diagram naplójában üzleti réteg összetevőinek hozzáférhet több adatforráshoz, például [SQL](virtual-machines-windows-sql-server-iaas-overview.md) Server Azure-ban egy helyszíni SQL Server [Azure virtuális hálózaton](../virtual-network/virtual-networks-overview.md)keresztül, illetve a .NET-keretrendszer adatokat szolgáltató technológiák használata [SQL-adatbázis](../sql-database/sql-database-technical-overview.md) IP-címet a magánjellegű belső keresztül. Ez a diagram Azure SQL-adatbázis-választható adatszolgáltatás.

N szintű hibrid alkalmazás mintát az alábbi munkafolyamatot meghatározott sorrendben is alkalmazhat:

1. Azonosítsa a vállalati adatbázis-alkalmazásokat, amely áthelyezi a [Microsoft értékelése és a tervezési (térkép) eszközkészlet](http://microsoft.com/map)cloud. A térkép eszközkészlet készlet és a teljesítmény adatait gyűjti össze a át virtualizációs tervezi számítógépek és javaslatokat tesz a beosztását, és a felmérés megtervezése.

1. Az erőforrások és a konfiguráció szükséges az Azure platform, például a tárterület-fiókok és a virtuális gépeken futó megtervezése.

1. A vállalati hálózathoz a helyszíni és [Azure virtuális hálózati](../virtual-network/virtual-networks-overview.md)hálózati kapcsolatának beállítása. A vállalati hálózathoz a helyszíni és az Azure virtuális gép közötti kapcsolat beállításához használja az alábbi két módszer közül:

    1. Közötti kapcsolat létrehozása a helyszíni és Azure virtuális gépen nyilvános végpontjait keresztül Azure-ban. Ez a módszer egy egyszerű telepítő, és lehetővé teszi, hogy az SQL Server-hitelesítést használ a virtuális gépen. Ezenkívül a hálózati biztonsági csoport szabályok beállítása szabályozhatja a virtuális nyilvános forgalmat. További tudnivalókért lásd: a [külső hozzáférés engedélyezése a virtuális az Azure portálon](virtual-machines-windows-nsg-quickstart-portal.md).

    1. Közötti kapcsolat létrehozása a helyszíni és Azure Azure virtuális személyes hálózathoz (VPN) alagutas keresztül. Ezzel a módszerrel, ha ki szeretné terjeszteni a tartomány házirendek az Azure virtuális géphez. Ezeken kívül tűzfalszabályokat beállítása és használata a Windows-hitelesítés a virtuális gépen. Azure jelenleg, biztonságos webhely VPN és webhely-pont virtuális Magánhálózati kapcsolat támogatja:

        - A biztonságos hely közötti kapcsolat hozható létre a hálózati kapcsolat között a helyszíni és a virtuális hálózat Azure-ban. A helyszíni adatok központ környezetben való csatlakozáshoz Azure ajánlott.

        - Biztonságos webhely pont-kapcsolaton keresztül a hálózati kapcsolat hozhat létre virtuális hálózatához Azure-ban és az egyes számítógépekre, tetszőleges futó között. Fejlesztési és tesztelési célú főleg ajánlott.

    Tájékoztatást arról, hogyan csatlakozhat az SQL Server Azure-ban olvassa el a [Csatlakozás az SQL Server virtuális gép a Azure](virtual-machines-windows-classic-sql-connect.md).

1. Állítsa be ütemezett feladatok és biztonsági mentése az Azure virtuális gép lemezen a helyszíni adatok az értesítéseket. További tudnivalókért lásd: az [SQL Server biztonsági mentése és visszaállítása az Azure Blob-tároló szolgáltatás](https://msdn.microsoft.com/library/jj919148.aspx) , és [biztonsági mentése és visszaállítása az Azure virtuális gépeken futó SQL Server](virtual-machines-windows-sql-backup-recovery.md).

1. Attól függően, hogy az alkalmazás igényeinek alkalmazhat, az alábbi három gyakori alkalmazási területek egyikét:

    1. Hagyja a webkiszolgálóra, alkalmazáskiszolgáló és nincs adatok egy adatbázis-kiszolgáló Azure-ban, mivel a bizalmas adatokat a helyszíni megtartani.

    1. Webkiszolgálón tárolhatja és alkalmazáskiszolgáló helyszíni mivel az adatbázis-kiszolgáló az Azure virtuális gépen.

    1. Az adatbázis-kiszolgáló, a webkiszolgáló tárolhatja, és a alkalmazáskiszolgáló helyszíni mivel az adatbázis-kópiák kordában az Azure virtuális gépeken futó. Ezzel a beállítással a helyszíni webkiszolgálón vagy az adatbázis-kópiák Azure-ban eléréséhez jelentéskészítő alkalmazásokban. Ezért érhet csökkentse a terhelést a helyszíni-adatbázisban. Azt javasoljuk, hogy nehéz az ebben az esetben alkalmazza olvassa el a munkaterhelésének és fejlesztési céljából. További információkat adatbázis-kópiák Azure-ban AlwaysOn elérhetősége csoport a [magas rendelkezésre állásának és a helyreállítás az Azure virtuális gépeken futó SQL Server](virtual-machines-windows-sql-high-availability-dr.md)megjelenítéséhez.

## <a name="comparing-web-development-strategies-in-azure"></a>Azure-ban a webes fejlesztési stratégiák összehasonlítása

Végrehajtása és a több szálon Azure-ban az SQL Server-alapú alkalmazás telepítéséhez, használhatja a következő két programozási lehetőségek közül választhat:

- Hagyományos webkiszolgálóra (IIS - Internet Information Services) beállítása az Azure virtuális gépeken futó SQL Server Azure és az access adatbázisban.

- Megvalósításához és az Azure egy felhőalapú szolgáltatás telepítése. Ellenőrizze, hogy a felhőalapú szolgáltatást Azure virtuális gépeken futó SQL Server-alapú adatbázisai hozzáférhet. Egy felhőalapú szolgáltatásba tartalmazhat több webhely és a dolgozó szerepkörök.

Az alábbi táblázat ismerteti a hagyományos webes fejlesztése Azure Cloud Services és részletez az Azure virtuális gépeken futó SQL Server Azure Web Apps alkalmazások összehasonlítása. Táblázat tartalmazza az Azure Web Apps alkalmazások szerint lehetséges használata SQL Server Azure virtuális adatforrásként az Azure-webalkalmazások nyilvános virtuális IP-cím vagy a DNS-nevét.

||Hagyományos webes fejlesztése az Azure virtuális gépeken futó|Cloud Services Azure-ban|Webes üzemeltetésű az Azure Web Apps alkalmazások|
|---|---|---|---|
|**Alkalmazás áttelepítése a helyszíni**|Meglévő alkalmazások, például-van.|Alkalmazások webes és dolgozó szerepkör szükséges.|Meglévő alkalmazások, például – de illeszkedik önálló webalkalmazások és rövid méretezhetőség igénylő webszolgáltatásokhoz.|
|**Fejlesztés és telepítéséhez**|Visual Studio, WebMatrix, vizuális Web Developer, WebDeploy, FTP, TFS, IIS-kezelőben PowerShell.|Visual Studio Azure SDK, TFS, PowerShell. Minden egyes felhőszolgáltatásba van a szolgáltatás csomag és a konfigurációs üzembe két környezetben: átmeneti és üzemi. Az átmeneti tárolásra szolgáló környezetbe kattintva tesztelje előtt gyártási előléptetés telepítheti a egy felhőalapú szolgáltatásba.|Visual Studio, WebMatrix, vizuális Web Developer, FTP, mely Számjegy, BitBucket, Codeplex webhelyen, a DropBox, GitHub, Mercurial, TFS, webhely üzembe helyezéséhez PowerShell.|
|**Felügyeleti és beállítása**|Ön a felelős felügyeleti feladatok az alkalmazás, adatok, tűzfalszabályokat, virtuális hálózati és operációs rendszer.|Ön a felelős az alkalmazás, adatok, tűzfalszabályokat és virtuális hálózati felügyeleti feladatok.|Ön a felelős az alkalmazás és a csak az adatokat a felügyeleti feladatok.|
|**Magas rendelkezésre állásának és a helyreállítás (HADR)**|Azt javasoljuk, hogy virtuális gépeken futó helyezze az elérhetőség mindazon és az ugyanazt a felhőalapú szolgáltatást. Ugyanaz a VMs megőrzési elérhetőségének beállítása lehetővé teszi, hogy a magas elérhetősége csomópontok helyezhet külön hibafa tartományok és frissítése a tartományok Azure. Hasonlóképpen ugyanazt a felhőalapú szolgáltatást a VMs megőrzési lehetővé teszi, hogy terheléselosztás, és VMs kommunikálni közvetlenül egymás belül az Azure adatközpont a helyi hálózaton keresztül.<br/><br/>Ön a felelős végrehajtása egy magas elérhetőségéről és az SQL Server katasztrófa helyreállítási megoldás az Azure virtuális gépeken futó bármely legrövidebb leállás elkerülése érdekében. Támogatott HADR technológiákkal olvassa el a [magas rendelkezésre állásának és a helyreállítás az Azure virtuális gépeken futó SQL Server](virtual-machines-windows-sql-high-availability-dr.md)című témakört.<br/><br/>Ön a felelős a biztonsági másolat készítése saját adatok és az alkalmazás.<br/><br/>Azure áthelyezheti a virtuális gépeken futó, ha az állomásgép az az Adatközpont hardver problémák miatt nem sikerül. Ezeken kívül lehet a virtuális gép tervezett állásidőt az állomásgép biztonsági vagy szoftver frissítésekor frissítéseket. Emiatt azt javasoljuk, hogy megmaradjanak a folyamatos rendelkezésre állásának minden alkalmazás réteg legalább két VMs. Azure nem nyújt SLA egyetlen virtuális géphez. További tudnivalókért lásd: [Azure tűrőképessége technikai útmutatást](../resiliency/resiliency-technical-guidance.md).|Azure kezeli az alapul szolgáló hardver vagy operációs rendszerrel szoftver eredő hibák. Azt javasoljuk, hogy a magas állásának az alkalmazás egy webhelyen vagy dolgozó szerepkör több példányon végrehajtása. Tudnivalók: [Cloud Services, a virtuális gépeken futó, és a virtuális hálózaton szinten szerződés feltételeit](http://www.microsoft.com/download/details.aspx?id=38427) , és [vészhelyreállítás és Azure-alkalmazásokhoz magas elérhetősége](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md)<br/><br/>Ön a felelős a biztonsági másolat készítése saját adatok és az alkalmazás.<br/><br/>Az SQL Server-adatbázis-Azure virtuális tartózkodó adatbázisok Ön felelős végrehajtása nagy rendelkezésre állásának és katasztrófa helyreállítási megoldást bármely legrövidebb leállás elkerülése érdekében. Támogatott HDAR technológiák az Azure virtuális gépeken futó SQL Server magas rendelkezésre állásának és a helyreállítás talál.<br/><br/>**Az SQL Server-adatbázis tükrözése**: Azure Cloud Services (webes/dolgozó szerepkörök) használata. Az SQL Server VMs és egy felhőalapú szolgáltatás project Azure virtuális hálózatához is lehet. Ha az SQL Server virtuális nem virtuális ugyanabba a hálózatba, és az SQL Server-példány irányítja a kapcsolati SQL Server Alias létrehozása szeretne. Ezeken kívül az alias neve egyeznie kell az SQL Server nevét.|Magas elérhetősége Azure dolgozó szerepkörök, Azure blob-tárolóhoz és Azure SQL-adatbázis öröklődik. Például az Azure tároló összes blob táblázat adatok, és a várakozási három replikáit tárolja. Egy időpontban Azure SQL-adatbázis továbbra is operációs rendszert futtató adatok három kópiák – egy elsődleges kópia és két másodlagos kópiája. További tudnivalókért lásd: [Azure-tárhely](https://azure.microsoft.com/documentation/services/storage/) és [Azure SQL-adatbázis](../sql-database/sql-database-technical-overview.md).<br/><br/>Amikor az SQL Server Azure virtuális adatforrásként az Azure webalkalmazások, ne feledje, hogy Azure Web Apps alkalmazások nem támogatja az Azure virtuális hálózat. Ez azt jelenti SQL Server VMs Azure-ban Azure Web Apps alkalmazások az összes kapcsolat haladjon végig virtuális gépeken futó nyilvános végpontjait. A magas rendelkezésre állásának és katasztrófa helyreállítási esetek bizonyos korlátozások okozhatnak. Például az ügyfélalkalmazás csatlakoztatása az SQL Server virtuális az adatbázis-tükrözéshez Azure Web Apps esetben nem tud csatlakozni az új elsődleges-kiszolgáló adatbázis-tükrözéshez igényel, állítsa be az Azure virtuális hálózati között az SQL Server host VMs Azure-ban. Ezért használata **SQL Server-adatbázis tükrözése** az Azure Web Apps alkalmazások nem támogatott jelenleg.<br/><br/>**Az SQL Server AlwaysOn rendelkezésre állási csoportok**: beállíthatja AlwaysOn rendelkezésre állási csoportok Azure Web Apps alkalmazások használata az SQL Server VMs Azure-ban. De kell beállítania AlwaysOn elérhetősége csoport figyelő és a kommunikáció irányítja a elsődleges replika nyilvános terheléselosztás végpontok keresztül.|
|**Idegen helyszíni kapcsolat**|Csatlakozás helyszíni Azure virtuális hálózati is használhatja.|Csatlakozás helyszíni Azure virtuális hálózati is használhatja.|Azure virtuális hálózati használata támogatott. További tudnivalókért olvassa el a [Web Apps alkalmazások virtuális hálózati integrációval](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/)című témakört.|
|**Méretezhetőség:**|Méretezés felfelé a virtuális gép méretének növelésével, vagy több lemez hozzáadása érhető el. Virtuális gép méretű kapcsolatos további tudnivalókért olvassa el a [Azure virtuális gép méretben](virtual-machines-windows-sizes.md)című témakört.<br/><br/>**Az adatbázis-kiszolgáló**: méretezési adatbázis particionáló technikákat és az SQL Server AlwaysOn rendelkezésre állási csoportok keresztül érhető el.<br/><br/>A nehéz olvasási munkaterhelésekből [AlwaysOn rendelkezésre állási csoportok](https://msdn.microsoft.com/library/hh510230.aspx) több másodlagos csomópontok, valamint az SQL Server-replikáció is használhatja.<br/><br/>Nehéz írási munkaterhelésekből alkalmazhat vízszintes particionáló adatok megadására alkalmazás skála meg több fizikai kiszolgálóin.<br/><br/>Alkalmazhat a méretezés kivételének ezenkívül [Függő útválasztási adatok SQL Server](https://technet.microsoft.com/library/cc966448.aspx)használatával. Az adatok függő Routing (DDR), kell a particionáló szerkezet végrehajtása az ügyfélalkalmazás, általában a vállalati réteg rétegben, és az adatbázis kérések több SQL Server-csomópontok irányítja. A vállalati réteg tartalmazza a hozzárendelések hogyan particionálva van-e az adatokat, és melyik csomópont adatokat tartalmazza.<br/><br/>Úgy méretezheti virtuális gépeken futó futó alkalmazást. További információért megtudhatja, [hogy miként méretezheti az alkalmazás](../cloud-services/cloud-services-how-to-scale.md).<br/><br/>**Fontos megjegyzés**: az **Automatikus méretezéssel** Azure-ban a funkcióval automatikusan növelheti vagy csökkentheti az alkalmazás által használt virtuális gépeken futó. Ez a szolgáltatás garantálja, hogy a végfelhasználói élmény nem befolyásolja negatív csúcs időszakokban és VMs állítsa le, amikor alacsony az igény szerinti. Javasoljuk, hogy nem be az Automatikus méretezéssel beállítás a felhőalapú szolgáltatás Ha SQL Server VMs tartalmazza. A hiba oka, hogy az Automatikus méretezéssel szolgáltatás lehetővé teszi-e a Azure virtuális gépen kapcsolja az, hogy virtuális processzorhasználata nagyobb, mint bizonyos küszöbértéket esetén, és kapcsolja ki a virtuális géphez, amikor a processzorhasználata kisebb, mint azt. Az Automatikus méretezéssel funkció akkor hasznos, az állapot nélküli alkalmazások, például webkiszolgálón hol bármely virtuális is kezelheti a terhelést a nélkül bármely előző állapotát mutató hivatkozásokat. Azonban az Automatikus méretezéssel funkció nem állapot-nyilvántartó alkalmazásokhoz, például SQL Server, ha csak egy példánya lehetővé teszi, hogy az adatbázis írás esetében hasznos lehet.|Méretezés telefonos több webhely és a dolgozó szerepkörök használatával érhető el. Virtuális gép méretű webes és dolgozó szerepkörhöz kapcsolatos további tudnivalókért olvassa el a [Méretben Cloud Services konfigurálása](../cloud-services/cloud-services-sizes-specs.md)című témakört.<br/><br/>**Cloud Services**használata esetén több szerepkörök feldolgozás terjesztése és is elérni az alkalmazás rugalmas méretezését adhatja meg. Minden egyes felhőszolgáltatásba tartalmaz egy vagy több webes szerepkörök és/vagy dolgozó szerepkörök, a saját alkalmazások fájljai és konfigurációs. Akkor skála felfelé egy felhőalapú szolgáltatásba növelésével (virtuális gépeken futó) szerepkör-példányok száma egy szerepkör rendszerbe és skála lefelé egy felhőalapú szolgáltatásba szerepkör-példányok száma csökkentésével. Részletes információkért lásd: [Azure végrehajtás modellek](../cloud-services/cloud-services-choose-me.md).<br/><br/>Méretezési [Cloud Services, a virtuális gépeken futó, és a virtuális hálózaton szinten szerződés feltételeit](http://www.microsoft.com/download/details.aspx?id=38427) és terheléselosztó beépített Azure magas elérhetősége támogatását keresztül érhető el.<br/><br/>A több szálon alkalmazáshoz azt javasoljuk, hogy csatlakozni webes/dolgozó szerepkörök alkalmazás adatbázis-kiszolgáló VMs Azure virtuális hálózaton keresztül. Azure emellett VMs ugyanazt a felhőalapú szolgáltatást, a terheléselosztás felhasználói kérések terjesztése át őket. Virtuális gépeken futó ily módon összekapcsolt kommunikálni közvetlenül egymás belül az Azure adatközpont a helyi hálózaton keresztül.<br/><br/>**Automatikus méretezéssel** beállíthatja az Azure klasszikus portálon, valamint az időbeosztás. További információért megtudhatja, [hogy miként méretezheti az alkalmazás](../cloud-services/cloud-services-how-to-scale.md).|**Felfelé és lefelé méretezni**: növelése és csökkentése a példány (virtuális), a webhely számára fenntartott méretét is.<br/><br/>Ki méretarány: a webhely további fenntartott példányok (VMs) is hozzáadhat.<br/><br/>**Automatikus méretezéssel** beállíthatja a portálon, valamint az időbeosztás. További információért tájékozódhat [skála Web Apps alkalmazások](../app-service-web/web-sites-scale.md).|

Választás az alábbi programozási módszerek a további tudnivalókért lásd: [Azure Web Apps alkalmazások, a Felhőszolgáltatások és a VMs: mikor érdemes használni, amely](../app-service-web/choose-web-site-cloud-service-vm.md).

## <a name="next-steps"></a>Következő lépések

Azure virtuális gépeken futó SQL Server operációs rendszert futtató a további tudnivalókért lásd: az [SQL Server Azure virtuális gépeken futó áttekintése](virtual-machines-windows-sql-server-iaas-overview.md).
