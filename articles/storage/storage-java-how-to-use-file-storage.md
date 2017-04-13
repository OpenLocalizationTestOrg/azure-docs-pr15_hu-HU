<properties
    pageTitle="Java a tárhely használata |} Microsoft Azure"
    description="Megtudhatja, hogy miként tölthet fel, töltse le a listát, és törölje a fájlokat a Azure szolgáltatás használatával. A minta Java nyelven íródott."
    services="storage"
    documentationCenter="java"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn" />

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robinsh"/>

# <a name="how-to-use-file-storage-from-java"></a>Java a tárhely használata

[AZURE.INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-files.md)]

## <a name="overview"></a>– Áttekintés

Ez az útmutató fogja megtudhatja, miként végezheti el az alapműveletek a a Microsoft Azure fájlt tároló szolgáltatás. Java nyelven írt mintákon keresztül áttekintheti, hogyan hozhat létre és az könyvtárak, töltse fel, lista, és fájlok törlése. Ha új Microsoft Azure fájl tárhelyszolgáltatáshoz, nagyon hasznos a minták megértéséhez keresztül az az alábbi szakaszok a fogalmak lesz.

[AZURE.INCLUDE [storage-file-concepts-include](../../includes/storage-file-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a>Java-alkalmazás létrehozása

A minta létrehozásához, szüksége lesz a Java fejlesztési Kit (JDK) és az [Java Azure tároló SDK] []. Érdemes is létrehozott Azure tárterület-fiókkal.

## <a name="setup-your-application-to-use-file-storage"></a>Az alkalmazás fájltároló beállítása

Az Azure tároló API-hoz, vegye fel a következő utasítás felső részén a hova kívánja elérni a tárhelyszolgáltatása Java-fájlt.

    // Include the following imports to use blob APIs.
    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.file.*;

## <a name="setup-an-azure-storage-connection-string"></a>Az Azure tároló kapcsolati karakterlánc beállítása

Tárhely használata esetén szüksége az Azure tároló fiókhoz való csatlakozáshoz. Az első lépés a kapcsolati karakterlánc, amely használjuk, hogy a tárterület-fiókhoz való csatlakozáshoz konfigurálása lenne. Vegyük megadása egy statikus változó megtennie.

    // Configure the connection-string with your values
    public static final String storageConnectionString =
        "DefaultEndpointsProtocol=http;" +
        "AccountName=your_storage_account_name;" +
        "AccountKey=your_storage_account_key";

> [AZURE.NOTE] A tényleges értékek tároló fiókjának your_storage_account_name és your_storage_account_key cserélje.

## <a name="connecting-to-an-azure-storage-account"></a>Azure tároló fiók csatlakoztatása

Fiókhoz való csatlakozáshoz a tárhely, kell használni az **CloudStorageAccount** objektum átadása egy kapcsolati karakterláncot az **elemzési** módszert.

    // Use the CloudStorageAccount object to connect to your storage account
    try {
        CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);
    } catch (InvalidKeyException invalidKey) {
        // Handle the exception
    }

**CloudStorageAccount.parse** okoz-InvalidKeyException, hogy behelyezi próbálja/tényleges blokkon belül kell.

## <a name="how-to-create-a-share"></a>Útmutató: megosztás létrehozása

A fájlok és a fájltároló könyvtárak **megosztás**nevű tároló tárolnak. A tároló fiók beállíthatja, hogy minél nagyobb megosztások a fiók kapacitás szolgáltatással. A megosztás és tartalomhoz való hozzáférés beszerzéséhez szeretne egy fájlt tároló ügyfelet használ.

    // Create the file storage client.
    CloudFileClient fileClient = storageAccount.createCloudFileClient();

A fájlt tároló ügyfelet használ, majd szerezhet be a megosztás hivatkozást.

    // Get a reference to the file share
    CloudFileShare share = fileClient.getShareReference("sampleshare");

A megosztás ténylegesen létrehozásához használja az CloudFileShare objektum **createIfNotExists** metódusát.

    if (share.createIfNotExists()) {
        System.out.println("New share created");
    }

Ezen a ponton **megosztása** mentesítésről megosztáshoz **sampleshare**nevű hivatkozást.

## <a name="how-to-upload-a-file"></a>Útmutató: a fájl feltöltése

Egy Azure fájlmegosztás tároló tartalmaz legalább, legfelső szintű könyvtárában fájlokat tároló is. Ebben a szakaszban megismerheti fogja mindig a legfelső szintű könyvtár megosztás helyi tárhelyről fájl feltöltése.

Az első lépés a fájl feltöltésekor beszerzése a címtárhoz hivatkozás, amelyben lakik kell. Ehhez hívja fel a megosztott objektum **getRootDirectoryReference** metódusát.

    //Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

Most, hogy a legfelső szintű directory, a megosztás hivatkozást, feltöltheti egy fájlt, az alábbi kód használatával az alakzatot.

    // Define the path to a local file.
    final String filePath = "C:\\temp\\Readme.txt";

    CloudFile cloudFile = rootDir.getFileReference("Readme.txt");
    cloudFile.uploadFromFile(filePath);

## <a name="how-to-create-a-directory"></a>Útmutató: könyvtár létrehozása

Tárterület rendezése elhelyez fájlok bízza meg az összes oldalról a legfelső szintű címtárban alszint könyvtárak belül. Az Azure fájlt tároló szolgáltatás lehetővé teszi, a fiókja lehetővé teszi, hogy minél nagyobb könyvtárak létrehozása. Az alábbi kód **sampledir** a legfelső szintű könyvtárban nevű alszint könyvtárában hoz létre.

    //Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

    //Get a reference to the sampledir directory
    CloudFileDirectory sampleDir = rootDir.getDirectoryReference("sampledir");

    if (sampleDir.createIfNotExists()) {
        System.out.println("sampledir created");
    } else {
        System.out.println("sampledir already exists");
    }

## <a name="how-to-list-files-and-directories-in-a-share"></a>Útmutató: a lista fájlok és mappák a megosztás

Fájlok és mappák belüli megosztás partnerlistáját hívja fel a **listFilesAndDirectories** CloudFileDirectory hivatkozást a könnyen történik. A módszer ListFileItem objektumok, amelyek meg is találta a listáját adja eredményül. Példaként a következő kódot felsorolásban, fájlok és mappák a legfelső szintű könyvtár belül.

    //Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

    for ( ListFileItem fileItem : rootDir.listFilesAndDirectories() ) {
        System.out.println(fileItem.getUri());
    }


## <a name="how-to-download-a-file"></a>Útmutató: a fájl letöltése

A gyakoribb műveletek elkészíti fájl tárolás ellen egyik fájlok letöltését. A következő példában a kód SampleFile.txt tölthető le, és megjeleníti a tartalmát.

    //Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

    //Get a reference to the directory that contains the file
    CloudFileDirectory sampleDir = rootDir.getDirectoryReference("sampledir");

    //Get a reference to the file you want to download
    CloudFile file = sampleDir.getFileReference("SampleFile.txt");

    //Write the contents of the file to the console.
    System.out.println(file.downloadText());

## <a name="how-to-delete-a-file"></a>Útmutató: fájl törlése

Egy másik közös fájlt tároló művelet a fájlok törlését. A következő kódot nevű SampleFile.txt belül **sampledir**nevű könyvtárában tárolt fájl törlése


    // Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

    // Get a reference to the directory where the file to be deleted is in
    CloudFileDirectory containerDir = rootDir.getDirectoryReference("sampledir");

    String filename = "SampleFile.txt"
    CloudFile file;

    file = containerDir.getFileReference(filename)
    if ( file.deleteIfExists() ) {
        System.out.println(filename + " was deleted");
    }


## <a name="how-to-delete-a-directory"></a>Útmutató: könyvtár törlése

A könyvtár törlése parancs a meglehetősen egyszerű feladat, bár Megjegyzendő, hogy nem törölhetők a fájlok továbbra is tartalmazó könyvtár vagy más könyvtárak.

    // Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

    // Get a reference to the directory you want to delete
    CloudFileDirectory containerDir = rootDir.getDirectoryReference("sampledir");

    // Delete the directory
    if ( containerDir.deleteIfExists() ) {
        System.out.println("Directory deleted");
    }


## <a name="how-to-delete-a-share"></a>Útmutató: megosztás törlése

Hívja fel a **deleteIfExists** módszer CloudFileShare objektum törlése a megosztás történik. Az alábbiakban a minta kódot, amely tartalmaz.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

        // Create the file client.
       CloudFileClient fileClient = storageAccount.createCloudFileClient();

       // Get a reference to the file share
       CloudFileShare share = fileClient.getShareReference("sampleshare");

       if (share.deleteIfExists()) {
           System.out.println("sampleshare deleted");
       }
    } catch (Exception e) {
        e.printStackTrace();
    }

## <a name="next-steps"></a>Következő lépések

Ha azt szeretné, ha többet szeretne megtudni más Azure tároló API-hoz, kövesse ezeket a hivatkozásokat.

- [Java Developer Center](http://azure.microsoft.com/develop/java/)
- [Azure tároló Java SDK](https://github.com/azure/azure-storage-java)
- [Azure tároló SDK androidra](https://github.com/azure/azure-storage-android)
- [Azure tároló ügyfél SDK hivatkozás](http://dl.windowsazure.com/storage/javadoc/)
- [Azure tárolása REST API-val](https://msdn.microsoft.com/library/azure/dd179355.aspx)
- [Azure tároló csapatának blogja](http://blogs.msdn.com/b/windowsazurestorage/)
- [A AzCopy parancssori segédprogram az adatok átvitele](storage-use-azcopy.md)
