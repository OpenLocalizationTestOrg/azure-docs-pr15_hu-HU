<properties
    pageTitle="Elérhetőség irányelvek beállítása |} Microsoft Azure"
    description="Tudjon meg többet a fontos tervezéséhez és kivitelezéséhez alapelve Azure infrastruktúra-szolgáltatások elérhetősége beállítása üzembe helyezése."
    documentationCenter=""
    services="virtual-machines-linux"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="availability-sets-guidelines"></a>Elérhetőség irányelvek állítja be.

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)] 

Ez a cikk koncentrál ismertetése elérhetősége eredménye ahhoz, hogy az alkalmazások tervezett vagy nem tervezett események alatt elérhető marad a szükséges tervezési lépéseket.

## <a name="implementation-guidelines-for-availability-sets"></a>Elérhetőség készletek végrehajtása szabályai

Határozatok:

- Hány elérhetőségének beállítása van szüksége a különféle szerepköröket és rétegek az alkalmazás infrastruktúra?

Feladatok:

- Minden egyes szüksége van az alkalmazás réteg VMs számának meghatározásához.
- Hibafa számának módosítása vagy frissítése az alkalmazás használandó tartományok szükségességének a megállapítása.
- Határozza meg a szükséges elérhetőségének beállítása az elnevezésük is egységes, és mi VMs tárolnak őket. A virtuális csak találhatók egy elérhetőségének beállítása. 

## <a name="availability-sets"></a>Elérhetőség beállítása

Az Azure-virtuális gépeken futó (VMs) is helyezett szeretne egy elérhetőségének beállítása nevű logikai csoportja. Az elérhetőség belül VMs létrehozásakor az Azure platform e VMs helyzetének elosztja az alapul szolgáló infrastruktúra. Legyen a tervezett karbantartás esemény az Azure platform vagy az alapul szolgáló hardver / infrastruktúra hiba, elérhetőségének beállítása e biztosítja, hogy legalább egy virtuális marad futó.

A legjobb módszer alkalmazások kell nem állnak a egyetlen virtuális. Az elérhetőség beállítása egy egyetlen virtuális tartalmazó nem szerezhet a tervezett vagy nem tervezett események belül az Azure platform bármely védekezéshez. Az [Azure SZOLGÁLTATÁSISZINT](https://azure.microsoft.com/support/legal/sla/virtual-machines) -beállítani, hogy engedélyezze az alapul szolgáló infrastruktúra keresztül VMs eloszlását elérhetősége belül két vagy több VMs szükséges.

A tartományok és a hiba tartományok oszlik az alapul szolgáló infrastruktúra Azure-ban. Ezeket a tartományokat vannak definiálva, milyen állomások közös frissítés ciklus vagy például power és a hálózat fizikai infrastruktúrát hasonló megosztás megosztása Azure automatikusan elosztja a VMs belül egy meg a tartományok kezelése elérhetőségét és hibatűrést elérhetőségét. Attól függően, hogy az alkalmazás és az VMs belül az elérhetőség méretét módosíthatja a használni kívánt tartományok számát. Erről további [irányító elérhetősége](virtual-machines-linux-manage-availability.md)és a frissítési és hibafa tartományok használatát.

Az alkalmazás infrastruktúra tervezésekor is meg kell terveznie meg az alkalmazás rétegek használatához. Csoport VMs, hogy ugyanazt a célt szolgálják az elérhetővé állítja be, például az elérhetőség az előtér-VMs nginx vagy Apache beállítása. Hozzon létre egy külön elérhetőségét a háttéradatbázist VMs MongoDB vagy MySQL beállítása. A cél, győződjön meg arról, hogy az alkalmazás minden összetevő védi az elérhetőség beállítása, és legalább egyszer példány mindig marad futó.

Az elérhetőség beállítása mellett működni, valamint forgalom mindig továbbíthatók futó példány minden alkalmazás réteg elé terheléselosztókkal fogja alkalmazni. Egy terheléselosztó nélkül a VMs továbbra is a teljes tervezett és a tervezett karbantartás események fut, de a végfelhasználók nem tudja elhárítani őket, ha az elsődleges virtuális nem érhető el.


## <a name="next-steps"></a>Következő lépések
[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)] 