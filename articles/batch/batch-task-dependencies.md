<properties
    pageTitle="A tevékenységfüggőségek Azure kötegben |} Microsoft Azure"
    description="Feldolgozási MapReduce stílus és hasonló nagy adatok számára elvégezhető további feladatokkal sikeres befejezésétől függenek-feladatok létrehozása az Azure köteg munkaterhelésekből."
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
    ms.date="09/28/2016"
    ms.author="marsma" />

# <a name="task-dependencies-in-azure-batch"></a>Tevékenység függések Azure kötegben

A tevékenység függések Azure köteg funkció közelítését Ha fel szeretne dolgozni:

- MapReduce stílusú munkaterhelésekből a felhőben.
- Feladatok, amelynek adatkezelési feladatok kifejezhető irányított aciklikus grafikonhoz (DAG).
- Amelyben lefelé irányuló feladatok függnek felsőbb szintű tevékenységek kimenetének bármely más feladat.

Tevékenységfüggőségek köteg csak egy vagy több feladat sikeres befejezését követően a számítási csomópontok végrehajtásához ütemezett tevékenységek létrehozása teszi lehetővé. Ha például egy feladat, amely az egyes keret egy külön, a párhuzamos feladatok 3D film-alapú is létrehozhat. Az utolsó feladat – a "egyesítés tevékenység" – egyesíti a leképezett keretek be a teljes film csak azt követően az összes képkocka sikeresen megtörtént megjelenítését.

Létrehozhat egy az egyhez vagy egy-a-többhöz kapcsolat más tevékenységek függő tevékenységek. Egy adott tevékenység függ, hogy csoport a tevékenység azonosítókat egy bizonyos tartományba tevékenységek sikeres befejezésétől tartomány függőséget is létrehozhat. Több-a-többhöz kapcsolatok létrehozásához három alapvető forgatókönyvekben is összevonhatja.

## <a name="task-dependencies-with-batch-net"></a>A köteg .NET tevékenységfüggések

