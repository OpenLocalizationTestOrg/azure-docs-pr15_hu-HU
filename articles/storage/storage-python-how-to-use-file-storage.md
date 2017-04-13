<properties
    pageTitle="Azure-tárhelyet a Python használata |} Microsoft Azure"
    description="Megtudhatja, hogy miként az Azure fájlt tároló Python feltöltése, a listában, a letöltés, és törölje a fájlokat."
    services="storage"
    documentationCenter="python"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="robinsh"/>

# <a name="how-to-use-azure-file-storage-from-python"></a>Azure-tárhelyet a Python használata

[AZURE.INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-files](../../includes/storage-try-azure-tools-files.md)]

## <a name="overview"></a>– Áttekintés

Ez a cikk bemutatja, hogyan a fájlt tároló esetei végrehajtásához. A minták Python írt, és használja a [Microsoft Azure tároló SDK Python tartalmaz]. Az érintett esetek feltöltése, listaelem, letöltéséről és fájlok törlése.

[AZURE.INCLUDE [storage-file-concepts-include](../../includes/storage-file-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-share"></a>Megosztás létrehozása

A **FileService** objektum lehetővé teszi a megosztás, könyvtárak és fájlokat. A következő kódot létrehoz egy **FileService** -objektumot. Adja hozzá a következő bármely Python fájlt, amelyben el szeretné érni programozás útján a Azure tároló felső részén.

    from azure.storage.file import FileService

A következő kódot objektumot hoz létre **FileService** a tárhely fiók nevét, és a fiók gombbal.  Cserélje "saját fiók" és "SajátKulcs" a fiók nevét és billentyűt.

    file_service = **FileService** (account_name='myaccount', account_key='mykey')

A következő példában kód egy **FileService** -objektum segítségével létrehozása a megosztást, ha még nem létezik.

    file_service.create_share('myshare')

## <a name="upload-a-file-into-a-share"></a>Fájl feltöltése a megosztás

Egy Azure fájlmegosztás tároló tartalmaz legalább, legfelső szintű könyvtárában fájlokat tároló is. Ebben a szakaszban megismerheti fogja mindig a legfelső szintű könyvtár megosztás helyi tárhelyről fájl feltöltése.

Hozzon létre egy fájlt, és feltölteni az adatokat, használhatja a **létrehozása\_fájl\_a\_elérési út**, **létrehozása\_fájl\_a\_adatfolyam**, **létrehozása\_fájl\_a\_bájt** vagy **létrehozása\_fájl\_a\_szöveg** módszereket. Azok a szükséges chunking, ha az adatok mérete meghaladja a 64 MB végrehajtása magas szintű módszereket.

**Hozzon létre\_fájl\_a\_elérési út** feltölt egy fájlt a megadott elérési tartalmának és **létrehozása\_fájl\_a\_adatfolyam** feltölt egy már megnyitott fájl adatfolyam tartalmát. **Hozzon létre\_fájl\_a\_bájt** bájt tömb feltölti és **létrehozása\_fájl\_a\_szöveg** feltölti a megadott szöveges értéket a megadott kódolással (alapértelmezés szerint UTF-8).

Az alábbi példa a **saját_fájl** fájlba feltöltések megjelenítése az **sunset.png** fájl tartalmát.

    from azure.storage.file import ContentSettings
    file_service.create_file_from_path(
        'myshare',
        None, # We want to create this blob in the root directory, so we specify None for the directory_name
        'myfile',
        'sunset.png',
        content_settings=ContentSettings(content_type='image/png'))

## <a name="how-to-create-a-directory"></a>Útmutató: könyvtár létrehozása

Tárterület rendezése elhelyez fájlok bízza meg az összes oldalról a legfelső szintű címtárban alszint könyvtárak belül. Az Azure fájlt tároló szolgáltatás lehetővé teszi lehetővé teszi, hogy a fiók könyvtárat létrehozása. Az alábbi kód **sampledir** a legfelső szintű könyvtárban nevű alszint könyvtárában hoz létre.

    file_service.create_directory('myshare', 'sampledir')

## <a name="how-to-list-files-and-directories-in-a-share"></a>Útmutató: a lista fájlok és mappák a megosztás

Fájlok és a megosztás könyvtárak listáját, használja a **lista\_könyvtárak\_és\_fájlok** módot. Ez a módszer a nyilvántartás-készítő alkalmazás adja eredményül. A következő kódot exportálja a minden fájl- és a megosztás a konzolhoz **nevét** .

    generator = file_service.list_directories_and_files('myshare')
    for file_or_dir in generator:
        print(file_or_dir.name)

## <a name="download-files"></a>Fájlok letöltése

Fájlból adatok letöltéséhez használható **első\_fájl\_való\_elérési út**, **első\_fájl\_való\_adatfolyam**, **első\_fájl\_való\_bájt**, vagy **első\_fájl\_való\_szöveg**. Azok a szükséges chunking, ha az adatok mérete meghaladja a 64 MB végrehajtása magas szintű módszereket.

Az alábbi példa bemutatja, hogy segítségével **első\_fájl\_való\_elérési út** letöltése a **saját_fájl** fájl tartalmát, és a **kimenő-sunset.png** fájlt tárolja.

    file_service.get_file_to_path('myshare', None, 'myfile', 'out-sunset.png')

## <a name="delete-a-file"></a>Fájl törlése

Ha törölni szeretne egy fájlt, végül **delete_file**hívja.

    file_service.delete_file('myshare', None, 'myfile')

## <a name="next-steps"></a>Következő lépések

Most, hogy megtanulta fájltároló alapjait, kövesse az alábbi hivatkozásokra kattintva további.

- [Python Developer Center](/develop/python/)
- [Azure tárolása REST API-val](http://msdn.microsoft.com/library/azure/dd179355)
- [Azure tároló csapatának blogja]
- [Microsoft Azure tároló Python SDK]

[Azure tároló csapatának blogja]: http://blogs.msdn.com/b/windowsazurestorage/
[Microsoft Azure tároló Python SDK]: https://github.com/Azure/azure-storage-python
