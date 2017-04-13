<properties 
    pageTitle="Adatok Factory - .NET API módosítási napló |} Microsoft Azure" 
    description="Ismerteti bekövetkezett változásokkal kapcsolatban, a szolgáltatás hozzáadását, a hibajavítás stb... az adatok Azure gyári .NET API az adott verziójában." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/21/2016" 
    ms.author="spelluru"/>

# <a name="azure-data-factory---net-api-change-log"></a>Azure adatok Factory - .NET API – módosítási napló 
Ebben a cikkben információt a változásokról Azure adatok gyári SDK adott verziójában. A legújabb NuGet csomag Azure Data Factory talál [Itt](https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactories) 

## <a name="version-4110"></a>Verzió 4.11.0
A szolgáltatás felvételéről:

- A következő csatolt szolgáltatás típusú felvétele:
    - [OnPremisesMongoDbLinkedService](https://msdn.microsoft.com/library/mt765129.aspx)
    - [AmazonRedshiftLinkedService](https://msdn.microsoft.com/library/mt765121.aspx)
    - [AwsAccessKeyLinkedService](https://msdn.microsoft.com/library/mt765144.aspx)
- Az alábbi adatkészlet-típusok felvétele: 
    - [MongoDbCollectionDataset](https://msdn.microsoft.com/library/mt765145.aspx)
    - [AmazonS3Dataset](https://msdn.microsoft.com/library/mt765112.aspx)
- A következő másolás forrástípus felvétele:
    - [MongoDbSource](https://msdn.microsoft.com/en-US/library/mt765123.aspx)

## <a name="version-4100"></a>Verzió 4.10.0
- A következő opcionális tulajdonságok hozzáadott TextFormat:
    - [SkipLineCount](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.textformat.skiplinecount.aspx)
    - [FirstRowAsHeader](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.textformat.firstrowasheader.aspx)
    - [TreatEmptyAsNull](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.textformat.treatemptyasnull.aspx)
- A következő csatolt szolgáltatás típusú felvétele:
    - [OnPremisesCassandraLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisescassandralinkedservice.aspx)
    - [SalesforceLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.salesforcelinkedservice.aspx)
- Az alábbi adatkészlet-típusok felvétele:
    - [OnPremisesCassandraTableDataset](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisescassandratabledataset.aspx)
- A következő másolás forrástípus felvétele:
    - [CassandraSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.cassandrasource.aspx)
- AzureMLBatchExecutionActivity [WebServiceInputs](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.azuremlbatchexecutionactivity.webserviceinputs.aspx) tulajdonság hozzáadása 
    - Több web service ráfordítások az Azure gépi tanulási kísérlet áthaladó engedélyezése


## <a name="version-491"></a>Verzió 4.9.1.

### <a name="bug-fix"></a>Hibajavítás

- [WebLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.weblinkedservice.authenticationtype.aspx)WebApi-alapú hitelesítés érvénytelenítése.

## <a name="version-490"></a>Verzió 4.9.0

### <a name="feature-additions"></a>Kiegészítés szolgáltatás

- [EnableStaging](https://msdn.microsoft.com/library/mt767916.aspx) és [StagingSettings](https://msdn.microsoft.com/library/mt767918.aspx) tulajdonságok hozzáadása CopyActivity. [Szakaszos másolás](data-factory-copy-activity-performance.md#staged-copy) talál részletes tudnivalókat a szolgáltatást.


### <a name="bug-fix"></a>Hibajavítás

- Túlterhelés [ActivityWindowOperationExtensions.List](https://msdn.microsoft.com/library/mt767915.aspx) módszer, amely egy [ActivityWindowsByActivityListParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.activitywindowsbyactivitylistparameters.aspx) példány veszi bevezetésére.
- Megjelölés [WriteBatchSize](https://msdn.microsoft.com/library/dn884293.aspx) és [WriteBatchTimeout](https://msdn.microsoft.com/library/dn884245.aspx) CopySink nem kötelező.

## <a name="version-480"></a>Verzió 4.8.0

### <a name="feature-additions"></a>Kiegészítés szolgáltatás
- A következő opcionális tulajdonságok felvettek másolás tevékenységtípus ahhoz, hogy másolás teljesítmény javítása:
    - [ParallelCopies](https://msdn.microsoft.com/library/mt767910.aspx)
    - [CloudDataMovementUnits](https://msdn.microsoft.com/library/mt767912.aspx)

## <a name="version-470"></a>Verzió 4.7.0

### <a name="feature-additions"></a>Kiegészítés szolgáltatás
* Adja hozzá az új StorageFormat típusa [OrcFormat](https://msdn.microsoft.com/library/mt723391.aspx) optimalizált sor oszlopos (ORC) formátumú fájlok másolásához.
* [AllowPolyBase](https://msdn.microsoft.com/library/mt723396.aspx) és PolyBaseSettings tulajdonságok hozzáadása SqlDWSink.
    * Az olyan adatokat másol be SQL adatraktár PolyBase használatát teszi lehetővé.

## <a name="version-461"></a>Verzió 4.6.1.

### <a name="bug-fixes"></a>Hibajavítás
* Kijavítja a bejegyzésére a tevékenység windows HTTP kér.
    * Erőforrás csoport nevét és az adatok gyári eltávolítja a kérelem tartalomban.

## <a name="version-460"></a>Verzió 4.6.0

### <a name="feature-additions"></a>Kiegészítés szolgáltatás

- [PipelineProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties_properties.aspx)felvette a következő tulajdonságokat:
    - [PipelineMode](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties.pipelinemode.aspx)
    - [ExpirationTime](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties.expirationtime.aspx)
    - [Adatkészletek](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties.datasets.aspx)
- [PipelineRuntimeInfo](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.common.models.pipelineruntimeinfo.aspx)felvette a következő tulajdonságokat:
    - [PipelineState](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.common.models.pipelineruntimeinfo.pipelinestate.aspx)
- A felvett új [StorageFormat](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.storageformat.aspx) írja be a [JsonFormat](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.jsonformat.aspx) típusának megadásához adatkészleteket, amelynek adatait a JSON formátumban van. 

## <a name="version-450"></a>Verzió 4.5.0

### <a name="feature-additions"></a>Kiegészítés szolgáltatás
* A felvett [tevékenység ablak műveletek felsorolása](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.activitywindowoperationsextensions.aspx).
    * Néhány további módszerek tevékenység windows beolvasni a szűrőkkel a személy típusú (Ez azt jelenti, hogy adatokat gyárak, adatkészleteket, folyamatok és a tevékenységek) alapján.
* A következő csatolt szolgáltatás típusú felvétele: 
    * [ODataLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.odatalinkedservice.aspx), [WebLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.weblinkedservice.aspx)
* Az alábbi adatkészlet-típusok felvétele: 
    * [ODataResourceDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.odataresourcedataset.aspx), [WebTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.webtabledataset.aspx)
* A következő másolás forrástípus felvétele:  
    * [WebSource](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.websource.aspx)

## <a name="version-440"></a>Verzió 4.4.0

### <a name="feature-additions"></a>Kiegészítés szolgáltatás

- A következő csatolt szolgáltatás típusa adatforrásaként kerül, és Mosogatók másolás tevékenységek:
    - [AzureStorageSasLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.azurestoragesaslinkedservice.aspx). [Azure Társítások csatolt Tárhelyszolgáltatáshoz](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service) talál tájékoztatást és példát tartalmaz. 

## <a name="version-430"></a>Verzió 4.3.0

### <a name="feature-additions"></a>Kiegészítés szolgáltatás

- A következő csatolt szolgáltatás típusú kikötőhely már hozzá adatforrásaként másolás tevékenységek:
    - [HdfsLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.hdfslinkedservice.aspx). [Fájlrendszerhez Data Factory használata az adatok áthelyezése](data-factory-hdfs-connector.md) talál tájékoztatást és példát tartalmaz. 
    - [OnPremisesOdbcLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisesodbclinkedservice.aspx). [Adatok az ODBC-adatok tárolja az Azure Data Factory használatával áthelyezése](data-factory-odbc-connector.md) talál tájékoztatást és példák. 

## <a name="version-420"></a>Verzió 4.2.0

### <a name="feature-additions"></a>Kiegészítés szolgáltatás

- A következő új tevékenységtípus hozzá lett adva: [AzureMLUpdateResourceActivity](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremlupdateresourceactivity.aspx). A tevékenység részleteit olvassa el a [frissítése Azure Machine Learning modellek a frissítés erőforrás tevékenység használatával](data-factory-azure-ml-batch-execution-activity.md#updating-azure-ml-models-using-the-update-resource-activity)című témakört.
- Egy új választható tulajdonság [updateResourceEndpoint](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremllinkedservice.updateresourceendpoint.aspx) [AzureMLLinkedService osztály](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremllinkedservice.aspx)lett hozzáadva. 
- A [DataFactoryManagementClient](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.aspx) osztály hozzáadott [LongRunningOperationInitialTimeout](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.longrunningoperationinitialtimeout.aspx) és [LongRunningOperationRetryTimeout](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.longrunningoperationretrytimeout.aspx) tulajdonságait. 
- Lehetővé teszi a időtúllépése ügyfél hívások az adatok gyári szolgáltatás konfigurálása. 


## <a name="version-410"></a>Verzió 4.1.0

### <a name="feature-additions"></a>Kiegészítés szolgáltatás
* A következő csatolt szolgáltatás típusú felvétele: 
    * [AzureDataLakeStoreLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx)
    * [AzureDataLakeAnalyticsLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx)
* A következő tevékenységtípusokat felvétele: 
    * [DataLakeAnalyticsUSQLActivity](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datalakeanalyticsusqlactivity.aspx)
* Az alábbi adatkészlet-típusok felvétele: 
    * [AzureDataLakeStoreDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoredataset.aspx)
* A következő forrás- és gyűjtő típusú másolás tevékenység felvétele:
    * [AzureDataLakeStoreSource](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoresource.aspx)
    * [AzureDataLakeStoreSink](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoresink.aspx)


## <a name="version-401"></a>Verzió 4.0.1

### <a name="breaking-changes"></a>Módosítások megszakítása
A következő osztályok átnevezett. Az új nevek volt az osztályok eredeti azoknak a 4.0.0 felengedése előtt. 
 
Nevezze el a 4.0.0 | Nevezze el a 4.0.1
:------------ | :------------ 
AzureSqlDataWarehouseDataset | [AzureSqlDataWarehouseTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuresqldatawarehousetabledataset.aspx)
AzureSqlDataset | [AzureSqlTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuresqltabledataset.aspx)
AzureDataset | [AzureTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuretabledataset.aspx)
OracleDataset | [OracleTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.oracletabledataset.aspx)
RelationalDataset | [RelationalTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.relationaltabledataset.aspx)
SqlServerDataset | [SqlServerTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.sqlservertabledataset.aspx)


## <a name="version-400"></a>Verzió 4.0.0

### <a name="breaking-changes"></a>Módosítások megszakítása



- A következő osztályok felületek átnevezett.

| Régi neve | Új név |
| :-------- | :-------- |
| ITableOperations | [IDatasetOperations](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.idatasetoperations.aspx) |  
| Táblázat | [Adatkészlet](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.dataset.aspx) | 
| TableProperties | [DatasetProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetproperties.aspx) | 
| TableTypeProprerties | [DatasetTypeProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasettypeproperties.aspx) |
| TableCreateOrUpdateParameters | [DatasetCreateOrUpdateParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdateparameters.aspx) | 
| TableCreateOrUpdateResponse | [DatasetCreateOrUpdateResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdateresponse.aspx) | 
| TableGetResponse | [DatasetGetResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetgetresponse.aspx) | 
| TableListResponse | [DatasetListResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetlistresponse.aspx) |
| CreateOrUpdateWithRawJsonContentParameters | [DatasetCreateOrUpdateWithRawJsonContentParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdatewithrawjsoncontentparameters.aspx) | 
    

- A **lista** módszerek most lapozható eredményt adnak. Ha a válasz nem üres **NextLink** tulajdonságot tartalmazza, az ügyfélalkalmazás kell folytassa a következő oldal beolvasása, amíg minden lap a visszaadott.  Lássunk egy példát: 

        PipelineListResponse response = client.Pipelines.List("ResourceGroupName", "DataFactoryName");
        var pipelines = new List<Pipeline>(response.Pipelines);
    
        string nextLink = response.NextLink;
        while (string.IsNullOrEmpty(response.NextLink))
        {
            PipelineListResponse nextResponse = client.Pipelines.ListNext(nextLink);
            pipelines.AddRange(nextResponse.Pipelines);
    
            nextLink = nextResponse.NextLink;
        }
    
- **Lista** folyamat API csak minden részlet helyett egy folyamat összesítését adja eredményül. Ha például folyamat összefoglaló tevékenységek név és típus csak tartalmazzák.

### <a name="feature-additions"></a>Kiegészítés szolgáltatás
- A [SqlDWSink](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqldwsink.aspx) osztály **SliceIdentifierColumnName** és **SqlWriterCleanupScript**, Azure SQL-adatraktár idempotent másolás támogatása két új tulajdonságokat támogatja. A témakör [Azure SQL-adatraktár](data-factory-azure-sql-data-warehouse-connector.md) , konkrétan a [mechanizmusa 1](data-factory-azure-sql-data-warehouse-connector.md#mechanism-1) és a [mechanizmusa 2](data-factory-azure-sql-data-warehouse-connector.md#mechanism-2) szakaszok ezeket a tulajdonságokat kapcsolatban további tájékoztatást.

- Most támogatja a Másolás tevékenység részeként futó tárolt eljárás Azure SQL-adatbázis és Azure SQL-adatraktár adatforrások. Az [SqlSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqlsource.aspx) és [SqlDWSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqldwsource.aspx) osztályoknak vannak a következő tulajdonságokat: **SqlReaderStoredProcedureName** és **StoredProcedureParameters**. Című témakörben olvashat [Azure SQL-adatbázis](data-factory-azure-sql-connector.md#sqlsource) és [Azure SQL-adatraktár](data-factory-azure-sql-data-warehouse-connector.md#sqldwsource) Azure.com tulajdonságokból kapcsolatban további tájékoztatást.  