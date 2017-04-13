<properties
    pageTitle="Egy rugalmas adatbázis-készlet létrehozása a és C# |} Microsoft Azure"
    description="C# adatbázis fejlesztési technikákat segítségével méretezhető rugalmas adatbázis-készlet létrehozása Azure SQL-adatbázisban, így azok megoszthatók erőforrások sok adatbázis keresztül."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="csharp"
    ms.workload="data-management"
    ms.date="10/04/2016"
    ms.author="sstein"/>

# <a name="create-an-elastic-database-pool-with-cx23"></a>A C és 23; #x egy rugalmas adatbázis-készlet létrehozása

> [AZURE.SELECTOR]
- [Azure portál](sql-database-elastic-pool-create-portal.md)
- [A PowerShell](sql-database-elastic-pool-create-powershell.md)
- [C#](sql-database-elastic-pool-create-csharp.md)


Ez a cikk ismerteti, hogyan segítségével C# egy Azure SQL-adatbázis rugalmas készlet létrehozása az [Azure SQL-adatbázis tár a .NET rendszerhez](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql). Hozzon létre egy önálló SQL-adatbázisban, tanulmányozza a [használata C# .NET SQL-adatbázis tárral SQL-adatbázis létrehozása](sql-database-get-started-csharp.md).

A Azure SQL-adatbázis tár a .NET rendszerhez tartalmaz egy [Erőforrás-kezelő Azure](../azure-resource-manager/resource-group-overview.md)-alapú API tördelt az [Erőforrás-kezelő-alapú SQL adatbázis REST API -t](https://msdn.microsoft.com/library/azure/mt163571.aspx).

>[AZURE.NOTE] Sok új funkcióval SQL-adatbázis használata esetén megadhatja az [erőforrás-kezelő Azure telepítési modell](../azure-resource-manager/resource-group-overview.md), így mindig a legújabb érdemes használni csak támogatott **Azure SQL adatbázis könyvtár a .NET rendszerhez ([dokumentumok](https://msdn.microsoft.com/library/azure/mt349017.aspx) | [NuGet csomag](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql))**. A régebbi [Klasszikus telepítési modell alapú tárak](https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Sql) támogatott csak visszamenőleges kompatibilitás, javasoljuk, hogy az újabb erőforrás-kezelő alapú tárak használata.

A jelen cikkben ismertetett lépések elvégzéséhez az alábbiakra van szükség:

- Egy Azure-előfizetést. Ha van szüksége az Azure előfizetéssel egyszerűen **Ingyenes FIÓKOT** a lap tetején kattintson, és ezután térjen vissza az Ez a cikk Befejezés.
- Visual Studio. Visual Studio ingyenes másolatának talál a [Visual Studio letöltési](https://www.visualstudio.com/downloads/download-visual-studio-vs) lapjáról.


## <a name="create-a-console-app-and-install-the-required-libraries"></a>Konzol alkalmazás létrehozása, és a szükséges tárak telepítése

1. Indítsa el a Visual Studio.
2. Kattintson a **fájl** > **Új** > **Projekt**.
3. Hozzon létre egy C# **Konzol alkalmazást** , és nevezze el: *SqlElasticPoolConsoleApp*


SQL-adatbázis létrehozása a és C#, töltse be a szükséges irányítás tárak ( [csomag kezelője konzol](http://docs.nuget.org/Consume/Package-Manager-Console)használata):

1. Kattintson az **eszközök** > **NuGet csomag Manager** > **Csomag kezelője konzol**.
2. Típus `Install-Package Microsoft.Azure.Management.Sql –Pre` a [Microsoft Azure SQL-könyvtár](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql)telepítéséhez.
3. Típus `Install-Package Microsoft.Azure.Management.ResourceManager –Pre` telepíteni a [Microsoft Azure erőforrás-kezelő tárat](https://www.nuget.org/packages/Microsoft.Azure.Management.ResourceManager).
4. Típus `Install-Package Microsoft.Azure.Common.Authentication –Pre` a [Microsoft Azure közös Authentication Library](https://www.nuget.org/packages/Microsoft.Azure.Common.Authentication)telepítéséhez. 



> [AZURE.NOTE] Ez a cikk példáiban az egyes API kérelmek szinkron képernyőt, és a rögzítés a mindaddig, amíg a többi kiegészítésének hívja fel az alapul szolgáló szolgáltatása. Aszinkron módszer áll rendelkezésre.


## <a name="create-a-sql-elastic-database-pool---c-example"></a>SQL rugalmas adatbázis-készlet létrehozása - C# példa

Az alábbi példa létrehoz egy erőforráscsoport, a kiszolgáló, a tűzfal szabály, a rugalmas készlet, és ekkor létrehoz egy SQL-adatbázis-készletben. Lásd: [létrehozása egyszerű erőforrások eléréséhez szolgáltatás](#create-a-service-principal-to-access-resources) első a `_subscriptionId, _tenantId, _applicationId, and _applicationSecret` változók.

**Program.cs** tartalmának cserélje ki az alábbi, és frissítse a `{variables}` az alkalmazás értékű (ne kerüljön bele a `{}`).


```
using Microsoft.Azure;
using Microsoft.Azure.Management.ResourceManager;
using Microsoft.Azure.Management.ResourceManager.Models;
using Microsoft.Azure.Management.Sql;
using Microsoft.Azure.Management.Sql.Models;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using System;

namespace SqlElasticPoolConsoleApp
{
    class Program
        {

        // For details about these four (4) values, see
        // https://azure.microsoft.com/documentation/articles/resource-group-authenticate-service-principal/
        static string _subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}";
        static string _tenantId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}";
        static string _applicationId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}";
        static string _applicationSecret = "{your-password}";

        // Create management clients for the Azure resources your app needs to work with.
        // This app works with Resource Groups, and Azure SQL Database.
        static ResourceManagementClient _resourceMgmtClient;
        static SqlManagementClient _sqlMgmtClient;

        // Authentication token
        static AuthenticationResult _token;

        // Azure resource variables
        static string _resourceGroupName = "{resource-group-name}";
        static string _resourceGrouplocation = "{Azure-region}";

        static string _serverlocation = _resourceGrouplocation;
        static string _serverName = "{server-name}";
        static string _serverAdmin = "{server-admin-login}";
        static string _serverAdminPassword = "{server-admin-password}";

        static string _firewallRuleName = "{firewall-rule-name}";
        static string _startIpAddress = "{0.0.0.0}";
        static string _endIpAddress = "{255.255.255.255}";

        static string _poolName = "{pool-name}";
        static string _poolEdition = "{Standard}";
        static int _poolDtus = {100};
        static int _databaseMinDtus = {10};
        static int _databaseMaxDtus = {100};

        static string _databaseName = "{elasticdbfromcsarticle}";



        static void Main(string[] args)
        {
            // Authenticate:
            _token = GetToken(_tenantId, _applicationId, _applicationSecret);
            Console.WriteLine("Token acquired. Expires on:" + _token.ExpiresOn);

            // Instantiate management clients:
            _resourceMgmtClient = new ResourceManagementClient(new Microsoft.Rest.TokenCredentials(_token.AccessToken));
            _sqlMgmtClient = new SqlManagementClient(new TokenCloudCredentials(_subscriptionId, _token.AccessToken));


            Console.WriteLine("Resource group...");
            ResourceGroup rg = CreateOrUpdateResourceGroup(_resourceMgmtClient, _subscriptionId, _resourceGroupName, _resourceGrouplocation);
            Console.WriteLine("Resource group: " + rg.Id);


            Console.WriteLine("Server...");
            ServerGetResponse sgr = CreateOrUpdateServer(_sqlMgmtClient, _resourceGroupName, _serverlocation, _serverName, _serverAdmin, _serverAdminPassword);
            Console.WriteLine("Server: " + sgr.Server.Id);

            Console.WriteLine("Server firewall...");
            FirewallRuleGetResponse fwr = CreateOrUpdateFirewallRule(_sqlMgmtClient, _resourceGroupName, _serverName, _firewallRuleName, _startIpAddress, _endIpAddress);
            Console.WriteLine("Server firewall: " + fwr.FirewallRule.Id);

            Console.WriteLine("Elastic pool...");
            ElasticPoolCreateOrUpdateResponse epr = CreateOrUpdateElasticDatabasePool(_sqlMgmtClient, _resourceGroupName, _serverName, _poolName, _poolEdition, _poolDtus, _databaseMinDtus, _databaseMaxDtus);
            Console.WriteLine("Elastic pool: " + epr.ElasticPool.Id);

            Console.WriteLine("Database...");
            DatabaseCreateOrUpdateResponse dbr = CreateOrUpdateDatabase(_sqlMgmtClient, _resourceGroupName, _serverName, _databaseName, _poolName);
            Console.WriteLine("Database: " + dbr.Database.Id);


            Console.WriteLine("Press any key to continue...");
            Console.ReadKey();
        }

        static ResourceGroup CreateOrUpdateResourceGroup(ResourceManagementClient resourceMgmtClient, string subscriptionId, string resourceGroupName, string resourceGroupLocation)
        {
            ResourceGroup resourceGroupParameters = new ResourceGroup()
            {
                Location = resourceGroupLocation,
            };
            resourceMgmtClient.SubscriptionId = subscriptionId;
            ResourceGroup resourceGroupResult = resourceMgmtClient.ResourceGroups.CreateOrUpdate(resourceGroupName, resourceGroupParameters);
            return resourceGroupResult;
        }

        static ServerGetResponse CreateOrUpdateServer(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverLocation, string serverName, string serverAdmin, string serverAdminPassword)
        {
            ServerCreateOrUpdateParameters serverParameters = new ServerCreateOrUpdateParameters()
            {
                Location = serverLocation,
                Properties = new ServerCreateOrUpdateProperties()
                {
                    AdministratorLogin = serverAdmin,
                    AdministratorLoginPassword = serverAdminPassword,
                    Version = "12.0"
                }
            };
            ServerGetResponse serverResult = sqlMgmtClient.Servers.CreateOrUpdate(resourceGroupName, serverName, serverParameters);
            return serverResult;
        }


        static FirewallRuleGetResponse CreateOrUpdateFirewallRule(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverName, string firewallRuleName, string startIpAddress, string endIpAddress)
        {
            FirewallRuleCreateOrUpdateParameters firewallParameters = new FirewallRuleCreateOrUpdateParameters()
            {
                Properties = new FirewallRuleCreateOrUpdateProperties()
                {
                    StartIpAddress = startIpAddress,
                    EndIpAddress = endIpAddress
                }
            };
            FirewallRuleGetResponse firewallResult = sqlMgmtClient.FirewallRules.CreateOrUpdate(resourceGroupName, serverName, firewallRuleName, firewallParameters);
            return firewallResult;
        }



        static ElasticPoolCreateOrUpdateResponse CreateOrUpdateElasticDatabasePool(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverName, string poolName, string poolEdition, int poolDtus, int databaseMinDtus, int databaseMaxDtus)
        {
            // Retrieve the server that will host this elastic pool
            Server currentServer = sqlMgmtClient.Servers.Get(resourceGroupName, serverName).Server;

            // Create elastic pool: configure create or update parameters and properties explicitly
            ElasticPoolCreateOrUpdateParameters newPoolParameters = new ElasticPoolCreateOrUpdateParameters()
            {
                Location = currentServer.Location,
                Properties = new ElasticPoolCreateOrUpdateProperties()
                {
                    Edition = poolEdition,
                    Dtu = poolDtus,
                    DatabaseDtuMin = databaseMinDtus,
                    DatabaseDtuMax = databaseMaxDtus
                }
            };

            // Create the pool
            var newPoolResponse = sqlMgmtClient.ElasticPools.CreateOrUpdate(resourceGroupName, serverName, poolName, newPoolParameters);
            return newPoolResponse;
        }




        static DatabaseCreateOrUpdateResponse CreateOrUpdateDatabase(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverName, string databaseName, string poolName)
        {
            // Retrieve the server that will host this database
            Server currentServer = sqlMgmtClient.Servers.Get(resourceGroupName, serverName).Server;

            // Create a database: configure create or update parameters and properties explicitly
            DatabaseCreateOrUpdateParameters newDatabaseParameters = new DatabaseCreateOrUpdateParameters()
            {
                Location = currentServer.Location,
                Properties = new DatabaseCreateOrUpdateProperties()
                {
                    CreateMode = DatabaseCreateMode.Default,
                    ElasticPoolName = poolName
                }
            };
            DatabaseCreateOrUpdateResponse dbResponse = sqlMgmtClient.Databases.CreateOrUpdate(resourceGroupName, serverName, databaseName, newDatabaseParameters);
            return dbResponse;
        }



        private static AuthenticationResult GetToken(string tenantId, string applicationId, string applicationSecret)
        {
            AuthenticationContext authContext = new AuthenticationContext("https://login.windows.net/" + tenantId);
            _token = authContext.AcquireToken("https://management.core.windows.net/", new ClientCredential(applicationId, applicationSecret));
            return _token;
        }
    }
}
```





## <a name="create-a-service-principal-to-access-resources"></a>A szolgáltatás fő erőforrások eléréséhez létrehozása

Az alábbi PowerShell parancsprogramot hoz létre, az Active Directory (AD) alkalmazás és a szolgáltatás egyszerű, amely a C# alkalmazás hitelesítő szükség. A parancsprogram szükséges az előző C# minta értékeket exportálja. Részletes tudnivalókért lásd: [Azure PowerShell használatá hozzon létre egy egyszerű erőforrások eléréséhez szolgáltatást](../resource-group-authenticate-service-principal.md).

   
    # Sign in to Azure.
    Add-AzureRmAccount
    
    # If you have multiple subscriptions, uncomment and set to the subscription you want to work with.
    #$subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}"
    #Set-AzureRmContext -SubscriptionId $subscriptionId
    
    # Provide these values for your new AAD app.
    # $appName is the display name for your app, must be unique in your directory.
    # $uri does not need to be a real uri.
    # $secret is a password you create.
    
    $appName = "{app-name}"
    $uri = "http://{app-name}"
    $secret = "{app-password}"
    
    # Create a AAD app
    $azureAdApplication = New-AzureRmADApplication -DisplayName $appName -HomePage $Uri -IdentifierUris $Uri -Password $secret
    
    # Create a Service Principal for the app
    $svcprincipal = New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId
    
    # To avoid a PrincipalNotFound error, I pause here for 15 seconds.
    Start-Sleep -s 15
    
    # If you still get a PrincipalNotFound error, then rerun the following until successful. 
    $roleassignment = New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $azureAdApplication.ApplicationId.Guid
    
    
    # Output the values we need for our C# application to successfully authenticate
    
    Write-Output "Copy these values into the C# sample app"
    
    Write-Output "_subscriptionId:" (Get-AzureRmContext).Subscription.SubscriptionId
    Write-Output "_tenantId:" (Get-AzureRmContext).Tenant.TenantId
    Write-Output "_applicationId:" $azureAdApplication.ApplicationId.Guid
    Write-Output "_applicationSecret:" $secret


  

## <a name="next-steps"></a>Következő lépések

- [A készlet kezelése](sql-database-elastic-pool-manage-csharp.md)
- [Rugalmas feladatok létrehozása](sql-database-elastic-jobs-overview.md): rugalmas feladatok lehetővé teszik az SQL-T parancsprogramokat futtassanak adatbázisok egy tetszőleges számú.
- [Méretezés kifelé az Azure SQL-adatbázis](sql-database-elastic-scale-introduction.md): méretezése rugalmas Adatbáziseszközök használatával.

## <a name="additional-resources"></a>További források

- [SQL-adatbázis](https://azure.microsoft.com/documentation/services/sql-database/)
- [Azure erőforrás-kezelés API-k](https://msdn.microsoft.com/library/azure/dn948464.aspx)
