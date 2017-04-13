<properties
    pageTitle="Első lépések az Azure Blob-tárolóhoz (objektum tároló) használatával .NET |} Microsoft Azure"
    description="Strukturálatlan adatokat tárolja az Azure Blob-tárolóhoz (objektum tároló) a felhőben."
    services="storage"
    documentationCenter=".net"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="get-started-with-azure-blob-storage-using-net"></a>Első lépések a .NET használatával Azure Blob-tárolóhoz

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>– Áttekintés

Azure Blob-tárolóhoz olyan strukturálatlan adatokat tároló a felhőben, objektumok és BLOB szolgáltatás. BLOB-tárolóhoz bármilyen szöveget vagy a bináris adatokat, például egy dokumentum, médiafájlok vagy alkalmazás installer tárolhat. BLOB-tárolóhoz is nevezik objektum tárhelyet.

### <a name="about-this-tutorial"></a>Ebben az oktatóanyagban kapcsolatban

Ebből az oktatóanyagból megtudhatja, hogy miként kódírás .NET-néhány gyakori alkalmazási területek Azure Blob-tárolóhoz használatával. Érintett esetek feltöltése, bejegyzésére, letöltéséről és BLOB törlése.

**Becsült időt vesz igénybe:** 45 perc

**Prerequisities:**

