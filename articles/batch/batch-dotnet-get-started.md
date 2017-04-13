<properties
    pageTitle="Oktatóanyag – első lépések az Azure köteg .NET tár |} Microsoft Azure"
    description="Ismerje meg, hogy az Azure köteget, és a köteg szolgáltatás, és egy példa forgatókönyv fejlesztésével alapfogalmak."
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-compute"
    ms.date="08/15/2016"
    ms.author="marsma"/>

# <a name="get-started-with-the-azure-batch-library-for-net"></a>A köteg Azure tár .NET használatának első lépései

> [AZURE.SELECTOR]
- [.NET](batch-dotnet-get-started.md)
- [Python](batch-python-tutorial.md)

[Azure] köteg alapjai[ azure_batch] és a [Köteg .NET] [ net_api] Ez a cikk a tárban, a C# minta alkalmazások lépésenkénti témákat ölelik fel. Hogyan minta alkalmazás használja a köteg szolgáltatás feldolgozása a párhuzamos terhelést a felhőben, valamint miként, hogyan kommunikáljon a [Azure tároló](../storage/storage-introduction.md) fájl előkészítése és lekérés áttekintjük. Megtudhatja, hogy a közös köteg alkalmazás munkafolyamat technikákat alkalmaz. Fogja a fő összetevői köteg, feladatok, feladatok, készletek, például egy alap megértése nyereség, és csomópontok számítja ki.

![A köteg megoldás munkafolyamat (basic)][11]<br/>

## <a name="prerequisites"></a>Előfeltételek

Ez a cikk tartalma feltételezi, hogy rendelkezik-e a és C# Visual Studio ismeretek. Is feltételezi, hogy, hogy meg tudja megfelelnek a fiók létrehozását megadott alatti Azure és a köteg- és szolgáltatások.

### <a name="accounts"></a>Fiókok

