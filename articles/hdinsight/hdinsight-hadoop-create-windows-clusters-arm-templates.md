<properties
   pageTitle="Windows-alapú Hadoop fürt létrehozása az Azure erőforrás-kezelő sablonok használata HDInsight |} Microsoft Azure"
    description="Megtudhatja, hogy miként fürt létrehozása az Azure erőforrás-kezelő sablonok használata Azure hdinsight szolgáltatásból lehetőségre."
   services="hdinsight"
   documentationCenter=""
   tags="azure-portal"
   authors="mumian"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/19/2016"
   ms.author="jgao"/>

# <a name="create-windows-based-hadoop-clusters-in-hdinsight-using-azure-resource-manager-templates"></a>Windows-alapú Hadoop fürt létrehozása a sablonokkal az erőforrás-kezelő Azure hdinsight szolgáltatáshoz

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Megtudhatja, hogyan hozhat létre sablonokkal az erőforrás-kezelő Azure hdinsight szolgáltatáshoz fürt. További tudnivalókért lásd: a [Deploy Azure erőforrás-kezelő sablonnal kérelmet](../resource-group-template-deploy.md). Más fürt létrehozása eszközök és szolgáltatások lapon kattintson a lap tetején lévő vagy [fürt létrehozási módszerek](hdinsight-provision-clusters.md#cluster-creation-methods)látható.

##<a name="prerequisites"></a>Előfeltételek:

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


Ez a cikk utasításait megkezdése előtt a következőket kell rendelkeznie:

- [Azure előfizetés](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Azure PowerShell vagy Azure CLI

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell-and-cli.md)] 

### <a name="access-control-requirements"></a>Access-ellenőrzési követelmények

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="resource-manager-templates"></a>Erőforrás-kezelő sablonok

Erőforrás-kezelő sablon egyszerűen HDInsight fürt, a függő erőforrások (például az alapértelmezett tároló fiók) és más erőforrások (például Azure SQL-adatbázis használata Apache Sqoop) létrehozása művelet egyetlen, összehangolt az alkalmazás. A sablonban meghatározása az erőforrásokat, az alkalmazáshoz szükséges, és adja meg a bemeneti értékek a különböző környezetekben telepítési paramétert. A sablont, ahol a JSON és kifejezések, amelyeket értékeket a telepítéshez összeállításához használhat.

