<properties
    pageTitle="Címkék használata az Azure erőforrások rendszerezéséhez |} Microsoft Azure"
    description="Megtudhatja, hogy miként rendszerezheti a számlázás és kezeléséhez erőforrások címkéket alkalmazhat."
    services="azure-resource-manager"
    documentationCenter=""
    authors="tfitzmac"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="azure-resource-manager"
    ms.workload="multiple"
    ms.tgt_pltfrm="AzurePortal"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/08/2016"
    ms.author="tomfitz"/>


# <a name="using-tags-to-organize-your-azure-resources"></a>Az Azure erőforrások rendszerezéséhez-címkék használata

Erőforrás-kezelő logikailag erőforrások rendezése címkéket teszi lehetővé. A címkék kulcs/érték párokká, amely azonosítja az erőforrásokat, amely csak tulajdonságok állnak. Szeretne megjelölni, azonos kategóriába tartozó erőforrások, ezek az erőforrások az adott címkét vonatkozik.

Amikor egy adott címkével erőforrások, forrásokban talál az összes erőforrás csoportból. Ön nem csak a azonos erőforráscsoport, amely lehetővé teszi, hogy az erőforrások oly módon, amely független a telepítési kapcsolatok rendezése erőforrásainak korlátozódik. Címkék akkor lehet hasznos, ha rendszerezze számlázási vagy management erőforrásokat kell.

Minden egyes ad hozzá egy erőforráshoz vagy erőforráscsoport címke automatikusan felkerül az előfizetés szintű osztályozási rendszer neveinek. Az osztályozási rendszer neveinek is megadhatják a címke nevét, és az erőforrások későbbi címkézett használni kívánt értékeket tartalmazó előfizetéséhez.

Minden erőforrás-erőforráscsoport legfeljebb 15 címkék állhat. A címke nevét a 512 karakternél korlátozódik, és a címke értéke legfeljebb 256 karakterből állhatnak.

> [AZURE.NOTE] Csak címkéket alkalmazhat információforrások, amelyek támogatják a erőforrás-kezelő műveleteket. Ha létrehozott egy virtuális gép, virtuális hálózati vagy tároló – a klasszikus telepítési modell (például a klasszikus portálon keresztül), adott erőforrás egy címkét nem alkalmazható. A támogatási címkézés, ezek az erőforrások erőforrás-kezelő újratelepítése. Minden más erőforrások támogatja a címkézhetőség.

## <a name="templates"></a>Sablonok

Címkézéséhez erőforrás a telepítés során, egyszerűen a **címkék** elem hozzáadása az erőforrás telepít, és adja meg a címke nevét és értékét. A címke nevét és az érték nem kell az előfizetést előre megtalálható. Az egyes erőforrásokhoz legfeljebb 15 címkék lehet nyújtani.

A következő példa bemutatja egy címkével ellátott tárterület-fiókkal.

    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2015-06-15",
            "name": "[concat('storage', uniqueString(resourceGroup().id))]",
            "location": "[resourceGroup().location]",
            "tags": {
                "dept": "Finance"
            },
            "properties": 
            {
                "accountType": "Standard_LRS"
            }
        }
    ]

Jelenleg erőforrás-kezelő nem támogatja a címke nevét és az értékek egy objektum feldolgozása. Ehelyett adják át a címkét az értékek egy objektumot, de továbbra is meg kell adnia a címke nevét, az alábbi példában látható módon.

    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "tagvalues": {
          "type": "object",
          "defaultValue": {
            "dept": "Finance",
            "project": "Test"
          }
        }
      },
      "resources": [
      {
        "apiVersion": "2015-06-15",
        "type": "Microsoft.Storage/storageAccounts",
        "name": "examplestorage",
        "tags": {
          "dept": "[parameters('tagvalues').dept]",
          "project": "[parameters('tagvalues').project]"
        },
        "location": "[resourceGroup().location]",
        "properties": {
          "accountType": "Standard_LRS"
        }
      }]
    }


## <a name="portal"></a>Portál

[AZURE.INCLUDE [resource-manager-tag-resource](../includes/resource-manager-tag-resources.md)]

## <a name="powershell"></a>A PowerShell

[AZURE.INCLUDE [resource-manager-tag-resources](../includes/resource-manager-tag-resources-powershell.md)]

## <a name="azure-cli"></a>Azure CLI

[AZURE.INCLUDE [resource-manager-tag-resources-cli](../includes/resource-manager-tag-resources-cli.md)]

## <a name="rest-api"></a>REST API-VAL

A portál és -PowerShell mind a háttérben az [Erőforrás-kezelő REST API -t](https://msdn.microsoft.com/library/azure/dn848368.aspx) használja. Ha integrálni a másik környezetbe címkézés kell címkék igénybe vehet egy erőforrás-azonosító beszerzése, és frissítse az címkék a javítás hívást.


## <a name="tags-and-billing"></a>Címkék és számlázás

Támogatott szolgáltatások a címkék segítségével a számlázási adatok csoport. Például [virtuális gépeken futó integrált Azure erőforrás-kezelővel](./virtual-machines/virtual-machines-windows-compare-deployment-models.md) lehetővé teszi definiálása és rendszerezheti a számlázási használatát a virtuális gépeken futó címkéket alkalmazhat. Ha több VMs más szervezetek futnak, a címkék csoport kihasználtsága költséghely is használhatja.  
Használhatja a címkék által futtatókörnyezet; költségek kategorizálásához többek között az operációs rendszert futtató üzemi környezetben VMs számlázási használatát.

Információ a címkék keresztül az [Azure Erőforrás kihasználtsága és a RateCard API-hoz](billing-usage-rate-card-overview.md) vagy a használatát vesszővel elválasztott értékek (CSV) fájl meghallgathatja. A használati fájl letöltése az [Azure fiókok portál](https://account.windowsazure.com/) vagy [EA portálon](https://ea.azure.com). További információt a számlázási információkat való hozzáférés [szerezhet be a Microsoft Azure erőforrás-felhasználás háttérismeretek](billing-usage-rate-card-overview.md)látható. REST API-műveletek az [Azure számlázási REST API – segédlet](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c)című cikkben.

Ha letölti a szolgáltatások, számlázási címkék támogató CSV használatát, a címkéket a **címkék** oszlopban jelennek meg. További részletekért olvassa el [a Microsoft Azure-számla](billing/billing-understand-your-bill.md).

![Lásd: a számlázás címkék](./media/resource-group-using-tags/billing_csv.png)

## <a name="next-steps"></a>Következő lépések

- Szabályok és korlátozásokat át az előfizetés a testre szabott házirendek alkalmazhat. A házirend azt határozza meg, hogy az összes erőforrás kell-e egy értéknek egy adott címke igényel. További tudnivalókért lásd: az [Erőforrások kezelésére és a hozzáférés vezérléséhez feltételei](resource-manager-policy.md).
- Azure PowerShell használatával, ha telepíti az erőforrások bemutatása olvassa el az [Azure PowerShell használatá a Azure-kezelő eszközzel](./powershell-azure-resource-manager.md)című témakört.
- Azure CLI használ, ha telepíti az erőforrások bemutatása olvassa el [az Azure CLI for Mac, Linux, és a Windows Azure erőforrások kezelése és használata](./xplat-cli-azure-resource-manager.md)című témakört.
- A Bevezetés a portálon talál [az Azure portálon az Azure erőforrások kezelése](./azure-portal/resource-group-portal.md)  
