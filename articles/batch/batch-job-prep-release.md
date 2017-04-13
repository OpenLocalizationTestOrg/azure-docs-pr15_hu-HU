<properties
    pageTitle="Projekt előkészítése és karbantartása a köteg |} Microsoft Azure"
    description="Azure köteg számítási csomópontok adatok átvitele összezárása feladat szintű előkészítése feladatok használatával, és engedje fel az csomópont karbantartása a feladat befejezése: a feladatok."
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
    ms.date="09/16/2016"
    ms.author="marsma" />

# <a name="run-job-preparation-and-completion-tasks-on-azure-batch-compute-nodes"></a>Futtassa a előkészítése és a Befejezés projektfeladatok Azure köteg számítási csomóponton

 Egy köteg Azure feladat gyakran előtt feladatainak kell végrehajtani, és a feladatok befejezése utáni feladat a karbantartási van szükség bizonyos űrlap a beállítás. Szükség lehet, töltse le a számítási csomópontok gyakori feladatok a bemeneti adatait, vagy töltse fel a kimeneti feladatadatokat Azure tárolóhoz a feladat befejeződése után. **Projekt előkészítése** és **engedje fel az feladat** tevékenységek segítségével a következő műveletek hajthatók végre.

## <a name="what-are-job-preparation-and-release-tasks"></a>Mi a projekt előkészítése, és engedje fel az feladatok?

Futtassa a projektfeladatok, mielőtt a projekt előkészítése tevékenység legalább egy tevékenységhez ütemezve minden számítási csomóponton fut. Miután a feladat befejezése után, a Megjelenés projektfeladat a készletben legalább egy tevékenység végrehajtott minden csomóponton fut. A normál köteg műveleteket, adja meg a parancssor képezheti projektfeladat előkészítése vagy release futtatásakor.

Előkészítés és megjelenés projektfeladatok kínálnak köteg tevékenység ismerős funkciói, például fájlletöltés ([erőforrás-fájljai][net_job_prep_resourcefiles]), magasabb szintű a végrehajtási, egyéni környezeti változók, maximális végrehajtási időtartam, kísérletek száma és fájl adatmegőrzési dátumát.

Az alábbi szakaszok használata a [JobPreparationTask] Dióhéjban[ net_job_prep] és [JobReleaseTask] [ net_job_release] osztályok található a [Köteg .NET] [ api_net] tárat.

> [AZURE.TIP] Előkészítés és megjelenés projektfeladatok hasznosak lehetnek "megosztott készlet" környezetben, amelyben számítási csomópontok erőforráskészlethez tartozik két feladat futtatása között továbbra is fennáll, és sok feladatok használják.

## <a name="when-to-use-job-preparation-and-release-tasks"></a>Mikor érdemes használni a projekt előkészítése és engedje fel az feladatok

Projekt előkészítése és megjelenés projektfeladatok közelítését az alábbi esetekben:

**Gyakori feladatok adatait letöltése**

Köteg feladatok gyakran szükség adatok közös csoportja a projektfeladatok bemeneteként. Például a kockázat adatelemzési számítások napi, piaci adata projektspecifikus, még gyakori feladatok a feladat. Piaci adat, gyakran több GB méretű, érdemes le minden számítási csomópont csak egyszer, hogy bármely tevékenység, amelyet a csomóponton futtat vele. **Projekt előkészítése feladatok** használatával töltse adat minden csomópontot, mielőtt a feladat végrehajtását adatait a másik tevékenységeket.

**Feladat és a tevékenység kimeneti törlése**

"Megosztott készlet" környezetben, amelyben egy készlet számítási csomópontok még nem leszerelt a feladatok között, előfordulhat, milyen gyakran fusson projekt adatainak törlése. Szükség lehet takarékos lemezterület a csomóponton és felel meg szervezete biztonsági házirendeket. **Megjelenés projektfeladat** segítségével előkészítése projektfeladat által letöltött, vagy feladat-végrehajtás során létre adatok törlése.

**Log adatmegőrzési**

Érdemes lehet az, hogy a feladatok készítése naplófájlok másolatának vagy sikertelen alkalmazások által létrehozott fájlok kiírása esetleg összeomlik. Ebben az esetben **Megjelenés projektfeladat** segítségével tömörítése, és töltse fel ezeket az adatokat az [Azure-tároló] [ azure_storage] fiókot.

>[AZURE.TIP] A kimeneti úgy is, hogy továbbra is fennáll, naplók és más feladatot, és a tevékenység adatai az [Azure köteg fájl szabályok](batch-task-output.md) könyvtár használatára.

## <a name="job-preparation-task"></a>Projektfeladat előkészítése

A feladat feladatok végrehajtására, mielőtt köteg minden számítási csomóponton ütemezett feladat futtatása a projekt előkészítése tevékenység hajt végre. A köteg szolgáltatás alapértelmezés szerint vár, a projekt előkészítése tevékenység hátralévő ütemezett végrehajtani a csomópontra a feladatokat futtatása előtt. Azt is beállíthatja azonban nem, hogy csak a szolgáltatást. Ha újraindul a csomópontot, a projekt előkészítése tevékenység ismét futtatja, de is letilthatja a jelenség.

