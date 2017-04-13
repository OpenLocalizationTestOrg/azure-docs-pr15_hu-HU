<properties
    pageTitle="Feladat és a tevékenység kimeneti Azure kötegben adatmegőrzési |} Microsoft Azure"
    description="Megtudhatja, hogy miként használhatja az Azure tárhely, mint egy tartós tárolása a köteg tevékenység és a feladat kimeneti és engedélyezése az e állandó kibocsátás megtekintése az Azure-portálon."
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
    ms.date="09/07/2016"
    ms.author="marsma" />

# <a name="persist-azure-batch-job-and-task-output"></a>Azure köteg feladatot, és a tevékenység kimeneti marad meg

A feladatok általában futtassa a köteg, amely kell tárolni és később visszakeresése a feldolgozás, az ügyfélalkalmazás a feladatot, vagy mindkettőt végrehajtott egyéb műveletek által kimeneti készít. A kimenet előfordulhat, hogy a bemeneti adatok feldolgozása által létrehozott fájlok vagy társított feladat-végrehajtás naplófájlok. Ez a cikk bemutatja a .NET osztálytár ilyen feladat kibocsátás Azure Blob-tárolóhoz azt elérhetővé tétele a készletek, feladatok, törlése és csomópontok kiszámítania után is fennáll a szabályok-alapú technika használó.

Ez a cikk az eljárás használatával is megtekintheti a tevékenység kimeneti **mentett kimeneti fájlokat** és a **Naplók mentése** az [Azure portál][portal].

![A kimeneti fájl mentése és mentett választók naplózza a portálon][1]

>[AZURE.NOTE] Az [Azure köteg fájl szabályok] [ nuget_package] a jelen cikkben tárgyalt .NET osztálytár jelenleg előzetes verzióban. Itt leírt szolgáltatások némelyike megváltozhat általános elérhetőség előtt.

## <a name="task-output-considerations"></a>Tevékenység kimeneti kapcsolatos szempontok

A köteg megoldás tervezésekor figyelembe kell vennie számos tényező kapcsolódó feladatot, és a tevékenység kimeneti értékeket.

* **Csomópont élettartam kiszámítása**: csomópontok gyakran tranziens, különösen készletek Automatikus méretezéssel engedélyező kiszámítására. A csomóponton futó feladatokat a kimeneti értékeket csak, miközben a csomópont létezik, és csak a fájl adatmegőrzési idő alatt a tevékenység beállított érhetők el. Annak érdekében, hogy a tevékenység kimenet megmarad, a feladatok kell ezért töltse fel a kimeneti fájl tartós mentése, például az Azure-tárolóhoz.

* **Kimeneti tároló**: továbbra is fennáll, tartós tárolóhoz kimeneti feladatadatokat, használhatja az [Azure tároló SDK](../storage/storage-dotnet-how-to-use-blobs.md) a tevékenység kódban töltse fel a tevékenység kimenet Blob-tároló tárolóhoz. Ha egy tároló és fájlelnevezési alkalmazza, az ügyfélalkalmazás vagy más feladatok a feladat keresse meg, majd töltse le a kimenet a kiállítás alapján.

* **Kimeneti lekérés**: meghallgathatja tevékenység kimeneti közvetlenül a készlet számítási csomópontját, vagy Azure-tárhelyről, ha a feladatok megmaradnak a kimeneti. A tevékenység kimeneti közvetlenül lekérhetők egy számítási csomópontot, szüksége van a fájl nevét, és a kimenet helyét a csomópontra. Ha a kimenet Azure tárolóhoz, feldolgozott feladatok továbbra is fennáll, vagy az ügyfélalkalmazás rendelkeznie kell a teljes elérési útját a fájl letöltéséhez az Azure tárolási SDK segítségével Azure-tárolóban lévő.

