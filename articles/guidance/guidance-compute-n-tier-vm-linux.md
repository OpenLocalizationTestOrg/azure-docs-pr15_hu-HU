<properties
   pageTitle="Linux VMs Azure-N szintű architektúrájának forgalmi |} Microsoft Azure"
   description="Hogyan lehet a Microsoft Azure-N szintű architektúrájának Linux VMs futtatása."
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
   ms.date="10/20/2016"
   ms.author="mwasson"/>

# <a name="running-linux-vms-for-an-n-tier-architecture-on-azure"></a>Linux VMs Azure-N szintű architektúrájának forgalmi

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]


> [AZURE.SELECTOR]
- [Linux VMs Azure-N szintű architektúrájának forgalmi](guidance-compute-n-tier-vm-linux.md)
- [Windows VMs Azure-N szintű architektúrájának forgalmi](guidance-compute-n-tier-vm.md)

Ez a cikk Linux virtuális gépeken futó (VMs) futó igazolt tanácsok halmazának ismertet-N szintű architektúrájának az alkalmazás. Azt a cikk [a Azure több VMs futó]épül[multi-vm].

> [AZURE.NOTE] Azure van két különböző telepítési modellek: [Erőforrás-kezelő] [ resource-manager-overview] és klasszikus. Ez a cikk az erőforrás-kezelő Microsoft javasolja új telepítési használja.

## <a name="architecture-diagram"></a>Architektúra diagramja

N szintű architektúrákban változata van. Különbségek az esetek többségében, a fenti ajánlást alkalmazásában kerülni a Webhelyfiókok számít. Ez a cikk tartalma feltételezi, hogy egy tipikus 3 szintű webalkalmazás:

- **Webes réteg.** Bejövő HTTP-kérelmeket kezeli. Válaszok a visszaadott a réteg keresztül.

- **Üzleti réteg.** Eszközök üzleti folyamatok és más funkcionális logika a rendszer.

- **Adatbázis réteg.** Magas elérhetősége Apache Cassandra verzióval, állandó adatok tárolására szolgál.

> A Visio-dokumentum, amely tartalmazza a architektúra diagram letölthető a [Microsoft letöltőközpontból][visio-download]. Ez az ábra be van kapcsolva a "számítási - többoldalas réteg (Linux).

![[0]][0]

- **Elérhetőség beállítása.** Az [Elérhetőség beállítása] létrehozása[ azure-availability-sets] minden egyes első csoportba tartozó, és legalább két VMs minden réteg kiépítése. Ezt a megközelítést [SLA] elérhetősége eléréséhez szükséges[ vm-sla] VMs számára.

- **Alhálózat.** Hozzon létre külön az egyes réteg alhálózat. Adja meg a cím tartomány- és alhálózat maszk [CIDR] jelölés használatával. 

- **Terheléselosztóként.** Az [internetes terheléselosztó] használata[ load-balancer-external] bejövő internetes forgalmat a webes réteg, és egy [belső terheléselosztó] terjesztheti[ load-balancer-internal] terjesztheti a vállalati réteg a webes réteg a hálózati forgalmat.

- **Jumpbox**. Egy _jumpbox_, más néven [megerősített host]egy virtuális rendszergazdák csatlakozhat más VMs a hálózaton lévő. A jumpbox tartalmaz egy NSG, amely lehetővé teszi, hogy csak a whitelisted nyilvános IP-címek távoli forgalmat. A NSG kell engedélyezi a biztonságos rendszerhéj (SSH) forgalmat.

- A **Figyelés**című témakört. Figyelés például [Nagios], [Zabbix]vagy [Icinga] szoftver is betekintést, válaszidő virtuális üzemidőt és a rendszer általános állapotának. A felügyeleti szoftver telepítése egy virtuális, amely egy külön management alhálózat kerül.

- **NSGs**. [Hálózati biztonsági csoportok] használata[ nsg] (NSGs) szeretné korlátozni a VNet belül a hálózati forgalmának engedélyezésére. Például a 3-as szintű architektúrájának itt látható, kattintson az adatbázis réteg nem fogadja el az előtér, csak a a vállalati réteg és a kezelés alhálózat érkező forgalmat.

- **Apache Cassandra adatbázis**. Az adatok réteg magas állásáról, mivel a replikáció és feladatátvételi ismertetése

## <a name="recommendations"></a>Javaslatok

Azure felajánlja a különféle számos különböző erőforrásokat és erőforrástípus, így a hivatkozás architektúra lehet kiépítve számos különböző módon. A fenti ajánlást követő hivatkozás architektúra telepíteni egy Azure erőforrás-kezelő sablon nyújtott. Ha úgy dönt, hogy hozzon létre saját hivatkozás architektúra hajtsa végre az alábbi javaslatokat adott követelmény, hogy nem férnek el a ajánlást működött.

