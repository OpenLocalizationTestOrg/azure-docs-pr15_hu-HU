<properties
    pageTitle="Az Azure-szolgáltatások értesítések létrehozása a platformok parancssori felület (CLI) segítségével |} Microsoft Azure"
    description="A parancssor segítségével Azure riasztások, válthatnak értesítések és automatizálási, a megadott feltételek teljesülése esetén."
    authors="rboucher"
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
    ms.date="10/24/2016"
    ms.author="robb"/>

# <a name="use-the-cross-platform-command-line-interface-cli-to-create-alerts-for-azure-services"></a>A parancssor (CLI platformok) segítségével az Azure-szolgáltatások értesítések létrehozása

> [AZURE.SELECTOR]
- [Portál](insights-alerts-portal.md)
- [A PowerShell](insights-alerts-powershell.md)
- [CLI](insights-alerts-command-line-interface.md)

## <a name="overview"></a>– Áttekintés

Ez a cikk bemutatja, hogyan állíthat be a parancssor CLI () használata Azure értesítéseket.

>[AZURE.NOTE] Azure monitorra "Azure háttérismeretek" nevezett új nevét, amíg 2016 Sept 25. Azonban a névtér, és így az alábbi parancsok továbbra is tartalmazzák a "háttérismeretek".

Az ellenőrző mértékek vagy események, a Azure szolgáltatások alapján értesítés kaphat.

- **Metrikus értékek** – a riasztási eseményindítók, amikor egy adott mérőszám értékének metszéspontja mindkét irányba hozzárendelése küszöbértéket. Ez azt jelenti, akkor mindkét eseményindítók amikor először teljesül a feltétel és majd utána mikor amely feltétel van már nem teljesül.    
- **Tevékenység naplóbeli események** - értesítés válthatnak *minden* esemény, illetve csak bizonyos számú események fordul elő.

Hajtsa végre a következő, amikor elindítja azt, hogy jelzést adhatja meg:

- a szolgáltatás-rendszergazda, és további rendszergazdák értesítő e-mailek küldése
- e-mailt küldjenek további e-mailek megadott feltételeknek.
- hívja fel a webhook
- Indítsa el az Azure runbook (csak az adott időben Azure portál) végrehajtása

Állítsa be, és riasztási szabályok használatával kapcsolatos tájékozódáshoz

