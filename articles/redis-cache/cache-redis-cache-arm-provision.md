<properties 
    pageTitle="Egy vgx.dll gyorsítótár kiépítése |} Microsoft Azure" 
    description="Erőforrás-kezelő Azure-sablon használatával az Azure vgx.dll gyorsítótár." 
    services="app-service" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="web" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/27/2016" 
    ms.author="sdanie"/>

# <a name="create-a-redis-cache-using-a-template"></a>Hozzon létre egy vgx.dll gyorsítótár sablon használatával

Ebben a témakörben megismerheti, hogyan hozhat létre egy erőforrás-kezelő Azure sablont, amely az Azure vgx.dll gyorsítótár üzembe helyezése. Diagnosztikai adatok megtartása a gyorsítótár használható egy meglévő tárterület-fiókkal. Azt is megtudhatja, hogyan határozza meg, hogy mely erőforrások van telepítve, és paramétert definiálásáról megadott mikor hajtja végre a telepítést. Ezt a sablont a saját telepítésekhez használja, vagy testre szabhatja a követelményeknek.

Jelenleg az összes gyorsítótárát ugyanabban a régióban előfizetéshez diagnosztikai beállítások megosztott. A tartományban lévő összes többi gyorsítótárát frissítése a tartományban lévő egyik gyorsítótár hatással van.

Sablonok létrehozásával kapcsolatos további tudnivalókért lásd: [Microsoft Azure erőforrás-kezelő sablonok létrehozása](../resource-group-authoring-templates.md).

A teljes sablon [Vgx.dll gyorsítótár sablon](https://github.com/Azure/azure-quickstart-templates/blob/master/101-redis-cache/azuredeploy.json)című témakör tartalmaz.

>[AZURE.NOTE] Az új [prémium réteg](cache-premium-tier-intro.md) erőforrás-kezelő sablonok érhetők el. 
>
>-    [Egy prémium vgx.dll gyorsítótár fürtözés létrehozása](https://azure.microsoft.com/documentation/templates/201-redis-premium-cluster-diagnostics/)
>-    [Adatok adatmegőrzési prémium vgx.dll gyorsítótár létrehozása](https://azure.microsoft.com/documentation/templates/201-redis-premium-persistence/)
>-    [Prémium vgx.dll gyorsítótár létrehozása VNet és a választható fürtözés](https://azure.microsoft.com/documentation/templates/201-redis-premium-vnet-cluster-diagnostics/)
>
>A legújabb sablonok kereséséhez [Azure quickstart útmutató](https://azure.microsoft.com/documentation/templates/) sablonok, és megkeresheti `Redis Cache`.

## <a name="what-you-will-deploy"></a>Mi telepíti

A sablonban telepíti az Azure, vgx.dll gyorsítótár diagnosztikai adatok egy meglévő tárterület-fiókot használ.

A telepítő automatikusan futtatásához kattintson a következő gombra:

[![Azure telepítése](./media/cache-redis-cache-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-redis-cache%2Fazuredeploy.json)

## <a name="parameters"></a>Paraméterek

Az Azure erőforrás-kezelő, akkor paraméterek beállítása értékeket szeretne megadni, ha telepíti a sablont. A sablonban szerepel, amely tartalmaz minden paraméterértékeket paraméterek nevű szakasz.
Be kell állítania egy ezeket az értékeket, hogy a projekt telepít alapján vagy a környezet, amelyben telepít paramétert. Nem paraméterek beállítása értékek, amelyek mindig érintetlen marad. Mindegyik paraméter segítségével a sablonban definiálása az erőforrásokat, van telepítve. 


[AZURE.INCLUDE [app-service-web-deploy-redis-parameters](../../includes/cache-deploy-parameters.md)]

### <a name="rediscachelocation"></a>redisCacheLocation

A vgx.dll gyorsítótár helyét. A legjobb teljesítmény elérése érdekében használata ugyanazon a helyen az alkalmazást a gyorsítótár használható.

    "redisCacheLocation": {
      "type": "string"
    }

### <a name="existingdiagnosticsstorageaccountname"></a>existingDiagnosticsStorageAccountName

A meglévő tároló fiók használatához a diagnosztikai neve. 

    "existingDiagnosticsStorageAccountName": {
      "type": "string"
    }

### <a name="enablenonsslport"></a>enableNonSslPort

Logikai érték, amely azt jelzi, hogy az SSL-portok keresztül hozzáférés engedélyezése.

    "enableNonSslPort": {
      "type": "bool"
    }

### <a name="diagnosticsstatus"></a>diagnosticsStatus

Egy érték, amely azt jelzi, hogy engedélyezve van-e a diagnosztika. Használat letiltásához.

    "diagnosticsStatus": {
      "type": "string",
      "defaultValue": "ON",
      "allowedValues": [
            "ON",
            "OFF"
        ]
    }
    
## <a name="resources-to-deploy"></a>Erőforrások terjesztése

### <a name="redis-cache"></a>Gyorsítótár vgx.dll

Létrehoz az Azure vgx.dll gyorsítótárból.

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('redisCacheName')]",
      "type": "Microsoft.Cache/Redis",
      "location": "[parameters('redisCacheLocation')]",
      "properties": {
        "enableNonSslPort": "[parameters('enableNonSslPort')]",
        "sku": {
          "capacity": "[parameters('redisCacheCapacity')]",
          "family": "[parameters('redisCacheFamily')]",
          "name": "[parameters('redisCacheSKU')]"
        }
      },
      "resources": [
        {
          "apiVersion": "2015-07-01",
          "type": "Microsoft.Cache/redis/providers/diagnosticsettings",
          "name": "[concat(parameters('redisCacheName'), '/Microsoft.Insights/service')]",
          "location": "[parameters('redisCacheLocation')]",
          "dependsOn": [
            "[concat('Microsoft.Cache/Redis/', parameters('redisCacheName'))]"
          ],
          "properties": {
            "status": "[parameters('diagnosticsStatus')]",
            "storageAccountName": "[parameters('existingDiagnosticsStorageAccountName')]"
          }
        }
      ]
    }



## <a name="commands-to-run-deployment"></a>Telepítési futtatandó parancsok

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)] 

### <a name="powershell"></a>A PowerShell

    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-redis-cache/azuredeploy.json -ResourceGroupName ExampleDeployGroup -redisCacheName ExampleCache -redisCacheLocation "West US"

### <a name="azure-cli"></a>Azure CLI

    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-redis-cache/azuredeploy.json -g ExampleDeployGroup


