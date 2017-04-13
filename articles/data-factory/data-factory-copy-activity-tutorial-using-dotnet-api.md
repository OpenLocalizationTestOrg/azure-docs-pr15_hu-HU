<properties 
    pageTitle="Oktatóprogram: Hozzon létre egy folyamat másolás tevékenység .NET API segítségével |} Microsoft Azure" 
    description="Ebben az oktatóanyagban létrehozása az Azure Data Factory folyamat egy másolatot a tevékenységhez a .NET API használatával." 
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
    ms.topic="get-started-article" 
    ms.date="10/27/2016" 
    ms.author="spelluru"/>

# <a name="tutorial-create-a-pipeline-with-copy-activity-using-net-api"></a>Oktatóprogram: Hozzon létre egy folyamat másolás tevékenység .NET API segítségével
> [AZURE.SELECTOR]
- [Áttekintés és a vonatkozó követelmények](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Másolja a varázsló](data-factory-copy-data-wizard-tutorial.md)
- [Azure portál](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [A PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [Erőforrás-kezelő Azure-sablon](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
- [REST API-VAL](data-factory-copy-activity-tutorial-using-rest-api.md)
- [.NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md)


Ebből az oktatóanyagból megtudhatja, hogyan hozhat létre, és figyelemmel az Azure adatok gyári a .NET API. Az adatok gyári folyamat egy példány tevékenység használja az adatok másolása az Azure Blob-tárolóhoz az Azure SQL-adatbázis.

A Másolás tevékenység az adatok mozgását Azure Data Factory hajt végre. A tevékenység globálisan elérhető szolgáltatással, amely másolhatja az adatokat a különböző adatokat tárolja, biztonságos, megbízható és méretezhető útján között van-e kapcsolva. [Mozgás a tevékenységekre vonatkozó adatok](data-factory-data-movement-activities.md) cikk lásd: a Másolás tevékenység kapcsolatban további tájékoztatást.   

> [AZURE.NOTE] 
> Ez a cikk nem tárgyalja összes az adatok gyári .NET API-t. Kapcsolatos további tudnivalók: [Adatok gyári .NET API hivatkozási](https://msdn.microsoft.com/library/mt415893.aspx) adatok gyári .NET SDK csomagjában talál. 

## <a name="prerequisites"></a>Előfeltételek
- [Oktatóanyag – áttekintés és a előtti követelmény](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) az oktatóprogram áttekintést kaphat, és hajtsa végre a **előfeltétel** folyamatát. 
- Visual Studio 2012 vagy 2013-ban vagy a Skype 2015
- Töltse le és telepítse az [Azure.NET SDK](http://azure.microsoft.com/downloads/)
- Azure PowerShell. Hajtsa végre az Azure PowerShell telepítése a számítógépen [Telepítse és állítsa be a Azure PowerShell módjáról](../powershell-install-configure.md) a cikk utasításait. Azure PowerShell használatával Azure Active Directory-alkalmazás létrehozása.

### <a name="create-an-application-in-azure-active-directory"></a>Az Azure Active Directory-alkalmazás létrehozása
Azure Active Directory-alkalmazás létrehozása, hozzon létre egy egyszerű alkalmazásához és az **Adatok gyári munkatársi** szerepkörök kiosztása.  

1. Indítsa el a **PowerShell**. 
1. A következő parancsot, és adja meg a felhasználónevet és jelszót, jelentkezzen be az Azure portálra.
    
        Login-AzureRmAccount   
2. Ehhez a fiókhoz az előfizetések megtekintése a következő parancs futtatásával.

        Get-AzureRmSubscription 
3. Jelölje ki a használni kívánt előfizetést a következő parancs futtatásával. Csere ** &lt;NameOfAzureSubscription** &gt; az Azure-előfizetése nevére. 

        Get-AzureRmSubscription -SubscriptionName <NameOfAzureSubscription> | Set-AzureRmContext

    > [AZURE.IMPORTANT] Ez a parancs a Megjegyzés **SubscriptionId** és **TenantId** le. 
4. Hozzon létre egy Azure erőforráscsoport **ADFTutorialResourceGroup** nevű a PowerShell a következő parancs futtatásával.  

        New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"

    Ha már létezik az erőforráscsoport, meghatározhatja, hogy frissíti (Y) vagy (n) tarthatja. 

    Ha egy másik erőforráscsoport használja, a csoport nevét, az erőforrás ADFTutorialResourceGroup helyett használja az oktatóprogram szeretne.
5. Azure Active Directory-alkalmazás létrehozása. 

        $azureAdApplication = New-AzureRmADApplication -DisplayName "ADFCopyTutotiralApp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.adfcopytutorialapp.org/example" -Password "Pass@word1"

    Ha a következő hibaüzenet jelenik meg, adjon meg egy másik URL-címet, és futtassa újra a parancsot. 

        Another object with the same value for property identifierUris already exists.

6. Hozzon létre az Active Directory-szolgáltatás egyszerű. 

        New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

7. Az **Adatok gyári munkatársi** szerepkörök szolgáltatás egyszerű hozzáadása. 

        New-AzureRmRoleAssignment -RoleDefinitionName "Data Factory Contributor" -ServicePrincipalName $azureAdApplication.ApplicationId.Guid

8. Az alkalmazás azonosító beszerzése

        $azureAdApplication

    Megjegyzés: az alkalmazás azonosítója (**applicationID** a eredményéből) lefelé.

Rendelkeznie kell követésének négy az alábbi lépéseket: 

- Bérlői azonosító
- Előfizetés azonosítója
- Azonosítója 
- A jelszó (az első parancs megadva)   

## <a name="walkthrough"></a>Útmutató
1. Visual Studio 2012 és 2013-ban és a Skype 2015 használ, a C# .NET console-alkalmazás létrehozása.
    1. Indítsa el a **Visual Studio** 2012 és 2013-ban és a Skype 2015.
    2. Kattintson a **fájl**fülre, mutasson az **Új**, és kattintson a **Projekt**.
    3. Bontsa ki a **sablonok**, és válassza **a Visual C#**. Az útmutató használja a C#, de bármely .NET nyelven használható.
    4. A jobb oldalon a projekttípusok listáját **Konzol alkalmazás** kijelölése
    5. Írja be a név **DataFactoryAPITestApp** .
    6. Jelölje be a hely **C:\ADFGetStarted** .
    7. Kattintson az **OK gombra** a projekt létrehozása.
2. Kattintson az **eszközök**, mutasson a **Nuget csomag kezelő**és **Csomag Manager konzolban**kattintson.
3.  A **Csomag kezelője konzol**hajtsa végre az alábbi lépéseket: 
    1.  Futtassa az adatok gyári szoftvercsomag telepítésekor a következő parancsot:`Install-Package Microsoft.Azure.Management.DataFactories`       
    2.  A következő parancsot Azure Active Directory-csomag telepítéséhez (az Active Directory API a használhatja a kódot):`Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.19.208020213`
6. A következő **appSetttings** szakasz hozzáadása a **App.config** fájlt. Ezekkel a beállításokkal használatosak helper módszerrel: **GetAuthorizationHeader**. 

    Az értékek lecserélése ** &lt;azonosítója&gt;**, ** &lt;jelszó&gt;**, ** &lt;előfizetés azonosítója&gt;**, és ** &lt;azonosító bérlői&gt; ** saját értékű. 

        <?xml version="1.0" encoding="utf-8" ?>
        <configuration>
            <startup> 
                <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2" />
            </startup>
            <appSettings>
                <add key="ActiveDirectoryEndpoint" value="https://login.windows.net/" />
                <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
                <add key="WindowsManagementUri" value="https://management.core.windows.net/" />

                <add key="ApplicationId" value="your application ID" />
                <add key="Password" value="Password you used while creating the AAD application" />
                <add key="SubscriptionId" value= "Subscription ID" />
                <add key="ActiveDirectoryTenantId" value="Tenant ID" />
            </appSettings>
        </configuration>
6. A forrásfájl (Program.cs) a projekt adja hozzá a következő **használata** kimutatásokban.

        using System.Threading;
        using System.Configuration;
        using System.Collections.ObjectModel;
        
        using Microsoft.Azure.Management.DataFactories;
        using Microsoft.Azure.Management.DataFactories.Models;
        using Microsoft.Azure.Management.DataFactories.Common.Models;
        
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Azure;
6. Adja hozzá a következő kódot, amely egy példányának **DataPipelineManagementClient** osztály a **fő** módszerrel hoz létre. Az objektum segítségével adatokat gyár, a csatolt szolgáltatás, a bemeneti és kimeneti adatkészleteket és a egy folyamat létrehozása. Az objektum egy adatkészlet futásidőben szeleteit Lync is használhatja.    

            // create data factory management client
            string resourceGroupName = "ADFTutorialResourceGroup";
            string dataFactoryName = "APITutorialFactory";

            TokenCloudCredentials aadTokenCredentials =
                new TokenCloudCredentials(
                    ConfigurationManager.AppSettings["SubscriptionId"],
                    GetAuthorizationHeader());

            Uri resourceManagerUri = new Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

            DataFactoryManagementClient client = new DataFactoryManagementClient(aadTokenCredentials, resourceManagerUri);

    > [AZURE.IMPORTANT] 
    > **ResourceGroupName** értékének cserélje az Azure erőforráscsoport a nevét. 
    > 
    > Frissítse a nevét az adatok gyári (**dataFactoryName**) egyedinek kell lennie. Az adatok gyári neve globálisan egyedinek kell lennie. [Adatok Factory - elnevezési szabályai](data-factory-naming-rules.md) témakört vonatkozó adatok gyári eltérések elnevezési szabályokat. 

7. Adja hozzá a következő kódot, amely a **fő** módszerrel **adatok gyári** hoz létre.

            // create a data factory
            Console.WriteLine("Creating a data factory");
            client.DataFactories.CreateOrUpdate(resourceGroupName,
                new DataFactoryCreateOrUpdateParameters()
                {
                    DataFactory = new DataFactory()
                    {
                        Name = dataFactoryName,
                        Location = "westus",
                        Properties = new DataFactoryProperties() { }
                    }
                }
            );

8. Adja hozzá a következő kódot, amely létrehoz egy **Azure tárolás csatolt szolgáltatás** a **fő** módszerrel. 

    > [AZURE.IMPORTANT] Cserélje **storageaccountname** és **accountkey** nevét és Azure tároló fiókjának billentyűt. 

            // create a linked service for input data store: Azure Storage
            Console.WriteLine("Creating Azure Storage linked service");
            client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new LinkedServiceCreateOrUpdateParameters()
                {
                    LinkedService = new LinkedService()
                    {
                        Name = "AzureStorageLinkedService",
                        Properties = new LinkedServiceProperties
                        (
                            new AzureStorageLinkedService("DefaultEndpointsProtocol=https;AccountName=<storageaccountname>;AccountKey=<accountkey>")
                        )
                    }
                }
            );
9. Adja hozzá a következő kódot, amely létrehoz egy **Azure SQL szolgáltatás csatolt** a **fő** módszerrel.
 
    > [AZURE.IMPORTANT] **Kiszolgálónév**, **adatbázisnév**, **felhasználónév**és **jelszó** cserélje azoknak a Azure SQL server, az adatbázis, a felhasználó, és a jelszavát.  

            // create a linked service for output data store: Azure SQL Database
            Console.WriteLine("Creating Azure SQL Database linked service");
            client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new LinkedServiceCreateOrUpdateParameters()
                {
                    LinkedService = new LinkedService()
                    {
                        Name = "AzureSqlLinkedService",
                        Properties = new LinkedServiceProperties
                        (
                            new AzureSqlDatabaseLinkedService("Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>;Password=<password>;Integrated Security=False;Encrypt=True;Connect Timeout=30")
                        )
                    }
                }
            );
10. Adja hozzá a következő kódot, amely a **bemeneti és kimeneti adatkészleteket** a **fő** módszerrel hoz létre. 

            // create input and output datasets
            Console.WriteLine("Creating input and output datasets");
            string Dataset_Source = "DatasetBlobSource";
            string Dataset_Destination = "DatasetAzureSqlDestination";

            Console.WriteLine("Creating input dataset of type: Azure Blob");
            client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,


            new DatasetCreateOrUpdateParameters()
            {
                Dataset = new Dataset()
                {
                    Name = Dataset_Source,
                    Properties = new DatasetProperties()
                    {
                        Structure = new List<DataElement>()
                        {
                            new DataElement() { Name = "FirstName", Type = "String" },
                            new DataElement() { Name = "LastName", Type = "String" }
                        },
                        LinkedServiceName = "AzureStorageLinkedService",
                        TypeProperties = new AzureBlobDataset()
                        {
                            FolderPath = "adftutorial/",
                            FileName = "emp.txt"
                        },
                        External = true,
                        Availability = new Availability()
                        {
                            Frequency = SchedulePeriod.Hour,
                            Interval = 1,
                        },

                        Policy = new Policy()
                        {
                            Validation = new ValidationPolicy()
                            {
                                MinimumRows = 1
                            }
                        }
                    }
                }
            });


            Console.WriteLine("Creating output dataset of type: Azure SQL");
            client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new DatasetCreateOrUpdateParameters()
                {
                    Dataset = new Dataset()
                    {
                        Name = Dataset_Destination,
                        Properties = new DatasetProperties()
                        {
                            Structure = new List<DataElement>()
                            {
                                new DataElement() { Name = "FirstName", Type = "String" },
                                new DataElement() { Name = "LastName", Type = "String" }
                            },
                            LinkedServiceName = "AzureSqlLinkedService",
                            TypeProperties = new AzureSqlTableDataset()
                            {
                                TableName = "emp"
                            },

                            Availability = new Availability()
                            {
                                Frequency = SchedulePeriod.Hour,
                                Interval = 1,
                            },
                        }
                    }
                });

11. A következő kódot, hogy hozzáadása **hoz létre, és a folyamat aktiválja** a **fő** módszerrel. Ez a folyamat, amely **BlobSource** adatforrásként **CopyActivity** és **BlobSink** megegyezik egy gyűjtő.

            // create a pipeline
            Console.WriteLine("Creating a pipeline");
            DateTime PipelineActivePeriodStartTime = new DateTime(2016, 8, 9, 0, 0, 0, 0, DateTimeKind.Utc);
            DateTime PipelineActivePeriodEndTime = PipelineActivePeriodStartTime.AddMinutes(60);
            string PipelineName = "ADFTutorialPipeline";

            client.Pipelines.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new PipelineCreateOrUpdateParameters()
                {
                    Pipeline = new Pipeline()
                    {
                        Name = PipelineName,
                        Properties = new PipelineProperties()
                        {
                            Description = "Demo Pipeline for data transfer between blobs",

                    // Initial value for pipeline's active period. With this, you won't need to set slice status
                    Start = PipelineActivePeriodStartTime,
                            End = PipelineActivePeriodEndTime,

                            Activities = new List<Activity>()
                            {
                                new Activity()
                                {
                                    Name = "BlobToAzureSql",
                                    Inputs = new List<ActivityInput>()
                                    {
                                        new ActivityInput() {
                                            Name = Dataset_Source
                                        }
                                    },
                                    Outputs = new List<ActivityOutput>()
                                    {
                                        new ActivityOutput()
                                        {
                                            Name = Dataset_Destination
                                        }
                                    },
                                    TypeProperties = new CopyActivity()
                                    {
                                        Source = new BlobSource(),
                                        Sink = new BlobSink()
                                        {
                                            WriteBatchSize = 10000,
                                            WriteBatchTimeout = TimeSpan.FromMinutes(10)
                                        }
                                    }
                                }
                            },
                        }
                    }
                }); 

