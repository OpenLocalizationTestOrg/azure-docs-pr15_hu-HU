<properties
    pageTitle="Hozzon létre egy esemény hubok névtér esemény központi, és engedélyezése az erőforrás-kezelő Azure-sablon segítségével archív |} Microsoft Azure"
    description="Hozzon létre egy esemény hubok névtér esemény központi és erőforrás-kezelő Azure-sablon segítségével archív engedélyezése"
    services="event-hubs"
    documentationCenter=".net"
    authors="ShubhaVijayasarathy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.devlang="tbd"
    ms.topic="article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="09/14/2016"
    ms.author="ShubhaVijayasarathy"/>

# <a name="create-an-event-hubs-namespace-with-event-hub-and-enable-archive-using-an-azure-resource-manager-template"></a>Hozzon létre egy esemény hubok névtér esemény központi és erőforrás-kezelő Azure-sablon segítségével archív engedélyezése

Ez a cikk bemutatja, hogyan, egy esemény hubok névtér létrehoz egy eseményt hubhoz, és lehetővé teszi az archiválás meg az esemény központi Azure erőforrás-kezelő sablonnal. Megismerheti, hogyan határozza meg, hogy mely erőforrások van telepítve, és paramétert definiálásáról megadott mikor hajtja végre a telepítést. Ezt a sablont a saját telepítésekhez használja, vagy testre szabhatja az követelményeknek

Sablonok létrehozásával kapcsolatos további tudnivalókért lásd: [Azure erőforrás-kezelő létrehozáshoz használható sablonok][].

További információt a gyakorlat és Azure erőforrások elnevezési szabályokat a mintázatok olvassa el [Azure erőforrások elnevezési szabályokat][].

A teljes sablon olvassa el a [esemény elosztót, és az archív sablon engedélyezése][] a GitHub.

>[AZURE.NOTE]
>A legújabb sablonokat, látogasson el az [Azure quickstart útmutató][] sablongyűjteményben és a Keresés az esemény hubok.

## <a name="what-you-deploy"></a>Mi rendszerbe?

Ezzel a sablonnal egy esemény hubok névtér az egy esemény központi telepítését, és engedélyezze a archív.

[Esemény hubok](../event-hubs/event-hubs-what-is-event-hubs.md) ahhoz, hogy bejövő Eseménynapló és telemetriai adatok Azure tömeges méreteket, a kis késés és a magas megbízhatóság használt szolgáltatás feldolgozása eseményről szó. Esemény hubok archív lehetővé teszi a automatikusan előadásához belül egy megadott idő vagy a méret időtartományt a választás az Azure Blob-tárolóhoz lehetőség az esemény hubok az adatfolyam.

A telepítő automatikusan futtatásához kattintson a következő gombra:

