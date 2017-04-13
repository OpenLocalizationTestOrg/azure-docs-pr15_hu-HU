<properties
   pageTitle="Nyilvános és titkos IP-címek az Azure erőforrás-kezelő |} Microsoft Azure"
   description="Tudnivalók a nyilvános és titkos IP-címek az Azure erőforrás-kezelő"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/27/2016"
   ms.author="jdial" />

# <a name="ip-addresses-in-azure"></a>IP-címek Azure-ban
IP-címek rendelhet más Azure erőforrások, a helyszíni hálózaton és az internetes kommunikáció Azure erőforrásokat. Az IP-címek Azure-ban használható két típusa van:

- **Nyilvános IP-címek**: kommunikálni az interneten, beleértve az Azure nyilvánosan elérhető szolgáltatásokat is használható.
- **Magánjellegű IP-címek**: belül az Azure virtuális hálózati (VNet) kommunikációhoz használt, és a helyi hálózati kiterjesztése a hálózat Azure virtuális Magánhálózati átjáró vagy készült ExpressRoute áramkör használatakor.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][Klasszikus telepítési modell](virtual-network-ip-addresses-overview-classic.md).

Ha jártas a klasszikus telepítési modell, jelölje be a [IP-címek klasszikus közötti különbségeket és erőforrás-kezelő](virtual-network-ip-addresses-overview-classic.md#Differences-between-Resource-Manager-and-classic-deployments).

## <a name="public-ip-addresses"></a>Nyilvános IP-címek
Nyilvános IP-címek engedélyezése kommunikálni az internetes és Azure nyilvános szolgáltatásokból, például a [Azure vgx.dll gyorsítótár](https://azure.microsoft.com/services/cache/), [Azure esemény hubok](https://azure.microsoft.com/services/event-hubs/), [SQL-adatbázisok](../sql-database/sql-database-technical-overview.md)és [Azure tároló](../storage/storage-introduction.md)Azure erőforrásokat.

Az Azure erőforrás-kezelőben: egy [nyilvános IP-](resource-groups-networking.md#public-ip-address) cím saját tulajdonságait tartalmazó erőforrás. Egy nyilvános IP-cím erőforrás társíthatja az alábbi források közül:

- Virtuális gépeken futó (virtuális)
- Internetes terheléselosztókkal
- Virtuális Magánhálózati átjárók
- Alkalmazás átjárók

### <a name="allocation-method"></a>Felosztás módszer
Két módon, amelyben egy *nyilvános* IP-erőforrás - *dinamikus* vagy *statikus*IP-címének hozzárendelt. Az alapértelmezett terhelés módja *dinamikus*, ahol IP-címet a létrehozása időpontjában **nincs** kiosztva. Ehelyett a nyilvános IP-cím felosztása indításakor (vagy hozzon létre) a hozzárendelt erőforrás (például egy virtuális vagy tölt terheléselosztó). Az IP-cím leállítása (vagy törlésekor) az erőforrás van fejlesztésének. Ennek hatására az IP-cím módosításához, ha a Leállítás, és indítsa el az adott erőforrás.

Annak érdekében, hogy a hozzárendelt erőforrás IP-címének változatlan marad, beállíthatja, hogy a felosztás módszer kifejezetten *statikus*. Ebben az esetben az IP-cím azonnal van rendelve. Csak akkor, ha Ön az erőforrás törlése vagy a felosztás módot *dinamikus*megjelent.

>[AZURE.NOTE] Akkor is, ha a hozzárendelés módszer *statikus*értékre állítja, a tényleges IP-cím, a *nyilvános IP-erőforráshoz*hozzárendelt nem adhat meg. Ehelyett, kap kiosztott az Azure helyen jön létre az erőforrás elérhető IP-címek készletből.

Statikus nyilvános IP-címek gyakran fordulnak elő az alábbi esetekben:

- Végfelhasználók tűzfalszabályokat kommunikálni az Azure erőforrások frissítenie kell.
- A DNS-névfeloldás, ahol az IP-cím módosítás lenne szükség egy rekordok frissítését.
- Az Azure erőforrások más alkalmazásokba vagy a szolgáltatások, amelyek az IP-cím-alapú biztonsági modell kommunikálni.
- Az SSL-tanúsítványok csatolva IP-címet használhatja.

>[AZURE.NOTE] A lista, amelyből nyilvános IP-címek (dinamikus/statikus) rendelt erőforrások Azure IP-tartományokat [Azure adatközpont IP-címtartományai](https://www.microsoft.com/download/details.aspx?id=41653)van közzétéve.

### <a name="dns-hostname-resolution"></a>DNS-feloldási-hostname (állomásnév)
Megadhatja, hogy egy nyilvános IP-erőforrás, amely hoz létre az *domainnamelabel*megfeleltetés DNS tartomány neve címkét. *hely*. cloudapp.azure.com Azure kezelt DNS Servers nyilvános IP-címére. Például ha hoz létre egy nyilvános IP-erőforrás **contoso** megegyezik egy *domainnamelabel* a **Nyugati USA** -beli Azure *helyét*, a tartomány teljesen minősített neve (FQDN) **contoso.westus.cloudapp.azure.com** fog úgy, hogy az erőforrás nyilvános IP-címét. Mutasson az Azure nyilvános IP-cím egyéni tartomány CNAME rekord létrehozásához a FQDN is használhatja.

>[AZURE.IMPORTANT] Minden tartomány nevét a címke létrehozott belül az Azure helyre egyedieknek kell lenniük.  

### <a name="virtual-machines"></a>Virtuális gépeken futó
Egy nyilvános IP-címet a **hálózati kapcsolat**hozzárendelésével [Windows](../virtual-machines/virtual-machines-windows-about.md) vagy [Linux](../virtual-machines/virtual-machines-linux-about.md) virtuális társíthat. Ha a több elem hálózati kapcsolat virtuális hozzárendelheti a csak az *elsődleges* hálózati kapcsolat. Egy virtuális hozzárendelheti egy dinamikus vagy egy nyilvános statikus IP-címet.

### <a name="internet-facing-load-balancers"></a>Internetes terheléselosztókkal
Egy nyilvános IP-cím társíthat a [Azure betöltése terheléselosztó](../load-balancer/load-balancer-overview.md) **frontend** konfiguráció betöltése terheléselosztó hozzárendelésével. A nyilvános IP-címe a terheléselosztás – ezek olyan virtuális IP-címet (virtuális) szolgál. Előtér-terheléselosztó hozzárendelheti egy dinamikus vagy egy nyilvános statikus IP-címet. Több nyilvános IP-címeket is rendelhet egy terheléselosztó előtér, amely lehetővé teszi a [Többszörös-virtuális](../load-balancer/load-balancer-multivip.md) esetek, például egy több bérlői környezet SSL-alapú webhelyekhez.

### <a name="vpn-gateways"></a>Virtuális Magánhálózati átjárók
[Azure virtuális Magánhálózati átjáró](../vpn-gateway/vpn-gateway-about-vpngateways.md) segítségével kapcsolódhat az Azure virtuális hálózati (VNet), más Azure VNets vagy egy helyszíni hálózaton. Meg kell rendelnie egy nyilvános IP-cím lehetővé teszi, hogy a hálózat távoli kommunikálni az **IP-beállítások** . Jelenleg csak hozzárendelheti egy *dinamikus* nyilvános IP-cím egy virtuális Magánhálózati átjárót.

### <a name="application-gateways"></a>Alkalmazás átjárók
Egy nyilvános IP-cím társíthat Azure [Alkalmazás átjárót](../application-gateway/application-gateway-introduction.md), az átjáró **frontend** konfigurációs hozzárendelésével. A nyilvános IP-címe a terheléselosztás virtuális szolgál. Jelenleg csak hozzárendelheti egy *dinamikus* nyilvános IP-címet az átjáró frontend Alkalmazásbeállítások.

### <a name="at-a-glance"></a>A-a-adatábrázolás
Az alábbi táblázatban látható az adott tulajdonság keresztül, amely a nyilvános IP-cím társítható egy felső szintű erőforrást, és az esetleges terhelés módszerek (dinamikus vagy statikus) használható.

|Legfelső szintű erőforrás|A társítási IP-cím|Dinamikus|Statikus|
|---|---|---|---|
|Virtuális gépen|Hálózati kapcsolat|igen|igen|
|Terheléselosztó|Előtér konfigurálása|igen|igen|
|Virtuális Magánhálózati átjáró|Gateway IP-beállítások|igen|nem|
|Alkalmazás átjáró|Előtér konfigurálása|igen|nem|

## <a name="private-ip-addresses"></a>Magánjellegű IP-címek
Magánjellegű IP-címek engedélyezése Azure erőforrások más erőforrások: a [virtuális hálózati](virtual-networks-overview.md) vagy egy helyszíni hálózaton keresztül a virtuális Magánhálózati átjáró vagy készült ExpressRoute áramkör, kommunikálni az interneten érhető el IP-cím használata nélkül.

Az erőforrás-kezelő Azure telepítési modell egy privát IP-cím társítva a következő típusú Azure erőforrások:

- VMs
- Belső terheléselosztókkal (ILBs)
- Alkalmazás átjárók

### <a name="allocation-method"></a>Felosztás módszer
A magánjellegű IP-címet a cím tartományban, amely az erőforrás hozzá van rendelve alhálózat történik. A cím tartomány a alhálózat magát a VNet címtartományokat része.

Két módszer, amelyben egy személyes IP-cím oszlik: *dinamikus* vagy *statikus*. Az alapértelmezett terhelés módszer akkor *dinamikus*, ahol az IP-cím automatikus hozzárendelése az erőforrás-alhálózat (DHCP használata). Az IP-cím módosíthatja, ha leállítása, majd indítsa el az erőforrás.

Beállíthatja, hogy a felosztás módszer *statikus* IP-címe változatlan marad a biztosítása érdekében. Ebben az esetben is kell adnia egy érvényes IP-címet, az erőforrás-alhálózat részét képező.

Statikus saját IP-címek általában arra használják:

- Amely a használni kívánt tartományt vezérlők vagy a DNS-kiszolgálók VMs.
- IP-cím használatával tűzfalszabályokat igénylő erőforrások.
- Az erőforrás elérhető egyéb alkalmazások és az erőforrások által IP-címet.

### <a name="virtual-machines"></a>Virtuális gépeken futó
A magánjellegű IP-címet a **hálózati kapcsolat** [Windows](../virtual-machines/virtual-machines-windows-about.md) vagy [Linux](../virtual-machines/virtual-machines-linux-about.md) virtuális van rendelve. Abban az esetben, ha több hálózati kapcsolat virtuális minden kapcsolat kap egy privát IP-cím. Dinamikus, vagy a hálózati illesztő statikus a felosztás módszerrel is megadhat.

#### <a name="internal-dns-hostname-resolution-for-vms"></a>Belső DNS hostname (állomásnév) felbontását (VMs)
Az összes Azure VMs van beállítva [Azure kezelt DNS-kiszolgálók](virtual-networks-name-resolution-for-vms-and-role-instances.md#azure-provided-name-resolution) alapértelmezés szerint kivéve, ha kifejezetten konfigurálása egyéni DNS-kiszolgálók. Ezeket a DNS-kiszolgálók névfeloldás belső VMs, hogy az azonos VNet belül találhatók.

Amikor létrehoz egy virtuális, személyes IP-címéhez hostname (állomásnév) hozzárendelése bekerül az Azure kezelt DNS-kiszolgálók. Abban az esetben, ha több hálózati kapcsolat virtuális a hostname (állomásnév) a elsődleges hálózati kapcsolaton magánjellegű IP-címe van rendelve.

Azure kezelt DNS-kiszolgálók konfigurált VMs tudják az összes VMs azok VNet belül a állomásnevekké úgy, hogy a saját IP-címek.

### <a name="internal-load-balancers-ilb--application-gateways"></a>Belső terheléselosztókkal (ILB) és az alkalmazás átjárók
Egy privát IP-cím rendelhet az [Azure belső terheléselosztó](../load-balancer/load-balancer-internal-overview.md) (ILB) vagy egy [Azure alkalmazás átjáró](../application-gateway/application-gateway-introduction.md) **előtér** beállításait. A saját IP-cím belső zárólap, csak az erőforrások (VNet) virtuális hálózatán belül hozzáférhető szolgál, és a távoli hálózatok a VNet csatlakoztatva. Az előtér-konfiguráció oszthatnak vagy egy dinamikus vagy statikus magánjellegű IP-címet.

### <a name="at-a-glance"></a>A-a-adatábrázolás
Az alábbi táblázatban látható az adott tulajdonság keresztül, amely egy személyes IP-cím társítható egy felső szintű erőforrást, és az esetleges terhelés módszerek (dinamikus vagy statikus) használható.

|Legfelső szintű erőforrás|IP-cím szövetség|Dinamikus|Statikus|
|---|---|---|---|
|Virtuális gépen|Hálózati kapcsolat|igen|igen|
|Terheléselosztó|Előtér konfigurálása|igen|igen|
|Alkalmazás átjáró|Előtér konfigurálása|igen|igen|

## <a name="limits"></a>Korlátai

Az IP-címek kirótt korlátozások jelzi a skype_for_businesshoz [korlátozza a hálózati](azure-subscription-service-limits.md#networking-limits) Azure-ban. Ezek a korlátok olyan / régió és előfizetésenként. Azt is megteheti, hogy a [Kapcsolatfelvétel az ügyfélszolgálattal](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) növelése az alapértelmezett korlátokat, akár a vállalati igényei szerint maximális számával.

## <a name="pricing"></a>Árak

Előfordulhat, hogy nyilvános IP-címek a névleges díjat. IP cím árak Azure-ban kapcsolatos további információért tekintse át a [IP-cím árak](https://azure.microsoft.com/pricing/details/ip-addresses) lapot.

## <a name="next-steps"></a>Következő lépések
- [A virtuális egy statikus, nyilvános IP-, az Azure portálon terjesztése](virtual-network-deploy-static-pip-arm-portal.md)
- [Az egy statikus nyilvános IP-cím sablon használatával egy virtuális terjesztése](virtual-network-deploy-static-pip-arm-template.md)
- [A virtuális üzembe egy statikus magánjellegű IP-címet az Azure portál használatával](virtual-networks-static-private-ip-arm-pportal.md)
