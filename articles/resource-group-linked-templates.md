<properties
   pageTitle="Erőforrás-kezelő sablonok csatolt |} Microsoft Azure"
   description="Az erőforrás-kezelő Azure-sablon csatolt sablonok segítségével elemes sablon megoldás létrehozása ismerteti. Paraméterek értékeket adják át, adja meg a paraméterek fájl és dinamikusan létrehozott URL-címek mutatja."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/02/2016"
   ms.author="tomfitz"/>

# <a name="using-linked-templates-with-azure-resource-manager"></a>Csatolt sablonok használata a Azure-kezelő eszközzel

A belül egy erőforrás-kezelő Azure sablonba hozzákapcsolhatja egy másik sablont, amely lehetővé teszi, hogy a üzembe alakítja felbontani célzott, célját-specifikus sablonokat. Akárcsak egy alkalmazást, több kódot viselkedésre decomposing, dekompozíciós tesztelés, újrafelhasználása és olvashatóság előnyökkel jár.  

Átviheti paraméterek fő sablonból csatolt sablonra, és ezeket a paramétereket paramétereket vagy a hívó sablon által elérhetővé tett változók közvetlenül képezhető. A csatolt sablon is átadhatja a kimeneti változó vissza az adatforrások sablont, a engedélyezése egy kétirányú adatcsere sablonok között.

## <a name="linking-to-a-template"></a>Sablon mutató hivatkozással

A fő sablont, amely a csatolt sablon mutat belül telepítési erőforrás hozzáadásával két sablonok között mutató hivatkozás létrehozása A **templateLink** tulajdonság a csatolt sablon URI-ra. Lehet nyújtani paraméterértékeket a csatolt sablon vagy közvetlenül a sablonban szereplő értékek megadásával vagy paraméter fájlra mutató hivatkozással. Az alábbi példában a **Paraméterek** tulajdonság adja meg közvetlenül a paraméter értéke.

    "resources": [ 
      { 
         "apiVersion": "2015-01-01", 
         "name": "linkedTemplate", 
         "type": "Microsoft.Resources/deployments", 
         "properties": { 
           "mode": "incremental", 
           "templateLink": {
              "uri": "https://www.contoso.com/AzureTemplates/newStorageAccount.json",
              "contentVersion": "1.0.0.0"
           }, 
           "parameters": { 
              "StorageAccountName":{"value": "[parameters('StorageAccountName')]"} 
           } 
         } 
      } 
    ] 

Az erőforrás-kezelő szolgáltatás elérheti a csatolt sablon kell lennie. A helyi vagy fájlt, amely csak akkor áll rendelkezésre, a csatolt sablon a helyi hálózaton nem adhat meg. Adja meg a URI érték, amely magában foglalja a **http-** vagy a **https**csak. Egy, hogy a csatolt sablon helyezze a tárterület-fiók, és használja a URI az elemhez, mint például az alábbi példában.

    "templateLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
        "contentVersion": "1.0.0.0",
    }

Bár a csatolt csak a külső felekkel érhető el, akkor nem szükséges általában a a nyilvánosság számára elérhetővé tenni. A sablon hozzáadhatja a személyes tárterület-fiókot, amelyet érhető el, csak a tárhely fióktulajdonos. Ezután hozzon létre egy megosztott aláírás (Társítások) jogkivonat hozzáférés engedélyezése a telepítés során. A biztonsági jogkivonat adja hozzá a URI a csatolt sablon. Egy sablont, a tárterület-fiók beállítása és a biztonsági jogkivonat létrehozása, című [Deploy erőforrások az erőforrás-kezelő sablonok és Azure PowerShell](resource-group-template-deploy.md) vagy [az erőforrás-kezelő sablonok és Azure CLI Deploy erőforrásokat](resource-group-template-deploy-cli.md). 

A következő példa bemutatja a szülő-sablon másik sablon mutató hivatkozást. A csatolt sablon a Társítások jogkivonat, amely a paraméterként átadott érhető el.

    "parameters": {
        "sasToken": { "type": "securestring" }
    },
    "resources": [
        {
            "apiVersion": "2015-01-01",
            "name": "linkedTemplate",
            "type": "Microsoft.Resources/deployments",
            "properties": {
              "mode": "incremental",
              "templateLink": {
                "uri": "[concat('https://storagecontosotemplates.blob.core.windows.net/templates/helloworld.json', parameters('sasToken'))]",
                "contentVersion": "1.0.0.0"
              }
            }
        }
    ],

