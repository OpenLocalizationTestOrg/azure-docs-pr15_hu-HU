<properties 
    pageTitle="Sablonok használata erőforrás-kezelő adatok gyár |} Microsoft Azure" 
    description="Megtudhatja, hogy miként hozhat létre és használhat az Azure erőforrás-kezelő sablonok létrehozása adatok gyári szervezetek." 
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor=""/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/24/2016" 
    ms.author="shlo"/>

# <a name="use-templates-to-create-azure-data-factory-entities"></a>Sablonok segítségével Azure Data Factory szervezetek létrehozása

## <a name="overview"></a>– Áttekintés
Használatakor Azure Data Factory az adatok integrálása igényeinek, akkor azt tapasztalhatja, saját maga újbóli felhasználása diagramsablonok azonos típusúak több különböző környezetekben és repetitively belül ugyanahhoz a megoldáshoz ugyanaz a feladat végrehajtásához. Sablonok segítségével megvalósításához és forgatókönyvekben kezelése egyszerűen módon. Az Azure Data Factory használhat, amelyek használó újrahasznosításának és az ismétlődés forgatókönyvekben ideális.
 
Fontolja meg a helyzet, ha olyan szervezettel rendelkezik világszerte 10 gyártási növény. A naplók az egyes növény tárolódnak helyszíni külön SQL Server-adatbázishoz. A vállalat szeretne egy egyetlen adatraktár alkalmi elemzéséhez a felhőben össze. Azt is szeretne azonos logikájának, de a másik konfigurációk fejlesztése, tesztelése és a gyártási környezetben. 

Ebben az esetben egy tevékenység van szüksége, meg kell ismételni azonos környezetben, de eltérő értékű a 10 adat minden olyan gyár gyárak keresztül. Gyakorlatilag azt mondja **az ismétlődés** szerepel. Templating lehetővé teszi, hogy ez általános flow (Ez azt jelenti, hogy ugyanazok a tevékenységek rendelkező minden adat gyári folyamatok) ivóvízkivételre, de az egyes gyártóüzem külön paraméter fájlt használ.

Ezenkívül a szervezet nem felel meg a következő 10 adatok gyárak többször üzembe támogatása a különböző környezetekben, sablonok használatával a **újrahasznosításának** felhasználásával külön paraméter fájlok fejlesztése, tesztelése és a gyártási környezetben.

