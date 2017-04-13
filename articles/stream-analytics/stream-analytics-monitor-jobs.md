<properties
    pageTitle="Értékáram-elemzés feladatok programozás útján figyelése |} Microsoft Azure"
    description="Megtudhatja, hogy miként programozás útján figyelheti a REST API-hoz, Azure SDK vagy Powershell keresztül létrehozott Értékáram-elemzés feladatok."
    keywords=".NET monitor, a feldolgozás képernyő, alkalmazás figyelése"
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


# <a name="programmatically-create-a-stream-analytics-job-monitor"></a>Értékáram-elemzés feldolgozás képernyő programozás útján létrehozása
 Ez a cikk bemutatja, hogyan ahhoz, hogy Értékáram-elemzés feladat figyelése. Végezze el a REST API-hoz, Azure SDK vagy Powershell keresztül létrehozott feladatok adatfolyam Analytics van figyelése alapértelmezés szerint nincs engedélyezve.  Manuális engedélyezheti Ez az Azure-portálon Navigálás a feladat Monitor lapra, és kattintson az Engedélyezés gombra, vagy a folyamat automatizálhatja az ebben a cikkben leírt lépéseket követve. Ellenőrző adatok jelennek meg a az Azure portálon, a megjelenítő Értékáram-elemzés projektre vonatkozóan a "Képernyő" fülre.

![feladat monitor feladatok lapon](./media/stream-analytics-monitor-jobs/stream-analytics-monitor-jobs-tab.png)

## <a name="prerequisites"></a>Előfeltételek
Ez a cikk megkezdése előtt a következőket kell rendelkeznie:

- Visual Studio 2012 vagy 2013-ban.
- Töltse le és telepítse az [Azure .NET SDK csomagjában talál](https://azure.microsoft.com/downloads/).
- Egy meglévő Értékáram-elemzés feladat elvégzendő ellenőrzés engedélyezve van.

## <a name="setup-a-project"></a>A projekt beállítása

1.  Visual Studio C# .net console-alkalmazás létrehozása.
2.  A csomag Manager konzolban, a következő parancsokat a NuGet csomagok telepítése. A legelső Azure adatfolyam Analytics Management .NET SDK. A második lehetőséget az Azure Monitor SDK ahhoz, hogy figyelése használt. Az utolsó egyik az Azure Active Directory-ügyfél hitelesítéshez használt lesz.

    ```
    Install-Package Microsoft.Azure.Management.StreamAnalytics
    Install-Package Microsoft.Azure.Insights -Pre
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
    ```

3.  A következő appSettings szakasz hozzáadása a App.config fájlt.

    ```
    <appSettings>
        <!--CSM Prod related values-->
        <add key="ResourceGroupName" value="RESOURCE GROUP NAME" />
        <add key="JobName" value="YOUR JOB NAME" />
        <add key="StorageAccountName" value="YOUR STORAGE ACCOUNT"/>
        <add key="ActiveDirectoryEndpoint" value="https://login.windows.net/" />
        <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
        <add key="WindowsManagementUri" value="https://management.core.windows.net/" />
        <add key="AsaClientId" value="1950a258-227b-4e31-a9cf-717495945fc2" />
        <add key="RedirectUri" value="urn:ietf:wg:oauth:2.0:oob" />
        <add key="SubscriptionId" value="YOUR AZURE SUBSCRIPTION ID" />
        <add key="ActiveDirectoryTenantId" value="YOUR TENANT ID" />
    </appSettings>
    ```
Értékek lecserélése *SubscriptionId* és *ActiveDirectoryTenantId* az Azure-előfizetéséhez, és lehetővé tevő dokumentumazonosítók bérlői. Az alábbi PowerShell-parancsmag futtatásával elérheti ezeket az értékeket:

    ```
    Get-AzureAccount
    ```
4.  Adja hozzá a következő kimutatások használatával a forrásfájl (Program.cs) a projektben.

    ```
        using System;
        using System.Configuration;
        using System.Threading;
        using Microsoft.Azure;
        using Microsoft.Azure.Management.Insights;
        using Microsoft.Azure.Management.Insights.Models;
        using Microsoft.Azure.Management.StreamAnalytics;
        using Microsoft.Azure.Management.StreamAnalytics.Models;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
    ```
5.  Adja hozzá a segítő hitelesítési módszert.

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

## <a name="create-management-clients"></a>Hozzon létre az ügyfelek kezelése
A következő kódot a szükséges változók és az ügyfelek kezelése beállítása lesz.

    string resourceGroupName = "<YOUR AZURE RESOURCE GROUP NAME>";
    string streamAnalyticsJobName = "<YOUR STREAM ANALYTICS JOB NAME>";

    // Get authentication token
    TokenCloudCredentials aadTokenCredentials =
        new TokenCloudCredentials(
            ConfigurationManager.AppSettings["SubscriptionId"],
            GetAuthorizationHeader());

    Uri resourceManagerUri = new
    Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

    // Create Stream Analytics and Insights management client
    StreamAnalyticsManagementClient streamAnalyticsClient = new
    StreamAnalyticsManagementClient(aadTokenCredentials, resourceManagerUri);
    InsightsManagementClient insightsClient = new
    InsightsManagementClient(aadTokenCredentials, resourceManagerUri);

## <a name="enable-monitoring-for-an-existing-stream-analytics-job"></a>Egy meglévő Értékáram-elemzés feladat figyelése engedélyezése

A következő kódot lehetővé teszi, egy **meglévő** Értékáram-elemzés feladat figyelése. A kód első része beolvasni adatait az adott Értékáram-elemzés projekthez ellen, a megjelenítő Értékáram-elemzés szolgáltatás GET kérésekben hajt végre. Akkor használja a "Azonosító" tulajdonság (a GET kérésekben beolvasása) paraméterként helyezése módszer a második fele a kód, amely egy helyezése küld kérése a hírcsatornájában szolgáltatás felügyeletet igényel, a megjelenítő Értékáram-elemzés feladathoz engedélyezéséhez.

> [AZURE.WARNING]
> Ha korábban engedélyezte a figyelése egy másik Értékáram-elemzés feladatot, vagy az Azure-portálon keresztül, vagy programozás útján keresztül az alábbi kódot, **azt ajánljuk, hogy Ön megadja-e a tárhely fiók névvel, figyelése korábban bekapcsolásakor elvégzett.**
>
> A tárterület-fiók van csatolva a régió, a megjelenítő Értékáram-elemzés feladatok a létrehozott, külön nem a feladat magát.
>
> Az összes Értékáram-elemzés feladat (és más Azure erőforrások) azonos régió megosztása a tárterület-fiók felügyeleti adatok tárolására. Ha megad egy másik tárterület-fiókkal, akkor várt hatásai a Értékáram-elemzés feladatok és/vagy más erőforrások: Azure figyelemmel kísérése jelenhet meg.
>
> A tároló fióknév cseréje használt ```“<YOUR STORAGE ACCOUNT NAME>”``` az alábbi kell a tároló fiókhoz, ugyanabban az előfizetésben, a megjelenítő Értékáram-elemzés feladatot engedélyezi figyelése.

    // Get an existing Stream Analytics job
    JobGetParameters jobGetParameters = new JobGetParameters()
    {
        PropertiesToExpand = "inputs,transformation,outputs"
    };
    JobGetResponse jobGetResponse = streamAnalyticsClient.StreamingJobs.Get(resourceGroupName, streamAnalyticsJobName, jobGetParameters);

    // Enable monitoring
    ServiceDiagnosticSettingsPutParameters insightPutParameters = new ServiceDiagnosticSettingsPutParameters()
    {
            Properties = new ServiceDiagnosticSettings()
            {
                StorageAccountName = "<YOUR STORAGE ACCOUNT NAME>"
            }
    };
    insightsClient.ServiceDiagnosticSettingsOperations.Put(jobGetResponse.Job.Id, insightPutParameters);



## <a name="get-support"></a>Technikai támogatás kérése
További segítségért próbálja meg az [Azure Értékáram-elemzés fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).


## <a name="next-steps"></a>Következő lépések

- [Azure Értékáram-elemzés – bevezetés](stream-analytics-introduction.md)
- [Értékáram-elemzés Azure használatának első lépései](stream-analytics-get-started.md)
- [Azure Értékáram-elemzés feladatok méretezése](stream-analytics-scale-jobs.md)
- [Azure adatfolyam Analytics lekérdezés nyelvi hivatkozása](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure adatfolyam Analytics kezelése REST API-útmutató](https://msdn.microsoft.com/library/azure/dn835031.aspx)