### <a name="vnet--subnets"></a>VNet / alhálózat

Amikor a VNet hoz létre, lefoglalhat elég címterület alhálózat lenne szüksége lesz. Adja meg a VNet cím tartomány- és alhálózat maszk [CIDR] jelölés használatával. Egy címterület, a normál [magánjellegű IP-címterületet]esik használata[private-ip-space], amelyeket 10.0.0.0/8, 172.16.0.0/12 és 192.168.0.0/16.

Abban az esetben, be kell állítania a VNet és a helyszíni hálózaton között az átjárók később, válassza a cím tartományon nem áll átfedésben a helyszíni hálózaton. Miután létrehozta a VNet, a cím tartomány nem módosítható.

Tervezze meg alhálózat szem előtt funkcionalitást és biztonsági követelményeknek. Az összes VMs ugyanabban a réteg vagy szerepkör kell mappába érkeznek ugyanahhoz az alhálózathoz, ami lehet biztonsági oszlopazonosító jobboldali. VNets és alhálózat tervezésével kapcsolatos további tudnivalókért lásd: a [terv és Azure virtuális hálózatok Tervező][plan-network].

Minden alhálózathoz adja meg a címterületet alhálózat CIDR formában. Például a "10.0.0.0/24" 256 IP-címek adattartomány hoz létre. (VMs használhatja az alábbiak 251; öt vannak fenntartva. Lásd: a [virtuális hálózati gyakran ismételt kérdések][vnet faq].) Ellenőrizze, hogy a címtartományokat keresztül alhálózathoz nem átfedésben.

### <a name="load-balancers"></a>Terheléselosztókkal

A külső terheléselosztó elosztja a webes réteg internetes forgalmat. Hozzon létre egy nyilvános IP-címet a terheléselosztó. [Az internetes terheléselosztó]létrehozása[lb-external-create].

A belső terheléselosztó elosztja a vállalati réteg a webes réteg a hálózati forgalmat. Adhat a terheléselosztó egy privát IP-címet, hozzon létre egy frontend IP-konfigurációja, és a vállalati réteg számára a alhálózat társítása. Lásd: az [első lépések megtételében egy belső terheléselosztó][lb-internal-create].

### <a name="cassandra"></a>Cassandra

Azt javasoljuk, hogy [DataStax vállalati] [ datastax] gyártási használatára, de a fenti ajánlást bármely Cassandra edition vonatkozik. Operációs rendszert futtató DataStax Azure-ban a további tudnivalókért lásd: [DataStax vállalati telepítési útmutató az Azure][cassandra-in-azure]. 

A VMs Cassandra fürt elhelyezése egy elérhetőségének beállítása meggyőződni arról, hogy a Cassandra replikák több hibafa tartományban vannak elosztva, és frissítse a tartomány. További információt a hibafa tartományok és a frissítési tartományok [kezelése]című cikkben talál a virtuális gépeken futó elérhetőségét[availability-sets-manage]. 

3 hibafa tartományok (legnagyobb) elérhetőségének beállítása egy beállítása. 

Egy elérhetőségének beállítása a 18 frissítési tartományok beállítása. Ezzel kap frissítési tartományok maximális száma, mint is továbbra is egyenletesen elosztott a hibafa tartományok.   

Állítsa be a csomópontok állvány-et módban. Hibafa-tartományok megfeleltetése állványon lévő a `cassandra-rackdc.properties` fájlt.

Egy terheléselosztó a fürt elé nem szükséges. Az ügyfél közvetlenül a fürt csatlakozik.

### <a name="jumpbox"></a>Jumpbox

Helyezze a jumpbox, ugyanazon a VNet, mint a többi VMs, de egy külön management alhálózat.

Hozzon létre egy [nyilvános IP-címet] a jumpbox.

A virtuális kis méret használata a jumpbox, például a szokásos A1.

Állítsa be a webes réteg, üzleti réteg, illetve adatbázis réteg alhálózat NSGs át az adatkezelési alhálózat felügyeleti (SSH) forgalmának engedélyezésére.

A jumpbox biztonságos, hozzon létre egy NSG, és alkalmazza a jumpbox alhálózathoz. Adja hozzá egy NSG szabályt, amely lehetővé teszi, hogy csak egy csoportjából whitelisted nyilvános IP-címek SSH kapcsolatok.

Az alhálózathoz vagy a jumpbox adaptert a NSG csatolható Ebben az esetben azt javasoljuk, csatolása a hálózati kártya, így SSH forgalom csak a jumpbox, hogy engedélyezve van, akkor is, ha más VMs vesz fel ugyanahhoz az alhálózathoz.

