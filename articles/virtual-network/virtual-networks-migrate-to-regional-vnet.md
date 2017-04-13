<properties 
   pageTitle="Területi virtuális hálózathoz (VNet) affinitás csoportok áttelepítése"
   description="Megtudhatja, hogy miként területi vnets affinitás csoportok áttelepítése"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="how-to-migrate-from-affinity-groups-to-a-regional-virtual-network-vnet"></a>Területi virtuális hálózathoz (VNet) affinitás csoportok áttelepítése

Annak érdekében, hogy a affinitás csoporton belül létrehozva erőforrások fizikailag telepített beállított közel vannak egymáshoz, ezek az erőforrások gyorsabb kommunikáció engedélyezése egy affinitás csoport is használhatja. Korábban affinitás csoportokat hozhat létre virtuális hálózatok (VNets) követelmény volt. Ekkor a hálózati kezelő szolgáltatás VNets kezelt sikerült csak akkor működik a fizikai kiszolgálókon vagy időosztás egységet belül. Építészeti fejlesztések van nagyobb hálózatkezelő területére körét.

E építészeti fejlesztések eredményeként affinitás csoportok már nem ajánlott, illetve virtuális hálózatok szükséges. Affinitás csoportok használható VNets régiók helyettesíti. Régiók társított VNets területi VNets nevezik.

Ezenkívül azt javasoljuk, hogy Ön nem használja az affinitás csoportok általános. Azon a VNet megkövetelésének affinitás csoportok is voltak fontos használatával biztosítani erőforrások, például a számítási és tárolás, kerültek egymás mellett. Jó helyen jár az aktuális Azure hálózati architektúra elhelyezése feltétel már nem szükséges. Hol lehet használni kívánt affinitás alkalmazás néhány fennmaradó bizonyos esetekben [Affinitás csoportok és a VMs](#Affinity-groups-and-VMs) talál.

## <a name="creating-and-migrating-to-regional-vnets"></a>Létrehozásáról és a területi VNets áttelepítés

*Régió*fog előre, új VNets létrehozásakor, használja. Az adatkezelési portálon lehetőség megjelenik. Ne feledje, hogy a hálózati konfigurációs fájl ez látható *helyeként*.

>[AZURE.IMPORTANT] Továbbra is értelemben lehet létrehozni egy affinitás csoporthoz társított virtuális hálózatot, de nem indokolt lenyűgöző erre. Sok új funkcióval, például hálózati biztonsági csoportokat, csak érhetők el a regionális VNet használata esetén, és nem jelennek meg a virtuális hálózatok affinitás csoportok társított.

### <a name="about-vnets-currently-associated-with-affinity-groups"></a>Tudnivalók a jelenleg társított affinitás csoportok VNets

Társított jelenleg affinitás csoportok VNets engedélyezett a regionális VNets áttelepítést. A regionális VNet áttelepítéséhez, kövesse az alábbi lépéseket:

1. A hálózati konfigurációs fájl exportálása A PowerShell és az adatkezelési portálon is használhatja. Az adatkezelési portál használatával című témakör nyújt tájékoztatást [konfigurálása a VNet hálózati konfigurációs fájl használatával](virtual-networks-using-network-configuration-file.md).

1. Az új értékeket a régi értékek cseréje a hálózati konfigurációs fájl szerkesztése. 

    > [AZURE.NOTE] A **hely** az régiót, amelynek van társítva a VNet affinitás csoportról megadott. Például ha a VNet társított frissítéskor nyugati Amerikai Egyesült Államokban található affinitás csoporttal, tartózkodási helyét kell mutatnia nyugati USA-beli. 
    
    A következő sorokat az értékek lecserélése saját a hálózati konfigurációs fájl szerkesztése: 

    **Régi érték:** \<VirtualNetworkSitename = "VNetUSWest" AffinityGroup = "VNetDemoAG"\> 

    **Új érték:** \<VirtualNetworkSitename = "VNetUSWest" hely "USA-beli nyugati" =\>

1. Mentse a módosításokat, és [importálja](virtual-networks-using-network-configuration-file.md) a hálózati konfigurálásról Azure.

>[AZURE.NOTE] Az áttelepítés nem okoz a szolgáltatások bármely állásidőt.

## <a name="affinity-groups-and-vms"></a>Affinitás csoportok és a VMs

Korábban említett, affinitás csoportok általában nem ajánlott VMs számára. Egy affinitás csoport csak akkor, ha egy sor olyan VMs között a VMs abszolút legalacsonyabb hálózati késés kell rendelkeznie kell használni. Helyezze a VMs affinitás csoportban, a VMs összes kerül számítási fürt vagy a méretezés-egységben.

Fontos tudni, hogy egy affinitás csoport használata két is esetleg negatív, a következmények:

- A virtuális méretű megadása a számítási időosztás egységet által kínált virtuális méretű halmazára korlátozódik.

- Nem tud majd lefoglalhat egy új virtuális nagyobb valószínűséggel nem. Ez történik, ha a meghatározott méretarány egysége a affinitás csoport kapacitás ki.

### <a name="what-to-do-if-you-have-a-vm-in-an-affinity-group"></a>Mi a teendő, ha van egy virtuális affinitás csoportban

Az éppen egy affinitás csoport VMs nem kell a affinitás csoportból eltávolítjuk.

Miután egy virtuális telepíti, egy egyetlen időosztás egységet telepíti. Affinitás csoportok korlátozhatják a rendelkezésre álló virtuális méretű új virtuális telepítés megadása, de bármelyik meglévő virtuális, hogy telepítve van már elérhető a időosztás egységet, amelyen telepítve van a virtuális virtuális méretű halmazára korlátozott. Emiatt a virtuális eltávolítása a affinitás csoportból nem lesz hatása.
 
