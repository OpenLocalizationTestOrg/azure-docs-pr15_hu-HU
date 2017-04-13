<properties
   pageTitle="Linux-alapú Hadoop fürt létrehozása az Azure erőforrás-kezelő sablonok használata HDInsight |} Microsoft Azure"
    description="Megtudhatja, hogy miként fürt létrehozása az Azure Azure erőforrás-kezelő sablonok használata Azure hdinsight szolgáltatásból lehetőségre."
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
   ms.date="09/02/2016"
   ms.author="jgao"/>

# <a name="create-linux-based-hadoop-clusters-in-hdinsight-using-azure-resource-manager-templates"></a>Hozzon létre Linux-alapú Hadoop fürt sablonokkal az erőforrás-kezelő Azure hdinsight szolgáltatáshoz

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Megtudhatja, hogyan hozhat létre HDInsight fürt Azure erőforrás Manager(ARM) sablonok használatával. További tudnivalókért lásd: a [Deploy Azure erőforrás-kezelő sablonnal kérelmet](../resource-group-template-deploy.md). Más fürt létrehozása eszközök és szolgáltatások lapon kattintson a lap tetején lévő vagy [fürt létrehozási módszerek](hdinsight-provision-clusters.md#cluster-creation-methods)látható.

##<a name="prerequisites"></a>Előfeltételek:

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Ez a cikk utasításait megkezdése előtt a következőket kell rendelkeznie:

- [Azure előfizetés](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Azure PowerShell és/vagy Azure CLI

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell-and-cli.md)]

### <a name="access-control-requirements"></a>Access-ellenőrzési követelmények

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="resource-manager-templates"></a>Erőforrás-kezelő sablonok

Erőforrás-kezelő sablon egyszerűen HDInsight fürt, a függő erőforrások (például az alapértelmezett tároló fiók) és más erőforrások (például Azure SQL-adatbázis használata Apache Sqoop) létrehozása művelet egyetlen, összehangolt az alkalmazás. A sablonban meghatározása az erőforrásokat, az alkalmazáshoz szükséges, és adja meg a bemeneti értékek a különböző környezetekben telepítési paramétert. A sablon JSON és kifejezések, amelyeket értékeket a telepítéshez összeállításához használhat áll.