Nincs engedélyezve a SSH hozzáférés nyilvános internetes a VMs futó alkalmazás terhelését szeretné. Ehelyett ezek VMs minden SSH hozzáférést a jumpbox keresztül kell származnia. A rendszergazda jelentkezik be a jumpbox, és kattintson a a jumpbox a virtuális rögzít. A jumpbox lehetővé teszi, hogy SSH forgalom az internetről, de csak az ismert, whitelisted IP-címet.

## <a name="availability-considerations"></a>Elérhetőség kapcsolatos szempontok

Minden réteg vagy a virtuális szerepkör helyezni egy külön elérhetőségének beállítása. Nem üzembe VMs különböző rétegek az elérhetőség mindazon. 

Az adatbázis réteg a több VMs problémákat nem automatikusan lefordítása magas rendelkezésre adatbázis. Relációs adatbázis esetén általában szüksége lesz replikációs és feladatátvételi használandó magas rendelkezésre állás érdekében.  

Ha szüksége van-e a [VMs az Azure SZOLGÁLTATÁSISZINT] -nál magasabb rendelkezésre állás[ vm-sla] tartalmaz, az alkalmazás bizonyos végig a két régió és Azure forgalom kezelővel feladatátvételi. További tudnivalókért lásd: az [Operációs rendszert futtató Linux VMs magas elérhetőség több régióban][multi-dc].  

## <a name="security-considerations"></a>Biztonsági megfontolások

NSG szabályok használatával korlátozása forgalom rétegek között. Például a 3-as szintű architektúrájának fent látható, az a webes réteg nem közvetlenül kommunikálni az adatbázis réteg. Ez a hivatkozási, az adatbázis réteg kell tiltsa le az a webes réteg alhálózat érkező forgalmat.  

  1. Hozzon létre egy NSG, és azt az adatbázist réteg alhálózathoz rendelheti.

  2. Adja hozzá egy szabályt, amely letiltja az összes bejövő forgalmat a VNet a. (Használata a `VIRTUAL_NETWORK` a szabály címkére.) 

  3. Adja hozzá egy szabályt, amely lehetővé teszi a vállalati réteg alhálózat bejövő forgalom magasabb prioritású. Ez a szabály felülbírálja az előző szabály, és lehetővé teszi, hogy az adatbázis réteg felvegye a vállalati réteg.

  4. Adja hozzá egy szabályt, amely lehetővé teszi, hogy a bejövő eső maga az adatbázis réteg alhálózat érkező forgalmat. Ez a szabály lehetővé teszi, hogy az adatbázis réteg, amelyre szükség van az adatbázis-replikáció és feladatátvevő VMs közötti kommunikációt.

  5. Adja hozzá egy szabályt, amely lehetővé teszi, hogy a jumpbox alhálózat SSH forgalmat. Ez a szabály lehetővé teszi, hogy az adatbázis réteg kapcsolódás a jumpbox a rendszergazdák.

  > [AZURE.NOTE] Egy NSG rendelkezik [alapértelmezett szabályok] [ nsg-rules] , amely engedélyezi a VNet belül minden bejövő forgalmat. Szabályok nem törölhető, de felülbírálhatja őket magasabb prioritású szabályokat hoz létre.

Fontolja meg egy hálózati virtuális készülék (NVA) hozhat létre egy nyilvános internetkapcsolat a hálózat és közötti Azure virtuális DMZ. NVA egy virtuális készülék, például tűzfal, csomag vizsgálati, a naplózás, egyéni útválasztás vagy egyéb műveleteket számos hálózati kapcsolódó tevékenységeket hajthatják végre az általános kifejezés. További tudnivalókért lásd: a [végrehajtási Azure és az Internet között a DMZ][dmz].

## <a name="scalability-considerations"></a>Méretezhetőség kapcsolatos szempontok

A terheléselosztókkal terjesztése a webes és üzleti rétegek hálózati forgalmat. Méretezze át vízszintesen új virtuális példányok hozzáadásával. Figyelje meg, hogy méretezheti a webes és üzleti rétegek független, a betöltés alapján. Szükséges a ügyfélaffinitás karbantartása okozta esetleges komplikációk csökkentéséhez a VMs a webes réteg állapot nélküli kell lennie. Az üzleti logikai funkcióinak szolgáltatója VMs is állapot nélküli kell lennie.

## <a name="manageability-considerations"></a>Kezelhetőség kapcsolatos szempontok

A teljes rendszerben kezelésének egyszerűsítése a központi felügyeleti eszközök, például az [Automatizálás Azure][azure-administration], [Microsoft műveletek Management Suite][operations-management-suite], [tölthetők le][chef], vagy [Puppet][puppet]. Ezeket az eszközöket is összevonhatja az, hogy a rendszer átfogó képet ad több VMs rögzített diagnosztikai és állapot információkat.

## <a name="solution-deployment"></a>Megoldás üzembe helyezése

