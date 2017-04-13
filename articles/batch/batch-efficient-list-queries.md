<properties
    pageTitle="Azure köteg hatékony lista lekérdezések |} Microsoft Azure"
    description="Teljesítmény növelése érdekében a lekérdezések szűréssel köteg erőforrások, mint a készletek, a feladatok, a feladatok információkat kérésekor, majd a csomópontok számítja ki."
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor="" />

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="big-compute"
    ms.date="10/25/2016"
    ms.author="marsma" />

# <a name="query-the-azure-batch-service-efficiently"></a>A köteg Azure szolgáltatás hatékony lekérdezés

Itt megismerheti az Azure köteg alkalmazás teljesítmény növelése érdekében a szolgáltatás által számítja ki a [Köteget .NET] a csomópontok és feladatok, feladatok, a lekérdezés által visszaadott adatok mennyiségét csökkentésével hogyan[ api_net] tárat.

Szinte minden köteg alkalmazást kell végrehajtani, hogy a köteg szolgáltatás gyakran lekérdezések rendszeres időközönként megfigyelések vagy egyéb művelet bizonyos típusú. Például annak megállapításához, hogy vannak-e bármilyen feladatban hátralévő várólistás feladatok, meg kell adatok beolvasása a projekt minden tevékenység. Határozza meg a készlet található csomópontok állapotát, előbb be kell szereznie a készletben minden csomóponton adatokat. Ez a cikk ismerteti, hogyan a leghatékonyabb mód az ilyen lekérdezések végrehajtásához.

## <a name="meet-the-detaillevel"></a>A DetailLevel felel meg

Egy gyártási köteg alkalmazás, a személyek, például a feladatok, a feladatok és a számítási csomópontok számozhatja a ezer. Ha ezek az erőforrások információkat kér, esetleg nagy mennyiségű adatot kell "kereszt a vezetékes" a köteg szolgáltatásból az alkalmazás minden lekérdezés alapján. Lévő elemek számának és a lekérdezés által visszaadott információtípust korlátozásával növelheti a lekérdezések sebességének, ezért az alkalmazás teljesítményét.

A [Köteg .NET] [ api_net] API-kódtöredéket *minden* tevékenység egy feladat együtt *minden* egyes tevékenységek tulajdonságainak társított sorolja fel:

```csharp
// Get a collection of all of the tasks and all of their properties for job-001
IPagedEnumerable<CloudTask> allTasks =
    batchClient.JobOperations.ListTasks("job-001");
```

Hajthatja végre sokkal hatékonyabban lista lekérdezés, azonban a lekérdezés az "részletességgel" alkalmazásával. Ehhez az [ODATADetailLevel] megadásával[ odata] a [JobOperations.ListTasks] objektum[ net_list_tasks] módot. A kódtöredék adja eredményül, csak a azonosítója, a parancssor és a számítási csomópont információk tulajdonságok a befejezett tevékenységek:

```csharp
// Configure an ODATADetailLevel specifying a subset of tasks and
// their properties to return
ODATADetailLevel detailLevel = new ODATADetailLevel();
detailLevel.FilterClause = "state eq 'completed'";
detailLevel.SelectClause = "id,commandLine,nodeInfo";

// Supply the ODATADetailLevel to the ListTasks method
IPagedEnumerable<CloudTask> completedTasks =
    batchClient.JobOperations.ListTasks("job-001", detailLevel);
```

