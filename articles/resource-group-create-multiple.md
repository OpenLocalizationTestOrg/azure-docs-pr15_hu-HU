<properties
   pageTitle="Erőforrások több példányának telepítése |} Microsoft Azure"
   description="Másolás és a tömbök használata egy erőforrás-kezelő Azure sablonban találta többször telepítésekor erőforrásokat."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="wpickett"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/30/2016"
   ms.author="tomfitz"/>

# <a name="create-multiple-instances-of-resources-in-azure-resource-manager"></a>Az erőforrások több példányon az Azure erőforrás-kezelő létrehozása

Ez a témakör bemutatja, hogyan hozhat létre több példányának egy erőforrás az Azure erőforrás-kezelő sablonban találta.

## <a name="copy-copyindex-and-length"></a>másolás, a copyIndex és a hossz

Az erőforrás többször létrehozásához, belül egy objektum, amely meghatározza, hogy hányszor forduljon találta **másolása** adhatja meg. A Másolás hajtja végre a következő formátumban:

    "copy": { 
        "name": "websitescopy", 
        "count": "[parameters('count')]" 
    } 

Az aktuális közelítését értéket a függvény a **copyIndex()** , például a összefűzés függvényen belül alább látható módon érheti el.

    [concat('examplecopy-', copyIndex())]

Az értékek tömbjét több erőforrások létrehozásakor a **hossz** függvénnyel számának megadása. A tömb a hossz függvény paraméterként adja meg.

    "copy": {
        "name": "websitescopy",
        "count": "[length(parameters('siteNames'))]"
    }

## <a name="use-index-value-in-name"></a>A név mezőbe írja be index értéket

Használhatja a Másolás művelet létrehozása több példányának egy erőforrás egyedileg nevű a növekvő index alapján. Például érdemes lehet egy egyedi számérték hozzáadása telepítik erőforrásnevek végére. A névvel ellátott három webhelyek üzembe:

- examplecopy-0
- examplecopy-1
- examplecopy-2.

A következő sablont használja:

    "parameters": { 
      "count": { 
        "type": "int", 
        "defaultValue": 3 
      } 
    }, 
    "resources": [ 
      { 
          "name": "[concat('examplecopy-', copyIndex())]", 
          "type": "Microsoft.Web/sites", 
          "location": "East US", 
          "apiVersion": "2015-08-01",
          "copy": { 
             "name": "websitescopy", 
             "count": "[parameters('count')]" 
          }, 
          "properties": {
              "serverFarmId": "hostingPlanName"
          }
      } 
    ]

## <a name="offset-index-value"></a>Eltolás index értéke

Az előző példában az index értéke 2 nulláról előbb feltűnik. Az index érték megadott mértékben eltolt, adják át egy értéket a **copyIndex()** függvény, például **copyIndex(1)**. A végrehajtandó közelítések számának még meg van határozva másolás eleme, de copyIndex értékét a megadott érték szerint az eltolás. Igen ugyanazt a sablont a használják az előző példában, de **copyIndex(1)** megadása szeretné üzembe nevű három webhelyek:

- examplecopy-1
- examplecopy-2
- examplecopy-3-as

## <a name="use-copy-with-array"></a>Tömb másolat használata
   
A Másolás a tömbök működik, mert a, minden elemet a tömb lehet Végigléptetés esetén kifejezetten hasznosak lehetnek. A névvel ellátott három webhelyek üzembe:

- a Contoso examplecopy
- a Fabrikam examplecopy
- examplecopy-"coho winery"

A következő sablont használja:

    "parameters": { 
      "org": { 
         "type": "array", 
             "defaultValue": [ 
             "Contoso", 
             "Fabrikam", 
             "Coho" 
          ] 
      }
    }, 
    "resources": [ 
      { 
          "name": "[concat('examplecopy-', parameters('org')[copyIndex()])]", 
          "type": "Microsoft.Web/sites", 
          "location": "East US", 
          "apiVersion": "2015-08-01",
          "copy": { 
             "name": "websitescopy", 
             "count": "[length(parameters('org'))]" 
          }, 
          "properties": {
              "serverFarmId": "hostingPlanName"
          } 
      } 
    ]

Természetesen beállítása a példányszám a tömb hossza nem értékre. Például meg sikerült sok értékeket tartalmazó tömb létrehozása, majd fázisban és a paraméter értéke, amely meghatározza az üzembe tömbelemek hány. Ebben az esetben a példányszám beállítása, az első példában látható módon. 

## <a name="depending-on-resources-in-a-loop"></a>Attól függően, hogy ismétlődve erőforrásokhoz

Megadhatja, hogy egy erőforrás egy másik erőforrás után telepíthető az **dependsOn** elem használatával. Ha kell, ismétlődve erőforrásokhoz gyűjteménye függ erőforrás üzembe, adja meg a nevét a **dependsOn** elem másolása le. A következő példa bemutatja, hogyan 3 tároló fiókok üzembe üzembe helyezése a virtuális gép előtt. Nem jelenik meg a teljes virtuális gép-definíciót. Látható, hogy a Másolás elemnek a **neve** **storagecopy** **storagecopy**is értéke a virtuális gépeken futó a **dependsOn** elemet.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {},
        "resources": [
            {
                "apiVersion": "2015-06-15",
                "type": "Microsoft.Storage/storageAccounts",
                "name": "[concat('storage', uniqueString(resourceGroup().id), copyIndex())]",
                "location": "[resourceGroup().location]",
                "properties": {
                    "accountType": "Standard_LRS"
                 },
                "copy": { 
                    "name": "storagecopy", 
                    "count": 3 
                }
            },
           {
               "apiVersion": "2015-06-15", 
               "type": "Microsoft.Compute/virtualMachines", 
               "name": "[concat('VM', uniqueString(resourceGroup().id))]",  
               "dependsOn": ["storagecopy"],
               ...
           }
        ],
        "outputs": {}
    }

