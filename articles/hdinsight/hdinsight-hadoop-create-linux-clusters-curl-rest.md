<properties
    pageTitle="Hozzon létre Hadoop, HBase vagy vihar fürt Linux HDInsight cURL és Azure REST API-t a |} Microsoft Azure"
    description="Megtudhatja, hogyan hozhat létre Linux-alapú HDInsight fürt cURL, erőforrás-kezelő Azure-sablonok és Azure REST API-t. Adja meg az fürt (Hadoop, HBase vagy vihar), vagy használja a parancsfájlok egyéni összetevők telepítése."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="10/11/2016"
    ms.author="larryfr"/>

#<a name="create-linux-based-clusters-in-hdinsight-using-curl-and-the-azure-rest-api"></a>Hozzon létre fürt Linux-alapú HDInsight cURL és az Azure REST API segítségével

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Az Azure REST API lehetővé teszi a Azure platform, beleértve az új erőforrások, például Linux-alapú HDInsight fürt kibocsátása üzemeltetett szolgáltatások adatkezelési műveletek hajthatók végre. A jelen dokumentum megtanulhatja, hogyan hozhat létre az erőforrás-kezelő Azure sablonok egy HDInsight fürt és a kapcsolódó tároló konfigurálása, majd telepítse az Azure REST API-t egy új HDInsight fürt létrehozása a sablon cURL használatával.