- [Azure portál](insights-alerts-portal.md)
- [A PowerShell](insights-alerts-powershell.md)
- [parancssori kezelőfelületről](insights-alerts-command-line-interface.md)
- [Azure Monitor REST API-val](https://msdn.microsoft.com/library/azure/dn931945.aspx)


Írjon be egy parancs mindig kaphat parancsok súgóját és üzembe – súgó végén. Példa:

    ```console
    azure insights alerts -help
    azure insights alerts actions email create -help
    ```

## <a name="create-alert-rules-using-the-cli"></a>Használja a CLI riasztási szabályok létrehozása

1. Elvégezheti a Előfeltételek, és jelentkezzen be az Azure. Lásd: [Azure Monitor CLI minták](insights-cli-samples.md). Röviden a CLI telepítése, és a parancsok. Azok az első-e jelentkezve, használ, és Felkészülés az Azure Monitor parancsokat futtathat előfizetés megjelenítése.


    ```console
    azure login
    azure account show
    azure config mode arm

    ```

2.  Erőforráscsoport meglévő szabályok listáját, használja a következő képernyőn **riasztások szabály lista azure háttérismeretek** *[kapcsolók] &lt;resourceGroup&gt; *

    ```console
    azure insights alerts rule list myresourcegroupname

    ```
3. Szabály létrehozása több fontos információkat vehet először van szükség.
    - Értesítés beállítása szeretné az erőforrás az **Erőforrás-azonosító**
    - Az adott erőforrás elérhető **metrikus definíciók**

    Az erőforrás-azonosító megszerezni egy úgy, hogy az Azure portálon használja. Feltételezve, hogy az erőforrás már létrehozott, jelölje ki azt a portálon. A Tovább gombra a lap, kattintson a *Tulajdonságok parancsra* a *Beállítások* szakaszban. Az *Erőforrás-azonosító* mező a következő lap. Egy másik úgy, hogy az [Azure erőforrás Explorer](https://resources.azure.com/).

    Egy példa egy webalkalmazás erőforrás-azonosító

    ```console
    /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename
    ```

    A rendelkezésre álló mértékek és erőforrás-mennyiség lista beszerzése az adott az előző példában az erőforrás-mértékek, a következő CLI paranccsal:  

    ```console
    azure insights metrics list /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename PT1M
    ```

    _PT1M_ a rendelkezésre álló mérték (1 perces intervallumok) granularitása. Különböző részletességek használatával különböző metrikus lehetőséget biztosít.


4. Metrikus rendszerű riasztási szabályt szeretne létrehozni, a paranccsal az alábbi képernyő:

    **Azure háttérismeretek riasztások szabály metrikus beállítása** *[kapcsolók] &lt;szabálynév&gt; &lt;hely&gt; &lt;resourceGroup&gt; &lt;ablakméret&gt; &lt;operátor&gt; &lt;küszöb&gt; &lt;targetResourceId&gt; &lt;metricName&gt; &lt;timeAggregationOperator&gt; *

    A következő példa beállítja a webhely erőforrás értesítés. A riasztási eseményindítók, amikor minden forgalom az 5 perc és újra forgalmat beérkezéskor 5 percig egységes kap.

    ```console
    azure insights alerts rule metric set myrule eastus myreasourcegroup PT5M GreaterThan 2 /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename BytesReceived Total

    ```

5. Webhook létrehozásához, és küldjön e-mailek, amikor az értesítés akkor indul el, először létre az e-mailek és/vagy webhooks. A szabály azonnal létrehozása később. Nem lehet rendelni webhook vagy e-mailek, a már létrehozott szabályok használatával a CLI.

    ```console
    azure insights alerts actions email create --customEmails myemail@contoso.com

    azure insights alerts actions webhook create https://www.contoso.com

    azure insights alerts rule metric set myrulewithwebhookandemail eastus myreasourcegroup PT5M GreaterThan 2 /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename BytesReceived Total
    ```


6. Szeretne létrehozni, akkor indul el, amelyek egy bizonyos feltétel a tevékenység naplója, használja az űrlap:

    **háttérismeretek riasztások szabály jelentkezzen beállítása** *[kapcsolók] &lt;szabálynév&gt; &lt;hely&gt; &lt;resourceGroup&gt; &lt;operationName&gt; *

    Ha például

    ```console
    azure insights alerts rule log set myActivityLogRule eastus myresourceGroupName Microsoft.Storage/storageAccounts/listKeys/action
    ```

    A operationName felel meg az esemény típusa, a tevékenység naplója a bejegyzéshez tartozó. Többek között *Microsoft.Compute/virtualMachines/delete* és *microsoft.insights/diagnosticSettings/write*.

    Paranccsal a PowerShell [Get-AzureRmProviderOperation](https://msdn.microsoft.com/library/mt603720.aspx) beszerzése lehetséges operationNames listáját. Az Azure portal segítségével váltakozva, a lekérdezés a tevékenység naplója, és konkrét múltbeli szóló értesítés létrehozása kívánt műveleteket. A műveletek rövid névvel grafikus napló nézetben látható. A JSON a bejegyzés keresni, és kijjebb OperationName értékét.   

7. Ellenőrizheti, hogy az értesítések létrehozott megfelelően megjeleníti az adott szabály.

    ```console
    azure insights alerts rule list myresourcegroup --ruleName myrule
    ```

8. Szabályok törléséhez használja a képernyő egy parancsot:

    **háttérismeretek riasztások szabály törlése** [kapcsolók] &lt;resourceGroup&gt; &lt;szabálynév&gt;

    Ezek a parancsok a korábban létrehozott ebben a cikkben szabályok törlése

    ```console
    azure insights alerts rule delete myresourcegroup myrule
    azure insights alerts rule delete myresourcegroup myrulewithwebhookandemail
    azure insights alerts rule delete myresourcegroup myActivityLogRule
    ```



## <a name="next-steps"></a>Következő lépések

* , [Áttekintést kaphat a Azure figyelése](monitoring-overview.md) többek között a típusú adatok összegyűjtése és figyelése.
* További tudnivalók [az értesítések konfigurálása webhooks](insights-webhooks-alerts.md).
* További tudnivalók az [Azure automatizálási Runbooks](..\automation\automation-starting-a-runbook.md).
* Ismerkedés [a diagnosztikai naplók összegyűjtési áttekintése](monitoring-overview-of-diagnostic-logs.md) részletes magas-gyakoriság mértékek összegyűjtheti az Ön szolgáltatása.
* Ismerkedés a [Mértékek webhelycsoport áttekintése](insights-how-to-customize-monitoring.md) és a szolgáltatás nem érhető el, és válaszol.
