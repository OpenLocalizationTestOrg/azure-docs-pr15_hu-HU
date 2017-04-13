<properties
    pageTitle="Gépi tanulási Azure erőforrás-kezelő sablonnal munkaterület telepítése |} Microsoft Azure"
    description="Erőforrás-kezelő Azure-sablon segítségével Azure gépi tanulási munkaterület telepítéséről"
    services="machine-learning"
    documentationCenter=""
    authors="ahgyger"
    manager="haining"
    editor="garye"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/23/2016"
    ms.author="ahgyger"/>
# <a name="deploy-machine-learning-workspace-using-azure-resource-manager"></a>Gépi tanulási Azure erőforrás-kezelővel munkaterület terjesztése

## <a name="introduction"></a>– Bevezetés
Az Azure-kezelő telepítési sablon menti, idő, biztosítva méretezhető lehetőség szerint használata egy adatérvényesítési az összekapcsolt összetevők telepítése, és próbálkozzon újra a mechanizmusa. Azure gépi tanulási munkaterületek beállítani, például meg kell először az Azure tároló fiók konfigurálása és telepítheti a munkaterület. Tegyük fel, ezzel manuálisan munkaterületek több száz. Egyszerűbb alternatív egy erőforrás-kezelő Azure sablon használatához az Azure gépi tanulási munkaterület és minden függőség telepítését. Ez a cikk ezt a folyamatot lépésről lépésre végigvezeti Önt. Az Azure erőforrás-kezelő alapos áttekintés [Azure erőforrás-kezelő áttekintése](../azure-resource-manager/resource-group-overview.md)című témakörben találhat.

## <a name="step-by-step-create-a-machine-learning-workspace"></a>Lépésenkénti útmutató: gépi tanulási munkaterület létrehozása
Azt szolgáltatás hozzon létre egy Azure erőforráscsoport, majd a üzembe egy új Azure tárterület-fiók és az új Azure gépi tanulási munkaterület erőforrás-kezelő sablon használatával. A telepítés befejezése után azt nyomtatja ki a munkaterületekről (az elsődleges kulcsot, a workspaceID és a munkaterület URL-CÍMÉT) létrehozott fontos adatokat.

### <a name="create-an-azure-resource-manager-template"></a>Erőforrás-kezelő Azure-sablon létrehozása
Gépi tanulási munkaterület tárolja az adatkészlet hozzá csatolt Azure tároló fiókra van szükség.
A következő sablont használ, a csoport nevét, az erőforrás kattintva állítsa elő a tárterület-fiók nevét, és a munkaterület nevét.  Azt is használja a tárhely fióknév tulajdonság a munkaterület létrehozásakor.

```
{
    "contentVersion": "1.0.0.0",
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "variables": {
        "namePrefix": "[resourceGroup().name]",
        "location": "[resourceGroup().location]",
        "mlVersion": "2016-04-01",
        "stgVersion": "2015-06-15",
        "storageAccountName": "[concat(variables('namePrefix'),'stg')]",
        "mlWorkspaceName": "[concat(variables('namePrefix'),'mlwk')]",
        "mlResourceId": "[resourceId('Microsoft.MachineLearning/workspaces', variables('mlWorkspaceName'))]",
        "stgResourceId": "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]",
        "storageAccountType": "Standard_LRS"
    },
    "resources": [
        {
            "apiVersion": "[variables('stgVersion')]",
            "name": "[variables('storageAccountName')]",
            "type": "Microsoft.Storage/storageAccounts",
            "location": "[variables('location')]",
            "properties": {
                "accountType": "[variables('storageAccountType')]"
            }
        },
        {
            "apiVersion": "[variables('mlVersion')]",
            "type": "Microsoft.MachineLearning/workspaces",
            "name": "[variables('mlWorkspaceName')]",
            "location": "[variables('location')]",
            "dependsOn": ["[variables('stgResourceId')]"],
            "properties": {
                "UserStorageAccountId": "[variables('stgResourceId')]"
            }
        }
    ],
    "outputs": {
        "mlWorkspaceObject": {"type": "object", "value": "[reference(variables('mlResourceId'), variables('mlVersion'))]"},
        "mlWorkspaceToken": {"type": "string", "value": "[listWorkspaceKeys(variables('mlResourceId'), variables('mlVersion')).primaryToken]"},
        "mlWorkspaceWorkspaceID": {"type": "string", "value": "[reference(variables('mlResourceId'), variables('mlVersion')).WorkspaceId]"},
        "mlWorkspaceWorkspaceLink": {"type": "string", "value": "[concat('https://studio.azureml.net/Home/ViewWorkspace/', reference(variables('mlResourceId'), variables('mlVersion')).WorkspaceId)]"}
    }
}

```
Ez a sablon mentése c:\temp\ mlworkspace.json fájlt.