[![Azure telepítése](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-eventhubs-create-namespace-and-enable-archive%2Fazuredeploy.json)

## <a name="parameters"></a>Paraméterek

Az Azure erőforrás-kezelő, akkor paraméterek beállítása értékeket szeretne megadni, ha telepíti a sablont. A sablonban szerepel a nevű szakasz `Parameters` , amely tartalmazza a paraméterértékeket. Be kell állítania egy ezeket az értékeket, hogy a projekt telepít alapján vagy a környezet, amelyben telepít paramétert. Nem paraméterek beállítása értékek, amelyek mindig érintetlen marad. Mindegyik paraméter segítségével a sablonban definiálása az erőforrásokat, van telepítve.

A sablon a következő paraméterek határozza meg.

### <a name="eventhubnamespacename"></a>eventHubNamespaceName

Az esemény hubok névtér létrehozása neve.

```
"eventHubNamespaceName":{  
     "type":"string",
     "metadata":{  
         "description":"Name of the EventHub namespace"
      }
}
```

### <a name="eventhubname"></a>eventHubName

A nevét az esemény-központban létrehozott az Eseménynapló hubok névtér.

```
"eventHubName":{  
    "type":"string",
    "metadata":{  
        "description":"Name of the Event Hub"
    }
}
```

### <a name="messageretentionindays"></a>messageRetentionInDays

Az üzenetek tartja meg az esemény-központban kívánt napok számát. 

```
"messageRetentionInDays":{
    "type":"int",
    "defaultValue": 1,
    "minValue":"1",
    "maxValue":"7",
    "metadata":{
       "description":"How long to retain the data in Event Hub"
     }
 }
```

### <a name="partitioncount"></a>partitionCount

Azt szeretné, hogy az esemény-központban partíciót száma.

```
"partitionCount":{
    "type":"int",
    "defaultValue":2,
    "minValue":2,
    "maxValue":32,
    "metadata":{
        "description":"Number of partitions chosen"
    }
 }
```

### <a name="archiveenabled"></a>archiveEnabled

Az archiválás meg az esemény központi engedélyezése.

```
"archiveEnabled":{
    "type":"string",
    "defaultValue":"true",
    "allowedValues": [
    "false",
    "true"],
    "metadata":{
        "description":"Enable or disable the Archive for your Event Hub"
    }
 }
```
### <a name="archiveencodingformat"></a>archiveEncodingFormat

A kódolás adhatja meg az esemény adatai szerializálni.

```
"archiveEncodingFormat":{
    "type":"string",
    "defaultValue":"Avro",
    "allowedValues":[
    "Avro"],
    "metadata":{
        "description":"The encoding format Archive serializes the EventData"
    }
}
```

### <a name="archivetime"></a>archiveTime

Az időszakot, amit a archívumba az Azure blob-tárolóban lévő adatok archiválása kezdi.

```
"archiveTime":{
    "type":"int",
    "defaultValue":300,
    "minValue":60,
    "maxValue":900,
    "metadata":{
         "description":"the time window in seconds for the archival"
    }
}
```

### <a name="archivesize"></a>archiveSize

A méret időköz, amelynél az archiválás elindítja az Azure blob-tárolóban lévő adatok archiválása.

```
"archiveSize":{
    "type":"int",
    "defaultValue":314572800,
    "minValue":10485760,
    "maxValue":524288000,
    "metadata":{
        "description":"the size window in bytes for archival"
    }
}
```

### <a name="destinationstorageaccountresourceid"></a>destinationStorageAccountResourceId

Az archiválás a tárterület-fiók erőforrás-azonosító, ahhoz, hogy a kívánt Azure tárolóhoz archív szükséges.

```
 "destinationStorageAccountResourceId":{
    "type":"string",
    "metadata":{
        "description":"Your existing storage Account resource id where you want the blobs be archived"
    }
 }
```

### <a name="blobcontainername"></a>blobContainerName

A blob-tárolóhoz, amelyre az esemény adatok archiválja.

```
 "blobContainerName":{
    "type":"string",
    "metadata":{
        "description":"Your existing storage Container that you want the blobs archived in"
    }
}
```


### <a name="apiversion"></a>apiVersion

A sablon API verzióját.

```
 "apiVersion":{  
    "type":"string",
    "defaultValue":"2015-08-01",
    "metadata":{  
        "description":"ApiVersion used by the template"
    }
 }
```

## <a name="resources-to-deploy"></a>Erőforrások terjesztése

Létrehoz egy esemény központi **EventHubs**, típusú névteret, és lehetővé teszi a archív.

```
"resources":[  
      {  
         "apiVersion":"[variables('ehVersion')]",
         "name":"[parameters('eventHubNamespaceName')]",
         "type":"Microsoft.EventHub/Namespaces",
         "location":"[variables('location')]",
         "sku":{  
            "name":"Standard",
            "tier":"Standard"
         },
         "resources":[  
            {  
               "apiVersion":"[variables('ehVersion')]",
               "name":"[parameters('eventHubName')]",
               "type":"EventHubs",
               "dependsOn":[  
                  "[concat('Microsoft.EventHub/namespaces/', parameters('eventHubNamespaceName'))]"
               ],
               "properties":{  
                  "path":"[parameters('eventHubName')]",
                  "MessageRetentionInDays":"[parameters('messageRetentionInDays')]",
                  "PartitionCount":"[parameters('partitionCount')]",
                  "ArchiveDescription":{
                        "enabled":"[parameters('archiveEnabled')]",
                        "encoding":"[parameters('archiveEncodingFormat')]",
                        "intervalInSeconds":"[parameters('archiveTime')]",
                        "sizeLimitInBytes":"[parameters('archiveSize')]",
                        "destination":{
                            "name":"EventHubArchive.AzureBlockBlob",
                            "properties":{
                                "StorageAccountResourceId":"[parameters('destinationStorageAccountResourceId')]",
                                "BlobContainer":"[parameters('blobContainerName')]"
                            }
                        } 
                  }
                  
               }
               
            }
         ]
      }
   ]
```

## <a name="commands-to-run-deployment"></a>Telepítési futtatandó parancsok

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>A PowerShell

```
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-eventhubs-create-namespace-and-enable-archive/azuredeploy.json
```

## <a name="azure-cli"></a>Azure CLI

```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri [https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-eventhubs-create-namespace-and-enable-archive/azuredeploy.json][]
```

[Erőforrás-kezelő Azure-sablonok létrehozása]: ../resource-group-authoring-templates.md
[Azure quickstart útmutató sablonok]:  https://azure.microsoft.com/documentation/templates/?term=event+hubs
[Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
[Event Hub and consumer group template]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-eventhubs-create-namespace-and-enable-archive/
[Elnevezési szabályokat Azure erőforrások]: https://azure.microsoft.com/en-us/documentation/articles/guidance-naming-conventions/
[Esemény elosztót, és az archív sablon engedélyezése]:https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-namespace-and-enable-archive