* **Kimeneti megtekintése**: amikor egy köteg tevékenység az Azure-portálon nyissa meg és jelölje ki **a csomópontra a fájlokat**, a feladathoz kapcsolódó összes fájl jelenik meg, nem csak a kimeneti fájl, amely érdekli. Ismét a számítási csomópontok fájlok érhetők el csak, miközben a csomópont létezik, és csak a fájl adatmegőrzési idő alatt a tevékenység állított be. Tevékenység kimeneti, amely a korábban állandó Azure tárolóhoz a portálon vagy egy alkalmazást, például az [Azure tároló Explorer]megtekintéséhez[storage_explorer], kell, hogy a helyét, és közvetlenül nyissa meg azt a fájlt.

## <a name="help-for-persisted-output"></a>Súgó a állandó kimeneti

Segít több egyszerűen továbbra is fennáll feladatot, és a kimeneti tevékenység, a köteg csapatának meghatározott és egy sor olyan elnevezési szabályokat, valamint a .NET osztálytár az [Azure köteg fájl szabályok] végrehajtott[ nuget_package] tárban, a köteg alkalmazásban használható. Ezeken kívül az Azure portál tisztában az alábbi elnevezési szabályokat, hogy később könnyen megtalálja a tár használatával már tárolt fájlok.

## <a name="using-the-file-conventions-library"></a>A fájl szabályok tár használata

[Azure köteg fájl szabályok] [ nuget_package] van egy .NET osztálytár, hogy a köteg .NET-alkalmazások egyszerű tárolására, valamint tevékenység kimeneti értékeket szeretne és Azure-tárhelyről használható. Célja, hogy a tevékenység- és ügyfél kód – az megőrzéséhez fájlok tevékenység kódját és a listája, valamint ügyfél kód használata. A feladatok számára beolvasása a kimeneti értékeket a felsőbb szintű tevékenységek, például a [tevékenységfüggőségek](batch-task-dependencies.md) helyzetben is használhatja a tárat.

A szabályok tár gondoskodik annak biztosítására, hogy a tároló és a tevékenység kimeneti fájlok megfelelően a kiállítás neve és a megfelelő helyre, amikor az Azure-tárolóhoz állandó van feltöltve. Amikor kimeneti értékeket, könnyen megtalálja a kimeneti értékeket egy adott feladatra, vagy a tevékenység listaelem vagy lekérdezése által azonosító és a célt, nem kell, hogy fájlnevek, vagy ha létezik a tárterület a kimeneti értékeket.

Például is használhatja a tár "a lista összes köztes fájl a feladat 7" vagy "beszerzése számomra a miniatűr előnézet feladat *mymovie*," anélkül, hogy meg, hogy a fájl nevét, vagy a tárterület-fiókja pontjára.

### <a name="get-the-library"></a>Ismerkedés a tárban

A tárban, amely tartalmazza az új osztályok, és kibővíti a [CloudJob] szerezhet[ net_cloudjob] és [CloudTask] [ net_cloudtask] új módszerek a [NuGet]az osztályok[nuget_package]. Adhat a Visual Studio project az [NuGet tár csomag Manager][nuget_manager].

>[AZURE.TIP] A [Forráskód] talál[ github_file_conventions] az Azure köteg fájl szabályok tárának GitHub a Microsoft Azure SDK .NET-tárhoz.

## <a name="requirement-linked-storage-account"></a>Kötelező: csatolt tárterület-fiók

