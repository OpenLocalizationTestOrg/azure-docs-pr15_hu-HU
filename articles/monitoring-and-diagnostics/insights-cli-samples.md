<properties
    pageTitle="Rövid útmutató az Azure Monitor CLI minták. | Microsoft Azure"
    description="Minta CLI parancsok az Azure Monitor szolgáltatásaival. Azure Monitor egy olyan Microsoft Azure szolgáltatás, amely lehetővé teszi, hogy értesítéseket küldeni, hívja a webes URL-címek konfigurált telemetriai adatokat, és az Automatikus méretezéssel Cloud Services, a virtuális gépeken futó és a Web Apps alkalmazások értékek alapján."
    authors="kamathashwin"
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
    ms.date="09/08/2016"
    ms.author="ashwink"/>

# <a name="azure-monitor--cross-platform-cli-quick-start-samples"></a>Azure Monitor platformok CLI rövid útmutató az első minták

Ebből a cikkből megtudhatja, minta parancssori kezelőfelületről parancs segítségével elérheti az Azure Monitor funkciók. Azure Monitor teszi Automatikus méretezéssel Felhőszolgáltatások, virtuális gépeken futó és Web Apps alkalmazások és értesítéseket küldeni, vagy felhívhatja őket webes URL-címek értékek konfigurált telemetriai adatok alapján.

>[AZURE.NOTE] Azure monitorra "Azure háttérismeretek" nevezett új nevét, amíg 2016 Sept 25. Azonban a névtér, és így az alábbi parancsok továbbra is tartalmazzák a "háttérismeretek".


## <a name="prerequisites"></a>Előfeltételek

Ha még nem telepítette az Azure CLI, olvassa el a [telepítse az Azure CLI](../xplat-cli-install.md)című témakört. Ha nem tudja pontosan, az Azure CLI, erről további használatkor [Az Azure CLI for Mac, Linux, és a Windows Azure erőforrás-kezelővel](../xplat-cli-azure-resource-manager.md)vele.