### <a name="deploy-the-resource-group-based-on-the-template"></a>Az erőforráscsoport a sablonon alapuló terjesztése
* Nyissa meg a PowerShell
* Erőforrás-kezelő Azure és az Azure Szolgáltatáskezelés modulok telepítése  

```
# Install the Azure Resource Manager modules from the PowerShell Gallery (press “A”)
Install-Module AzureRM -Scope CurrentUser

# Install the Azure Service Management modules from the PowerShell Gallery (press “A”)
Install-Module Azure -Scope CurrentUser
```

   Ezeket a lépéseket töltse le és telepítse a modulokat a hátralévő lépéseket befejezéséhez szükséges. Ez csak egyszer kell végrehajtani a környezetben, ahol van végrehajtása a PowerShell-parancsait kell.   

* Azure hitelesítéshez  

```
# Authenticate (enter your credentials in the pop-up window)
Add-AzureRmAccount
```
Ezt a lépést kell minden munkamenethez kell ismételni. Hitelesítését követően az előfizetési adatokat kell látszania.

![Azure-fiók][1]

Most, hogy elkészült a Azure elérését, akkor az erőforráscsoport hozhat létre.

* Erőforráscsoport létrehozása

```
$rg = New-AzureRmResourceGroup -Name "uniquenamerequired523" -Location "South Central US"
$rg
```

Győződjön meg arról, hogy az erőforráscsoport megfelelően már kiépítve. **ProvisioningState** célszerű lehet "sikeres."
Az erőforrás-csoport nevét a sablon a tárhely fióknév használt. A tárterület-fiók neve 3 és 24 karakter hosszúságú között legyen, és használja a számokat és csak a kisbetűk betűket.

![Erőforráscsoport][2]

* Az erőforrás csoport környezetben használja, telepítse az új gépi tanulási munkaterület.

```
# Create a Resource Group, TemplateFile is the location of the JSON template.
$rgd = New-AzureRmResourceGroupDeployment -Name "demo" -TemplateFile "C:\temp\mlworkspace.json" -ResourceGroupName $rg.ResourceGroupName
```

A telepítés befejezése után célszerű túl bonyolult feladat, hogy telepítette a munkaterület tulajdonságait. Ha például is elérheti az elsődleges kulcs Token.

```
# Access Azure ML Workspace Token after its deployment.
$rgd.Outputs.mlWorkspaceToken.Value
```

A meglévő workspace tokenek beolvasásához egy másik úgy, hogy a Invoke-AzureRmResourceAction paranccsal. Ha például jeleníthet meg az összes munkaterületek elsődleges és másodlagos tokenek.

```  
# List the primary and secondary tokens of all workspaces
Get-AzureRmResource |? { $_.ResourceType -Like "*MachineLearning/workspaces*"} |% { Invoke-AzureRmResourceAction -ResourceId $_.ResourceId -Action listworkspacekeys -Force}  
```
A munkaterület már kiépítve, miután is automatizálhatja a sok Azure gépi tanulási Studio feladatot a [Azure gépi tanulási PowerShell-modult](http://aka.ms/amlps)használja.

## <a name="next-steps"></a>Következő lépések 
* További tudnivalók [a szerzői Azure erőforrás-kezelő sablonokat](../resource-group-authoring-templates.md). 
* Az [Azure quickstart útmutató sablonok tárházba](https://github.com/Azure/azure-quickstart-templates)meg van. 
* Ez a videó [Azure erőforrás-kezelő](https://channel9.msdn.com/Events/Ignite/2015/C9-39)kapcsolatban. 
 
<!--Image references-->
[1]: ../media/machine-learning-deploy-with-resource-manager-template/azuresubscription.png
[2]: ../media/machine-learning-deploy-with-resource-manager-template/resourcegroupprovisioning.png


<!--Link references-->
