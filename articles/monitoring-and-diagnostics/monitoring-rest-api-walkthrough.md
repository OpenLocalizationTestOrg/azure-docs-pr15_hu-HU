<properties
    pageTitle="Útmutató a REST API-val figyelése Azure |} Microsoft Azure"
    description="Hogyan lehet kérések hitelesítő és az Azure figyelése REST API-t használja."
    authors="mcollier, rboucher"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="mcollier"/>

# <a name="azure-monitoring-rest-api-walkthrough"></a>Azure REST API-útmutató figyelése
Ez a cikk bemutatja, hogyan hitelesítését, így a kód segítségével a [Microsoft Azure Monitor REST API-hivatkozás](https://msdn.microsoft.com/library/azure/dn931943.aspx).         

Az Azure Monitor API programozás útján beolvasni a rendelkezésre álló alapértelmezett metrikus definíciók (a írjon be egy mérőszám, például a Processzor időt, a összehívások, a stb.), Granularitás és metrikus értékek lehetővé teszi. Miután beolvassa, az adatokat külön adattárban, például az Azure SQL-adatbázissal, DocumentDB vagy Azure adatok tó takaríthat meg. Innen további elemzés végezheti el szükség szerint.

Mellett különböző metrikus adatpontok dolgozik, mint ez a cikk bemutatja, a Monitor API lehetővé teszi riasztási szabályok, tevékenység naplók megtekintése és egyéb elemek listája. Egy teljes listát az elérhető műveletek olvassa el a a [Microsoft Azure Monitor REST API-hivatkozás](https://msdn.microsoft.com/library/azure/dn931943.aspx)című témakört.

## <a name="authenticating-azure-monitor-requests"></a>Azure Monitor kérések hitelesítés

Első lépésként hitelesítést végezni a kérést.

Az Azure Monitor API ellen végrehajtott feladatokat az Azure erőforrás-kezelő hitelesítési modell használata. Emiatt az Azure Active Directory (Azure Active Directory) hitelesíteni kell összes kérelmet. Egy megközelítés hitelesítést végezni az ügyfélalkalmazás létrehozása az Azure Active Directory szolgáltatás egyszerű, valamint a hitelesítés (JWT) jogkivonat. A következő mintaparancsfájl bemutatja, hogyan létrehozása az Azure Active Directory szolgáltatásainak fő PowerShell keresztül. Részletesebb segédlet tekintse át a [hozhat létre egy egyszerű erőforrások eléréséhez szolgáltatás Azure PowerShell használatával](../resource-group-authenticate-service-principal.md#authenticate-service-principal-with-password—powershell)kapcsolatos dokumentációt. Akkor is hozhat [létre egy szolgáltatás alapelvet az Azure portálon keresztül](../resource-group-create-service-principal-portal.md).

```PowerShell
$subscriptionId = "{azure-subscription-id}"
$resourceGroupName = "{resource-group-name}"
$location = "centralus"

# Authenticate to a specific Azure subscription.
Login-AzureRmAccount -SubscriptionId $subscriptionId

# Password for the service principal
$pwd = "{service-principal-password}"

# Create a new Azure AD application
$azureAdApplication = New-AzureRmADApplication `
                        -DisplayName "My Azure Monitor" `
                        -HomePage "https://localhost/azure-monitor" `
                        -IdentifierUris "https://localhost/azure-monitor" `
                        -Password $pwd

# Create a new service principal associated with the designated application
New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

# Assign Reader role to the newly created service principal
New-AzureRmRoleAssignment -RoleDefinitionName Reader `
                          -ServicePrincipalName $azureAdApplication.ApplicationId.Guid

```

A lekérdezés az Azure Monitor API-t, az ügyfélalkalmazás célszerű használni a korábban létrehozott szolgáltatás egyszerű hitelesítést végezni. Az alábbi PowerShell parancsprogramot példában egy megközelítés, az [Active Directory Authentication Library](../active-directory/active-directory-authentication-libraries.md) (ADAL) használata a JWT hitelesítés jogkivonat megkezdéséhez. A JWT jogkivonat az Azure Monitor REST API-HTTP engedélyezési paraméter összehívásokban részeként átadott.

```PowerShell
$azureAdApplication = Get-AzureRmADApplication -IdentifierUri "https://localhost/azure-monitor"

$subscription = Get-AzureRmSubscription -SubscriptionId $subscriptionId

$clientId = $azureAdApplication.ApplicationId.Guid
$tenantId = $subscription.TenantId
$authUrl = "https://login.windows.net/${tenantId}"

$AuthContext = [Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext]$authUrl
$cred = New-Object -TypeName Microsoft.IdentityModel.Clients.ActiveDirectory.ClientCredential -ArgumentList ($clientId, $pwd)
 
$result = $AuthContext.AcquireToken("https://management.core.windows.net/", $cred)

# Build an array of HTTP header values 
$authHeader = @{
'Content-Type'='application/json'
'Accept'='application/json'
'Authorization'=$result.CreateAuthorizationHeader()
}
```

A hitelesítés beállítása lépés befejezése után lekérdezések majd futtatható az Azure Monitor REST API-t. Van két hasznos lekérdezések:

1. Az erőforrás metrikus definíciók lista

2. A metrikus értékek beolvasása


## <a name="retrieve-metric-definitions"></a>Metrikus definíciók beolvasása
>[AZURE.NOTE] A Azure Monitor REST API metrikus definíciók beolvasásához, használja az "2016-03-01" a API-verziót.

```PowerShell
$apiVersion = "2016-03-01"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/${resourceProviderNamespace}/${resourceType}/${resourceName}/providers/microsoft.insights/metricDefinitions?api-version=${apiVersion}"

Invoke-RestMethod -Uri $request `
                  -Headers $authHeader `
                  -Method Get `
                  -Verbose
```
Az Azure összefüggés-alkalmazásokban a metrikus definíciók tűnik az alábbi képernyőképen hasonló:

![ALT "Metrikus definition válasz megtekintése JSON."](./media/monitoring-rest-api-walkthrough/available_metric_definitions_logic_app_json_response_clean.png)

További információ a [lista az Azure Monitor REST API -t az erőforráshoz tartozó metrikus definíciók](https://msdn.microsoft.com/library/azure/mt743621.aspx) dokumentációjában.

## <a name="retrieve-metric-values"></a>Metrikus érték beolvasása
Miután a rendelkezésre álló metrikus definíciók ismertek, majd lehetőség beolvasni a kapcsolódó metrikus értékeket. Mezőbe írja be a mérőszám "értéket" (nem a "localizedValue") szűrési kérelmeket (például beolvasása "CpuTime" és "kérelmeket metrikus adatpontok). Ha nincs szűrő van megadva, az alapértelmezett metrikus adja vissza.

>[AZURE.NOTE] A Azure Monitor REST API metrikus értékek beolvasásához, használja az "2016-06-01" a API-verziót.

**Metódus**: GET

**Kérés URI**: https://management.azure.com/subscriptions/_{Előfizetés azonosítója}_/resourceGroups/_{erőforrás-csoportnevet}_/providers/_{erőforrás-szolgáltató-névtér}_/_{erőforrástípus}_/_{erőforrásnév}_/providers/microsoft.insights/metrics?$filter=_{szűrő}_és az api-verzió =_{apiVersion}_

Például RunsSucceeded metrikus adatpontok az adott idő alatt tartományhoz és az 1 órás egy idő szemek megszerezni a kérelem lenne az alábbi képlettel történik:

```PowerShell
$apiVersion = "2016-06-01"
$filter = "(name.value eq 'RunsSucceeded') and aggregationType eq 'Total' and startTime eq 2016-09-23 and endTime eq 2016-09-24 and timeGrain eq duration'PT1H'"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/${resourceProviderNamespace}/${resourceType}/${resourceName}/providers/microsoft.insights/metrics?`$filter=${filter}&api-version=${apiVersion}"
(Invoke-RestMethod -Uri $request `
                   -Headers $authHeader `
                   -Method Get `
                   -Verbose).Value | ConvertTo-Json
```

Az eredmény a következő képernyőkép példához hasonló lenne jelennek meg:

![ALT "Átlagos válaszidő metrikus értéket megjelenítő JSON válasz"](./media/monitoring-rest-api-walkthrough/available_metrics_logic_app_json_response.png)

Több adatok vagy az összesítés pontok lekérdezni hozzáadása metrikus definition nevét és összesítési típusát a szűrőt, az alábbi példában látható módon:

```PowerShell
$apiVersion = "2016-06-01"
$filter = "(name.value eq 'ActionsCompleted' or name.value eq 'RunsSucceeded') and (aggregationType eq 'Total' or aggregationType eq 'Average') and startTime eq 2016-09-23T13:30:00Z and endTime eq 2016-09-23T14:30:00Z and timeGrain eq duration'PT1M'"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/${resourceProviderNamespace}/${resourceType}/${resourceName}/providers/microsoft.insights/metrics?`$filter=${filter}&api-version=${apiVersion}"
(Invoke-RestMethod -Uri $request `
                   -Headers $authHeader `
                   -Method Get `
                   -Verbose).Value | ConvertTo-Json
```

### <a name="use-armclient"></a>ARMClient használata
PowerShell (mint alább látható), helyett környezetbe [ARMClient](https://github.com/projectkudu/ARMClient) Windows számítógépén. ARMClient automatikusan kezeli az Azure Active Directory authentication (és az eredményül kapott JWT jogkivonat). A szerkezet metrikus adatok visszakeresése ARMClient is használja az alábbi lépéseket:

1. Telepítse a [Chocolatey](https://chocolatey.org/) és [ARMClient](https://github.com/projectkudu/ARMClient).

2. Terminálszolgáltatások ablakban írja be a _bejelentkezési armclient.exe_. Ez a kéri, hogy jelentkezzen be az Azure.

3. Típus _armclient GET [your_resource_id]/providers/microsoft.insights/metricdefinitions?api-version=2016-03-01_

4. Típus _armclient GET [your_resource_id]/providers/microsoft.insights/metrics?api-version=2016-06-01_


![ALT "Használata ARMClient az Azure figyelés REST API végezhető"](./media/monitoring-rest-api-walkthrough/armclient_metricdefinitions.png)


## <a name="retrieve-the-resource-id"></a>Az erőforrás-azonosító beolvasása
A REST API valójában segít megérteni a rendelkezésre álló metrikus definíciókat, Granularitás és kapcsolódó értékeket. Ezt az információt csak akkor hasznos, az [Azure könyvtár](https://msdn.microsoft.com/library/azure/mt417623.aspx)használata esetén.

Az előző kód használata az erőforrás-azonosító értéke nem teljes elérési útját a kívánt Azure erőforrás. Például az Azure-Webappokban elleni lekérdezni az erőforrás-azonosító a következő lesz:

*/Subscriptions/{Subscription-ID}/resourceGroups/{Resource-group-name}/Providers/Microsoft.Web/Sites/{Site-Name}/*

Az alábbi lista tartalmazza az erőforrás azonosító formátumok különböző Azure erőforrások néhány példa:

* **IoT központi** - /subscriptions/_{Előfizetés azonosítója}_/resourceGroups/_{erőforrás-csoportnevet}_/providers/Microsoft.Devices/IotHubs/_{iot-elosztóhoz-neve}_

* **Rugalmas SQL-készlet** - /subscriptions/_{Előfizetés azonosítója}_/resourceGroups/_{erőforrás-csoportnevet}_/providers/Microsoft.Sql/servers/_{alkalmazáskészlet-adatbázis}_/elasticpools/_{sql-készlet-neve}_

* **SQL-adatbázis (v12)** – /subscriptions/_{Előfizetés azonosítója}_/resourceGroups/_{erőforrás-csoportnevet}_/providers/Microsoft.Sql/servers/_{kiszolgálónév}_/databases/_{adatbázisnév}_

* **Szolgáltatás Bus** - /subscriptions/_{Előfizetés azonosítója}_/resourceGroups/_{erőforrás-csoportnevet}_/providers/Microsoft.ServiceBus/_{névtér}_/_{servicebus neve}_

* **Virtuális skála készletek** - /subscriptions/_{Előfizetés azonosítója}_/resourceGroups/_{erőforrás-csoportnevet}_/providers/Microsoft.Compute/virtualMachineScaleSets/_{virtuális neve}_

* **VMs** - /subscriptions/_{Előfizetés azonosítója}_/resourceGroups/_{erőforrás-csoportnevet}_/providers/Microsoft.Compute/virtualMachines/_{virtuális neve}_

* **Esemény hubok** - /subscriptions/_{Előfizetés azonosítója}_/resourceGroups/_{erőforrás-csoportnevet}_/providers/Microsoft.EventHub/namespaces/_{eventhub-névtér}_


Nincsenek alternatív megközelítés az erőforrás-azonosító, beleértve a Intézővel Azure erőforrás, a kívánt erőforrás megtekintése, az Azure-portálon és PowerShell, illetve az Azure CLI beolvasása.

### <a name="azure-resource-explorer"></a>Azure erőforrás Explorer
Az erőforrás-azonosító kívánt erőforrás megkereséséhez egy hasznos megközelítés, az [Azure erőforrás Explorer](https://resources.azure.com) eszközzel. Nyissa meg a kívánt erőforrásnézethez, és tekintse meg az azonosító, ahogy az alábbi képernyőképen látható:

![ALT "Azure erőforrás Explorer"](./media/monitoring-rest-api-walkthrough/azure_resource_explorer.png)

### <a name="azure-portal"></a>Azure portál
Az erőforrás-azonosító is szerezhető be az Azure portálról. Ehhez nyissa meg a kívánt erőforrás, és válassza a Tulajdonságok parancsot. Az erőforrás-azonosító az a tulajdonságokat lap jelennek meg az alábbi képernyőképen látható módon:

![ALT "erőforrás-azonosító jelenik meg a Tulajdonságok lap az Azure-portálon"](./media/monitoring-rest-api-walkthrough/resourceid_azure_portal.png)

### <a name="azure-powershell"></a>Azure PowerShell
Az erőforrás-azonosító, valamint az Azure PowerShell-parancsmagok használata tudja visszaszerezni. Az erőforrás-azonosító beszerzése az Azure webes alkalmazásokban, például a Get-AzureRmWebApp parancsmag, ahogy az alábbi képernyőképen végrehajtása:

![ALT "erőforrás-azonosító PowerShell kapott"](./media\monitoring-rest-api-walkthrough\resourceid_powershell.png)

### <a name="azure-cli"></a>Azure CLI
Lekérése az Azure CLI segítségével erőforrás-azonosító, hajtsa végre az "azure webappban megjelenítése" parancsot, adja meg a "– json" beállítás, az alábbi képernyőképen látható módon:

![ALT "erőforrás-azonosító PowerShell kapott"](./media\monitoring-rest-api-walkthrough\resourceid_azurecli.png)

## <a name="retrieve-activity-log-data"></a>Tevékenység naplója adatok beolvasása
Kívül metrikus definíciókat, és a kapcsolódó értékek használata esetén, az is lehetséges Azure erőforrásokkal kapcsolatos további érdekes háttérismeretek beolvasásához. Példaként lekérdezés [tevékenység](https://msdn.microsoft.com/library/azure/dn931934.aspx) naplóadatok lehetőség. A következő példa bemutatja az Azure Monitor REST API-t a tevékenység naplóadatok lekérdezés adott időtartományon belüli használata az Azure előfizetéssel:

```PowerShell
$apiVersion = "2014-04-01"
$filter = "eventTimestamp ge '2016-09-23' and eventTimestamp le '2016-09-24'and eventChannels eq 'Admin, Operation'"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/providers/microsoft.insights/eventtypes/management/values?api-version=${apiVersion}&`$filter=${filter}"
(Invoke-RestMethod -Uri $request `
                   -Headers $authHeader `
                   -Method Get `
                   -Verbose).Value | ConvertTo-Json
```

## <a name="next-steps"></a>Következő lépések
* Tekintse át a [figyelése áttekintése](monitoring-overview.md).
* A [támogatott mértékek Azure monitorral rendelkező](monitoring-supported-metrics.md)megtekintése.
* Tekintse át a [Microsoft Azure Monitor REST API-hivatkozás](https://msdn.microsoft.com/library/azure/dn931943.aspx).
* Tekintse át az [Azure könyvtár](https://msdn.microsoft.com/library/azure/mt417623.aspx).