- **Azure-fiók**: Ha még nem rendelkezik az Azure előfizetéssel, [ingyenes Azure-fiók létrehozása][azure_free_account].
- **Köteg fiók**: Miután Azure szóló előfizetés, [Hozzon létre egy köteg Azure-fiókot](batch-account-create-portal.md).
- **Tárterület-fiók**: [tároló fiók létrehozása](../storage/storage-create-storage-account.md#create-a-storage-account) az [Azure kapcsolatban tároló fiókok](../storage/storage-create-storage-account.md)című témakör tartalmaz.

> [AZURE.IMPORTANT] Köteg jelenleg támogatott *csak* az **Általános célú** tárterület-fiók típusára, ahogy lépés #5 [tároló fiók létrehozása](../storage/storage-create-storage-account.md#create-a-storage-account) az [Azure kapcsolatban tároló fiókok](../storage/storage-create-storage-account.md).

### <a name="visual-studio"></a>Visual Studio

**Visual Studio 2015** össze a minta projekt kell rendelkeznie. A [Visual Studio 2015 termékek áttekintése]a Visual Studio ingyenes és a próba-verziók található[visual_studio].

### <a name="dotnettutorial-code-sample"></a>*DotNetTutorial* kód minta

A [DotNetTutorial] [ github_dotnettutorial] minta egyike a sok mintakódok az [azure-köteg-minták] talált[ github_samples] GitHub tárházából. A kezdőlap folyamattárház **Letöltése ZIP-** gombjára, vagy az [azure-köteg-minták-master.zip] kattintva letöltheti a minta[ github_samples_zip] közvetlen letöltési hivatkozásra. A ZIP-fájl tartalmának már kibontása, ha a megoldás a következő mappában találja:

`\azure-batch-samples\CSharp\ArticleProjects\DotNetTutorial`

### <a name="azure-batch-explorer-optional"></a>Azure köteg Explorer (nem kötelező)

Az [Azure köteg Explorer] [ github_batchexplorer] egy ingyenes segédprogram, amelyben szerepel [a minták köteg azure] [ github_samples] GitHub tárházából. Miközben nem ebben az oktatóanyagban befejezéséhez szükséges, célszerű lehet fejlesztése, és a köteg megoldások hibakeresése során.

## <a name="dotnettutorial-sample-project-overview"></a>DotNetTutorial minta projektek áttekintése

A *DotNetTutorial* kód minta, amely a két projektet tartalmaz Visual Studio 2015 megoldást: **DotNetTutorial** és **TaskApplication**.

- **DotNetTutorial** az ügyfélalkalmazás a párhuzamos terhelési végrehajtani a köteget, és a tárhely szolgáltatások szoftverekkel csomópontok (virtuális gépeken futó) számítja ki. A helyi számítógépen DotNetTutorial fut.

- **TaskApplication** , a program, amelyet futtat a számítási csomópontok a tényleges munka elvégzéséhez Azure-ban. A minta `TaskApplication.exe` elemzi a fájlban a szöveg töltöttem le az Azure-tárhelyről (a bemeneti fájl). Kattintson a felső három szavakat, amelyek akkor jelennek meg a bemeneti fájl listáját tartalmazó szövegfájl (a kimeneti fájl) elő. Után a kimeneti fájl létrehozott, TaskApplication Azure tárolóhoz feltölti a fájlt. Ez számára elérhetővé teszi azt az ügyfélalkalmazás letöltésre. TaskApplication párhuzamosan futtat, a több számítási csomópontok a köteg szolgáltatásban.

Az alábbi ábra mutatja be, az elsődleges, a ügyfélalkalmazás, *DotNetTutorial*és az alkalmazás által a feladatokat, *TaskApplication*végrehajtható végrehajtott műveletek. Egyszerű munkafolyamathoz sok számítási megoldások köteg eszközzel létrehozott jellemző. Miközben meg nem bemutatják, minden szolgáltatás érhető el a köteget szolgáltatásban, szinte minden köteg forgatókönyv hasonló folyamatok.

![Példa a munkafolyamat köteg][8]<br/>

[**Lépés: 1.**](#step-1-create-storage-containers) **Tárolók** létrehozása az Azure Blob-tárolóhoz.<br/>
[**Lépés: 2.**](#step-2-upload-task-application-and-data-files) Tárolók tevékenység alkalmazás és a bemeneti fájlokat feltölteni.<br/>
[**3 a lépést.**](#step-3-create-batch-pool) A köteg **készlet**létrehozása.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**3a.** A készlet **StartTask** a tevékenység bináris fájlokat (TaskApplication) letölti a csomópontok, ahogy azok a csatlakozás a készlet.<br/>
[**Lépés: 4.**](#step-4-create-batch-job) Hozzon létre egy köteg **feladatot**.<br/>
[**5 a lépést.**](#step-5-add-tasks-to-job) **Tevékenységek** hozzáadása a projekthez.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**5a.** A feladatokat végrehajtani a csomópontok van ütemezve.<br/>
    &nbsp;&nbsp;&nbsp;&nbsp;**5b.** Minden tevékenység letölti a bemeneti adatok Azure-tárhelyről, majd a végrehajtás megkezdése.<br/>
[**6 a lépést.**](#step-6-monitor-tasks) Lync-feladatokat.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**6a.** Feladatok elvégzettként azok töltse fel a kimeneti adatai Azure tároló.<br/>
[**Lépés 7.**](#step-7-download-task-output) Töltse le a tevékenység kimeneti tároló.

Említett, nem minden köteg megoldás hajt végre az alábbi pontos lépéseket, és sok egyebet tartalmazhat, de az *DotNetTutorial* minta alkalmazás bemutatja a köteg megoldást található általános folyamat.

## <a name="build-the-dotnettutorial-sample-project"></a>Hozza létre a *DotNetTutorial* minta projekt

A minta sikeres futtatásához, meg kell adnia köteg és a tárhely fiók hitelesítő adatait a *DotNetTutorial* projekt `Program.cs` fájlt. Ha még nem tette meg, nyissa meg azt a megoldást a Visual Studióban duplán kattintva a `DotNetTutorial.sln` megoldásfájlt. Vagy megnyitni a Visual Studio segítségével a **Fájl > megnyitása > Projekt/megoldás** menü.

Nyissa meg `Program.cs` az *DotNetTutorial* projektben. Ezután adja a hitelesítő adatok megadott felső részén a fájlt:

```
// Update the Batch and Storage account credential strings below with the values
// unique to your accounts. These are used when constructing connection strings
// for the Batch and Storage client objects.

// Batch account credentials
private const string BatchAccountName = "";
private const string BatchAccountKey  = "";
private const string BatchAccountUrl  = "";

// Storage account credentials
private const string StorageAccountName = "";
private const string StorageAccountKey  = "";
```

> [AZURE.IMPORTANT] Amint már említettük, meg kell adnia egy **Általános célú** tároló fiók hitelesítő adatait jelenleg Azure tároló. A köteg alkalmazások használata az **Általános célú** tároló számla belüli blob-tárolóhoz. A hitelesítő adatokat tároló fiókot az *Blob-tárolóhoz* fiók kiválasztása által létrehozott nem ad meg.

A köteg- és fiók hitelesítő adatait belül az egyes szolgáltatásokhoz a fiók lap a az [Azure portálon]található[azure_portal]:

![A portál hitelesítő adatok köteg][9]
![tároló hitelesítő adatokat a portálon][10]<br/>

Most, hogy a rendszer a projekt a hitelesítő adatokkal, kattintson a jobb gombbal a megoldást Intézőben a megoldást, és kattintson a **Szerkesztés megoldás**. A visszaállítás bármely NuGet csomagok, győződjön meg arról, ha a rendszer kéri.

> [AZURE.TIP] Ha a NuGet csomagok automatikusan nem visszaállítani, vagy ha megjelenik a csomagok visszaállítása hiba kapcsolatos hibák, ellenőrizze, hogy a [NuGet csomag Manager] [ nuget_packagemgr] telepítve. Hiányzó csomagok letöltése majd engedélyezze. Lásd: a [Engedélyezése csomag visszaállítása során létre] [ nuget_restore] ahhoz, hogy csomag letöltése.

Az alábbi szakaszok azt lebontva feldolgozása a terhelést a köteg szolgáltatásban végrehajtott lépések a minta alkalmazást, és ezek a lépések részletesen ismertetik. Javasoljuk, hogy a megnyitott megoldást a Visual Studióban hivatkoznak, mivel nem minden sort a mintában tárgyalt Ez a cikk a többi alkalmazással munka közben.

Nyissa meg azt a felső részén a `MainAsync` módszer a *DotNetTutorial* projekt `Program.cs` fájl lépés 1 kezdődik. Minden egyes lépés az alábbi, majd a nagyjából követi haladását módszer hívásai a `MainAsync`.

## <a name="step-1-create-storage-containers"></a>Lépés: 1: A tároló létrehozása

![Tárolók létrehozása az Azure-tárolóhoz][1]
<br/>

Köteg Azure tároló személlyel beépített támogatja. Tárolók a tárterület-fiókjában nyújt a fájlokat, szükség szerint a köteg fiókjában futó feladatokat. A tárolók is biztosít, amelyek a feladatok eredményeznek kimeneti adatai tárolhatja. A legfontosabb dolog a *DotNetTutorial* ügyfélalkalmazás kerül a következő három tárolók létrehozása az [Azure Blob-tárolóhoz](../storage/storage-introduction.md):

- **alkalmazás**: Ez a tároló tárolja az alkalmazás futtatásához a feladatokat, valamint a függőségeket, például a DLL-ek közül.
- **beviteli**: tevékenységek letölti a *bemeneti* tárolóból feldolgozása adatfájlok.
- **Kimenet**: tevékenységek beviteli feldolgozása elvégezni, ha azok feltöltődnek az eredmények a *kimeneti* tároló.

A tárterület-fiók használata és a tárolók létrehozása az [Azure tároló ügyfél-tár .NET]használjuk[net_api_storage]. Azt a fiókot hivatkozás létrehozása a [CloudStorageAccount][net_cloudstorageaccount], és hogy hozzon létre egy [CloudBlobClient][net_cloudblobclient]:

```csharp
// Construct the Storage account connection string
string storageConnectionString = String.Format(
    "DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}",
    StorageAccountName,
    StorageAccountKey);

// Retrieve the storage account
CloudStorageAccount storageAccount =
    CloudStorageAccount.Parse(storageConnectionString);

// Create the blob client, for use in obtaining references to
// blob storage containers
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
```

Használja a `blobClient` az alkalmazáson keresztül hivatkozásra, és paraméterként átadni többféle módszer áll rendelkezésére. Példa erre az azonnal követi a fenti, ahol hívása kód blokk `CreateContainerIfNotExistAsync` a tárolók ténylegesen létrehozásához.

```csharp
// Use the blob client to create the containers in Azure Storage if they don't
// yet exist
const string appContainerName    = "application";
const string inputContainerName  = "input";
const string outputContainerName = "output";
await CreateContainerIfNotExistAsync(blobClient, appContainerName);
await CreateContainerIfNotExistAsync(blobClient, inputContainerName);
await CreateContainerIfNotExistAsync(blobClient, outputContainerName);
```

```csharp
private static async Task CreateContainerIfNotExistAsync(
    CloudBlobClient blobClient,
    string containerName)
{
        CloudBlobContainer container =
            blobClient.GetContainerReference(containerName);

        if (await container.CreateIfNotExistsAsync())
        {
                Console.WriteLine("Container [{0}] created.", containerName);
        }
        else
        {
                Console.WriteLine("Container [{0}] exists, skipping creation.",
                    containerName);
        }
}
```

A tárolók létrehozása után az alkalmazás letöltése feltöltheti a fájlokat, a feladatok által használt.

> [AZURE.TIP] [A .NET Blob-tárolóhoz használata](../storage/storage-dotnet-how-to-use-blobs.md) áttekintést kaphat jó Azure tároló és BLOB használata. Kell a olvasási lista tetején köteg együtt kezd dolgozni.

## <a name="step-2-upload-task-application-and-data-files"></a>Lépés: 2: A tevékenység alkalmazás és az adatok fájlok feltöltése

![Tárolók tevékenység alkalmazás és a beviteli (adatok) fájlok feltöltése][2]
<br/>

A fájl feltöltése művelet *DotNetTutorial* először határozza meg az **alkalmazás** és a **bemeneti** fájl elérési útja gyűjtemények léteznek a helyi számítógépen. Majd a tárolók az előző lépésben létrehozott feltölti meg ezeket a fájlokat.

```
// Paths to the executable and its dependencies that will be executed by the tasks
List<string> applicationFilePaths = new List<string>
{
    // The DotNetTutorial project includes a project reference to TaskApplication,
    // allowing us to determine the path of the task application binary dynamically
    typeof(TaskApplication.Program).Assembly.Location,
    "Microsoft.WindowsAzure.Storage.dll"
};

// The collection of data files that are to be processed by the tasks
List<string> inputFilePaths = new List<string>
{
    @"..\..\taskdata1.txt",
    @"..\..\taskdata2.txt",
    @"..\..\taskdata3.txt"
};

// Upload the application and its dependencies to Azure Storage. This is the
// application that will process the data files, and will be executed by each
// of the tasks on the compute nodes.
List<ResourceFile> applicationFiles = await UploadFilesToContainerAsync(
    blobClient,
    appContainerName,
    applicationFilePaths);

// Upload the data files. This is the data that will be processed by each of
// the tasks that are executed on the compute nodes within the pool.
List<ResourceFile> inputFiles = await UploadFilesToContainerAsync(
    blobClient,
    inputContainerName,
    inputFilePaths);
```

A két módszer `Program.cs` , amely a feltöltés folyamat játszik szerepet:

- `UploadFilesToContainerAsync`: Ez a módszer [ResourceFile] gyűjteményének beolvasása[ net_resourcefile] objektumok (lásd alább), és belső hívások `UploadFileToContainerAsync` a *filePaths* paraméterben átadott minden fájl feltöltéséhez.
- `UploadFileToContainerAsync`: Ez a ténylegesen a többi fájlfeltöltéshez hajt végre, és a [ResourceFile] hoz létre a módszer[ net_resourcefile] objektumok. A feltöltést követően azt beolvassa a fájlt a megosztott access aláírás (Társítások), és egy ResourceFile-objektum, amely azt adja eredményül. Az access aláírások is itt tárgyalt megosztott.

```
private static async Task<ResourceFile> UploadFileToContainerAsync(
    CloudBlobClient blobClient,
    string containerName,
    string filePath)
{
        Console.WriteLine(
            "Uploading file {0} to container [{1}]...", filePath, containerName);

        string blobName = Path.GetFileName(filePath);

        CloudBlobContainer container = blobClient.GetContainerReference(containerName);
        CloudBlockBlob blobData = container.GetBlockBlobReference(blobName);
        await blobData.UploadFromFileAsync(filePath, FileMode.Open);

        // Set the expiry time and permissions for the blob shared access signature.
        // In this case, no start time is specified, so the shared access signature
        // becomes valid immediately
        SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy
        {
                SharedAccessExpiryTime = DateTime.UtcNow.AddHours(2),
                Permissions = SharedAccessBlobPermissions.Read
        };

        // Construct the SAS URL for blob
        string sasBlobToken = blobData.GetSharedAccessSignature(sasConstraints);
        string blobSasUri = String.Format("{0}{1}", blobData.Uri, sasBlobToken);

        return new ResourceFile(blobSasUri, blobName);
}
```

### <a name="resourcefiles"></a>ResourceFiles

Egy [ResourceFile] [ net_resourcefile] tartalmaz a feladatok a köteg letölti-e egy számítási csomópontot, hogy a feladat futtatása előtt Azure-tárolóban lévő fájl URL-címét. A [ResourceFile.BlobSource] [ net_resourcefile_blobsource] tulajdonság adja meg a teljes URL-CÍMÉT a fájl Azure-tárolóban lévő található. Az URL-címet is, ahol a fájl biztonságos hozzáférés átengedése aláírás (Társítások). A legtöbb feladatok belül köteg .NET típusai *ResourceFiles* tulajdonságait, például:

- [CloudTask][net_task]
- [StartTask][net_pool_starttask]
- [JobPreparationTask][net_jobpreptask]
- [JobReleaseTask][net_jobreltask]

A minta DotNetTutorial alkalmazás nem használja az JobPreparationTask vagy JobReleaseTask tevékenységtípusok, de több erről a [Futtatás előkészítése és a Befejezés projektfeladatok Azure köteg kiszámítania csomópontok](batch-job-prep-release.md)róluk.

### <a name="shared-access-signature-sas"></a>Megosztott access aláírás (Társítások)

Aláírások átengedése karakterláncok, amelyek – ha az URL-CÍMEK részét képező – tárolók és Azure-tárolóban lévő BLOB biztonságos hozzáférés nyújtása. Az alkalmazás használ blob- és a megosztott tároló DotNetTutorial aláírás URL-címek érhet el és bemutatja, hogyan ezek átengedése aláírás karakterláncok beszerzése a tárhely szolgáltatásból.

- **A megosztott access aláírások BLOB**: A készlet StartTask DotNetTutorial a megosztott blob access aláírásokat használ, amikor az alkalmazás bináris és a bemeneti adatok fájlok letölti tárhelyről (lásd az alábbi lépést #3). A `UploadFileToContainerAsync` DotNetTutorial's módszer `Program.cs` , amely beolvassa az egyes blob átengedése aláírás kódot tartalmazza. Azt is így tesz, hívja fel a [CloudBlob.GetSharedAccessSignature][net_sas_blob].

- **Container hozzáférési aláírások megosztott**: egyes tevékenységek befejezte a munkát, a számítási csomópontra, ahogy azt feltölti a kimeneti fájl a *kimeneti* tároló Azure-tárolóban lévő. Ehhez a TaskApplication használja, amely a tároló írási hozzáférést biztosít az elérési út részeként, amikor feltöltések megjelenítése az a fájlt tároló átengedése aláírás. A tároló átengedése aláírás beszerzése befejeződött, hasonló módon megosztásakor a blob beszerzése aláírás access. A DotNetTutorial, látni fogja, hogy a `GetContainerSasUrl` segítő módszer felhívja [CloudBlobContainer.GetSharedAccessSignature] [ net_sas_container] erre. Olvassa el kell arról, hogyan TaskApplication használja-e a tároló további megosztott access aláírás a "6 lépés: Monitor feladatok."

> [AZURE.TIP] Nézze meg a megosztott access aláírások, a két részből álló sorozat [rész 1: a megosztott access aláírás (Társítások) modell ismertetése](../storage/storage-dotnet-shared-access-signature-part-1.md) és [rész 2: létrehozása és használata (Társítások) átengedése aláírást tartalmazó Blob-tárolóhoz](../storage/storage-dotnet-shared-access-signature-part-2.md), ha többet szeretne tudni a tárterület-fiókjában adatok biztonságos hozzáférés biztosítása.

## <a name="step-3-create-batch-pool"></a>3 lépés: A köteg készlet létrehozása

![A köteg készlet létrehozása][3]
<br/>

A köteg **készlet** számítási csomópontok (virtuális gépeken futó), amelyen a köteg végrehajtja a egy projektfeladatok gyűjteménye.

Az alkalmazás és az adatok fájlokat a tárterület-fiókba feltöltések megjelenítése, miután az *DotNetTutorial* a köteg szolgáltatással a kapcsolati a köteg .NET tár indítása. Ehhez a [BatchClient] [ net_batchclient] első létrehozásakor:

```csharp
BatchSharedKeyCredentials cred = new BatchSharedKeyCredentials(
    BatchAccountUrl,
    BatchAccountName,
    BatchAccountKey);

using (BatchClient batchClient = BatchClient.Open(cred))
{
    ...
```

Ezután a hívást, hogy a köteg fiók számítási csomópontok erőforráskészlethez tartozik jön létre `CreatePoolAsync`. `CreatePoolAsync`használja a [BatchClient.PoolOperations.CreatePool] [ net_pool_create] módszer om, pedig a köteg szolgáltatásban a készlet létrehozása.

```csharp
private static async Task CreatePoolAsync(
    BatchClient batchClient,
    string poolId,
    IList<ResourceFile> resourceFiles)
{
    Console.WriteLine("Creating pool [{0}]...", poolId);

    // Create the unbound pool. Until we call CloudPool.Commit() or CommitAsync(),
    // no pool is actually created in the Batch service. This CloudPool instance is
    // therefore considered "unbound," and we can modify its properties.
    CloudPool pool = batchClient.PoolOperations.CreatePool(
            poolId: poolId,
            targetDedicated: 3,           // 3 compute nodes
            virtualMachineSize: "small",  // single-core, 1.75 GB memory, 224 GB disk
            cloudServiceConfiguration:
                new CloudServiceConfiguration(osFamily: "4")); // Win Server 2012 R2

    // Create and assign the StartTask that will be executed when compute nodes join
    // the pool. In this case, we copy the StartTask's resource files (that will be
    // automatically downloaded to the node by the StartTask) into the shared
    // directory that all tasks will have access to.
    pool.StartTask = new StartTask
    {
        // Specify a command line for the StartTask that copies the task application
        // files to the node's shared directory. Every compute node in a Batch pool
        // is configured with several pre-defined environment variables that you can
        // reference by using commands or applications run by tasks.

        // Since a successful execution of robocopy can return a non-zero exit code
        // (e.g. 1 when one or more files were successfully copied) we need to
        // manually exit with a 0 for Batch to recognize StartTask execution success.
        CommandLine = "cmd /c (robocopy %AZ_BATCH_TASK_WORKING_DIR% %AZ_BATCH_NODE_SHARED_DIR%) ^& IF %ERRORLEVEL% LEQ 1 exit 0",
        ResourceFiles = resourceFiles,
        WaitForSuccess = true
    };

    await pool.CommitAsync();
}
```

Ha hoz létre egy készlet [CreatePool][net_pool_create], paramétert több például számítási csomópontok, [a csomópontok méretének](../cloud-services/cloud-services-sizes-specs.md)és a csomópontok operációs rendszer számát. A *DotNetTutorial*, [CloudServiceConfiguration] használjuk[ net_cloudserviceconfiguration] megadhatja a Windows Server 2012 R2 [Felhőbeli](../cloud-services/cloud-services-guestos-update-matrix.md)szolgáltatásokkal. Egy [VirtualMachineConfiguration] megadásával azonban[ net_virtualmachineconfiguration] ehelyett hozhat létre piactér képekből létrehozott csomópontok készletek amely magában foglalja a Windows és a Linux képek – [rendelkezést Linux kiszámítania Azure köteg készletek található csomópontok](batch-linux-nodes.md) további tudnivalókat talál.

> [AZURE.IMPORTANT] A köteg számítási erőforrások az előfizetést terhelő. Költségek minimalizálásához alsó `targetDedicated` 1, a minta futtatása előtt.

Fizikai csomópont tulajdonságokból együtt is megadhat egy [StartTask] [ net_pool_starttask] gyűjtő. A StartTask minden csomóponton végrehajtja a csomópontra a készlet csatlakozik, és minden alkalommal, amikor újraindítja csomópontot. A StartTask esetén különösen hasznos alkalmazások telepítése számítási csomópontok feladatok végrehajtása előtt. Például a feladatok adatfeldolgozás Python parancsfájlok használatával, a StartTask a Python telepítése a számítási csomópontokat is használhatja.

A minta alkalmazásban, a StartTask másolja a tárhelyről letöltődő fájlok (amely a [StartTask]használatával megadott[net_starttask].[ ResourceFiles] [ net_starttask_resourcefiles] tulajdonság) a a StartTask munkakönyvtár a megosztott Directoryhoz, *a csomóponton futó feladatok* is rendelkeznek hozzáféréssel. Alapvetően, a program másolja `TaskApplication.exe` és az egyes csomópontok megosztott könyvtárában található függőségét, a csomópont csatlakozik a készlet, hogy a csomóponton futó egyetlen tevékenységet sem érhető el.

> [AZURE.TIP] Az **alkalmazáscsomagok** Azure köteg szolgáltatása úgy is kaphat az alakzatot a számítási csomópontok alkalmazás erőforráskészlethez tartozik. Lásd: [alkalmazás környezet, amelyben az Azure köteg alkalmazáscsomagok](batch-application-packages.md) további információt.

A fenti kódrészletet lényeges is a két környezetben változók használata a StartTask tulajdonságlapján a *parancssor* : `%AZ_BATCH_TASK_WORKING_DIR%` és `%AZ_BATCH_NODE_SHARED_DIR%`. Minden számítási csomópont belül egy köteg készlet automatikusan köteg jellemző környezet többváltozós van beállítva. Tevékenység végrehajtott folyamatának ezek a környezeti változók hozzáférése van.

> [AZURE.TIP] További tudnivalók a környezet számítási csomópontokat egy köteg készlet és a tevékenység munka könyvtárak, olvashat a rendelkezésre álló változók olvassa el a [környezeti beállítások tevékenységek](batch-api-basics.md#environment-settings-for-tasks) és a [fájlok és mappák](batch-api-basics.md#files-and-directories) csoport a [Köteg szolgáltatás áttekintése fejlesztők számára](batch-api-basics.md).

## <a name="step-4-create-batch-job"></a>Lépés: 4: Köteg feladat létrehozása

![Köteg feladat létrehozása][4]<br/>

A köteg **feladat** feladatok gyűjteménye, és a számítási csomópontok erőforráskészlethez tartozik egy társított. A kapcsolódó készlet számítási csomópontok hajtsa végre a projekt feladatait.

A feladat nemcsak rendszerezése és a kapcsolódó munkaterhelésekből, hanem olyan bizonyos korlátozások – például a feladat (és kiterjesztés, feladatainak) a maximális futtatókörnyezet feladat prioritás viszonyított köteg fiók más feladatok és a feladatok nyomon követése a is használhatja. Ebben a példában azonban a feladat társítva csak a készlet #3 lépésben létrehozott. Nincsenek további tulajdonság be van állítva.

Az összes köteg feladatok társítva adott erőforráskészlethez tartozik. A társítási azt jelzi, hogy mely a projektfeladatok hajtja végre a csomópontok. Adja meg a [CloudJob.PoolInformation] használatával[ net_job_poolinfo] tulajdonság, az alábbi kódrészletet látható módon.

```csharp
private static async Task CreateJobAsync(
    BatchClient batchClient,
    string jobId,
    string poolId)
{
    Console.WriteLine("Creating job [{0}]...", jobId);

    CloudJob job = batchClient.JobOperations.CreateJob();
    job.Id = jobId;
    job.PoolInformation = new PoolInformation { PoolId = poolId };

    await job.CommitAsync();
}
```

Most, hogy a feladat már elkészült, a feladatok megjelennek a vonhatnak.

## <a name="step-5-add-tasks-to-job"></a>Lépés 5: Tevékenységek hozzáadása a projekthez

![Tevékenységek hozzáadása a projekthez][5]<br/>
*(1) feladatok bekerülnek a feladatot, (2) a tevékenységek mikorra van ütemezve csomóponton és (3) a feladatokat, töltse le az adatfájlok feldolgozása*

**Feladatok** köteg az egyes mennyiségű munka, hajtsa végre a számítási csomópontok. A feladatot a parancssor és futtatja a parancsfájlok vagy adja meg, ha a parancssorban végrehajtható.

Ténylegesen vonhatnak, feladatok hozzá kell adnia egy feladatot. Minden egyes [CloudTask] [ net_task] állítható be a parancssori tulajdonság és [ResourceFiles] [ net_task_resourcefiles] (mint az erőforráskészlet StartTask), hogy a tevékenység letölti a csomópontra a parancssor automatikusan végrehajtása előtt. A *DotNetTutorial* minta projekt minden tevékenység végrehajtja a csak egy fájlt. ResourceFiles levétel így egyetlen elemet tartalmaz.

```csharp
private static async Task<List<CloudTask>> AddTasksAsync(
    BatchClient batchClient,
    string jobId,
    List<ResourceFile> inputFiles,
    string outputContainerSasUrl)
{
    Console.WriteLine("Adding {0} tasks to job [{1}]...", inputFiles.Count, jobId);

    // Create a collection to hold the tasks that we'll be adding to the job
    List<CloudTask> tasks = new List<CloudTask>();

    // Create each of the tasks. Because we copied the task application to the
    // node's shared directory with the pool's StartTask, we can access it via
    // the shared directory on the node that the task runs on.
    foreach (ResourceFile inputFile in inputFiles)
    {
        string taskId = "topNtask" + inputFiles.IndexOf(inputFile);
        string taskCommandLine = String.Format(
            "cmd /c %AZ_BATCH_NODE_SHARED_DIR%\\TaskApplication.exe {0} 3 \"{1}\"",
            inputFile.FilePath,
            outputContainerSasUrl);

        CloudTask task = new CloudTask(taskId, taskCommandLine);
        task.ResourceFiles = new List<ResourceFile> { inputFile };
        tasks.Add(task);
    }

    // Add the tasks as a collection, as opposed to issuing a separate AddTask call
    // for each. Bulk task submission helps to ensure efficient underlying API calls
    // to the Batch service.
    await batchClient.JobOperations.AddTaskAsync(jobId, tasks);

    return tasks;
}
```

> [AZURE.IMPORTANT] Amikor a környezeti változók hozzáférésük, például: `%AZ_BATCH_NODE_SHARED_DIR%` vagy egy alkalmazás nem található a csomópont végrehajtása `PATH`, feladat parancssorokat előtaggal együtt kell `cmd /c`. Ezzel kifejezetten hajtsa végre a parancs értelmező, és kérje meg, hogy megszünteti a parancs végrehajtása után. Ez a követelmény általában szükségtelen, ha a feladatok végrehajtása egy alkalmazást a csomópontra a `PATH` (például *robocopy.exe* vagy *powershell.exe*), és nincsenek környezet változók használnak.

Belül a `foreach` a fenti kódtöredék hurok, láthatja, hogy a parancssorból a tevékenységhez tartozó összeállítás, hogy az három parancssori argumentumok át a *TaskApplication.exe*:

1. Az **első argumentum** értéke a fájl elérési útját a feldolgozása. A csomóponton található az a helyi fájl elérési útját. Ha a ResourceFile objektum `UploadFileToContainerAsync` először létrehozott fent, a fájl neve használták a tulajdonság (a ResourceFile konstruktor paraméter). Ez azt jelzi, hogy a fájl *TaskApplication.exe*ugyanabban a mappában található.

2. A **második argumentumot** Megadja, hogy a legfelső *N* szavakat a kimeneti fájl kell írni. A minta ez van csomagolásukkor, hogy a felső három szó szerepel a kimeneti fájl kerülnek.

3. A **harmadik argumentum** értéke a megosztott access aláírás (Társítások), amely a **kimeneti** tároló Azure-tárolóban lévő írási hozzáférést biztosít. *TaskApplication.exe* használja a megosztott access aláírás URL-CÍMÉT, ha a Azure tárhely azt feltölti a kimeneti fájl. Ezt a megtalálhatja a kódot a `UploadFileToContainer` módszer a TaskApplication projekt `Program.cs` fájl:

```csharp
// NOTE: From project TaskApplication Program.cs

private static void UploadFileToContainer(string filePath, string containerSas)
{
        string blobName = Path.GetFileName(filePath);

        // Obtain a reference to the container using the SAS URI.
        CloudBlobContainer container = new CloudBlobContainer(new Uri(containerSas));

        // Upload the file (as a new blob) to the container
        try
        {
                CloudBlockBlob blob = container.GetBlockBlobReference(blobName);
                blob.UploadFromFile(filePath, FileMode.Open);

                Console.WriteLine("Write operation succeeded for SAS URL " + containerSas);
                Console.WriteLine();
        }
        catch (StorageException e)
        {

                Console.WriteLine("Write operation failed for SAS URL " + containerSas);
                Console.WriteLine("Additional error information: " + e.Message);
                Console.WriteLine();

                // Indicate that a failure has occurred so that when the Batch service
                // sets the CloudTask.ExecutionInformation.ExitCode for the task that
                // executed this application, it properly indicates that there was a
                // problem with the task.
                Environment.ExitCode = -1;
        }
}
```

## <a name="step-6-monitor-tasks"></a>Lépés a 6: Monitor feladatok

![Képernyő-feladatok][6]<br/>
*Az ügyfélalkalmazás (1) figyelése a feladatok befejezése és állapota success, majd a (2) a tevékenységek feltölteni az eredmény adatokat Azure-tárolóhoz*

Tevékenységek felvétele után egy projektbe, ezeket automatikusan aszinkron és ütemeznie meg a készlet a feladathoz kapcsolódó számítási található csomópontok. A megadott beállítások alapján, köteg kezeli az összes tevékenység queuing, ütemezés, újra próbálkozik és más tevékenység felügyeleti feladatokat meg. Vannak olyan sok megközelítés feladat-végrehajtás figyelése. DotNetTutorial egy egyszerű példa, hogy csak a befejezési és a tevékenység hiba vagy success jelentések államok jeleníti meg.

Belül a `MonitorTasks` DotNetTutorial's módszer `Program.cs`, vannak három köteg .NET fogalmak megértéséhez, hogy a vitafórum garantálja. Szerepelnek alatt a megjelenési sorrendjének:

1. **ODATADetailLevel**: [ODATADetailLevel] megadása[ net_odatadetaillevel] listában műveletek (például egy projektfeladatok partnerlistáját) elengedhetetlen köteg alkalmazás teljesítményének biztosítása. Hozzáadása [a köteg Azure szolgáltatás hatékony lekérdezés](batch-efficient-list-queries.md) olvasási listájához Ha módon figyelése a köteg jelennek állapota a rendezést.

2. **TaskStateMonitor**: [TaskStateMonitor] [ net_taskstatemonitor] lehetővé teszi a köteg .NET segítő segédprogramok tevékenység államok figyelemmel kísérésére. A `MonitorTasks`, *DotNetTutorial* megvárja, amíg minden tevékenység [TaskState.Completed] eléréséhez[ net_taskstate] határidőre. Kattintson a feladat szünteti meg.

3. **TerminateJobAsync**: [JobOperations.TerminateJobAsync] feladat megszakítása[ net_joboperations_terminatejob] (vagy a blokkolási JobOperations.TerminateJob) befejezettként jelöli meg, hogy a feladat. Nagyon fontos módja, ha a köteg megoldás használ egy [JobReleaseTask][net_jobreltask]. Ez a tevékenységhez, amelynek az [előkészítés és a Befejezés projektfeladatok](batch-job-prep-release.md)ismertetett speciális típusát.

A `MonitorTasks` származó *DotNetTutorial*mód `Program.cs` alatt jelenik meg:

```csharp
private static async Task<bool> MonitorTasks(
    BatchClient batchClient,
    string jobId,
    TimeSpan timeout)
{
    bool allTasksSuccessful = true;
    const string successMessage = "All tasks reached state Completed.";
    const string failureMessage = "One or more tasks failed to reach the Completed state within the timeout period.";

    // Obtain the collection of tasks currently managed by the job. Note that we use
    // a detail level to  specify that only the "id" property of each task should be
    // populated. Using a detail level for all list operations helps to lower
    // response time from the Batch service.
    ODATADetailLevel detail = new ODATADetailLevel(selectClause: "id");
    List<CloudTask> tasks =
        await batchClient.JobOperations.ListTasks(JobId, detail).ToListAsync();

    Console.WriteLine("Awaiting task completion, timeout in {0}...",
        timeout.ToString());

    // We use a TaskStateMonitor to monitor the state of our tasks. In this case, we
    // will wait for all tasks to reach the Completed state.
    TaskStateMonitor taskStateMonitor
        = batchClient.Utilities.CreateTaskStateMonitor();

    try
    {
        await taskStateMonitor.WhenAll(tasks, TaskState.Completed, timeout);
    }
    catch (TimeoutException)
    {
        await batchClient.JobOperations.TerminateJobAsync(jobId, failureMessage);
        Console.WriteLine(failureMessage);
        return false;
    }

    await batchClient.JobOperations.TerminateJobAsync(jobId, successMessage);

    // All tasks have reached the "Completed" state, however, this does not
    // guarantee all tasks completed successfully. Here we further check each task's
    // ExecutionInfo property to ensure that it did not encounter a scheduling error
    // or return a non-zero exit code.

    // Update the detail level to populate only the task id and executionInfo
    // properties. We refresh the tasks below, and need only this information for
    // each task.
    detail.SelectClause = "id, executionInfo";

    foreach (CloudTask task in tasks)
    {
        // Populate the task's properties with the latest info from the
        // Batch service
        await task.RefreshAsync(detail);

        if (task.ExecutionInformation.SchedulingError != null)
        {
            // A scheduling error indicates a problem starting the task on the node.
            // It is important to note that the task's state can be "Completed," yet
            // still have encountered a scheduling error.

            allTasksSuccessful = false;

            Console.WriteLine("WARNING: Task [{0}] encountered a scheduling error: {1}",
                task.Id,
                task.ExecutionInformation.SchedulingError.Message);
        }
        else if (task.ExecutionInformation.ExitCode != 0)
        {
            // A non-zero exit code may indicate that the application executed by
            // the task encountered an error during execution. As not every
            // application returns non-zero on failure by default (e.g. robocopy),
            // your implementation of error checking may differ from this example.

            allTasksSuccessful = false;

            Console.WriteLine("WARNING: Task [{0}] returned a non-zero exit code - this may indicate task execution or completion failure.", task.Id);
        }
    }

    if (allTasksSuccessful)
    {
        Console.WriteLine("Success! All tasks completed successfully within the specified timeout period.");
    }

    return allTasksSuccessful;
}
```

## <a name="step-7-download-task-output"></a>7 lépés: A tevékenység kimeneti letöltése

![Töltse le a tevékenység kimeneti tárhely][7]<br/>

Most, hogy a feladat befejezése után, a feladatok kimenetét letölthető Azure-tárhelyről. Ez a lépés hívást kezdeményez `DownloadBlobsFromContainerAsync` a *DotNetTutorial* `Program.cs`:

```csharp
private static async Task DownloadBlobsFromContainerAsync(
    CloudBlobClient blobClient,
    string containerName,
    string directoryPath)
{
        Console.WriteLine("Downloading all files from container [{0}]...", containerName);

        // Retrieve a reference to a previously created container
        CloudBlobContainer container = blobClient.GetContainerReference(containerName);

        // Get a flat listing of all the block blobs in the specified container
        foreach (IListBlobItem item in container.ListBlobs(
                    prefix: null,
                    useFlatBlobListing: true))
        {
                // Retrieve reference to the current blob
                CloudBlob blob = (CloudBlob)item;

                // Save blob contents to a file in the specified folder
                string localOutputFile = Path.Combine(directoryPath, blob.Name);
                await blob.DownloadToFileAsync(localOutputFile, FileMode.Create);
        }

        Console.WriteLine("All files downloaded to {0}", directoryPath);
}
```

> [AZURE.NOTE] A hívás `DownloadBlobsFromContainerAsync` a *DotNetTutorial* alkalmazás adja meg, hogy a fájlok kell letölt a `%TEMP%` mappát. Nyugodtan módosítása a kimenet helyét.

## <a name="step-8-delete-containers"></a>Lépés 8: Törlés tárolók

Az-előfizetést terhelő, amely az Azure-tárolóban lévő adatok, mert azt értéke mindig célszerű az eltávolítása bármely BLOB, amely már nincs szükség a köteg feladatokhoz. A cég DotNetTutorial `Program.cs`, ez a lépés a segítő módszer három hívásainak `DeleteContainerAsync`:

```csharp
// Clean up Storage resources
await DeleteContainerAsync(blobClient, appContainerName);
await DeleteContainerAsync(blobClient, inputContainerName);
await DeleteContainerAsync(blobClient, outputContainerName);
```

A módszer magát csupán beolvassa a tároló mutató hivatkozás, és majd meghívja [CloudBlobContainer.DeleteIfExistsAsync][net_container_delete]:

```csharp
private static async Task DeleteContainerAsync(
    CloudBlobClient blobClient,
    string containerName)
{
    CloudBlobContainer container = blobClient.GetContainerReference(containerName);

    if (await container.DeleteIfExistsAsync())
    {
        Console.WriteLine("Container [{0}] deleted.", containerName);
    }
    else
    {
        Console.WriteLine("Container [{0}] does not exist, skipping deletion.",
            containerName);
    }
}
```

## <a name="step-9-delete-the-job-and-the-pool"></a>Lépés a 9: A feladatot, és a készlet törlése

Az utolsó lépésben a felhasználó törlése a feladatot, és a készlet DotNetTutorial alkalmazásával készített kéri. Bár nem az előfizetést terhelő feladatok és a *rendszer* saját maguk, feladatok számítási csomópontok az előfizetést. Emiatt azt javasoljuk, csomópontok lefoglalhat csak igény szerint. Használaton kívüli készletek törlése, lehet, hogy a karbantartás folyamat részeként.

A BatchClient [JobOperations] [ net_joboperations] és [PoolOperations] [ net_pooloperations] egyaránt rendelkezik a megfelelő törlési módszerről nevezzük, ha a felhasználó megerősíti törlés:

```csharp
// Clean up the resources we've created in the Batch account if the user so chooses
Console.WriteLine();
Console.WriteLine("Delete job? [yes] no");
string response = Console.ReadLine().ToLower();
if (response != "n" && response != "no")
{
    await batchClient.JobOperations.DeleteJobAsync(JobId);
}

Console.WriteLine("Delete pool? [yes] no");
response = Console.ReadLine();
if (response != "n" && response != "no")
{
    await batchClient.PoolOperations.DeletePoolAsync(PoolId);
}
```

> [AZURE.IMPORTANT] Ne feledje, hogy az erőforrások számítási-előfizetést terhelő – még nem használt készletek törlése összezárása költség. Is tartsa szem előtt, hogy egy készlet törlésekor az összes számítási található csomópontok készlethez, és, hogy a csomópontok adatokat fog nem lehet visszaállítani a készlet törlése után.

## <a name="run-the-dotnettutorial-sample"></a>Futtassa a *DotNetTutorial* minta

A minta alkalmazás futtatásakor a konzol eredménye az alábbihoz hasonló lesz. Végrehajtás során a szünet fog tapasztalni `Awaiting task completion, timeout in 00:30:00...` közben a készlet számítási csomópontokat elindított. Az [Azure portál] használata[ azure_portal] a készlet követésére kiszámítania csomópontok, a feladatot, és a feladatok alatt és végrehajtása után. Az [Azure portál] használata[ azure_portal] vagy az [Azure tároló Explorer] [ storage_explorers] a tárhely erőforrások (tárolók és BLOB) az alkalmazás által létrehozott megtekintéséhez.

Tipikus végrehajtás ideje **körülbelül 5 percig tart** az alkalmazás alapértelmezett konfigurációban futtatásakor.

```
Sample start: 1/8/2016 09:42:58 AM

Container [application] created.
Container [input] created.
Container [output] created.
Uploading file C:\repos\azure-batch-samples\CSharp\ArticleProjects\DotNetTutorial\bin\Debug\TaskApplication.exe to container [application]...
Uploading file Microsoft.WindowsAzure.Storage.dll to container [application]...
Uploading file ..\..\taskdata1.txt to container [input]...
Uploading file ..\..\taskdata2.txt to container [input]...
Uploading file ..\..\taskdata3.txt to container [input]...
Creating pool [DotNetTutorialPool]...
Creating job [DotNetTutorialJob]...
Adding 3 tasks to job [DotNetTutorialJob]...
Awaiting task completion, timeout in 00:30:00...
Success! All tasks completed successfully within the specified timeout period.
Downloading all files from container [output]...
All files downloaded to C:\Users\USERNAME\AppData\Local\Temp
Container [application] deleted.
Container [input] deleted.
Container [output] deleted.

Sample end: 1/8/2016 09:47:47 AM
Elapsed time: 00:04:48.5358142

Delete job? [yes] no: yes
Delete pool? [yes] no: yes

Sample complete, hit ENTER to exit...
```

## <a name="next-steps"></a>Következő lépések

Nyugodtan kísérletezhet a különböző számítási esetek a *DotNetTutorial* és a *TaskApplication* . Például felveszi egy adatvégrehajtás késleltetés *TaskApplication*, például a [Thread.Sleep][net_thread_sleep], hogy hosszabb ideig futó feladatok szimulálhatja, és figyelje őket a portálon. Próbálja ki további tevékenységek felvétele és a számítási csomópontok számának módosítása. Keresésének, és egy meglévő készlet sebesség végrehajtási idő használhatók logikát épít (*Tipp*: kivétele `ArticleHelpers.cs` a [Microsoft.Azure.Batch.Samples.Common] a[ github_samples_common] mintákban [azure-köteg –]project[github_samples]).

Most, hogy a köteg megoldások alapvető munkafolyamat ismerős, pedig alaposabban meg a további szolgáltatások a köteg szolgáltatás.

- Olvassa el a [Köteg szolgáltatás áttekintése a fejlesztők számára](batch-api-basics.md), amely javasoljuk az összes új köteget felhasználók.
- Indítsa el a további köteg fejlesztési cikkek **fejlesztési meg szeretné vizsgálni** a [Köteg tanulási javaslat]alatt a[batch_learning_path].
- Olvassa el a terhelést a "legnépszerűbb vagy legnagyobb N szavak" feldolgozása a [TopNWords] köteg használatával különböző végrehajtásának[ github_topnwords] minta.

[azure_batch]: https://azure.microsoft.com/services/batch/
[azure_free_account]: https://azure.microsoft.com/free/
[azure_portal]: https://portal.azure.com
[batch_learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/
[github_batchexplorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[github_dotnettutorial]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/DotNetTutorial
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_common]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/Common
[github_samples_zip]: https://github.com/Azure/azure-batch-samples/archive/master.zip
[github_topnwords]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/TopNWords
[net_api]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[net_api_storage]: https://msdn.microsoft.com/library/azure/mt347887.aspx
[net_batchclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudblobclient]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobclient.aspx
[net_cloudblobcontainer]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.aspx
[net_cloudstorageaccount]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.cloudstorageaccount.aspx
[net_cloudserviceconfiguration]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudserviceconfiguration.aspx
[net_container_delete]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.deleteifexistsasync.aspx
[net_job]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_job_poolinfo]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.protocol.models.cloudjob.poolinformation.aspx
[net_joboperations]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.joboperations
[net_joboperations_terminatejob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.terminatejobasync.aspx
[net_jobpreptask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobpreparationtask.aspx
[net_jobreltask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobreleasetask.aspx
[net_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.aspx
[net_odatadetaillevel]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_pool_create]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[net_pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_pooloperations]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.pooloperations
[net_resourcefile]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.resourcefile.aspx
[net_resourcefile_blobsource]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.resourcefile.blobsource.aspx
[net_sas_blob]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblob.getsharedaccesssignature.aspx
[net_sas_container]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblobcontainer.getsharedaccesssignature.aspx
[net_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.aspx
[net_starttask_resourcefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.resourcefiles.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_task_resourcefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.resourcefiles.aspx
[net_taskstate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.taskstate.aspx
[net_taskstatemonitor]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskstatemonitor.aspx
[net_thread_sleep]: https://msdn.microsoft.com/library/274eh01d(v=vs.110).aspx
[net_virtualmachineconfiguration]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.virtualmachineconfiguration.aspx
[nuget_packagemgr]: https://docs.nuget.org/consume/installing-nuget
[nuget_restore]: https://docs.nuget.org/consume/package-restore/msbuild-integrated#enabling-package-restore-during-build
[storage_explorers]: http://storageexplorer.com/
[visual_studio]: https://www.visualstudio.com/products/vs-2015-product-editions

[1]: ./media/batch-dotnet-get-started/batch_workflow_01_sm.png "Tárolók létrehozása az Azure-tárolóhoz"
[2]: ./media/batch-dotnet-get-started/batch_workflow_02_sm.png "Tárolók tevékenység alkalmazás és a beviteli (adatok) fájlok feltöltése"
[3]: ./media/batch-dotnet-get-started/batch_workflow_03_sm.png "Köteg készlet létrehozása"
[4]: ./media/batch-dotnet-get-started/batch_workflow_04_sm.png "Köteg feladat létrehozása"
[5]: ./media/batch-dotnet-get-started/batch_workflow_05_sm.png "Tevékenységek hozzáadása a projekthez"
[6]: ./media/batch-dotnet-get-started/batch_workflow_06_sm.png "Képernyő-feladatok"
[7]: ./media/batch-dotnet-get-started/batch_workflow_07_sm.png "Töltse le a tevékenység kimeneti tárhely"
[8]: ./media/batch-dotnet-get-started/batch_workflow_sm.png "A köteg megoldás munkafolyamat (teljes diagram)"
[9]: ./media/batch-dotnet-get-started/credentials_batch_sm.png "A portál a köteg hitelesítő adatok"
[10]: ./media/batch-dotnet-get-started/credentials_storage_sm.png "Tárterület hitelesítő adatok portálon"
[11]: ./media/batch-dotnet-get-started/batch_workflow_minimal_sm.png "A köteg megoldás munkafolyamat (minimális diagram)"