A Windows rendszerben telepítse a [Node.js webhely](https://nodejs.org/)npm. A Futtatás rendszergazdaként jogosultságokkal CMD.exe használja a telepítés befejezése után hajtsa végre a következő a mappából, amelyen telepítve van a npm:

```console
npm install azure-cli --global
```

Ezután nyissa meg bármelyik/helyet a mappának szeretné, és írja be a parancssorban:

```console
azure help
```

## <a name="log-in-to-azure"></a>Bejelentkezés az Azure

Az első lépésként jelentkezzen be az Azure-fiókjába.

```console
azure login
```

A parancs használata után kell bejelentkezni a képernyőn megjelenő utasításokat. Ezután megjelenik a fiókot, TenantId és alapértelmezett előfizetés azonosítójával. Minden parancs használata az alapértelmezett előfizetés keretében.

A jelenlegi előfizetését a részletek listában használja a következő parancsot.

```console
azure account show
```

Másik előfizetést munka környezet módosításához használja a következő parancsot.

```console
azure account set "subscription ID or subscription name"
```

Erőforrás-kezelő Azure és Azure Monitor parancsok használata, kell lennie Azure erőforrás-kezelő módban.

```console
azure config mode arm
```

Az összes támogatott Azure Monitor parancsok listájának megtekintéséhez tegye a következőket.

```console
azure insights
```

## <a name="view-audit-logs-for-a-subscription"></a>Nézet naplókat előfizetéshez

Naplókat listájának megtekintéséhez tegye a következőket.

```console
azure insights logs list [options]
```

Hajtsa végre az alábbi az összes rendelkezésre álló beállítások megtekintése.

```console
azure insights logs list -help
```

Íme egy példa szeretne egy resourceGroup lista naplók

```console
azure insights logs list --resourceGroup "myrg1"
```

Példa a hívó lista naplók

```console
azure insights logs list --caller "myname@company.com"
```

Példa egy erőforrás típusa, a kezdő és befejező dátumát belül a hívó lista naplók

```console
azure insights logs list --resourceProvider "Microsoft.Web" --caller "myname@company.com" --startTime 2016-03-08T00:00:00Z --endTime 2016-03-16T00:00:00Z
```

## <a name="work-with-alerts"></a>Értesítések kezelése
A szakasz a információk segítségével értesítések kezelése.

### <a name="get-alert-rules-in-a-resource-group"></a>A figyelmeztetési szabályok egyben az erőforráscsoport

```console
azure insights alerts rule list abhingrgtest123
azure insights alerts rule list abhingrgtest123 --ruleName andy0323
```

### <a name="create-a-metric-alert-rule"></a>Metrikus riasztási szabály létrehozása

```console
azure insights alerts actions email create --customEmails foo@microsoft.com
azure insights alerts actions webhook create https://someuri.com
azure insights alerts rule metric set andy0323 eastus abhingrgtest123 PT5M GreaterThan 2 /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Web-EastUS/providers/Microsoft.Web/serverfarms/Default1 BytesReceived Total
```

### <a name="create-a-log-alert-rule"></a>Log riasztási szabály létrehozása

```console
azure insights alerts rule log set ruleName eastus resourceGroupName someOperationName
```

### <a name="create-webtest-alert-rule"></a>Webtest riasztási szabály létrehozása

```console
azure insights alerts rule webtest set leowebtestr1-webtestr1 eastus Default-Web-WestUS PT5M 1 GSMT_AvRaw /subscriptions/b67f7fec-69fc-4974-9099-a26bd6ffeda3/resourcegroups/Default-Web-WestUS/providers/microsoft.insights/webtests/leowebtestr1-webtestr1
```

### <a name="delete-an-alert-rule"></a>Egy szabályt törlése

```console
azure insights alerts rule delete abhingrgtest123 andy0323
```

## <a name="log-profiles"></a>Log profilok
Ebben a szakaszban szereplő információk segítségével napló profilok kezelése.

### <a name="get-a-log-profile"></a>A napló profilok beszerzése

```console
azure insights logprofile list
azure insights logprofile get -n default
```


### <a name="add-a-log-profile-without-retention"></a>Adatmegőrzési nélkül napló profil hozzáadása

```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia
```

### <a name="remove-a-log-profile"></a>Log profil eltávolítása

```console
azure insights logprofile delete --name default
```

### <a name="add-a-log-profile-with-retention"></a>Az adatmegőrzési napló profil hozzáadása

```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia --retentionInDays 90
```

### <a name="add-a-log-profile-with-retention-and-eventhub"></a>Adatmegőrzési és EventHub napló profil hozzáadása

```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --serviceBusRuleId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/testshoeboxeastus/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia --retentionInDays 90
```


## <a name="diagnostics"></a>Diagnosztikai
Ebben a szakaszban szereplő információk segítségével diagnosztikai beállítások használata.

### <a name="get-a-diagnostic-setting"></a>Ismerkedés a diagnosztikai beállítása

```console
azure insights diagnostic get --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp
```

### <a name="disable-a-diagnostic-setting"></a>Tiltsa le a diagnosztikai beállítása

```console
azure insights diagnostic set --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp --storageId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/shibanitesting --enabled false
```

### <a name="enable-a-diagnostic-setting-without-retention"></a>A diagnosztikai beállítása nélkül adatmegőrzés engedélyezése

```console
azure insights diagnostic set --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp --storageId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/shibanitesting --enabled true
```


## <a name="autoscale"></a>Automatikus méretezéssel
Ebben a szakaszban szereplő információk segítségével Automatikus méretezéssel beállítások használata. Ezek a példák módosítania.

### <a name="get-autoscale-settings-for-a-resource-group"></a>Erőforráscsoport Automatikus méretezéssel beállítások

```console
azure insights autoscale setting list montest2
```

### <a name="get-autoscale-settings-by-name-in-a-resource-group"></a>Automatikus méretezéssel beállítások egyben az erőforráscsoport név szerint

```console
azure insights autoscale setting list montest2 -n setting2
```


### <a name="set-auotoscale-settings"></a>Auotoscale beállításainak megadása

```console
azure insights autoscale setting set montest2 -n setting2 --settingSpec
```
