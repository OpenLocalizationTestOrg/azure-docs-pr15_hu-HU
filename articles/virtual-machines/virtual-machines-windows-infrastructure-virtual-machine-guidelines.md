<properties
    pageTitle="a Windows virtuális gépeken futó irányelvek |} Microsoft Azure"
    description="További tudnivalók a fontos tervezéséhez és kivitelezéséhez alapelve telepítése windows virtuális gépeken futó az Azure:"
    documentationCenter=""
    services="virtual-machines-windows"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="virtual-machines-guidelines"></a>Virtuális gépeken futó útmutató

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)] 

Ez a cikk koncentrál ismertetése létrehozásához és kezeléséhez virtuális gépeken futó (VMs) az Azure környezetben szükséges tervezési lépéseket.

## <a name="implementation-guidelines-for-vms"></a>VMs végrehajtása szabályai
Határozatok:

- Hány VMs szüksége a különböző alkalmazás rétegek és a infrastruktúra összetevői?
- Milyen Processzor- és erőforrások minden virtuális-nek, valamint hogy mik a tárhelyre?

Feladatok:

- Adja meg a feladatok az alkalmazás és a VMs igénylő erőforrások.
- Minden egyes virtuális megfelelő virtuális méretét és tároló típusú erőforrás igényeivel igazíthatja.
- A különböző rétegek és a infrastruktúra összetevői az erőforrás csoportok megadása parancsra.
- Adja meg a virtuális elnevezésük is egységes.
- Hozzon létre a VMs az Azure PowerShell, webes portál, vagy az erőforrás-kezelő sablonok használatával.

## <a name="virtual-machines"></a>Virtuális gépeken futó

A fő összetevői az Azure környezetben egyik valószínűleg VMs. Az, ha futtatja az alkalmazásokat, a adatbázisok, a hitelesítési szolgáltatások, a stb.

Fontos, hogy a [különböző virtuális méretű](virtual-machines-windows-sizes.md) megfelelően méretét a teljesítmény és a költség perspektíva a környezet ismertetése. A VMs nincs elég Processzormagok vagy memóriát, ha az alkalmazás teljesítményének szenved, függetlenül attól, mennyire van verziójához, és fejlesztett. A javasolt feladatok minden virtuális adatsor kiindulási pontként elolvasni, úgy dönt, hogy milyen méretben virtuális minden összetevő a infrastruktúra használható. Azt is megteheti [méretének módosítása: a virtuális](https://azure.microsoft.com/blog/resize-virtual-machines/) telepítés után.

Tárhely játszik szerepet virtuális teljesítmény. Szabványos tárolására, normál frissítésjelző lemezt használó vagy prémium tároló használható magas I/O munkaterhelésekből és csúcs teljesítmény elérése érdekében SSD lemezt használó. A virtuális méretű ott vannak költség megfontolások az tárolóeszközre kijelölése. Erről a [tárhely infrastruktúra irányelvek cikk](virtual-machines-windows-infrastructure-storage-solutions-guidelines.md) megtudhatja, hogyan tervezése a megfelelő tárolási a VMs az optimális teljesítmény.


## <a name="resource-groups"></a>Erőforrás-csoportok
Például VMs összetevők esetében könnyű kezelés és [Azure erőforrás csoportok](../azure-resource-manager/resource-group-overview.md)használata a karbantartási logikailag csoportosított együtt. Erőforrás-csoportok használatával elkészítheti, kezelheti és egy adott alkalmazás alkotó összes erőforrás figyelése. [Szerepköralapú hozzáférés - vezérlők](../active-directory/role-based-access-control-what-is.md) való hozzáférést a szükséges forrásokat munkacsoporton belül másoknak is alkalmazhat. Tervezheti meg, hogy az erőforrás-csoportokat és a szerepkör-hozzárendelések az időt vehet igénybe. Íme néhány más módszer, ténylegesen tervezése és megvalósítása erőforráscsoport, így figyeljen arra, hogy hogyan lehet a legjobban megérteni a VMs kiépítésekor a [erőforrás csoportok irányelvek cikk](virtual-machines-windows-infrastructure-resource-groups-guidelines.md) .


## <a name="templates"></a>Sablonok 
Sablonok, deklaráció JSON a fájlok létrehozásához a VMs által meghatározott készíthet. Sablonok általában is hozhat létre a szükséges tárhely, hálózatok, hálózati kapcsolatok, IP-címek, együtt a saját maguk VMs stb. Sablonok segítségével egységes reprodukálható környezetekben fejlesztési létrehozása és tesztelése céljából egyszerűen bizonyos gyártási környezetekben, és ez fordítva is igaz. Bővebb információ [összeállítását, és a sablonok használata](../azure-resource-manager/resource-group-overview.md#template-deployment) megértéséhez, hogy hogyan lehet a segítségükkel létrehozásához és üzembe helyezése a VMs.


## <a name="next-steps"></a>Következő lépések
[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)] 