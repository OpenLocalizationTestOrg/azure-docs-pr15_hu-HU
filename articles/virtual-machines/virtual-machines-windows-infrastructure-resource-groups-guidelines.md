<properties
    pageTitle="Erőforráscsoport útmutatások |} Microsoft Azure"
    description="Tudjon meg többet a fontos tervezéséhez és kivitelezéséhez alapelve erőforráscsoport Azure infrastruktúrájának szolgáltatásai üzembe helyezése."
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

# <a name="azure-resource-group-guidelines"></a>Azure erőforrás csoport útmutató

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)] 

Ez a cikk koncentrál logikailag a környezet kiépítésekor, és az erőforrás csoportok minden összetevő csoport ismertetése.


## <a name="implementation-guidelines-for-resource-groups"></a>Az erőforrás csoportok végrehajtása szabályai

Határozatok:

- Lesz a kiépítésekor erőforrás csoportok, az alapvető infrastruktúra-összetevők, vagy a teljes alkalmazás telepítési?
- Szükség van korlátozhatja a hozzáférést az erőforrás-csoportok használatával szerepköralapú hozzáférés-vezérlők?

Feladatok:

- Határozza meg, mi alapvető infrastruktúra-összetevők és a dedikált van szüksége az erőforrás-csoportok.
- Tekintse át az erőforrás-kezelő sablonok konzisztens és reprodukálható telepítésekhez megvalósításáról.
- Határozza meg, milyen felhasználói hozzáférés szerepkörök erőforráscsoport való hozzáférés szabályozása szükséges.
- Hozzon létre az erőforrás-csoportok használatával az elnevezésük is egységes. Azure PowerShell és a portálon is használhatja.


## <a name="resource-groups"></a>Erőforrás-csoportok

Az Azure-, logikailag csoportosíthatja a kapcsolódó források, például a tárterület-fiókok, virtuális hálózatok és virtuális gépeken futó (VMs) telepíthető, kezelése és karbantartása őket egy egységként. Ezt a megközelítést megkönnyíti alkalmazások telepítéséhez, miközben a kapcsolódó források együtt a kezelés szemszögéből, illetve hozzáférést másokkal az erőforrások csoporthoz. Egy erőforrás csoportok átfogóbb ismertetése, olvassa el az [Azure erőforrás szolgáltatásának áttekintése](../azure-resource-manager/resource-group-overview.md).

A fő erőforrás csoportok funkció azt jelenti, hogy a sablonok használata környezet kiépítésekor. Sablon egyszerűen JSON fájl deklarálása tárolására, hálózatot, és az erőforrások számítja ki. Is meghatározhat kapcsolódó egyéni parancsfájlok vagy a beállítások alkalmazásához. Ezek a sablonok használatával az alkalmazások hozzon létre konzisztens és reprodukálható telepítések. Ezt a megközelítést megkönnyíti a fejlesztői környezet kiépítésekor, majd ugyanazt a sablont hozzon létre egy éles üzemi vagy fordítva. Jobban megértheti, sablonok használata, olvassa el [a sablon forgatókönyv](../resource-manager-template-walkthrough.md) , amely végigvezeti Önt ki egy sablont az épület lépésein.

Van két különböző módszer, amikor a környezetben, az erőforrás-csoportok tervezése:

- Erőforrás-csoportok egyesíti a tárterület-fiókok, virtuális hálózatok és alhálózat, VMs, minden alkalmazás telepítéshez betöltése balancers stb.
- Központi erőforrás-csoportok a core virtuális hálózati és alhálózat vagy tárolási fiókok tartalmaz. Az alkalmazások majd szerepelnek, a saját VMs, terheléselosztókkal, hálózati kapcsolatok stb tartalmazó erőforrás-csoportokat.

Akkor skála, a virtuális hálózati és alhálózat központi erőforrás-csoportok létrehozása könnyebb össze a határokon helyszíni hálózati kapcsolatainak hibrid csatlakozási beállítások. Az alternatív megközelítés, az egyes alkalmazások saját virtuális hálózat konfigurálása és karbantartása igénylő van.  [Szerepköralapú hozzáférés-vezérlők](../active-directory/role-based-access-control-what-is.md) nyújt az erőforrás csoportok való hozzáférés szabályozása részletes lehetőséget. Gyártási alkalmazások megadhatja, hogy a felhasználóknak, hogy ezek az erőforrások is hozzáférhet, vagy az alapvető infrastruktúra erőforrásokat csak infrastruktúra mérnökök segítségükkel korlátozhatja. Az alkalmazás tulajdonosok csak férhet hozzá az erőforráscsoport és nem a fő a környezet Azure infrastruktúra belül a alkalmazásösszetevők. Ha szeretné, megtervezheti a környezetben, figyelembevételével felhasználókat a férnie az erőforrásokat, és ennek megfelelően a az erőforrás-csoportok tervezése 


## <a name="next-steps"></a>Következő lépések

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)] 