> [AZURE.IMPORTANT] A lépéseket a dokumentum egy HDInsight fürthöz az alapértelmezett szám dolgozó csomópontok (4) használata. Ha több mint 32 dolgozó csomópontok, vagy fürt hoz létre, vagy a fürt átméretezés után létrehozása, majd ki kell választania egy központi csomópont méretet legalább 8 magmintákat és 14GB ram.
>
> Csomópont méretét és a kapcsolódó költségek a további tudnivalókért olvassa el a [HDInsight árak](https://azure.microsoft.com/pricing/details/hdinsight/)című témakört.

##<a name="prerequisites"></a>Előfeltételek

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


- **Az Azure-előfizetés**. Lásd: [Ismerkedés az Azure ingyenes próbaverziót](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- __Azure CLI__. Az Azure CLI szolgáltatást hozhat létre egyszerű, majd az Azure REST API-nak kérések hitelesítési tokenek létrehozásához használt szolgál.

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

- __cURL__. A segédprogram elérhetők a csomag a projektirányítási rendszerek, vagy letölthető [http://curl.haxx.se/](http://curl.haxx.se/).

    > [AZURE.NOTE] Ha a PowerShell használatával a parancsokat a dokumentumban, el kell távolítani a `curl` alias, amely alapértelmezés szerint hoz létre. Az alias cURL használatakor helyett használja a Invoke-WebRequest, egy PowerShell-parancsmag a `curl` parancsot a PowerShell-parancssorában, és hibát ad vissza készült, a dokumentumban használt parancsot.
    > 
    > Az alias eltávolításához használja a következő parancsot a PowerShell-parancssorában:
    >
    > `Remove-item alias:curl`
    >
    > Az alias megszűnt, miután látnia kell a számítógépen telepített cURL verzióját szeretné használni.

### <a name="access-control-requirements"></a>Access-ellenőrzési követelmények

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

##<a name="create-a-template"></a>Sablon létrehozása

Azure az erőforrás-kezelés sablonok JSON dokumentumokat, amelyek bemutatják __erőforráscsoport__ és a benne lévő összes erőforrás, (például HDInsight.) Ez a sablon-alapú megközelítés lehetővé teszi a HDInsight egy sablonba szükséges összes erőforrás megadásához, és kezelheti a módosításokat a csoportba egészének keresztül __telepítések__ , amely a csoportba alkalmazni a módosításokat.

Sablonok általában nyújtott két részből áll; a sablon, és a paraméterek fájl feltöltése egyedi értékekkel a konfigurációban. Exmaple, a csoport nevét, rendszergazda nevét és jelszavát. Amikor közvetlenül a REST API-t, ki kell alakítania ezek egy fájlba. A JSON-dokumentum formátuma:

    {
        "properties": {
            "template": {
                contents of template file
            },
            "mode": "deploymentmode",
            "Parameters": {
                contents of the parameters element from parameters file
            }
        }
    }

Ha például az alábbiakban [https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password](https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password), amely a SSH felhasználói fiók biztonságos jelszó használatával Linux-alapú fürt hoz létre a sablon és a paraméterek fájlok egyesülés.

    {
        "properties": {
            "template": {
                "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                "contentVersion": "1.0.0.0",
                "parameters": {
                    "location": {
                        "type": "string",
                        "allowedValues": ["Central US",
                        "East Asia",
                        "East US",
                        "Japan East",
                        "Japan West",
                        "North Europe",
                        "South Central US",
                        "Southeast Asia",
                        "West Europe",
                        "West US"],
                        "metadata": {
                            "description": "The location where all azure resources will be deployed."
                        }
                    },
                    "clusterType": {
                        "type": "string",
                        "allowedValues": ["hadoop",
                        "hbase",
                        "storm",
                        "spark"],
                        "metadata": {
                            "description": "The type of the HDInsight cluster to create."
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
                    "clusterStorageAccountName": {
                        "type": "string",
                        "metadata": {
                            "description": "The name of the storage account to be created and be used as the cluster's storage."
                        }
                    },
                    "clusterWorkerNodeCount": {
                        "type": "int",
                        "defaultValue": 4,
                        "metadata": {
                            "description": "The number of nodes in the HDInsight cluster."
                        }
                    }
                },
                "variables": {
                    "defaultApiVersion": "2015-05-01-preview",
                    "clusterApiVersion": "2015-03-01-preview"
                },
                "resources": [{
                    "name": "[parameters('clusterStorageAccountName')]",
                    "type": "Microsoft.Storage/storageAccounts",
                    "location": "[parameters('location')]",
                    "apiVersion": "[variables('defaultApiVersion')]",
                    "dependsOn": [],
                    "tags": {
                        
                    },
                    "properties": {
                        "accountType": "Standard_LRS"
                    }
                },
                {
                    "name": "[parameters('clusterName')]",
                    "type": "Microsoft.HDInsight/clusters",
                    "location": "[parameters('location')]",
                    "apiVersion": "[variables('clusterApiVersion')]",
                    "dependsOn": ["[concat('Microsoft.Storage/storageAccounts/',parameters('clusterStorageAccountName'))]"],
                    "tags": {
                        
                    },
                    "properties": {
                        "clusterVersion": "3.2",
                        "osType": "Linux",
                        "clusterDefinition": {
                            "kind": "[parameters('clusterType')]",
                            "configurations": {
                                "gateway": {
                                    "restAuthCredential.isEnabled": true,
                                    "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
                                    "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
                                }
                            }
                        },
                        "storageProfile": {
                            "storageaccounts": [{
                                "name": "[concat(parameters('clusterStorageAccountName'),'.blob.core.windows.net')]",
                                "isDefault": true,
                                "container": "[parameters('clusterName')]",
                                "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('clusterStorageAccountName')), variables('defaultApiVersion')).key1]"
                            }]
                        },
                        "computeProfile": {
                            "roles": [{
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
                            }]
                        }
                    }
                }],
                "outputs": {
                    "cluster": {
                        "type": "object",
                        "value": "[reference(resourceId('Microsoft.HDInsight/clusters',parameters('clusterName')))]"
                    }
                }
            },
            "mode": "incremental",
            "Parameters": {
                "location": {
                    "value": "North Europe"
                },
                "clusterName": {
                    "value": "newclustername"
                },
                "clusterType": {
                    "value": "hadoop"
                },
                "clusterStorageAccountName": {
                    "value": "newstoragename"
                },
                "clusterLoginUserName": {
                    "value": "admin"
                },
                "clusterLoginPassword": {
                    "value": "changeme"
                },
                "sshUserName": {
                    "value": "sshuser"
                },
                "sshPassword": {
                    "value": "changeme"
                }
            }
        }
    }

Ez a példa fog szerepelni a dokumentum című témakör lépéseit. A helyőrző _értékeket_ a __Paraméterek__ szakaszban a dokumentum végén ezt kell lecserélnie a fürt használni kívánt értékeket.

##<a name="login-to-your-azure-subscription"></a>Jelentkezzen be az Azure előfizetés

Kövesse a lépéseket a [Microsoft Azure-előfizetésbe a az Azure parancssori kezelőfelületről Azure](../xplat-cli-connect.md) dokumentált és az előfizetés használatával csatlakozhat a `azure login` parancsot.

##<a name="create-a-service-principal"></a>Hozzon létre egy egyszerű

> [AZURE.NOTE] Ezeket a lépéseket az információkat [a szolgáltatás egyszerű Azure erőforrás-kezelővel hitelesítése](../resource-group-authenticate-service-principal.md#authenticate-service-principal-with-password---azure-cli) dokumentum _hitelesítés szolgáltatás fő jelszóval - Azure CLI_ szakaszban leírt egy egyszerűsített verzióját is. Ezeket a lépéseket hozzon létre egy új szolgáltatás fő, amely a REST API-kérelmek Azure erőforrások, például egy HDInsight fürthöz létrehozásához használt hitelesítő használható.

1. A parancssorból munkamenet vagy felületén a következő paranccsal az Azure-előfizetések listáját.

        azure account list
        
    A listában jelölje ki azt az előfizetést, szeretné használni, és jegyezze fel az __azonosító__ oszlopot. __Előfizetés azonosítója__ , és a használandó a legtöbb, lépés a dokumentumban.

2. Új alkalmazás létrehozása az Azure Active Directory.

        azure ad app create --name "exampleapp" --home-page "https://www.contoso.org" --identifier-uris "https://www.contoso.org/example" --password <Your_Password>
        
    A értékek cseréje a `--name`, `--home-page`, és `--identifier-uris` saját értékű. Adja meg a jelszót az új Active Directory-bejegyzés.
    
    > [AZURE.NOTE] Mivel ez az alkalmazás az authentication és a szolgáltatás egyszerű hoz létre a `--home-page` és `--identifier-uris` értékeket nem kell az internethez; is tényleges weblap hivatkozás csak kell lenniük egyedi URL-címe.
    
    A visszaadott adatok mentése __AppId__ értékét.
    
        data:    AppId:          4fd39843-c338-417d-b549-a545f584a745
        data:    ObjectId:       4f8ee977-216a-45c1-9fa3-d023089b2962
        data:    DisplayName:    exampleapp
        ...
        info:    ad app create command OK
    
3. Hozzon létre egy korábban fő használva __AppId__ visszaadott szolgáltatást.

        azure ad sp create 4fd39843-c338-417d-b549-a545f584a745
        
     A visszaadott adatok mentése __Objektumazonosító__ értékét.
     
        info:    Executing command ad sp create
        - Creating service principal for application 4fd39843-c338-417d-b549-a545f584a74+
        data:    Object Id:        7dbc8265-51ed-4038-8e13-31948c7f4ce7
        data:    Display Name:     exampleapp
        data:    Service Principal Names:
        data:                      4fd39843-c338-417d-b549-a545f584a745
        data:                      https://www.contoso.org/example
        info:    ad sp create command OK
        
4. A __tulajdonos__ szerepkör hozzárendelése a szolgáltatás fő használva __Objektumazonosító__ korábban adja vissza. __Előfizetés azonosítója__ korábbi szerezte be is kell használnia.
    
        azure role assignment create --objectId 7dbc8265-51ed-4038-8e13-31948c7f4ce7 -o Owner -c /subscriptions/{SubscriptionID}/
        
    Ha ez a parancs befejeződött, a fő szolgáltatás most már hozzáférhet tulajdonosa a megadott előfizetés azonosítója.

##<a name="get-an-authentication-token"></a>Get-hitelesítési jogkivonat

1. A következő segítségével megkeresheti a __Bérlői azonosító__ előfizetéséhez.

        azure account show -s <subscription ID>
        
    A visszaadott adatok keresse meg a __Bérlői azonosítója__.
    
        info:    Executing command account show
        data:    Name                        : MyAzureAccount
        data:    ID                          : 45a1014d-0f27-25d2-b838-b8f373d6d52e
        data:    State                       : Enabled
        data:    Tenant ID                   : 22f988bf-56f1-41af-91ab-3d7cd011db47
        data:    Is Default                  : true
        data:    Environment                 : AzureCloud
        data:    Has Certificate             : No
        data:    Has Access Token            : Yes
        data:    User name                   : myname@contoso.org
        data:    
        info:    account show command OK

2. Hoz létre egy új jogkivonat Azure REST API-t használja.

        curl -X "POST" "https://login.microsoftonline.com/TenantID/oauth2/token" \
        -H "Cookie: flight-uxoptin=true; stsservicecookie=ests; x-ms-gateway-slice=productionb; stsservicecookie=ests" \
        -H "Content-Type: application/x-www-form-urlencoded" \
        --data-urlencode "client_id=AppID" \
        --data-urlencode "grant_type=client_credentials" \
        --data-urlencode "client_secret=password" \
        --data-urlencode "resource=https://management.azure.com/"
    
    Nyert vagy korábban használt értékeket cserélje __TenantID__, __AppID__és __jelszavát__ .

    Sikeres a kérés esetén 200 sorozat választ kap, és a válasz szervezet JSON dokumentum fog tartalmazni.

    A JSON-dokumentumot, a kérelem által visszaadott __access_token__; névvel ellátott elem fog tartalmazni. a hozzáférési jogkivonat kell használnia a hitelesítési a kérelmeket, használja a következő szakaszokban a dokumentum: Ez az elem értéke.
    
        {
            "token_type":"Bearer",
            "expires_in":"3599",
            "expires_on":"1463409994",
            "not_before":"1463406094",
            "resource":"https://management.azure.com/","access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWoNBVGZNNXBPWWlKSE1iYTlnb0VLWSIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiJodHRwczovL21hbmFnZW1lbnQuYXp1cmUuY29tLyIsImlzcyI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI2Ny8iLCJpYXQiOjE0NjM0MDYwOTQsIm5iZiI6MTQ2MzQwNjA5NCwiZXhwIjoxNDYzNDA5OTk5LCJhcHBpZCI6IjBlYzcyMzM0LTZkMDMtNDhmYi04OWU1LTU2NTJiODBiZDliYiIsImFwcGlkYWNyIjoiMSIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0Ny8iLCJvaWQiOiJlNjgxZTZiMi1mZThkLTRkZGUtYjZiMS0xNjAyZDQyNWQzOWYiLCJzdWIiOiJlNjgxZTZiMi1mZThkLTRkZGUtYjZiMS0xNjAyZDQyNWQzOWYiLCJ0aWQiOiI3MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMWRiNDciLCJ2ZXIiOiIxLjAifQ.nJVERbeDHLGHn7ZsbVGBJyHOu2PYhG5dji6F63gu8XN2Cvol3J1HO1uB4H3nCSt9DTu_jMHqAur_NNyobgNM21GojbEZAvd0I9NY0UDumBEvDZfMKneqp7a_cgAU7IYRcTPneSxbD6wo-8gIgfN9KDql98b0uEzixIVIWra2Q1bUUYETYqyaJNdS4RUmlJKNNpENllAyHQLv7hXnap1IuzP-f5CNIbbj9UgXxLiOtW5JhUAwWLZ3-WMhNRpUO2SIB7W7tQ0AbjXw3aUYr7el066J51z5tC1AK9UC-mD_fO_HUP6ZmPzu5gLA6DxkIIYP3grPnRVoUDltHQvwgONDOw"
        }

##<a name="create-a-resource-group"></a>Erőforráscsoport létrehozása

A következő használatával hozzon létre egy új erőforráscsoport. Az erőforrások, például a HDInsight fürt létrehozása előtt először kell létrehoznia a a csoporthoz. 

* Cserélje ki az előfizetés azonosítója, a szolgáltatás egyszerű létrehozása során kapott __SubscriptionID__ .
* Cserélje ki a hozzáférési jogkivonat, az előző lépésben kapott __AccessToken__ .
* Cserélje ki az Adatközpont szeretne létrehozni a erőforráscsoport, és az erőforrások, a __DataCenterLocation__ . Ha például "Dél központi US". 
* __ResourceGroupName__ cserélje le kívánja használni a csoport nevére:

```
curl -X "PUT" "https://management.azure.com/subscriptions/SubscriptionID/resourcegroups/ResourceGroupName?api-version=2015-01-01" \
    -H "Authorization: Bearer AccessToken" \
    -H "Content-Type: application/json" \
    -d $'{
"location": "DataCenterLocation"
}'
```

Sikeres a kérés esetén 200 sorozat választ kap, és a válasz szervezet fogja tartalmazni a csoportra vonatkozó információkat tartalmazó JSON dokumentum. A `"provisioningState"` elem érték fog tartalmazni `"Succeeded"`.

##<a name="create-a-deployment"></a>A telepítés létrehozása

Használatával az alábbi a fürt konfigurálása (sablon és a paraméterek értéket,) az erőforrás-csoporthoz.

* Cserélje ki a korábban használt értékek __SubscriptionID__ és __AccessToken__ . 
* Cserélje ki az erőforrás-csoport nevét, az előző részben létrehozott __ResourceGroupName__ .
* __DeploymentName__ cserélje le a telepítéshez használni kívánt nevére.

```
curl -X "PUT" "https://management.azure.com/subscriptions/SubscriptionID/resourcegroups/ResourceGroupName/providers/microsoft.resources/deployments/DeploymentName?api-version=2015-01-01" \
-H "Authorization: Bearer AccessToken" \
-H "Content-Type: application/json" \
-d "{set your body string to the template and parameters}"
```

> [AZURE.NOTE] Ha már mentette a JSON-dokumentumot, amelyben a sablon és a paraméterek fájlba, a következő helyett is használhatja `-d "{ template and parameters}"`:
>
> `--data-binary "@/path/to/file.json"`

Sikeres a kérés esetén 200 sorozat választ kap, és a válasz szervezet fogja tartalmazni a telepítési művelet információt tartalmazó JSON dokumentumot.

> [AZURE.IMPORTANT] Megjegyzés: a telepítés elküldése, de jelenleg nem fejeződött. Eltarthat néhány percig, általában körülbelül 15, a telepítés befejezéséhez.

##<a name="check-the-status-of-a-deployment"></a>A telepítés állapotának ellenőrzése

A telepítés állapotának ellenőrzése, használja az alábbiakat:

* Cserélje ki a korábban használt értékek __SubscriptionID__ és __AccessToken__ . 
* Cserélje ki az erőforrás-csoport nevét, az előző részben létrehozott __ResourceGroupName__ .

```
curl -X "GET" "https://management.azure.com/subscriptions/SubscriptionID/resourcegroups/ResourceGroupName/providers/microsoft.resources/deployments/DeploymentName?api-version=2015-01-01" \
-H "Authorization: Bearer AccessToken" \
-H "Content-Type: application/json"
```

Ez ad vissza információkat a telepítési művelet információt tartalmazó JSON dokumentumot. A `"provisioningState"` elem a telepítési; állapotának fog tartalmazni. Ha ezt az értéket tartalmaz `"Succeeded"`, majd a telepítés sikeresen befejeződött. Ezen a ponton a fürt kell használható.

##<a name="next-steps"></a>Következő lépések

Most, hogy sikeresen létrehozott egy HDInsight fürthöz, használja az alábbi megtudhatja, hogy miként dolgozhat a fürt. 

###<a name="hadoop-clusters"></a>Hadoop fürt

* [HDInsight struktúra használata](hdinsight-use-hive.md)
* [Malac használata hdinsight szolgáltatáshoz](hdinsight-use-pig.md)
* [HDInsight MapReduce használata](hdinsight-use-mapreduce.md)

###<a name="hbase-clusters"></a>HBase fürt

* [A HDInsight HBase – első lépések](hdinsight-hbase-tutorial-get-started-linux.md)
* [A HDInsight HBase az Java-alkalmazások fejlesztői számára](hdinsight-hbase-build-java-maven-linux.md)

###<a name="storm-clusters"></a>Vihar fürt

* [Fejleszthet olyan Java topológiák vihar a hdinsight szolgáltatáshoz](hdinsight-storm-develop-java-topology.md)
* [A HDInsight vihar Python összetevők használata](hdinsight-storm-develop-python-topology.md)
* [Üzembe helyezéséhez és a HDInsight a vihar topológiák figyelése](hdinsight-storm-deploy-monitor-topology-linux.md)
