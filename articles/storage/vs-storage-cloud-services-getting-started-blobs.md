<properties
    pageTitle="Első lépések a blob tárhely és a Visual Studio a csatlakoztatott szolgáltatások (felhőalapú szolgáltatások) |} Microsoft Azure"
    description="Ismerkedés az Azure Blob-tárolóhoz használata egy felhőalapú szolgáltatás project a Visual Studióban, miután részletesen a tárterület a Visual Studio segítségével a csatlakoztatott szolgáltatások"
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

# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-cloud-services-projects"></a>Első lépések Azure Blob-tárolóhoz és a Visual Studio a csatlakoztatott szolgáltatások (cloud services projektek)

[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>– Áttekintés

Ez a cikk ismerteti a használatba Azure Blob-tárolóhoz után létrehozott, vagy a hivatkozott Azure tárterület-fiók a Visual Studio **Csatlakoztatott szolgáltatások hozzáadása** párbeszédpanelen a Visual Studio cloud services projekt használatával. Bemutatjuk, hogyan nyithat meg és blob tárolók létrehozása, és hogyan végezhetők el a gyakori műveletek, például feltöltése, a bejegyzésére és a letöltés BLOB. A minták írt C\# és a [Microsoft Azure tároló ügyfél tár a .NET rendszerhez](https://msdn.microsoft.com/library/azure/dn261237.aspx).

Azure Blob-tárolóhoz olyan szolgáltatás, a nagy mennyiségű strukturálatlan adatokat, bárhol is elérhető a világ HTTP-és HTTPS tárolásához. Egyetlen blob bármilyen méretű lehet. BLOB összetevőjét, például képek, hang-és videofájlok, nyers adatokból és dokumentumfájlok lehet.

Él fájlok, mappák, ahogyan a tárhely BLOB élő tárolókban. Miután létrehozott egy tárhely, a tárhely hozzon létre egy vagy több tárolók. Például "Lapkivágások" nevű tárolására, a tárolók az úgynevezett "képek" fényképek tárolására tárolója hozhat létre, és egy másik hang tárolásához "hang" nevű. Miután létrehozta a tárolók, egyes blob-fájlok feltöltheti őket.

- Programozás útján kezelésére szolgáló BLOB kapcsolatos további tudnivalókért olvassa el az [Azure Blob-tárolóhoz .NET használata – első lépések](storage-dotnet-how-to-use-blobs.md)című témakört.
- Azure tárhely, általános információt a [tárhely](https://azure.microsoft.com/documentation/services/storage/)dokumentációjában.
- Azure Felhőszolgáltatások, általános információt a [Cloud Services](https://azure.microsoft.com/documentation/services/cloud-services/)dokumentációjában.
- Programozási ASP.NET-alkalmazásokkal kapcsolatos további tudnivalókért lásd: az [ASP.NET](http://www.asp.net).

## <a name="access-blob-containers-in-code"></a>Az Access blob tárolók a kódot.

Felhőalapú szolgáltatás projektekben BLOB programozás útján történő eléréséhez kell hozzáadnia az alábbiakat, ha az azok nem bemutató.

1. Adja hozzá a következő kódot névtér nyilatkozatokat bármely C# fájlt, amelyben el szeretné érni programozás útján a Azure tároló tetején.

        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;

2. Egy **CloudStorageAccount** objektum, amely a tárhely fiókadatok beolvasása. Az alábbi kód használatával beszerzése a a tárhely kapcsolati karakterlánc és a tárhely fiók adatait a Azure szolgáltatás beállításai.

        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("<storage account name>_AzureStorageConnectionString"));

3. Ismerkedés a tárterület-fiókjában létező tárolót hivatkozni szeretne egy **CloudBlobClient** objektum.

        // Create a blob client.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

4. Egy adott blob-tárolóhoz hivatkozni szeretne egy **CloudBlobContainer** objektum kaphat.

        // Get a reference to a container named “mycontainer.”
        CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

> [AZURE.NOTE] Az összes használja az alábbi szakaszok kód előtt az előző eljárás a kódot.

## <a name="create-a-container-in-code"></a>A tároló létrehozása a kódot.

> [AZURE.NOTE] Egyes végezze el az Azure tároló hívásainak ASP.NET API-k aszinkron. [Aszinkron programozási aszinkron és Await](http://msdn.microsoft.com/library/hh191443.aspx) talál további információt. A kód, a következő példa feltételezi, hogy aszinkron programozási módszereket kell használnia.

A tárterület-fiókjában tároló létrehozásához kell tennie mindössze hozzáadása hívást kezdeményez **CreateIfNotExistsAsync** ahogy a következő kódot:

    // If “mycontainer” doesn’t exist, create it.
    await container.CreateIfNotExistsAsync();


Elérhetővé szeretné tenni a fájlokat a tárolóban mindenkinek, beállíthatja a tárolót kell a nyilvános az alábbi kód használatával.

    await container.SetPermissionsAsync(new BlobContainerPermissions
    {
        PublicAccess = BlobContainerPublicAccessType.Blob
    });


Az interneten bárki láthatja a BLOB-tárolóban nyilvános, de módosíthatja vagy törölheti őket, csak akkor, ha rendelkezik a megfelelő hozzáférési billentyűt.

## <a name="upload-a-blob-into-a-container"></a>A tároló blob feltölteni

Azure tároló letiltása BLOB és a lap BLOB támogatja. Az esetek többségében a továbbfejlesztett fájlblokkolás blob a használata ajánlott típusa.

Fájl feltöltése a továbbfejlesztett fájlblokkolás blob, tároló hivatkozás, és segítségével blokk blob hivatkozás. Ha befejezte a blob-hivatkozások, hívja fel a **UploadFromStream** módszer bármilyen adat-adatfolyam feltöltheti azt. Ez a művelet a blob hoz létre, ha korábban már nem létezik, vagy felülírja ezt, ha létezik. A következő példa bemutatja, hogyan szeretné feltölteni a tároló blob és feltételezi, hogy a tároló már hozták létre.

    // Retrieve a reference to a blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite the "myblob" blob with contents from a local file.
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        blockBlob.UploadFromStream(fileStream);
    }

## <a name="list-the-blobs-in-a-container"></a>A BLOB tárolóban lista

A BLOB tárolóban megtekintéséhez először első tároló hivatkozást. A tároló **ListBlobs** módszer segítségével beolvashatja a BLOB és a benne könyvtárak majd. A visszaadott **IListBlobItem**tulajdonságainak és módszerek a multimédiás csoport elérése, meg kell átalakítva **CloudBlockBlob**, **CloudPageBlob**vagy **CloudBlobDirectory** objektum. A fájl típusa ismeretlen, ha egy típusának ellenőrzése használatával, hogy melyik konvertálható azt. A következő kódot bemutatja, hogyan olvashat be és az egyes elemek a **fényképek** tárolóban URI kimeneti:

    // Loop over items within the container and output the length and URI.
    foreach (IListBlobItem item in container.ListBlobs(null, false))
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

Ahogy az előző kód minta, a blob-szolgáltatás a amely könyvtárak belül tárolók, valamint a tartalmaz. Ez az, hogy a BLOB több mappa hasonló struktúrában rendezheti. Például érdemes megvizsgálni a következő készlete blokk BLOB-tárolóban **fényképek**nevű:

    photo1.jpg
    2010/architecture/description.txt
    2010/architecture/photo3.jpg
    2010/architecture/photo4.jpg
    2011/architecture/photo5.jpg
    2011/architecture/photo6.jpg
    2011/architecture/description.txt
    2011/photo7.jpg

A tároló (például az előző minta) a hívásakor **ListBlobs** a a visszaadott gyűjtemény a könyvtárak és a legfelső szinten lévő BLOB objektumok **CloudBlobDirectory** és **CloudBlockBlob** tartalmazza. Az alábbiakban az eredményül kapott kimenetét:

    Directory: https://<accountname>.blob.core.windows.net/photos/2010/
    Directory: https://<accountname>.blob.core.windows.net/photos/2011/
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg


Ha szükséges beállíthatja, hogy a **UseFlatBlobListing** paramétere: a **ListBlobs** módszer **Igaz**. Ennek eredményeképpen minden blob éppen egy **CloudBlockBlob**, függetlenül a címtár adja vissza. Az alábbiakban a **ListBlobs**hívást:

    // Loop over items within the container and output the length and URI.
    foreach (IListBlobItem item in container.ListBlobs(null, true))
    {
       ...
    }

és Íme az eredményeket:

    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2010/architecture/description.txt
    Block blob of length 314618: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo3.jpg
    Block blob of length 522713: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo4.jpg
    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2011/architecture/description.txt
    Block blob of length 419048: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo5.jpg
    Block blob of length 506388: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo6.jpg
    Block blob of length 399751: https://<accountname>.blob.core.windows.net/photos/2011/photo7.jpg
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg

További tudnivalókért olvassa el a [CloudBlobContainer.ListBlobs](https://msdn.microsoft.com/library/azure/dd135734.aspx)című témakört.

## <a name="download-blobs"></a>BLOB letöltése

BLOB letöltéséhez először beolvasásához blob hivatkozást, és akkor hívja meg a **DownloadToStream** módszert. Az alábbi példában a **DownloadToStream** módszerrel adhatja át a blob tartalmát, majd a helyi fájlként is megmaradnak adatfolyam-objektum.

    // Get a reference to a blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save blob contents to a file.
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        blockBlob.DownloadToStream(fileStream);
    }

A **DownloadToStream** módszer segítségével töltse le a csatolást karaktersorozatot blob tartalmát.

    // Get a reference to a blob named "myblob.txt"
    CloudBlockBlob blockBlob2 = container.GetBlockBlobReference("myblob.txt");

    string text;
    using (var memoryStream = new MemoryStream())
    {
        blockBlob2.DownloadToStream(memoryStream);
        text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
    }

## <a name="delete-blobs"></a>Bináris objektumok törlése

Blob törléséhez először első blob hivatkozást, és akkor hívja meg a **Delete** metódus.

    // Get a reference to a blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete the blob.
    blockBlob.Delete();


## <a name="list-blobs-in-pages-asynchronously"></a>Aszinkron lista BLOB-lapokon

Ha meg vannak bejegyzésére BLOB sok, vagy meg szeretné adni egy listaelem művelettel vissza eredmények számát, az eredmények lapokon BLOB jeleníthet meg. Ez a példa bemutatja, hogyan jelenjen meg eredmény az oldalak aszinkron, hogy való visszatéréshez a találatok nagyszámú várakozás közben nem blokkolja a végrehajtás.

Ebben a példában egy egyszerű, strukturálatlan blob bejegyzésére, de hierarchikus listáját, végezze el a **ListBlobsSegmentedAsync** módszer a **useFlatBlobListing** paraméter **értéke hamis**megadásával.

A minta módszer felhívja egy aszinkron módszer, mivel a **aszinkron** kulcsszóval is tartalmazni, és a **tevékenység** objektum kell visszaadnia. A megadott **ListBlobsSegmentedAsync** mód await kulcsszó felfüggeszti a minta metódus végrehajtása, a nevére a partnerlistában tevékenység befejeződéséig.

    async public static Task ListBlobsSegmentedInFlatListing(CloudBlobContainer container)
    {
        // List blobs to the console window, with paging.
        Console.WriteLine("List blobs in pages:");

        int i = 0;
        BlobContinuationToken continuationToken = null;
        BlobResultSegment resultSegment = null;

        // Call ListBlobsSegmentedAsync and enumerate the result segment returned, while the continuation token is non-null.
        // When the continuation token is null, the last page has been returned and execution can exit the loop.
        do
        {
            // This overload allows control of the page size. You can return all remaining results by passing null for the maxResults parameter,
            // or by calling a different overload.
            resultSegment = await container.ListBlobsSegmentedAsync("", true, BlobListingDetails.All, 10, continuationToken, null, null);
            if (resultSegment.Results.Count<IListBlobItem>() > 0) { Console.WriteLine("Page {0}:", ++i); }
            foreach (var blobItem in resultSegment.Results)
            {
                Console.WriteLine("\t{0}", blobItem.StorageUri.PrimaryUri);
            }
            Console.WriteLine();

            //Get the continuation token.
            continuationToken = resultSegment.ContinuationToken;
        }
        while (continuationToken != null);
    }

## <a name="next-steps"></a>Következő lépések

[AZURE.INCLUDE [vs-storage-dotnet-blobs-next-steps](../../includes/vs-storage-dotnet-blobs-next-steps.md)]
