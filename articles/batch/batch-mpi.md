<properties
    pageTitle="Azure köteg MPI alkalmazások futtatása több példány feladatokkal |} Microsoft Azure"
    description="Megtudhatja, hogy miként végrehajtani üzenet továbbítása felület (MPI) alkalmazások használatával több példány tevékenységtípus Azure kötegben."
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
    ms.date="10/21/2016"
    ms.author="marsma" />

# <a name="use-multi-instance-tasks-to-run-message-passing-interface-mpi-applications-in-azure-batch"></a>Üzenet továbbítása felület (MPI) alkalmazások futtatásához Azure kötegben több példány feladatok használatával

Több elem példány feladatok egyszerre több számítási csomópontok futtatni Azure köteg feladatról teszi lehetővé. Az alábbi műveleteket engedélyezze a nagy teljesítményű számítások esetek, például a köteget az üzenet továbbítása felület (MPI) alkalmazásokat. Ebben a cikkben megismerheti, hogyan a [Köteg .NET] több példány feladatok végrehajtásához[ api_net] tárat.

>[AZURE.NOTE] Bár ez a cikk példáiban koncentráljon köteg .NET, az MS-MPI, és Windows csomópontok számítja ki, az itt tárgyalt több példány tevékenység fogalmak vonatkoznak más platformokon és -technológiák (Python és Linux csomóponton, például az Intel MPI).

## <a name="multi-instance-task-overview"></a>Több elem példány feladatok áttekintése

A köteg, minden feladata rendszerint egyetlen számítási csomóponton – végrehajtott projekt több tevékenység elküldése és a köteg szolgáltatás egyes tevékenységek végrehajtásához a csomóponton ütemezi. Azonban egy tevékenység **több példány beállítások**konfigurálásával, közölje köteg inkább hozzon létre egy elsődleges tevékenység- és több altevékenységet, akkor több csomóponton hajtja végre.

![Több elem példány feladatok áttekintése][1]

Feladat több példány beállításokkal projekt elküldésekor a köteg több példány tevékenységekhez egyedi több műveleteket hajtja végre:

