<properties
    pageTitle="Első lépések a blob tárhely és a Visual Studio a csatlakoztatott szolgáltatások (ASP.NET-5) |} Microsoft Azure"
    description="Hogyan kezdjek hozzá az Azure Blob-tárolóhoz Visual Studio ASP.NET 5 projekt, miután létrehozott egy Visual Studio segítségével tároló fiók kapcsolt szolgáltatások"
    services="storage"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="web"
    ms.tgt_pltfrm="vs-getting-started"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/18/2016"
    ms.author="tarcher"/>

# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-aspnet-5"></a>Első lépések az Azure Blob tárhely és a Visual Studio a csatlakoztatott szolgáltatások (ASP.NET-5)

[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

##<a name="overview"></a>– Áttekintés

Ez a cikk ismerteti, hogyan kezdjek hozzá az Azure Blob-tárolóhoz a Visual Studióban, után a korábban létrehozott vagy ASP.NET 5 projektben Azure tároló fiók hivatkozott a Visual Studio csatlakoztatott-szolgáltatás hozzáadása párbeszédpanel segítségével.

Azure Blob-tárolóhoz olyan szolgáltatás, a nagy mennyiségű strukturálatlan adatokat, bárhol is elérhető a világ HTTP-és HTTPS tárolásához. Egyetlen blob bármilyen méretű lehet. BLOB összetevőjét, például képek, hang-és videofájlok, nyers adatokból és dokumentumfájlok lehet. Ez a cikk ismerteti, hogyan veheti használatba blob-tárolóhoz között, a Visual Studio **Csatlakoztatott szolgáltatások hozzáadása** párbeszédpanelen a ASP.NET 5 projektben Azure tároló fiók létrehozását követően.

Él fájlok, mappák, ahogyan a tárhely BLOB élő tárolókban. Miután létrehozott egy tárhely, a tárhely hozzon létre egy vagy több tárolók. Például "Lapkivágások" nevű tárolására, a tárolók az úgynevezett "képek" fényképek tárolására tárolója hozhat létre, és egy másik hang tárolásához "hang" nevű. Miután létrehozta a tárolók, egyes blob-fájlok feltöltheti őket. Lásd: az [első lépések az Azure Blob-tárolóhoz .NET használatával](storage-dotnet-how-to-use-blobs.md) programozás útján kezelésére szolgáló BLOB további információt.

##<a name="access-blob-containers-in-code"></a>Az Access blob tárolók a kódot.

ASP.NET-5 projektekben BLOB programozás útján történő eléréséhez kell hozzáadnia az alábbiakat, ha az azok nem bemutató.

1. Adja hozzá a következő kódot névtér nyilatkozatokat tetején lévő C# fájlok programozás útján történő elérése az Azure tárterületet szeretne.

        using Microsoft.Extensions.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Extensions.Logging.LogLevel;

2. Egy **CloudStorageAccount** objektum, amely a tárhely fiókadatok beolvasása. Az alábbi kód használatával beszerzése a a tárhely kapcsolati karakterlánc és a tárhely fiók adatait a Azure szolgáltatás beállításai.

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

    **Megjegyzés:** Az alábbi szakaszok használata az összes elé a kódot a fenti kódot.


3. Segítségével a **CloudBlobClient** objektum egy meglévő tároló **CloudBlobContainer** hivatkozást a tárterület-fiókjában.

        // Create a blob client.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        // Get a reference to a container named “mycontainer.”
        CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");



##<a name="create-a-container-in-code"></a>A tároló létrehozása a kódot.

A **CloudBlobClient** hoz létre a tárolót a tárterület-fiókjában is használhatja. Összes kell tennie, ha a hívást kezdeményez **CreateIfNotExistsAsync** ahogy az alábbi kód hozzáadása:

    // Create a blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Get a reference to a container named “my-new-container.”
    CloudBlobContainer container = blobClient.GetContainerReference("my-new-container");

    // If “mycontainer” doesn’t exist, create it.
    await container.CreateIfNotExistsAsync();


**Megjegyzés:** Végezze el a hívások Azure tárolóhoz ASP.NET 5 API-k aszinkron. [Aszinkron programozási aszinkron és Await](http://msdn.microsoft.com/library/hh191443.aspx) talál további információt. Az alábbi kód feltételezi, hogy aszinkron programozási módszereket használják.

Elérhetővé szeretné tenni a fájlokat a tárolóban mindenkinek, beállíthatja a tárolót kell a nyilvános az alábbi kód használatával.

    await container.SetPermissionsAsync(new BlobContainerPermissions
    {
        PublicAccess = BlobContainerPublicAccessType.Blob
    });

##<a name="upload-a-blob-into-a-container"></a>A tároló blob feltölteni

Szeretné feltölteni a blob-fájlt tároló, tároló hivatkozás, és annak segítségével blob hivatkozás. Után blob hivatkozást, hívja fel a **UploadFromStreamAsync** módszer bármilyen adat-adatfolyam feltöltheti azt. Ez a művelet a blob hoz létre, ha nem szerepel ott, vagy ha létezik felülírja azt. A következő példa bemutatja, hogyan szeretné feltölteni a tároló blob és feltételezi, hogy a tároló már hozták létre.

    // Get a reference to a blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite the "myblob" blob with the contents of a local file
    // named “myfile”.
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        await blockBlob.UploadFromStreamAsync(fileStream);
    }

##<a name="list-the-blobs-in-a-container"></a>A BLOB tárolóban lista
A BLOB tárolóban megtekintéséhez először első tároló hivatkozást. A tároló **ListBlobsSegmentedAsync** módszer a BLOB és a benne könyvtárak beolvasásához majd felhívhatja. A visszaadott **IListBlobItem**tulajdonságainak és módszerek a multimédiás csoport elérése, meg kell átalakítva **CloudBlockBlob**, **CloudPageBlob**vagy **CloudBlobDirectory** objektum. Ha nem tudja, hogy a blob-típus, hogy melyik konvertálható, hogy egy típusának ellenőrzése segítségével. A következő kódot bemutatja, hogyan olvashat be és az egyes elemek tárolóban URI kimeneti.

    BlobContinuationToken token = null;
    do
    {
        BlobResultSegment resultSegment = await container.ListBlobsSegmentedAsync(token);
        token = resultSegment.ContinuationToken;

        foreach (IListBlobItem item in resultSegment.Results)
        {
            if (item.GetType() == typeof(CloudBlockBlob))
            {
                CloudBlockBlob blob = (CloudBlockBlob)item;
                Console.WriteLine("Block blob of length {0}: {1}", blob.Properties.Length, blob.Uri);
            }

            else if (item.GetType() == typeof(CloudPageBlob))
            {
                CloudPageBlob pageBlob = (CloudPageBlob)item;

                Console.WriteLine("Page blob of length {0}: {1}", pageBlob.Properties.Length, pageBlob.Uri);
            }

            else if (item.GetType() == typeof(CloudBlobDirectory))
            {
                CloudBlobDirectory directory = (CloudBlobDirectory)item;

                Console.WriteLine("Directory: {0}", directory.Uri);
            }
        }
    } while (token != null);

Módon mások blob-tároló tartalmának megjelenítéséhez. Lásd: az [első lépések az Azure Blob-tárolóhoz .NET használatával](storage-dotnet-how-to-use-blobs.md#list-the-blobs-in-a-container) további információt.

##<a name="download-a-blob"></a>Blob letöltése
Blob letöltéséhez először beszerzése a blob hivatkozást, és akkor hívja meg a **DownloadToStreamAsync** módszert. Az alábbi példában a **DownloadToStreamAsync** módszer blob tartalmát átviteli adatfolyam-objektum, majd a helyi fájlként mentheti.

    // Get a reference to a blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save the blob contents to a file named “myfile”.
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        await blockBlob.DownloadToStreamAsync(fileStream);
    }

Az alábbi egyéb módon BLOB mentése fájlként. Lásd: az [első lépések az Azure Blob-tárolóhoz .NET használatával](storage-dotnet-how-to-use-blobs.md#download-blobs) további információt.

##<a name="delete-a-blob"></a>Blob törlése
Blob törléséhez először beszerzése a blob hivatkozást, és a **DeleteAsync** módszer telefonál.

    // Get a reference to a blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete the blob.
    await blockBlob.DeleteAsync();

## <a name="next-steps"></a>Következő lépések

[AZURE.INCLUDE [vs-storage-dotnet-blobs-next-steps](../../includes/vs-storage-dotnet-blobs-next-steps.md)]
