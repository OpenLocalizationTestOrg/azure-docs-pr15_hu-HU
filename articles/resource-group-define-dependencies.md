<properties
   pageTitle="Az erőforrás-kezelő sablonok függőségek |} Microsoft Azure"
   description="Megtudhatja, hogy miként értéke lehet egy erőforrás egy másik erőforrás függő erőforrások van telepítve a megfelelő sorrendben biztosítása érdekében a telepítés során."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/12/2016"
   ms.author="tomfitz"/>

# <a name="defining-dependencies-in-azure-resource-manager-templates"></a>Az erőforrás-kezelő Azure sablonok függőségeket meghatározó

Egy adott erőforrás lehet egyéb információforrások, amelyek léteznie kell, mielőtt az erőforrás telepítve van. Például SQL server léteznie kell, mielőtt megpróbálja telepíteni az SQL-adatbázishoz. Ez a kapcsolat definiálása való megjelöléséről egy erőforrást, mint a többi erőforrás függ. Általában a **dependsOn** elemmel függőség definiálása, de azt is megadhatja, hogy a **hivatkozás** funkción keresztül. 

Erőforrás-kezelő kiértékeli a függőségeket erőforrások között, és a függő sorrendjüket az üzembe helyezése őket. Ha erőforrások nem egymástól függenek, erőforrás-kezelő üzembe helyezése őket párhuzamosan.

## <a name="dependson"></a>dependsOn

Belül a sablont a dependsOn elem lehetővé teszi, hogy a meghatározásával egy erőforrás egy függő egy vagy több erőforrásait. Az érték lehet az erőforrások nevei vesszővel tagolt listáját. 

A következő példa bemutatja egy virtuális gép skála csoportja, amelyek egy terheléselosztó virtuális hálózati és által létrehozott több tárterületet fiók ismétlődve függ. Ezek az erőforrások nem jelennek meg a következő példában, de máshol vannak a sablonban kell.

    {
      "type": "Microsoft.Compute/virtualMachineScaleSets",
      "name": "[variables('namingInfix')]",
      "location": "[variables('location')]",
      "apiVersion": "2016-03-30",
      "tags": {
        "displayName": "VMScaleSet"
      },
      "dependsOn": [
        "storageLoop",
        "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]",
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
      ],
      ...
    }

Annak meghatározásához, erőforrás és erőforrások másolás ismétlődve keresztül létrehozott közötti függőség, állítsa le nevét az dependsOn elemet. Példát olvassa el a [erőforrások az Azure erőforrás-kezelő több példányának létrehozása](resource-group-create-multiple.md)című témakört.

Előfordulhat, hogy ferde, az erőforrások közötti kapcsolatokat leképezéséhez dependsOn használandó, azt is fontos megértéséhez, hogy miért csinál, mert azt hatással lehet a üzembe teljesítményét. Ha például a dokumentumhoz, milyen erőforrások összekapcsolt, dependsOn nem a megfelelő megközelítés. Nem tudja lekérdezni, mely erőforrások voltak gyökérelemben lesz definiálva dependsOn telepítés után. DependsOn használatával, esetleg hatással lehet telepítési idő, mert az erőforrás-kezelő nem telepíthető a párhuzamos két, függőség rendelkező erőforrások. Dokumentum-erőforrások közötti kapcsolatokat, helyett a [Csatolás erőforrás](resource-group-link-resources.md)használni.

## <a name="child-resources"></a>Child erőforrások

Az erőforrások tulajdonság gyermek források, amelyek az erőforrás definiálását való megadását teszi lehetővé. Child erőforrások definiált öt szintig állhat. Fontos tudni, hogy egy implicit függőség nem jön létre a gyermek erőforrás és a szülő erőforrás között. A gyermek erőforrás telepítését követően a szülő erőforrás van szüksége, ha kifejezetten meg kell adni a dependsOn tulajdonság függőséget. 

Minden egyes szülőwebhely erőforrás csak bizonyos erőforrástípus elfogadja a gyermek erőforrásként. Az elfogadott erőforrástípus a [sablon séma](https://github.com/Azure/azure-resource-manager-schemas) a szülő erőforrás vannak megadva. A gyermek erőforrás-típus nevére a szülő erőforrástípus nevét tartalmazza, például **Microsoft.Web/sites/config** és **Microsoft.Web/sites/extensions** a **Microsoft.Web/sites**mindkét gyermek erőforrásait.

A következő példa bemutatja az SQL server és a SQL-adatbázishoz. Figyelje meg, hogy egy explicit függőség definiálva van-e az SQL-adatbázis és az SQL server közötti annak ellenére, hogy az adatbázis kiszolgáló stílust.

    "resources": [
      {
        "name": "[variables('sqlserverName')]",
        "type": "Microsoft.Sql/servers",
        "location": "[resourceGroup().location]",
        "tags": {
          "displayName": "SqlServer"
        },
        "apiVersion": "2014-04-01-preview",
        "properties": {
          "administratorLogin": "[parameters('administratorLogin')]",
          "administratorLoginPassword": "[parameters('administratorLoginPassword')]"
        },
        "resources": [
          {
            "name": "[parameters('databaseName')]",
            "type": "databases",
            "location": "[resourceGroup().location]",
            "tags": {
              "displayName": "Database"
            },
            "apiVersion": "2014-04-01-preview",
            "dependsOn": [
              "[variables('sqlserverName')]"
            ],
            "properties": {
              "edition": "[parameters('edition')]",
              "collation": "[parameters('collation')]",
              "maxSizeBytes": "[parameters('maxSizeBytes')]",
              "requestedServiceObjectiveName": "[parameters('requestedServiceObjectiveName')]"
            }
          }
        ]
      }
    ]


## <a name="reference-function"></a>hivatkozás függvény

A [hivatkozás függvény](resource-group-template-functions.md#reference) lehetővé teszi, hogy az érték származtatása más JSON neve és érték párokká, vagy futtatókörnyezet erőforrások kifejezés. Hivatkozás a kifejezések implicit módon az elemeket rekordként, hogy egy erőforrás attól függ, hogy egy másik számítógépre. 

    reference('resourceName').propertyPath

Használhatja ezt az elemet vagy a dependsOn elem függőségeket, de nem kell függő ugyanaz az erőforrás mindkettőt használni. Amikor csak lehetséges, ne véletlenül vegyen fel egy fölösleges függőség egy implicit hivatkozási használatával.

További tudnivalókért olvassa el a [hivatkozás függvény](resource-group-template-functions.md#reference)című témakört.

## <a name="next-steps"></a>Következő lépések

- Erőforrás-kezelő Azure sablonok létrehozásával kapcsolatos további tudnivalókért olvassa el a [sablonok létrehozása](resource-group-authoring-templates.md)című témakört. 
- A használható függvényeket sablonba listáját lásd: [sablon függvények](resource-group-template-functions.md).

