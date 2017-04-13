<properties 
    pageTitle="Csatolt GitHub összegyűjti webes alkalmazások terjesztése" 
    description="Az erőforrás-kezelő Azure-sablon használatával egy tartalmazó GitHub összegyűjti a project web App alkalmazásban." 
    services="app-service" 
    documentationCenter="" 
    authors="cephalin" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="04/27/2016" 
    ms.author="cephalin"/>

# <a name="deploy-a-web-app-linked-to-a-github-repository"></a>Csatolt egy GitHub tárházba webes alkalmazások terjesztése

Ez a témakör megtanulhatja, hogyan hozhat létre egy erőforrás-kezelő Azure sablont, amely egy GitHub tárban tárolnak projekt csatolt webalkalmazást üzembe helyezése. Megtanulhatja, hogyan határozza meg, hogy mely erőforrások van telepítve, és paramétert definiálásáról megadott mikor hajtja végre a telepítést. Ezt a sablont a saját telepítésekhez használja, vagy testre szabhatja a követelményeknek.

Sablonok létrehozásával kapcsolatos további tudnivalókért lásd: [Microsoft Azure erőforrás-kezelő sablonok létrehozása](../resource-group-authoring-templates.md).

A teljes sablon című témakör tartalmaz [Web App csatolt GitHub sablonra](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-github-deploy/azuredeploy.json).

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="what-you-will-deploy"></a>Mi telepíti

Ezzel a sablonnal telepíti, amely tartalmazza a kódot a GitHub projektből webalkalmazást.

A telepítő automatikusan futtatásához kattintson a következő gombra:

[![Azure telepítése](./media/app-service-web-arm-from-github-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-github-deploy%2Fazuredeploy.json)

## <a name="parameters"></a>Paraméterek

[AZURE.INCLUDE [app-service-web-deploy-web-parameters](../../includes/app-service-web-deploy-web-parameters.md)]

### <a name="repourl"></a>repoURL

A projekt üzembe tartalmazó GitHub tárházba URL-CÍMÉT. A paraméter egy alapértelmezett értéket tartalmaz, de ezt az értéket mutatja, hogy miként URL-CÍMÉT a tárházba szükséges csak szolgál. Használhatja ezt az értéket, ha teszteli a sablont, de fog szeretne megadni a saját tárházba URL-CÍMÉT a sablon való munkavégzés során.

    "repoURL": {
        "type": "string",
        "defaultValue": "https://github.com/davidebbo-test/Mvc52Application.git"
    }

### <a name="branch"></a>fiók

Az alkalmazás telepítésekor használni a tárházba részlege. Az alapértelmezett érték fő, de megadhatja, hogy a telepíteni kívánt adattárban bármely fiókja nevét.

    "branch": {
        "type": "string",
        "defaultValue": "master"
    }
    
## <a name="resources-to-deploy"></a>Erőforrások terjesztése

[AZURE.INCLUDE [app-service-web-deploy-web-host](../../includes/app-service-web-deploy-web-host.md)]

### <a name="web-app"></a>Web App alkalmazásban

Hoz létre csatolt GitHub a a project web App alkalmazásban. 

A web app **siteName** paraméterrel nevét, és hol található a web app – a **siteLocation** paraméter megadása Az **dependsOn** elemet a sablon határozza meg a web app, a terv szolgáltatója szolgáltatás függ. Mivel függő az Office csomagot, a web app nem jön létre, amíg az Office csomagot nem fejeződött eredményezne. A **dependsOn** elem csak használatos telepítési sorrend meghatározásához. Ha nem jelöli be a web app, mint az Office csomagot függ, Azure erőforrás Mananger megpróbálja mindkét erőforrások létrehozni egy időben, és hiba jelenhet meg, ha az Office csomagot előtt hozza létre a web App alkalmazásban.

A web app az alábbi **források** részben megadott gyermek erőforrás tartalmaz. A gyermek erőforrás határozza meg, hogy a projekt a webalkalmazással rendszerbe adatforrás-vezérlő. Ez a sablon a verziókövetés egy adott GitHub tárházba van csatolva. A GitHub tárházba van megadva, a kódot tartalmazó **"RepoUrl": "https://github.com/davidebbo-test/Mvc52Application.git"** , előfordulhat, hogy merevlemez-kód tárházba URL-címe során létre szeretne hozni egy sablont, amely többször üzembe helyezése egyetlen projektként megkövetelése a lehető legkevesebb paraméterek közben.
Helyett a merevlemez-kódolási tárházba URL-CÍMÉT, paraméter hozzáadásának tárházba URL-címe, és használja ezt az értéket a **RepoUrl** tulajdonság.

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('siteName')]",
      "type": "Microsoft.Web/sites",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[resourceId('Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
      ],
      "properties": {
        "serverFarmId": "[parameters('hostingPlanName')]"
      },
      "resources": [
        {
          "apiVersion": "2015-08-01",
          "name": "web",
          "type": "sourcecontrols",
          "dependsOn": [
            "[resourceId('Microsoft.Web/Sites', parameters('siteName'))]"
          ],
          "properties": {
            "RepoUrl": "[parameters('repoURL')]",
            "branch": "[parameters('branch')]",
            "IsManualIntegration": true
          }
        }
      ]
    }

## <a name="commands-to-run-deployment"></a>Telepítési futtatandó parancsok

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>A PowerShell

    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-github-deploy/azuredeploy.json -siteName ExampleSite -hostingPlanName ExamplePlan -siteLocation "West US" -ResourceGroupName ExampleDeployGroup

### <a name="azure-cli"></a>Azure CLI

    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-github-deploy/azuredeploy.json


 