## <a name="looping-on-a-nested-resource"></a>A beágyazott erőforrás hurok

Másolás hurkot nem használhat beágyazott erőforrás. Ha több példányt, amely általában meghatározásával másik forráshoz beágyazva egy erőforrás hozzon létre kell kell inkább hozzon létre egy felső szintű erőforrásként az erőforrás, és meghatározása a szülő erőforrás **típusa** és **neve** tulajdonságlapján viszonyban.

Tegyük fel, hogy általában meghatározásával egy adatkészlet adatok gyár belüli egymásba ágyazott erőforrás.

    "parameters": {
        "dataFactoryName": {
            "type": "string"
         },
         "dataSetName": {
            "type": "string"
         }
    },
    "resources": [
    {
        "type": "Microsoft.DataFactory/datafactories",
        "name": "[parameters('dataFactoryName')]",
        ...
        "resources": [
        {
            "type": "datasets",
            "name": "[parameters('dataSetName')]",
            "dependsOn": [
                "[parameters('dataFactoryName')]"
            ],
            ...
        }
    }]
    
Hozzon létre adatkészleteket több példányával, kellene sablon módosítása az alább látható módon. Értesítés, a teljes elérési, és a nevét tartalmazza az gyári nevét.

    "parameters": {
        "dataFactoryName": {
            "type": "string"
         },
         "dataSetName": {
            "type": "array"
         }
    },
    "resources": [
    {
        "type": "Microsoft.DataFactory/datafactories",
        "name": "[parameters('dataFactoryName')]",
        ...
    },
    {
        "type": "Microsoft.DataFactory/datafactories/datasets",
        "name": "[concat(parameters('dataFactoryName'), '/', parameters('dataSetName')[copyIndex()])]",
        "dependsOn": [
            "[parameters('dataFactoryName')]"
        ],
        "copy": { 
            "name": "datasetcopy", 
            "count": "[length(parameters('dataSetName'))]" 
        } 
        ...
    }]

## <a name="create-multiple-instances-when-copy-wont-work"></a>Létrehozása a több példányával, ha nem működik a Másolás

**Másolás** csak a erőforrástípus, a tulajdonságok egy erőforrás típusa belül nem használható. Ez problémákat okozhat meg során létre szeretne hozni több példányával, amit az erőforrás része. A leggyakoribb helyzet, ha több adatot lemezre virtuális géphez. Mivel a **dataDisks** a virtuális gépen nem saját erőforrás típusa tulajdonság **másolása** nem használata az adatok lemezt. Ehelyett hoz létre egy tömb annyi adatok lemez lesz szüksége, és adják át az adatok létrehozásához lemez tényleges száma. A virtuális gép definícióban úgy juthat az elemeket, amelyeket ténylegesen csak a számot a tömb, használja a **készítése** függvényt.

A minta a teljes példa sablon [létrehozása egy virtuális dinamikus kijelölt adatok lemezt a](https://azure.microsoft.com/documentation/templates/201-vm-dynamic-data-disks-selection/) megjelenítés.

A telepítési sablon a vonatkozó szakaszokat alatt jelennek meg. A sablon sok jelölje ki az érintett dinamikusan létrehozása az adatok lemezt számos szakaszok megszűnt. Figyelje meg a paraméterek **numDataDisks** , amely lehetővé teszi, hogy adják át az létrehozásához lemez száma. 

```
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    ...
    "numDataDisks": {
      "type": "int",
      "maxValue": 64,
      "metadata": {
        "description": "This parameter allows you to select the number of disks you want"
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'dynamicdisk')]",
    "sizeOfDataDisksInGB": 100,
    "diskCaching": "ReadWrite",
    "diskArray": [
      {
        "name": "datadisk1",
        "lun": 0,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk1.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      },
      {
        "name": "datadisk2",
        "lun": 1,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk2.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      },
      {
        "name": "datadisk3",
        "lun": 2,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk3.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      },
      {
        "name": "datadisk4",
        "lun": 3,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk4.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      },
      ...
      {
        "name": "datadisk63",
        "lun": 62,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk63.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      },
      {
        "name": "datadisk64",
        "lun": 63,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk64.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      }
    ]
  },
  "resources": [
    ...
    {
      "type": "Microsoft.Compute/virtualMachines",
      "properties": {
        ...
        "storageProfile": {
          ...
          "dataDisks": "[take(variables('diskArray'),parameters('numDataDisks'))]"
        },
        ...
      }
      ...
    }
  ]
}
```


## <a name="next-steps"></a>Következő lépések
- Ha azt szeretné, ha többet szeretne tudni a szakaszok, egy sablon, olvassa el a [Azure erőforrás-kezelő sablonok létrehozása](./resource-group-authoring-templates.md)című témakört.
- Az összes a sablonba használható függvények lásd: [Azure erőforrás-kezelő sablon függvények](./resource-group-template-functions.md).
- A sablon telepítéséről című témakörben talál [Deploy Azure erőforrás-kezelő sablonnal kérelmet](resource-group-template-deploy.md).
