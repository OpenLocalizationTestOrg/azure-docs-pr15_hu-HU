<properties
    pageTitle="Hozzon létre szolgáltatás Bus névteret témakör, az előfizetést, és a szabály egy erőforrás-kezelő Azure-sablon segítségével |} Microsoft Azure"
    description="Szolgáltatás Bus névtér témakör, előfizetés és erőforrás-kezelő Azure-sablon segítségével szabály létrehozása"
    services="service-bus"
    documentationCenter=".net"
    authors="ShubhaVijayasarathy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="tbd"
    ms.topic="article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="10/25/2016"
    ms.author="ShubhaVijayasarathy"/>

# <a name="create-a-service-bus-namespace-with-topic-subscription-and-rule-using-an-azure-resource-manager-template"></a>Szolgáltatás Bus névtér témakör, előfizetés és erőforrás-kezelő Azure-sablon segítségével szabály létrehozása

Ez a cikk bemutatja, hogyan Azure erőforrás-kezelő sablonnal, amely a szolgáltatás Bus névteret létrehoz egy témát, előfizetés és szabály (szűrő). Megismerheti, hogyan határozza meg, hogy mely erőforrások van telepítve, és paramétert definiálásáról megadott, amikor a telepítő hajtja végre. Ezt a sablont a saját telepítésekhez használja, vagy testre szabhatja az követelményeknek

Sablonok létrehozásával kapcsolatos további tudnivalókért lásd: [Azure erőforrás-kezelő létrehozáshoz használható sablonok][].

További információt a gyakorlat és mintázatok a Azure erőforrások elnevezési szabályai című témakörben talál [Azure erőforrások elnevezési szabályokat][].

A teljes sablon [szolgáltatás Bus névtér témakör, az előfizetést, és a szabály][] sablon látható.

>[AZURE.NOTE] Az alábbi erőforrás-kezelő Azure-sablonok letöltése és telepítéséhez érhetők el.
>
>-    [Szolgáltatás Bus névtér várólista és engedélyezési szabály létrehozása](service-bus-resource-manager-namespace-auth-rule.md)
>-    [Hozzon létre egy szolgáltatás Bus névtér várólista](service-bus-resource-manager-namespace-queue.md)
>-    [Szolgáltatás Bus névtér létrehozása](service-bus-resource-manager-namespace.md)
>-    [A témakör és előfizetés szolgáltatás Bus névtér létrehozása](service-bus-resource-manager-namespace-topic.md)
>
>A legújabb sablonokat, látogasson el az [Azure quickstart útmutató][] sablongyűjteményben és szolgáltatás Bus keresése.

## <a name="what-will-you-deploy"></a>Mit fog rendszerbe?

Ezzel a sablonnal témakör, előfizetés és szabály (szűrő) szolgáltatás Bus névteret rendszerbe.