A hivatkozás-architektúra, amely a fenti ajánlást egy telepítésének érhető el a [Github][github-folder]. A hivatkozás architektúra egy webes réteg, üzleti réteg és egy adatok réteg tartalmazza.

1. Kattintson a gombra.  
[! ["Telepítéséhez Azure"] [1]][2]

2. Miután a hivatkozásra az Azure-portálon megnyílt, írja be a következő értékeket: 
  - Az **erőforráscsoport** neve már van megadva a paraméter fájlt, így válassza az **Új létrehozása** , és adja meg `ra-ntier-sql-network-rg` a szövegmezőbe.
  - A **hely** legördülő listából válassza ki a régió.
  - Ne módosítsa a **Sablon legfelső szintű Uri** vagy a **Paraméter legfelső szintű Uri** mezőbe.
  - Tekintse át a szerződési feltételeket, majd jelölje be az **elfogadom a fenti feltételek** jelölőnégyzetet.
  - Kattintson a **vásárlás** gombra.

3. Jelölje be az üzenet a telepítési Azure portál bejelentést.

4. A paraméter fájloknak egy csomagolásukkor rendszergazda felhasználóneveket és jelszavakat, és ajánlott, hogy azonnal módosítsa az összes VMs mindkettőt. Kattintson a minden virtuális az Azure-portálon, majd kattintson a **jelszó alaphelyzetbe állítása** a **támogatási + hibaelhárítási** lap gombra. Jelölje be a **jelszó alaphelyzetbe állítása** **üzemmód** legördülő mezőben, majd jelölje be az új **felhasználó nevét** és **jelszavát**. A **frissítés** gombra kattintva továbbra is fennáll, az új felhasználó nevét és jelszavát.

## <a name="next-steps"></a>Következő lépések

Magas elérhetőségének e hivatkozás architektúra elérése érdekében azt javasoljuk, [több régiók bevezetéshez][multi-dc].

<!-- links -->

[azure-administration]: ../automation/automation-intro.md
[azure-availability-sets]: ../virtual-machines/virtual-machines-windows-manage-availability.md#configure-each-application-tier-into-separate-availability-sets
[availability-sets-manage]: ../virtual-machines/virtual-machines-windows-manage-availability.md
[megerősített host]: https://en.wikipedia.org/wiki/Bastion_host
[cassandra-consistency]: http://docs.datastax.com/en/cassandra/2.0/cassandra/dml/dml_config_consistency_c.html
[cassandra-consistency-usage]: http://medium.com/@foundev/cassandra-how-many-nodes-are-talked-to-with-quorum-also-should-i-use-it-98074e75d7d5#.b4pb4alb2
[cassandra-in-azure]: https://docs.datastax.com/en/datastax_enterprise/4.5/datastax_enterprise/install/installAzure.html
[cassandra-replication]: http://www.planetcassandra.org/data-replication-in-nosql-databases-explained/
[CIDR]: https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing
[chef]: https://www.chef.io/solutions/azure/
[datastax]: http://www.datastax.com/products/datastax-enterprise
[dmz]: guidance-iaas-ra-secure-vnet-dmz.md
[github-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier
[lb-external-create]: ../load-balancer/load-balancer-get-started-internet-portal.md
[lb-internal-create]: ../load-balancer/load-balancer-get-started-ilb-arm-portal.md
[load-balancer-external]: ../load-balancer/load-balancer-internet-overview.md
[load-balancer-internal]: ../load-balancer/load-balancer-internal-overview.md
[multi-dc]: guidance-compute-multiple-datacenters-linux.md
[multi-vm]: guidance-compute-multi-vm.md
[naming conventions]: guidance-naming-conventions.md
[nsg]: ../virtual-network/virtual-networks-nsg.md
[nsg-rules]: ../best-practices-resource-manager-security.md#network-security-groups
[operations-management-suite]: https://www.microsoft.com/en-us/server-cloud/operations-management-suite/overview.aspx
[plan-network]: ../virtual-network/virtual-network-vnet-plan-design-arm.md
[private-ip-space]: https://en.wikipedia.org/wiki/Private_network#Private_IPv4_address_spaces
[nyilvános IP-cím]: ../virtual-network/virtual-network-ip-addresses-overview-arm.md
[puppet]: https://puppetlabs.com/blog/managing-azure-virtual-machines-puppet
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[vm-sla]: https://azure.microsoft.com/en-us/support/legal/sla/virtual-machines
[vnet faq]: ../virtual-network/virtual-networks-faq.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[Nagios]: https://www.nagios.org/
[Zabbix]: http://www.zabbix.com/
[Icinga]: http://www.icinga.org/
[0]: ./media/blueprints/compute-n-tier-linux.png "Használja a Microsoft Azure N szintű architektúrájának"
[1]: ./media/blueprints/deploybutton.png 
[2]: https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-n-tier%2Fazuredeploy.json


