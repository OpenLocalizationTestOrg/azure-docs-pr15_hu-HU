<properties
    pageTitle="A témakör és erőforrás-kezelő Azure-sablon segítségével előfizetés szolgáltatás Bus névtér létrehozása |} Microsoft Azure"
    description="A témakör és erőforrás-kezelő Azure-sablon segítségével előfizetés szolgáltatás Bus névtér létrehozása"
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="tbd"
    ms.topic="article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="10/14/2016"
    ms.author="sethm;shvija"/>

# <a name="create-a-service-bus-namespace-with-topic-and-subscription-using-an-azure-resource-manager-template"></a>A témakör és erőforrás-kezelő Azure-sablon segítségével előfizetés szolgáltatás Bus névtér létrehozása

Ez a cikk bemutatja, hogyan, amelyet a szolgáltatás Bus névteret létrehoz egy témakör és előfizetés Azure erőforrás-kezelő sablonnal. Megtanulhatja, hogyan határozza meg, hogy mely erőforrások van telepítve, és paramétert definiálásáról megadott mikor hajtja végre a telepítést. Ezt a sablont a saját telepítésekhez használja, vagy testre szabhatja az követelményeknek

Sablonok létrehozásával kapcsolatos további tudnivalókért lásd a [szerzői Azure erőforrás-kezelő sablonokat][].

A teljes sablon [szolgáltatás Bus névtér témakör és előfizetés][] sablon látható.

>[AZURE.NOTE] Az alábbi erőforrás-kezelő Azure-sablonok letöltése és telepítéséhez érhetők el.
>
>-    [Szolgáltatás Bus névtér várólista és engedélyezési szabály létrehozása](service-bus-resource-manager-namespace-auth-rule.md)
>-    [Hozzon létre egy szolgáltatás Bus névtér várólista](service-bus-resource-manager-namespace-queue.md)
>-    [Szolgáltatás Bus névtér létrehozása](service-bus-resource-manager-namespace.md)
>-    [Hozzon létre egy esemény hubok névtér esemény központi és a fogyasztói csoporttal](../event-hubs/event-hubs-resource-manager-namespace-event-hub.md)
>
>A legújabb sablonokat, látogasson el az [Azure quickstart útmutató][] sablongyűjteményben, és keressen a "Szolgáltatás Bus."

## <a name="what-will-you-deploy"></a>Mit fog rendszerbe?

Ezzel a sablonnal telepíti a szolgáltatást Bus névteret témakör és előfizetés.

[Szolgáltatás Bus témakörök és előfizetések](service-bus-queues-topics-subscriptions.md#topics-and-subscriptions) adja meg egy-a-többhöz megjeleníthető a kommunikációt a *Közzététel/előfizetés* mintát.

A telepítő automatikusan futtatásához kattintson a következő gombra:

[![Azure telepítése](./media/service-bus-resource-manager-namespace-topic/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-topic-and-subscription%2Fazuredeploy.json)

## <a name="parameters"></a>Paraméterek

Az Azure erőforrás-kezelő, akkor paraméterek beállítása értékeket szeretne megadni, ha telepíti a sablont. A sablonban szerepel a nevű szakasz `Parameters` , amely tartalmazza az összes paraméterértékeket. Be kell állítania a paramétert, ezek az értékek, amelyek alapján a projekten telepít vagy a környezet, amelyben telepít változhatnak. Nem paraméterek beállítása értékek, amelyek mindig érintetlen marad. Mindegyik paraméter segítségével a sablonban definiálása az erőforrásokat, van telepítve.

A sablon a következő paraméterek határozza meg.

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

### <a name="servicebusapiversion"></a>serviceBusApiVersion

A szolgáltatás Bus API verziója a sablont.

```
"serviceBusApiVersion": {
"type": "string"
}
```
## <a name="resources-to-deploy"></a>Erőforrások terjesztése

Létrehoz egy szokásos szolgáltatás Bus névtér típusú **üzenetküldés**, témakör és az előfizetés.

```
"resources ": [{
        "apiVersion": "[variables('sbVersion')]",
        "name": "[parameters('serviceBusNamespaceName')]",
        "type": "Microsoft.ServiceBus/Namespaces",
        "location": "[variables('location')]",
        "kind": "Messaging",
        "sku": {
            "name": "StandardSku",
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
                "path": "[parameters('serviceBusTopicName')]",
            },
            "resources": [{
                "apiVersion": "[variables('sbVersion')]",
                "name": "[parameters('serviceBusSubscriptionName')]",
                "type": "Subscriptions",
                "dependsOn": [
                    "[parameters('serviceBusTopicName')]"
                ],
                "properties": {}
            }]
        }]
    }]
```

## <a name="commands-to-run-deployment"></a>Telepítési futtatandó parancsok

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>A PowerShell

```
New-AzureResourceGroupDeployment -Name \<deployment-name\> -ResourceGroupName \<resource-group-name\> -TemplateUri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-and-subscription/azuredeploy.json>
```

## <a name="azure-cli"></a>Azure CLI

```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-and-subscription/azuredeploy.json>
```

## <a name="next-steps"></a>Következő lépések

Most, hogy már létrehozott és Azure erőforrás-kezelővel erőforrások rendszerbe, megtudhatja, hogy miként kezelheti, ezek az erőforrások és ezek a cikkek megtekintése:

- [A PowerShell szolgáltatás Bus kezelése](service-bus-powershell-how-to-provision.md)
- [A szolgáltatás Bus Intézővel szolgáltatás Bus erőforrások kezelése](https://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)


  [Erőforrás-kezelő Azure-sablonok létrehozása]: ../resource-group-authoring-templates.md
  [Azure quickstart útmutató sablonok]: https://azure.microsoft.com/documentation/templates/?term=service+bus
  [Learn more about Service Bus topics and subscriptions]: service-bus-queues-topics-subscriptions.md
  [Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
  [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
  [A témakör és előfizetés szolgáltatás Bus névtere]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-topic-and-subscription/
