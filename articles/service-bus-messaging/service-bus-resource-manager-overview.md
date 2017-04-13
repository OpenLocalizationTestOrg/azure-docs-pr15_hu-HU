<properties
    pageTitle="Erőforrás-kezelő Azure sablonokkal szolgáltatás Bus erőforrások létrehozása |} Microsoft Azure"
    description="Erőforrás-kezelő Azure sablonok használatával automatizálhatja a szolgáltatás Bus erőforrások létrehozása"
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
    ms.author="sethm"/>

# <a name="create-service-bus-resources-using-azure-resource-manager-templates"></a>Erőforrás-kezelő Azure sablonokkal szolgáltatás Bus erőforrások létrehozása

Ez a cikk ismerteti, hogyan hozhat létre és üzembe Azure erőforrás-kezelő sablonok, a PowerShell és a szolgáltatás Bus erőforrás szolgáltató szolgáltatás Bus és esemény hubok erőforrásokat.

Azure erőforrás-kezelő sablonok segítségével, hogy az erőforrások megoldást üzembe helyezése, és adja meg a paraméterek és, amely lehetővé teszi a bemeneti értékek különböző környezetekben a változó. A sablont, ahol a JSON és kifejezések, amelyeket a telepítéshez értékek összeállításához használhat. Erőforrás-kezelő Azure-sablonok és a sablon formátum vitafórum írásához részletes információt a [szerzői Azure erőforrás-kezelő sablonok](../resource-group-authoring-templates.md)témakörben található. 

>[AZURE.NOTE] Ez a cikk példáiban segítségével Azure erőforrás-kezelő szolgáltatás Bus névtér és üzenetben entitás (várólista) módját mutatják. További sablon példákat keresse fel az [Azure quickstart útmutató sablongyűjteményben][] , és keressen a "Szolgáltatás Bus."

## <a name="service-bus-and-event-hubs-resource-manager-templates"></a>Szolgáltatás Bus és esemény hubok erőforrás-kezelő sablonok

A szolgáltatás Bus és esemény hubok Azure erőforrás-kezelő sablonok letöltés és telepítés érhetők el. Egyenként, amely hivatkozásokat tartalmaz a sablonok a GitHub az olvashat az alábbi hivatkozásokra kattintva: 

- [Szolgáltatás Bus névtér létrehozása](service-bus-resource-manager-namespace.md)
- [Hozzon létre egy szolgáltatás Bus névtér várólista](service-bus-resource-manager-namespace-queue.md)
- [A témakör és előfizetés szolgáltatás Bus névtér létrehozása](service-bus-resource-manager-namespace-topic.md)
- [Szolgáltatás Bus névtér várólista és engedélyezési szabály létrehozása](service-bus-resource-manager-namespace-auth-rule.md)
- [Hozzon létre egy esemény hubok névtér esemény központi és a fogyasztói csoporttal](../event-hubs/event-hubs-resource-manager-namespace-event-hub.md)

## <a name="deploy-with-powershell"></a>A PowerShell telepítése

