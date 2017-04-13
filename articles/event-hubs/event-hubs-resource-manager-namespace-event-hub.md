<properties
    pageTitle="Hozzon létre egy esemény hubok névtér erőforrás-kezelő Azure-sablon segítségével esemény központi és a fogyasztói csoporttal |} Microsoft Azure"
    description="Hozzon létre egy esemény hubok névtér esemény központi és erőforrás-kezelő Azure-sablon segítségével fogyasztói csoport"
    services="event-hubs"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.devlang="tbd"
    ms.topic="article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="08/31/2016"
    ms.author="sethm;shvija"/>

# <a name="create-an-event-hubs-namespace-with-event-hub-and-consumer-group-using-an-azure-resource-manager-template"></a>Hozzon létre egy esemény hubok névtér erőforrás-kezelő Azure-sablon segítségével esemény központi és a fogyasztói csoporttal

Ez a cikk bemutatja, hogyan, amelyet egy esemény hubok névtér létrehoz egy eseményt hubhoz és a fogyasztói csoport Azure erőforrás-kezelő sablonnal. Megtanulhatja, hogyan határozza meg, hogy mely erőforrások van telepítve, és paramétert definiálásáról megadott mikor hajtja végre a telepítést. Ezt a sablont a saját telepítésekhez használja, vagy testre szabhatja az követelményeknek

Sablonok létrehozásával kapcsolatos további tudnivalókért lásd a [szerzői Azure erőforrás-kezelő sablonokat][].

A teljes sablon olvassa el a [esemény elosztót, és a fogyasztói csoport sablon][] a GitHub.

>[AZURE.NOTE]
>A legújabb sablonokat, látogasson el az [Azure quickstart útmutató][] sablongyűjteményben és a Keresés az esemény hubok.

## <a name="what-will-you-deploy"></a>Mit fog rendszerbe?

Ezzel a sablonnal egy esemény hubok névtér az esemény központi és a fogyasztói csoport telepíti.

[Esemény hubok](../event-hubs/event-hubs-what-is-event-hubs.md) ahhoz, hogy bejövő Eseménynapló és telemetriai adatok Azure tömeges méreteket, a kis késés és a magas megbízhatóság használt szolgáltatás feldolgozása eseményről szó.

A telepítő automatikusan futtatásához kattintson a következő gombra:

[![Azure telepítése](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-event-hubs-create-event-hub-and-consumer-group%2Fazuredeploy.json)

## <a name="parameters"></a>Paraméterek

Az Azure erőforrás-kezelő, akkor paraméterek beállítása értékeket szeretne megadni, ha telepíti a sablont. A sablonban szerepel nevű szakasz `Parameters` , amely tartalmazza az összes paraméterértékeket. Be kell állítania a paramétert, ezek az értékek, amelyek alapján a projekten telepít vagy a környezet, amelyben telepít változhatnak. Nem paraméterek beállítása értékek, amelyek mindig érintetlen marad. Mindegyik paraméter segítségével a sablonban definiálása az erőforrásokat, van telepítve.

A sablon a következő paraméterek határozza meg.

### <a name="eventhubnamespacename"></a>eventHubNamespaceName

Az esemény hubok névtér létrehozása neve.

```
"eventHubNamespaceName": {
"type": "string"
}
```

### <a name="eventhubname"></a>eventHubName

A nevét az esemény-központban létrehozott az Eseménynapló hubok névtér.

```
"eventHubName": {
"type": "string"
}
```

### <a name="eventhubconsumergroupname"></a>eventHubConsumerGroupName

Az esemény-központban létrehozott fogyasztói csoport nevét.

```
"eventHubConsumerGroupName": {
"type": "string"
}
```

### <a name="apiversion"></a>apiVersion

A sablon API verzióját.

```
"apiVersion": {
"type": "string"
}
```

## <a name="resources-to-deploy"></a>Erőforrások terjesztése

Egy esemény elosztót, és a fogyasztói csoport típusú **EventHubs**, névteret hoz létre.

```
"resources":[  
      {  
         "apiVersion":"[variables('ehVersion')]",
         "name":"[parameters('namespaceName')]",
         "type":"Microsoft.EventHub/Namespaces",
         "location":"[variables('location')]",
         "sku":{  
            "name":"Standard",
            "tier":"Standard"
         },
         "resources":[  
            {  
               "apiVersion":"[variables('ehVersion')]",
               "name":"[parameters('eventHubName')]",
               "type":"EventHubs",
               "dependsOn":[  
                  "[concat('Microsoft.EventHub/namespaces/', parameters('namespaceName'))]"
               ],
               "properties":{  
                  "path":"[parameters('eventHubName')]"
               },
               "resources":[  
                  {  
                     "apiVersion":"[variables('ehVersion')]",
                     "name":"[parameters('consumerGroupName')]",
                     "type":"ConsumerGroups",
                     "dependsOn":[  
                        "[parameters('eventHubName')]"
                     ],
                     "properties":{  

                     }
                  }
               ]
            }
         ]
      }
   ],
```

## <a name="commands-to-run-deployment"></a>Telepítési futtatandó parancsok

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>A PowerShell

```
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-event-hubs-create-event-hub-and-consumer-group/azuredeploy.json
```

## <a name="azure-cli"></a>Azure CLI

```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri [https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-event-hubs-create-event-hub-and-consumer-group/azuredeploy.json][]
```

[Erőforrás-kezelő Azure-sablonok létrehozása]: ../resource-group-authoring-templates.md
[Azure quickstart útmutató sablonok]:  https://azure.microsoft.com/documentation/templates/?term=event+hubs
[Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
[Esemény központi és a fogyasztói csoport sablon]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-event-hubs-create-event-hub-and-consumer-group/
