<properties
    pageTitle="Az erőforrás-kezelő Azure-sablon segítségével várólista szolgáltatást Bus névtér létrehozása |} Microsoft Azure"
    description="A szolgáltatás Bus névtér és erőforrás-kezelő Azure-sablon segítségével várólista létrehozása"
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

# <a name="create-a-service-bus-namespace-and-a-queue-using-an-azure-resource-manager-template"></a>A szolgáltatás Bus névtér és erőforrás-kezelő Azure-sablon segítségével várólista létrehozása

Ez a cikk bemutatja, hogyan által létrehozott szolgáltatás Bus névteret és várólista Azure erőforrás-kezelő sablonnal. Megtanulhatja, hogyan határozza meg, hogy mely erőforrások van telepítve, és paramétert definiálásáról megadott mikor hajtja végre a telepítést. Ezt a sablont a saját telepítésekhez használja, vagy testre szabhatja a követelményeknek.

Sablonok létrehozásával kapcsolatos további tudnivalókért lásd a [szerzői Azure erőforrás-kezelő sablonokat][].

A teljes sablon nézze meg a [szolgáltatás Bus névtér és várólista sablon][] GitHub.

>[AZURE.NOTE] Az alábbi erőforrás-kezelő Azure-sablonok letöltése és telepítéséhez érhetők el.
>
>-    [Szolgáltatás Bus névtér várólista és engedélyezési szabály létrehozása](service-bus-resource-manager-namespace-auth-rule.md)
>-    [A témakör és előfizetés szolgáltatás Bus névtér létrehozása](service-bus-resource-manager-namespace-topic.md)
>-    [Szolgáltatás Bus névtér létrehozása](service-bus-resource-manager-namespace.md)
>-    [Hozzon létre egy esemény hubok névtér esemény központi és a fogyasztói csoporttal](../event-hubs/event-hubs-resource-manager-namespace-event-hub.md)
>
>A legújabb sablonokat, látogasson el az [Azure quickstart útmutató][] sablongyűjteményben, és keressen a "Szolgáltatás Bus."

## <a name="what-will-you-deploy"></a>Mit fog rendszerbe?

Ezzel a sablonnal tartalmazó várólista szolgáltatást Bus névtér telepíti.

[Szolgáltatás Bus sorban várakozó](service-bus-queues-topics-subscriptions.md#queues) tud nyújtani első, első ki (FIFO) üzenetkézbesítés egy vagy több versengő felhasználóknak.

A telepítő automatikusan futtatásához kattintson a következő gombra:

[![Azure telepítése](./media/service-bus-resource-manager-namespace-queue/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-queue%2Fazuredeploy.json)

## <a name="parameters"></a>Paraméterek

Az Azure erőforrás-kezelő, akkor paraméterek beállítása értékeket szeretne megadni, ha telepíti a sablont. A sablonban szerepel a nevű szakasz `Parameters` , amely tartalmazza az összes paraméterértékeket. Be kell állítania a paramétert, ezek az értékek, amelyek alapján a projekten telepít vagy a környezet, amelyben telepít változhatnak. Nem paraméterek beállítása értékek, amelyek mindig érintetlen marad. Mindegyik paraméter segítségével a sablonban definiálása az erőforrásokat, van telepítve.

A sablon a következő paraméterek határozza meg.

### <a name="servicebusnamespacename"></a>serviceBusNamespaceName

A szolgáltatás Bus névtér létrehozása neve.

```
"serviceBusNamespaceName": {
"type": "string",
"metadata": { 
    "description": "Name of the Service Bus namespace" 
    }
}
```

### <a name="servicebusqueuename"></a>serviceBusQueueName

A szolgáltatás Bus névtér létrehozott várólista neve.

```
"serviceBusQueueName": {
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

Szokásos szolgáltatás Bus névteret **üzenetküldés**, típusú várólista hoz létre.

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
            "name": "[parameters('serviceBusQueueName')]",
            "type": "Queues",
            "dependsOn": [
                "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
            ],
            "properties": {
                "path": "[parameters('serviceBusQueueName')]",
            }
        }]
    }]
```

## <a name="commands-to-run-deployment"></a>Telepítési futtatandó parancsok

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>A PowerShell

```
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-queue/azuredeploy.json>
```

## <a name="azure-cli"></a>Azure CLI

```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-queue/azuredeploy.json>
```

## <a name="next-steps"></a>Következő lépések

Most, hogy már létrehozott és Azure erőforrás-kezelővel erőforrások rendszerbe, megtudhatja, hogy miként kezelheti, ezek az erőforrások és ezek a cikkek megtekintése:

- [A PowerShell szolgáltatás Bus kezelése](service-bus-powershell-how-to-provision.md)
- [A szolgáltatás Bus Intézővel szolgáltatás Bus erőforrások kezelése](https://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)


  [Erőforrás-kezelő Azure-sablonok létrehozása]: ../resource-group-authoring-templates.md
  [Szolgáltatás Bus névtér és várólista sablon]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/
  [Azure quickstart útmutató sablonok]: https://azure.microsoft.com/documentation/templates/?term=service+bus
  [Learn more about Service Bus queues]: service-bus-queues-topics-subscriptions.md
  [Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
  [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