Az alábbi eljárás a PowerShell használatával hoz létre egy **szokásos** réteg szolgáltatás Bus névtér, és egy várólista belül, hogy egy erőforrás-kezelő Azure-sablon ismerteti. Ebben a példában a [tartalmazó várólista szolgáltatást Bus névtér létrehozása](https://github.com/Azure/azure-quickstart-templates/tree/master/201-servicebus-create-queue) sablon alapján. A közelítő munkafolyamat van a következőképpen:

1. PowerShell telepítése.
2. A sablon és (nem kötelező) a paraméter fájl létrehozása.
2. A PowerShell jelentkezzen be az Azure-fiókjába.
3. Ha nem létezik, hozzon létre egy új erőforráscsoport.
4. A telepítés tesztelése.
5. Ha azt szeretné, állítsa a telepítési mód.
6. Telepítse a sablont.

Erőforrás-kezelő Azure sablonok telepítésével kapcsolatos részletes információkért témakörökben [Deploy Azure erőforrás-kezelő sablonokkal][].

### <a name="install-powershell"></a>PowerShell telepítése

Telepítse az Azure PowerShell [telepítése és konfigurálása az Azure PowerShell](../powershell-install-configure.md)utasításait követve.

### <a name="create-a-template"></a>Sablon létrehozása

Klónozhatja, és másolja a [201 servicebus létrehozása-várólista -](https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/azuredeploy.json) sablon GitHub:

```
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "serviceBusNamespaceName": {
            "type": "string",
            "metadata": {
                "description": "Name of the Service Bus namespace"
            }
        },
        "serviceBusQueueName": {
            "type": "string",
            "metadata": {
                "description": "Name of the Queue"
            }
        },
        "serviceBusApiVersion": {
            "type": "string",
            "defaultValue": "2015-08-01",
            "metadata": {
                "description": "Service Bus ApiVersion used by the template"
            }
        }
    },
    "variables": {
        "location": "[resourceGroup().location]",
        "sbVersion": "[parameters('serviceBusApiVersion')]",
        "defaultSASKeyName": "RootManageSharedAccessKey",
        "authRuleResourceId": "[resourceId('Microsoft.ServiceBus/namespaces/authorizationRules', parameters('serviceBusNamespaceName'), variables('defaultSASKeyName'))]"
    },
    "resources": [{
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
                "path": "[parameters('serviceBusQueueName')]"
            }
        }]
    }],
    "outputs": {
        "NamespaceConnectionString": {
            "type": "string",
            "value": "[listkeys(variables('authRuleResourceId'), variables('sbVersion')).primaryConnectionString]"
        },
        "SharedAccessPolicyPrimaryKey": {
            "type": "string",
            "value": "[listkeys(variables('authRuleResourceId'), variables('sbVersion')).primaryKey]"
        }
    }
}
```

### <a name="create-a-parameters-file-optional"></a>Paraméterek fájl létrehozása (nem kötelező)

Választható paraméterek fájlt használ, másolja a [201 servicebus létrehozása-várólista -](https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/azuredeploy.parameters.json) fájlt. A értéket `serviceBusNamespaceName` a szolgáltatás Bus névtér szeretne létrehozni a környezetben, és értékének lecserélése nevű `serviceBusQueueName` a várólista szeretne létrehozni a nevet. 

```
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "serviceBusNamespaceName": {
            "value": "<myNamespaceName>"
        },
        "serviceBusQueueName": {
            "value": "<myQueueName>"
        },
        "serviceBusApiVersion": {
            "value": "2015-08-01"
        }
    }
}
```

További információ a [paraméter fájl](../resource-group-template-deploy.md#parameter-file) témakörben talál.

### <a name="log-in-to-azure-and-set-the-azure-subscription"></a>Jelentkezzen be az Azure és az Azure előfizetés beállítása

Egy PowerShell-parancssorában futtassa az alábbi parancsot:

```
Login-AzureRmAccount
```

Jelentkezzen be az Azure-fiók kéri. Miután bejelentkezett, a következő parancsot a rendelkezésre álló előfizetések megtekintéséhez.

```
Get-AzureRMSubscription
```

Ez a parancs a rendelkezésre álló Azure-előfizetések listáját adja eredményül. Válassza ki az aktuális szakasz az előfizetés a következő parancs futtatásával. Csere `<YourSubscriptionId>` az Azure előfizetés használni kívánt GUID.

```
Set-AzureRmContext -SubscriptionID <YourSubscriptionId>
```

### <a name="set-the-resource-group"></a>Az erőforráscsoport beállítása

Ha nem egy meglévő erőforráscsoport, hozzon létre új erőforráscsoport a **New-AzureRmResourceGroup** paranccsal. Adja meg a nevét az erőforráscsoport és a használni kívánt helyre. Példa:

```
New-AzureRmResourceGroup -Name MyDemoRG -Location "West US"
```

Sikerrel jár, ha az új erőforráscsoport összefoglalását jelenik meg.

```
ResourceGroupName : MyDemoRG
Location          : westus
ProvisioningState : Succeeded
Tags              :
ResourceId        : /subscriptions/<GUID>/resourceGroups/MyDemoRG
```

### <a name="test-the-deployment"></a>A telepítés tesztelése

A telepítési érvényesítése futtatásával a `Test-AzureRmResourceGroupDeployment` parancsmag. Amikor a központi telepítés tesztelése, adja meg a paramétereket pontosan módon végrehajtása a telepítés során.

```
Test-AzureRmResourceGroupDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json
```

### <a name="create-the-deployment"></a>A telepítési létrehozása

Az új telepítési létrehozásához, futtassa a `New-AzureRmResourceGroupDeployment` parancsot, és küldje el a szükséges paramétereket, amikor a rendszer kéri. A paraméterek szerepel a név, a telepítéshez, a erőforráscsoport és az elérési út vagy URL-CÍMÉT a sablonfájl nevét. Ha a **mód** paraméter nincs megadva, az alapértelmezett érték **Léptetési távolság** használják. További tudnivalókért lásd: a [Léptetési távolság és a teljes telepítések](../resource-group-template-deploy.md#incremental-and-complete-deployments).

A következő parancsot a PowerShell ablakában a három paraméterekkel kéri:

```
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json
```

Paraméterek fájl inkább megadásához használja a következő parancsot.

```
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json -TemplateParameterFile <path to parameters file>\azuredeploy.parameters.json
```

Beágyazott paraméterek telepítési parancsmag futtatásakor is használhatja. A parancs nem az alábbi képlettel történik:

```
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json -parameterName "parameterValue"
```

Egy [teljes](../resource-group-template-deploy.md#incremental-and-complete-deployments) telepítési futtatásához a **mód** paraméter értéke **teljes**:

```
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -Mode Complete -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json 
```

### <a name="verify-the-deployment"></a>A telepítési ellenőrzése

Az erőforrások sikeresen van telepítve, ha a telepítő összefoglalását a PowerShell ablakában jelennek meg:

```
DeploymentName    : MyDemoDeployment
ResourceGroupName : MyDemoRG
ProvisioningState : Succeeded
Timestamp         : 4/19/2016 10:38:30 PM
Mode              : Incremental
TemplateLink      :
Parameters        :
                    Name             Type                       Value
                    ===============  =========================  ==========
                    serviceBusNamespaceName  String             <namespaceName>
                    serviceBusQueueName  String                 <queueName>
                    serviceBusApiVersion  String                2015-08-01

```

## <a name="next-steps"></a>Következő lépések

Most már láthatta, az egyszerű munkafolyamat és parancsok találnak egy erőforrás-kezelő Azure-sablon. További információ kövesse az alábbi hivatkozásokat:

- [Azure erőforrás szolgáltatásának áttekintése][]
- [Erőforrás-kezelő Azure sablonokkal erőforrások terjesztése][]
- [Sablonok létrehozása](../resource-group-authoring-templates.md)


[Azure erőforrás szolgáltatásának áttekintése]: ../resource-group-overview.md
[Erőforrás-kezelő Azure sablonokkal erőforrások terjesztése]: ../resource-group-template-deploy.md
[Azure quickstart útmutató sablongyűjteményben]: https://azure.microsoft.com/documentation/templates/?term=service+bus