## <a name="templating-with-azure-resource-manager"></a>Templating a Azure-kezelő eszközzel
[Erőforrás-kezelő Azure használhat](../azure-resource-manager/resource-group-overview.md#template-deployment) , amelyek az Azure Data Factory templating elérése kiválóan használhatók. Erőforrás-kezelő sablonok az infrastruktúra és konfigurálása az Azure megoldást határozzák meg a JSON-fájl segítségével. Azure erőforrás-kezelő sablonok használata az összes/legtöbb Azure szolgáltatás, mert azt körben használható egyszerűen kezelése az összes erőforrás az Azure eszközök. Lásd: a [szerzői Azure erőforrás-kezelő sablonok](../resource-group-authoring-templates.md) , ha többet szeretne megtudni az erőforrás-kezelő sablonok általános. 

## <a name="tutorials"></a>Oktatóanyagok
Az alábbi oktatóanyagok hozhat létre az adatok gyár szervezetek erőforrás-kezelő sablonok használatával részletes útmutatást talál:

- [Oktatóprogram: Az erőforrás-kezelő Azure-sablon használatával másolhatja az adatokat egy folyamat létrehozása](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
- [Oktatóprogram: Létrehozása egy folyamat folyamat adatok Azure erőforrás-kezelő sablon használatával](data-factory-build-your-first-pipeline.md)

## <a name="data-factory-templates-on-github"></a>Adatok Factory-sablonok az Github
Olvassa el az alábbi rövid útmutató az Azure-sablonok az Github: 

- [Adatok gyár adatok másolása Azure Blob-tárolóhoz Azure SQL-adatbázis létrehozása](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-blob-to-sql-copy)
- [Adatok gyár Azure hdinsight szolgáltatáshoz fürt struktúra tevékenység létrehozása](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-hive-transformation)
- [Adatok másolása Salesforce Azure BLOB adatok gyár létrehozása](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-salesforce-to-blob-copy)

Nyugodtan az [Azure rövid útmutató](https://azure.microsoft.com/documentation/templates/)az Azure Data Factory sablonok megosztása. Olvassa el a [hozzájárulása útmutató](https://github.com/Azure/azure-quickstart-templates/tree/master/1-CONTRIBUTION-GUIDE) sablonokat, amelyek a tárházba keresztül megosztható kidolgozása során. 

A következő szakaszokban a Data Factory erőforrások definiálása az erőforrás-kezelő sablon olvashat. 

## <a name="defining-data-factory-resources-in-templates"></a>Sablonok Data Factory erőforrások meghatározása
A legfelső szintű sablon adatok gyár definiálása van:

    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": { ...
    },
    "variables": { ...
    },
    "resources": [
    {
        "name": "[parameters('dataFactoryName')]",
        "apiVersion": "[variables('apiVersion')]",
        "type": "Microsoft.DataFactory/datafactories",
        "location": "westus",
        "resources": [
        { "type": "linkedservices",
            ...
        },
        {"type": "datasets",
            ...
        },
        {"type": "dataPipelines",
            ...
        }
    }

### <a name="define-data-factory"></a>Adatok gyári meghatározása

Adatok gyár definiálása az erőforrás-kezelő sablon az alábbi példa látható módon:

    "resources": [
    {
        "name": "[variables('<mydataFactoryName>')]",
        "apiVersion": "2015-10-01",
        "type": "Microsoft.DataFactory/datafactories",
        "location": "East US"
    }

A dataFactoryName a "változók" határozható meg:

    "dataFactoryName": "[concat('<myDataFactoryName>', uniqueString(resourceGroup().id))]",

### <a name="define-linked-services"></a>Kapcsolt szolgáltatások meghatározása 
    
    "type": "linkedservices",
    "name": "[variables('<LinkedServiceName>')]",
    "apiVersion": "2015-10-01",
    "dependsOn": [ "[variables('<dataFactoryName>')]" ],
    "properties": {
        ...
    }


Kapcsolatos további tudnivalók: [Csatolt Tárhelyszolgáltatáshoz](data-factory-azure-blob-connector.md#azure-storage-linked-service) vagy a [Csatolt szolgáltatások számítja ki](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) a telepíteni kívánt adott csatolt szolgáltatás JSON tulajdonságait. A "dependsOn" paramétert a megfelelő adatokat gyári nevét adja meg. Példa egy csatolt Azure tároló szolgáltatás meghatározása a következő JSON-definícióban látható:

### <a name="define-datasets"></a>Adatkészletek meghatározása

    "type": "datasets",
    "name": "[variables('<myDatasetName>')]",
    "dependsOn": [
        "[variables('<dataFactoryName>')]",
        "[variables('<myDatasetLinkedServiceName>')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
        ...
    }

Olvassa el az [adatokat tárolja támogatja](data-factory-data-movement-activities.md#supported-data-stores-and-formats) a telepítéshez használni kívánt adott adatkészlet típus JSON tulajdonságainak részleteket. A "dependsOn" paraméterrel nevét a megfelelő adatokat gyári és tároló jegyzet követendő feladatként csatolva szolgáltatás. Azure blob-tárolóhoz adatkészlet típusú meghatározása példa az alábbi JSON meghatározása:

    "type": "datasets",
    "name": "[variables('storageDataset')]",
    "dependsOn": [
        "[variables('dataFactoryName')]",
        "[variables('storageLinkedServiceName')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "[variables('storageLinkedServiceName')]",
    "typeProperties": {
        "folderPath": "[concat(parameters('sourceBlobContainer'), '/')]",
        "fileName": "[parameters('sourceBlobName')]",
        "format": {
            "type": "TextFormat"
        }
    },
    "availability": {
        "frequency": "Hour",
        "interval": 1
    }

### <a name="define-pipelines"></a>Folyamatok meghatározása

    "type": "dataPipelines",
    "name": "[variables('<mypipelineName>')]",
    "dependsOn": [
        "[variables('<dataFactoryName>')]",
        "[variables('<inputDatasetLinkedServiceName>')]",
        "[variables('<outputDatasetLinkedServiceName>')]",
        "[variables('<inputDataset>')]",
        "[variables('<outputDataset>')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
        activities: {
            ...
        }
    }

Keresse meg a [folyamatok definiáló](data-factory-create-pipelines.md#pipeline-json) az adott folyamat és a telepítéshez használni kívánt tevékenységek definiálásával JSON tulajdonságainak kapcsolatban további tájékoztatást. Megjegyzés: a "dependsOn" paramétert a data factory nevét adja meg, és a szolgáltatások és adatkészleteket bármely megfelelő-e kapcsolva. Példa egy folyamat, amely az Azure Blob-tárolóhoz Azure SQL-adatbázishoz adatait másolja a következő JSON kódtöredékének által megjelenített:

    "type": "datapipelines",
    "name": "[variables('pipelineName')]",
    "dependsOn": [
        "[variables('dataFactoryName')]",
        "[variables('azureStorageLinkedServiceName')]",
        "[variables('azureSqlLinkedServiceName')]",
        "[variables('blobInputDatasetName')]",
        "[variables('sqlOutputDatasetName')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
        "activities": [
        {
            "name": "CopyFromAzureBlobToAzureSQL",
            "description": "Copy data frm Azure blob to Azure SQL",
            "type": "Copy",
            "inputs": [
                {
                    "name": "[variables('blobInputDatasetName')]"
                }
            ],
            "outputs": [
                {
                    "name": "[variables('sqlOutputDatasetName')]"
                }
            ],
            "typeProperties": {
                "source": {
                    "type": "BlobSource"
                },
                "sink": {
                    "type": "SqlSink",
                    "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM {0}', 'emp')"
                },
                "translator": {
                    "type": "TabularTranslator",
                    "columnMappings": "Column0:FirstName,Column1:LastName"
                }
            },
            "Policy": {
                "concurrency": 1,
                "executionPriorityOrder": "NewestFirst",
                "retry": 3,
                "timeout": "01:00:00"
            }
        }
        ],
        "start": "2016-10-03T00:00:00Z",
        "end": "2016-10-04T00:00:00Z"

## <a name="parameterizing-data-factory-template"></a>Paraméterezése Data Factory-sablon
Gyakorlati tanácsok a paraméterezése, lásd: [Gyakorlati tanácsok az erőforrás-kezelő Azure-sablonok létrehozása](../resource-manager-template-best-practices.md#parameters) a cikk az. Az általános paraméter használatát kell tartani, különösen akkor, ha inkább változót is használható. Csak adja meg a paraméterek az alábbi esetekben:

- Környezet típusától függően változnak beállítások (például: fejlesztési, tesztelése és a gyártási)
- Titkos kulcsok (például jelszavak)

Ha csoportosítani titkos kulcsok a [Azure kulcs tárolóból elemre](../key-vault/key-vault-get-started.md) , sablonok használata Azure Data Factory szervezetek telepítésekor van szüksége, adja meg a **fő tárolóból elemre** , és a **titkos nevét** az alábbi példában látható módon:

    "parameters": {
        "storageAccountKey": { 
            "reference": {
                "keyVault": {
                    "id":"/subscriptions/<subscriptionID>/resourceGroups/<resourceGroupName>/providers/Microsoft.KeyVault/vaults/<keyVaultName>",
                },
                "secretName": "<secretName>"
            }, 
        },
        ...
    }

> [AZURE.NOTE] A meglévő adatok gyárak sablonok exportálása jelenleg még nem támogatott, azt is az együttműködik. 