Annak ellenére, hogy a token átadott a biztonságos karakterláncként, a csatolt sablon, beleértve a biztonsági jogkivonat URI be van jelentkezve a csoporthoz tartozó erőforrás üzembe helyezési műveleteket. Korlátozhatja a biztonsági kockázatot, állítsa be a jogkivonat-lejárati idejét.

## <a name="linking-to-a-parameter-file"></a>Paraméter fájl csatolása

A következő példában a **parametersLink** tulajdonság paraméter fájlra mutató hivatkozás.

    "resources": [ 
      { 
         "apiVersion": "2015-01-01", 
         "name": "linkedTemplate", 
         "type": "Microsoft.Resources/deployments", 
         "properties": { 
           "mode": "incremental", 
           "templateLink": {
              "uri":"https://www.contoso.com/AzureTemplates/newStorageAccount.json",
              "contentVersion":"1.0.0.0"
           }, 
           "parametersLink": { 
              "uri":"https://www.contoso.com/AzureTemplates/parameters.json",
              "contentVersion":"1.0.0.0"
           } 
         } 
      } 
    ] 

A paraméter csatolt fájl URI értéke nem lehet helyi fájl, és tartalmaznia kell a **http-** vagy a **https**. A paraméter fájl korlátozott hozzáférés a biztonsági jogkivonat is.

## <a name="using-variables-to-link-templates"></a>Hivatkozás: sablonok változók segítségével

Az előző példákban a sablon hivatkozások URL-cím értékeit csomagolásukkor mutatott. Ezt a megközelítést egyszerű sablon működni, de nem működik jól, ha nagyszámú elemes sablonok használata. Ehelyett statikus változó tároló alap URL-címe a fő-sablon létrehozása és majd dinamikusan URL-címek létrehozásához a csatolt sablonok az adott alap URL-címe. Az eljárás előnye egyszerűen áthelyezheti vagy ágaznak a sablont, mert csak akkor van szüksége a statikus változó a fő sablon módosítása. A fő sablon adja meg a lebontott sablon egész helyes URL-címe.

A következő példa bemutatja az alap URL használatát hozhat létre olyan csatolt sablonok (**sharedTemplateUrl** és **vmTemplate**) két URL-címét. 

    "variables": {
        "templateBaseUrl": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/postgresql-on-ubuntu/",
        "sharedTemplateUrl": "[concat(variables('templateBaseUrl'), 'shared-resources.json')]",
        "tshirtSizeSmall": {
            "vmSize": "Standard_A1",
            "diskSize": 1023,
            "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
            "vmCount": 2,
            "slaveCount": 1,
            "storage": {
                "name": "[parameters('storageAccountNamePrefix')]",
                "count": 1,
                "pool": "db",
                "map": [0,0],
                "jumpbox": 0
            }
        }
    }

Is segítségével [deployment()](resource-group-template-functions.md#deployment) alap URL-CÍMÉT az aktuális sablon, és segítségével, hogy az URL-cím más sablonok ugyanazon a helyen. Ez a módszer akkor hasznos, ha a sablon helyét (például miatt verziószámozás) változik, vagy el szeretné kerülni, hogy nehéz kódolása a sablonfájlt URL-címek. 

    "variables": {
        "sharedTemplateUrl": "[uri(deployment().properties.templateLink.uri, 'shared-resources.json')]"
    }

## <a name="conditionally-linking-to-templates"></a>Sablonok feltételesen csatolása

Más sablonok által átadása a paraméter értéke, amely a URI csatolt sablon létrehozása a hivatkozás. Ezt a megközelítést jól használható, amikor meg kell adni a használni kívánt sablon kapcsolódó a telepítés során. Megadhatja például egy meglévő tárterület-fiókból egy sablont, és egy másik sablont az új fiók tároló használatára.

A következő példa bemutatja egy paramétert a tárterület-fiók nevére, és megadhatja, hogy a tárterület-fiókkal-e új vagy meglévő paraméter.

    "parameters": {
        "storageAccountName": {
            "type": "String"
        },
        "newOrExisting": {
            "type": "String",
            "allowedValues": [
                "new",
                "existing"
            ]
        }
    },

A sablon, amely tartalmazza az új vagy meglévő paraméter értéke URI változó hoz létre.

    "variables": {
        "templatelink": "[concat('https://raw.githubusercontent.com/exampleuser/templates/master/',parameters('newOrExisting'),'StorageAccount.json')]"
    },

Adott változó értéket az üzembe erőforrás adja meg.

    "resources": [
        {
            "apiVersion": "2015-01-01",
            "name": "linkedTemplate",
            "type": "Microsoft.Resources/deployments",
            "properties": {
                "mode": "incremental",
                "templateLink": {
                    "uri": "[variables('templatelink')]",
                    "contentVersion": "1.0.0.0"
                },
                "parameters": {
                    "StorageAccountName": {
                        "value": "[parameters('storageAccountName')]"
                    }
                }
            }
        }
    ],

A URI úgy oldja fel névvel ellátott **existingStorageAccount.json** vagy a **newStorageAccount.json**sablonra. Sablonok létrehozása e URL-címe.

A következő példa bemutatja a **existingStorageAccount.json** sablont.

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "storageAccountName": {
          "type": "String"
        }
      },
      "variables": {},
      "resources": [],
      "outputs": {
        "storageAccountInfo": {
          "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName')),providers('Microsoft.Storage', 'storageAccounts').apiVersions[0])]",
          "type" : "object"
        }
      }
    }

