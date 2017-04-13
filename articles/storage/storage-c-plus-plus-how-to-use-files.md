<properties
    pageTitle="A C++ fájltároló használata |} Microsoft Azure"
    description="Fájl adattárolásra az Azure-tárhelyet a felhőben."
    services="storage"
    documentationCenter=".net"
    authors="seguler"
    manager="jahogg"
    editor="tysonn" />

<tags ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="seguler" />

# <a name="how-to-use-file-storage-from-c"></a>A C++ fájltároló használata

[AZURE.INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-files](../../includes/storage-try-azure-tools-files.md)]
[AZURE.INCLUDE [storage-file-overview-include](../../includes/storage-file-overview-include.md)]

## <a name="about-this-tutorial"></a>Ebben az oktatóanyagban kapcsolatban

Ebben az oktatóanyagban fogja megtudhatja, miként végezheti el az alapműveletek a a Microsoft Azure fájlt tároló szolgáltatás. C++ nyelven írt minták keresztül áttekintheti, hogyan hozhat létre és az könyvtárak, töltse fel, lista, és fájlok törlése. Ha új Microsoft Azure fájl tárhelyszolgáltatáshoz, nagyon hasznos a minták megértéséhez keresztül az az alábbi szakaszok a fogalmak lesz.

[AZURE.INCLUDE [storage-file-concepts-include](../../includes/storage-file-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a>C++-alkalmazás létrehozása

A minta létrehozásához, szüksége lesz az Azure tároló ügyfél tár 2.4.0 C++ telepítése. Érdemes is létrehozott Azure tárterület-fiókkal.

Telepítse az Azure tároló ügyfelet 2.4.0 for C++ segítségével az alábbi lehetőségek közül:

-   **Linux:** Kövesse az [Azure tároló ügyfél-tár C++ fontos](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) lapon utasításait.

-   **Windows:** Kattintson a Visual Studióban, **Eszközök &gt; NuGet csomag Manager &gt; csomag Manager konzolhoz**. A következő parancsot írja be a [NuGet csomag Manager konzolhoz](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) , és nyomja le az **ENTER BILLENTYŰT**.

    Telepítés-csomag wastorage

## <a name="set-up-your-application-to-use-file-storage"></a>Állítsa be az alkalmazás használata a fájlok tárolásához

Adja meg a következő utasítások tetején a C++ fájlt, amelyre az Azure tároló API-khoz fájlok eléréséhez használni a tartalmazza:

    #include "was/storage_account.h"
    #include "was/file.h"

## <a name="set-up-an-azure-storage-connection-string"></a>Az Azure tároló kapcsolati karakterlánc beállítása

Tárhely használata esetén szüksége az Azure tároló fiókhoz való csatlakozáshoz. Az első lépés a kapcsolati karakterlánc, amely használjuk, hogy a tárterület-fiókhoz való csatlakozáshoz konfigurálása lenne. Vegyük megadása egy statikus változó megtennie.

    // Define the connection-string with your values.
    const utility::string_t 
    storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));

## <a name="connecting-to-an-azure-storage-account"></a>Azure tároló fiók csatlakoztatása

A tároló fiókadatok ábrázolásához **cloud_storage_account** osztály is használhatja. A tároló fiókadatok lekérése a tárterület-kapcsolati karakterláncot, a **elemzési** módszert is használhatja.

    // Retrieve storage account from connection string. 
    azure::storage::cloud_storage_account storage_account = 
      azure::storage::cloud_storage_account::parse(storage_connection_string);

## <a name="how-to-create-a-share"></a>Útmutató: megosztás létrehozása

A fájlok és a fájltároló könyvtárak **megosztás**nevű tároló tárolnak. Tárterület-fiókja beállíthatja, hogy a fiók kapacitás szolgáltatással annyi megosztások. A megosztás és tartalomhoz való hozzáférés beszerzéséhez szeretne egy fájlt tároló ügyfelet használ.

    // Create the file storage client.
    azure::storage::cloud_file_client file_client = 
      storage_account.create_cloud_file_client();

A fájlt tároló ügyfelet használ, majd szerezhet be a megosztás hivatkozást.

    // Get a reference to the file share
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));