Ebben a cikkben a tevékenységfüggőségek beállításáról a [Köteg .NET] használatával ölelik[ net_msdn] tárat. Először bemutatjuk, hogyan [tevékenységfüggőség engedélyezése](#enable-task-dependencies) a feladatok, és bemutatják, majd [feladatként függőségek](#create-dependent-tasks)beállítása. Végezetül ölelik fel, amely támogatja a köteg [függőség esetek](#dependency-scenarios) .

## <a name="enable-task-dependencies"></a>Tevékenység függések engedélyezése

Tevékenységfüggőségek használni a köteg alkalmazást, akkor kell először megállapítani, hogy a köteg szolgáltatás, hogy a feladat használja-e a tevékenységfüggőségek. A köteg .NET, engedélyezni a [CloudJob] [ net_cloudjob] annak [UsesTaskDependencies] megadásával[ net_usestaskdependencies] tulajdonság `true`:

```csharp
CloudJob unboundJob = batchClient.JobOperations.CreateJob( "job001",
    new PoolInformation { PoolId = "pool001" });

// IMPORTANT: This is REQUIRED for using task dependencies.
unboundJob.UsesTaskDependencies = true;
```

Az előző kódtöredéket "batchClient" egy példányát a [BatchClient] [ net_batchclient] osztály.

## <a name="create-dependent-tasks"></a>Függő tevékenységek létrehozása

Feladat, amely egy vagy több feladat sikeres befejezésétől függenek létrehozásához, megadhatja, hogy köteg, hogy a tevékenység "függ, hogy" a másik tevékenységeket. A köteg .NET, állítsa be a [CloudTask][net_cloudtask]. [DependsOn] [net_dependson] tulajdonság-példányt tartson a [TaskDependencies] [ net_taskdependencies] osztály:

```csharp
// Task 'Flowers' depends on completion of both 'Rain' and 'Sun'
// before it is run.
new CloudTask("Flowers", "cmd.exe /c echo Flowers")
{
    DependsOn = TaskDependencies.OnIds("Rain", "Sun")
},
```

A kódtöredék az azonosító "Virágok" futtassa a számítási csomópont csak azt követően a tevékenységeket, amelyek lehetővé tevő dokumentumazonosítók "Eső" és "H" sikeresen befejeződött ütemezett feladat létrehozása.

 > [AZURE.NOTE] Tevékenység hátralévő, ha a **befejeződött** állapotú, és a **kilépési kód** számít `0`. A köteg .NET, ez azt jelenti, hogy egy [CloudTask][net_cloudtask]. [Állam] [net_taskstate] tulajdonság értéke `Completed` és a CloudTask [TaskExecutionInformation][net_taskexecutioninformation]. [ExitCode] [net_exitcode] tulajdonság értéke `0`.

## <a name="dependency-scenarios"></a>Függőség felhasználási területei

Három forgatókönyv egyszerű feladat függőség Azure kötegben használható: egy az egyhez, egy-a-többhöz és Tevékenységazonosító tartományt függőség. Ezek a negyedik esetben, több-a-többhöz megadására kombinálhatók.

 Eset&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | Példa | |
 :-------------------: | ------------------- | -------------------
 [Egy az egyhez](#one-to-one) | *taskB* *taskA* függ. <p/> *taskB* fog nem kell ütemeznie mindaddig, amíg a *taskA* sikeresen befejeződött | ![Ábra: egy az egyhez tevékenységfüggőség][1]
 [Egy-a-többhöz](#one-to-many) | *taskC* függ, hogy *taskA* és a *taskB* <p/> *taskC* fog nem kell ütemeznie mindaddig, amíg a *taskA* és a *taskB* sikeresen befejeződött | ![Ábra: egy-a-többhöz tevékenységfüggőség][2]
 [Tevékenység azonosító tartomány](#task-id-range) | *taskD* attól függ, hogy a tevékenységek tartomány <p/> *taskD* fog nem kell ütemeznie mindaddig, amíg a tevékenységeket, amelyek lehetővé tevő dokumentumazonosítók *1* – *10* sikeresen befejeződött | ![Ábra: Tevékenységfüggőség azonosító tartomány][3]

>[AZURE.TIP] **Több-a-többhöz** kapcsolatot, például, hogy hol C, D, E, és F egyes tevékenységek függő tevékenységek a és b hozhat létre Ez akkor hasznos, ha például parallelized előfeldolgozási forgatókönyvek, ahol a későbbi feladatok attól függenek, több felsőbb szintű tevékenység kimenetét.

### <a name="one-to-one"></a>Egy az egyhez

Szeretne létrehozni, amelynek függőség sikeres befejezésétől egy másik tevékenységet a tevékenység, a [TaskDependencies]Azonosítóra egyetlen feladat ellátására[net_taskdependencies]. [OnId] [net_onid] statikus módszer, ha elhelyezte a [DependsOn] [ net_dependson] [CloudTask]tulajdonsága[net_cloudtask].

```csharp
// Task 'taskA' doesn't depend on any other tasks
new CloudTask("taskA", "cmd.exe /c echo taskA"),

// Task 'taskB' depends on completion of task 'taskA'
new CloudTask("taskB", "cmd.exe /c echo taskB")
{
    DependsOn = TaskDependencies.OnId("taskA")
},
```

### <a name="one-to-many"></a>Egy-a-többhöz

Feladat a [TaskDependencies]azonosítók gyűjteménye megadnia, amely több tevékenység sikeres befejezésétől a feladatot létrehozni,[net_taskdependencies]. [OnIds] [net_onids] statikus módszer, ha elhelyezte a [DependsOn] [ net_dependson] [CloudTask]tulajdonsága[net_cloudtask].

```csharp
// 'Rain' and 'Sun' don't depend on any other tasks
new CloudTask("Rain", "cmd.exe /c echo Rain"),
new CloudTask("Sun", "cmd.exe /c echo Sun"),

// Task 'Flowers' depends on completion of both 'Rain' and 'Sun'
// before it is run.
new CloudTask("Flowers", "cmd.exe /c echo Flowers")
{
    DependsOn = TaskDependencies.OnIds("Rain", "Sun")
},
```

### <a name="task-id-range"></a>Tevékenység azonosító tartomány

Feladat, amely a tevékenységek csoportja sikeres befejezésétől amelynek azonosítók vannak egy tartományon belül létrehozásához, adja meg az első és utolsó tevékenység azonosítók a [TaskDependencies]a tartomány[net_taskdependencies]. [OnIdRange] [net_onidrange] statikus módszer, ha elhelyezte a [DependsOn] [ net_dependson] [CloudTask]tulajdonsága[net_cloudtask].

>[AZURE.IMPORTANT] A függőségeket tevékenység azonosító tartományokat használ, amikor a azonosítók *kell* tartományt feladata karakterlánc megadott egész értékeket. Emellett a tartomány minden tevékenység végre kell hajtania sikeres végrehajtás ütemezhető a függő tevékenység.

```csharp
// Tasks 1, 2, and 3 don't depend on any other tasks. Because
// we will be using them for a task range dependency, we must
// specify string representations of integers as their ids.
new CloudTask("1", "cmd.exe /c echo 1"),
new CloudTask("2", "cmd.exe /c echo 2"),
new CloudTask("3", "cmd.exe /c echo 3"),

// Task 4 depends on a range of tasks, 1 through 3
new CloudTask("4", "cmd.exe /c echo 4")
{
    // To use a range of tasks, their ids must be integer values.
    // Note that we pass integers as parameters to TaskIdRange,
    // but their ids (above) are string representations of the ids.
    DependsOn = TaskDependencies.OnIdRange(1, 3)
},
```

## <a name="code-sample"></a>Példa a kódot.

A [TaskDependencies] [ github_taskdependencies] minta projekt egyike [Azure köteg mintakódok] [ github_samples] a GitHub. A Visual Studio 2015 megoldás bemutatja, hogyan tevékenységfüggőség feladat engedélyezése, másik tevékenységeket függő tevékenységek létrehozása, és hajtsa végre ezeket a feladatokat számítási csomópontok erőforráskészlethez tartozik.

## <a name="next-steps"></a>Következő lépések

### <a name="application-deployment"></a>Alkalmazások telepítésének

Az [alkalmazáscsomagok](batch-application-packages.md) szolgáltatás köteg egyszerűvé telepíteni őket, és az alkalmazások, a feladatok végrehajtása a csomópontok kiszámítania verziót.

### <a name="installing-applications-and-staging-data"></a>Alkalmazások telepítése, és átmeneti adatokat

Nézze meg az [alkalmazások telepítése, és a köteg az átmeneti tárolásra szolgáló adatok kiszámítása csomópontok] [ forum_post] közzé a különböző módszereket előkészítése feladatok futtatásához a csomópontok áttekintése Azure köteg fórumán. Az Azure köteg csapattagok egyike által írt, ebben a bejegyzésben (beleértve az alkalmazások és a tevékenység bemeneti adatok) fájlok különböző módokon a jó alapozó a számítási csomópontokat alakzatot.

[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[github_taskdependencies]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/TaskDependencies
[github_samples]: https://github.com/Azure/azure-batch-samples
[net_batchclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_dependson]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.dependson.aspx
[net_exitcode]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskexecutioninformation.exitcode.aspx
[net_msdn]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_onid]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onid.aspx
[net_onids]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onids.aspx
[net_onidrange]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onidrange.aspx
[net_taskexecutioninformation]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskexecutioninformation.aspx
[net_taskstate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.taskstate.aspx
[net_usestaskdependencies]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.usestaskdependencies.aspx
[net_taskdependencies]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskdependencies.aspx

[1]: ./media/batch-task-dependency/01_one_to_one.png "Ábra: egy az egyhez típusú függés"
[2]: ./media/batch-task-dependency/02_one_to_many.png "Ábra: egy-a-többhöz típusú függés"
[3]: ./media/batch-task-dependency/03_task_id_range.png "Ábra: tevékenységfüggőség azonosító tartomány"
