<properties 
    pageTitle="Erőforrás-kezelő Azure sablonok használata a Azure alkalmazás szolgáltatás logika alkalmazás létrehozása |} Microsoft Azure" 
    description="Egy erőforrás-kezelő Azure-sablon segítségével egy üres logika alkalmazások terjesztése munkafolyamatok meghatározásához." 
    services="logic-apps" 
    documentationCenter="" 
    authors="MSFTMan" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/25/2016" 
    ms.author="deonhe"/>

# <a name="create-a-logic-app-using-a-template"></a>A sablon használatával összefüggés-alkalmazás létrehozása

Hozzon létre egy üres logika alkalmazás meghatározásához munkafolyamatokat használható egy Azure erőforrás-kezelő sablonnal. Meghatározhatja, hogy mely erőforrások van telepítve, és hogyan paraméterek vannak megadva, a telepítő futtatásakor. Ezt a sablont a saját telepítésekhez használja, vagy testre szabhatja a követelményeknek.

Logika alkalmazás tulajdonságait, lásd: [Logika alkalmazás munkafolyamat Management API -val](https://msdn.microsoft.com/library/azure/mt643788.aspx). 

Példák a definíció magát című témakörben talál [Szerző logika alkalmazás definíciók](app-service-logic-author-definitions.md). 

Sablonok létrehozásával kapcsolatos további tudnivalókért lásd: [Microsoft Azure erőforrás-kezelő sablonok létrehozása](../resource-group-authoring-templates.md).

A teljes sablon [logika alkalmazássablon](https://github.com/Azure/azure-quickstart-templates/blob/master/101-logic-app-create/azuredeploy.json)című témakör tartalmaz.

## <a name="what-you-will-deploy"></a>Mi telepíti

Ezzel a sablonnal logika alkalmazások terjesztése.

A telepítő automatikusan futtatni, jelölje be a következő gombra:  

[![Azure telepítése](media/app-service-logic-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-logic-app-create%2Fazuredeploy.json)

## <a name="parameters"></a>Paraméterek

[AZURE.INCLUDE [app-service-logic-deploy-parameters](../../includes/app-service-logic-deploy-parameters.md)]

### <a name="testuri"></a>testUri

     "testUri": {
        "type": "string",
        "defaultValue": "http://azure.microsoft.com/en-us/status/feed/"
      }
    
## <a name="resources-to-deploy"></a>Erőforrások terjesztése

### <a name="logic-app"></a>Logika alkalmazás

A összefüggés-alkalmazás létrehozása

A sablonok a paraméter értéke az összefüggés-alkalmazás nevét használja. Hol található a logika alkalmazást az erőforráscsoport ugyanazon a helyen értékre állítja. 

Ez a meghatározás óránként lefut, és a helyet, a **testUri** paraméterben megadott pingelése. 

    {
      "type": "Microsoft.Logic/workflows",
      "apiVersion": "2016-06-01",
      "name": "[parameters('logicAppName')]",
      "location": "[resourceGroup().location]",
      "tags": {
        "displayName": "LogicApp"
      },
      "properties": {
        "definition": {
          "$schema": "http://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "testURI": {
              "type": "string",
              "defaultValue": "[parameters('testUri')]"
            }
          },
          "triggers": {
            "recurrence": {
              "type": "recurrence",
              "recurrence": {
                "frequency": "Hour",
                "interval": 1
              }
            }
          },
          "actions": {
            "http": {
              "type": "Http",
              "inputs": {
                "method": "GET",
                "uri": "@parameters('testUri')"
              },
              "runAfter": {}
            }
          },
          "outputs": {}
        },
        "parameters": {}
      }
    }


## <a name="commands-to-run-deployment"></a>Telepítési futtatandó parancsok

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>A PowerShell

    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json -ResourceGroupName ExampleDeployGroup

### <a name="azure-cli"></a>Azure CLI

    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json -g ExampleDeployGroup


 