A következő példa bemutatja a **newStorageAccount.json** sablont. Figyelje meg, hogy például a meglévő tárhelyét fiók sablon a tárolási fiók objektum ad vissza az a kimeneti értékeket. A fő sablon működik-e mindkét csatolt sablont.

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "storageAccountName": {
          "type": "string"
        }
      },
      "resources": [
        {
          "type": "Microsoft.Storage/storageAccounts",
          "name": "[parameters('StorageAccountName')]",
          "apiVersion": "2016-01-01",
          "location": "[resourceGroup().location]",
          "sku": {
            "name": "Standard_LRS"
          },
          "kind": "Storage",
          "properties": {
          }
        }
      ],
      "outputs": {
        "storageAccountInfo": {
          "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('StorageAccountName')),providers('Microsoft.Storage', 'storageAccounts').apiVersions[0])]",
          "type" : "object"
        }
      }
    }

## <a name="complete-example"></a>Kész példa

Az alábbi példa sablonok megjelenítése egy mutatja be, a jelen cikkben szereplő fogalmak számos csatolt sablonok egyszerűsített elrendezést. Azt feltételezi, hogy a sablonok felvettek egy tároló fiókban ugyanabban a tárolóban ki van kapcsolva, a nyilvános hozzáféréssel rendelkező. A csatolt sablon továbbítja egy értéket a **Kimenet** rész a fő sablont.

A **parent.json** fájl áll:

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "containerSasToken": { "type": "string" }
      },
      "resources": [
        {
          "apiVersion": "2015-01-01",
          "name": "linkedTemplate",
          "type": "Microsoft.Resources/deployments",
          "properties": {
            "mode": "incremental",
            "templateLink": {
              "uri": "[concat(uri(deployment().properties.templateLink.uri, 'helloworld.json'), parameters('containerSasToken'))]",
              "contentVersion": "1.0.0.0"
            }
          }
        }
      ],
      "outputs": {
        "result": {
          "type": "object",
          "value": "[reference('linkedTemplate').outputs.result]"
        }
      }
    }

A **helloworld.json** fájl áll:

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {},
      "variables": {},
      "resources": [],
      "outputs": {
        "result": {
            "value": "Hello World",
            "type" : "string"
        }
      }
    }
    
A PowerShell jogkivonat beszerzése a tároló, és a sablonok az üzembe:

    Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates
    $token = New-AzureStorageContainerSASToken -Name templates -Permission r -ExpiryTime (Get-Date).AddMinutes(30.0)
    New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateUri ("https://storagecontosotemplates.blob.core.windows.net/templates/parent.json" + $token) -containerSasToken $token

Azure CLI jogkivonat beszerzése a tároló, és a sablonok kódot a következő telepítése. Egy nevet a telepítéshez jelenleg meg kell adnia egy sablont, amely tartalmazza a biztonsági jogkivonat URI használata esetén.  

    expiretime=$(date -I'minutes' --date "+30 minutes")  
    azure storage container sas create --container templates --permissions r --expiry $expiretime --json | jq ".sas" -r
    azure group deployment create -g ExampleGroup --template-uri "https://storagecontosotemplates.blob.core.windows.net/templates/parent.json?{token}" -n tokendeploy  

A biztonsági jogkivonat paraméterként megadását kéri. Kell a jogkivonat **?**található.

## <a name="next-steps"></a>Következő lépések
- Az erőforrások telepítési sorrendjének megadása című témakörben talál [az Azure erőforrás-kezelő sablonok függőségeket meghatározó](resource-group-define-dependencies.md)
- Hogyan megadása egy erőforrást, de sok példányának létrehozása című témakörben talál [több példányával erőforrások az Azure erőforrás-kezelő létrehozása](resource-group-create-multiple.md)