12. A következő kód hozzáadása a **fő** módszert állapotuk egy szeletre, a kimeneti adatkészletből. Csak a következő példában a várt szeletet van.   
 
            // Pulling status within a timeout threshold
            DateTime start = DateTime.Now;
            bool done = false;

            while (DateTime.Now - start < TimeSpan.FromMinutes(5) && !done)
            {
                Console.WriteLine("Pulling the slice status");
                // wait before the next status check
                Thread.Sleep(1000 * 12);

                var datalistResponse = client.DataSlices.List(resourceGroupName, dataFactoryName, Dataset_Destination,
                    new DataSliceListParameters()
                    {
                        DataSliceRangeStartTime = PipelineActivePeriodStartTime.ConvertToISO8601DateTimeString(),
                        DataSliceRangeEndTime = PipelineActivePeriodEndTime.ConvertToISO8601DateTimeString()
                    });

                foreach (DataSlice slice in datalistResponse.DataSlices)
                {
                    if (slice.State == DataSliceState.Failed || slice.State == DataSliceState.Ready)
                    {
                        Console.WriteLine("Slice execution is done with status: {0}", slice.State);
                        done = true;
                        break;
                    }
                    else
                    {
                        Console.WriteLine("Slice status is: {0}", slice.State);
                    }
                }
            }

