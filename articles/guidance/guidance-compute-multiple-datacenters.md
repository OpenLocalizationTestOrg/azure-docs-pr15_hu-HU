<properties
   pageTitle="Futó Windows VMs magas elérhetőség több régióban |} Architektúra hivatkozás |} Microsoft Azure"
   description="A magas rendelkezésre állásának és tűrőképessége Azure a több régióban VMs telepítéséről."
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="roshar"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/21/2016"
   ms.author="mwasson"/>

# <a name="running-windows-vms-in-multiple-regions-for-high-availability"></a>Futó Windows VMs több régióban magas elérhetősége

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

> [AZURE.SELECTOR]
- [Több területre magas elérhetőség Linux VMs fut](guidance-compute-multiple-datacenters-linux.md)
- [Futó Windows VMs több régióban magas elérhetősége](guidance-compute-multiple-datacenters.md)

Ebben a cikkben egy sor olyan eljárások futtatásához a Windows virtuális gépeken futó (VMs) több Azure régióban elérhetőségét, és egy robusztus katasztrófa helyreállítási infrastruktúra elérése érdekében azt javasoljuk.

> [AZURE.NOTE] Azure van két különböző telepítési modellek: [Erőforrás-kezelő] [ resource groups] és klasszikus. Ez a cikk az erőforrás-kezelő Microsoft javasolja új telepítési használja.

A több elem régió architektúra megadhatja az egyetlen régióhoz telepítése, mint magasabb rendelkezésre állás. Ha egy területi üzemszünetek hatással van az elsődleges régió,-kezelővel [Forgalom] [ traffic-manager] átadni a másodlagos területére. A architektúra is segíthetnek, ha egy adott alrendszer az alkalmazás nem sikerül. 

Íme néhány magas elérhetősége különböző adatközpontokban eléréséhez több általános módszer:

- Aktív/passzív meleg készenléti együtt. Egy adott területre, miközben a többi vár készenléti forgalom ugrik. A másodlagos régióban VMs, hogy lefoglalt és futó mindig.

- Aktív/passzív hideg készenléti együtt. Az ugyanaz, de a másodlagos régióban VMs nem osztott mindaddig, amíg a feladatátvételi szükséges. Ezt a megközelítést költségek kisebb szeretné futtatni, de általában lesz már során hiba lefelé.

- Aktív. A két régió aktív, és kérés kerül közöttük. Ha egy adatközpont elérhetetlenné válik, ki a Forgatás származik. 

Ez a felépítés aktív/passzív a meleg készenléti forgalom kezelővel feladatátvételi koncentrál. Ne feledje, hogy sikerült kisszámú VMs meleg készenléti üzembe és szükség szerint majd méretezze.

## <a name="architecture-diagram"></a>Architektúra diagramja

A következő ábrán látható [Azure-N szintű architektúrája hozzáadása megbízhatóságát](guidance-compute-n-tier-vm.md)architektúráját épül.

> A Visio-dokumentum, amely tartalmazza a architektúra diagram letölthető a [Microsoft letöltőközpontból][visio-download]. Ez az ábra be van kapcsolva a "számítási - többoldalas régióban (Windows).

[! [0]][0]

- **Az elsődleges és másodlagos régiók**. Ez a felépítés használja a két régió magasabb rendelkezésre állás érdekében. Az elsődleges régió egyik. Normál működés közben az elsődleges régióra továbbítás hálózati forgalmának engedélyezésére. De, amely nem érhető el, ha forgalom van-e irányítva a másodlagos területére.

