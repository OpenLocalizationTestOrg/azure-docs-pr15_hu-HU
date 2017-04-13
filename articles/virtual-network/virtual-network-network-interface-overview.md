<properties 
   pageTitle="A hálózati kapcsolatok |} Microsoft Azure"
   description="Tudjon meg többet az Azure erőforrás-kezelő Azure hálózati kapcsolatok."
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="jdial" />

# <a name="network-interfaces"></a>Hálózati kapcsolatok

A hálózati kapcsolat (hálózati) a virtuális gép (virtuális) és az alapul szolgáló szoftver hálózati összekapcsolása egy. Ez a cikk ismerteti a hálózati illesztő, és hogyan használják a Azure erőforrás-kezelő telepítési modell.

Javasoljuk az erőforrás-kezelő telepítési modell használata új erőforrások üzembe helyezése, de a hálózati kapcsolat a [Klasszikus](virtual-network-ip-addresses-overview-classic.md) telepítési modell VMs is telepítheti. Ha jártas a Klasszikus modell, fontos különbségek vannak az erőforrás-kezelő telepítési modell virtuális hálózatokban. További tudnivalók a különbségeket a [virtuális gép hálózat - klasszikus](virtual-network-ip-addresses-overview-classic.md#differences-between-resource-manager-and-classic-deployments) cikk elolvasásával.

Az Azure, a hálózati kapcsolat:

1. Egy erőforrás létrehozható, töröl, és saját konfigurálható beállításait.
2. Kell csatlakoznia egy alhálózat egy Azure virtuális hálózatban (VNet) után. Ha nem ismeri a VNets, tudjon meg többet a őket a [virtuális hálózati áttekintés](virtual-networks-overview.md) cikk elolvasásával. A hálózati kártya egy adott megtalálható az Azure ugyanazon a [helyen](https://azure.microsoft.com/regions) és a adaptert [előfizetés](../azure-glossary-cloud-terminology.md#subscription) VNet kell csatlakoznia. A hálózati kártya létrehozását követően módosíthatja az alhálózathoz csatlakoztatva van, de nem módosítható a VNet csatlakoztatva van.
3. Hozzárendelt nevét, hogy a hálózati kártya létrehozását követően nem módosíthatók. A nevét az Azure [erőforráscsoport](../azure-resource-manager/resource-group-overview.md#resource-groups)belül egyedinek kell lennie, de nem tartozik, az előfizetést, az Azure helyét, akkor jön létre vagy a VNet csatlakoztatva van belül egyedinek kell lennie. Több NIC általában az Azure előfizetéssel belül jönnek létre. Javasoljuk, hogy, mivel ezek elnevezni, amely több mint alapértelmezett nevekkel könnyebb NIC kezelése. A [javasolt Azure erőforrások elnevezési szabályai](../guidance/guidance-naming-conventions.md) cikke vonatkozó ajánlások.
4. A virtuális lehet csatolni, de csak kapcsolódhat egy egyetlen virtuális, amely a adaptert ugyanazon a helyen létezik
5. A MAC cím mezőt, amely megőrződnek, a hálózati az mindaddig, amíg ez egy virtuális csatolva van. A MAC-cím megőrződnek, hogy a virtuális újraindításáig (a belül az operációs rendszer) vagy leállítása (vonja kiosztott) és kezdheti el használni az Azure-portálra, Azure PowerShell vagy Azure parancssort. Ha egy virtuális le, és egy másik virtuális csatolva, a hálózati kártya kapja meg más MAC címet. Ha a hálózati kártya törölnek, a MAC-cím más NIC van rendelve.
6. Kell rendelkeznie egy elsődleges **magánjellegű** *IPv4* statikus vagy dinamikus IP-cím hozzárendelve.
8. Előfordulhat, hogy egy nyilvános IP-cím erőforráshoz társított rá.
9. Támogatja az egyetlen-legfelső szintű I/O virtualizációs (SR-IOV) virtuális méretét a Microsoft Windows Server operációs rendszer adott verziója fut a hálózat gyorsabb. ELŐNÉZETI funkció kapcsolatos további információért olvassa el a [virtuális gép hálózati gyorsított](virtual-network-accelerated-networking-powershell.md) cikket.
10. Kaphatnak a magánjellegű IP-címek, ha IP-továbbítás engedélyezve van a adaptert rendelt nem adatforgalmat Ha egy virtuális például tűzfal szoftver fut, irányítja a saját IP-címek nem szánt csomagokat. A virtuális továbbra is kell futtatni a szoftver alkalmas útválasztás vagy a forgalom átirányítása, de ehhez IP-továbbítás engedélyeznie kell a adaptert
11. Gyakran hoz létre a virtuális csatolva van, vagy az azonos VNet, hogy csatlakoztatva van, az azonos erőforráscsoport abban az esetben, ha ez nem kell lennie.

## <a name="vms-with-multiple-network-interfaces"></a>Több hálózat felülettel rendelkező VMs

Több kapcsolható egy virtuális, de amikor az ehhez a művelethez, tartsa szem előtt a következőket:  

- A virtuális mérete támogatnia kell több. Megtudhatja, hogy arról, hogy melyik virtuális méretű támogatja a több, olvassa el a [Windows Server virtuális méretű](../virtual-machines/virtual-machines-windows-sizes.md) vagy [Linux virtuális méretű](../virtual-machines/virtual-machines-linux-sizes.md) cikkeket.   
- A virtuális legalább két kártyát kell létrehozni. Ha a virtuális létrehozott csak egy hálózati kártya, akkor is, ha egynél több támogatja a virtuális méretét, nem csatolhat további NIC a virtuális létrehozása után. Mindaddig, amíg a virtuális legalább két kártyát jött létre, akkor csatolhat további NIC a virtuális létrehozása, után mindaddig, amíg a virtuális mérete támogatja a több mint két kártyát.  
- Másodlagos NIC (az elsődleges hálózati kártya nem leválasztva) is leválasztás egy virtuális az, ha a virtuális legalább három NIC csatolva van. NIC nem leválasztása, ha a virtuális kettő vagy kevesebb NIC csatolva.  
- A virtuális belül a hálózati adapterrel sorrendjének véletlen lesz, és Azure infrastruktúra frissítések közötti is változhat. Az IP-címek és a megfelelő ethernet MAC azonban címeket, változatlan marad. Tegyük fel, hogy az operációs rendszer azonosítja Azure NIC1 Eth1 szerint. Eth1 10.1.0.100 és 00-0D-3A-B0-39-0D MAC címet tartalmaz. Miután az Azure-infrastruktúrát frissíteni, és indítsa újra, az operációs rendszer most az Azure NIC1 Eth2, előfordulhat, hogy azonosítása, de az IP- és a MAC címek azonos lesz az operációs rendszer azonosította Azure NIC1 Eth1, amikor használtakkal. Ha a számítógép újraindítása ügyfél által kezdeményezett, a hálózati kártya sorrendben változatlan marad az operációs rendszer belül.  
- Ha a virtuális [elérhetőségének beállítása](../azure-glossary-cloud-terminology.md#availability-set)egy tag, elérhetőség meghatározott belül minden VMs egyetlen hálózati egy vagy több kell rendelkeznie. Ha a VMs több van, a számot minden rendelkeznek nem kell lennie az azonos, mindaddig, amíg minden virtuális tartalmaz legalább két kártyát.

## <a name="next-steps"></a>Következő lépések

- Megtudhatja, hogy miként hozzon létre egy virtuális egy egyetlen hálózati olvasásával [létrehozása egy virtuális](../virtual-machines/virtual-machines-windows-hero-tutorial.md) olvashatók.
- Megtudhatja, hogy miként hozzon létre egy virtuális több a [Deploy egy virtuális több hálózati kártya a](virtual-network-deploy-multinic-arm-ps.md) témakör elolvasásával.
- Megtudhatja, hogy miként hozzon létre egy hálózati kártya több IP-konfigurációjának az [Azure virtuális gépeken futó több IP-címek](virtual-network-multiple-ip-addresses-powershell.md) cikk elolvasásával.
