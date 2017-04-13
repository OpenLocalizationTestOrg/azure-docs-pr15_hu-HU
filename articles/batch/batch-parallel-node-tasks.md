<properties
    pageTitle="Teljes méretűre állíthatja a köteg csomópont programmal párhuzamos feladatok |} Microsoft Azure"
    description="Hatékonyság és alsó költségek növelése az Azure köteg készletben minden csomóponton kevesebb számítási csomópontokat és futó egyidejű feladatok használatával"
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

# <a name="maximize-azure-batch-compute-resource-usage-with-concurrent-node-tasks"></a>Azure köteg számítási erőforrás-kihasználtság egyidejű csomópontot a tevékenységek teljes méretre

Futtatásával egynél több tevékenység egyidejű minden számítási csomóponton az Azure köteg készletben, erőforrás-kihasználtság meg a készlet csomópontok kisebb számot teljes méretre állítása Az egyes munkaterhelésekből ez eredményezhet rövidebb feladat idővel és az alsó költség.

Bizonyos esetekben összekapcsolhatók hogy minden olyan csomópont erőforrás egy tevékenységhez, miközben több helyzetekben előnyei ezek az erőforrások megosztása több tevékenység engedélyezése:

 - Ha feladatok nem tudja megosztani az adatokat, a **kis méretre adatátvitel** . Ebben az esetben jelentősen csökkentheti adatok átadás díjak által megosztott adatok másolása egy kisebb csomópontok számának és a feladatok végrehajtása minden csomóponton párhuzamosan. Ez akkor különösen igaz, ha minden csomópont kell másolni az adatokat át kell földrajzi régiók között.

 - **Maximizing memóriahasználat** amikor feladatokhoz nagy mennyiségű memóriát, de csak időszakokban rövid idő, valamint a végrehajtás során változó időpontokban szükséges. Hatékonyan kezelheti az ilyen kiugrásainak megfelelő több memóriát kevesebb, de a nagyobb, számítási csomópontokat is alkalmazhat. A csomópontok szeretné, hogy minden csomóponton párhuzamosan futó több tevékenység, de egyes tevékenységek volna kihasználhatja a csomópontok bőséges memória különböző időpontokban.

 - A **csomópont szám korlátai enyhítő** esedékességekor csomópont közötti kommunikációt belül erőforráskészlethez tartozik. Be van állítva a csomópont közötti kommunikációt készletek jelenleg legfeljebb 50 számítási csomópontot. Ha minden csomópont egy készletben feladatok indíthatnak párhuzamosan, nagyobb számú, a feladatok egyszerre futtatható.

 - **Egy helyszíni replikálása kiszámítania fürt**, például amikor először áthelyez egy számítási környezet Azure. Ha a jelenlegi helyszíni megoldás több tevékenység egyes számítási csomópontok hajt végre, növelésével csomópontjának tevékenységek jobban tükrözze a konfigurációs maximális száma.

## <a name="example-scenario"></a>Példa eset