A projekt előkészítése tevékenység csak ütemezett feladat futtatása csomópontok végrehajtása. Abban az esetben, ha egy csomópontot nem tartozik egy tevékenység így előkészítése tevékenység szükségtelen végrehajtását. Ez akkor fordulhat elő, ha a projekt a tevékenységek száma kisebb, mint egy készlet csomópontok számának. Ha [egyidejű feladat-végrehajtás](batch-parallel-node-tasks.md) engedélyezve van, amely egyes csomópontok üresjárati elhagyja a feladatok száma kisebb, mint az összes lehetséges egyidejű feladatok esetén is érinti. Nem fut a projekt előkészítése tevékenység üresjárati csomóponton, tölthet adatok átadás költségek kevesebb pénzt.

> [AZURE.NOTE] [JobPreparationTask] [ net_job_prep_cloudjob] különböznek egymástól [CloudPool.StartTask] [ pool_starttask] annak, hogy JobPreparationTask egyes a projekt elején hajt végre, mivel StartTask hajt végre, csak akkor, ha egy számítási csomópont először csatlakozik egy készlet vagy újraindul.

## <a name="job-release-task"></a>Megjelenés projektfeladat

A feladat kész megjelölést, miután a Megjelenés projektfeladat végrehajtása a készletben legalább egy tevékenység végrehajtott egyes csomópontot. Akkor elvégzettként megjelölni egy feladat lezáró küld. A köteg szolgáltatás majd állítja be a feladat állapota *végződő*, megszakítja a feladathoz kapcsolódó bármelyik aktív vagy futó feladatok és fut, a Megjelenés projektfeladat. A feladat *befejezése* állam lép.

> [AZURE.NOTE] Feladat törlését is végrehajtja a Megjelenés projektfeladat. Ha azonban a feladat már lett állítva, a feladat visszaadása van nem futtatni másodszori, ha a feladat később törlődik.

## <a name="job-prep-and-release-tasks-with-batch-net"></a>Feladat prep le és engedje fel a köteg .NET feladatok

Projekt előkészítése tevékenység használatához hozzárendelése egy [JobPreparationTask] [ net_job_prep] a feladatok [CloudJob.JobPreparationTask] objektum[ net_job_prep_cloudjob] tulajdonság. Hasonlóképpen, inicializálni egy [JobReleaseTask] [ net_job_release] , és rendelje hozzá a feladatok [CloudJob.JobReleaseTask] [ net_job_prep_cloudjob] tulajdonság beállítása a feladat feladat visszaadása.

Kattintson a kódtöredék `myBatchClient` [BatchClient]egy példánya[net_batch_client], és `myPool` egy meglévő készlet a köteg számla belüli van.

```csharp
// Create the CloudJob for CloudPool "myPool"
CloudJob myJob =
    myBatchClient.JobOperations.CreateJob(
        "JobPrepReleaseSampleJob",
        new PoolInformation() { PoolId = "myPool" });

// Specify the command lines for the job preparation and release tasks
string jobPrepCmdLine =
    "cmd /c echo %AZ_BATCH_NODE_ID% > %AZ_BATCH_NODE_SHARED_DIR%\\shared_file.txt";
string jobReleaseCmdLine =
    "cmd /c del %AZ_BATCH_NODE_SHARED_DIR%\\shared_file.txt";

// Assign the job preparation task to the job
myJob.JobPreparationTask =
    new JobPreparationTask { CommandLine = jobPrepCmdLine };

// Assign the job release task to the job
myJob.JobReleaseTask =
    new JobPreparationTask { CommandLine = jobReleaseCmdLine };

await myJob.CommitAsync();
```

A korábban említett a feladat visszaadása végre, ha a feladat megszakítása vagy törölt. Egy feladat [JobOperations.TerminateJobAsync]Leállítás[net_job_terminate]. A [JobOperations.DeleteJobAsync]feladat törlése[net_job_delete]. A szokásos megszakítása vagy feladat törlése a tevékenység időre elkészüljön vagy egy korábban megadott időkorlát elérésekor.

```csharp
// Terminate the job to mark it as Completed; this will initiate the
// Job Release Task on any node that executed job tasks. Note that the
// Job Release Task is also executed when a job is deleted, thus you
// need not call Terminate if you typically delete jobs after task completion.
await myBatchClient.JobOperations.TerminateJobAsy("JobPrepReleaseSampleJob");
```

## <a name="code-sample-on-github"></a>A GitHub kód minta

Projekt előkészítése és engedje fel az feladatok működését, olvassa el a [JobPrepRelease] [ job_prep_release_sample] GitHub minta projekt. A New az alábbi műveleteket végzi el:

