<properties
    pageTitle="A PHP (objektum tároló) blob-tárolóhoz használata |} Microsoft Azure"
    description="Strukturálatlan adatokat tárolja az Azure Blob-tárolóhoz (objektum tároló) a felhőben."
    documentationCenter="php"
    services="storage"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="how-to-use-blob-storage-from-php"></a>Használatáról a PHP blob-tárolóhoz

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>– Áttekintés

Azure Blob-tárolóhoz olyan strukturálatlan adatokat tároló a felhőben, objektumok és BLOB szolgáltatás. BLOB-tárolóhoz bármilyen szöveget vagy a bináris adatokat, például egy dokumentum, médiafájlok vagy alkalmazás installer tárolhat. BLOB-tárolóhoz is nevezik objektum tárhelyet.

Ez az útmutató megtudhatja, miként végezheti el a gyakori alkalmazási területek, az Azure blob-szolgáltatás használatával. A minták PHP írt, és használja az [Azure SDK PHP] [download]. Az érintett esetek **feltöltése**, **listaelem**, **letöltése**és **törlése** BLOB. BLOB a további tudnivalókért lásd: a [következő lépésekkel](#next-steps) szakaszban.

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a>PHP-alkalmazás létrehozása

Csak egy az Azure blob-szolgáltatás hozzáférő PHP-alkalmazás létrehozása kötelező, a hivatkozási az Azure SDK az osztályok a PHP-belül a kódot. Bármely Fejlesztőeszközök hozhat létre a az alkalmazásokban, például a Jegyzettömbben.

Ebből az útmutatóból a helyben vagy az Azure webes szerepkör, a dolgozó szerepkör és a webhely futó kód egy PHP alkalmazáson belül nevű szolgáltatások használata.

## <a name="get-the-azure-client-libraries"></a>Ismerkedés az Azure ügyfél-tárak

[AZURE.INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-access-the-blob-service"></a>Az alkalmazás elérése a blob-szolgáltatás konfigurálása

Az Azure blob-szolgáltatás API-khoz használatához kell:

1. Hivatkozás a automatikus betöltő fájlt a [require_once] utasítással és
2. Hivatkozás bármelyik osztályok használhat.

A következő példa bemutatja, hogyan lehet az automatikus betöltő fájl és hivatkozást a **ServicesBuilder** osztály.

> [AZURE.NOTE] Ez a példa (és más, a jelen cikkben szereplő példák) tegyük fel, a PHP ügyfél képtárak Azure zeneszerző keresztül telepítette. A tárak manuálisan telepíteni, szükségességének hivatkozni szeretne a `WindowsAzure.php` automatikus betöltő fájlt.

    require_once 'vendor/autoload.php';
    use WindowsAzure\Common\ServicesBuilder;


Az alábbi példában a `require_once` utasítás mindig megjelenik, de csak az osztályok a példa végrehajtásához szükséges hivatkozunk.

## <a name="set-up-an-azure-storage-connection"></a>Azure tárterület-alapú beállítása

Az Azure blob-szolgáltatás ügyfél létrehozni, előbb érvényes kapcsolat-karakterlánc kell rendelkeznie. Az a blob-szolgáltatás kapcsolati karakterlánc esetén:

Élő szolgáltatás eléréséhez:

    DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]

A tároló irányító eléréséhez:

    UseDevelopmentStorage=true


Azure service jegyzetlapon létrehozásához kell használni az **ServicesBuilder** osztály. képes vagy:

* Átadni a kapcsolati karakterlánc közvetlenül vagy
* Jelölje be a kapcsolati karakterlánc több külső forrásból használja a **CloudConfigurationManager (CCM)** :
    * Alapértelmezés szerint akkor megtalálható egy külső adatforrás - környezeti változók támogatása.
    * Új források a **ConnectionStringSource** osztály időtartamának is hozzáadhat.

Az itt ismertetett példák a kapcsolati karakterlánc fog átadandó közvetlenül.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;

    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);

## <a name="create-a-container"></a>A tároló létrehozása

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