1. A köteg szolgáltatás létrehoz egy **elsődleges** és több **altevékenységet** több példány beállítások alapján. A (Minden altevékenység plusz elsődleges) tevékenységek száma **példányok** (számítási csomópontok) adja meg, ha több példányt beállításainak száma megegyezik.
1. Köteg jelöli ki a számítási csomópontok közül a **mesterlapok**, és az elsődleges végrehajtani a diaminta a tevékenység ütemezése. Ütemezés végrehajtani a kiosztott a több példány tevékenységhez, az egyes csomópontok egy altevékenység számítási csomópontok maradékát altevékenységet.
1. Az elsődleges és a Minden altevékenység töltse le az összes **erőforrás-fájljai** , adja meg, ha több példányt beállításainak.
1. Az általános erőforrás után fájl töltötte le, az elsődleges és altevékenységek adja meg, ha több példányt beállításainak **effektusával parancs** végrehajtása. A effektusával parancs rendszerint használatos csomópontok Felkészülés a tevékenység végrehajtása. Ez elindítja a háttérben szolgáltatásokat is elhelyezhet (például [Microsoft MPI][msmpi_msdn]'s `smpd.exe`) és annak ellenőrzése, hogy a csomópontok készen áll a feldolgozni közötti csomópont üzeneteket.
1. Az elsődleges tevékenység az **alkalmazás parancs** a fő csomópont *után* a effektusával parancs sikeresen befejeződött, az elsődleges és a Minden altevékenység a hajt végre. Az alkalmazás parancs a parancssorból a több elem példány tevékenység magát, és csak az elsődleges tevékenység teljesítette. A az [MS-MPI][msmpi_msdn]-alapú megoldást, ahol, hajtsa végre a MPI engedélyező alkalmazás használata: az `mpiexec.exe`.

> [AZURE.NOTE] Bár funkcionális különböző "több példány" pedig nem egyedi tevékenységtípus, például a [StartTask] [ net_starttask] vagy [JobPreparationTask][net_jobprep]. A több elem példány valóban egyszerűen szabványos köteg feladatot ([CloudTask] [ net_task] a köteg .NET) amelynek több példány beállításai. Ebben a cikkben hivatkozunk meg, a **több elem példány feladatot**.

## <a name="requirements-for-multi-instance-tasks"></a>Több elem példány feladatok vonatkozó követelmények

Több elem példány feladatok szükség **közötti csomópont kommunikáció engedélyezése**és **letiltása egyidejű feladat-végrehajtás**erőforráskészlethez tartozik. Ha több példány tevékenység futtatásakor a készletben szártag kommunikációs letiltva, illetve 1-nél nagyobb *maxTasksPerNode* érték, a tevékenység soha nem ütemezett – marad végtelen időre szóló "aktív" állapotú. A kódtöredék ilyen erőforráskészlethez tartozik a köteg .NET-tár használata létrehozását mutatja.

```csharp
CloudPool myCloudPool =
    myBatchClient.PoolOperations.CreatePool(
        poolId: "MultiInstanceSamplePool",
        targetDedicated: 3
        virtualMachineSize: "small",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));

// Multi-instance tasks require inter-node communication, and those nodes
// must run only one task at a time.
myCloudPool.InterComputeNodeCommunicationEnabled = true;
myCloudPool.MaxTasksPerComputeNode = 1;
```

Ezenkívül több példány feladatok hajthat végre *csak* **14 December 2015 után létrehozott készletek**található csomópontok.

### <a name="use-a-starttask-to-install-mpi"></a>Egy StartTask segítségével MPI telepítése

Több elem példány tevékenység MPI alkalmazások futtatásához, először telepítenie kell egy MPI végrehajtása (MS-MPI vagy Intel MPI, például) meg a készlet számítási csomópontok. Ez az egy időben használni egy [StartTask][net_starttask], amelyek végrehajtása, valahányszor csomópont csatlakozik egy készlet vagy újraindítása után. A kódtöredék létrehoz egy StartTask, amely meghatározza az MS-MPI telepítőcsomag [erőforrásfájl][net_resourcefile]. A kezdő tevékenység parancssori végrehajtása után az erőforrás-fájl letöltődik a csomópontot. Ebben az esetben a parancssorban az MS-MPI felügyelet nélküli telepítés hajt végre.

```csharp
// Create a StartTask for the pool which we use for installing MS-MPI on
// the nodes as they join the pool (or when they are restarted).
StartTask startTask = new StartTask
{
    CommandLine = "cmd /c MSMpiSetup.exe -unattend -force",
    ResourceFiles = new List<ResourceFile> { new ResourceFile("https://mystorageaccount.blob.core.windows.net/mycontainer/MSMpiSetup.exe", "MSMpiSetup.exe") },
    RunElevated = true,
    WaitForSuccess = true
};
myCloudPool.StartTask = startTask;

// Commit the fully configured pool to the Batch service to actually create
// the pool and its compute nodes.
await myCloudPool.CommitAsync();
```

### <a name="remote-direct-memory-access-rdma"></a>Távoli közvetlen memória-hozzáférési (RDMA)

Ha úgy dönt, például az A9 egy [RDMA internetezésre alkalmas méretét](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md) a számítási csomópontok a köteg készletben, a MPI alkalmazás kihasználhassa Azure a nagy teljesítményű, alacsony-késés távoli közvetlen memória hozzáférési (RDMA) hálózathoz.

Keresse meg a következő cikkekben "RDMA alkalmas" megadott mérete:

* **CloudServiceConfiguration** készletek

  * [A Cloud Services méretek](../cloud-services/cloud-services-sizes-specs.md) (Csak Windows)

* **VirtualMachineConfiguration** készletek

  * [Az Azure virtuális gépeken futó méretben](../virtual-machines/virtual-machines-linux-sizes.md) (Linux)

  * [Az Azure virtuális gépeken futó méretben](../virtual-machines/virtual-machines-windows-sizes.md) (Windows)

>[AZURE.NOTE] Kihasználhatja RDMA a [Linux kiszámítania csomópontok](batch-linux-nodes.md), **Intel MPI** a csomóponton kell használnia. CloudServiceConfiguration és VirtualMachineConfiguration készletek a további tudnivalókért olvassa el a a a köteg szolgáltatás áttekintése [készlet](batch-api-basics.md#Pool) szakaszát.

## <a name="create-a-multi-instance-task-with-batch-net"></a>A köteg .NET több példány feladat létrehozása

Most, hogy azt már szereplő a készlet követelmények és a MPI csomag telepítését, létrehozása a több elem példány feladatot. Az ebben a kódtöredék hozzunk létre egy szokásos [CloudTask][net_task], majd állítsa be a saját [MultiInstanceSettings] [ net_multiinstance_prop] tulajdonság. A korábban említett a több elem példány tevékenység nincs különböző tevékenység típusának, de több példány beállításokkal konfigurált szabványos köteg tevékenység.

```csharp
// Create the multi-instance task. Its command line is the "application command"
// and will be executed *only* by the primary, and only after the primary and
// subtasks execute the CoordinationCommandLine.
CloudTask myMultiInstanceTask = new CloudTask(id: "mymultiinstancetask",
    commandline: "cmd /c mpiexec.exe -wdir %AZ_BATCH_TASK_SHARED_DIR% MyMPIApplication.exe");

// Configure the task's MultiInstanceSettings. The CoordinationCommandLine will be executed by
// the primary and all subtasks.
myMultiInstanceTask.MultiInstanceSettings =
    new MultiInstanceSettings(numberOfNodes) {
    CoordinationCommandLine = @"cmd /c start cmd /c ""%MSMPI_BIN%\smpd.exe"" -d",
    CommonResourceFiles = new List<ResourceFile> {
    new ResourceFile("https://mystorageaccount.blob.core.windows.net/mycontainer/MyMPIApplication.exe",
                     "MyMPIApplication.exe")
    }
};

// Submit the task to the job. Batch will take care of splitting it into subtasks and
// scheduling them for execution on the nodes.
await myBatchClient.JobOperations.AddTaskAsync("mybatchjob", myMultiInstanceTask);
```

## <a name="primary-task-and-subtasks"></a>Elsődleges tevékenység és altevékenységekkel

Feladat több példány beállításainak létrehozásakor, amelyek a feladat végrehajtásához számítási csomópontok számát adja meg. A feladat egy feladat elküldésekor a köteg szolgáltatás létrehoz egy **elsődleges** tevékenység- és elegendő **altevékenységet** együtt a megadott csomópontok számának egyeznie.

Az alábbi műveleteket *numberOfInstances* - 1-0 számos egész azonosító van rendelve. A 0-azonosítóval valóban az elsődleges feladatot, és minden más azonosítók altevékenységeket. Például az alábbi beállításokat több példányának egy tevékenység hoz létre, ha az elsődleges tevékenység szeretné, hogy a 0-azonosító, és az altevékenységek szeretné, hogy azonosítók 1-9.

```csharp
int numberOfNodes = 10;
myMultiInstanceTask.MultiInstanceSettings = new MultiInstanceSettings(numberOfNodes);
```

### <a name="master-node"></a>Fő csomópontot.

Több példány tevékenység elküldésekor a köteg szolgáltatás jelöli ki a számítási csomópontok közül a "fő" csomópontot, és az elsődleges végrehajtani a fő csomópontot a tevékenység ütemezése. Az altevékenységek végrehajtani a fennmaradó a több elem példány tevékenységhez hozzárendelt csomópontok van ütemezve.

## <a name="coordination-command"></a>Effektusával parancs

A **effektusával parancs** végrehajtása, mind az elsődleges és altevékenységekkel.

A hívás a effektusával parancs blokkolja – köteg nem hajtsa végre az alkalmazás parancsot mindaddig, amíg a effektusával parancs van esetén a visszaadott sikeresen Minden altevékenység menüpontra. A effektusával parancsot kell ezért indítsa el a szükséges háttér szolgáltatások, győződjön meg róla, hogy azok használatra kész, és zárja be. Ha például 7 elindítja a SMPD szolgáltatás a csomóponton MS-MPI verziójával megoldás a effektusával parancs majd kilép:

```
cmd /c start cmd /c ""%MSMPI_BIN%\smpd.exe"" -d
```

Megjegyzés: az e `start` effektusával parancs. Ez szükség, mert a `smpd.exe` alkalmazás végrehajtása után azonnal nem ad vissza. [Indítsa el] a használata nélkül[ cmd_start] parancsot, a effektusával parancs nem adja vissza, és ezért blokkolhatja futtatását a alkalmazás parancsot.

## <a name="application-command"></a>Alkalmazás parancs

Az elsődleges tevékenység- és minden altevékenység végzett a effektusával parancs végrehajtása, miután a több elem példány tevékenység parancssori teljesítette az elsődleges tevékenység *csak*. Azt hívja meg ez az **alkalmazás parancs** azok megkülönböztetése az összehangolás parancsot.

MS-MPI alkalmazások, a paranccsal alkalmazás hajtsa végre a MPI engedélyező alkalmazással `mpiexec.exe`. Íme például egy alkalmazás parancs használatáról MS-MPI 7-es verzió megoldás:

```
cmd /c ""%MSMPI_BIN%\mpiexec.exe"" -c 1 -wdir %AZ_BATCH_TASK_SHARED_DIR% MyMPIApplication.exe
```

>[AZURE.NOTE] Mivel az MS-MPI `mpiexec.exe` használja a `CCP_NODES` változó alapértelmezés szerint (lásd: a [környezeti változók](#environment-variables)) a példában a fenti alkalmazások parancssor kizárja azt.

## <a name="environment-variables"></a>A környezeti változók

Köteg hoz létre a [környezeti változók] [ msdn_env_var] adott több példány a feladatok a számítási csomópontok, hogy több példány tevékenység rendelve. A effektusával és az alkalmazás parancssorokat ezek a környezeti változók hivatkozhat, amennyit a parancsprogramokat és a programok azokat a végrehajtása lehet.

Az alábbi környezeti változók több példány feladatok szerint létrejönnek a köteg szolgáltatás használata:

* `CCP_NODES`
* `AZ_BATCH_NODE_LIST`
* `AZ_BATCH_HOST_LIST`
* `AZ_BATCH_MASTER_NODE`
* `AZ_BATCH_TASK_SHARED_DIR`
* `AZ_BATCH_IS_CURRENT_NODE_MASTER`

Teljes ezek és a többi köteg számítási csomópont környezet változót, azok tartalma és láthatóság című témakör tartalmaz részletes [csomópont környezeti változók kiszámítania][msdn_env_var].

>[AZURE.TIP] A köteg Linux MPI kód minta tartalmaz egy példa, hogy ezek a környezeti változók számos használható-e. A [effektusával-cmd] [ coord_cmd_example] Bash parancsfájl letöltések közös alkalmazást, és a beviteli Azure tárterület-fájljait, lehetővé teszi, hogy egy hálózati fájl (NFS) megosztása a fő csomóponton és állítja be a több elem példány tevékenységhez rendelt NFS ügyfelek csomópontok.

## <a name="resource-files"></a>Erőforrásfájl

Kétféle erőforrásfájl több példány tevékenységek szempontok van: *minden* tevékenység letöltése **általános erőforrás-fájlok** (mind az elsődleges és altevékenységekkel), és **erőforrás-fájl** a megadott több példány magát, mely *csak az elsődleges* feladat letölti a tevékenység.

Egy vagy több **általános erőforrás-fájljai** megadhatja a tevékenység több példány beállításait. A közös erőforrás-fájlok az egyes csomópontok **tevékenység megosztott könyvtár** az elsődleges és a Minden altevékenység [Azure](./../storage/storage-introduction.md) -tárhelyről töltődnek le. Elérhetők a tevékenység megosztott címtár parancssorokat alkalmazás és a effektusával használatával a `AZ_BATCH_TASK_SHARED_DIR` környezeti. A `AZ_BATCH_TASK_SHARED_DIR` elérési útja azonos minden csomóponton a több elem példány tevékenység rendelve, így megoszthat egyetlen effektusával parancs az elsődleges és a Minden altevékenység között. Köteg "megosztva" a címtárban távelérési értelemben, de csatlakoztatási használni, vagy megoszthatja a tipp a környezeti változók korábban említett pont.

Erőforrás megadott a több elem példány tevékenység magát letöltődnek a tevékenység munkakönyvtárat, `AZ_BATCH_TASK_WORKING_DIR`, alapértelmezés szerint. Említett, általános erőforrás-fájljai alkalmazással szemben a csak az elsődleges tevékenység letölti a több elem példány tevékenység magát a megadott erőforrás-fájlok.

> [AZURE.IMPORTANT] Mindig a környezeti változók használata `AZ_BATCH_TASK_SHARED_DIR` és `AZ_BATCH_TASK_WORKING_DIR` ezeket a parancsokat könyvtárak hivatkozik. Próbálja meg manuálisan Egyenletszerkesztővel az elérési út.

## <a name="task-lifetime"></a>Tevékenység élettartam

Az elsődleges tevékenység időtartama határozza meg a teljes több példány tevékenység időtartama. Amikor kijelentkezik az elsődleges, minden altevékenységet megszakadtak. Az elsődleges kilépési kódját a kilépési kódot a tevékenység, és ezért azt határozza meg, a megoldás sikeres vagy sikertelen a tevékenység újrapróbálkozási céljából.

Ha nem sikerül az altevékenységek közül, nullától visszatérési kóddal Kilépés például a teljes több példány sikerül a művelet. A több elem példány tevékenység van szüntetni, majd újból, a kísérletek korlátot felfelé.

Ha több példányt tevékenység töröl, akkor az elsődleges és a Minden altevékenység is törlődnek a köteg szolgáltatás. Az összes altevékenység könyvtárak, és az fájljait a számítási csomópontok ugyanúgy, mint a szokásos tevékenység törlődnek.

[TaskConstraints] [ net_taskconstraints] több példány feladat, például a [MaxTaskRetryCount][net_taskconstraint_maxretry], [MaxWallClockTime][net_taskconstraint_maxwallclock], és [RetentionTime] [ net_taskconstraint_retention] tulajdonságok vannak fogadja a szabványos feladat vannak, és az elsődleges és a Minden altevékenység vonatkoznak. Jó helyen jár Ha például módosítja a [RetentionTime] [ net_taskconstraint_retention] tulajdonság a több elem példány tevékenység hozzáadása a projekthez, ezt a módosítást után csak az elsődleges tevékenység van alkalmazva. Minden altevékenységet továbbra is használhatja az eredeti [RetentionTime][net_taskconstraint_retention].

A számítási csomópont legutóbbi feladatlista egy altevékenység azonosítója tükrözi, ha a legutóbbi feladat volt több példány tevékenység részét.

## <a name="obtain-information-about-subtasks"></a>Altevékenységek információhoz juthat

Altevékenységek olvashat a köteg .NET tár beszerzéséhez hívja fel a [CloudTask.ListSubtasks] [ net_task_listsubtasks] módot. Ez a módszer minden altevékenység információkat, és a számítási csomópontot, a feladatok végrehajtott információt adja eredményül. Ezt az információt a megállapítható, hogy minden altevékenység legfelső szintű directory, a készlet azonosítót, annak aktuális állapotában, kilépési kódot és az egyéb. Ez az információ használható együtt a [PoolOperations.GetNodeFile] [ poolops_getnodefile] az altevékenység fájlok beszerzéséhez. Megjegyzés: Ez a módszer nem ad vissza az elsődleges tevékenység (0-azonosító) adatait.

> [AZURE.NOTE] Hiányában köteg .NET módszerek, amely a működjön, a több elem példány [CloudTask] [ net_task] magát alkalmazása *csak* az elsődleges tevékenységhez. Ha például a a [CloudTask.ListNodeFiles] hívás[ net_task_listnodefiles] módszer több példány a tevékenységen, csak az elsődleges tevékenység fájlok ad vissza.

A következő kódrészletet altevékenység információk beszerzése, valamint a fájl tartalmát kérése a csomópontok, amelyen végrehajtásra mutatja.

```csharp
// Obtain the job and the multi-instance task from the Batch service
CloudJob boundJob = batchClient.JobOperations.GetJob("mybatchjob");
CloudTask myMultiInstanceTask = boundJob.GetTask("mymultiinstancetask");

// Now obtain the list of subtasks for the task
IPagedEnumerable<SubtaskInformation> subtasks = myMultiInstanceTask.ListSubtasks();

// Asynchronously iterate over the subtasks and print their stdout and stderr
// output if the subtask has completed
await subtasks.ForEachAsync(async (subtask) =>
{
    Console.WriteLine("subtask: {0}", subtask.Id);
    Console.WriteLine("exit code: {0}", subtask.ExitCode);

    if (subtask.State == TaskState.Completed)
    {
        ComputeNode node =
            await batchClient.PoolOperations.GetComputeNodeAsync(subtask.ComputeNodeInformation.PoolId,
                                                                 subtask.ComputeNodeInformation.ComputeNodeId);

        NodeFile stdOutFile = await node.GetNodeFileAsync(subtask.ComputeNodeInformation.TaskRootDirectory + "\\" + Constants.StandardOutFileName);
        NodeFile stdErrFile = await node.GetNodeFileAsync(subtask.ComputeNodeInformation.TaskRootDirectory + "\\" + Constants.StandardErrorFileName);
        stdOut = await stdOutFile.ReadAsStringAsync();
        stdErr = await stdErrFile.ReadAsStringAsync();

        Console.WriteLine("node: {0}:", node.Id);
        Console.WriteLine("stdout.txt: {0}", stdOut);
        Console.WriteLine("stderr.txt: {0}", stdErr);
    }
    else
    {
        Console.WriteLine("\tSubtask {0} is in state {1}", subtask.Id, subtask.State);
    }
});
```

## <a name="code-sample"></a>Példa a kódot.

A [MultiInstanceTasks] [ github_mpi] GitHub a kód minta bemutatja, hogyan lehet több példány tevékenység az [MS-MPI] futtatásához használt[ msmpi_msdn] köteg alkalmazás csomópontok számítja ki. Kövesse a [előkészítése](#preparation) és futtatása a minta [végrehajtása](#execution) .

### <a name="preparation"></a>Előkészítése

1. Kövesse a első két [fordítása és egy egyszerű MS-MPI programindítás][msmpi_howto]. Ez a következő lépésben a prerequesites felel meg.
1. A *végleges* verzióját a [MPIHelloWorld] összeállítása[ helloworld_proj] minta MPI programot. Ez az a program a több elem példány feladat által futtatandó számítási csomóponton.
1. Tartalmazó zip fájl létrehozása `MPIHelloWorld.exe` (mely, beépített lépés: 2) és `MSMpiSetup.exe` (amely letöltött lépés: 1). A zip-fájl a következő lépésben egy alkalmazás csomagként tölti fel.
1. Az [Azure portál] használata[ portal] szeretne létrehozni a "MPIHelloWorld" nevű egy köteg [alkalmazást](batch-application-packages.md) , és adja meg a zip-fájl létrehozott az előző lépésben az alkalmazáscsomag "1.0" változata. [Töltse fel és az alkalmazások kezelése](batch-application-packages.md#upload-and-manage-applications) talál további információt.

>[AZURE.TIP] *Végleges* verzióját összeállítása `MPIHelloWorld.exe` , hogy nem kell minden olyan további függőségek tartalmazza (például `msvcp140d.dll` vagy `vcruntime140d.dll`) az alkalmazás csomagban.

### <a name="execution"></a>Végrehajtási

1. Töltse le [a minták köteg azure] [ github_samples_zip] a GitHub.
1. Nyissa meg azt a MultiInstanceTasks **megoldás** a Visual Studio 2015. A `MultiInstanceTasks.sln` megoldásfájlt található:

    `azure-batch-samples\CSharp\ArticleProjects\MultiInstanceTasks\`

1. Írja be a köteget, és a tárhely fiókhoz tartozó hitelesítő adatok `AccountSettings.settings` a **Microsoft.Azure.Batch.Samples.Common** projekt.
1. **Létrehozása és futtatása** a MultiInstanceTasks megoldást a MPI végrehajtani a példa a alkalmazás számítja ki egy köteg készletben csomópontot.
1. *Nem kötelező*: az [Azure portál] használata[ portal] vagy a [Köteg Explorer] [ batch_explorer] szemügyre veszi a minta készlet, a feladatot, és a tevékenység ("MultiInstanceSamplePool", "MultiInstanceSampleJob", "MultiInstanceSampleTask") előtt az erőforrások törlése.

>[AZURE.TIP] Töltse le [A Visual Studio közösségi] [ visual_studio] szabad, ha nem rendelkezik a Visual Studio.

A kimenet `MultiInstanceTasks.exe` a következőhöz hasonló:

```
Creating pool [MultiInstanceSamplePool]...
Creating job [MultiInstanceSampleJob]...
Adding task [MultiInstanceSampleTask] to job [MultiInstanceSampleJob]...
Awaiting task completion, timeout in 00:30:00...

Main task [MultiInstanceSampleTask] is in state [Completed] and ran on compute node [tvm-1219235766_1-20161017t162002z]:
---- stdout.txt ----
Rank 2 received string "Hello world" from Rank 0
Rank 1 received string "Hello world" from Rank 0

---- stderr.txt ----

Main task completed, waiting 00:00:10 for subtasks to complete...

---- Subtask information ----
subtask: 1
        exit code: 0
        node: tvm-1219235766_3-20161017t162002z
        stdout.txt:
        stderr.txt:
subtask: 2
        exit code: 0
        node: tvm-1219235766_2-20161017t162002z
        stdout.txt:
        stderr.txt:

Delete job? [yes] no: yes
Delete pool? [yes] no: yes

Sample complete, hit ENTER to exit...
```

## <a name="next-steps"></a>Következő lépések

- A Microsoft HPC és Azure köteg csapat blogbejegyzés azt ismerteti, hogy [MPI Azure köteg Linux támogatása][blog_mpi_linux], többek között a [OpenFOAM] használatával és[ openfoam] a köteget. Python mintakódok megtalálhatja a [OpenFOAM példa a GitHub][github_mpi].

- További tudnivalók a [Linux számítási csomópontok készletek létrehozása](batch-linux-nodes.md) az Azure köteg MPI megoldások a való használatra.

[helloworld_proj]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/MultiInstanceTasks/MPIHelloWorld

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[blog_mpi_linux]: https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/
[cmd_start]: https://technet.microsoft.com/library/cc770297.aspx
[coord_cmd_example]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/article_samples/mpi/data/linux/openfoam/coordination-cmd
[github_mpi]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/MultiInstanceTasks
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_zip]: https://github.com/Azure/azure-batch-samples/archive/master.zip
[msdn_env_var]: https://msdn.microsoft.com/library/azure/mt743623.aspx
[msmpi_msdn]: https://msdn.microsoft.com/library/bb524831.aspx
[msmpi_sdk]: http://go.microsoft.com/FWLink/p/?LinkID=389556
[msmpi_howto]: http://blogs.technet.com/b/windowshpc/archive/2015/02/02/how-to-compile-and-run-a-simple-ms-mpi-program.aspx
[openfoam]: http://www.openfoam.com/
[visual_studio]: https://www.visualstudio.com/vs/community/

[net_jobprep]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.aspx
[net_multiinstance_class]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.aspx
[net_multiinstance_prop]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.multiinstancesettings.aspx
[net_multiinsance_commonresfiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.commonresourcefiles.aspx
[net_multiinstance_coordcmdline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.coordinationcommandline.aspx
[net_multiinstance_numinstances]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.numberofinstances.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_pool_create]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[net_pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_resourcefile]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.resourcefile.aspx
[net_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.aspx
[net_starttask_cmdline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.commandline.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_taskconstraints]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.aspx
[net_taskconstraint_maxretry]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.maxtaskretrycount.aspx
[net_taskconstraint_maxwallclock]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.maxwallclocktime.aspx
[net_taskconstraint_retention]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.retentiontime.aspx
[net_task_listsubtasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listsubtasks.aspx
[net_task_listnodefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listnodefiles.aspx
[poolops_getnodefile]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.getnodefile.aspx

[portal]: https://portal.azure.com
[rest_multiinstance]: https://msdn.microsoft.com/library/azure/mt637905.aspx

[1]: ./media/batch-mpi/batch_mpi_01.png "Több elem példány – áttekintés"