A megosztás létrehozásához használja az **cloud_file_share** objektum **create_if_not_exists** metódusát.

    if (share.create_if_not_exists()) { 
        std::wcout << U("New share created") << std::endl;  
    }

Ezen a ponton **megosztása** rendelkezik egy **saját minta megosztás**nevű megosztás hivatkozást.

## <a name="how-to-upload-a-file"></a>Útmutató: a fájl feltöltése

Legalább egy Azure fájlmegosztás tároló tartalmaz fájlokat is tároló legfelső szintű könyvtárat. Ebben a szakaszban megismerheti fogja mindig a legfelső szintű könyvtár megosztás helyi tárhelyről fájl feltöltése.

Az első lépés a fájl feltöltésekor beszerzése a címtárhoz hivatkozás, amelyben lakik kell. Ehhez hívja fel a megosztott objektum **get_root_directory_reference** metódusát.

    //Get a reference to the root directory for the share.
    azure::storage::cloud_file_directory root_dir = share.get_root_directory_reference();

Most, hogy a legfelső szintű directory, a megosztás hivatkozást, feltöltheti egy fájlt, azt az alakzatot. Ez a példa egy fájlból, szöveget és adatfolyam feltölti.

    // Upload a file from a stream.
    concurrency::streams::istream input_stream = 
      concurrency::streams::file_stream<uint8_t>::open_istream(_XPLATSTR("DataFile.txt")).get();

    azure::storage::cloud_file file1 = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-1"));
    file1.upload_from_stream(input_stream);

    // Upload some files from text.
    azure::storage::cloud_file file2 = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-2"));
    file2.upload_text(_XPLATSTR("more text"));

    // Upload a file from a file.
    azure::storage::cloud_file file4 = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-3"));
    file4.upload_from_file(_XPLATSTR("DataFile.txt"));  

## <a name="how-to-create-a-directory"></a>Útmutató: könyvtár létrehozása

Tárterület rendezése fájlok bízza meg az összes oldalról a legfelső szintű címtárban alkönyvtárak belül helyezésének. Az Azure fájlt tároló szolgáltatás lehetővé teszi, a fiókja lehetővé teszi, hogy minél nagyobb könyvtárak létrehozása. Az alábbi kód **saját minta címtár** csoportban a legfelső szintű könyvtár, valamint a **saját minta alkönyvtár**nevű nevű könyvtárában hoz létre.
    
    // Retrieve a reference to a directory
    azure::storage::cloud_file_directory directory = share.get_directory_reference(_XPLATSTR("my-sample-directory"));
    
    // Return value is true if the share did not exist and was successfully created.
    directory.create_if_not_exists();
    
    // Create a subdirectory.
    azure::storage::cloud_file_directory subdirectory = 
      directory.get_subdirectory_reference(_XPLATSTR("my-sample-subdirectory"));
    subdirectory.create_if_not_exists();

## <a name="how-to-list-files-and-directories-in-a-share"></a>Útmutató: a lista fájlok és mappák a megosztás

Fájlok és mappák belüli megosztás partnerlistáját hívja fel a **list_files_and_directories** **cloud_file_directory** hivatkozást a könnyen történik. Hozzáférés a multimédiás tulajdonságokat és a visszaadott **list_file_and_directory_item**módszerei, meg kell hívni a **list_file_and_directory_item.as_file** megszerezni egy **cloud_file** objektumot vagy a **list_file_and_directory_item.as_directory** módszerek megszerezni egy **cloud_file_directory** objektumot.

A következő kódot bemutatja, hogyan olvashat be és az egyes cikkek a legfelső szintű címtárban a megosztás URI kimeneti.

    //Get a reference to the root directory for the share.
    azure::storage::cloud_file_directory root_dir = 
      share.get_root_directory_reference();
    
    // Output URI of each item.
    azure::storage::list_file_and_diretory_result_iterator end_of_results;
    
    for (auto it = directory.list_files_and_directories(); it != end_of_results; ++it)
    {
        if(it->is_directory())
        {
            ucout << "Directory: " << it->as_directory().uri().primary_uri().to_string() << std::endl;
        }
        else if (it->is_file())
        {
            ucout << "File: " << it->as_file().uri().primary_uri().to_string() << std::endl;
        }       
    }
    