Kimeneti értékeket használja a fájl szabályok tár tartós tárolóhoz tárolja, és megtekintheti azokat az Azure-portálra, a köteg fiókjába [hivatkozás Azure tárterület-fiókkal](batch-application-packages.md#link-a-storage-account) kell. Ha még nem tette meg, a tárterület-fiók a köteg ügyfélhez csatolni a Azure portál használatával:

**Köteg fiók** lap > **Beállítások** > **Tároló fiók** > **Tárterület-fiókot** (nincs) > Válassza ki a tárterület-fiókot az előfizetés

A részletes segédlet csatolásáról egy tárterület-fiókkal a [alkalmazás környezet, amelyben az Azure köteg alkalmazáscsomagok](batch-application-packages.md)című témakör tartalmaz.

## <a name="persist-output"></a>Továbbra is fennáll a kimeneti

Nincsenek két elsődleges műveletek elvégzéséhez a feladatot, és a tevékenység kimeneti mentésekor a fájl szabályok tárral: a tárterület tároló létrehozása és mentése a kimeneti a tárolóba.

>[AZURE.WARNING] Az összes feladatot, és a tevékenység kimeneti értékeket az ugyanabban a tárolóban tárolja, mert [tároló szabályozásának korlátai](../storage/storage-performance-checklist.md#blobs) érvényesíteni lehet Ha nagyszámú feladatok próbál továbbra is fennáll a fájlok egy időben.

### <a name="create-storage-container"></a>Tárterület tároló létrehozása

Tárterület megőrzéséhez kimeneti a tevékenységek előtt, amelyhez azok tölti fel a kimeneti blob-tároló tároló kell létrehoznia. Ehhez a [CloudJob]hívásával[net_cloudjob]. [PrepareOutputStorageAsync] [net_prepareoutputasync]. Ez a módszer bővítmény megnyitja a [CloudStorageAccount] [ net_cloudstorageaccount] paraméterként objektum, és az Azure-portálra, és később a témakörben tárgyalt lekérés módszerek oly módon, hogy a jegyzetfüzetek oldalai találhatók nevű tároló hoz létre.

Ez a kód általában helyezze az ügyfélalkalmazás – az alkalmazás, amely a készletek, a feladatok és a feladatok hoz létre.

```csharp
CloudJob job = batchClient.JobOperations.CreateJob(
    "myJob",
    new PoolInformation { PoolId = "myPool" });

// Create reference to the linked Azure Storage account
CloudStorageAccount linkedStorageAccount =
    new CloudStorageAccount(myCredentials, true);

// Create the blob storage container for the outputs
await job.PrepareOutputStorageAsync(linkedStorageAccount);
```

### <a name="store-task-outputs"></a>Tevékenység kimeneti értékeket tárolása

Most, hogy a tároló blob-tárolóban lévő kész feladatok kimeneti a mentheti a tárolót a [TaskOutputStorage] használatával[ net_taskoutputstorage] osztály a fájl szabályok tárban található.

A tevékenység kódban először hozzon létre egy [TaskOutputStorage] [ net_taskoutputstorage] objektumot, majd a tevékenység befejezése a munkahelyi hívása a [TaskOutputStorage][net_taskoutputstorage]. [SaveAsync] [net_saveasync] az eredményt mentse az Azure-tárolóhoz módszert.

```csharp
CloudStorageAccount linkedStorageAccount = new CloudStorageAccount(myCredentials);
string jobId = Environment.GetEnvironmentVariable("AZ_BATCH_JOB_ID");
string taskId = Environment.GetEnvironmentVariable("AZ_BATCH_TASK_ID");

TaskOutputStorage taskOutputStorage = new TaskOutputStorage(
    linkedStorageAccount, jobId, taskId);

/* Code to process data and produce output file(s) */

await taskOutputStorage.SaveAsync(TaskOutputKind.TaskOutput, "frame_full_res.jpg");
await taskOutputStorage.SaveAsync(TaskOutputKind.TaskPreview, "frame_low_res.jpg");
```

A "típusú kimeneti" paraméter kategorizálja az állandó fájlokat. Vannak a négy előre [TaskOutputKind] [ net_taskoutputkind] típusok: "TaskOutput", "TaskPreview", "TaskLog" és "TaskIntermediate." Megadhatja az egyéni típusokat, illetve, ha az azok az munkafolyamatokban segítségére lehetnek.

Ezek a kimeneti típusok adja meg, hogy milyen típusú kimeneti értékeket felsoroló, előfordulhat, hogy később köteg az állandó a kimeneti értékeket, egy adott tevékenység teszi lehetővé. Ez azt jelenti, amikor a kimeneti értékeket a tevékenység listában a listáját szűrheti a kimeneti típusok egyik. Például ", adja meg a *kép* eredménye a tevékenység *109*." Több bejegyzésére és a kimeneti értékeket lekérését jelenik meg a [kimeneti beolvasása](#retrieve-output) a cikk későbbi részében.

>[AZURE.TIP] Kimeneti milyen jelöl ki is, ahol az Azure-portálon egy adott fájl jelenik meg: *TaskOutput*-kategorizált fájlok jelennek meg a "Tevékenység kimeneti fájlok", és *TaskLog* fájlok jelennek meg a "Tevékenység naplókban."

### <a name="store-job-outputs"></a>Feladat kimeneti értékeket tárolása

Mellett tárolása tevékenység kimeneti értékeket, a kimeneti értékeket egy teljes projekt társított tárolhat. Például az egyesítés a feladatban film megjelenítését feladat, sikerült áll fenn a teljes mértékben leképezett film egy feladat kimenet. A feladat befejezése után az ügyfélalkalmazás is egyszerűen lista és a beolvasni a kimeneti értékeket a feladathoz és, nem szükséges az egyes feladatok lekérdezéséhez.

Feladat kimeneti tárolása hívja fel a [JobOutputStorage][net_joboutputstorage]. [SaveAsync] [net_joboutputstorage_saveasync] módszer, és adja meg a [JobOutputKind] [ net_joboutputkind] és a fájlnév:

```
CloudJob job = await batchClient.JobOperations.GetJobAsync(jobId);
JobOutputStorage jobOutputStorage = job.OutputStorage(linkedStorageAccount);

await jobOutputStorage.SaveAsync(JobOutputKind.JobOutput, "mymovie.mp4");
await jobOutputStorage.SaveAsync(JobOutputKind.JobPreview, "mymovie_preview.mp4");
```

A tevékenység kimeneti értékeket TaskOutputKind, hogy a [JobOutputKind] használatával,[ net_joboutputkind] paraméter egy feladat kategorizálásához adatait a tárolt fájlokat. A paraméter tesz lehetővé, újabb Query (lista) kimeneti egy bizonyos típusú. A JobOutputKind kimeneti és a kép tartalmazza, és egyéni típusok létrehozását támogatja.

### <a name="store-task-logs"></a>Tevékenység naplók tárolása

Nemcsak a pályától tartós tartós tárolóhoz egy fájlt, egy feladat vagy projekt befejezésekor, előfordulhat, hogy megtalálta szükséges marad meg, hogy mikor frissüljenek a tevékenység – naplófájlok végrehajtása során fájlokat vagy `stdout.txt` és `stderr.txt`, például. Erre a célra az Azure köteg fájl szabályok tár biztosít a [TaskOutputStorage][net_taskoutputstorage]. [SaveTrackedAsync] [net_savetrackedasync] módot. A [SaveTrackedAsync][net_savetrackedasync], a csomóponton (időközönként megadott) fájl változásainak nyomon követése és a marad meg azokat a frissítéseket Azure tárolóhoz.

A következő kódrészletet használjuk [SaveTrackedAsync] [ net_savetrackedasync] frissítése `stdout.txt` a tevékenység végrehajtása során 15 másodpercenként Azure-tárolóban lévő:

```csharp
TimeSpan stdoutFlushDelay = TimeSpan.FromSeconds(3);
string logFilePath = Path.Combine(
    Environment.GetEnvironmentVariable("AZ_BATCH_TASK_DIR"), "stdout.txt");

// The primary task logic is wrapped in a using statement that sends updates to
// the stdout.txt blob in Storage every 15 seconds while the task code runs.
using (ITrackedSaveOperation stdout =
        await taskStorage.SaveTrackedAsync(
        TaskOutputKind.TaskLog,
        logFilePath,
        "stdout.txt",
        TimeSpan.FromSeconds(15)))
{
    /* Code to process data and produce output file(s) */

    // We are tracking the disk file to save our standard output, but the
    // node agent may take up to 3 seconds to flush the stdout stream to
    // disk. So give the file a moment to catch up.
    await Task.Delay(stdoutFlushDelay);
}
```

`Code to process data and produce output file(s)`helyőrzője egyszerűen a kódot, amely a feladat általában kell elvégeznie. Ha például lehet, hogy kód, amely adatok letöltések Azure-tárhelyről és hajt végre átalakítása vagy számítási rajta. A kódtöredék fontos része van igazoló hogyan is van lehetőség a ilyen kód egy `using` letiltás rendszeres időközönként frissítse fájl [SaveTrackedAsync][net_savetrackedasync].

A `Task.Delay` végén található ez szükséges `using` letiltás gondoskodjon arról, hogy a csomópont agent ürítse ki a normál kimenő tartalmát a stdout.txt fájlt (a csomópont agent a program, hogy minden egyes készletben csomóponton elindul, és a csomóponton és a köteg szolgáltatás között a parancs és a vezérlő felhasználói felületet nyújt) a csomóponton időt. Ez a késedelem nélkül ajánlatos az utolsó néhány másodpercet kimeneti kihagy. A késleltetés nem feltétlenül szükséges az összes fájlt.

>[AZURE.NOTE] Ha engedélyezi a követési SaveTrackedAsync fájl, csak *hozzáfűzi* a nyomon követett fájl vannak állandó Azure tárolóhoz. Ez a módszer csak a követés naplófájlok nem elforgatása vagy más, amelyek a program hozzáfűzi, azaz, az adatok csak bekerül a fájl végére frissítése esetén használható.

## <a name="retrieve-output"></a>Kimeneti beolvasása

Amikor a használja az Azure köteg fájl szabályok tár állandó kimeneti, teheti meg egy tevékenység és feladat-kötődnek módon. A kimenet kérhet adott tevékenység vagy anélkül, hogy az elérési út blob tároló vagy akár a fájl nevét, hogy feladatot. Ha egyszerűen kimondja, ", adja meg a tevékenység *109*kimeneti fájljai."

A következő kódrészletet függvény az összes olyan projektfeladatok, nyomtatja ki a kimenet fájlokat a tevékenység adatait, és letölti tárhelyről annak fájljait.

```csharp
foreach (CloudTask task in myJob.ListTasks())
{
    foreach (TaskOutputStorage output in
        task.OutputStorage(storageAccount).ListOutputs(
            TaskOutputKind.TaskOutput))
    {
        Console.WriteLine($"output file: {output.FilePath}");

        output.DownloadToFileAsync(
            $"{jobId}-{output.FilePath}",
            System.IO.FileMode.Create).Wait();
    }
}
```

## <a name="task-outputs-and-the-azure-portal"></a>Tevékenység kimeneti értékeket és az Azure-portálra

Az Azure portál jelenít meg a tevékenység kimeneti értékeket és naplók, hogy a rendszer egy csatolt Azure tárterület-fiókját az [Azure köteg fájl szabályok – fontos fájl]található elnevezési szabályokat[github_file_conventions_readme]. Alkalmazhat szabályoknak saját maga a kiválasztott nyelven, vagy használhatja a fájl szabályok tárat a .NET-alkalmazásokban.

### <a name="enable-portal-display"></a>Portál megjelenítendő engedélyezése

Ha engedélyezni szeretné a kimeneti értékeket a portálon megjelenítését, meg kell felelniük az alábbi követelmények:

 1. [Hivatkozás: Azure tároló fiók](#requirement-linked-storage-account) a köteg-fiókjába.
 2. Ha pályától tartós kimeneti értékeket igazodjon az előre definiált elnevezési szabályai tároló és a fájlokat. A következő szabályok meghatározását megtalálhatja a fájl szabályok tárban [– Fontos fájl][github_file_conventions_readme]. Ha használja-e az [Azure köteg fájl szabályok] [ nuget_package] tár áll fenn a kimeneti, ez a követelmény teljesül.

### <a name="view-outputs-in-the-portal"></a>Nézet kimeneti értékeket a portálon

Az Azure-portálon tevékenység kimeneti értékeket és a naplók megtekintéséhez nyissa meg a tevékenység kimenete érdeklődését, majd kattintson a **mentett kimeneti fájlok** vagy a **Naplók mentése**gombra. Ezen a képen látható, a **mentett kimeneti fájl** a feladat "007" azonosítójú:

![Tevékenység kimeneti értékeket lap az Azure-portálon][2]

## <a name="code-sample"></a>Példa a kódot.

A [PersistOutputs] [ github_persistoutputs] minta projekt egyike [Azure köteg mintakódok] [ github_samples] a GitHub. A Visual Studio 2015 megoldás bemutatja, hogyan használható az Azure köteg fájl szabályok könyvtár továbbra is fennáll a tevékenység kimeneti tartós tárolóhoz. A minta futtatásához tegye a következőket:

1. Nyissa meg a projekt a **Visual Studio 2015**.
2. Adja hozzá a köteget, és a tárhely **fiók hitelesítő adatait** a Microsoft.Azure.Batch.Samples.Common projekt **AccountSettings.settings** .
3. **Összeállítása** (de ne futtasson) a megoldást. Ha a rendszer kéri, visszaállíthatja az bármely NuGet csomagok.
4. Az Azure portal segítségével egy [Alkalmazáscsomag](batch-application-packages.md) feltöltése a **PersistOutputsTask**. Tartalmazza a `PersistOutputsTask.exe` és a .zip csomagban, a függő összeállítások adja meg az alkalmazás azonosítója "PersistOutputsTask" és "1,0", az alkalmazás csomag verziójával.
5. **Indítása** Projekt (Futtatás) a **PersistOutputs** .

## <a name="next-steps"></a>Következő lépések

### <a name="application-deployment"></a>Alkalmazások telepítésének

Az [alkalmazáscsomagok](batch-application-packages.md) szolgáltatás köteg egyszerűvé telepíteni őket, és az alkalmazások, a feladatok végrehajtása a csomópontok kiszámítania verziót.

### <a name="installing-applications-and-staging-data"></a>Alkalmazások telepítése, és átmeneti adatokat

Nézze meg az [alkalmazások telepítése, és a köteg az átmeneti tárolásra szolgáló adatok kiszámítása csomópontok] [ forum_post] kérdések feltevése: különböző módszerek az csomópontok előkészítése futó feladatok áttekintése az Azure köteg fórum. Az Azure köteg csapattagok egyike által írt, ebben a bejegyzésben (beleértve az alkalmazások és a tevékenység bemeneti adatok) fájlok különböző módokon a jó alapozó a számítási csomópontot, és néhány szempontot mindegyik módszernek az alakzatot.

[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[github_file_conventions]: https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/Batch/FileConventions
[github_file_conventions_readme]: https://github.com/Azure/azure-sdk-for-net/blob/AutoRest/src/Batch/FileConventions/README.md
[github_persistoutputs]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/PersistOutputs
[github_samples]: https://github.com/Azure/azure-batch-samples
[net_batchclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_cloudstorageaccount]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.cloudstorageaccount.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_fileconventions_readme]: https://github.com/Azure/azure-sdk-for-net/blob/AutoRest/src/Batch/FileConventions/README.md
[net_joboutputkind]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputkind.aspx
[net_joboutputstorage]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputstorage.aspx
[net_joboutputstorage_saveasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputstorage.saveasync.aspx
[net_msdn]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_prepareoutputasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.cloudjobextensions.prepareoutputstorageasync.aspx
[net_saveasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.saveasync.aspx
[net_savetrackedasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.savetrackedasync.aspx
[net_taskoutputkind]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputkind.aspx
[net_taskoutputstorage]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.aspx
[nuget_manager]: https://docs.nuget.org/consume/installing-nuget
[nuget_package]: https://www.nuget.org/packages/Microsoft.Azure.Batch.Conventions.Files
[portal]: https://portal.azure.com
[storage_explorer]: http://storageexplorer.com/

[1]: ./media/batch-task-output/task-output-01.png "A kimeneti fájl mentése és mentett választók naplózza a portálon"
[2]: ./media/batch-task-output/task-output-02.png "Tevékenység kimeneti értékeket lap az Azure-portálon"