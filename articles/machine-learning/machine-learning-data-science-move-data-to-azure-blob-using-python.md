<properties
    pageTitle="Adatok áthelyezése és az Azure Blob-tárolóhoz Python használatával |} Microsoft Azure"
    description="Adatok áthelyezése és az Azure Blob-tárolóhoz Python használatával"
    services="machine-learning,storage"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="bradsev" />

# <a name="move-data-to-and-from-azure-blob-storage-using-python"></a>Adatok áthelyezése és az Azure Blob-tárolóhoz Python használatával

Ez a témakör ismerteti a lista feltöltése és letöltése a Python API BLOB. Az Azure SDK leírt Python API van lehetősége:

- A tároló létrehozása
- A tároló blob feltölteni
- BLOB letöltése
- A BLOB tárolóban lista
- Blob törlése

A Python API használatával kapcsolatos további tudnivalókért lásd: [Python Blob Tárhelyszolgáltatása használatáról](../storage/storage-python-how-to-use-blob-storage.md).

Adatok áthelyezése és/vagy az Azure Blob-tárolóhoz használt technológiák útmutatást a itt kapcsolódnak:

[AZURE.INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]


> [AZURE.NOTE] Ha használ, úgy lett beállítva az [Azure-ban adatok tudományos virtuális gépeken futó](machine-learning-data-science-virtual-machines.md)által biztosított parancsfájlok virtuális, majd AzCopy már telepítve van a virtuális.

> [AZURE.NOTE] Egy teljes – bevezetés Azure blob-tárolóhoz olvassa el a [Azure Blob](../storage/storage-dotnet-how-to-use-blobs.md) -alapokhoz és [Azure Blob](https://msdn.microsoft.com/library/azure/dd179376.aspx)-szolgáltatásba.


## <a name="prerequisites"></a>Előfeltételek

A dokumentum feltételezi, hogy rendelkezik-e az Azure előfizetéssel, a tárterület-fiók és a megfelelő tároló billentyűt az adott fiókhoz. Adatok feltöltése és letöltése, mielőtt ismernie kell a tároló Azure nevét és a fiók fiókkulcs.

- Szeretne beállítani egy Azure-előfizetést, olvassa el a [hónap próba-előfizetésre](https://azure.microsoft.com/pricing/free-trial/)című témakört.

- Útmutatást a tárterület-fiók létrehozása és a fiók és a fontos információkat ismertető [kapcsolatos Azure tároló fiókok](../storage/storage-create-storage-account.md)témakörben találhatók.


## <a name="upload-data-to-blob"></a>Adatok Blob feltöltése

Adja hozzá a következő kódtöredékének bármely Python kód, amelyben el szeretné érni programozás útján a Azure tároló felső részén:

    from azure.storage.blob import BlobService

A **BlobService** objektum lehetővé teszi a tárolók és BLOB. A következő kódot objektumot hoz létre BlobService a tárhely fiók nevét, és a fiók gombbal. Cserélje le a fiók nevét, és a fiókkulcs valós fiók és billentyűt.

    blob_service = BlobService(account_name="<your_account_name>", account_key="<your_account_key>")

Az alábbi módszerekkel blob feltölteni az adatokat:

1. Helyezze\_blokk\_blob\_a\_elérési utat (feltölt egy fájlt a megadott elérési tartalmának)
2. Helyezze\_block_blob\_a\_(feltölti a tartalmat egy már megnyitott fájl adatfolyam) fájl
3. Helyezze\_blokk\_blob\_a\_bájt (feltöltések egy tömb bájtok számát)
4. Helyezze\_blokk\_blob\_a\_szöveg (feltölti a megadott szöveges értéket a megadott kódolást használ)

A következő példa kódot egy helyi fájlt tároló feltölti:

    blob_service.put_block_blob_from_path("<your_container_name>", "<your_blob_name>", "<your_local_file_name>")

A következő példa kódot blob-tárolóhoz helyi könyvtárában található feltöltések megjelenítése az összes fájlt (kivéve a könyvtárak):

    from azure.storage.blob import BlobService
    from os import listdir
    from os.path import isfile, join

    # Set parameters here
    ACCOUNT_NAME = "<your_account_name>"
    ACCOUNT_KEY = "<your_account_key>"
    CONTAINER_NAME = "<your_container_name>"
    LOCAL_DIRECT = "<your_local_directory>"     

    blob_service = BlobService(account_name=ACCOUNT_NAME, account_key=ACCOUNT_KEY)
    # find all files in the LOCAL_DIRECT (excluding directory)
    local_file_list = [f for f in listdir(LOCAL_DIRECT) if isfile(join(LOCAL_DIRECT, f))]

    file_num = len(local_file_list)
    for i in range(file_num):
        local_file = join(LOCAL_DIRECT, local_file_list[i])
        blob_name = local_file_list[i]
        try:
            blob_service.put_block_blob_from_path(CONTAINER_NAME, blob_name, local_file)
        except:
            print "something wrong happened when uploading the data %s"%blob_name


## <a name="download-data-from-blob"></a>Adatok letöltése a Blob

Az alábbi módszerekkel adatok letöltése a blob:
1. első\_blob\_való\_elérési út
2. első\_blob\_való\_fájl
3. első\_blob\_való\_bájt
4. első\_blob\_való\_szöveg

Módszerekben végre a szükséges chunking, ha az adatok mérete meghaladja a 64 MB.

A következő példa kódot tárolóban blob tartalmának helyi fájlként tölthető le:

    blob_service.get_blob_to_path("<your_container_name>", "<your_blob_name>", "<your_local_file_name>")

A következő példa kódot tároló valamennyi BLOB letölti. Akkor használja a lista\_úgy tekintheti meg a rendelkezésre álló BLOB listáját a tároló BLOB- és letölti a helyi könyvtár.

    from azure.storage.blob import BlobService
    from os.path import join

    # Set parameters here
    ACCOUNT_NAME = "<your_account_name>"
    ACCOUNT_KEY = "<your_account_key>"
    CONTAINER_NAME = "<your_container_name>"
    LOCAL_DIRECT = "<your_local_directory>"     

    blob_service = BlobService(account_name=ACCOUNT_NAME, account_key=ACCOUNT_KEY)

    # List all blobs and download them one by one
    blobs = blob_service.list_blobs(CONTAINER_NAME)
    for blob in blobs:
        local_file = join(LOCAL_DIRECT, blob.name)
        try:
            blob_service.get_blob_to_path(CONTAINER_NAME, blob.name, local_file)
        except:
            print "something wrong happened when downloading the data %s"%blob.name