Példa ebben az esetben, ha a feladat, ezer a második lekérdezés eredményei általában visszatér sokkal gyorsabb, mint az első. A köteg .NET API-val listatételeket, amikor ODATADetailLevel használatáról további információt tartalmaz [alatt](#efficient-querying-in-batch-net).

> [AZURE.IMPORTANT]
> Azt javasoljuk, hogy *mindig* adja meg annak biztosítása érdekében a leghatékonyabban és az alkalmazás teljesítményének lista .NET API hívásait ODATADetailLevel objektum. A részletek szint megadásával segítséget nyújthat köteg szolgáltatás válaszidő alsó, javítható a hálózati kihasználtsági és összezárása memóriahasználat ügyfélalkalmazások.

## <a name="filter-select-and-expand"></a>Szűrés, jelölje ki és kibontása

A [Köteg .NET] [ api_net] és a [Többi köteg] [ api_rest] API-hoz, adja meg az azt jelenti, hogy mindkét listában a visszaadott elemek számát, és mennyi információ minden egyes visszaküldött csökkentése. Ehhez adja meg a **szűrő**, **Jelölje ki**, és **bontsa ki a karakterláncok** lista lekérdezések végrehajtásakor.

### <a name="filter"></a>Szűrő
A szűrő karakterlánca egy kifejezés, amely csökkenti a visszaadott elemek számát. Például csak egy feladat futó feladatok lista, vagy csak a feladatok futtatásához készen állnak-számítási csomópontok listája.

- A szűrő karakterlánc áll egy vagy több kifejezések, egy kifejezés, amely a tulajdonság neve, operátorok és érték áll. Adható meg a tulajdonságok minden személy típus, hogy a lekérdezés, egyes érhető el, amelyek az egyes tulajdonság támogatott operátorokat.
- Több kifejezések kombinálhatók által használt logikai operátorok `and` és `or`.
- Ez a példa szűrőlisták karakterlánc csak a haladási "jeleníti meg a" tevékenységek: `(state eq 'running') and startswith(id, 'renderTask')`.

### <a name="select"></a>Válassza a
A select karakterlánc, amely az egyes elemek szerepeljenek eredményként tulajdonságértékeit korlátozza. Tulajdonság nevek listáját adja, és csak azokat a tulajdonság az értékeket a lekérdezés eredményében a elemek szerepeljenek eredményként.

- A select karakterláncot, ahol a tulajdonság nevek egy vesszővel elválasztott listája. A személy típusú lekérdezett tulajdonságainak is megadhat.
- Ez a példa választó karakterlánc, adja meg, hogy az egyes tevékenységek csak három tulajdonságértékeket visszaadandó: `id, state, stateTransitionTime`.

### <a name="expand"></a>Bontsa ki a
A kibontás karakterlánc csökkenti a számot az egyes információkhoz juthat szükséges API-hívások. Kibontás karakterláncra használatakor egyes elemekkel kapcsolatban további információt szerezhető be a egyetlen API-hívást. Helyett beszerzése a személyek, majd a minden elem listájában információkat kér listáját használatával Kibontás karakterláncra beszerzése egyetlen API-hívás ugyanazokat az adatokat. Kevesebb API hívásokat azt jelenti, hogy a jobb teljesítmény.

- Hasonlóan, mint a választó karakterláncot, a kibontás karakterlánc szabályozza, hogy bizonyos adatok lista lekérdezés eredményében szerepel-e.
- A kibontás karakterlánc csak akkor használható, a feladatok, a projekt ütemezését, a feladatok és a készletek bejegyzésére alkalmazásakor. Jelenleg csak a támogatott statisztikai adatok.
- Ha minden tulajdonság szükség, és nincs választó karakterlánc megadva, a kibontás karakterlánc *kell* használni statisztika információ. Ha juthat hozzá egy részét tulajdonságok, majd válassza a karakterlánc használja `stats` is kell megadni a választó karakterláncot, és a kibontás karakterlánc nem kell megadni.
- Ez a példa bontsa ki a karakterlánc Megadja, hogy a statisztikai adatok vissza kell-e a lista minden eleménél: `stats`.

> [AZURE.NOTE] A három lekérdezési karakterlánc típusú kiszámításakor (szűrés, jelölje ki, és bontsa ki a), győződjön meg arról, hogy a tulajdonság neve és a eset megegyezzen a REST API-elem mint. Például a.NET [CloudTask](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask) osztály-használatakor meg kell adnia **állam** helyett **állapot**, annak ellenére, hogy a .NET tulajdonság [CloudTask.State](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.state). Tulajdonságok megfeleltetéseinek a .NET rendszerhez és a REST API-khoz között az alábbi táblázatokban talál.

### <a name="rules-for-filter-select-and-expand-strings"></a>Szabályok szűrő, jelölje ki, és bontsa ki a karakterláncok

- Tulajdonságok nevét a szűrő, jelölje ki, és bontsa ki a karakterláncok jelenjen meg, mint a [Többi köteg] [ api_rest] API – akkor is, ha a [Köteg .NET] használt[ api_net] vagy köteg SDK csomagok egyikére.
- Minden tulajdonság név-és nagybetűk, de tulajdonságértékeket nagybetűk között.
- Dátum/idő karakterláncok kétféleképp lehet és előtt szerepelnie kell a `DateTime`.

  - Példa a W3C-DTF formázása:`creationTime gt DateTime'2011-05-08T08:49:37Z'`
  - Példa a RFC 1123 formázása:`creationTime gt DateTime'Sun, 08 May 2011 08:49:37 GMT'`
- Logikai karakterláncokat vagy `true` vagy `false`.
- Ha egy érvénytelen tulajdonság vagy operátorral van megadva, a `400 (Bad Request)` hibát okoz.

## <a name="efficient-querying-in-batch-net"></a>Hatékony köteg .NET lekérdezése

A [Köteg .NET] belül[ api_net] API-t, a [ODATADetailLevel] [ odata] osztály használható szűrő megadásával, jelölje ki, és bontsa ki a karakterláncok műveletek. A ODataDetailLevel osztály három nyilvános karakterlánc tulajdonságaik vannak, amelyek a konstruktor megadott, vagy állítsa közvetlenül az objektum van. Ezután adja meg a ODataDetailLevel objektum paraméterként a különféle műveletek például [ListPools][net_list_pools], [ListJobs][net_list_jobs], és [ListTasks][net_list_tasks].

- [ODATADetailLevel][odata]. [FilterClause] [ odata_filter]: A visszaadott elemek számának korlátozása.
- [ODATADetailLevel][odata]. [SelectClause] [ odata_select]: Adja meg, hogy mely tulajdonság értékeit az egyes elemek szerepeljenek eredményként.
- [ODATADetailLevel][odata]. [ExpandClause] [ odata_expand]: Adatok beolvasása egy API összes eleme hívás helyett külön hívások mindegyik elemhez.

A következő kódrészletet hatékony lekérdezés készletek meghatározott statisztika a köteg szolgáltatást a köteg .NET API-t használja. Ebben az esetben a köteg felhasználónak próba és a gyártási készletek. A próba-készletre azonosítók "próbához" elé, és a termelési gyűjtő azonosítók a "termékkatalógus" elé. Kattintson a kódtöredék *myBatchClient* -e a [BatchClient](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient) osztály megfelelően inicializált példány.

```csharp
// First we need an ODATADetailLevel instance on which to set the filter, select,
// and expand clause strings
ODATADetailLevel detailLevel = new ODATADetailLevel();

// We want to pull only the "test" pools, so we limit the number of items returned
// by using a FilterClause and specifying that the pool IDs must start with "test"
detailLevel.FilterClause = "startswith(id, 'test')";

// To further limit the data that crosses the wire, configure the SelectClause to
// limit the properties that are returned on each CloudPool object to only
// CloudPool.Id and CloudPool.Statistics
detailLevel.SelectClause = "id, stats";

// Specify the ExpandClause so that the .NET API pulls the statistics for the
// CloudPools in a single underlying REST API call. Note that we use the pool's
// REST API element name "stats" here as opposed to "Statistics" as it appears in
// the .NET API (CloudPool.Statistics)
detailLevel.ExpandClause = "stats";

// Now get our collection of pools, minimizing the amount of data that is returned
// by specifying the detail level that we configured above
List<CloudPool> testPools =
    await myBatchClient.PoolOperations.ListPools(detailLevel).ToListAsync();
```

> [AZURE.TIP] Egy példányának [ODATADetailLevel] [ odata] jelölje ki a konfigurált és kibontás záradékok is a megfelelő Get módszerek, például [PoolOperations.GetPool](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.getpool.aspx), a függvény által visszaadott adatok mennyiségét korlátozni átadandó.

## <a name="batch-rest-to-net-api-mappings"></a>A köteg többi .NET API rendelése

Tulajdonságok neve a szűrőben, jelölje ki, és bontsa ki a karakterláncok a REST API mint, mind a név és eset *kell* megfelelően. Az alábbi táblázat a REST API-val és a .NET rendszerhez készült közötti hozzárendelés biztosítanak.

### <a name="mappings-for-filter-strings"></a>Hozzárendelések szűrő karakterláncok

- **.NET-lista módszerek**: Ebben az oszlopban a .NET API módszer fogad egy [ODATADetailLevel] [ odata] paraméter az objektumot.
- **Többi lista kérések**: Ebben az oszlopban csatolva REST API-t minden egyes lap egy olyan táblát, adja meg a tulajdonságok és engedélyezett műveletek *szűrő* karakterláncok tartalmazza. A tulajdonságok neve és a műveletek fog használnia, ha egy [ODATADetailLevel.FilterClause] állítja össze[ odata_filter] karakterlánc.

| .NET-lista módszerek | TÖBBI lista kérések |
|---|---|
| [CertificateOperations.ListCertificates][net_list_certs] | [A fiók tanúsítványok lista][rest_list_certs]
| [CloudTask.ListNodeFiles][net_list_task_files] | [A fájlok egy tevékenységhez kapcsolódó lista][rest_list_task_files]
| [JobOperations.ListJobPreparationAndReleaseTaskStatus][net_list_jobprep_status] | [A projekt előkészítése és megjelenés projektfeladatok feladat állapotának lista][rest_list_jobprep_status]
| [JobOperations.ListJobs][net_list_jobs] | [A fiók a feladatok lista][rest_list_jobs]
| [JobOperations.ListNodeFiles][net_list_nodefiles] | [A csomóponton fájlok listája][rest_list_nodefiles]
| [JobOperations.ListTasks][net_list_tasks] | [A feladat társított feladatok listája][rest_list_tasks]
| [JobScheduleOperations.ListJobSchedules][net_list_job_schedules] | [A feladat ütemtervek a fiók lista][rest_list_job_schedules]
| [JobScheduleOperations.ListJobs][net_list_schedule_jobs] | [A feladat ütemezése társított feladatok listája][rest_list_schedule_jobs]
| [PoolOperations.ListComputeNodes][net_list_compute_nodes] | [A számítási csomópontok a készlet listában][rest_list_compute_nodes]
| [PoolOperations.ListPools][net_list_pools] | [A fiók a készletek lista][rest_list_pools]

### <a name="mappings-for-select-strings"></a>Hozzárendelések választó karakterláncok

- **Köteg .NET típusokat**: köteg .NET API-típusokat.
- **REST API-szervezetek**: Ebben az oszlopban minden lap tartalmaz egy vagy több táblát, amely a REST API-tulajdonságok neve a típus listában. A tulajdonság nevek *Válassza a* karakterláncok állítja össze használnak. A tulajdonság nevű fogja használni, amikor egy [ODATADetailLevel.SelectClause] állítja össze[ odata_select] karakterlánc.

| Köteg .NET típusokat | REST API-szervezetek |
|---|---|
| [Tanúsítvány][net_cert] | [Tanúsítvány adatainak megtekintése][rest_get_cert] |
| [CloudJob][net_job] | [A projekt adatainak megtekintése][rest_get_job] |
| [CloudJobSchedule][net_schedule] | [Egy projekt ütemezése adatainak megtekintése][rest_get_schedule] |
| [ComputeNode][net_node] | [A csomópont adatainak megtekintése][rest_get_node] |
| [CloudPool][net_pool] | [Egy készlet adatainak megtekintése][rest_get_pool] |
| [CloudTask][net_task] | [A tevékenység adatainak megtekintése][rest_get_task] |

## <a name="example-construct-a-filter-string"></a>Példa: Egyenletszerkesztővel szűrési karakterlánc.

Amikor az [ODATADetailLevel.FilterClause]szűrési karakterlánc Egyenletszerkesztővel[odata_filter], olvassa el a "Hozzárendelések szűrő karakterláncok" a fenti táblázatban, amely megfelel a lista művelet végrehajtása használni kívánt keresési REST API-val dokumentáció lapra. A szűrhető tulajdonságok és a támogatott operátorok azon az oldalon található első multirow táblázatban találja. Ha minden tevékenység amelynek kilépési kódot lett nullától különböző, például [a feladathoz kapcsolódó feladatok lista] a sor beolvasásához kívánja[ rest_list_tasks] alkalmazandó tulajdonság karakterlánc és megengedett operátorok határozza meg:

| A tulajdonság | Engedélyezett műveletek | Típus |
| :--- | :--- | :--- |
| `executionInfo/exitCode` | `eq, ge, gt, le , lt` | `Int` |

Ezért a szűrő karakterlánc összes tevékenység nullától különböző hibakóddal azoknak a következő lesz:

`(executionInfo/exitCode lt 0) or (executionInfo/exitCode gt 0)`

## <a name="example-construct-a-select-string"></a>Példa: válassza a karakterlánc létrehozása

Az Egyenletszerkesztővel [ODATADetailLevel.SelectClause][odata_select], olvassa el a "Select karakterláncok hozzárendelések" a fenti táblázatban, és nyissa meg a REST API-lapot, a rendszer bejegyzésére entitás adattípusra. A választható tulajdonságok és a támogatott operátorok azon az oldalon található első multirow táblázatban találja. Ha csak az azonosító és a parancssorban az egyes tevékenységek lista beolvasásához kívánja, például található meg ezeket a sorokat a megfelelő tábla a [tevékenység adatainak][rest_get_task]:

| A tulajdonság | Típus | Jegyzetek |
| :--- | :--- | :--- |
| `id` | `String` | `The ID of the task.` |
| `commandLine` | `String` | `The command line of the task.` |

A select karakterláncban, csak az azonosító és a parancssorban az egyes listában szereplő feladat lesz:

`id, commandLine`

## <a name="code-samples"></a>Mintakódok

### <a name="efficient-list-queries-code-sample"></a>Hatékony lista lekérdezések kód minta

Nézze meg a [EfficientListQueries] [ efficient_query_sample] GitHub, hogy hogyan hatékony lista lekérdezése minta projekt hatással lehet a teljesítmény az alkalmazásokban. C# konzolon alkalmazás hoz létre, és a feladatok nagyszámú ad hozzá egy feladatot. Ezután a [JobOperations.ListTasks] több hívásainak van[ net_list_tasks] módot és fázisok [ODATADetailLevel] [ odata] objektumok, amelyek eltérő értékű visszaadott adatmennyiség változhat vannak beállítva. Az alábbihoz hasonló eredményt ad elő:

```
Adding 5000 tasks to job jobEffQuery...
5000 tasks added in 00:00:47.3467587, hit ENTER to query tasks...

4943 tasks retrieved in 00:00:04.3408081 (ExpandClause:  | FilterClause: state eq 'active' | SelectClause: id,state)
0 tasks retrieved in 00:00:00.2662920 (ExpandClause:  | FilterClause: state eq 'running' | SelectClause: id,state)
59 tasks retrieved in 00:00:00.3337760 (ExpandClause:  | FilterClause: state eq 'completed' | SelectClause: id,state)
5000 tasks retrieved in 00:00:04.1429881 (ExpandClause:  | FilterClause:  | SelectClause: id,state)
5000 tasks retrieved in 00:00:15.1016127 (ExpandClause:  | FilterClause:  | SelectClause: id,state,environmentSettings)
5000 tasks retrieved in 00:00:17.0548145 (ExpandClause: stats | FilterClause:  | SelectClause: )

Sample complete, hit ENTER to continue...
```

Ahogy a eltelt idő, nagyban csökkentheti lekérdezés válaszidő korlátozza a tulajdonságok és a visszaadott elemek számát. Ez, és más minta projektek megtalálhatja [a minták köteg azure] [ github_samples] GitHub tárházából.

### <a name="batchmetrics-library-and-code-sample"></a>Tár- és minta BatchMetrics

Az EfficientListQueries kód mintán kívül a fenti megtalálhatja a [BatchMetrics] [ batch_metrics] project [a minták köteg azure] [ github_samples] GitHub tárházba. A BatchMetrics minta projekt bemutatja, hogyan dolgozhat hatékonyabban a Lync-Azure köteg feladat előrehaladását a köteg API.

A [BatchMetrics] [ batch_metrics] minta .NET osztály tár projekt részévé saját projektek és egyszerű parancssori bemutatják a tár használatát, és a program, amely tartalmazza.

A projektben a minta alkalmazás mutatja be, a következő műveleteket:

1. Adott jellemzők kijelölése annak érdekében, hogy csak a szükséges tulajdonságokat letöltése
2. Szűrés állapot áttűnés időpontok annak érdekében, hogy az utolsó lekérdezés óta csak változások letöltése

Például az alábbi módon jelenik meg, a BatchMetrics tárban. Ad vissza, amely meghatározza, hogy csak egy ODATADetailLevel a `id` és `state` tulajdonságok kell beszerezni a rendszer megkérdezi, hogy a személyek számára. Azt is, hogy csak azok a személyek állapotú megváltozott a meghatározott óta `DateTime` paraméter vissza kell.

```csharp
internal static ODATADetailLevel OnlyChangedAfter(DateTime time)
{
    return new ODATADetailLevel(
        selectClause: "id, state",
        filterClause: string.Format("stateTransitionTime gt DateTime'{0:o}'", time)
    );
}
```

## <a name="next-steps"></a>Következő lépések

### <a name="parallel-node-tasks"></a>A párhuzamos csomópontjának tevékenységek

[Azure köteg maximalizálása kiszámítania egyidejű csomópontjának tevékenységek erőforrás-kihasználtság](batch-parallel-node-tasks.md) egy másik köteg alkalmazás teljesítménnyel kapcsolatos. Bizonyos típusú munkaterhelésekből előnyeivel párhuzamos feladatok végrehajtása a nagyobb – de kevesebb – a csomópontok számítja ki. Nézze meg a [Példa forgatókönyv](batch-parallel-node-tasks.md#example-scenario) a cikkben egy ilyen esetben részletesen.

### <a name="batch-forum"></a>Köteg-fórum

Az [Azure köteg fórum] [ forum] MSDN webhelyen található kiváló hely az megvitatása a köteget, és a szolgáltatással kapcsolatos kérdéseket. Központi a fölé a hasznos "öntapadó" bejegyzéseket, és a, amíg a köteg megoldások generál merülnek fel.


[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_listjobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_metrics]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchMetrics
[efficient_query_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/EfficientListQueries
[forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[github_samples]: https://github.com/Azure/azure-batch-samples
[odata]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.aspx
[odata_ctor]: https://msdn.microsoft.com/library/azure/dn866178.aspx
[odata_expand]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.expandclause.aspx
[odata_filter]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.filterclause.aspx
[odata_select]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.selectclause.aspx

[net_list_certs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificateoperations.listcertificates.aspx
[net_list_compute_nodes]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listcomputenodes.aspx
[net_list_job_schedules]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobschedules.aspx
[net_list_jobprep_status]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobpreparationandreleasetaskstatus.aspx
[net_list_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[net_list_nodefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listnodefiles.aspx
[net_list_pools]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listpools.aspx
[net_list_schedule_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobs.aspx
[net_list_task_files]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listnodefiles.aspx
[net_list_tasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listtasks.aspx

[rest_list_certs]: https://msdn.microsoft.com/library/azure/dn820154.aspx
[rest_list_compute_nodes]: https://msdn.microsoft.com/library/azure/dn820159.aspx
[rest_list_job_schedules]: https://msdn.microsoft.com/library/azure/mt282174.aspx
[rest_list_jobprep_status]: https://msdn.microsoft.com/library/azure/mt282170.aspx
[rest_list_jobs]: https://msdn.microsoft.com/library/azure/dn820117.aspx
[rest_list_nodefiles]: https://msdn.microsoft.com/library/azure/dn820151.aspx
[rest_list_pools]: https://msdn.microsoft.com/library/azure/dn820101.aspx
[rest_list_schedule_jobs]: https://msdn.microsoft.com/library/azure/mt282169.aspx
[rest_list_task_files]: https://msdn.microsoft.com/library/azure/dn820142.aspx
[rest_list_tasks]: https://msdn.microsoft.com/library/azure/dn820187.aspx

[rest_get_cert]: https://msdn.microsoft.com/library/azure/dn820176.aspx
[rest_get_job]: https://msdn.microsoft.com/library/azure/dn820106.aspx
[rest_get_node]: https://msdn.microsoft.com/library/azure/dn820168.aspx
[rest_get_pool]: https://msdn.microsoft.com/library/azure/dn820165.aspx
[rest_get_schedule]: https://msdn.microsoft.com/library/azure/mt282171.aspx
[rest_get_task]: https://msdn.microsoft.com/library/azure/dn820133.aspx

[net_cert]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificate.aspx
[net_job]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_schedule]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjobschedule.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
