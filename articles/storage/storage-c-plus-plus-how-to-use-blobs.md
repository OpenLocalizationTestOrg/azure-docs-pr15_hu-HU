<properties
    pageTitle="A C++ (objektum tároló) blob-tárolóhoz használata |} Microsoft Azure"
    description="Strukturálatlan adatokat tárolja az Azure Blob-tárolóhoz (objektum tároló) a felhőben."
    services="storage"
    documentationCenter=".net"
    authors="dineshmurthy"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="dineshm"/>

# <a name="how-to-use-blob-storage-from-c"></a>Használatáról a C++ Blob-tárolóhoz  

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>– Áttekintés

Azure Blob-tárolóhoz olyan strukturálatlan adatokat tároló a felhőben, objektumok és BLOB szolgáltatás. BLOB-tárolóhoz bármilyen szöveget vagy a bináris adatokat, például egy dokumentum, médiafájlok vagy alkalmazás installer tárolhat. BLOB-tárolóhoz is nevezik objektum tárhelyet.

Ez az útmutató elvégzéséhez az Azure Blob-tároló szolgáltatással esetei fog szemléltetik. A minták C++ írt, és a [Azure tároló ügyfél-tár C++](http://github.com/Azure/azure-storage-cpp/blob/master/README.md). Az érintett esetek **feltöltése**, **listaelem**, **letöltése**és **törlése** BLOB.  

>[AZURE.NOTE] Ez az útmutató az Azure tároló ügyfél tár célként, C++ verzió 1.0.0 és felett. A javasolt verziója tároló ügyfél tár 2.2.0, melyiket érdemes [NuGet](http://www.nuget.org/packages/wastorage) vagy [GitHub](https://github.com/Azure/azure-storage-cpp)keresztül érhető el.

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]
[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a>C++-alkalmazás létrehozása
Ebből az útmutatóból tárolási lehetőségek, amely egy C++ alkalmazáson belül futtathatja fogja használni.  

Ehhez, meg kell telepítse az Azure tároló ügyfél tár C++, és Azure tároló fiók létrehozása az Azure-előfizetésben.   

Az Azure tároló ügyfél tár telepítése C++, az alábbi módszerek használhatja:

-   **Linux:** Kövesse az [Azure tároló ügyfél-tár C++ fontos](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) lapon utasításait.  
-   **Windows:** Kattintson a Visual Studióban, **Eszközök > NuGet csomag kezelő > csomag kezelője konzol**. A következő parancsot írja be a [NuGet csomag kezelője konzol](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) , és nyomja le az **ENTER BILLENTYŰT**.  

        Install-Package wastorage

## <a name="configure-your-application-to-access-blob-storage"></a>Állítsa be az alkalmazás Blob-tárolóhoz eléréséhez  
Adja meg a következő utasítások tetején a C++ fájlt, amelyhez tárolására API-khoz Azure BLOB eléréséhez használni a tartalmazza:  

    #include "was/storage_account.h"
    #include "was/blob.h"

## <a name="setup-an-azure-storage-connection-string"></a>Az Azure tároló kapcsolati karakterlánc beállítása
Az Azure tároló ügyfél tároló kapcsolati karakterlánc használja a végpontok és adatok kezelése szolgáltatások eléréséhez szükséges hitelesítő adatok tárolására szolgáló. Ügyfélalkalmazás fut, meg kell adnia a tárhely kapcsolati karakterláncot, a következő formátumban *fióknév* és *AccountKey* az értékek az [Azure-portálon](https://portal.azure.com) szereplő tároló partner nevét a tárterület-fiók és a tárhely hívóbetű használata. A tároló partnereket és a hívóbetűk további tudnivalókért lásd [Azure tároló fiókokat kapcsolatban](storage-create-storage-account.md). Ez a példa mutatja, hogy hogyan tartsa lenyomva az ujját a kapcsolati karakterlánc statikus mező is deklarálhatnak:  

    // Define the connection-string with your values.
    const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));

Az alkalmazás tesztelése a Windows helyi számítógépen, a Microsoft Azure [tároló irányító](storage-use-emulator.md) , hogy telepítve van az [Azure SDK](https://azure.microsoft.com/downloads/)is használhatja. A tároló irányító egy, amely a helyi fejlesztési számítógépen elérhető az Azure Blob várólista és táblázat szolgáltatások segédprogramot. A következő példa bemutatja, hogyan szeretné a helyi tároló irányító tartsa lenyomva az ujját a kapcsolati karakterlánc statikus mező is deklarálhatnak:

    // Define the connection-string with Azure Storage Emulator.
    const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  

Az Azure tároló irányító elindításához kattintson a **Start** gombra, vagy nyomja le a **Windows** billentyűt. Írja be az **Azure tároló irányító**, és jelölje ki **A Microsoft Azure tároló irányító** -alkalmazások listájából.  

Az alábbi példa feltételezi, hogy már használta az alábbi két módszerek egyikét a tárhely kapcsolati karakterláncát.  

## <a name="retrieve-your-connection-string"></a>A kapcsolati karakterlánc beolvasása
A tároló fiókadatok ábrázolásához **cloud_storage_account** osztály is használhatja. A tároló fiókadatok lekérése a tárterület-kapcsolati karakterláncot, a **elemzési** módszert is használhatja.  

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

Ezután kerülni a **cloud_blob_client** osztály hivatkozás lehetővé teszi olyan objektumok, amelyek a tárolók és a Blob-tároló szolgáltatás tárolva BLOB jelölik beolvasásához. A következő kód hozza létre a egy **cloud_blob_client** objektum fölé visszakeresve, amely azt tároló fiók objektumot használva:  

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();  

## <a name="how-to-create-a-container"></a>Útmutató: a tároló létrehozása

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

A példa bemutatja, hogyan hozhat létre a tároló, ha még nem létezik:  

    try
    {
        // Retrieve storage account from connection string.
        azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

        // Create the blob client.
        azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

        // Retrieve a reference to a container.
        azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

        // Create the container if it doesn't already exist.
        container.create_if_not_exists();
    }
    catch (const std::exception& e)
    {
        std::wcout << U("Error: ") << e.what() << std::endl;
    }  

Alapértelmezés szerint a új tároló privát, és meg kell adnia a tárhely hívóbetű tároló BLOB letöltése érdekében. Ha szeretné elérhetővé teheti a fájlok (BLOB) a tárolóban mindenkinek, beállíthatja, hogy a tároló közzé a következő kód használatával:  

    // Make the blob container publicly accessible.
    azure::storage::blob_container_permissions permissions;
    permissions.set_public_access(azure::storage::blob_container_public_access_type::blob);
    container.upload_permissions(permissions);  

Az interneten bárki láthatja a BLOB-tárolóban nyilvános, de módosíthatja vagy törölheti őket, csak akkor, ha rendelkezik a megfelelő hozzáférési billentyűt.  

## <a name="how-to-upload-a-blob-into-a-container"></a>Útmutató: blob feltölteni a tároló
Azure Blob-tároló letiltása BLOB és a lap BLOB támogatja. Az esetek többségében a továbbfejlesztett fájlblokkolás blob a használata ajánlott típusa.  

Fájl feltöltése a továbbfejlesztett fájlblokkolás blob, tároló hivatkozás, és segítségével blokk blob hivatkozás. Ha befejezte a blob-hivatkozások, hívja fel a **upload_from_stream** módszer bármilyen adat-adatfolyam feltöltheti azt. Ez a művelet a blob hoz létre, ha azt az előzőleg nem létezik, vagy írja felül, ha létezik. A következő példa bemutatja, hogyan szeretné feltölteni a tároló blob és feltételezi, hogy a tároló már hozták létre.  

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a previously created container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Retrieve reference to a blob named "my-blob-1".
    azure::storage::cloud_block_blob blockBlob = container.get_block_blob_reference(U("my-blob-1"));

    // Create or overwrite the "my-blob-1" blob with contents from a local file.
    concurrency::streams::istream input_stream = concurrency::streams::file_stream<uint8_t>::open_istream(U("DataFile.txt")).get();
    blockBlob.upload_from_stream(input_stream);
    input_stream.close().wait();

    // Create or overwrite the "my-blob-2" and "my-blob-3" blobs with contents from text.
    // Retrieve a reference to a blob named "my-blob-2".
    azure::storage::cloud_block_blob blob2 = container.get_block_blob_reference(U("my-blob-2"));
    blob2.upload_text(U("more text"));

    // Retrieve a reference to a blob named "my-blob-3".
    azure::storage::cloud_block_blob blob3 = container.get_block_blob_reference(U("my-directory/my-sub-directory/my-blob-3"));
    blob3.upload_text(U("other text"));  

Azt is megteheti a **upload_from_file** módszer segítségével fájl feltöltése a továbbfejlesztett fájlblokkolás blob.

## <a name="how-to-list-the-blobs-in-a-container"></a>Útmutató: a BLOB tárolóban lista
A BLOB tárolóban megtekintéséhez először első tároló hivatkozást. A tároló **list_blobs** módszer segítségével beolvashatja a BLOB és a benne könyvtárak majd. Hozzáférés a multimédiás tulajdonságokat és a visszaadott **list_blob_item**módszerei, meg kell hívni a **list_blob_item.as_blob** megszerezni egy **cloud_blob** objektumot vagy a **list_blob.as_directory** módszerek megszerezni egy cloud_blob_directory objektumot. A következő kódot bemutatja, hogyan olvashat be és az egyes elemek a **saját minta tároló** tárolóban URI kimeneti:

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a previously created container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Output URI of each item.
    azure::storage::list_blob_item_iterator end_of_results;
    for (auto it = container.list_blobs(); it != end_of_results; ++it)
    {
        if (it->is_blob())
        {
            std::wcout << U("Blob: ") << it->as_blob().uri().primary_uri().to_string() << std::endl;
        }
        else
        {
            std::wcout << U("Directory: ") << it->as_directory().uri().primary_uri().to_string() << std::endl;
        }
    }

A bejegyzésére a műveletek, lásd: [C++ lista Azure tároló-erőforrások](storage-c-plus-plus-enumeration.md).

## <a name="how-to-download-blobs"></a>Útmutató: BLOB letöltése
BLOB letöltéséhez először beolvasásához blob hivatkozást, és akkor hívja meg a **download_to_stream** módszert. Az alábbi példában a **download_to_stream** módszerrel adhatja át a blob tartalmát, majd a helyi fájlként is megmaradnak adatfolyam-objektum.  

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a previously created container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Retrieve reference to a blob named "my-blob-1".
    azure::storage::cloud_block_blob blockBlob = container.get_block_blob_reference(U("my-blob-1"));

    // Save blob contents to a file.
    concurrency::streams::container_buffer<std::vector<uint8_t>> buffer;
    concurrency::streams::ostream output_stream(buffer);
    blockBlob.download_to_stream(output_stream);

    std::ofstream outfile("DownloadBlobFile.txt", std::ofstream::binary);
    std::vector<unsigned char>& data = buffer.collection();

    outfile.write((char *)&data[0], buffer.size());
    outfile.close();  

Azt is megteheti blob tartalmának töltse le egy fájlt a **download_to_file** módszerrel is használhatja.
Ezeken kívül is használhatja a **download_text** módszer töltse le a csatolást karaktersorozatot blob tartalmát.  

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a previously created container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Retrieve reference to a blob named "my-blob-2".
    azure::storage::cloud_block_blob text_blob = container.get_block_blob_reference(U("my-blob-2"));

    // Download the contents of a blog as a text string.
    utility::string_t text = text_blob.download_text();

## <a name="how-to-delete-blobs"></a>Útmutató: BLOB törlése
Blob törléséhez először első blob hivatkozást, és a **delete_blob** módszer telefonál.  

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a previously created container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Retrieve reference to a blob named "my-blob-1".
    azure::storage::cloud_block_blob blockBlob = container.get_block_blob_reference(U("my-blob-1"));

    // Delete the blob.
    blockBlob.delete_blob();

## <a name="next-steps"></a>Következő lépések
Most, hogy megtanulta blob-tárolóhoz alapjait, kövesse ezeket a hivatkozásokat, ha többet szeretne tudni az Azure-tárterületet.  

-   [A C++ várólista-tároló használata](storage-c-plus-plus-how-to-use-queues.md)
-   [A C++ Táblatároló használata](storage-c-plus-plus-how-to-use-tables.md)
-   [Lista Azure tároló C++-erőforrások](storage-c-plus-plus-enumeration.md)
-   [Tárterület ügyfél tár C++ hivatkozás](http://azure.github.io/azure-storage-cpp)
-   [Azure tároló dokumentáció](https://azure.microsoft.com/documentation/services/storage/)
- [A AzCopy parancssori segédprogrammal adatátviteli](storage-use-azcopy.md)