Erőforrás-kezelő sablonból hozhat létre egy HDInsight fürthöz és a függő Azure tárterület-fiók megtalálható a [Függelék-A](#appx-a-arm-template). Az [erőforrás-kezelő kiterjesztés](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools) vagy szövegszerkesztőben platformok [VSCode](https://code.visualstudio.com/#alt-downloads) segítségével egy fájlba, a számítógépen a sablon mentéséhez. Megtanulhatja, hogyan hívja fel a különböző módszerekkel sablont.

Az erőforrás-kezelő sablon kapcsolatos további tudnivalókért lásd:

- [A szerző Azure erőforrás-kezelő sablonok](../resource-group-authoring-templates.md)
- [Erőforrás-kezelő Azure sablonnal alkalmazások telepítése](../resource-group-template-deploy.md)

Az egyes elemek JSON séma meg, kövesse az alábbi lépésekkel:

1. Nyissa meg a egy HDInsight fürthöz [Azure portálon](https://porta.azure.com) .  Lásd: [létrehozása Linux-alapú fürt a HDInsight az Azure portálon](hdinsight-hadoop-create-linux-clusters-portal.md).
2. Állítsa be a szükséges elemet, és az elemeket a JSON séma van szüksége.
3. Előtt kattintson a **Létrehozás**, kattintson az **Automatizálási beállítások** az alábbi képernyőképen látható módon:

    ![HDInsight Hadoop fürt erőforrás-kezelő séma automatizálási beállítási létrehozása](./media/hdinsight-hadoop-create-linux-clusters-arm-templates/hdinsight-create-cluster-resource-manager-template-automation-option.png)

    A portál létrehoz egy erőforrás-kezelő sablon alapján a konfigurációk.
## <a name="deploy-with-powershell"></a>A PowerShell telepítése

Az alábbi eljárás Linux-alapú HDInsight fürt hoz létre.

**Erőforrás-kezelő sablonnal fürt terjesztése**

1. A json-fájl mentése a [mintája](#appx-a-arm-template) munkaállomástól. A PowerShell parancsfájl a *C:\HDITutorials-ARM\hdinsight-arm-template.json*kell a fájlnevet.
2. Ha szükséges, állítsa a paramétereket és a változók.
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
        $parameters = @{clusterName="$hdinsightClusterName"}

        New-AzureRmResourceGroupDeployment `
            -Name $armDeploymentName `
            -ResourceGroupName $resourceGroupName `
            -TemplateFile $templateFile `
            -TemplateParameterObject $parameters

        # List cluster
        Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName 

    A PowerShell-parancsprogramot, csak adja meg a csoport nevét. A tároló fiók neve szoftveresen kötött a sablonban. Kérni fogja adja meg a fürt felhasználó jelszavát (az alapértelmezett felhasználónév a *felügyeleti*); és a SSH felhasználó jelszavát (az alapértelmezett SSH felhasználónév a *sshuser*).  
    
További információ [a PowerShell Deploy](../resource-group-template-deploy.md#deploy-with-powershell)megtekintése

## <a name="deploy-with-azure-cli"></a>Az Azure CLI terjesztése

A következő példa létrehoz egy fürt és függő tárterület-fiók és a tároló hívja fel az erőforrás-kezelő sablon:

    azure login
    azure config mode arm
    azure group create -n hdi1229rg -l "East US"
    azure group deployment create --resource-group "hdi1229rg" --name "hdi1229" --template-file "C:\HDITutorials-ARM\hdinsight-arm-template.json"
    
Adja meg a csoport nevét, fürt felhasználó jelszavát (az alapértelmezett felhasználónév a *felügyeleti*) és a SSH felhasználó jelszavát (az alapértelmezett SSH felhasználónév a *sshuser*) kéri. Az adja meg a soros paramétereket:

    azure group deployment create --resource-group "hdi1229rg" --name "hdi1229" --template-file "c:\Tutorials\HDInsightARM\create-linux-based-hadoop-cluster-in-hdinsight.json" --parameters '{\"clusterName\":{\"value\":\"hdi1229\"},\"clusterLoginPassword\":{\"value\":\"Pass@word1\"},\"sshPassword\":{\"value\":\"Pass@word1\"}}'

## <a name="deploy-with-rest-api"></a>REST API-val terjesztése

Lásd: [a REST API-val telepítése](../resource-group-template-deploy.md#deploy-with-the-rest-api).

## <a name="deploy-with-visual-studio"></a>A Visual Studio terjesztése

A Visual Studióban hozzon létre egy erőforrás projektek, és a felhasználói felületen üzembe Azure. Az erőforrások a projektben adja, és ezek az erőforrások automatikusan hozzáadja az erőforrás-kezelő sablont. A projekt is tartalmaz egy PowerShell-parancsprogramot bevezetését tervezi a sablont.

Visual Studio segítségével erőforrás csoportok bemutatása olvassa el a [létrehozása és a Visual Studio segítségével Azure erőforráscsoport telepítése](../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md)című témakört.

##<a name="next-steps"></a>Következő lépések
Ebben a cikkben többféle módon is létre HDInsight fürt megtanulta azt van. További tudnivalókért lásd: az alábbi cikkekben:

- Példa üzembe helyezése a .NET-ügyfél tár keresztül erőforrások olvassa el a [Deploy erőforrások .NET-tárak és a sablon használatával](../virtual-machines/virtual-machines-windows-csharp-template.md)című témakört.
- Meg szeretné vizsgálni példa az alkalmazások telepítése, című [rendelkezés és Azure-ban előre microservices üzembe](../app-service-web/app-service-deploy-complex-application-predictably.md).
- A megoldást üzembe helyezné a különböző környezetekben, olvassa el [fejlesztés és a próba-verziót tartalmazó környezetek Microsoft Azure-ban](../solution-dev-test-environments.md).
- Az erőforrás-kezelő Azure-sablon szakaszok kapcsolatos további tudnivalókért olvassa el a [sablonok létrehozása](../resource-group-authoring-templates.md)című témakört.
- Az erőforrás-kezelő Azure-sablon használható függvények listáját lásd: [sablon függvények](../resource-group-template-functions.md).

##<a name="appx-a-resource-manager-template"></a>Erőforrás-kezelő AlkX-A: sablon

Az alábbi erőforrás-kezelő Azure-sablon Linux-alapú Hadoop fürt a függő Azure tárterület-fióknak hoz létre. 

> [AZURE.NOTE] A minta struktúra metastore és Oozie metastore konfigurációs adatait tartalmazza.  Eltávolítja a szakaszt, vagy állítsa be a szakaszban a sablon használata előtt.

    {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
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
            "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
        }
        },
        "sshUserName": {
        "type": "string",
        "defaultValue": "sshuser",
        "metadata": {
            "description": "These credentials can be used to remotely access the cluster."
        }
        },
        "sshPassword": {
        "type": "securestring",
        "metadata": {
            "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
        }
        },
        "location": {
        "type": "string",
        "defaultValue": "East US",
        "allowedValues": [
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
            "Australia East",
            "Australia Southeast"
        ],
        "metadata": {
            "description": "The location where all azure resources will be deployed."
        }
        },
        "clusterType": {
        "type": "string",
        "defaultValue": "hadoop",
        "allowedValues": [
            "hadoop",
            "hbase",
            "storm",
            "spark"
        ],
        "metadata": {
            "description": "The type of the HDInsight cluster to create."
        }
        },
        "clusterWorkerNodeCount": {
        "type": "int",
        "defaultValue": 2,
        "metadata": {
            "description": "The number of nodes in the HDInsight cluster."
        }
        }
    },
    "variables": {
        "defaultApiVersion": "2015-05-01-preview",
        "clusterApiVersion": "2015-03-01-preview",
        "clusterStorageAccountName": "[concat(parameters('clusterName'),'store')]"
    },
    "resources": [
        {
        "name": "[variables('clusterStorageAccountName')]",
        "type": "Microsoft.Storage/storageAccounts",
        "location": "[parameters('location')]",
        "apiVersion": "[variables('defaultApiVersion')]",
        "dependsOn": [ ],
        "tags": { },
        "properties": {
            "accountType": "Standard_LRS"
        }
        },
        {
        "name": "[parameters('clusterName')]",
        "type": "Microsoft.HDInsight/clusters",
        "location": "[parameters('location')]",
        "apiVersion": "[variables('clusterApiVersion')]",
        "dependsOn": [ "[concat('Microsoft.Storage/storageAccounts/',variables('clusterStorageAccountName'))]" ],
        "tags": {

        },
        "properties": {
            "clusterVersion": "3.4",
            "osType": "Linux",
            "tier": "standard",
            "clusterDefinition": {
            "kind": "[parameters('clusterType')]",
            "configurations": {
                "gateway": {
                "restAuthCredential.isEnabled": true,
                "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
                "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
                },
                "hive-site": {
                    "javax.jdo.option.ConnectionDriverName": "com.microsoft.sqlserver.jdbc.SQLServerDriver",
                    "javax.jdo.option.ConnectionURL": "jdbc:sqlserver://myadla0901dbserver.database.windows.net;database=myhive20160901;encrypt=true;trustServerCertificate=true;create=false;loginTimeout=300",
                    "javax.jdo.option.ConnectionUserName": "johndole",
                    "javax.jdo.option.ConnectionPassword": "myPassword$"
                },
                "hive-env": {
                    "hive_database": "Existing MSSQL Server database with SQL authentication",
                    "hive_database_name": "myhive20160901",
                    "hive_database_type": "mssql",
                    "hive_existing_mssql_server_database": "myhive20160901",
                    "hive_existing_mssql_server_host": "myadla0901dbserver.database.windows.net",
                    "hive_hostname": "myadla0901dbserver.database.windows.net"
                },
                "oozie-site": {
                    "oozie.service.JPAService.jdbc.driver": "com.microsoft.sqlserver.jdbc.SQLServerDriver",
                    "oozie.service.JPAService.jdbc.url": "jdbc:sqlserver://myadla0901dbserver.database.windows.net;database=myhive20160901;encrypt=true;trustServerCertificate=true;create=false;loginTimeout=300",
                    "oozie.service.JPAService.jdbc.username": "johndole",
                    "oozie.service.JPAService.jdbc.password": "myPassword$",
                    "oozie.db.schema.name": "oozie"
                },
                "oozie-env": {
                    "oozie_database": "Existing MSSQL Server database with SQL authentication",
                    "oozie_database_name": "myhive20160901",
                    "oozie_database_type": "mssql",
                    "oozie_existing_mssql_server_database": "myhive20160901",
                    "oozie_existing_mssql_server_host": "myadla0901dbserver.database.windows.net",
                    "oozie_hostname": "myadla0901dbserver.database.windows.net"
                }            
            }
            },
            "storageProfile": {
            "storageaccounts": [
                {
                "name": "[concat(variables('clusterStorageAccountName'),'.blob.core.windows.net')]",
                "isDefault": true,
                "container": "[parameters('clusterName')]",
                "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('clusterStorageAccountName')), variables('defaultApiVersion')).key1]"
                }
            ]
            },
            "computeProfile": {
            "roles": [
                {
                "name": "headnode",
                "targetInstanceCount": "2",
                "hardwareProfile": {
                    "vmSize": "Standard_D3"
                },
                "osProfile": {
                    "linuxOperatingSystemProfile": {
                    "username": "[parameters('sshUserName')]",
                    "password": "[parameters('sshPassword')]"
                    }
                }
                },
                {
                "name": "workernode",
                "targetInstanceCount": "[parameters('clusterWorkerNodeCount')]",
                "hardwareProfile": {
                    "vmSize": "Standard_D3"
                },
                "osProfile": {
                    "linuxOperatingSystemProfile": {
                    "username": "[parameters('sshUserName')]",
                    "password": "[parameters('sshPassword')]"
                    }
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