Egy **BlobRestProxy** -objektum segítségével blob-tároló **createContainer** módszerrel. Tároló létrehozásakor beállíthatja, hogy a tároló beállításai, de ez így nem szükséges. (Az alábbi példa mutatja container hozzáférési szabálygyűjteményt (vezérlés) és a tároló metaadatok beállítása.)

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Blob\Models\CreateContainerOptions;
    use MicrosoftAzure\Storage\Blob\Models\PublicAccessType;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    // OPTIONAL: Set public access policy and metadata.
    // Create container options object.
    $createContainerOptions = new CreateContainerOptions();

    // Set public access policy. Possible values are
    // PublicAccessType::CONTAINER_AND_BLOBS and PublicAccessType::BLOBS_ONLY.
    // CONTAINER_AND_BLOBS:
    // Specifies full public read access for container and blob data.
    // proxys can enumerate blobs within the container via anonymous
    // request, but cannot enumerate containers within the storage account.
    //
    // BLOBS_ONLY:
    // Specifies public read access for blobs. Blob data within this
    // container can be read via anonymous request, but container data is not
    // available. proxys cannot enumerate blobs within the container via
    // anonymous request.
    // If this value is not specified in the request, container data is
    // private to the account owner.
    $createContainerOptions->setPublicAccess(PublicAccessType::CONTAINER_AND_BLOBS);

    // Set container metadata.
    $createContainerOptions->addMetaData("key1", "value1");
    $createContainerOptions->addMetaData("key2", "value2");

    try {
        // Create container.
        $blobRestProxy->createContainer("mycontainer", $createContainerOptions);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

Hívás **setPublicAccess (PublicAccessType::CONTAINER\_és\_BLOB)** teszi az adatok tároló és blob névtelen kérések keresztül érhető el. Hívja fel a **setPublicAccess(PublicAccessType::BLOBS_ONLY)** teszi csak blob-adatok névtelen kérések keresztül. Hozzáférés-vezérlési listák tároló kapcsolatos további tudnivalókért lásd a [beállítása tároló vezérlés (REST API -val)][container-acl].

Blob-szolgáltatás hibakódok további információkért lásd a [Blob-szolgáltatás hibakódok][error-codes].

## <a name="upload-a-blob-into-a-container"></a>A tároló blob feltölteni

Blob-ként fájl feltöltése, használja a **BlobRestProxy -> createBlockBlob** módszert. Ez a művelet a blob hoz létre, ha nem létezik, vagy felülírja ezt, ha létezik. Az alábbi példa feltételezi, hogy a tároló már létrehozott és a [fopen] [ fopen] adatfolyamként a fájl megnyitásához.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    $content = fopen("c:\myfile.txt", "r");
    $blob_name = "myblob";

    try {
        //Upload blob
        $blobRestProxy->createBlockBlob("mycontainer", $blob_name, $content);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

Figyelje meg, hogy az előző minta feltöltések megjelenítése az adatfolyamként blob. Azonban blob is tölthet fel egy karakterlánc használja, mint például a [fájl\_első\_tartalma] [ file_get_contents] függvény. Ehhez használja az előző minta módosítása `$content = fopen("c:\myfile.txt", "r");` való `$content = file_get_contents("c:\myfile.txt");`.

## <a name="list-the-blobs-in-a-container"></a>A BLOB tárolóban lista

A BLOB tárolóban listában használata a **BlobRestProxy -> listBlobs** módszer **foreach** Szülőalakzathoz való hurok át az eredményt. A következő kódot minden blob nevét jeleníti meg ezt az eredményt tárolóban, és megjeleníti a felhasználó saját URI a böngészőben.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    try {
        // List blobs.
        $blob_list = $blobRestProxy->listBlobs("mycontainer");
        $blobs = $blob_list->getBlobs();

        foreach($blobs as $blob)
        {
            echo $blob->getName().": ".$blob->getUrl()."<br />";
        }
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }


## <a name="download-a-blob"></a>Blob letöltése

Blob letöltéséhez hívja fel a **BlobRestProxy -> getBlob** módszert, majd a **getContentStream** módszer telefonál az eredményül kapott **GetBlobResult** objektum.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    try {
        // Get blob.
        $blob = $blobRestProxy->getBlob("mycontainer", "myblob");
        fpassthru($blob->getContentStream());
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

Figyelje meg, hogy a fenti példa blob kap adatfolyam erőforrásként (az alapértelmezés). Azonban használhatja a [adatfolyam\_első\_tartalma] [ stream-get-contents] függvény, a visszaadott adatfolyam konvertálandó karakterlánc.

## <a name="delete-a-blob"></a>Blob törlése

Blob törléséhez átadni tároló nevét és blob **-> deleteBlob BlobRestProxy**.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    try {
        // Delete blob.
        $blobRestProxy->deleteBlob("mycontainer", "myblob");
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

## <a name="delete-a-blob-container"></a>Törölje a blob-tárolóhoz

Végezetül blob-tároló törléséhez átadni a tároló neve **BlobRestProxy -> deleteContainer**.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    try {
        // Delete container.
        $blobRestProxy->deleteContainer("mycontainer");
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

## <a name="next-steps"></a>Következő lépések

Most, hogy megtanulta az Azure blob-szolgáltatás alapjait, kövesse ezeket a hivatkozásokat, ha többet szeretne tudni az összetettebb tároló tevékenységek.

- Keresse fel az [Azure tároló csapatának blogja](http://blogs.msdn.com/b/windowsazurestorage/)
- Lásd az [PHP blokk blob példa](https://github.com/WindowsAzure/azure-sdk-for-php-samples/blob/master/storage/BlockBlobExample.php).
- Lásd az [PHP lap blob példa](https://github.com/WindowsAzure/azure-sdk-for-php-samples/blob/master/storage/PageBlobExample.php).
- [A AzCopy parancssori segédprogram az adatok átvitele](storage-use-azcopy.md)
 
További tudnivalókért lásd még: a [PHP Developer Center](/develop/php/).


[download]: http://go.microsoft.com/fwlink/?LinkID=252473
[container-acl]: http://msdn.microsoft.com/library/azure/dd179391.aspx
[error-codes]: http://msdn.microsoft.com/library/azure/dd179439.aspx
[file_get_contents]: http://php.net/file_get_contents
[require_once]: http://php.net/require_once
[fopen]: http://www.php.net/fopen
[stream-get-contents]: http://www.php.net/stream_get_contents