14. Adja hozzá a következő kódot vonatkozó adatok szelet első futtatása a **fő** módszerrel.

            Console.WriteLine("Getting run details of a data slice");

            // give it a few minutes for the output slice to be ready
            Console.WriteLine("\nGive it a few minutes for the output slice to be ready and press any key.");
            Console.ReadKey();

            var datasliceRunListResponse = client.DataSliceRuns.List(
                    resourceGroupName,
                    dataFactoryName,
                    Dataset_Destination,
                    new DataSliceRunListParameters()
                    {
                        DataSliceStartTime = PipelineActivePeriodStartTime.ConvertToISO8601DateTimeString()
                    }
                );

            foreach (DataSliceRun run in datasliceRunListResponse.DataSliceRuns)
            {
                Console.WriteLine("Status: \t\t{0}", run.Status);
                Console.WriteLine("DataSliceStart: \t{0}", run.DataSliceStart);
                Console.WriteLine("DataSliceEnd: \t\t{0}", run.DataSliceEnd);
                Console.WriteLine("ActivityId: \t\t{0}", run.ActivityName);
                Console.WriteLine("ProcessingStartTime: \t{0}", run.ProcessingStartTime);
                Console.WriteLine("ProcessingEndTime: \t{0}", run.ProcessingEndTime);
                Console.WriteLine("ErrorMessage: \t{0}", run.ErrorMessage);
            }

            Console.WriteLine("\nPress any key to exit.");
            Console.ReadKey();