Erőforrás-kezelő sablon létrehozása egy HDInsight fürthöz és a függő Azure tárterület-fiók megtalálható a [Függelék-A](#appx-a-arm-template). Szövegszerkesztővel egy fájlba, a számítógépen a sablon mentéséhez. Megtanulhatja, hogy hogyan hívja fel a különböző eszközök segítségével sablont.

Az erőforrás-kezelő sablon kapcsolatos további tudnivalókért lásd:

- [A szerző Azure erőforrás-kezelő sablonok](../resource-group-authoring-templates.md)
- [Erőforrás-kezelő Azure sablonnal alkalmazások telepítése](../resource-group-template-deploy.md)


## <a name="deploy-with-powershell"></a>A PowerShell telepítése

Az alábbi eljárás létrehoz egy HDInsight fürthöz.

**Erőforrás-kezelő sablonnal fürt terjesztése**

1. A json-fájl mentése a [mintája](#appx-a-arm-template) munkaállomástól.
2. A paraméterek beállítása, ha szükséges.
3. Futtassa a sablon használatával az alábbi PowerShell parancsprogramot:

        ####################################
        # Set these variables
        ####################################
        #region - used for creating Azure service names
        $nameToken = "<Enter an Alias>" 
        $templateFile = "C:\HDITutorials-ARM\hdinsight-arm-template.json"
        #endregion

        ####################################
        # Service names and varialbes
        ####################################
        #region - service names
        $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

        $resourceGroupName = $namePrefix + "rg"
        $hdinsightClusterName = $namePrefix + "hdi"
        $defaultStorageAccountName = $namePrefix + "store"
        $defaultBlobContainerName = $hdinsightClusterName

        $location = "East US 2"

        $armDeploymentName = $namePrefix
        #endregion

        ####################################
        # Connect to Azure
        ####################################
        #region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        try{Get-AzureRmContext}
        catch{Login-AzureRmAccount}
        #endregion

        # Create a resource group
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $Location

        # Create cluster and the dependent storage accounge
        $parameters = @{clusterName="$hdinsightClusterName";clusterStorageAccountName="$defaultStorageAccountName"}

        New-AzureRmResourceGroupDeployment `
            -Name $armDeploymentName `
            -ResourceGroupName $resourceGroupName `
            -TemplateFile $templateFile `
            -TemplateParameterObject $parameters

        # List cluster
        Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName 

    A PowerShell-parancsprogramot, csak beállítja a fürt és a tárterület-fiók nevét.  Beállíthatja, hogy más értékek az erőforrás-kezelő sablonban. 
    
További tudnivalókért olvassa el a [Deploy PowerShell](../resource-group-template-deploy.md#deploy-with-powershell)című témakört.

## <a name="deploy-with-azure-cli"></a>Az Azure CLI terjesztése

A következő példa létrehoz egy fürt és függő tárterület-fiók és a tároló hívja fel az erőforrás-kezelő sablon:

    azure login
    azure config mode arm
    azure group create -n hdi1229rg -l "East US 2"
    azure group deployment create "hdi1229rg" "hdi1229" --template-file "C:\HDITutorials-ARM\hdinsight-arm-template.json" -p "{\"clusterName\":{\"value\":\"hdi1229win\"},\"clusterStorageAccountName\":{\"value\":\"hdi1229store\"},\"location\":{\"value\":\"East US 2\"},\"clusterLoginPassword\":{\"value\":\"Pass@word1\"}}"





## <a name="deploy-with-rest-api"></a>REST API-val terjesztése

Lásd: [a REST API-val telepítése](../resource-group-template-deploy.md#deploy-with-the-rest-api).

## <a name="deploy-with-visual-studio"></a>A Visual Studio terjesztése

A Visual Studióban hozzon létre egy erőforrás projektek, és a felhasználói felületen üzembe Azure. Az erőforrások a projektben adja, és ezek az erőforrások automatikusan hozzáadja az erőforrás-kezelő sablont. A projekt a telepítéshez használni a sablon egy PowerShell-parancsprogramot is tartalmaz.

Visual Studio segítségével erőforrás csoportok bemutatása olvassa el a [létrehozása és a Visual Studio segítségével Azure erőforráscsoport telepítése](../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md)című témakört.

##<a name="next-steps"></a>Következő lépések
Ebben a cikkben többféle módon is létre HDInsight fürt megtanulta azt van. További tudnivalókért lásd: az alábbi cikkekben:


- Példa üzembe helyezése a .NET-ügyfél tár keresztül erőforrások olvassa el a [Deploy erőforrások .NET-tárak és a sablon használatával](../virtual-machines/virtual-machines-windows-csharp-template.md)című témakört.
- Meg szeretné vizsgálni példa az alkalmazások telepítése, című [rendelkezés és Azure-ban előre microservices üzembe](../app-service-web/app-service-deploy-complex-application-predictably.md).
- A megoldást üzembe helyezné a különböző környezetekben, olvassa el [fejlesztés és a próba-verziót tartalmazó környezetek Microsoft Azure-ban](../solution-dev-test-environments.md).
- Az erőforrás-kezelő Azure-sablon szakaszok kapcsolatos további tudnivalókért olvassa el a [sablonok létrehozása](../resource-group-authoring-templates.md)című témakört.
- Az erőforrás-kezelő Azure-sablon használható függvények listáját lásd: [sablon függvények](../resource-group-template-functions.md).



##<a name="appx-a-resource-manager-template"></a>Erőforrás-kezelő AlkX-A: sablon

Az alábbi erőforrás-kezelő Azure-sablon a Windows-alapú Hadoop fürtre a függő Azure tárterület-fióknak hoz létre.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
    "parameters": {
        "location": {
        "type": "string",
        "defaultValue": "East US 2",
        "allowedValues": [
            "Central US",
            "North Europe",
            "East US",
            "East US 2",
            "North Central US",
            "South Central US",
            "West US",
            "North Europe",
            "West Europe",
            "East Asia",
            "Southeast Asia",
            "Japan East",
            "Japan West",
            "Brizil South",
            "Australia East",
            "Australia Southeast",
            "Central India"
        ],
        "metadata": {
            "description": "The location where all azure resources will be deployed."
        }
        },
        "clusterName": {
        "type": "string",
        "metadata": {
            "description": "The name of the HDInsight cluster to create."
        }
        },
        "clusterLoginUserName": {
        "type": "string",
        "defaultValue": "admin",
        "metadata": {
            "description": "These credentials can be used to submit jobs to the cluster and to log into cluster dashboards."
        }
        },
        "clusterLoginPassword": {
        "type": "securestring",
        "metadata": {
            "description": "The password for the cluster login."
        }
        },
        "clusterStorageAccountName": {
        "type": "string",
        "metadata": {
            "description": "The name of the storage account to be created and be used as the cluster's storage."
        }
        },
        "clusterStorageType": {
        "type": "string",
        "defaultValue": "Standard_LRS",
        "allowedValues": [
            "Standard_LRS",
            "Standard_GRS",
            "Standard_ZRS"
        ]
        },
        "clusterWorkerNodeCount": {
        "type": "int",
        "defaultValue": 4,
        "metadata": {
            "description": "The number of nodes in the HDInsight cluster."
        }
        }
    },
        "variables": {},
        "resources": [
            {
            "name": "[parameters('clusterStorageAccountName')]",
            "type": "Microsoft.Storage/storageAccounts",
            "location": "[parameters('location')]",
            "apiVersion": "2015-05-01-preview",
            "dependsOn": [],
            "tags": {},
            "properties": {
                "accountType": "[parameters('clusterStorageType')]"
            }
            },
            {
            "name": "[parameters('clusterName')]",
            "type": "Microsoft.HDInsight/clusters",
            "location": "[parameters('location')]",
            "apiVersion": "2015-03-01-preview",
            "dependsOn": [
                "[concat('Microsoft.Storage/storageAccounts/',parameters('clusterStorageAccountName'))]"
            ],
            "tags": {},
            "properties": {
                "clusterVersion": "3.2",
                "osType": "Windows",
                "clusterDefinition": {
                "kind": "hadoop",
                "configurations": {
                    "gateway": {
                    "restAuthCredential.isEnabled": true,
                    "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
                    "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
                    }
                }
                },
                "storageProfile": {
                "storageaccounts": [
                    {
                    "name": "[concat(parameters('clusterStorageAccountName'),'.blob.core.windows.net')]",
                    "isDefault": true,
                    "container": "[parameters('clusterName')]",
                    "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('clusterStorageAccountName')), '2015-05-01-preview').key1]"
                    }
                ]
                },
                "computeProfile": {
                "roles": [
                    {
                    "name": "headnode",
                    "targetInstanceCount": "1",
                    "hardwareProfile": {
                        "vmSize": "Large"
                    }
                    },
                    {
                    "name": "workernode",
                    "targetInstanceCount": "[parameters('clusterWorkerNodeCount')]",
                    "hardwareProfile": {
                        "vmSize": "Large"
                    }
                    }
                ]
                }
            }
            }
        ],
        "outputs": {
            "cluster": {
            "type": "object",
            "value": "[reference(resourceId('Microsoft.HDInsight/clusters',parameters('clusterName')))]"
            }
        }
    }