## <a name="how-to-download-a-file"></a>Útmutató: a fájl letöltése

Fájlok letöltése, először beolvashatja egy hivatkozást, és akkor hívja meg a fájl tartalmának küldjenek egy adatfolyam-objektumok, amelyek, majd a helyi fájlként is megmaradnak a **download_to_stream** módszert. Azt is megteheti töltse le a fájl tartalmát egy helyi fájl a **download_to_file** módszerrel is használhatja. A **download_text** módszer segítségével töltse le a csatolást karaktersorozatot fájl tartalmát.

Az alábbi példában a **download_to_stream** és **download_text** módszerek bemutatják, hogy az előző szakaszban létrehozott fájlok letöltését.

    // Download as text
    azure::storage::cloud_file text_file = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-2"));
    utility::string_t text = text_file.download_text();
    ucout << "File Text: " << text << std::endl;
    
    // Download as a stream.
    azure::storage::cloud_file stream_file = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-3"));
    
    concurrency::streams::container_buffer<std::vector<uint8_t>> buffer;
    concurrency::streams::ostream output_stream(buffer);
    stream_file.download_to_stream(output_stream);
    std::ofstream outfile("DownloadFile.txt", std::ofstream::binary);
    std::vector<unsigned char>& data = buffer.collection();
    outfile.write((char *)&data[0], buffer.size());
    outfile.close();

## <a name="how-to-delete-a-file"></a>Útmutató: fájl törlése

Egy másik közös fájlt tároló művelet a fájlok törlését. A következő kódot nevű saját-minta-fájl-3 – a legfelső szintű könyvtárban tárolt fájl törlése

    // Get a reference to the root directory for the share. 
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));
    
    azure::storage::cloud_file_directory root_dir = 
      share.get_root_directory_reference();
    
    azure::storage::cloud_file file = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-3"));
    
    file.delete_file_if_exists();

## <a name="how-to-delete-a-directory"></a>Útmutató: könyvtár törlése

Egy egyszerű feladat könyvtárában törlése parancs, bár a Megjegyzendő, hogy nem törölhetők a fájlok továbbra is tartalmazó könyvtár vagy más könyvtárak.
    
    // Get a reference to the share.
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));
    
    // Get a reference to the directory.
    azure::storage::cloud_file_directory directory = 
      share.get_directory_reference(_XPLATSTR("my-sample-directory"));
    
    // Get a reference to the subdirectory you want to delete.
    azure::storage::cloud_file_directory sub_directory =
      directory.get_subdirectory_reference(_XPLATSTR("my-sample-subdirectory"));
    
    // Delete the subdirectory and the sample directory.
    sub_directory.delete_directory_if_exists();
    
    directory.delete_directory_if_exists();

## <a name="how-to-delete-a-share"></a>Útmutató: megosztás törlése

Hívja fel a **delete_if_exists** módszer cloud_file_share objektum törlése a megosztás történik. Az alábbiakban a minta kódot, amely tartalmaz.
    
    // Get a reference to the share.
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));
    
    // delete the share if exists
    share.delete_share_if_exists();

## <a name="set-the-maximum-size-for-a-file-share"></a>A fájlmegosztás maximális méret beállítása

A fájlmegosztás gigabájtban beállíthatja az kvóta (vagy a maximális méret). Érdemes is tekintheti meg, hogy mennyi adatot tárolt a megosztott.

Megosztás kvótája megadásával korlátozhatja a megosztott tárolt fájlok összesített méretét. Ha a megosztott fájlok teljes mérete meghaladja a kvóta beállítása a megosztott, majd ügyfelek tudják meglévő fájlok méretének növelése vagy új fájlok létrehozása, kivéve, ha azokat a fájlokat nem üres.