13. Adja hozzá a következő segítő módszer a **fő** módszert a **Program** osztály által használt.  
 
        public static string GetAuthorizationHeader()
        {
            AuthenticationResult result = null;
            var thread = new Thread(() =>
            {
                try
                {
                    var context = new AuthenticationContext(ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] + ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);

                    ClientCredential credential = new ClientCredential(ConfigurationManager.AppSettings["ApplicationId"], ConfigurationManager.AppSettings["Password"]);
                    result = context.AcquireToken(resource: ConfigurationManager.AppSettings["WindowsManagementUri"], clientCredential: credential);
                }
                catch (Exception threadEx)
                {
                    Console.WriteLine(threadEx.Message);
                }
            });

            thread.SetApartmentState(ApartmentState.STA);
            thread.Name = "AcquireTokenThread";
            thread.Start();
            thread.Join();

            if (result != null)
            {
                return result.AccessToken;
            }

            throw new InvalidOperationException("Failed to acquire token");
        }  


15. A megoldást Explorer bontsa ki a projektet (**DataFactoryAPITestApp**), kattintson a jobb gombbal a **hivatkozást**, és kattintson a **Hivatkozás hozzáadása**gombra. Jelölje be az "**System.Configuration**" összeállítási jelölőnégyzetet, és kattintson az **OK gombra**. 
16. A New összeállítása. Kattintson a **Szerkesztés** menü, és kattintson a **Szerkesztés megoldás**. 
16. Győződjön meg arról, hogy nincs legalább egy fájlt a **adftutorial** tárolóban Azure blob-tárolóhoz. Ha nem, a Jegyzettömb **Emp.txt** fájl létrehozása a következő tartalommal, és töltse fel a adftutorial tároló.

        John, Doe
        Jane, Doe
     
