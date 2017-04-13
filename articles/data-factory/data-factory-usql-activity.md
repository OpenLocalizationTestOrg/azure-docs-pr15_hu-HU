<properties 
    pageTitle="Futtassa U-SQL nyelvben parancsfájlt Azure adatok tó Analytics Azure Data Factory" 
    description="Megtudhatja, hogy miként adatfeldolgozás parancsfájlok U – SQL Azure adatok tó Analytics számítási szolgáltatása futtatásával." 
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
    ms.date="09/12/2016" 
    ms.author="spelluru"/>

# <a name="run-u-sql-script-on-azure-data-lake-analytics-from-azure-data-factory"></a>Futtassa U-SQL nyelvben parancsfájlt Azure adatok tó Analytics Azure Data Factory
> [AZURE.SELECTOR]
[A struktúra](data-factory-hive-activity.md)  
[Malac](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[A folyamatos átvitelű Hadoop](data-factory-hadoop-streaming-activity.md)
[Gépi tanulási](data-factory-azure-ml-batch-execution-activity.md) 
[Tárolt eljárás](data-factory-stored-proc-activity.md)
[U-SQL nyelvben adatok tó Analytics](data-factory-usql-activity.md)
[egyéni .NET](data-factory-use-custom-activities.md)
 
Egy folyamat egy Azure adatok gyári a csatolt tárolása az adatokat a csatolt számítási szolgáltatásokkal dolgozza fel. A tevékenységek, ahol az egyes tevékenységek adott feldolgozás műveletet hajt végre sorozatában tartalmazza. Ez a cikk ismerteti az **Azure adatok tó Analytics** csatolt számítási szolgáltatásainak **U-SQL nyelvben** parancsfájl futó **Adatok tó Analytics U-SQL nyelvben tevékenységet** . 

> [AZURE.NOTE] 
> Azure adatok tó Analytics fiók létrehozása egy folyamat létrehozása adatok tó Analytics U-SQL nyelvben tevékenységgel előtt. Azure adatok tó Analytics kapcsolatos további tudnivalókért olvassa el a [Azure adatok tó Analytics – első lépések](../data-lake-analytics/data-lake-analytics-get-started-portal.md)című témakört.
>  
> Tekintse át [a folyamat első oktatóprogram összeállítása](data-factory-build-your-first-pipeline.md) a lépések részletes adatok gyár, csatolt szolgáltatások, adatkészleteket és a folyamat létrehozása. Az adatok gyári szerkesztő vagy Visual Studio, illetve Azure PowerShell JSON kódrészletek hozhat létre az adatok gyári szervezetek.

## <a name="azure-data-lake-analytics-linked-service"></a>Azure adatok tó Analytics csatolt szolgáltatás
Az **Azure adatok tó Analytics** csatolt szolgáltatásainak Azure adatok tó Analytics-számítási szolgáltatásainak hivatkozni szeretne egy Azure adatok gyári hoz létre. Az adatok tó Analytics SQL-U tevékenység a során a csatolt szolgáltatás hivatkozik. 

Az alábbi példa az Azure adatok tó Analytics csatolt szolgáltatásainak JSON definícióját tartalmaz. 

    {
        "name": "AzureDataLakeAnalyticsLinkedService",
        "properties": {
            "type": "AzureDataLakeAnalytics",
            "typeProperties": {
                "accountName": "adftestaccount",
                "dataLakeAnalyticsUri": "datalakeanalyticscompute.net",
                "authorization": "<authcode>",
                "sessionId": "<session ID>", 
                "subscriptionId": "<subscription id>",
                "resourceGroupName": "<resource group name>"
            }
        }
    }


A következő táblázat a JSON-definícióban használt tulajdonságok leírását. 

A tulajdonság | Leírás | Szükséges
-------- | ----------- | --------
Típus | A type tulajdonság kell állíthatók be: **AzureDataLakeAnalytics**. | igen
Fióknév | Azure adatok tó Analytics-fiók neve. | igen
dataLakeAnalyticsUri | Azure adatok tó Analytics URI. |  nem 
engedély | A program automatikusan beolvassa az engedélyezési kód **engedélyezése** gombra az adatok gyári szerkesztő kattintva, majd az OAuth bejelentkezés befejezése után.  | igen 
subscriptionId | Azure előfizetés azonosítója | Nem (Ha nincs megadva, az adatok gyári az előfizetés-alapú). 
resourceGroupName | Azure erőforráscsoport neve |  Nem (Ha nincs megadva, az adatok gyári erőforráscsoport-alapú).
munkamenet-azonosító | munkamenet-azonosító az OAuth engedélyezési munkamenetből. Minden egyes munkamenet-azonosító egyedi, és előfordulhat, hogy csak egyszer lehet használni. A munkamenet-azonosító jön létre automatikusan a adatok Factory-szerkesztőben. | igen

A **engedélyezése** gombjával generált engedélyezési kód után valamikor jár le. Az alábbi táblázatban talál a felhasználói fiókok különböző típusú elévülési idejét. Az alábbi hibaüzenet jelenhet meg üzenet mikor **jogkivonat lejár**hitelesítés: a hitelesítő adatok művelet hiba: invalid_grant - AADSTS70002: hitelesítő adatok érvényesítése hiba. AADSTS70008: A megadott access támogatás lejárt vagy visszavonva. Nyomkövetés azonosító: d18629e8-af88-43c5-88e3-d8419eb1fca1 Korrelációazonosító: fac30a0c-6be6-4e02-8d69-a776d2ffefd7 időbélyeg: a Skype 2015 – 12-15 21:09:31Z

 
| Felhasználó típusa | Lejárta után |
| :-------- | :----------- | 
| Azure Active Directory nem kezeli a felhasználói fiókok (@hotmail.com, @live.com, stb.) | 12 óra |
| Felhasználói fiókok felügyelt által Azure Active Directory (AAD) | Futtassa a 14 nappal az utolsó szelet után. <br/><br/>90 napon, ha egy csatolt szolgáltatás OAuth-alapú alapján szeletet fut 14 naponta legalább egyszer. |

Ez a hiba elkerülése/feloldás, zavartalanul használatával az **Engedélyezés** gombra mikor a **token lejár** , és a csatolt szolgáltatás újratelepítés. **Munkamenet** és **engedélyezési** tulajdonságok programozás útján a következő szakaszban a kód használatával értékeit is hozhat létre. 

  
### <a name="to-programmatically-generate-sessionid-and-authorization-values"></a>Munkamenet és a hitelesítési értékeket programozás útján létrehozásához 

    if (linkedService.Properties.TypeProperties is AzureDataLakeStoreLinkedService ||
        linkedService.Properties.TypeProperties is AzureDataLakeAnalyticsLinkedService)
    {
        AuthorizationSessionGetResponse authorizationSession = this.Client.OAuth.Get(this.ResourceGroupName, this.DataFactoryName, linkedService.Properties.Type);

        WindowsFormsWebAuthenticationDialog authenticationDialog = new WindowsFormsWebAuthenticationDialog(null);
        string authorization = authenticationDialog.AuthenticateAAD(authorizationSession.AuthorizationSession.Endpoint, new Uri("urn:ietf:wg:oauth:2.0:oob"));

        AzureDataLakeStoreLinkedService azureDataLakeStoreProperties = linkedService.Properties.TypeProperties as AzureDataLakeStoreLinkedService;
        if (azureDataLakeStoreProperties != null)
        {
            azureDataLakeStoreProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
            azureDataLakeStoreProperties.Authorization = authorization;
        }

        AzureDataLakeAnalyticsLinkedService azureDataLakeAnalyticsProperties = linkedService.Properties.TypeProperties as AzureDataLakeAnalyticsLinkedService;
        if (azureDataLakeAnalyticsProperties != null)
        {
            azureDataLakeAnalyticsProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
            azureDataLakeAnalyticsProperties.Authorization = authorization;
        }
    }

Témakörök [AzureDataLakeStoreLinkedService osztály](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx), [AzureDataLakeAnalyticsLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx), és [AuthorizationSessionGetResponse osztály](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx) részletes tudnivalókat az adatok gyári osztályok használt a kódot. Mutató hivatkozás hozzáadása: az WindowsFormsWebAuthenticationDialog osztály Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll. 
 
 
## <a name="data-lake-analytics-u-sql-activity"></a>Adatok tó Analytics U-SQL nyelvben tevékenység 

A következő JSON kódtöredékének egy folyamat adatok tó Analytics U-SQL nyelvben tevékenységgel határozza meg. A tevékenység meghatározása a korábban létrehozott Azure adatok tó Analytics csatolt szolgáltatás hivatkozást tartalmaz.   
  

    {
        "name": "ComputeEventsByRegionPipeline",
        "properties": {
            "description": "This is a pipeline to compute events for en-gb locale and date less than 2012/02/19.",
            "activities": 
            [
                {
                    "type": "DataLakeAnalyticsU-SQL",
                    "typeProperties": {
                        "scriptPath": "scripts\\kona\\SearchLogProcessing.txt",
                        "scriptLinkedService": "StorageLinkedService",
                        "degreeOfParallelism": 3,
                        "priority": 100,
                        "parameters": {
                            "in": "/datalake/input/SearchLog.tsv",
                            "out": "/datalake/output/Result.tsv"
                        }
                    },
                    "inputs": [
                        {
                            "name": "DataLakeTable"
                        }
                    ],
                    "outputs": 
                    [
                        {
                            "name": "EventsByRegionTable"
                        }
                    ],
                    "policy": {
                        "timeout": "06:00:00",
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "name": "EventsByRegion",
                    "linkedServiceName": "AzureDataLakeAnalyticsLinkedService"
                }
            ],
            "start": "2015-08-08T00:00:00Z",
            "end": "2015-08-08T01:00:00Z",
            "isPaused": false
        }
    }


Az alábbi táblázat ismerteti a felsorolását és leírását a tevékenység jellemző tulajdonságok. 

A tulajdonság | Leírás | Szükséges
:-------- | :----------- | :--------
típus | A type tulajdonság meg kell **DataLakeAnalyticsU-SQL nyelvben**. | igen
Parancsfájl_elérési_útja | Mappa elérési útját, amely tartalmazza a U-SQL-parancsfájlokat. A fájl neve kis-és nagybetűk. | Nem (Ha parancsfájl használata)
scriptLinkedService | Csatolt szolgáltatás, amely a tárhely, amely tartalmazza az adatok gyári parancsfájl | Nem (Ha parancsfájl használata)
parancsfájl | Adja meg a beágyazott parancsfájl Parancsfájl_elérési_útja és scriptLinkedService helyett. Például: "parancsfájl": "Adatbázis létrehozása próba". | Nem (ha Parancsfájl_elérési_útja és scriptLinkedService)
degreeOfParallelism | Egyidejű a feladat futtatásához használt csomópontok maximális száma. | nem
prioritás | Határozza meg, hogy mely feladatok kívül minden sorban állnak választják első futtatásához. Az alsó a szám, annál nagyobb a prioritás. | nem 
Paraméterek | Paraméterek U-SQL nyelvben parancsfájl | nem 

Lásd: [SearchLogProcessing.txt parancsfájl meghatározása](#script-definition) a parancsprogram-definíciót. 

## <a name="sample-input-and-output-datasets"></a>Példa a bemeneti és kimeneti adatkészleteket

### <a name="input-dataset"></a>Beviteli adatkészlet
Ebben a példában a bemeneti tárolt adatokhoz Azure adatok tó tárban (datalake/beviteli mappában SearchLog.tsv fájl). 

    {
        "name": "DataLakeTable",
        "properties": {
            "type": "AzureDataLakeStore",
            "linkedServiceName": "AzureDataLakeStoreLinkedService",
            "typeProperties": {
                "folderPath": "datalake/input/",
                "fileName": "SearchLog.tsv",
                "format": {
                    "type": "TextFormat",
                    "rowDelimiter": "\n",
                    "columnDelimiter": "\t"
                }
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }   

### <a name="output-dataset"></a>Kimeneti adatkészlet
Ebben a példában a U-SQL nyelvben parancsfájl készített kimeneti adatai egy Azure adatok tó áruházból (datalake/kimeneti mappa) vannak tárolva. 

    {
        "name": "EventsByRegionTable",
        "properties": {
            "type": "AzureDataLakeStore",
            "linkedServiceName": "AzureDataLakeStoreLinkedService",
            "typeProperties": {
                "folderPath": "datalake/output/"
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

### <a name="sample-data-lake-store-linked-service"></a>Minta tó adattár csatolt szolgáltatás
Az alábbiakban a definíció a minta Azure tó adattár csatolva, a bemeneti és kimeneti adatkészleteket által használt szolgáltatást. 

    {
        "name": "AzureDataLakeStoreLinkedService",
        "properties": {
            "type": "AzureDataLakeStore",
            "typeProperties": {
                "dataLakeUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
                "sessionId": "<session ID>",
                "authorization": "<authorization URL>"
            }
        }
    }

Lásd: [és Azure tó adattár az adatok áthelyezése](data-factory-azure-datalake-connector.md) cikk JSON tulajdonságok leírását. 

## <a name="sample-u-sql-script"></a>Az SQL-U mintaparancsfájl 

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM @in
        USING Extractors.Tsv(nullEscape:"#NULL#");
    
    @rs1 =
        SELECT Start, Region, Duration
        FROM @searchlog
    WHERE Region == "en-gb";
    
    @rs1 =
        SELECT Start, Region, Duration
        FROM @rs1
        WHERE Start <= DateTime.Parse("2012/02/19");
    
    OUTPUT @rs1   
        TO @out
          USING Outputters.Tsv(quoting:false, dateTimeFormat:null);

A értékének **@in** és **@out** U – SQL-parancsfájl az átadott paraméter dinamikusan ADF által "parameters" szakaszának használatával. A folyamat meghatározása a "parameters" című.

A feladatok az Azure adatok tó Analytics szolgáltatást futtató folyamat definíciójának, valamint megadhatja más tulajdonságait, például degreeOfParallelism és prioritását.

## <a name="dynamic-parameters"></a>Dinamikus paraméterei
A minta folyamat definícióját és kicsinyítés paraméterek van rendelve csomagolásukkor értékű. 

    "parameters": {
        "in": "/datalake/input/SearchLog.tsv",
        "out": "/datalake/output/Result.tsv"
    }

Ajánlatos dinamikus paraméterek használja helyette. Példa: 

    "parameters": {
        "in": "$$Text.Format('/datalake/input/{0:yyyy-MM-dd HH:mm:ss}.tsv', SliceStart)",
        "out": "$$Text.Format('/datalake/output/{0:yyyy-MM-dd HH:mm:ss}.tsv', SliceStart)"
    }

Ebben az esetben bemeneti fájlok továbbra is felvett a /datalake/input mappából, és a kimeneti fájl a /datalake/output mappában jönnek létre. A fájlnevekben meg dinamikus szeletet kezdetének alapján.  