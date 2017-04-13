<properties
    pageTitle="A fonetikus (objektum tároló) Blob-tárolóhoz használata |} Microsoft Azure"
    description="Strukturálatlan adatokat tárolja az Azure Blob-tárolóhoz (objektum tároló) a felhőben."
    services="storage"
    documentationCenter="ruby"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="ruby"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="tamram"/>


# <a name="how-to-use-blob-storage-from-ruby"></a>A fonetikus Blob-tárolóhoz használata

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>– Áttekintés

Azure Blob-tárolóhoz olyan strukturálatlan adatokat tároló a felhőben, objektumok és BLOB szolgáltatás. BLOB-tárolóhoz bármilyen szöveget vagy a bináris adatokat, például egy dokumentum, médiafájlok vagy alkalmazás installer tárolhat. BLOB-tárolóhoz is nevezik objektum tárhelyet.

Ez az útmutató megtudhatja, hogyan végezhetők el esetei Blob-tárolóhoz használatával. A minták kerülnek a fonetikus API. Az érintett esetek **feltöltése, listaelem, le,** és **törlése** BLOB.

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a>Fonetikus-alkalmazás létrehozása

Fonetikus-alkalmazás létrehozása. Című cikkben olvashat [a sínek webalkalmazás-Azure virtuális fonetikus](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md)

## <a name="configure-your-application-to-access-storage"></a>A tároló eléréséhez konfigurálása

Azure tároló használata esetén szüksége letöltéséről és használatáról a fonetikus, kényelmesebbé olyan tárat, amellyel kommunikálni a tároló többi szolgáltatást tartalmaz azure csomagot.

### <a name="use-rubygems-to-obtain-the-package"></a>A csomag beszerzése RubyGems használatával

1. Használja a parancssor, például **PowerShell** (Windows), a **terminált** (Mac) vagy a **Bash** (Unix).

2. Írja be a parancssorablakban telepítheti a gem és függőségek "gem telepítés azure".

### <a name="import-the-package"></a>A csomag importálása

A fonetikus fájlt, ahol tároló használni kívánt tetején a kedvenc szövegszerkesztővel, adja hozzá a következő:

    require "azure"

## <a name="setup-an-azure-storage-connection"></a>Az Azure tárterület-kapcsolat beállítása

Az azure modul felirat jelenik meg a környezeti változók **AZURE\_tároló\_fiók** és **AZURE\_tároló\_ACCESS_KEY** az Azure tároló fiókhoz való csatlakozáshoz szükséges információkat. Ha ezek a környezeti változók nincsenek beállítva, meg kell adnia a fiók adatait, mielőtt **Azure::Blob::BlobService** használata a következő kódot:

    Azure.config.storage_account_name = "<your azure storage account>"
    Azure.config.storage_access_key = "<your azure storage access key>"


Ezek az értékek beolvasása a klasszikus vagy az erőforrás-kezelő tároló fiókot az Azure-portálon:

1. Jelentkezzen be az [Azure-portálon](https://portal.azure.com).
2. Nyissa meg a használni kívánt tárterület-fiókjába.
3. Kattintson a jobb oldalon a beállítások lap, a **Hívóbillentyűk**.
4. Az Access billentyűk lap látható láthatja, a hívóbetű 1 és 2 hívóbetű. Bármelyikéhez használhatja. 
5. Kattintson a Másolás ikonra a kulcs másolja a vágólapra. 

Ezek az értékek beszerzése a klasszikus Azure portálon klasszikus tárterület-fiókból:

1. Jelentkezzen be a [Klasszikus Azure portálon](https://manage.windowsazure.com).
2. Nyissa meg a használni kívánt tárterület-fiókjába.
3. Kattintson a **HÍVÓBETŰK kezelése** a navigációs ablak alján.
4. Az előugró párbeszédpanel látni fogja a tárhely fiók nevét, az access elsődleges kulcs és másodlagos hívóbetű. Az access-billentyűt vagy az elsődleges adott, vagy a másodlagos is is használhatja. 
5. Kattintson a Másolás ikonra a kulcs másolja a vágólapra.

## <a name="create-a-container"></a>A tároló létrehozása

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

A **Azure::Blob::BlobService** objektum lehetővé teszi a tárolók és BLOB. A tároló hozhat létre a **létrehozása\_container()** módot.

Az alábbi kód példa létrehoz egy tároló vagy nyomtassa ki a hiba, ha bármelyik.

    azure_blob_service = Azure::Blob::BlobService.new
    begin
      container = azure_blob_service.create_container("test-container")
    rescue
      puts $!
    end

Ha azt szeretné, végezze el a fájlokat a tároló nyilvános, beállíthatja, hogy a tároló engedélyei.

Csak módosíthatja a <strong>létrehozása\_container()</strong> átadni a hívást a **: nyilvános\_access\_szint** beállítást:

    container = azure_blob_service.create_container("test-container",
      :public_access_level => "<public access level>")


Az érvényes értékek a **: nyilvános\_access\_szint** lehetőség van:

* **blob:** Adja meg a teljes körű nyilvános olvasási hozzáférést tároló és blob-adatokhoz. Ügyfelek BLOB névtelen elküldött kérésre tárolóban is számbavétele, de nem számba veheti a tárhely számla belüli tárolók.

* **tároló:** Adja meg a nyilvános olvasási hozzáférést BLOB. Névtelen elküldött kérésre BLOB-adatok a tárolóban is olvashatók, de tároló adatok nem érhető el. Ügyfelek nem számba veheti a névtelen elküldött kérésre tárolóban BLOB.

Azt is megteheti, módosíthatja a tároló nyilvános hozzáférési szintjének használatával **beállítása\_tároló\_acl()** használatával adja meg a nyilvános hozzáférési szintet.

Az alábbi példa a nyilvános hozzáférési szintet **tároló**módosult hivatkozás:

    azure_blob_service.set_container_acl('test-container', "container")

## <a name="upload-a-blob-into-a-container"></a>A tároló blob feltölteni

Tartalmak feltöltése blob, használja a **létrehozása\_blokk\_blob()** módszer a blob, létrehozásához használni egy fájlt vagy a karakterlánc a blob tartalmát.

A következő kódot a fájl **test.png** feltöltések megjelenítése az "kép-blob" a tárolóban nevű új blob-ként.

    content = File.open("test.png", "rb") { |file| file.read }
    blob = azure_blob_service.create_block_blob(container.name,
      "image-blob", content)
    puts blob.name

## <a name="list-the-blobs-in-a-container"></a>A BLOB tárolóban lista

A tárolók listáját, a **list_containers()** módszert.
A tároló belül BLOB listában használja **lista\_blobs()** módot.

Ez az URL-címét a fiókom összes tárolókban valamennyi BLOB jelenít meg.

    containers = azure_blob_service.list_containers()
    containers.each do |container|
      blobs = azure_blob_service.list_blobs(container.name)
      blobs.each do |blob|
        puts blob.name
      end
    end

## <a name="download-blobs"></a>BLOB letöltése

BLOB letöltéséhez használja a **első\_blob()** módszer beolvasásához tartalmát.

A következő példa bemutatja, hogy segítségével **első\_blob()** töltse le a "kép-blob" tartalmát, és írni a helyi fájl.

    blob, content = azure_blob_service.get_blob(container.name,"image-blob")
    File.open("download.png","wb") {|f| f.write(content)}

## <a name="delete-a-blob"></a>Blob törlése
Végezetül blob törléséhez használja a **törlése\_blob()** módot. A következő példa bemutatja, hogyan blob törlése.

    azure_blob_service.delete_blob(container.name, "image-blob")

## <a name="next-steps"></a>Következő lépések

Összetettebb tároló feladatok kapcsolatos további tudnivalókért kövesse ezeket a hivatkozásokat:

- [Azure tároló csapatának blogja](http://blogs.msdn.com/b/windowsazurestorage/)
- [Azure SDK fonetikus](https://github.com/WindowsAzure/azure-sdk-for-ruby) tárházából GitHub
- [A AzCopy parancssori segédprogram az adatok átvitele](storage-use-azcopy.md)