[Szolgáltatás Bus témakörök és előfizetések](service-bus-queues-topics-subscriptions.md#topics-and-subscriptions) adja meg egy-a-többhöz megjeleníthető a kommunikációt a *Közzététel/előfizetés* mintát. Elosztott alkalmazás részei nem egymással közvetlenül, témák és az előfizetéseket, használata esetén inkább azok üzenetváltásra témakört, amely végpontként közvetítő keresztül. A tematikus-előfizetést hasonló, akkor egy virtuális várólista, amely a témakörre küldött üzenetek példányban kapja. Előfizetés lehetővé teszi, hogy a szűrő megadhatja, melyik témakör küldött üzenetek belül megjelennek a témakör bizonyos előfizetést.

## <a name="what-are-rules-filters"></a>Mik azok a szabályok (szűrők)?

Sok esetben az adott jellemzők üzeneteket különböző módokon fel kell dolgozni. Engedélyezéséhez beállíthatja az előfizetések tulajdonságok van szükség, és hajtsa végre az a tulajdonságokat bizonyos módosítások üzenetek kereséséhez. Szolgáltatás Bus előfizetések a témakör küldött összes üzenet jelenik meg, amíg a virtuális előfizetés sorba csak másolhatja azokat az üzeneteket egy részét. A elvégezni, előfizetés szűrők használatával. Rules(filters) a további tudnivalókért olvassa el a [szolgáltatás Bus sorban várakozó, a témakörök és az előfizetések][]című témakört.

A telepítő automatikusan futtatásához kattintson a következő gombra:

[![Azure telepítése](./media/service-bus-resource-manager-namespace-topic/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-topic-subscription-rule%2Fazuredeploy.json)

## <a name="parameters"></a>Paraméterek

Az Azure erőforrás-kezelő, be kell állítania paramétereinek értékeket szeretne megadni, ha telepíti a sablont. A sablonban szerepel a nevű szakasz `Parameters` , amely tartalmazza a paraméterértékeket. Be kell állítania egy ezeket az értékeket, hogy a projekt telepít alapján vagy a környezet, amelyben telepít paramétert. Nem paraméterek beállítása értékek, amelyek mindig érintetlen marad. Mindegyik paraméter segítségével a sablonban definiálása az erőforrásokat, van telepítve.

A sablon a következő paraméterek határozza meg:

### <a name="servicebusnamespacename"></a>serviceBusNamespaceName

A szolgáltatás Bus névtér létrehozása neve.

```
"serviceBusNamespaceName": {
"type": "string"
}
```

### <a name="servicebustopicname"></a>serviceBusTopicName

A témakör a szolgáltatás Bus névtér létrehozott neve.

```
"serviceBusTopicName": {
"type": "string"
}
```

### <a name="servicebussubscriptionname"></a>serviceBusSubscriptionName

A szolgáltatás Bus névtér létrehozott az előfizetése nevére.

```
"serviceBusSubscriptionName": {
"type": "string"
}
```
### <a name="servicebusrulename"></a>serviceBusRuleName

A szolgáltatás Bus névtér létrehozott rule(filter) neve.

```
   "serviceBusRuleName": {
   "type": "string",
  }
```
### <a name="servicebusapiversion"></a>serviceBusApiVersion

A szolgáltatás Bus API verziója a sablont.

```
"serviceBusApiVersion": {
"type": "string"
}
```
## <a name="resources-to-deploy"></a>Erőforrások terjesztése

Szokásos szolgáltatás Bus névteret típusú **üzenetküldés**, témakör és előfizetés és szabályok létrehozása

```
 "resources": [{
        "apiVersion": "[variables('sbVersion')]",
        "name": "[parameters('serviceBusNamespaceName')]",
        "type": "Microsoft.ServiceBus/Namespaces",
        "location": "[variables('location')]",
        "sku": {
            "name": "Standard",
            "tier": "Standard"
        },
        "resources": [{
            "apiVersion": "[variables('sbVersion')]",
            "name": "[parameters('serviceBusTopicName')]",
            "type": "Topics",
            "dependsOn": [
                "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
            ],
            "properties": {
                "path": "[parameters('serviceBusTopicName')]"
            },
            "resources": [{
                "apiVersion": "[variables('sbVersion')]",
                "name": "[parameters('serviceBusSubscriptionName')]",
                "type": "Subscriptions",
                "dependsOn": [
                    "[parameters('serviceBusTopicName')]"
                ],
                "properties": {},
                "resources": [{
                    "apiVersion": "[variables('sbVersion')]",
                    "name": "[parameters('serviceBusRuleName')]",
                    "type": "Rules",
                    "dependsOn": [
                        "[parameters('serviceBusSubscriptionName')]"
                    ],
                    "properties": {
                        "filter": {
                            "sqlExpression": "StoreName = 'Store1'"
                        },
                        "action": {
                            "sqlExpression": "set FilterTag = 'true'"
                        }
                    }
                }]
            }]
        }]
    }]
```

## <a name="commands-to-run-deployment"></a>Telepítési futtatandó parancsok

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>A PowerShell

```
New-AzureResourceGroupDeployment -Name \<deployment-name\> -ResourceGroupName \<resource-group-name\> -TemplateUri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-subscription-rule/azuredeploy.json>
```

## <a name="azure-cli"></a>Azure CLI

```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-subscription-rule/azuredeploy.json>
```

## <a name="next-steps"></a>Következő lépések

Most, hogy már létrehozott és Azure erőforrás-kezelővel erőforrások rendszerbe, megtudhatja, hogy miként kezelheti, ezek az erőforrások és ezek a cikkek megtekintése:

- [Azure Service Bus Azure automatizálási használatával kezelése](service-bus-automation-manage.md)
- [A PowerShell szolgáltatás Bus kezelése](service-bus-powershell-how-to-provision.md)
- [A szolgáltatás Bus Intézővel szolgáltatás Bus erőforrások kezelése](https://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)


  [Erőforrás-kezelő Azure-sablonok létrehozása]: ../resource-group-authoring-templates.md
  [Azure quickstart útmutató sablonok]: https://azure.microsoft.com/documentation/templates/?term=service+bus
  [Learn more about Service Bus topics and subscriptions]: service-bus-queues-topics-subscriptions.md
  [Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
  [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
  [Elnevezési szabályokat Azure erőforrások]: https://azure.microsoft.com/en-us/documentation/articles/guidance-naming-conventions/
  [Témakör, az előfizetést és a szabály szolgáltatás Bus névtere]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-topic-subscription-rule/
  [Szolgáltatás Bus sorban várakozó, témák és előfizetések]:service-bus-queues-topics-subscriptions.md
  
