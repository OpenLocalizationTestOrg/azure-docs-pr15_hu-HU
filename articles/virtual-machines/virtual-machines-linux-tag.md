<properties
   pageTitle="Hogyan címkézéséhez Linux virtuális gépen |} Microsoft Azure"
   description="Ismerje meg, az erőforrás-kezelő telepítési modell Azure-ban létrehozott Linux virtuális gép címkézésével kapcsolatban."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="mmccrory"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="07/05/2016"
   ms.author="memccror"/>

# <a name="how-to-tag-a-linux-virtual-machine-in-azure"></a>Hogyan Linux virtuális gép címkézéséhez Azure-ban

Ez a cikk ismerteti a változatos módszerekkel, amelyekkel Linux virtuális gép címkézése Azure-ban, az erőforrás-kezelő telepítési modell keresztül. Címkék felhasználói kulcs/érték párokká, amely mentesíthetők közvetlenül egy erőforráshoz vagy erőforrás csoport a rendszer. Azure jelenleg támogatott legfeljebb 15 címkék egy erőforrás- és erőforrás-csoportot. Címkék létrehozása idején erőforrás helyezett el vagy hozzáadni a meglévő erőforrás. Felhívjuk a figyelmét arra, csak az erőforrás-kezelő telepítési modell segítségével létrehozott erőforrások címkék támogatott.

[AZURE.INCLUDE [virtual-machines-common-tag](../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-azure-cli"></a>Az Azure CLI megcímkézése

[Telepítse és állítsa be az Azure CLI](../xplat-cli-azure-resource-manager.md) a kezdéshez, és ellenőrizze, hogy az erőforrás-kezelő módban nem (`azure config mode arm`).

Az összes tulajdonság egy adott virtuális a számítógépen, beleértve a címkéket, ezzel a paranccsal tekintheti meg:

        azure vm show -g MyResourceGroup -n MyTestVM

Fel egy új virtuális keresztül az Azure CLI, használja a `azure vm set` parancs a címke **-t**paraméter együtt:

        azure vm set -g MyResourceGroup -n MyTestVM –t myNewTagName1=myNewTagValue1;myNewTagName2=myNewTagValue2

Eltávolítja az összes címkét, használhatja a **– T** paramétert a `azure vm set` parancsot.

        azure vm set – g MyResourceGroup –n MyTestVM -T


Most, hogy az erőforrás-Azure CLI a portálon címkék alkalmazott azt, nézzük egy használatát részleteinek megtekintéséhez a címkéket a számlázási portálon.

[AZURE.INCLUDE [virtual-machines-common-tag-usage](../../includes/virtual-machines-common-tag-usage.md)]

## <a name="next-steps"></a>Következő lépések

* Az Azure erőforrások címkézésével kapcsolatban további információért lásd: [Azure erőforrás-kezelő – áttekintés][] és a [Címkék használata az Azure erőforrások rendszerezéséhez][].
* Hogyan lehet címkék segítségével az Azure erőforrások használt kezelése című témakör tartalmazza [Azure számlájának ismertetése][] és [szerezhet be a Microsoft Azure erőforrás-felhasználás az összefüggéseket][].





[Azure CLI environment]: ./xplat-cli-azure-resource-manager.md
[Azure erőforrás szolgáltatásának áttekintése]: ../azure-resource-manager/resource-group-overview.md
[Az Azure erőforrások rendszerezéséhez-címkék használata]: ../resource-group-using-tags.md
[Azure számlájának ismertetése]: ../billing/billing-understand-your-bill.md
[Nyereség háttérismeretek be a Microsoft Azure erőforrás-felhasználás]: ../billing-usage-rate-card-overview.md
