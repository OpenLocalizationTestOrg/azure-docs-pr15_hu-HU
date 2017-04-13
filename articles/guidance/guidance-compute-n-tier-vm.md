<properties
   pageTitle="Futtatja a Windows VMs-N szintű architektúrájának |} Architektúra hivatkozás |} Microsoft Azure"
   description="Hogyan lehet megvalósítani a több szálon architektúra Azure-elérhetőségét, biztonsági, méretezhetőség és kezelhetőség biztonsági kifizető külön figyelmet."
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/20/2016"
   ms.author="mwasson"/>

# <a name="running-windows-vms-for-an-n-tier-architecture-on-azure"></a>Windows VMs Azure-N szintű architektúrájának forgalmi

> [AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

> [AZURE.SELECTOR]
- [Linux VMs Azure-N szintű architektúrájának forgalmi](guidance-compute-n-tier-vm-linux.md)
- [Windows VMs Azure-N szintű architektúrájának forgalmi](guidance-compute-n-tier-vm.md)

Ez a cikk a Windows virtuális gépeken futó (VMs) futó igazolt tanácsok halmazának ismertet-N szintű architektúrájának az alkalmazás. Azt a cikk [a Azure több VMs futó]épül[multi-vm].

> [AZURE.NOTE] Azure van két különböző telepítési modellek: [Erőforrás-kezelő] [ resource-manager-overview] és klasszikus. Ez a cikk az erőforrás-kezelő Microsoft javasolja új telepítési használja.

## <a name="architecture-diagram"></a>Architektúra diagramja

N szintű architektúrákban változata van. Különbségek az esetek többségében, a fenti ajánlást alkalmazásában kerülni a Webhelyfiókok számít. Ez a cikk tartalma feltételezi, hogy egy tipikus 3 szintű webalkalmazás:

- **Webes réteg.** Bejövő HTTP-kérelmeket kezeli. Válaszok a visszaadott a réteg keresztül.

- **Üzleti réteg.** Eszközök üzleti folyamatok és más funkcionális logika a rendszer.

- **Adatbázis réteg.** Állandó adattárolás, [SQL Server mindig a elérhetősége csoportokon] keresztül biztosít[ sql-alwayson] magas elérhetőség.

> A Visio-dokumentum, amely tartalmazza a architektúra diagram letölthető a [Microsoft letöltőközpontból][visio-download]. Ez az ábra be van kapcsolva a "számítási - többoldalas réteg (Windows).

![[0]][0]

- **Elérhetőség beállítása.** Az [Elérhetőség beállítása] létrehozása[ azure-availability-sets] minden egyes első csoportba tartozó, és legalább két VMs minden réteg kiépítése. Ezt a megközelítést [SLA] elérhetősége eléréséhez szükséges[ vm-sla] VMs számára.

- **Alhálózat.** Hozzon létre külön az egyes réteg alhálózat. Adja meg a cím tartomány- és alhálózat maszk [CIDR] jelölés használatával. 

- **Terheléselosztóként.** Az [internetes terheléselosztó] használata[ load-balancer-external] bejövő internetes forgalmat a webes réteg, és egy [belső terheléselosztó] terjesztheti[ load-balancer-internal] terjesztheti a vállalati réteg a webes réteg a hálózati forgalmat.

- **Jumpbox**. Egy _jumpbox_, más néven [megerősített host]egy virtuális rendszergazdák csatlakozhat más VMs a hálózaton lévő. A jumpbox tartalmaz egy NSG, amely lehetővé teszi, hogy csak a whitelisted nyilvános IP-címek távoli forgalmat. A NSG lehetővé kell tennie, hogy a távoli asztal (RDP) forgalmat.

- A **Figyelés**című témakört. Figyelés például [Nagios], [Zabbix]vagy [Icinga] szoftver is betekintést, válaszidő virtuális üzemidőt és a rendszer általános állapotának. A felügyeleti szoftver telepítése egy virtuális, amely egy külön management alhálózat kerül.

- **NSGs**. [Hálózati biztonsági csoportok] használata[ nsg] (NSGs) szeretné korlátozni a VNet belül a hálózati forgalmának engedélyezésére. Például a 3-as szintű architektúrájának itt látható, kattintson az adatbázis réteg nem fogadja el az előtér, csak a a vállalati réteg és a kezelés alhálózat érkező forgalmat.

- **SQL Server mindig az elérhetőség csoportban.** Az adatok réteg magas állásáról, mivel a replikáció és feladatátvételi ismertetése

- **Az active Directory tartományi szolgáltatások (AD DS) kiszolgálók**. Active Directory tartományi szolgáltatások (AD DS) címtár-adatokat tárol, és a felhasználók és tartományok, például a felhasználó bejelentkezési folyamatot, a hitelesítési és a keresés Directoryban közötti kommunikáció kezeli. Az Active Directory tartományi vezérlő az Active Directory tartományi szolgáltatások operációs rendszert futtató kiszolgáló. A Windows Server 2016, előtt mindig a elérhetősége csoportok kell tartományhoz csatlakozik. Ennek az oka rendelkezésre állási csoportok Windows Server feladatátvevő fürt (WSFC) technológia függnek. A Windows Server 2016 az azt jelenti, hogy hozzon létre egy Feladatátvevőfürt, az Active Directory nem vezet be. További tudnivalókért lásd: [a feladatátvételét a Windows Server 2016 újdonságai][wsfc-whats-new]

## <a name="recommendations"></a>Javaslatok

Azure felajánlja a különféle számos különböző erőforrásokat és erőforrástípus, így a hivatkozás architektúra lehet kiépítve számos különböző módon. A fenti ajánlást követő hivatkozás architektúra telepíteni egy Azure erőforrás-kezelő sablon nyújtott. Ha úgy dönt, hogy hozzon létre saját hivatkozás architektúra hajtsa végre az alábbi javaslatokat adott követelmény, hogy nem férnek el a ajánlást működött.

### <a name="vnet--subnets"></a>VNet / alhálózat

Amikor a VNet hoz létre, lefoglalhat elég címterület alhálózat lenne szüksége lesz. Adja meg a VNet cím tartomány- és alhálózat maszk [CIDR] jelölés használatával. Egy címterület, a normál [magánjellegű IP-címterületet]esik használata[private-ip-space], amelyeket 10.0.0.0/8, 172.16.0.0/12 és 192.168.0.0/16.

Abban az esetben, be kell állítania a VNet és a helyszíni hálózaton között az átjárók később, válassza a cím tartományon nem áll átfedésben a helyszíni hálózaton. Miután létrehozta a VNet, a cím tartomány nem módosítható.

Tervezze meg alhálózat szem előtt funkcionalitást és biztonsági követelményeknek. Az összes VMs ugyanabban a réteg vagy szerepkör kell mappába érkeznek ugyanahhoz az alhálózathoz, ami lehet biztonsági oszlopazonosító jobboldali. VNets és alhálózat tervezésével kapcsolatos további tudnivalókért lásd: a [terv és Azure virtuális hálózatok Tervező][plan-network].

Minden alhálózathoz adja meg a címterületet alhálózat CIDR formában. Például a "10.0.0.0/24" 256 IP-címek adattartomány hoz létre. (VMs használhatja az alábbiak 251; öt vannak fenntartva. Lásd: a [virtuális hálózati gyakran ismételt kérdések][vnet faq].) Ellenőrizze, hogy a címtartományokat keresztül alhálózathoz nem átfedésben.

### <a name="network-security-groups"></a>Hálózati biztonsági csoportok

NSG szabályok használatával korlátozása forgalom rétegek között. Például a 3-as szintű architektúrájának fent látható, az a webes réteg nem közvetlenül kommunikálni az adatbázis réteg. Ez a hivatkozási, az adatbázis réteg kell tiltsa le az a webes réteg alhálózat érkező forgalmat.  

  1. Hozzon létre egy NSG, és azt az adatbázist réteg alhálózathoz rendelheti.

  2. Adja hozzá egy szabályt, amely letiltja az összes bejövő forgalmat a VNet a. (Használata a `VIRTUAL_NETWORK` a szabály címkére.) 

  3. Adja hozzá egy szabályt, amely lehetővé teszi a vállalati réteg alhálózat bejövő forgalom magasabb prioritású. Ez a szabály felülbírálja az előző szabály, és lehetővé teszi, hogy az adatbázis réteg felvegye a vállalati réteg.

  4. Adja hozzá egy szabályt, amely lehetővé teszi, hogy a bejövő eső maga az adatbázis réteg alhálózat érkező forgalmat. Ez a szabály lehetővé teszi, hogy az adatbázis réteg, amelyre szükség van az adatbázis-replikáció és feladatátvevő VMs közötti kommunikációt.

  5. Adja hozzá egy szabályt, amely lehetővé teszi, hogy a jumpbox alhálózat RDP-forgalmat. Ez a szabály lehetővé teszi, hogy az adatbázis réteg kapcsolódás a jumpbox a rendszergazdák.

  > [AZURE.NOTE] Egy NSG rendelkezik [alapértelmezett szabályok] [ nsg-rules] , amely engedélyezi a VNet belül minden bejövő forgalmat. Szabályok nem törölhető, de felülbírálhatja őket magasabb prioritású szabályokat hoz létre.

### <a name="load-balancers"></a>Terheléselosztókkal

A külső terheléselosztó elosztja a webes réteg internetes forgalmat. Hozzon létre egy nyilvános IP-címet a terheléselosztó. [Az internetes terheléselosztó]létrehozása[lb-external-create].

A belső terheléselosztó elosztja a vállalati réteg a webes réteg a hálózati forgalmat. Adhat a terheléselosztó egy privát IP-címet, hozzon létre egy frontend IP-konfigurációja, és a vállalati réteg számára a alhálózat társítása. Lásd: az [első lépések megtételében egy belső terheléselosztó][lb-internal-create].

### <a name="sql-server-always-on-availability-groups"></a>SQL Server mindig rendelkezésre állási csoportok

Azt javasoljuk, hogy [Mindig a elérhetősége csoportok] [ sql-alwayson] az SQL Server magas elérhetőség. Mindig a elérhetősége csoportok a tartományvezérlőnek szükség. Az elérhetőség csoportban csomópontjait AD tartománynevek kell lennie.

Az adatbázis- [elérhetősége csoport figyelő]keresztül csatlakozhat más rétegek[sql-alwayson-listeners]. A figyelő lehetővé teszi, hogy egy SQL ügyfélhez való csatlakozás nélkül arra a fizikai SQL Server-példány nevét. A VMs, hogy az adatbázis az access kell lennie a tartományhoz. Az ügyfél (jelen esetben egy másik) DNS-használja a figyelő virtuális hálózat nevét az IP-címek feloldásához.

Konfigurálja az SQL Server mindig a következőképpen:

- A Windows Server feladatátvevő fürtözés (WSFC) fürtre és az SQL Server mindig a elérhetősége csoport létrehozása. További tudnivalókért olvassa el az [Ismerkedés a mindig a elérhetősége csoportokat][sql-alwayson-getting-started].

- Hozzon létre egy belső terheléselosztó egy privát statikus IP-címet.

- Hozzon létre egy elérhetősége csoport figyelő, és a figyelő tartománynév hozzárendelése egy belső terheléselosztó IP-címét. 

- Az SQL Server listening port (TCP port 1433 alapértelmezés szerint) terhelés terheléselosztó szabály létrehozása. A betöltés terheléselosztó szabály engedélyeznie kell a _szövegen kívüli IP_, más néven közvetlen vissza. Ennek hatására a virtuális közvetlenül az ügyfél, amely lehetővé teszi az elsődleges replika közvetlen kapcsolat megválaszolása.

    > [AZURE.NOTE] Ha szövegen kívüli IP engedélyezve van, az előtér-portszámot ugyanaz, mint a háttéradatbázist portszámot a betöltés terheléselosztó szabály kell lennie.

Amikor egy SQL ügyfélhez csatlakozni próbál létrehozni, a terheléselosztó a csatlakozási kérelem az, hogy az aktuális elsődleges replika irányítja. Ha egy replikává áttérni, a terheléselosztó ezután valaki automatikusan az új elsődleges replika irányítja. További tudnivalókért lásd: a [Configure terheléselosztó SQL mindig a][sql-alwayson-ilb].

Meglévő ügyfélkapcsolatok egy feladatátvételkor be nem zár. A feladatátvételi befejeződése után az új kapcsolatokat átirányítja az új elsődleges replika.

Ha az alkalmazás készíti mint írások jelentősen további olvasása, kiürítése a másodlagos kópia írásvédett lekérdezések részét. Lásd: [figyelő használatával csatlakozhat egy írásvédett másodlagos (csak olvasható Útválasztás) replika][sql-alwayson-read-only-routing].

A telepítés tesztelése [kézi feladatátvevő]kényszeríti[sql-alwayson-force-failover].

### <a name="jumpbox"></a>Jumpbox

Nincs engedélyezve a RDP hozzáférés nyilvános internetes a VMs futó alkalmazás terhelését szeretné. Ehelyett ezek VMs minden RDP/SSH hozzáférést a jumpbox keresztül kell származnia. A rendszergazda jelentkezik be a jumpbox, és kattintson a a jumpbox a virtuális rögzít. A jumpbox az internetről, de csak az ismert, whitelisted IP-címek lehetővé teszi a RDP-forgalmat.

Helyezze a jumpbox, ugyanazon a VNet, mint a többi VMs, de egy külön management alhálózat.

Hozzon létre egy [nyilvános IP-címet] a jumpbox.

A virtuális kis méret használata a jumpbox, például a szokásos A1.

Állítsa be a webes réteg, üzleti réteg, illetve adatbázis réteg alhálózat NSGs át az adatkezelési alhálózat felügyeleti (RDP) forgalmának engedélyezésére.

A jumpbox biztonságos, hozzon létre egy NSG, és alkalmazza a jumpbox alhálózathoz. Egy NSG szabályt, amely lehetővé teszi, hogy csak egy csoportjából whitelisted nyilvános IP-címek RDP-kapcsolatok felvétele.

Az alhálózathoz vagy a jumpbox adaptert a NSG csatolható Ebben az esetben azt javasoljuk, csatolása a hálózati kártya, így RDP-forgalmat csak a jumpbox, hogy engedélyezve van, akkor is, ha más VMs vesz fel ugyanahhoz az alhálózathoz.

## <a name="availability-considerations"></a>Elérhetőség kapcsolatos szempontok

Minden réteg vagy a virtuális szerepkör helyezni egy külön elérhetőségének beállítása. Nem üzembe VMs különböző rétegek az elérhetőség mindazon. 

Az adatbázis réteg a több VMs problémákat nem automatikusan lefordítása magas rendelkezésre adatbázis. Relációs adatbázis esetén általában szüksége lesz replikációs és feladatátvételi használandó magas rendelkezésre állás érdekében. Az SQL Server, azt javasoljuk, [Mindig a elérhetősége csoportok]használatával[sql-alwayson]. 

Ha szüksége van-e a [VMs az Azure SZOLGÁLTATÁSISZINT] -nál magasabb rendelkezésre állás[ vm-sla] tartalmaz, az alkalmazás bizonyos végig a két régió és Azure forgalom kezelővel feladatátvételi. További tudnivalókért lásd: [Windows VMs operációs rendszert futtató magas elérhetőség több régióban][multi-dc].   

## <a name="security-considerations"></a>Biztonsági megfontolások

Titkosítsa a többi adatot. [Azure kulcs tárolóra] használata[ azure-key-vault] az adatbázis-titkosítás kulcsok kezeléséhez. Kulcs tárolóból elemre tárolhatja a titkosítási kulcs hardver biztonsági modulban (HSMs). További tudnivalókért lásd: [Konfigurálása Azure kulcs tárolóból elemre integráció az SQL Server Azure VMs] [ sql-keyvault] is ajánlott alkalmazás titkos kulcsok adatbázis-kapcsolatot karakterláncok, például tárolását kulcs tárolóból elemre.

Fontolja meg egy hálózati virtuális készülék (NVA) hozhat létre egy nyilvános internetkapcsolat a hálózat és közötti Azure virtuális DMZ. NVA egy virtuális készülék, például tűzfal, csomag vizsgálati, a naplózás, egyéni útválasztás vagy egyéb műveleteket számos hálózati kapcsolódó tevékenységeket hajthatják végre az általános kifejezés. További tudnivalókért lásd: a [végrehajtási Azure és az Internet között a DMZ][dmz].

## <a name="scalability-considerations"></a>Méretezhetőség kapcsolatos szempontok

A terheléselosztókkal terjesztése a webes és üzleti rétegek hálózati forgalmat. Méretezze át vízszintesen új virtuális példányok hozzáadásával. Figyelje meg, hogy méretezheti a webes és üzleti rétegek független, a betöltés alapján. Szükséges a ügyfélaffinitás karbantartása okozta esetleges komplikációk csökkentéséhez a VMs a webes réteg állapot nélküli kell lennie. Az üzleti logikai funkcióinak szolgáltatója VMs is állapot nélküli kell lennie.

## <a name="manageability-considerations"></a>Kezelhetőség kapcsolatos szempontok

A teljes rendszerben kezelésének egyszerűsítése a központi felügyeleti eszközök, például az [Automatizálás Azure][azure-administration], [Microsoft műveletek Management Suite][operations-management-suite], [tölthetők le][chef], vagy [Puppet][puppet]. Ezeket az eszközöket is összevonhatja az, hogy a rendszer átfogó képet ad több VMs rögzített diagnosztikai és állapot információkat.

## <a name="solution-deployment"></a>Megoldás üzembe helyezése

A hivatkozás-architektúra, amely a fenti ajánlást egy telepítésének érhető el a [Github][github-folder]. A hivatkozás architektúra egy webes réteg, üzleti réteg, egy adatok réteg, valamint jumpbox virtuális és az Active Directory tartományi vezérlők tartalmazza.

A hivatkozás architektúra az alábbi útmutatást követve telepítheti: 

1. Kattintson a gombra.  
[! ["Telepítéséhez Azure"] [1]][2]

2. Miután a hivatkozásra az Azure-portálon megnyílt, írja be a következő értékeket: 
  - Az **erőforráscsoport** neve már van megadva a paraméter fájlt, így válassza az **Új létrehozása** , és adja meg `ra-ntier-sql-network-rg` a szövegmezőbe.
  - A **hely** legördülő listából válassza ki a régió.
  - Ne módosítsa a **Sablon legfelső szintű Uri** vagy a **Paraméter legfelső szintű Uri** mezőbe.
  - Tekintse át a szerződési feltételeket, majd jelölje be az **elfogadom a fenti feltételek** jelölőnégyzetet.
  - Kattintson a **vásárlás** gombra.

3. Jelölje be az üzenet a telepítési Azure portál bejelentést.

4. Kattintson a gombra.  
[! ["Telepítéséhez Azure"] [1]][3]

5. Miután a hivatkozásra az Azure-portálon megnyílt, írja be a következő értékeket: 
  - Az **erőforráscsoport** neve már van megadva a paraméter fájlt, így válassza a **Meglévő használatát** , és adja meg `ra-ntier-sql-workload-rg` a szövegmezőbe.
  - A **hely** legördülő listából válassza ki a régió.
  - Ne módosítsa a **Sablon legfelső szintű Uri** vagy a **Paraméter legfelső szintű Uri** mezőbe.
  - Tekintse át a szerződési feltételeket, majd jelölje be az **elfogadom a fenti feltételek** jelölőnégyzetet.
  - Kattintson a **vásárlás** gombra.

6. Jelölje be az üzenet a telepítési Azure portál bejelentést.

7. Kattintson a gombra.  
[! ["Telepítéséhez Azure"] [1]][4]

8. Miután a hivatkozásra az Azure-portálon megnyílt, írja be a következő értékeket: 
  - Az **erőforráscsoport** neve már van megadva a paraméter fájlt, így válassza az **Új létrehozása** , és adja meg `ra-ntier-sql-network-rg` a szövegmezőbe.
  - A **hely** legördülő listából válassza ki a régió.
  - Ne módosítsa a **Sablon legfelső szintű Uri** vagy a **Paraméter legfelső szintű Uri** mezőbe.
  - Tekintse át a szerződési feltételeket, majd jelölje be az **elfogadom a fenti feltételek** jelölőnégyzetet.
  - Kattintson a **vásárlás** gombra.

9. Jelölje be az üzenet a telepítési Azure portál bejelentést.

10. A paraméter fájloknak egy csomagolásukkor rendszergazda felhasználóneveket és jelszavakat, és ajánlott, hogy azonnal módosítsa az összes VMs mindkettőt. Kattintson a minden virtuális az Azure-portálon, majd kattintson a **jelszó alaphelyzetbe állítása** a **támogatási + hibaelhárítási** lap gombra. Jelölje be a **jelszó alaphelyzetbe állítása** **üzemmód** legördülő mezőben, majd jelölje be az új **felhasználó nevét** és **jelszavát**. A **frissítés** gombra kattintva továbbra is fennáll, az új felhasználó nevét és jelszavát. 

További módszerek, a hivatkozás architektúra üzembe a további tudnivalókért lásd a [útmutatást-egyetlen-virtuális] fontos fájl[ github-folder] Github mappát. 

## <a name="next-steps"></a>Következő lépések

Magas elérhetőségének e hivatkozás architektúra elérése érdekében azt javasoljuk, [több régiók bevezetéshez][multi-dc].

<!-- links -->

[arm-templates]: https://azure.microsoft.com/documentation/articles/resource-group-authoring-templates/
[azure-administration]: ../automation/automation-intro.md
[azure-audit-logs]: ../resource-group-audit.md
[azure-availability-sets]: ../virtual-machines/virtual-machines-windows-manage-availability.md#configure-each-application-tier-into-separate-availability-sets
[azure-cli]: ../virtual-machines-command-line-tools.md
[azure-key-vault]: https://azure.microsoft.com/services/key-vault.md
[azure-load-balancer]: ../load-balancer/load-balancer-overview.md
[megerősített host]: https://en.wikipedia.org/wiki/Bastion_host
[CIDR]: https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing
[chef]: https://www.chef.io/solutions/azure/
[dmz]: guidance-iaas-ra-secure-vnet-dmz.md
[github-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier-sql
[lb-external-create]: ../load-balancer/load-balancer-get-started-internet-portal.md
[lb-internal-create]: ../load-balancer/load-balancer-get-started-ilb-arm-portal.md
[load-balancer-external]: ../load-balancer/load-balancer-internet-overview.md
[load-balancer-internal]: ../load-balancer/load-balancer-internal-overview.md
[multi-dc]: guidance-compute-multiple-datacenters.md
[multi-vm]: guidance-compute-multi-vm.md
[n-tier]: guidance-compute-n-tier-vm.md
[naming conventions]: guidance-naming-conventions.md
[nsg]: ../virtual-network/virtual-networks-nsg.md
[nsg-rules]: ../best-practices-resource-manager-security.md#network-security-groups
[operations-management-suite]: https://www.microsoft.com/en-us/server-cloud/operations-management-suite/overview.aspx
[plan-network]: ../virtual-network/virtual-network-vnet-plan-design-arm.md
[private-ip-space]: https://en.wikipedia.org/wiki/Private_network#Private_IPv4_address_spaces
[nyilvános IP-cím]: ../virtual-network/virtual-network-ip-addresses-overview-arm.md
[puppet]: https://puppetlabs.com/blog/managing-azure-virtual-machines-puppet
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[sql-alwayson]: https://msdn.microsoft.com/en-us/library/hh510230.aspx
[sql-alwayson-force-failover]: https://msdn.microsoft.com/en-us/library/ff877957.aspx
[sql-alwayson-getting-started]: https://msdn.microsoft.com/en-us/library/gg509118.aspx
[sql-alwayson-ilb]: https://blogs.msdn.microsoft.com/igorpag/2016/01/25/configure-an-ilb-listener-for-sql-server-alwayson-availability-groups-in-azure-arm/
[sql-alwayson-listeners]: https://msdn.microsoft.com/en-us/library/hh213417.aspx
[sql-alwayson-read-only-routing]: https://technet.microsoft.com/en-us/library/hh213417.aspx#ConnectToSecondary
[sql-keyvault]: ../virtual-machines/virtual-machines-windows-ps-sql-keyvault.md
[vm-planned-maintenance]: ../virtual-machines/virtual-machines-windows-planned-maintenance.md
[vm-sla]: https://azure.microsoft.com/en-us/support/legal/sla/virtual-machines
[vnet faq]: ../virtual-network/virtual-networks-faq.md
[wsfc-whats-new]: https://technet.microsoft.com/windows-server-docs/failover-clustering/whats-new-in-failover-clustering
[Nagios]: https://www.nagios.org/
[Zabbix]: http://www.zabbix.com/
[Icinga]: http://www.icinga.org/
[VM-sizes]: https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/
[solution-script]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/Deploy-ReferenceArchitecture.ps1
[solution-script-bash]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/deploy-reference-architecture.sh
[vnet-parameters-windows]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/windows/virtualNetwork.parameters.json
[vnet-parameters-linux]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/linux/virtualNetwork.parameters.json
[nsg-parameters-windows]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/windows/networkSecurityGroups.parameters.json
[nsg-parameters-linux]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/linux/networkSecurityGroups.parameters.json
[webtier-parameters-windows]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/windows/webTier.parameters.json
[webtier-parameters-linux]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/linux/webTier.parameters.json
[businesstier-parameters-windows]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/windows/businessTier.parameters.json
[businesstier-parameters-linux]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/linux/businessTier.parameters.json
[datatier-parameters-windows]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/windows/dataTier.parameters.json
[datatier-parameters-linux]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/linux/dataTier.parameters.json
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[0]: ./media/blueprints/compute-n-tier.png "Használja a Microsoft Azure N szintű architektúrájának"
[1]: ./media/blueprints/deploybutton.png 
[2]: https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-n-tier-sql%2FvirtualNetwork.azuredeploy.json
[3]: https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-n-tier-sql%2Fworkload.azuredeploy.json
[4]: https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-n-tier-sql%2Fsecurity.azuredeploy.json