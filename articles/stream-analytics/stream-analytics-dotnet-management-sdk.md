<properties
    pageTitle="Értékáram-elemzés .NET SDK kezelésének |} Microsoft Azure"
    description="Első lépések az adatfolyam Analytics Management .NET SDK. Megtudhatja, hogy miként állíthatja be és analytics feladatok futtatása: hozzon létre egy projektet, a bemeneti adatok alapján, a kimeneti értékeket és a átalakítások."
    keywords=".NET SDK analytics API"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>


# <a name="management-net-sdk-set-up-and-run-analytics-jobs-using-the-azure-stream-analytics-api-for-net"></a>.NET SDK kezelése: Állítsa be, és futtassa az Azure-adatfolyam Analytics API segítségével a .NET rendszerhez analytics-feladatok

Megtudhatja, hogy miként állíthat be egy futtatása analytics-feladatok az adatfolyam Analytics API segítségével a .NET rendszerhez, a kezelés .NET SDK használatával. Projekt beállítása, bemeneti és kimeneti források, átalakítások és kezdő létrehozása és feladatok leállítása. A analytics feladatok folyamatos átvitelére adatok Blob-tárolóhoz vagy egy esemény központból.

Az [adatkezelési hivatkozás dokumentációjában találhat a .NET rendszerhez, a megjelenítő Értékáram Analytics API](https://msdn.microsoft.com/library/azure/dn889315.aspx)talál.

Azure Értékáram-elemzés olyan teljes körű felügyelt szolgáltatás, alacsony-késés, könnyen hozzáférhető, méretezhető, összetett esemény feldolgozása nyújtó fölé adatfolyam a felhőben. Értékáram-elemzés lehetővé teszi, hogy az ügyfelek számára a folyamatos átvitelű feladatok beállítása adatfolyamok elemzéséhez, és lehetővé teszi a meghajtó valós idejű analytics közelében.  


## <a name="prerequisites"></a>Előfeltételek
Ez a cikk megkezdése előtt a következőket kell rendelkeznie:

- Telepítse az Visual Studio 2012 vagy 2013-ban.
- Töltse le és telepítse az [Azure .NET SDK csomagjában talál](https://azure.microsoft.com/downloads/).
- Hozzon létre egy Azure erőforráscsoport előfizetéséhez. Az alábbiakban Azure PowerShell mintaparancsfájl. Azure PowerShell információ [Telepítse és állítsa be a Azure PowerShell](../powershell-install-configure.md);  


        # Log in to your Azure account
        Add-AzureAccount

        # Select the Azure subscription you want to use to create the resource group
        Select-AzureSubscription -SubscriptionName <subscription name>

            # If Stream Analytics has not been registered to the subscription, remove the remark symbol (#) to run the Register-AzureRMProvider cmdlet to register the provider namespace
            #Register-AzureRMProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'

        # Create an Azure resource group
        New-AzureResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>


-   Állítsa be a bemeneti forrás- és kimeneti. Fellelhetõ útmutatást talál [Hozzáadása ráfordítások](stream-analytics-add-inputs.md) beállítása egy minta beviteli és [Hozzáadása kimeneti értékeket](stream-analytics-add-outputs.md) egy minta kimeneti beállítása


## <a name="set-up-a-project"></a>A projekt előkészítése

Segítségével hozhat létre egy analytics feladatot a adatfolyam Analytics API a .NET rendszerhez, először állítsa be a projekt.

1. Hozzon létre egy Visual Studio C# .NET konzol alkalmazást.
2. A csomag Manager konzolban, a következő parancsokat a NuGet csomagok telepítése. A legelső Azure adatfolyam Analytics Management .NET SDK. A második lehetőséget az Azure Active Directory-ügyfél hitelesítéshez használt.

        Install-Package Microsoft.Azure.Management.StreamAnalytics
        Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory

4. A következő **appSettings** szakasz hozzáadása a App.config fájlt:

        <appSettings>
          <!--CSM Prod related values-->
          <add key="ActiveDirectoryEndpoint" value="https://login.windows.net/" />
          <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
          <add key="WindowsManagementUri" value="https://management.core.windows.net/" />
          <add key="AsaClientId" value="1950a258-227b-4e31-a9cf-717495945fc2" />
          <add key="RedirectUri" value="urn:ietf:wg:oauth:2.0:oob" />
          <add key="SubscriptionId" value="YOUR AZURE SUBSCRIPTION" />
          <add key="ActiveDirectoryTenantId" value="YOU TENANT ID" />
        </appSettings>


    Értékek lecserélése **SubscriptionId** és **ActiveDirectoryTenantId** az Azure-előfizetéséhez, és lehetővé tevő dokumentumazonosítók bérlői. A következő Azure PowerShell-parancsmag futtatásával elérheti ezeket az értékeket:

        Get-AzureAccount

5. A következő **használata** kimutatásokban hozzáadása a project a forrásfájl nélkül (Program.cs):

        using System;
        using System.Configuration;
        using System.Threading;
        using Microsoft.Azure;
        using Microsoft.Azure.Management.StreamAnalytics;
        using Microsoft.Azure.Management.StreamAnalytics.Models;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;

6.  Adja hozzá a segítő hitelesítési módszert:

        public static string GetAuthorizationHeader()
        {
            AuthenticationResult result = null;
            var thread = new Thread(() =>
            {
                try
                {
                    var context = new AuthenticationContext(
                        ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] +
                        ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);

                    result = context.AcquireToken(
                        resource: ConfigurationManager.AppSettings["WindowsManagementUri"],
                        clientId: ConfigurationManager.AppSettings["AsaClientId"],
                        redirectUri: new Uri(ConfigurationManager.AppSettings["RedirectUri"]),
                        promptBehavior: PromptBehavior.Always);
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


## <a name="create-a-stream-analytics-management-client"></a>Értékáram-elemzés management ügyfél létrehozása

Egy **StreamAnalyticsManagementClient** objektum lehetővé teszi, hogy a feladat és feladat-összetevők, például beviteli, kimeneti és átalakítási kezelése.

A következő kód hozzáadása a **fő** módszer az elején:

    string resourceGroupName = "<YOUR AZURE RESOURCE GROUP NAME>";
    string streamAnalyticsJobName = "<YOUR STREAM ANALYTICS JOB NAME>";
    string streamAnalyticsInputName = "<YOUR JOB INPUT NAME>";
    string streamAnalyticsOutputName = "<YOUR JOB OUTPUT NAME>";
    string streamAnalyticsTransformationName = "<YOUR JOB TRANSFORMATION NAME>";

    // Get authentication token
    TokenCloudCredentials aadTokenCredentials =
        new TokenCloudCredentials(
            ConfigurationManager.AppSettings["SubscriptionId"],
            GetAuthorizationHeader());

    // Create Stream Analytics management client
    StreamAnalyticsManagementClient client = new StreamAnalyticsManagementClient(aadTokenCredentials);

A **resourceGroupName** változó érték megegyezik a csoport nevét, az erőforrás létrehozott, vagy a kiválasztott kapcsolatban előzetesen szükséges lépéseket kell lennie.

[A szolgáltatás egyszerű Azure erőforrás-kezelővel hitelesítő](../resource-group-authenticate-service-principal.md)automatizálhatja a feladat létrehozása a hitelesítő adatok bemutató eleme, hivatkozhat.

Ez a cikk a hátralévő szakaszok feltételezik, hogy ezt a kódot a **fő** módszer az elején.

## <a name="create-a-stream-analytics-job"></a>Értékáram-elemzés feladat létrehozása

A következő kódot hoz létre az erőforrás csoportban megadott Értékáram-elemzés feladatot. Ön hozzáadja beviteli, a kibocsátás és a átalakítása a feladat később.

    // Create a Stream Analytics job
    JobCreateOrUpdateParameters jobCreateParameters = new JobCreateOrUpdateParameters()
    {
        Job = new Job()
        {
            Name = streamAnalyticsJobName,
            Location = "<LOCATION>",
            Properties = new JobProperties()
            {
                EventsOutOfOrderPolicy = EventsOutOfOrderPolicy.Adjust,
                Sku = new Sku()
                {
                    Name = "Standard"
                }
            }
        }
    };

    JobCreateOrUpdateResponse jobCreateResponse = client.StreamingJobs.CreateOrUpdate(resourceGroupName, jobCreateParameters);


## <a name="create-a-stream-analytics-input-source"></a>Értékáram-elemzés beviteli adatforrás létrehozása

A következő kódot Értékáram-elemzés beviteli forrást a bemeneti blob típusa és a CSV-szerializálási hoz létre. Hozzon létre egy központi beviteli forrás, amellyel **EventHubStreamInputDataSource** **BlobStreamInputDataSource**helyett. Hasonlóképpen testre szabhatja a bemeneti forrás szerializálási típusát.

    // Create a Stream Analytics input source
    InputCreateOrUpdateParameters jobInputCreateParameters = new InputCreateOrUpdateParameters()
    {
        Input = new Input()
        {
            Name = streamAnalyticsInputName,
            Properties = new StreamInputProperties()
            {
                Serialization = new CsvSerialization
                {
                    Properties = new CsvSerializationProperties
                    {
                        Encoding = "UTF8",
                        FieldDelimiter = ","
                    }
                },
                DataSource = new BlobStreamInputDataSource
                {
                    Properties = new BlobStreamInputDataSourceProperties
                    {
                        StorageAccounts = new StorageAccount[]
                        {
                            new StorageAccount()
                            {
                                AccountName = "<YOUR STORAGE ACCOUNT NAME>",
                                AccountKey = "<YOUR STORAGE ACCOUNT KEY>"
                            }
                        },
                        Container = "samples",
                        PathPattern = ""
                    }
                }
            }
        }
    };

    InputCreateOrUpdateResponse inputCreateResponse =
        client.Inputs.CreateOrUpdate(resourceGroupName, streamAnalyticsJobName, jobInputCreateParameters);

Beviteli forrásokból, a Blob-tárolóhoz vagy egy esemény-központban tartozik egy adott feladathoz. Használja a ugyanarra a bemeneti forrás különböző feladatok, hívja meg újra a módszerrel, és adjon meg egy másik projekt nevet.


## <a name="test-a-stream-analytics-input-source"></a>Értékáram-elemzés beviteli forrás tesztelése

A **TestConnection** módszer azt vizsgálja, hogy a Értékáram-elemzés feladat tud csatlakozni a bemeneti forrás, valamint egyéb bemeneti forrástípus különleges szempontokat. Például az előző lépésben létrehozott blob beviteli forrás, a módszer ellenőrzi, hogy a tárterület-fiók felhasználónevét és a fő pár a tárterület-fiókomhoz, valamint a ellenőrizze, hogy van-e a megadott tároló használható.

    // Test input source connection
    DataSourceTestConnectionResponse inputTestResponse =
        client.Inputs.TestConnection(resourceGroupName, streamAnalyticsJobName, streamAnalyticsInputName);

## <a name="create-a-stream-analytics-output-target"></a>Értékáram-elemzés cél kimeneti létrehozásához

Egy kimenet cél létrehozása nagyon hasonlít Értékáram-elemzés beviteli forrás létrehozása. Cél kimeneti bemeneti forrásokhoz, például egy adott feladathoz tartozik. Kimeneti célhelyen azonos használható különböző feladatok, hívja meg újra a módszerrel, és adjon meg egy másik projekt nevet.

A következő kódrészlet egy kimenet target (Azure SQL-adatbázis) hoz létre. A kimenet cél adattípus és/vagy serialization típus is testre szabhatja.

    // Create a Stream Analytics output target
    OutputCreateOrUpdateParameters jobOutputCreateParameters = new OutputCreateOrUpdateParameters()
    {
        Output = new Output()
        {
            Name = streamAnalyticsOutputName,
            Properties = new OutputProperties()
            {
                DataSource = new SqlAzureOutputDataSource()
                {
                    Properties = new SqlAzureOutputDataSourceProperties()
                    {
                        Server = "<YOUR DATABASE SERVER NAME>",
                        Database = "<YOUR DATABASE NAME>",
                        User = "<YOUR DATABASE LOGIN>",
                        Password = "<YOUR DATABASE LOGIN PASSWORD>",
                        Table = "<YOUR DATABASE TABLE NAME>"
                    }
                }
            }
        }
    };

    OutputCreateOrUpdateResponse outputCreateResponse =
        client.Outputs.CreateOrUpdate(resourceGroupName, streamAnalyticsJobName, jobOutputCreateParameters);

## <a name="test-a-stream-analytics-output-target"></a>Értékáram-elemzés cél kimeneti tesztelése

Értékáram-elemzés cél kimeneti kapcsolatok tesztelése módszer a **TestConnection** tartalmaz.

    // Test output target connection
    DataSourceTestConnectionResponse outputTestResponse =
        client.Outputs.TestConnection(resourceGroupName, streamAnalyticsJobName, streamAnalyticsOutputName);

## <a name="create-a-stream-analytics-transformation"></a>Értékáram-elemzés átalakítás létrehozása

A következő kódot Értékáram-elemzés átalakítás hoz létre a lekérdezést "válassza a * a beviteli", és itt megadhatja, hogy egy adatfolyam egység lefoglalhat a Értékáram-elemzés projektre vonatkozóan. Adatfolyam mennyiség módosítása a további tudnivalókért lásd: a [Méretarány Azure Értékáram-elemzés feladatok](stream-analytics-scale-jobs.md).


    // Create a Stream Analytics transformation
    TransformationCreateOrUpdateParameters transformationCreateParameters = new TransformationCreateOrUpdateParameters()
    {
        Transformation = new Transformation()
        {
            Name = streamAnalyticsTransformationName,
            Properties = new TransformationProperties()
            {
                StreamingUnits = 1,
                Query = "select * from Input"
            }
        }
    };

    var transformationCreateResp =
        client.Transformations.CreateOrUpdate(resourceGroupName, streamAnalyticsJobName, transformationCreateParameters);

Az adott Értékáram-elemzés projekthez alapján jött létre is tartozik a bemeneti és kimeneti, például egy átalakítások.

## <a name="start-a-stream-analytics-job"></a>Értékáram-elemzés feladat indítása
Miután létrehozott egy Értékáram-elemzés feladat és a input(s), output(s) és átalakítási, indítsa el a feladat hívja fel a **Kezdés** módszerrel.

Az alábbi példa a kód indítást Értékáram-elemzés projekt a kezdési időpontjának egyéni kimeneti beállítása December 12, 2012, 12:12:12 UTC:

    // Start a Stream Analytics job
    JobStartParameters jobStartParameters = new JobStartParameters
    {
        OutputStartMode = OutputStartMode.CustomTime,
        OutputStartTime = new DateTime(2012, 12, 12, 0, 0, 0, DateTimeKind.Utc)
    };

    LongRunningOperationResponse jobStartResponse = client.StreamingJobs.Start(resourceGroupName, streamAnalyticsJobName, jobStartParameters);



## <a name="stop-a-stream-analytics-job"></a>Értékáram-elemzés feladat leállítása
Hívja fel a **Stop** metódus leállíthatja a futó Értékáram-elemzés feladatot.

    // Stop a Stream Analytics job
    LongRunningOperationResponse jobStopResponse = client.StreamingJobs.Stop(resourceGroupName, streamAnalyticsJobName);

## <a name="delete-a-stream-analytics-job"></a>Értékáram-elemzés feladat törlése
A **Törlés** módszer a feladatot, valamint az alapul szolgáló alszint erőforrások, köztük az input(s) output(s) és a feladat átalakítása is törli.

    // Delete a Stream Analytics job
    LongRunningOperationResponse jobDeleteResponse = client.StreamingJobs.Delete(resourceGroupName, streamAnalyticsJobName);


## <a name="get-support"></a>Technikai támogatás kérése
További segítségért próbálja meg az [Azure Értékáram-elemzés fórum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).


## <a name="next-steps"></a>Következő lépések

A .NET SDK használatával hozhat létre és futtathat analytics feladatok alapjait tanulási végezte el. További információért olvassa el az alábbiakat:

- [Azure Értékáram-elemzés – bevezetés](stream-analytics-introduction.md)
- [Értékáram-elemzés Azure használatának első lépései](stream-analytics-get-started.md)
- [Azure Értékáram-elemzés feladatok méretezése](stream-analytics-scale-jobs.md)
- [Azure adatfolyam Analytics Management .NET SDK](https://msdn.microsoft.com/library/azure/dn889315.aspx).
- [Azure adatfolyam Analytics lekérdezés nyelvi hivatkozása](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure adatfolyam Analytics kezelése REST API-útmutató](https://msdn.microsoft.com/library/azure/dn835031.aspx)


<!--Image references-->
[5]: ./media/markdown-template-for-new-articles/octocats.png
[6]: ./media/markdown-template-for-new-articles/pretty49.png
[7]: ./media/markdown-template-for-new-articles/channel-9.png


<!--Link references-->
[azure.blob.storage]: http://azure.microsoft.com/documentation/services/storage/
[azure.blob.storage.use]: http://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/

[azure.event.hubs]: http://azure.microsoft.com/services/event-hubs/
[azure.event.hubs.developer.guide]: http://msdn.microsoft.com/library/azure/dn789972.aspx

[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.forum]: http://go.microsoft.com/fwlink/?LinkId=512151

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.developer.guide]: stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
