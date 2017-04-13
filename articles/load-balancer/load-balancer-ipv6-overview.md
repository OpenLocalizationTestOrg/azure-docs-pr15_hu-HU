<properties
    pageTitle="Az IPv6 Azure terheléselosztó áttekintése |} Microsoft Azure"
    description="Az IPv6 támogatása az Azure betöltése terheléselosztó és terheléselosztás VMs ismertetése."
    services="load-balancer"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
    keywords="az IPv6, azure terheléselosztó, kettős Papírhalom, nyilvános ip, natív ipv6, Mobiltelefonról, iot"
/>
<tags
    ms.service="load-balancer"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/14/2016"
    ms.author="sewhee"
/>

# <a name="overview-of-ipv6-for-azure-load-balancer"></a>Az IPv6 Azure terheléselosztó áttekintése

Internetes terheléselosztókkal IPv6-címmel elvégezhető. Nemcsak a IPv4-kapcsolat, a szolgáltatás lehetővé teszi az alábbi funkciókat:

* Natív végpontok közötti IPv6-kapcsolatot nyilvános Internet-ügyfelek és Azure virtuális gépeken futó (VMs) között a terheléselosztó keresztül.
* Natív végpontok közötti IPv6 kimenő kapcsolatok VMs és nyilvános Internet IPv6-kompatibilis ügyfélprogramok között.

Az alábbi képen az IPv6-funkciók az Azure terheléselosztó mutatja be.

![Azure terheléselosztó és az IPv6](./media/load-balancer-ipv6-overview/load-balancer-ipv6.png)

Miután rendszerbe, egy IPv4- vagy az Internet IPv6-kompatibilis ügyfél kommunikálni tudjanak a nyilvános IPv4- vagy IPv6-címei (vagy állomásnevekké) a Azure internetes terheléselosztó. A terheléselosztó a VMs hálózati címfordítást (hálózati Címfordítást) használata a magánjellegű IPv6-címei az IPv6-csomagok irányítja. Az IPv6 internetes ügyfél nem lehet kommunikálni közvetlenül a VMs IPv6-címét.

## <a name="features"></a>Szolgáltatások

Via Azure erőforrás-kezelő rendszerbe VMs natív az IPv6 támogatása nyújtja:

1. Az IPv6-szolgáltatások terheléselosztás – az IPv6-ügyfelek az interneten
2. Natív IPv6 és IPv4 végpontjait VMs ("kettős halmozott")
3. A bejövő és kimenő kezdeményezésére natív IPv6-kapcsolat
4. Támogatott protokollok, például a TCP, UDP és HTTP (S) teljes körű funkciókészlettel szolgáltatási architektúrákban engedélyezése

## <a name="benefits"></a>Legfontosabb előny

Ez a funkció lehetővé teszi, hogy a következő kulcsok előnyöket nyújtja:

* Felel meg, hogy csak IPv6-ügyfelek számára hozzáférhető legyen-e új alkalmazások igénylő kormányzati szabályzatát
* Engedélyezés mobile és a dolog (IOT) fejlesztők kettős halmozott (IPv4 + IPv6) Azure virtuális gépeken futó használni a növekvő mobile és a IOT piacokon címet az Internet

## <a name="details-and-limitations"></a>Részletek és korlátai

Részletek

* Az Azure DNS-szolgáltatás IPv4 A és a IPv6 AAAA névrekordok tartalmaz, és válasza a terheléselosztó is rekordokat egy. Az ügyfél úgy dönt, hogy milyen cím (IPv4 vagy IPv6) is kommunikálhatnak.
* Amikor egy virtuális kezdeményez egy nyilvános Internet IPv6-csatlakoztatott eszközt kapcsolat, a a virtuális forrás IPv6-címek, hálózati címre lefordítani (hálózati Címfordítást) a terheléselosztó nyilvános IPv6-címét.
* A Linux operációs rendszert futtató VMs kell beállítania, hogy keresztül DHCP IPv6 IP-címét. Az Azure gyűjtemény Linux képek számos van már be van állítva az IPv6 támogatása módosítás nélkül. További tudnivalókért olvassa el a [Konfigurálása DHCPv6 Linux VMs az](load-balancer-ipv6-for-linux.md) című témakört.
* Ha úgy dönt, hogy egy állapot vizsgálati használata a terheléselosztó, hozzon létre egy IPv4-vizsgálati, és használja azt a IPv4 és az IPv6-végpontok. Ha a szolgáltatást a virtuális megszakad, forgatás a IPv4 és az IPv6-végpontokhoz nem veszi.

Korlátozások

* Az IPv6-terhelés terheléselosztó szabályok nem adhatja hozzá az Azure-portálon. A szabályok csak a sablon CLI, PowerShell keresztül hozhatók létre.
* Előfordulhat, hogy nem frissítése a meglévő VMs IPv6-címei használni. Új VMs kell telepítenie.
* Egy egyetlen IPv6 address az egyes virtuális egyetlen hálózat felhasználói felülete rendelhetők.
* A nyilvános IPv6-címei nem lehet egy virtuális rendelni. Is csak rendelni egy terheléselosztó.
* A fordított DNS-ben nem beállítása a nyilvános IPv6-címei.
* Az IPv6-os cím a VMs nem lehetnek tagjai az egy Azure Felhőszolgáltatásba. Ezeket az Azure virtuális hálózati (VNet) kapcsolhat össze és egymással a IPv4-címei fölé.
* Magánjellegű IPv6-címei erőforráscsoport az egyes VMs telepíthető, de nem telepíthető a erőforrás csoportba skála készletek keresztül.
* Azure VMs IPv6 más VMs, más Azure services vagy a helyszíni eszközökön keresztül nem tud csatlakozni. Csak kommunikálhatnak az Azure terheléselosztó IPv6 fölé. Jó helyen jár az alábbi más forrásokból IPv4 használatával tudnak kommunikálni.
* Egymást fedő kettős (IPv4 + IPv6) telepítések hálózati biztonsági csoport (NSG) védelem IPv4-használata támogatott. Az IPv6-végpontot NSGs nem vonatkoznak.
* Az IPv6-végpontot, kattintson a virtuális nincs kitéve közvetlenül az internethez. Egy terheléselosztó mögött van. Csak a megadott a betöltés terheléselosztó szabályokat portokat IPv6 keresztül elérhetők.
* Az IPv6-IdleTimeout paraméter módosítása **jelenleg nem támogatott**. Az alapértelmezett érték négy perc.

## <a name="next-steps"></a>Következő lépések

Megtudhatja, hogy miként egy terheléselosztó és az IPv6 telepítése.

* [Elérhetőség a régió szerint az IPv6](https://go.microsoft.com/fwlink/?linkid=828357)
* [Egy sablon használatával és az IPv6 terheléselosztó terjesztése](load-balancer-ipv6-internet-template.md)
* [Egy Azure PowerShell használatával és az IPv6 terheléselosztó terjesztése](load-balancer-ipv6-internet-ps.md)
* [Egy Azure CLI használatával és az IPv6 terheléselosztó terjesztése](load-balancer-ipv6-internet-cli.md)
