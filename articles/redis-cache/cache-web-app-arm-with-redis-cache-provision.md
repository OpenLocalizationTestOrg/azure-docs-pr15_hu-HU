<properties 
    pageTitle="A gyorsítótár vgx.dll rendelkezést Web App alkalmazásban" 
    description="Erőforrás-kezelő Azure-sablon használatával vgx.dll gyorsítótár web app." 
    services="app-service" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erickson-doug" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="create-a-web-app-plus-redis-cache-using-a-template"></a>Hozzon létre egy Web App, valamint a vgx.dll gyorsítótár sablon használatával

Ebben a témakörben megtanulhatja, hogy miként hozhat létre egy erőforrás-kezelő Azure sablont, amely az Azure-Webappokban vgx.dll gyorsítótárban való üzembe helyezése. Megtanulhatja, hogyan határozza meg, hogy mely erőforrások van telepítve, és paramétert definiálásáról megadott mikor hajtja végre a telepítést. Ezt a sablont a saját telepítésekhez használja, vagy testre szabhatja a követelményeknek.

Sablonok létrehozásával kapcsolatos további tudnivalókért lásd: [Microsoft Azure erőforrás-kezelő sablonok létrehozása](../resource-group-authoring-templates.md).

A teljes sablon [Web App gyorsítótárának vgx.dll sablonnal](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-with-redis-cache/azuredeploy.json)című témakör tartalmaz.

## <a name="what-you-will-deploy"></a>Mi telepíti

Ezzel a sablonnal telepíti:

- Azure Web App alkalmazásban
- Azure vgx.dll gyorsítótár.

A telepítő automatikusan futtatásához kattintson a következő gombra:

[![Azure telepítése](./media/cache-web-app-arm-with-redis-cache-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-with-redis-cache%2Fazuredeploy.json)

## <a name="parameters-to-specify"></a>Paraméter megadása

[AZURE.INCLUDE [app-service-web-deploy-web-parameters](../../includes/app-service-web-deploy-web-parameters.md)]

[AZURE.INCLUDE [cache-deploy-parameters](../../includes/cache-deploy-parameters.md)]

## <a name="variables-for-names"></a>Nevek változói

Ezzel a sablonnal össze az erőforrások nevei változók használja. Az erőforráscsoport azonosítója alapuló összeállításához a [uniqueString](../resource-group-template-functions.md#uniquestring) függvényt azt használja.

    "variables": {
      "hostingPlanName": "[concat('hostingplan', uniqueString(resourceGroup().id))]",
      "webSiteName": "[concat('webSite', uniqueString(resourceGroup().id))]",
      "cacheName": "[concat('cache', uniqueString(resourceGroup().id))]"
    },


## <a name="resources-to-deploy"></a>Erőforrások terjesztése

[AZURE.INCLUDE [app-service-web-deploy-web-host](../../includes/app-service-web-deploy-web-host.md)]

### <a name="redis-cache"></a>Gyorsítótár vgx.dll

Hoz létre, amely a webalkalmazással Azure vgx.dll gyorsítótár. A gyorsítótár nevét a **cacheName** változó van megadva.

A sablon a gyorsítótár erőforráscsoport ugyanazon a helyen hoz létre. 

    {
      "name": "[variables('cacheName')]",
      "type": "Microsoft.Cache/Redis",
      "location": "[resourceGroup().location]",
      "apiVersion": "2015-08-01",
      "dependsOn": [ ],
      "tags": {
        "displayName": "cache"
      },
      "properties": {
        "sku": {
          "name": "[parameters('cacheSKUName')]",
          "family": "[parameters('cacheSKUFamily')]",
          "capacity": "[parameters('cacheSKUCapacity')]"
        }
      }
    }


### <a name="web-app"></a>Web App alkalmazásban

A web app hoz megadott változó **Webhelynév helyére írja be** a nevet.

Figyelje meg, hogy a web app alkalmazás beállítás tulajdonságok, amelyek lehetővé teszik a vgx.dll gyorsítótár használata van-e beállítva. Az alkalmazás beállításai készült dinamikusan a telepítés során megadott értékek alapján.
        
    {
      "apiVersion": "2015-08-01",
      "name": "[variables('webSiteName')]",
      "type": "Microsoft.Web/sites",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Web/serverFarms/', variables('hostingPlanName'))]",
        "[concat('Microsoft.Cache/Redis/', variables('cacheName'))]"
      ],
      "tags": {
        "[concat('hidden-related:', resourceGroup().id, '/providers/Microsoft.Web/serverfarms/', variables('hostingPlanName'))]": "empty",
        "displayName": "Website"
      },
      "properties": {
        "name": "[variables('webSiteName')]",
        "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]"
      },
      "resources": [
        {
          "apiVersion": "2015-08-01",
          "type": "config",
          "name": "appsettings",
          "dependsOn": [
            "[concat('Microsoft.Web/Sites/', variables('webSiteName'))]",
            "[concat('Microsoft.Cache/Redis/', variables('cacheName'))]"
          ],
          "properties": {
            "CacheConnection": "[concat(variables('cacheName'),'.redis.cache.windows.net,abortConnect=false,ssl=true,password=', listKeys(resourceId('Microsoft.Cache/Redis', variables('cacheName')), '2015-08-01').primaryKey)]"
          }
        }
      ]
    }

## <a name="commands-to-run-deployment"></a>Telepítési futtatandó parancsok

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>A PowerShell

    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-with-redis-cache/azuredeploy.json -ResourceGroupName ExampleDeployGroup

### <a name="azure-cli"></a>Azure CLI

    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-with-redis-cache/azuredeploy.json -g ExampleDeployGroup