- [Microsoft Visual Studio](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx)
- [Azure tároló ügyfél tár a .NET rendszerhez](https://www.nuget.org/packages/WindowsAzure.Storage/)
- [A .NET rendszerhez Azure Konfigurációkezelő](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
- [Azure tárterület-fiók](storage-create-storage-account.md#create-a-storage-account)


[AZURE.INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

### <a name="more-samples"></a>További minták

További példákat Blob-tárolóhoz használ olvassa el a [Azure Blob-tárolóhoz .NET – első lépések](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/)című témakört. A minta alkalmazás letöltése és indítsa el, vagy keresse meg a GitHub kódot.


[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[AZURE.INCLUDE [storage-development-environment-include](../../includes/storage-development-environment-include.md)]

### <a name="add-namespace-declarations"></a>Névtér deklarációs hozzáadása

Adja hozzá a következő `using` kimutatások tetején a `program.cs` fájl:

    using Microsoft.Azure; // Namespace for CloudConfigurationManager
    using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
    using Microsoft.WindowsAzure.Storage.Blob; // Namespace for Blob storage types

### <a name="parse-the-connection-string"></a>A kapcsolati karakterlánc elemzése

[AZURE.INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-the-blob-service-client"></a>A Blob-szolgáltatási ügyfelet létrehozása

A **CloudBlobClient** osztály beolvasni a tárolók és Blob-tárolóban lévő BLOB teszi lehetővé. Az alábbiakban az egyik módja a szolgáltatási ügyfelet létrehozása:

    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

Most már készen áll, amely beolvassa az adatokat, és adatot ír Blob-tárolóhoz kódírás.

## <a name="create-a-container"></a>A tároló létrehozása

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

A példa bemutatja, hogyan hozhat létre a tároló, ha még nem létezik:

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve a reference to a container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Create the container if it doesn't already exist.
    container.CreateIfNotExists();

Alapértelmezés szerint a új tároló magánjellegű, ami azt jelenti, hogy meg kell adnia a tárhely hívóbetű BLOB letöltését a tároló. Ha a fájlokat a tárolóban mindenki számára elérhetővé szeretne, beállíthatja, hogy a tároló közzé a következő kód használatával:

    container.SetPermissions(
        new BlobContainerPermissions { PublicAccess = BlobContainerPublicAccessType.Blob });

Az interneten bárki láthatja a BLOB-tárolóban nyilvános, de módosíthatja vagy törölheti őket, csak akkor, ha rendelkezik a megfelelő fiókot hívóbetű vagy átengedése aláírás.

## <a name="upload-a-blob-into-a-container"></a>A tároló blob feltölteni

Azure Blob-tároló letiltása BLOB és a lap BLOB támogatja.  Az esetek többségében a továbbfejlesztett fájlblokkolás blob a használata ajánlott típusa.

Fájl feltöltése block blob, tároló hivatkozás, és annak segítségével block blob hivatkozás. Ha befejezte a blob-hivatkozások, a **UploadFromStream** módszer hívásával bármilyen adat-adatfolyam feltöltheti azt. Ez a művelet a blob hoz létre, ha azt az előzőleg nem létezik, vagy írja felül, ha létezik.

A következő példa bemutatja, hogyan szeretné feltölteni a tároló blob és feltételezi, hogy a tároló már hozták létre.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite the "myblob" blob with contents from a local file.
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        blockBlob.UploadFromStream(fileStream);
    }

## <a name="list-the-blobs-in-a-container"></a>A BLOB tárolóban lista

A BLOB tárolóban megtekintéséhez először első tároló hivatkozást. A tároló **ListBlobs** módszer segítségével beolvashatja a BLOB és a benne könyvtárak majd. A visszaadott **IListBlobItem**tulajdonságainak és módszerek a multimédiás csoport elérése, meg kell átalakítva **CloudBlockBlob**, **CloudPageBlob**vagy **CloudBlobDirectory** objektum.  A fájl típusa ismeretlen, ha egy típusának ellenőrzése használatával, hogy melyik konvertálható azt.  A következő kódot bemutatja, hogyan olvashat be és az egyes cikkek URI kimeneti a `photos` tároló:

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("photos");

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

A fentiek nevet adhat a nevük elérési út adatokat, például BLOB. Ezzel létrehozott egy virtuális könyvtár struktúra, rendezheti és a fájlrendszer hagyományos módon bejárása. Figyelje meg, hogy a címtár-struktúra virtuális csak - a rendelkezésre álló Blob-tárolóhoz csak erőforrások tárolók és BLOB. A tároló ügyfél tár azonban kínál a virtuális könyvtár hivatkozik, és egyszerűbbé vannak rendezve, így okkal dolgozik egy **CloudBlobDirectory** objektum.

Például érdemes megvizsgálni a következő készlete blokk BLOB-tárolóban nevű `photos`:

    photo1.jpg
    2010/architecture/description.txt
    2010/architecture/photo3.jpg
    2010/architecture/photo4.jpg
    2011/architecture/photo5.jpg
    2011/architecture/photo6.jpg
    2011/architecture/description.txt
    2011/photo7.jpg

A "képek" tároló (ahogy a fenti minta) a hívásakor **ListBlobs** hierarchikus listáját ad vissza. A könyvtárak és BLOB-tárolóban, a kurzor jelölő **CloudBlobDirectory** és a **CloudBlockBlob** objektumokat tartalmaz. Az eredményül kapott kimenetét néz ki:

    Directory: https://<accountname>.blob.core.windows.net/photos/2010/
    Directory: https://<accountname>.blob.core.windows.net/photos/2011/
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg


Ha szükséges beállíthatja, hogy a **UseFlatBlobListing** paramétere: a **ListBlobs** módszer **Igaz**. Ebben az esetben a tárolóban lévő minden blob **CloudBlockBlob** objektumként adja vissza. A hívást **ListBlobs** való visszatéréshez strukturálatlan listaelem néz ki:

    // Loop over items within the container and output the length and URI.
    foreach (IListBlobItem item in container.ListBlobs(null, true))
    {
       ...
    }

és az eredmény a következőhöz hasonlóan:

    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2010/architecture/description.txt
    Block blob of length 314618: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo3.jpg
    Block blob of length 522713: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo4.jpg
    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2011/architecture/description.txt
    Block blob of length 419048: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo5.jpg
    Block blob of length 506388: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo6.jpg
    Block blob of length 399751: https://<accountname>.blob.core.windows.net/photos/2011/photo7.jpg
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg


## <a name="download-blobs"></a>BLOB letöltése

BLOB letöltéséhez először beolvasásához blob hivatkozást, és akkor hívja meg a **DownloadToStream** módszert. Az alábbi példában a **DownloadToStream** módszerrel adhatja át a blob tartalmát, majd a helyi fájlként is megmaradnak adatfolyam-objektum.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save blob contents to a file.
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        blockBlob.DownloadToStream(fileStream);
    }

A **DownloadToStream** módszer segítségével töltse le a csatolást karaktersorozatot blob tartalmát.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "myblob.txt"
    CloudBlockBlob blockBlob2 = container.GetBlockBlobReference("myblob.txt");

    string text;
    using (var memoryStream = new MemoryStream())
    {
        blockBlob2.DownloadToStream(memoryStream);
        text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
    }

## <a name="delete-blobs"></a>Bináris objektumok törlése

Blob törléséhez először első blob hivatkozást, és a **törlése** módszer telefonál.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete the blob.
    blockBlob.Delete();


## <a name="list-blobs-in-pages-asynchronously"></a>Aszinkron lista BLOB-lapokon

Ha meg vannak bejegyzésére BLOB sok, vagy meg szeretné adni egy listaelem művelettel vissza eredmények számát, az eredmények lapokon BLOB jeleníthet meg. Ez a példa bemutatja, hogyan jelenjen meg eredmény az oldalak aszinkron, hogy való visszatéréshez a találatok nagyszámú várakozás közben nem blokkolja a végrehajtás.

Ebben a példában egy egyszerű, strukturálatlan blob bejegyzésére, de a hierarchikus listáját, megadásával is elvégezheti a `useFlatBlobListing` paramétere: a **ListBlobsSegmentedAsync** módszert `false`.

A minta módszer felhívja egy aszinkron módszer, mert meg kell tartalmazni a `async` kulcsszó és kell eredményül adnia egy **tevékenység** -objektumot. A megadott **ListBlobsSegmentedAsync** mód await kulcsszó felfüggeszti a minta metódus végrehajtása, a nevére a partnerlistában tevékenység befejeződéséig.

    async public static Task ListBlobsSegmentedInFlatListing(CloudBlobContainer container)
    {
        //List blobs to the console window, with paging.
        Console.WriteLine("List blobs in pages:");

        int i = 0;
        BlobContinuationToken continuationToken = null;
        BlobResultSegment resultSegment = null;

        //Call ListBlobsSegmentedAsync and enumerate the result segment returned, while the continuation token is non-null.
        //When the continuation token is null, the last page has been returned and execution can exit the loop.
        do
        {
            //This overload allows control of the page size. You can return all remaining results by passing null for the maxResults parameter,
            //or by calling a different overload.
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

## <a name="writing-to-an-append-blob"></a>Egy hozzáfűző blob írása

Egy hozzáfűző blob egy olyan blob, verziójával bevezetett új típusú 5.x az Azure tároló ügyfél tár a .NET rendszerhez. Egy hozzáfűző blob hozzáfűző műveletek, például a naplózás van optimalizálva. Továbbfejlesztett fájlblokkolás blob, például egy hozzáfűző blob blokkok tevődik, de amikor felvesz egy új letiltása egy hozzáfűző blob, akkor mindig hozzáfűzi a blob végére. Nem frissíthetők, és az egy hozzáfűző blob-meglévő blokk törlése. Egy hozzáfűző blob-Tiltás lehetővé tevő dokumentumazonosítók nem láthatóak blokk blob-változatlanok maradnak.

Minden egyes tiltása a egy hozzáfűző blob lehet a különböző méretű 4 MB-os, maximum, és egy hozzáfűző blob tartalmazhatnak legfeljebb 50 000 blokkok. Egy hozzáfűző blob maximális mérete ezért 195 GB-nál több kissé (4 MB-os X 50 000 blokkolja).

Az alábbi példa létrehoz egy új hozzáfűző blob, és hozzáfűzi bizonyos adatok, amelyek egy egyszerű naplózás művelet.

    //Parse the connection string for the storage account.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

    //Create service client for credentialed access to the Blob service.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    //Get a reference to a container.
    CloudBlobContainer container = blobClient.GetContainerReference("my-append-blobs");

    //Create the container if it does not already exist.
    container.CreateIfNotExists();

    //Get a reference to an append blob.
    CloudAppendBlob appendBlob = container.GetAppendBlobReference("append-blob.log");

    //Create the append blob. Note that if the blob already exists, the CreateOrReplace() method will overwrite it.
    //You can check whether the blob exists to avoid overwriting it by using CloudAppendBlob.Exists().
    appendBlob.CreateOrReplace();

    int numBlocks = 10;

    //Generate an array of random bytes.
    Random rnd = new Random();
    byte[] bytes = new byte[numBlocks];
    rnd.NextBytes(bytes);

    //Simulate a logging operation by writing text data and byte data to the end of the append blob.
    for (int i = 0; i < numBlocks; i++)
    {
        appendBlob.AppendText(String.Format("Timestamp: {0:u} \tLog Entry: {1}{2}",
            DateTime.UtcNow, bytes[i], Environment.NewLine));
    }

    //Read the append blob to the console window.
    Console.WriteLine(appendBlob.DownloadText());

Lásd: [ismertetése blokk BLOB, oldal BLOB, és hozzáfűző BLOB](https://msdn.microsoft.com/library/azure/ee691964.aspx) BLOB háromféle közötti különbségekről olvashat.

## <a name="managing-security-for-blobs"></a>Biztonsági BLOB-beállításainak kezelése

Alapértelmezés szerint Azure tároló megőrzi az adatok biztonságos korlátozza a hozzáférést a fiók tulajdonosának, aki birtokában a fiók hívóbetűk. Amikor a tárterület-fiókjában blob-adatok megosztása van szüksége, fontos ehhez a fiókhoz hívóbetűk biztonságát nélkül. Ezenkívül blob-adatok annak érdekében, hogy az Áttekintés a hálózaton és Azure-tárolóban lévő biztonságos titkosítható.

[AZURE.INCLUDE [storage-account-key-note-include](../../includes/storage-account-key-note-include.md)]

### <a name="controlling-access-to-blob-data"></a>Blob-adatokhoz való hozzáférés szabályozása

Alapértelmezés szerint blob a tárterület-fiók adatai csak a tárhely fióktulajdonos érhető el. A fiók hívóbetű ellen Blob-tárolóhoz kérések hitelesítése igényel alapértelmezés szerint. Azonban érdemes bizonyos blob-adatait más felhasználók számára elérhetővé tenni. Két lehetőség közül választhat:

- **Név nélküli hozzáférés:** Legyen a tároló vagy annak BLOB nyilvánosan elérhető a névtelen hozzáférés. [Kezelés névtelen olvasási hozzáférést tárolók és BLOB](storage-manage-access-to-resources.md) talál további információt.
- **Megosztott access aláírások:** Ügyfelek átengedése aláírás (Társítások), amely a tárterület-fiókjában erőforrás delegált hozzáférést biztosít a megadott engedélyekkel, és egy megadott időtartamon hozzá lehet adni. További információt talál [Használatával megosztott Access aláírások (Társítások)](storage-dotnet-shared-access-signature-part-1.md) .

### <a name="encrypting-blob-data"></a>A titkosított blob-adatok

Azure tároló támogatja blob-adatok az ügyfél, mind a kiszolgáló titkosított:

- **Ügyféloldali titkosítás:** A .NET rendszerhez, a tárhely ügyfél tár támogatja a ügyfélalkalmazásokban adatainak titkosítása Azure tároló feltöltése, és az ügyfél letöltése közben az adatok visszafejtése előtt. A tár is támogatja a tárhely kulcsfontosságú fiókkezelés integrálása az Azure kulcs tárolóból elemre. További információt [A Microsoft Azure-tárterületre .NET ügyféloldali titkosítás](storage-client-side-encryption.md) talál. További tájékoztatás [oktatóprogram: titkosítása és visszafejteni az Azure kulcs tárolóra használata a Microsoft Azure-tárolóban lévő BLOB](storage-encrypt-decrypt-blobs-key-vault.md).
- **Kiszolgálóoldali titkosítás**: Azure tároló kiszolgálóoldali titkosítási támogatja. Lásd: [Azure tároló szolgáltatás titkosítási az adatokat a többi (előzetes verzió)](storage-service-encryption.md).

## <a name="next-steps"></a>Következő lépések

Most, hogy megtanulta Blob-tárolóhoz alapjait, kövesse az alábbi hivatkozásokra kattintva további.

### <a name="microsoft-azure-storage-explorer"></a>Microsoft Azure tárolási Explorer
- [Microsoft Azure tároló Explorer (MASE)](../vs-azure-tools-storage-manage-with-storage-explorer.md) egy ingyenes, különálló alkalmazás a Microsofttól, amely lehetővé teszi, hogy adatokkal végzett munkához vizuálisan Azure-tárhely Windows, OS X vagy Linux rendszerhez.

### <a name="blob-storage-samples"></a>BLOB-tároló minták

- [Első lépések a .NET Azure Blob-tárolóhoz](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/)

### <a name="blob-storage-reference"></a>BLOB-tároló hivatkozás

- [Tárterület ügyfél dokumentumtár a .NET-hivatkozás](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
- [REST API-hivatkozás](http://msdn.microsoft.com/library/azure/dd179355)

### <a name="conceptual-guides"></a>Magyarázó rész segédvonalak

- [A AzCopy parancssori segédprogrammal adatátviteli](storage-use-azcopy.md)
- [Tárhely .NET használatának első lépései](storage-dotnet-how-to-use-files.md)
- [A WebJobs SDK Azure blob-tárolóhoz használata](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)

  [Blob5]: ./media/storage-dotnet-how-to-use-blobs/blob5.png
  [Blob6]: ./media/storage-dotnet-how-to-use-blobs/blob6.png
  [Blob7]: ./media/storage-dotnet-how-to-use-blobs/blob7.png
  [Blob8]: ./media/storage-dotnet-how-to-use-blobs/blob8.png
  [Blob9]: ./media/storage-dotnet-how-to-use-blobs/blob9.png

  [Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
  [Configuring Connection Strings]: http://msdn.microsoft.com/library/azure/ee758697.aspx
  [.NET client library reference]: http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409
  [REST API reference]: http://msdn.microsoft.com/library/azure/dd179355