1. Létrehoz egy készlet két "– kicsi" csomópontot.
2. Létrehoz egy feladatot projekt előkészítése, a Megjelenés és a szokásos tevékenységek.
3. Futtatja az előkészítés először ír a csomópont-azonosító egy csomópont "megosztott" címtárban szövegfájl feladat feladatot.
4. Feladat, amely ugyanannak a fájlnak szöveget ír a Tevékenységazonosító minden csomóponton fut.
5. Miután minden tevékenység időre elkészüljön (vagy a időtúllépését), a konzol minden csomópont szövegfájl tartalmának nyomtatása
6. A feladat befejezése után futtatja a Megjelenés projektfeladat a fájl törlése a csomópontot.
6. A kilépési kódokat az előkészítés és megjelenés projektfeladatok minden végrehajtva amely csomópont-nyomtatja.
7. Szünet végrehajtás feladat és/vagy a készlet törlésének megerősítését engedélyezni.

A minta alkalmazás kimenetét az alábbihoz hasonló:

```
Attempting to create pool: JobPrepReleaseSamplePool
Created pool JobPrepReleaseSamplePool with 2 small nodes
Checking for existing job JobPrepReleaseSampleJob...
Job JobPrepReleaseSampleJob not found, creating...
Submitting tasks and awaiting completion...
All tasks completed.

Contents of shared\job_prep_and_release.txt on tvm-2434664350_1-20160623t173951z:
-------------------------------------------
tvm-2434664350_1-20160623t173951z tasks:
  task001
  task004
  task005
  task006

Contents of shared\job_prep_and_release.txt on tvm-2434664350_2-20160623t173951z:
-------------------------------------------
tvm-2434664350_2-20160623t173951z tasks:
  task008
  task002
  task003
  task007

Waiting for job JobPrepReleaseSampleJob to reach state Completed
...

tvm-2434664350_1-20160623t173951z:
  Prep task exit code:    0
  Release task exit code: 0

tvm-2434664350_2-20160623t173951z:
  Prep task exit code:    0
  Release task exit code: 0

Delete job? [yes] no
yes
Delete pool? [yes] no
yes

Sample complete, hit ENTER to exit...
```

>[AZURE.NOTE] A változó létrehozását és a kezdési ideje (egyes csomópontok készen állnak a tevékenységek előtt mások) az új készlet található csomópontok, hogy a különböző kimeneti jelenhet meg. Kifejezetten a feladatok elvégzéséhez gyorsan, mert a készlet csomópontok egyike futhat-e az összes a projektfeladatokat. Ez akkor fordulhat elő, ha megfigyelheti, hogy a projekt előkészítése és a fontos feladatok nem léteznek a csomópont, amelyek nincsenek feladatok végrehajtása.

### <a name="inspect-job-preparation-and-release-tasks-in-the-azure-portal"></a>Nézze meg a projekt előkészítése és a fontos feladatok az Azure-portálon

A minta alkalmazás futtatásakor is használhatja az [Azure portál] [ portal] a feladatot, és a tevékenységekhez tulajdonságainak megtekintése és a megosztott szövegfájl, a projektfeladatok módosított még letöltéséhez szükséges.

Az alábbi képernyőképen a **előkészítése feladatok lap** az Azure-portálon egy a minta kérelem futtatása után. Nyissa meg a *JobPrepReleaseSampleJob* tulajdonságait, a feladatok befejezése után (de a feladatot, és a készlet törlése előtt), majd kattintson a **előkészítése feladatok** és a **fontos feladatok** tulajdonságaik megtekintéséhez.

![Projekt előkészítése-tulajdonságok az Azure-portálon][1]

## <a name="next-steps"></a>Következő lépések

### <a name="application-packages"></a>Alkalmazáscsomagok

A projekt előkészítése feladat mellett is használhatja az [alkalmazáscsomagok](batch-application-packages.md) szolgáltatás köteg számítási csomópontok Felkészülés feladat-végrehajtás. Ez a funkció különösen hasznos, ha a telepítő, sok fájl (100 +) tartalmazó alkalmazások vagy szigorú verziókövetés igénylő alkalmazások nem igénylő alkalmazások telepítése.

### <a name="installing-applications-and-staging-data"></a>Alkalmazások telepítése, és átmeneti adatokat

Fórum az MSDN webhelyen bejegyzéssel áttekintést nyújt az a csomópontok előkészítése feladatok futtatása, több lehetőség is kínálkozik:

[Alkalmazások telepítése, és átmeneti adatokat a köteg kiszámítania csomópontok][forum_post]

Az Azure köteg csapattagok egyike által írt, azt ismerteti, hogy számos módszert terjesztése az alkalmazások és az adatok csomópontok kiszámítására használható.

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_listjobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[azure_storage]: https://azure.microsoft.com/services/storage/
[portal]: https://portal.azure.com
[job_prep_release_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/JobPrepRelease
[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[net_batch_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]:https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_job_prep]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.aspx
[net_job_prep_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobpreparationtask.aspx
[net_job_prep_resourcefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.resourcefiles.aspx
[net_job_delete]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.deletejobasync.aspx
[net_job_terminate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.terminatejobasync.aspx
[net_job_release]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobreleasetask.aspx
[net_job_release_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobreleasetask.aspx
[pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx

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

[1]: ./media/batch-job-prep-release/portal-jobprep-01.png