A párhuzamos feladat-végrehajtás előnyei bemutatásához példaként tegyük fel, hogy a tevékenység alkalmazás Processzor- és követelményei vannak-e, hogy az [szabványos\_D1](../cloud-services/cloud-services-sizes-specs.md#general-purpose-d) csomópontokat is elegendő. De annak érdekében, hogy a feladat befejezéséhez szükséges idő a csomópontok 1000 van szükség.

Normál használata helyett\_D1 csomópontokat 1 Processzor core rendelkező, használhatja is [szabványos\_D14](../cloud-services/cloud-services-sizes-specs.md#memory-intensive-d) csomópontokat, amelyre 16 magmintákat, és a párhuzamos feladat-végrehajtás engedélyezése. Ezért *16 időpontok kevesebb csomópontok* felhasználható – 1000 csomópontok helyett, csak 63 lenne szükség. Ezenkívül minden csomópont alkalmazás nagyméretű fájlok vagy hivatkozási adatok szükségesek, ha feladat-időtartam és hatékonyság újra növelése az adatokat másolja az alkalmazás csak 16 csomópontok óta.

## <a name="enable-parallel-task-execution"></a>A párhuzamos feladat-végrehajtás engedélyezése

A párhuzamos feladat-végrehajtás számítási csomópontok készlet szintre állítsa be. A köteg .NET-tárral beállítása a [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] tulajdonság, amikor a készlet létrehozása. Ha a köteg REST API-t használ, adja meg a [maxTasksPerNode] [ rest_addpool] elem készlet létrehozása során összehívás törzsébe.

Azure köteg lehetővé teszi, hogy meg a maximális feladatok egyes csomópontok legfeljebb négyszer (4 x) csomópontot magmintákat számát. Például ha be van állítva a készlet csomópontokkal a méretezés "Nagy" (négy egyes) majd `maxTasksPerNode` kétféleképpen állítható be 16. Az egyes csomópont méretű magmintákat számú részletekért olvassa el [Cloud Services méretét](../cloud-services/cloud-services-sizes-specs.md). Szolgáltatás korlátai kapcsolatos további tudnivalókért olvassa el a [kvótáinak és a köteg Azure szolgáltatás korlátozások](batch-quota-limit.md)című témakört.

> [AZURE.TIP] Ügyeljen arra, figyelembe kell vennie a `maxTasksPerNode` értéke, ha egy [képlet Automatikus méretezéssel] állítja össze[ enable_autoscaling] a készlet. Az eredmény képlet például `$RunningTasks` jelentősen befolyásolhatja a tevékenység / csomópont növelésével. [Automatikus méretezés kiszámítása a csomópontok az Azure köteg készletben](batch-automatic-scaling.md) talál további információt.

## <a name="distribution-of-tasks"></a>A tevékenységek ki.

Egy készlet számítási csomópontját egyidejűleg feladatok hajtsa végre, amikor célszerű adja meg, hogy a feladatokat a készlet csomópontját legyen elosztva.

A [CloudPool.TaskSchedulingPolicy] használatával[ task_schedule] tulajdonság, megadhatja, hogy feladatokat ki kell osztani egyenletesen minden csomópontra a készletben ("terjesztése"). Vagy megadhatja, hogy minél több feladatokat a lehető ki kell osztani minden csomópontra tevékenységek vannak rendelve egy másik csomópont ("csomagolást") készletben előtt.

Példa hogyan Ez a funkció akkor hasznos, fontolja meg a készlet [szabványos\_D14](../cloud-services/cloud-services-sizes-specs.md#memory-intensive-d) csomópontokat (a példában a fenti) be van állítva egy [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] 16-os értéket. Ha a [CloudPool.TaskSchedulingPolicy] [ task_schedule] van beállítva egy [ComputeNodeFillType] [ fill_type] *csomag*, azt, akinek maximalizálása minden csomópont összes 16 mag használatát és engedélyezése egy [autoscaling készlet](batch-automatic-scaling.md) metszi még nem használt csomópontok a készletből (csomópontok bármely kiosztott feladatok nélkül). Ez a kis méretűre állítása az Erőforrás kihasználtsága és pénzt menti.

## <a name="batch-net-example"></a>Példa a köteg .NET

A [Köteg .NET] [ api_net] API kódrészletet jeleníti meg, amely tartalmazza az egyes csomópontok négy feladatok legfeljebb négy nagy csomópontok készlet létrehozása a kérést. Azt adja meg a tevékenység ütemezése házirendet, amely minden csomópontjának tevékenységek előtt a feladat kiosztása egy másik csomópontra a készletben beírja. A köteg .NET API készletek hozzáadása a további tudnivalókért lásd: [BatchClient.PoolOperations.CreatePool][poolcreate_net].

```csharp
CloudPool pool =
    batchClient.PoolOperations.CreatePool(
        poolId: "mypool",
        targetDedicated: 4
        virtualMachineSize: "large",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));

pool.MaxTasksPerComputeNode = 4;
pool.TaskSchedulingPolicy = new TaskSchedulingPolicy(ComputeNodeFillType.Pack);
pool.Commit();
```

## <a name="batch-rest-example"></a>Példa a köteg többi

A [Köteg többi] [ api_rest] API kódtöredékének jeleníti meg, amely tartalmazza az egyes csomópontok négy feladatok legfeljebb két nagy csomópont-készlet létrehozása a kérést. -Készletek hozzáadása a REST API-val kapcsolatos további tudnivalókért lásd: [a fiók-készlet hozzáadása][rest_addpool].

```json
{
  "odata.metadata":"https://myaccount.myregion.batch.azure.com/$metadata#pools/@Element",
  "id":"mypool",
  "vmSize":"large",
  "cloudServiceConfiguration": {
    "osFamily":"4",
    "targetOSVersion":"*",
  }
  "targetDedicated":2,
  "maxTasksPerNode":4,
  "enableInterNodeCommunication":true,
}
```

> [AZURE.NOTE] Beállíthatja, hogy a `maxTasksPerNode` elemet, valamint [MaxTasksPerComputeNode] [ maxtasks_net] tulajdonság csak időben készlet létrehozása. Azok a készlet létrehozása után nem módosíthatók.

## <a name="code-sample"></a>Példa a kódot.

A [ParallelNodeTasks] [ parallel_tasks_sample] GitHub projekt szemlélteti a [CloudPool.MaxTasksPerComputeNode] e[ maxtasks_net] tulajdonság.

Ez a C# konzol alkalmazás használja a [Köteg .NET] [ api_net] egy vagy több számítási csomópontokkal készlet létrehozása a tárban. A tevékenységek konfigurálható szám hasonlóan változó betöltése e csomóponton hajt végre. Az alkalmazás kimenetét adja meg, hogy mely csomópontok végrehajtott egyes tevékenységek. Az alkalmazás is biztosít a feladat paramétereket és az időtartam összefoglalását. Két különböző futtatja a minta alkalmazásának kimenetét összefoglaló részét alatt jelenik meg.

```
Nodes: 1
Node size: large
Max tasks per node: 1
Tasks: 32
Duration: 00:30:01.4638023
```

A minta alkalmazás első végrehajtása azt mutatja, hogy egy csomópontra a készlet és egy tevékenység egyes csomópontok, az alapértelmezett beállítás a feladat időtartamának 30 percet van.

```
Nodes: 1
Node size: large
Max tasks per node: 4
Tasks: 32
Duration: 00:08:48.2423500
```

A második futtassa a minta megmutatja, hogy a jelentősen csökkenhet a feladat időtartamának. Ennek oka az, a készlet csomópontot, amely lehetővé teszi, hogy a párhuzamos feladat végrehajtásához a feladat elvégzéséhez gyakorlatilag egy negyedév az idő / négy feladatokkal konfigurálásáról.

> [AZURE.NOTE] A fenti összefoglalók a feladat-időtartamok ne kerüljön bele a készlet létrehozási idejének. A fenti feladatok mindegyike elindították, a korábban létrehozott készletek számítási csomópontjainak beadványokra adott időben szerepeltek az *Inaktív* állapotot.

## <a name="next-steps"></a>Következő lépések

### <a name="batch-explorer-heat-map"></a>Köteg Explorer Hőtérkép

Az [Azure köteg Explorer][batch_explorer], az egyik Azure kötegben [mintából alkalmazások][github_samples], *Hőtérkép* szolgáltatás, hogy a feladat-végrehajtás képi megjelenítés tartalmazza. Amikor éppen végrehajtása a [ParallelTasks] [ parallel_tasks_sample] minta alkalmazást, a Hőtérkép funkció használatával egyszerűen ábrázolása minden csomóponton párhuzamos feladatok végrehajtására.

![Köteg Explorer Hőtérkép][1]

*Köteg Explorer négy csomópontok, az egyes jelenleg a négy feladatok végrehajtása csomópont erőforráskészlethez tartozik ábrázoló Hőtérkép*

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[enable_autoscaling]: https://msdn.microsoft.com/library/azure/dn820173.aspx
[fill_type]: https://msdn.microsoft.com/library/microsoft.azure.batch.common.computenodefilltype.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[maxtasks_net]: http://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.maxtaskspercomputenode.aspx
[rest_addpool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[parallel_tasks_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/ParallelTasks
[poolcreate_net]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[task_schedule]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudpool.taskschedulingpolicy.aspx

[1]: ./media/batch-parallel-node-tasks\heat_map.png