17. Indítása a minta **hibakeresési** -> **Hibakeresési indítsa el** a menüben. Amikor megjelenik az **első futtatása a szeletre részleteit**, várjon néhány percet, és nyomja le az **ENTER BILLENTYŰT**. 
18. Az Azure portal segítségével ellenőrizze, hogy az adatok gyári **APITutorialFactory** jön létre a következő eltérések rendelkező: 
    - Csatolt szolgáltatás: **LinkedService_AzureStorage** 
    - Adatkészlet: **DatasetBlobSource** és **DatasetBlobDestination**.
    - Folyamat: **PipelineBlobSample** 
18. Győződjön meg arról, hogy a két alkalmazott jönnek létre bejegyzések a "**a vállalati projektirányítási**" tábla a megadott Azure SQL-adatbázisban.

## <a name="next-steps"></a>Következő lépések

- Olvassa el a [Mozgás a tevékenységekre vonatkozó adatok](data-factory-data-movement-activities.md) a cikket, amely a Másolás tevékenység használta az oktatóprogram részletes információt tartalmaz.
- Kapcsolatos további tudnivalók: [Adatok gyári .NET API hivatkozási](https://msdn.microsoft.com/library/mt415893.aspx) adatok gyári .NET SDK csomagjában talál. Ez a cikk nem tárgyalja összes az adatok gyári .NET API-t. 

 
