<properties
    pageTitle="Erőforrás-kezelő sablon használatával szolgáltatás Bus névtér létrehozása |} Microsoft Azure"
    description="Erőforrás-kezelő Azure-sablon szolgáltatás Bus névtér létrehozása"
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
    ms.date="10/04/2016"
    ms.author="sethm;shvija"/>

# <a name="create-a-service-bus-namespace-using-an-azure-resource-manager-template"></a>Erőforrás-kezelő Azure-sablon segítségével szolgáltatás Bus névtér létrehozása

Ez a cikk ismerteti, hogy a Fájltípus **üzenetküldés** szolgáltatás Bus névteret létrehoz egy szabványos/Basic Termékváltozat az erőforrás-kezelő Azure sablonnal. A cikk is megadja a paramétereket, amelyeket a telepítés végrehajtásához. Ezt a sablont a saját telepítésekhez használja, vagy testre szabhatja a követelményeknek.

Sablonok létrehozásával kapcsolatos további tudnivalókért lásd a [szerzői Azure erőforrás-kezelő sablonokat][].

A teljes sablon nézze meg a [szolgáltatás Bus névtér sablon][] GitHub.

>[AZURE.NOTE] Az alábbi erőforrás-kezelő Azure-sablonok letöltése és telepítéséhez érhetők el. 
>
>-    [Hozzon létre egy esemény hubok névtér esemény központi és a fogyasztói csoporttal](../event-hubs/event-hubs-resource-manager-namespace-event-hub.md)
>-    [Hozzon létre egy szolgáltatás Bus névtér várólista](service-bus-resource-manager-namespace-queue.md)
>-    [A témakör és előfizetés szolgáltatás Bus névtér létrehozása](service-bus-resource-manager-namespace-topic.md)
>-    [Szolgáltatás Bus névtér várólista és engedélyezési szabály létrehozása](service-bus-resource-manager-namespace-auth-rule.md)
>
>A legújabb sablonokat, látogasson el az [Azure quickstart útmutató][] sablongyűjteményben és szolgáltatás Bus keresése.

## <a name="what-will-you-deploy"></a>Mit fog rendszerbe?

Ezzel a sablonnal [egyszerű, normál, vagy magasabb szintű](https://azure.microsoft.com/pricing/details/service-bus/) Termékváltozat szolgáltatás Bus névteret telepíti.

A telepítő automatikusan futtatásához kattintson a következő gombra:

[![Azure telepítése](./media/service-bus-resource-manager-namespace/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-servicebus-create-namespace%2Fazuredeploy.json)

## <a name="parameters"></a>Paraméterek

Az Azure erőforrás-kezelő, akkor paraméterek beállítása értékeket szeretne megadni, ha telepíti a sablont. A sablonban szerepel a nevű szakasz `Parameters` , amely tartalmazza az összes paraméterértékeket. Be kell állítania a paramétert, ezek az értékek, amelyek alapján a projekten telepít vagy a környezet, amelyben telepít változhatnak. Nem paraméterek beállítása értékek, amelyek mindig érintetlen marad. Mindegyik paraméter segítségével a sablonban definiálása az erőforrásokat, van telepítve.

Ez a sablon a következő paraméterek határozza meg.

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

### <a name="servicebussku"></a>serviceBusSKU

A szolgáltatás Bus [Termékváltozat](https://azure.microsoft.com/pricing/details/service-bus/) létrehozása neve.

```
"serviceBusSku": { 
    "type": "string", 
    "allowedValues": [ 
        "Basic", 
        "Standard",
        "Premium" 
    ], 
    "defaultValue": "Standard", 
    "metadata": { 
        "description": "The messaging tier for service Bus namespace" 
    } 

```

Ehhez a paraméterhez (Basic, normál és Premium) megengedett értékeket meghatározza a sablont, és rendel egy alapértelmezett értéket (hagyományos), ha nem ad meg értéket.

Szolgáltatás Bus árak kapcsolatos további tudnivalókért olvassa el a [szolgáltatás Bus árak, és a számlázási][]című témakört.

### <a name="servicebusapiversion"></a>serviceBusApiVersion

A szolgáltatás Bus API verziója a sablont.

```
"serviceBusApiVersion": { 
       "type": "string", 
       "defaultValue": "2015-08-01", 
       "metadata": { 
           "description": "Service Bus ApiVersion used by the template" 
       } 
```

## <a name="resources-to-deploy"></a>Erőforrások terjesztése

### <a name="service-bus-namespace"></a>Szolgáltatás Bus névtere

Létrehoz egy szokásos szolgáltatás Bus névtér **üzenetküldés**típusú.

```
"resources": [
    {
        "apiVersion": "[parameters('serviceBusApiVersion')]",
        "name": "[parameters('serviceBusNamespaceName')]",
        "type": "Microsoft.ServiceBus/Namespaces",
        "location": "[variables('location')]",
        "kind": "Messaging",
        "sku": {
            "name": "StandardSku",
            "tier": "Standard"
        },
        "properties": {
        }
    }
]
```

## <a name="commands-to-run-deployment"></a>Telepítési futtatandó parancsok

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>A PowerShell

```
New-AzureRmResourceGroupDeployment -ResourceGroupName <resource-group-name> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-servicebus-create-namespace/azuredeploy.json
```

### <a name="azure-cli"></a>Azure CLI

```
azure config mode arm

azure group deployment create <my-resource-group> <my-deployment-name> --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-servicebus-create-namespace/azuredeploy.json
```

## <a name="next-steps"></a>Következő lépések

Most, hogy már létrehozott és Azure erőforrás-kezelővel erőforrások rendszerbe, megtudhatja, hogy miként kezelheti, ezek az erőforrások és ezek a cikkek olvasása:

- [A PowerShell szolgáltatás Bus kezelése](service-bus-powershell-how-to-provision.md)
- [A szolgáltatás Bus Intézővel szolgáltatás Bus erőforrások kezelése](https://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)

  [Erőforrás-kezelő Azure-sablonok létrehozása]: ../resource-group-authoring-templates.md
  [Szolgáltatás Bus névtér sablon]: https://github.com/Azure/azure-quickstart-templates/blob/master/101-servicebus-create-namespace/
  [Azure quickstart útmutató sablonok]: https://azure.microsoft.com/documentation/templates/?term=service+bus
  [Árak és a számlázási szolgáltatás Bus]: https://azure.microsoft.com/documentation/articles/service-bus-pricing-billing/
  [Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
  [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