- ** [Azure forgalom Manager] [ traffic-manager] ** továbbítja az elsődleges régió bejövő felkérést. Régió nem érhető el, ha forgalom Manager átadja a másodlagos területére. További információ című [Forgalom Manager beállítása](#configuring-traffic-manager).

- **Erőforrás-csoportokat**. Hozzon létre külön [erőforráscsoport] [ resource groups] az elsődleges régió, a másodlagos régió és az forgalom parancsra. Ez a rugalmasságot biztosít kezelése az egyes régiókra erőforrások egyetlen gyűjteményei. Egy adott területre, például sikerült újratelepítése lefelé másikat vétele nélkül. [Hivatkozás: az erőforrás csoportokat][resource-group-links], hogy az alkalmazás az erőforrások listájának lekérdezést futtat.

- **VNets**. Hozzon létre egy külön VNet az egyes régiókra vonatkozó. Győződjön meg arról, hogy a cím szóközöket nem áll átfedésben. 

- **SQL Server mindig az elérhetőség csoportban**. Ha SQL-kiszolgálót használ, javasolt a [SQL mindig a Availabilty csoportok] [ sql-always-on] magas elérhetőség. Hozzon létre egy egyetlen elérhetőségét, amely tartalmazza az SQL Server-példányok a két régió. További információ [az SQL Server mindig a rendelkezésre állási csoport konfigurálása](#configuring-the-sql-server-alwayson-availability-group )című.

> [AZURE.NOTE] Is vegye figyelembe az [Azure SQL-adatbázis][azure-sql-db], amely tartalmaz egy felhőalapú szolgáltatásba, relációs adatbázisból. SQL-adatbázissal nem kell az elérhetőség csoport konfigurálása vagy feladatátvevő kezelése.  

- **Virtuális Magánhálózati átjárók**: [virtuális Magánhálózati átjáró] létrehozása[ vpn-gateway] az egyes VNet, és [VNet-VNet kapcsolat]beállítása[vnet-to-vnet], ahhoz, hogy a két VNets közötti hálózati forgalmat. Az SQL mindig a rendelkezésre állási csoport szükséges.

## <a name="recommendations"></a>Javaslatok

Azure felajánlja a különféle számos különböző erőforrásokat és erőforrástípus, így a hivatkozás architektúra lehet kiépítve számos különböző módon. A fenti ajánlást követő hivatkozás architektúra telepíteni egy Azure erőforrás-kezelő sablon nyújtott. Ha úgy dönt, hogy hozzon létre saját hivatkozás architektúra hajtsa végre az alábbi javaslatokat adott követelmény, hogy nem férnek el a ajánlást működött.

### <a name="regional-pairing"></a>Területi párosítása

Azure területenként ugyanazt a geography egy másik területéhez van megfeleltetni. Az általános válasszon régiók ugyanarra területi a két (például kelet-Amerikai Egyesült Államokban 2 és US központi). Ha így előnyei többek között:

- Egy széles üzemszünetek esetén minden pár ki legalább egy régió helyreállítási prioritása van.

- Azure rendszer tervezett frissítések történő közzétételének párosított régiók egymás után, lekicsinyítheti a lehető legrövidebb leállás.

- Párban belül az azonos földrajzi adatokat illetőségére követelményeknek találhatók.

Győződjön meg arról, hogy a két régió támogatja a minden alkalmazás szükséges Azure szolgáltatás ( [régió szerint]című témakör tartalmaz[services-by-region]). Területi pár további információkért lásd [üzleti folytonosságot és katasztrófa helyreállítási (BCDR): Azure párosított régiók][regional-pairs].

### <a name="traffic-manager-configuration"></a>Kezelő forgalom konfigurálása

Vegye figyelembe az alábbiakat forgalmat a tartalomkeresési manager az Ön esetében:

- **Továbbítás.** Adatforgalom Manager támogatja több [útválasztási algoritmusok][tm-routing]. A jelen cikkben ismertetett eset használja a _Prioritás_ routing (korábbi nevén _feladatátvevő_ Útválasztás). Ezzel a beállítással forgalom Manager összes kérést küld az elsődleges terület, kivéve ha az elsődleges régió válik nem érhető el. Ezen a ponton automatikusan sikertelen fölé a másodlagos területére. Lásd: [konfigurálása feladatátvevő útválasztási módszer][tm-configure-failover].

- **Állapot vizsgálati.** Adatforgalom-kezelő használja a HTTP (vagy HTTPS) [vizsgálati] [ tm-monitoring] figyelemmel az egyes régiókra elérhető. A vizsgálati ellenőrzi, hogy egy adott Elérési utat HTTP 200 választ. A legjobb zárólap, hogy az alkalmazás általános állapotának-jelentések létrehozása, és az állapot vizsgálati a végpont használni. Ellenkező esetben a vizsgálati jelenthet egy "megfelelő" végpontot, amikor az alkalmazás részei kritikus valójában nem működnek. További tudnivalókért lásd: [Állapot végpont figyelése mintát][health-endpoint-monitoring-pattern].   

Ha forgalom Manager nem sikerül fölé, nincs ügyfelek esetén nem tudják elérni az alkalmazás, amely lehet néhány percig, amíg ideje. Két tényező befolyásolja a teljes időtartama:

- Az állapot vizsgálati kell észleli, hogy az elsődleges adatközpont vált, nem érhető el.

- A DNS-kiszolgálók frissítenie kell az IP-címet, amely függ, hogy a DNS time-to-live (TTL) gyorsítótárazott DNS-rekordjait. Az alapértelmezett TTL (élettartam) 300 másodperc (5 perc), de ezt az értéket a forgalom Manager profil létrehozásakor beállíthatja.

A részletekért lásd: a [Kapcsolatos forgalom Manager figyelése][tm-monitoring]. 

Ha forgalom Manager átvétele, ajánlott elvégzéséhez a kézi visszaállás helyett automatikusan adatkapcsolat vissza. Győződjön meg róla, hogy az összes alkalmazás alrendszerek megfelelő először. Egyéb esetben hozhat létre olyan helyzet, ha az alkalmazás tükrözésekor másikba adatközpontokban között.

Alapértelmezés szerint forgalom kezelő automatikusan meghibásodik vissza. Ennek elkerülése érdekében manuálisan alsó az elsődleges régió prioritás egy feladatátvevő esemény után. Tegyük fel például, hogy az elsődleges régió 1-es prioritás, és a másodlagos prioritás 2. Után feladatátvevő állítsa az elsődleges régió prioritás 3, hogy az automatikus visszaállás. Ha készen áll a vissza váltani, frissítse a prioritás 1.

A következő Azure CLI parancs frissíti a prioritás:

```bat
azure network traffic-manager  endpoint set --resource-group <resource-group> --profile-name <profile>
    --name <traffic-manager-name> --type AzureEndpoints --priority 3
```    

Billenőkör elkerülése érdekében egy másik módja, ha ideiglenesen tiltsa le a végpont:

```bat
azure network traffic-manager  endpoint set --resource-group <resource-group> --profile-name <profile>
    --name <traffic-manager-name> --type AzureEndpoints --status Disabled
```

Attól függően, hogy a feladatátvevő probléma okát előfordulhat, hogy kell redploy az erőforrások terület belül. Előtt vissza nem működnek, hajtsa végre az működőképes állapot próba. Ellenőrizze, hogy a vizsgálat összetevőjét, például:

- VMs helyesen vannak beállítva. (Az összes szükséges szoftvert telepítve van, az IIS fut, stb.)

- Alkalmazás alrendszerek is megfelelő. 

- Funkcionális tesztelése. (Például a adatbázis réteg a érhetők el a webes réteg felosztással.)

### <a name="sql-server-always-on-configuration"></a>Az SQL Server mindig a konfiguráció

Elérhetőség az SQL Server mindig a csoportok a tartományvezérlőnek szükség. Az elérhetőség csoportban csomópontjait AD tartománynevek kell lennie. SQL Server mindig a elérhetősége csoport konfigurálása vonatkozó útmutatást az alábbiakat:

- Legalább két tartomány vezérlő helyezze egyes régiókra. 

- Adjon meg minden tartományvezérlőnek a statikus IP-címet.

- Ahhoz, hogy a VNets közötti kommunikáció VNet-VNet kapcsolat létrehozása.

- Minden egyes VNet adja hozzá a tartományt (a két régió) a DNS-kiszolgáló lista vezérlők IP-címét. A következő CLI parancsot is használhatja. További tudnivalókért lásd: [a virtuális hálózati (VNet) által használt Manage DNS-kiszolgálók][vnet-dns].

```bat
azure network vnet set --resource-group dc01-rg --name dc01-vnet --dns-servers "10.0.0.4,10.0.0.6,172.16.0.4,172.16.0.6"
```

- A [Windows Server feladatátvételét] létrehozása[ wsfc] , amely tartalmazza az SQL Server-példányok a két régióban (WSFC) fürt. 

- Hozzon létre egy SQL Server mindig a elérhetősége csoportot, amely tartalmazza az SQL Server-példányok az elsődleges és másodlagos régiókban. Lásd: [Bővítése mindig a elérhetősége csoport távoli Azure adatközponthoz (PowerShell)](https://blogs.msdn.microsoft.com/sqlcat/2014/09/22/extending-alwayson-availability-group-to-remote-azure-datacenter-powershell/) lépések. 

- Az elsődleges replika elhelyezése az elsődleges régió.

- Egy vagy több másodlagos kópiák elhelyezése az elsődleges régió. Állítsa be ezeket a szinkronizált jóváhagyás használata automatikusan átveszi.

- Egy vagy több másodlagos kópiák helyezi el a másodlagos régióban. Állítsa be ezeket a *aszinkron* jóváhagyás, használja a teljesítmény okok miatt. (Ellenkező esetben az összes SQL-tranzakciók kell várni a másodlagos régió a hálózaton keresztül egy üzenetváltási.) 

> [AZURE.NOTE] Aszinkron jóváhagyás kópiák nem teszik lehetővé automatikus. 

További tudnivalókért lásd: [Windows VMs fut az Azure-N szintű architektúrája](guidance-compute-n-tier-vm.md#SQL-AlwaysOn-Availability-Group).

## <a name="availability-considerations"></a>Elérhetőség kapcsolatos szempontok

Összetett N szintű alkalmazást nincs szükség lehet való replikáció a teljes alkalmazás, a másodlagos régióban. Csak Ehelyett, előfordulhat, hogy bizonyos kritikus alrendszer, amely támogatja az üzleti folytonosságot van szükség.

Adatforgalom kezelő egy esetleg nem sikerül a rendszer található. Ha nem sikerül a szolgáltatást, az ügyfelek nem tudnak hozzáférni az alkalmazás az állásidőt során. Tekintse át a [Forgalom Manager SLA][tm-sla], és meghatározzák, hogy e-kezelővel forgalom egyedül megfelel-e a vállalati magas elérhetőségét. Ha nem, akkor fontolja meg egy másik forgalom-kezelési megoldás, egy visszaállás. Ha nem sikerül az Azure-forgalmat kezelő szolgáltatás, módosítsa a CNAME rekordokat a DNS-ben, mutasson a többi forgalom alkalmazáskezelési szolgáltatás. (Ezt a lépést kell manuálisan elvégezni, és az alkalmazás nem lesz elérhető mindaddig, amíg a DNS-módosításokat vannak propagált.) 

Az SQL Server fürt forgatókönyv két feladatátvevő is célszerű figyelembe venni:

1. Az összes az elsődleges régió SQL kópiájába sikertelen lesz. Ha például ennek a regionális üzemszünetek során. Ebben az esetben meg kell manuálisan átadni a SQL elérhetősége csoport, annak ellenére, hogy a forgalmat Manager automatikus átvétele kattintson az előtér. Kövesse a [végezze el az SQL Server elérhetősége csoport kényszerített kézi feladatátvevő](https://msdn.microsoft.com/library/ff877957.aspx), amely ismerteti, hogyan lehet az SQL Server 2016-ban az SQL Server Management Studio eszközben, a Transact-SQL nyelvben vagy a PowerShell használatával végezze el a kényszerített feladatátvevő. 

    > [AZURE.WARNING] A kényszerített feladatátvételi az adatok elvesztését a kockázat van. Az elsődleges régió vissza online állapotban, pillanatkép készítése az adatbázist, és keresse meg a különbségeket [tablediff] használatával.

2. A másodlagos régió átvétele forgalom Manager, de az elsődleges SQL replika továbbra is használhatók. Ha például az előtér-réteg előfordulhat, hogy nem, az SQL-VMs megtartásával. Ebben az esetben internetforgalom továbbítás a másodlagos területére, és a régió továbbra is csatlakozhat az elsődleges SQL replika. Azonban lesznek nagyobb késés, mert az SQL-kapcsolatok különböző régiók megy. Ebben az esetben a következőképpen kell végezni kézi feladatátvevő: 

    - Ideiglenes váltson a másodlagos tartományban lévő SQL kópia *szinkron* jóváhagyás. Ezzel biztosíthatja, hogy nem lesznek az adatok elvesztését a feladatátvételkor.

    - Adott SQL replika átadni. 
    
    - Ha nem sikerül vissza az elsődleges régió, visszaállíthatja az aszinkron jóváhagyás beállítás. 

## <a name="manageability-considerations"></a>Kezelhetőség kapcsolatos szempontok

Amikor frissíti a üzembe, frissítse a egy régió egyszerre csökkentheti az esélye annak globális a hiba-helytelen konfigurációja vagy az alkalmazás hibát.

Tesztelje a tűrőképessége hibák a rendszer. Íme néhány gyakori hiba forgatókönyvek tesztelése:

- Állítsa le a virtuális példányok.

- Nyomása erőforrások, például a Processzor- és.

- A hálózat leválasztása/késleltetés.

- Folyamat összeomolhat.

- Tanúsítványok lejár.

- Hardveres hibák hasonlóan.

- Állítsa le a DNS-szolgáltatás, kattintson a tartomány-vezérlőket.

Mérje le a helyreállítási időpontját, és ellenőrizze a megfelelnek az üzleti igényeknek megfelelően alakíthatja. Tesztelje a hiba módok, valamint a kombinációja.

## <a name="next-steps"></a>Következő lépések

Sorozat tiszta felhő telepítések van összpontosít. Vállalati esetek gyakran szükség egy helyszíni hálózaton összekapcsolása az Azure virtuális hálózati hibrid hálózaton. Megtudhatja, hogyan hozhat létre a hibrid hálózaton, lásd: a [végrehajtási egy hibrid hálózat architektúrája Azure és a helyszíni VPN][hybrid-vpn].

<!-- Links -->

[azure-sla]: https://azure.microsoft.com/support/legal/sla/
[azure-sql-db]: https://azure.microsoft.com/en-us/documentation/services/sql-database/
[health-endpoint-monitoring-pattern]: https://msdn.microsoft.com/library/dn589789.aspx
[hybrid-vpn]: guidance-hybrid-network-vpn.md
[regional-pairs]: ../best-practices-availability-paired-regions.md
[resource groups]: ../azure-resource-manager/resource-group-overview.md
[resource-group-links]: ../resource-group-link-resources.md
[services-by-region]: https://azure.microsoft.com/en-us/regions/#services
[sql-always-on]: https://msdn.microsoft.com/en-us/library/hh510230.aspx
[tablediff]: https://msdn.microsoft.com/en-us/library/ms162843.aspx
[tm-configure-failover]: ../traffic-manager/traffic-manager-configure-failover-routing-method.md
[tm-monitoring]: ../traffic-manager/traffic-manager-monitoring.md
[tm-routing]: ../traffic-manager/traffic-manager-routing-methods.md
[tm-sla]: https://azure.microsoft.com/en-us/support/legal/sla/traffic-manager/v1_0/
[traffic-manager]: https://azure.microsoft.com/en-us/services/traffic-manager/
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vnet-dns]: ../virtual-network/virtual-networks-manage-dns-in-vnet.md
[vnet-to-vnet]: ../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md
[vpn-gateway]: ../vpn-gateway/vpn-gateway-about-vpngateways.md
[wsfc]: https://msdn.microsoft.com/en-us/library/hh270278.aspx
[0]: ./media/blueprints/compute-multi-dc.png "Azure N szintű alkalmazások könnyen hozzáférhető hálózati architektúra"