Az alábbi példában látható, az aktuális használat megosztáshoz ellenőrzése és a megosztás kvóta beállítása.

    // Parse the connection string for the storage account.
    azure::storage::cloud_storage_account storage_account = 
      azure::storage::cloud_storage_account::parse(storage_connection_string);
    
    // Create the file client.
    azure::storage::cloud_file_client file_client = 
      storage_account.create_cloud_file_client();
    
    // Get a reference to the share.
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));
    if (share.exists())
    {
        std::cout << "Current share usage: " << share.download_share_usage() << "/" << share.properties().quota();
    
        //This line sets the quota to 2560GB
        share.resize(2560);
    
        std::cout << "Quota increased: " << share.download_share_usage() << "/" << share.properties().quota();
    
    }

## <a name="generate-a-shared-access-signature-for-a-file-or-file-share"></a>Fájl vagy fájlmegosztás átengedése aláírás létrehozása

A megosztott access aláírás (Társítások) fájlmegosztásra vagy egy külön fájlba hozhat létre. A megosztott hozzáférési házirend kezelheti a megosztott access aláírások fájlmegosztáson is létrehozhat. Megosztott access-házirend létrehozása ajánlott, lehetővé teszi a Társítások vonhat vissza, ha meg kell sérül eszközei.

Az alábbi példa létrehoz egy megosztott hozzáférési házirend megosztáson, és házirendhez segítségével egy Társítások található fájlok megosztása a szükséges az érvényességi tartományán.

    // Parse the connection string for the storage account.
    azure::storage::cloud_storage_account storage_account = 
      azure::storage::cloud_storage_account::parse(storage_connection_string);
    
    // Create the file client and get a reference to the share
    azure::storage::cloud_file_client file_client = 
      storage_account.create_cloud_file_client();
    
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));
    
    if (share.exists())
    {
        // Create and assign a policy
        utility::string_t policy_name = _XPLATSTR("sampleSharePolicy");

        azure::storage::file_shared_access_policy sharedPolicy = 
          azure::storage::file_shared_access_policy();

        //set permissions to expire in 90 minutes
        sharedPolicy.set_expiry(utility::datetime::utc_now() + 
          utility::datetime::from_minutes(90));

        //give read and write permissions
        sharedPolicy.set_permissions(azure::storage::file_shared_access_policy::permissions::write | azure::storage::file_shared_access_policy::permissions::read);

        //set permissions for the share
        azure::storage::file_share_permissions permissions; 

        //retrieve the current list of shared access policies
        azure::storage::shared_access_policies<azure::storage::file_shared_access_policy> policies;
        
        //add the new shared policy
        policies.insert(std::make_pair(policy_name, sharedPolicy));

        //save the updated policy list
        permissions.set_policies(policies);
        share.upload_permissions(permissions);

        //Retrieve the root directory and file references
        azure::storage::cloud_file_directory root_dir = 
          share.get_root_directory_reference();
        azure::storage::cloud_file file = 
          root_dir.get_file_reference(_XPLATSTR("my-sample-file-1"));

        // Generate a SAS for a file in the share 
        //  and associate this access policy with it.       
        utility::string_t sas_token = file.get_shared_access_signature(sharedPolicy);
        
        // Create a new CloudFile object from the SAS, and write some text to the file.     
        azure::storage::cloud_file file_with_sas(azure::storage::storage_credentials(sas_token).transform_uri(file.uri().primary_uri()));
        utility::string_t text = _XPLATSTR("My sample content");        
        file_with_sas.upload_text(text);        
        
        //Download and print URL with SAS.
        utility::string_t downloaded_text = file_with_sas.download_text();      
        ucout << downloaded_text << std::endl;      
        ucout << azure::storage::storage_credentials(sas_token).transform_uri(file.uri().primary_uri()).to_string() << std::endl;
    
    }

Létrehozásával és átengedése aláírások használatával kapcsolatos további tudnivalókért lásd: [Segítségével a megosztott Access aláírások (Társítások)](storage-dotnet-shared-access-signature-part-1.md).

## <a name="next-steps"></a>Következő lépések

Azure tároló olvashat az alábbi források:

-   [Tárterület ügyfél dokumentumtár a C++](https://github.com/Azure/azure-storage-cpp)

-   [Azure tároló Explorer](http://go.microsoft.com/fwlink/?LinkID=822673&clcid=0x409)

-   [Azure tároló dokumentáció](https://azure.microsoft.com/documentation/services/storage/)
