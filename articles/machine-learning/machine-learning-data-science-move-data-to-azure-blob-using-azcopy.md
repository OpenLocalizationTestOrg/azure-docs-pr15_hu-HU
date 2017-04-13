<properties
    pageTitle="Adatok áthelyezése és az Azure Blob-tárolóhoz AzCopy használatával |} Microsoft Azure"
    description="Adatok áthelyezése és az Azure Blob-tárolóhoz AzCopy használatával"
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

# <a name="move-data-to-and-from-azure-blob-storage-using-azcopy"></a>Adatok áthelyezése és az Azure Blob-tárolóhoz AzCopy használatával

AzCopy tölt fel, le, és az adatok másolása, és a Microsoft Azure blob, fájlt és táblatároló készült parancssori segédprogramot.

Telepítése AzCopy és további információt a használat és a Azure platform, tanulmányozza [Az első lépések a AzCopy parancssori segédprogram](../storage/storage-use-azcopy.md).

Adatok áthelyezése és/vagy az Azure Blob-tárolóhoz használt technológiák útmutatást a itt kapcsolódnak:

[AZURE.INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]


> [AZURE.NOTE] Ha használ, úgy lett beállítva az [Azure-ban adatok tudományos virtuális gépeken futó](machine-learning-data-science-virtual-machines.md)által biztosított parancsfájlok virtuális, majd AzCopy már telepítve van a virtuális.

> [AZURE.NOTE] Egy teljes – bevezetés Azure blob-tárolóhoz olvassa el a [Azure Blob](../storage/storage-dotnet-how-to-use-blobs.md) -alapokhoz és [Azure Blob](https://msdn.microsoft.com/library/azure/dd179376.aspx)-szolgáltatásba.


## <a name="prerequisites"></a>Előfeltételek

A dokumentum feltételezi, hogy rendelkezik-e az Azure előfizetéssel, a tárterület-fiók és a megfelelő tároló billentyűt az adott fiókhoz. Adatok feltöltése és letöltése, mielőtt ismernie kell a tároló Azure nevét és a fiók fiókkulcs.

- Szeretne beállítani egy Azure-előfizetést, olvassa el a [hónap próba-előfizetésre](https://azure.microsoft.com/pricing/free-trial/)című témakört.

- Útmutatást a tárterület-fiók létrehozása és a fiók és a fontos információkat ismertető [kapcsolatos Azure tároló fiókok](../storage/storage-create-storage-account.md)témakörben találhatók.


## <a name="run-azcopy-commands"></a>Parancsok AzCopy futtatása

AzCopy parancsokat, nyisson meg egy parancs ablakot, és nyissa meg a számítógépen, ahol a végrehajtható AzCopy.exe található AzCopy telepítési könyvtárat. 

Egyszerű AzCopy parancsok szintaxisa:

    AzCopy /Source:<source> /Dest:<destination> [Options]

>[AZURE.NOTE] Vegye fel a AzCopy telepítési helyet a rendszer elérési útját, és futtassa a parancsok bármely címtárból. Alapértelmezés szerint AzCopy *% ProgramFiles(x86) %\Microsoft SDKs\Azure\AzCopy* vagy *%ProgramFiles%\Microsoft SDKs\Azure\AzCopy*van telepítve.

## <a name="upload-files-to-an-azure-blob"></a>Fájlok feltöltése az Azure blob

Fájl feltöltéséhez használja az alábbi parancsot:

    # Upload from local file system
    AzCopy /Source:<your_local_directory> /Dest: https://<your_account_name>.blob.core.windows.net/<your_container_name> /DestKey:<your_account_key> /S


## <a name="download-files-from-an-azure-blob"></a>Fájlok letöltése az Azure blob

Töltse le a fájl egy Azure blob, használja az alábbi parancsot:

    # Downloading blobs to local file system
    AzCopy /Source:https://<your_account_name>.blob.core.windows.net/<your_container_name>/<your_sub_directory_at_blob>  /Dest:<your_local_directory> /SourceKey:<your_account_key> /Pattern:<file_pattern> /S


## <a name="transfer-blobs-between-azure-containers"></a>Tárolók Azure BLOB átvitele

Azure tárolók között adhatja át a BLOB, használja a következő parancsot:

    # Transferring blobs between Azure containers
    AzCopy /Source:https://<your_account_name1>.blob.core.windows.net/<your_container_name1>/<your_sub_directory_at_blob1> /Dest:https://<your_account_name2>.blob.core.windows.net/<your_container_name2>/<your_sub_directory_at_blob2> /SourceKey:<your_account_key1> /DestKey:<your_account_key2> /Pattern:<file_pattern> /S

    <your_account_name>: your storage account name
    <your_account_key>: your storage account key
    <your_container_name>: your container name
    <your_sub_directory_at_blob>: the sub directory in the container
    <your_local_directory>: directory of local file system where files to be uploaded from or the directory of local file system files to be downloaded to
    <file_pattern>: pattern of file names to be transferred. The standard wildcards are supported


## <a name="tips-for-using-azcopy"></a>Tippek a AzCopy

> [AZURE.TIP]   
> 1. Amikor a fájlok **feltöltése** */S* feltölti a fájlok rekurzív. Ez a paraméter nélkül a alkönyvtárak fájlok nem feltöltve.  
> 2. Ha a fájl **letöltése** */S* addig, amíg az összes fájl a megadott könyvtárat és alkönyvtárait vagy az összes fájl, amely megfelel a megadott mintának az adott könyvtárat és alkönyvtárait a keresést végez a tároló rekurzív, töltődnek le.  
> 3.  Egy **adott blob-fájl** letöltése a *forrás* paraméter használatával nem adhat meg. Egy adott fájl letöltéséhez fájlnevet blob letöltése a */Pattern* paraméter használatával. Keresse meg a fájl neve mintát rekurzív AzCopy van **/S** paraméter használható. A minta paraméter nélkül a AzCopy könyvtár lévő összes fájl tölthető le.
