<properties
   pageTitle="Nyilvános és titkos IP-címek (klasszikus) Azure-ban |} Microsoft Azure"
   description="Tudnivalók a nyilvános és titkos IP-címek a Azure-ban"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-service-management" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/11/2016"
   ms.author="jdial" />

# <a name="ip-addresses-classic-in-azure"></a>IP-címek (klasszikus) Azure-ban
IP-címek rendelhet más Azure erőforrások, a helyszíni hálózaton és az internetes kommunikáció Azure erőforrásokat. Az IP-címek Azure-ban használható két típusa van: nyilvános és titkos.

Nyilvános IP-címek segítségével kommunikálni az interneten, beleértve az Azure nyilvánosan elérhető szolgáltatásokat.

A hálózat kiterjesztése Azure virtuális Magánhálózati átjáró vagy készült ExpressRoute áramkör használatakor magán IP-címek segítségével kommunikációs az Azure virtuális hálózati (VNet), egy felhőalapú szolgáltatásba, és a helyszíni hálózaton belül.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Megtudhatja, miként [végezheti el ezeket a lépéseket az erőforrás-kezelő telepítési modellt használja](virtual-network-ip-addresses-overview-arm.md).

## <a name="public-ip-addresses"></a>Nyilvános IP-címek
Nyilvános IP-címek engedélyezése kommunikálni az internetes és Azure nyilvános szolgáltatásokból, például a [Azure vgx.dll gyorsítótár](https://azure.microsoft.com/services/cache/), [Azure esemény hubok](https://azure.microsoft.com/services/event-hubs/), [SQL-adatbázisok](../sql-database/sql-database-technical-overview.md)és [Azure tároló](../storage/storage-introduction.md)Azure erőforrásokat.

Egy nyilvános IP-cím társítva a következő típusú erőforrások:

- Felhőszolgáltatások
- IaaS virtuális gépeken futó (VMs)
- Szerepkör-példányok PaaS
- Virtuális Magánhálózati átjárók
- Alkalmazás átjárók

### <a name="allocation-method"></a>Felosztás módszer
Egy nyilvános IP-címet kell az Azure-erőforrás-e kiosztani, esetén *dinamikusan* elérhető nyilvános IP-címének a helyre, az erőforrás létrejön egy készletből kiosztva. IP-cím van fejlesztésének, amikor az erőforrás van állítva. Abban az esetben, ha egy felhőalapú szolgáltatásba, ez történik, ha a szerepkör összes előfordulását le vannak, amelyek elkerülhetők legyenek egy *statikus* (fenntartott) IP-cím használatával (lásd: a [Felhőszolgáltatások](#Cloud-services)).

>[AZURE.NOTE] IP-tartományokat, amelyből nyilvános IP-címek rendelt Azure erőforrások listája [Azure adatközpont IP-címtartományai](https://www.microsoft.com/download/details.aspx?id=41653)van közzétéve.

### <a name="dns-hostname-resolution"></a>DNS-feloldási-hostname (állomásnév)
Ha egy felhőalapú szolgáltatásba vagy egy IaaS virtuális hoz létre, meg kell adnia egy felhőalapú szolgáltatás DNS neve, vagyis egyedi Azure-ban összes erőforrás között. Ez a megfeleltetés hoz létre a Azure kezelt DNS-kiszolgálóiról *DNS_név*. cloudapp.net az erőforrás nyilvános IP-címére. Például ha egy felhőalapú szolgáltatás DNS-neve **contoso**hoz létre egy felhőalapú szolgáltatásba, a tartomány teljesen minősített neve (FQDN) **contoso.cloudapp.net** fog úgy, hogy egy nyilvános IP-címe (virtuális) a felhőalapú szolgáltatást. Mutasson az Azure nyilvános IP-cím egyéni tartomány CNAME rekord létrehozásához a FQDN is használhatja.

### <a name="cloud-services"></a>Felhőszolgáltatások
Egy felhőalapú szolgáltatásba mindig rendelkezik egy olyan virtuális IP-cím (virtuális) néven nyilvános IP-címet. Egy felhőalapú szolgáltatásba való társításához a virtuális VMs és szerepkör-példányok belül a felhőbeli szolgáltatástól belső portokra különböző portok a végpontokat is létrehozhat. 

Egy felhőalapú szolgáltatásba tartalmazhat több IaaS VMs vagy PaaS szerepkör-példányok, az összes elérhető a egy felhőalapú szolgáltatás virtuális keresztül. [Ha egy felhőalapú szolgáltatásba, több VIP](../load-balancer/load-balancer-multivip.md), amely lehetővé teszi például több bérlői környezet-SSL-alapú webhely többszörös-virtuális esetek is rendelhet.

Biztosíthatja, hogy egy felhőalapú szolgáltatásba nyilvános IP-címe változatlan marad, akkor is, ha a szerepkör-példányok leállítja, akkor a egy *statikus* nyilvános IP-cím használatával, [Fenntartott IP](virtual-networks-reserved-public-ip.md)néven. Hozzon létre egy (fenntartott) statikus IP-erőforrás egy adott helyen, és rendelje hozzá az adott helyen felhőalapú szolgáltatáshoz. Nem adhat meg a tényleges IP-címet a fenntartott IP-hez, a készlet létrehozása a helyen elérhető IP-címek hozzárendelése. Az IP-cím nem fejlesztésének, amíg explicit módon törölheti.

Statikus (fenntartott) nyilvános IP-címek gyakran fordulnak elő az jelenik meg, ha egy felhőalapú szolgáltatásba:

- végfelhasználók beállítás legyen tűzfalszabályokat szükséges.
- a külső DNS-névfeloldás, attól függ, és egy dinamikus IP lenne szükség egy rekordok frissítését.
- IP-alapú biztonsági modell használata külső webszolgáltatásokhoz fogyaszt.
- az SSL-tanúsítványok csatolva IP-címet használja.

>[AZURE.NOTE] A klasszikus virtuális létrehozásakor Azure, amely egy virtuális IP-cím (virtuális) mellett egy *felhőalapú szolgáltatásba* tároló hozza létre. A létrehozás portálon keresztül befejezése után alapértelmezett RDP vagy SSH *végpont* állítható be a portálon, a virtuális a felhőalapú szolgáltatás virtuális keresztül csatlakozhat. A felhőalapú szolgáltatás virtuális foglalható, amely hatékony nyújt a virtuális csatlakozhat egy fenntartott IP-címet. További portokat megnyithatja további végpontok konfigurálásával.

### <a name="iaas-vms-and-paas-role-instances"></a>Szerepkör-példányok IaaS VMs és PaaS
Hozzárendelheti egy nyilvános IP-címet közvetlenül egy [virtuális](../virtual-machines/virtual-machines-linux-about.md) IaaS vagy PaaS szerepkör példány belül egy felhőalapú szolgáltatásba. Ez egy példány szintű nyilvános IP-cím ([ILPIP](virtual-networks-instance-level-public-ip.md)) nevezik. Csak a nyilvános IP-cím lehet dinamikus.

>[AZURE.NOTE] Ez különbözik a virtuális, a felhőbeli szolgáltatástól, amely IaaS VMs vagy PaaS szerepkör-példányok, mivel az egy felhőalapú szolgáltatásba tartalmazhat több IaaS VMs vagy PaaS szerepkör-példányok, mind a egy felhőalapú szolgáltatás virtuális keresztül elérhetővé tett tároló.

### <a name="vpn-gateways"></a>Virtuális Magánhálózati átjárók
A [virtuális Magánhálózati átjárót](../vpn-gateway/vpn-gateway-about-vpngateways.md) egy Azure VNet csatlakozhat más Azure VNets vagy a helyszíni hálózatok használható. A virtuális Magánhálózati átjárót egy nyilvános IP cím *dinamikusan*, amely lehetővé teszi a hálózat távoli való kommunikáció hozzá van rendelve.

### <a name="application-gateways"></a>Alkalmazás átjárók
Az Azure [alkalmazás átjáró](../application-gateway/application-gateway-introduction.md) Layer7 terheléselosztási útvonal hálózati forgalmának engedélyezésére HTTP alapján történő használhatók. Alkalmazás átjáró egy nyilvános IP cím *dinamikusan*, amely a terheléselosztás virtuális az hozzá van rendelve.

### <a name="at-a-glance"></a>Áttekintése
Az alábbi táblázatban látható a lehetséges terhelés módszerek (dinamikus/statikus), és az azt jelenti, hogy több nyilvános IP-címek hozzárendelése az egyes erőforrás típusa.

|Erőforrás|Dinamikus|Statikus|Több IP-címek|
|---|---|---|---|
|Felhőalapú szolgáltatás|igen|igen|igen|
|Szerepkör-példány IaaS virtuális vagy PaaS|igen|nem|nem|
|Virtuális Magánhálózati átjáró|igen|nem|nem|
|Alkalmazás átjáró|igen|nem|nem|

## <a name="private-ip-addresses"></a>Magánjellegű IP-címek
Magánjellegű IP-címek Azure erőforrások, kommunikáció más erőforrások: egy felhőalapú szolgáltatásba vagy egy [virtuális hálózati](virtual-networks-overview.md)(VNet), illetve a helyszíni hálózathoz (VPN-átjáró vagy keresztül készült ExpressRoute áramkör), engedélyezze a Internet elérhető IP-címmel használata nélkül.

Azure klasszikus telepítési modellben egy privát IP-címet a következő Azure erőforrások rendelhetők:

- Szerepkör-példányok IaaS VMs és PaaS
- Belső terheléselosztó
- Alkalmazás átjáró

### <a name="iaas-vms-and-paas-role-instances"></a>Szerepkör-példányok IaaS VMs és PaaS
Virtuális gépeken futó (VMs) és a klasszikus telepítési modellt létrehozott egy felhőalapú szolgáltatásba, PaaS szerepkör példányaiban hasonló mindig kerülnek. Működése a saját IP-címek így hasonlóak az alábbi forrásokból.

Fontos tudni, hogy kétféle módon telepíthető-e egy felhőalapú szolgáltatásba:

- *Önálló* felhő szolgáltatásnak, ha még nem egy virtuális hálózaton belül.
- Virtuális hálózat részeként.

#### <a name="allocation-method"></a>Felosztás módszer
Abban az esetben, ha egy *önálló* felhőszolgáltatásba erőforrások kinyerése egy privát IP kiosztott cím *dinamikusan* magánjellegű Azure adatközponthoz-IP-címtartományokat. Csak az ugyanazt a felhőalapú szolgáltatást belül más VMs való kommunikáció használható. Az IP-cím módosíthatja, ha az erőforrás leállt és lépések.

Abban az esetben, ha egy felhőalapú szolgáltatásba, egy virtuális hálózaton belül van telepítve az erőforrások első a cím cellatartomány a társított subnet(s) (meghatározott a hálózati konfigurációja) kiosztott magánjellegű IP-címei. A magánjellegű IP-címei használható a VNet belül minden VMs közötti kommunikációt.

Ezenkívül abban az esetben, ha egy VNet belül felhőszolgáltatások egy privát IP-cím felosztása *dinamikusan* (DHCP használatával) alapértelmezés szerint. Ha az erőforrás leállt és lépések módosíthatja. Annak érdekében, hogy az IP-cím változatlan marad, meg kell *statikus*állítsa a felosztás módszert, és küldje el a megfelelő címet tartományon belül egy érvényes IP-címet.

Statikus saját IP-címek általában arra használják:

 - Amely a használni kívánt tartományt vezérlők vagy a DNS-kiszolgálók VMs.
 - IP-cím használatával tűzfalszabályokat igénylő VMs.
 - Egyéb alkalmazások IP-cím elérhető szolgáltatásokat futtató VMs.

#### <a name="internal-dns-hostname-resolution"></a>Belső DNS-hostname (állomásnév) feloldás
Az összes Azure VMs és PaaS szerepkör-példányok van beállítva az [Azure kezelt DNS-kiszolgálók](virtual-networks-name-resolution-for-vms-and-role-instances.md#azure-provided-name-resolution) alapértelmezés szerint kivéve, ha kifejezetten konfigurálása egyéni DNS-kiszolgálók. Ezeket a DNS-kiszolgálók nyújt a belső névfeloldás VMs szerepkör-példányok megjelenítő ugyanazt a VNet vagy felhőalapú szolgáltatást.

Amikor létrehoz egy virtuális, személyes IP-címéhez hostname (állomásnév) hozzárendelése bekerül az Azure kezelt DNS-kiszolgálók. Abban az esetben, ha a többszörös-hálózati virtuális a hostname (állomásnév) megfeleltetve az elsődleges adaptert magánjellegű IP-címe Azonban ennek a hozzárendelésnek a erőforrásainak ugyanazt a felhőalapú szolgáltatást vagy VNet korlátozódik.

Abban az esetben, ha egy *önálló* felhőszolgáltatásba megoldható állomásnevekké összes VMs/szerepkör-példány belül csak a azonos felhőalapú szolgáltatást fogja. Abban az esetben, ha egy felhőalapú szolgáltatásba belül egy VNet tudja oldani a VNet belül VMs/szerepkör-példányok állomásnevekké fogja.

### <a name="internal-load-balancers-ilb--application-gateways"></a>Belső terheléselosztókkal (ILB) és az alkalmazás átjárók
Egy privát IP-cím rendelhet az [Azure belső terheléselosztó](../load-balancer/load-balancer-internal-overview.md) (ILB) vagy egy [Azure alkalmazás átjáró](../application-gateway/application-gateway-introduction.md) **előtér** beállításait. A saját IP-cím belső zárólap, csak az erőforrások (VNet) virtuális hálózatán belül hozzáférhető szolgál, és a távoli hálózatok a VNet csatlakoztatva. Az előtér-konfiguráció oszthatnak vagy egy dinamikus vagy statikus magánjellegű IP-címet. Több saját IP-címek engedélyezése a többszörös-virtuális esetek is rendelhet.

### <a name="at-a-glance"></a>Áttekintése
Az alábbi táblázatban látható a lehetséges terhelés módszerek (dinamikus/statikus), és az azt jelenti, hogy több saját IP-címek hozzárendelése az egyes erőforrás típusa.

|Erőforrás|Dinamikus|Statikus|Több IP-címek|
|---|---|---|---|
|Virtuális (a felhőben *különálló* szolgáltatásként)|igen|igen|igen|
|Szerepkör-példány PaaS (a felhőben *különálló* szolgáltatásként)|igen|nem|igen|
|Virtuális vagy PaaS szerepkör példány (egy VNet)|igen|igen|igen|
|Belső betöltés terheléselosztó előtér|igen|igen|igen|
|Átjáró előtér-alkalmazás|igen|igen|igen|

## <a name="limits"></a>Korlátai

Az alábbi táblázatban látható előfizetésenként Azure-ban megcímezheti az IP-bevezetett korlátai. Azt is megteheti, hogy a [Kapcsolatfelvétel az ügyfélszolgálattal](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) növelése az alapértelmezett korlátokat, akár a vállalati igényei szerint maximális számával.

||Alapértelmezett korlát|Maximális érték|
|---|---|---|
|Nyilvános IP-címek (dinamikus)|5|Kapcsolatfelvétel az ügyfélszolgálattal|
|Fenntartott nyilvános IP-címek|20|Kapcsolatfelvétel az ügyfélszolgálattal|
|Nyilvános virtuális per telepítési (felhőalapú szolgáltatás)|5|Kapcsolatfelvétel az ügyfélszolgálattal|
|A magánjellegű a virtuális (ILB) a használati telepítési (felhőalapú szolgáltatás)|1|1|

Győződjön meg arról, olvassa el a skype_for_businesshoz [korlátozza a hálózati](azure-subscription-service-limits.md#networking-limits) Azure-ban.

## <a name="pricing"></a>Árak

A legtöbb esetben nyilvános IP-címek szabadon. A névleges díjat további és/vagy statikus nyilvános IP-címek használatára van. Győződjön meg arról, hogy megismeri a [nyilvános IP-címei struktúrájának árak](https://azure.microsoft.com/pricing/details/ip-addresses/).

## <a name="differences-between-resource-manager-and-classic-deployments"></a>Erőforrás-kezelő és klasszikus telepítések közötti különbségek
Az alábbi képen egy erőforrás-kezelő és a klasszikus telepítési modell IP címzésének szolgáltatásainak összehasonlítása.

||Erőforrás|Klasszikus|Erőforrás-kezelő|
|---|---|---|---|
|**Nyilvános IP-cím**|VIRTUÁLIS|A továbbiakban egy ILPIP (csak a dinamikus):|A továbbiakban olyan nyilvános IP (dinamikus vagy statikus):|
|||Egy IaaS virtuális vagy egy PaaS szerepkör-példány rendelt|A virtuális hálózati társított|
||Szemben lévő terheléselosztó Internet|A továbbiakban virtuális (dinamikus) vagy fenntartott IP (statikus):|A továbbiakban olyan nyilvános IP (dinamikus vagy statikus):|
|||Ha egy felhőalapú szolgáltatásba rendelt|A terheléselosztó előtér config társított|
||||
|**Magánjellegű IP-cím**|VIRTUÁLIS|A továbbiakban olyan DIP:|A magánjellegű IP-címet a továbbiakban|
|||Egy IaaS virtuális vagy egy PaaS szerepkör-példány rendelt|A virtuális hálózati rendelt|
||Belső terheléselosztó (ILB)|A ILB (dinamikus vagy statikus) rendelt|A ILB előtér konfigurációs (dinamikus vagy statikus) rendelt|

## <a name="next-steps"></a>Következő lépések
- [Egy privát statikus IP-cím virtuális Deploy](virtual-networks-static-private-ip-classic-pportal.md) a klasszikus portálon.
