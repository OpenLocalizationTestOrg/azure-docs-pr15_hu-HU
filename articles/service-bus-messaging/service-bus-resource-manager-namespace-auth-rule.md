<properties
    pageTitle="Erőforrás-kezelő Azure-sablon segítségével Bus szolgáltatás engedélyezési szabály létrehozása |} Microsoft Azure"
    description="Névtér és erőforrás-kezelő Azure-sablon segítségével várólista szolgáltatást Bus engedélyezési szabály létrehozása"
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

# <a name="create-a-service-bus-authorization-rule-for-namespace-and-queue-using-an-azure-resource-manager-template"></a>Névtér és erőforrás-kezelő Azure-sablon segítségével várólista szolgáltatást Bus engedélyezési szabály létrehozása

Ez a cikk bemutatja, hogyan Azure erőforrás-kezelő sablonnal, amely egy szolgáltatás Bus névtér és várólista [engedélyezési szabály](service-bus-authentication-and-authorization.md#shared-access-signature-authentication) hoz létre. Megtanulhatja, hogyan határozza meg, hogy mely erőforrások van telepítve, és paramétert definiálásáról megadott mikor hajtja végre a telepítést. Ezt a sablont a saját telepítésekhez használja, vagy testre szabhatja a követelményeknek.

Sablonok létrehozásával kapcsolatos további tudnivalókért lásd a [szerzői Azure erőforrás-kezelő sablonokat][].

A teljes sablon nézze meg a [szolgáltatás Bus auth szabály sablon][] GitHub.

>[AZURE.NOTE] Az alábbi erőforrás-kezelő Azure-sablonok letöltése és telepítéséhez érhetők el.
>
>-    [Hozzon létre egy esemény hubok névtér esemény központi és a fogyasztói csoporttal](../event-hubs/event-hubs-resource-manager-namespace-event-hub.md)
>-    [Hozzon létre egy szolgáltatás Bus névtér várólista](service-bus-resource-manager-namespace-queue.md)
>-    [A témakör és előfizetés szolgáltatás Bus névtér létrehozása](service-bus-resource-manager-namespace-topic.md)
>-    [Szolgáltatás Bus névtér létrehozása](service-bus-resource-manager-namespace.md)
>
>A legújabb sablonokat, látogasson el az [Azure quickstart útmutató][] sablongyűjteményben, és keressen a "Szolgáltatás Bus."

## <a name="what-will-you-deploy"></a>Mit fog rendszerbe?

Ezzel a sablonnal telepíti a névtér és üzenetben entitás (ebben az esetben egy várólista) szolgáltatás Bus engedélyezési szabályt.

Ezzel a sablonnal [Megosztott Access-aláírás (Társítások)](service-bus-sas-overview.md) hitelesítési használja. Társítások lehetővé teszi, hogy a névtér, illetve az üzenetek entitás (várólista vagy témakör), amellyel adott tartalomvédelmi beállítva hívóbetű használ szolgáltatási Bus hitelesíteni alkalmazásokat. A kulcs Társítások jogkivonat, amely az ügyfelek viszont használatával szolgáltatás Bus hitelesíteni létrehozásához használhatja.

A telepítő automatikusan futtatásához kattintson a következő gombra:

[![Azure telepítése](./media/service-bus-resource-manager-namespace-auth-rule/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F301-servicebus-create-authrule-namespace-and-queue%2Fazuredeploy.json)

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

### <a name="namespaceauthorizationrulename"></a>namespaceAuthorizationRuleName 

A névtér az engedélyezési szabály nevét.

```
"namespaceAuthorizationRuleName ": {
"type": "string"
}
```

### <a name="servicebusqueuename"></a>serviceBusQueueName

A szolgáltatás Bus névtér a várólista neve.

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

Szokásos szolgáltatás Bus névteret típusú **üzenetkezelés**és névtér és entitás Bus szolgáltatás engedélyezési szabály hoz létre.

```
"resources": [
        {
            "apiVersion": "[variables('sbVersion')]",
            "name": "[parameters('serviceBusNamespaceName')]",
            "type": "Microsoft.ServiceBus/namespaces",
            "location": "[variables('location')]",
            "kind": "Messaging",
            "sku": {
                "name": "StandardSku",
                "tier": "Standard"
            },
            "resources": [
                {
                    "apiVersion": "[variables('sbVersion')]",
                    "name": "[parameters('serviceBusQueueName')]",
                    "type": "Queues",
                    "dependsOn": [
                        "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
                    ],
                    "properties": {
                        "path": "[parameters('serviceBusQueueName')]"
                    },
                    "resources": [
                        {
                            "apiVersion": "[variables('sbVersion')]",
                            "name": "[parameters('queueAuthorizationRuleName')]",
                            "type": "authorizationRules",
                            "dependsOn": [
                                "[parameters('serviceBusQueueName')]"
                            ],
                            "properties": {
                                "Rights": ["Listen"]
                            }
                        }
                    ]
                }
            ]
        }, {
            "apiVersion": "[variables('sbVersion')]",
            "name": "[variables('namespaceAuthRuleName')]",
            "type": "Microsoft.ServiceBus/namespaces/authorizationRules",
            "dependsOn": ["[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"],
            "location": "[resourceGroup().location]",
            "properties": {
                "Rights": ["Send"]
            }
        }
    ]
```

## <a name="commands-to-run-deployment"></a>Telepítési futtatandó parancsok

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>A PowerShell

```
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/301-servicebus-create-authrule-namespace-and-queue/azuredeploy.json>
```

## <a name="azure-cli"></a>Azure CLI

```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/301-servicebus-create-authrule-namespace-and-queue/azuredeploy.json>
```

## <a name="next-steps"></a>Következő lépések

Most, hogy már létrehozott és Azure erőforrás-kezelővel erőforrások rendszerbe, megtudhatja, hogy miként kezelheti, ezek az erőforrások és ezek a cikkek megtekintése:

- [A PowerShell szolgáltatás Bus kezelése](service-bus-powershell-how-to-provision.md)
- [A szolgáltatás Bus Intézővel szolgáltatás Bus erőforrások kezelése](https://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)
- [Szolgáltatás Bus hitelesítése és engedélyezése](service-bus-authentication-and-authorization.md)

  [Erőforrás-kezelő Azure-sablonok létrehozása]: ../resource-group-authoring-templates.md
  [Azure quickstart útmutató sablonok]: https://azure.microsoft.com/documentation/templates/?term=service+bus
  [Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
  [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
  [Szolgáltatás Bus auth szabály sablon]: https://github.com/Azure/azure-quickstart-templates/blob/master/301-servicebus-create-authrule-namespace-and-queue/
