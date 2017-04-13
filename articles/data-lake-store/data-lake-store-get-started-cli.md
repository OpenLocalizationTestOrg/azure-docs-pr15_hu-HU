<properties
   pageTitle="Első lépések – platformok parancssori felület használatával tó adattár |} Microsoft Azure"
   description="Azure-platformok parancs segítségével tó adattár fiók létrehozásához, és végezze el az alapműveletek"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/27/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-azure-command-line"></a>Első lépések az Azure tó adattár Azure parancssor használatával

> [AZURE.SELECTOR]
- [Portál](data-lake-store-get-started-portal.md)
- [A PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST API-VAL](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [NODE.js](data-lake-store-manage-use-nodejs.md)

Megtudhatja, hogy miként Azure parancssor segítségével tó adattár Azure-fiók létrehozása, és olyan mappában, létrehozásakor fájlok feltöltése és letöltése adatok alapvető műveleteket, törölje a fiókot, stb. Tó adattár kapcsolatos további tudnivalókért olvassa el a [Áttekintése a tó adattár](data-lake-store-overview.md)című témakört.

Az Azure CLI Node.js a megvalósított. Bármely platformon Node.js, beleértve a Windows, Mac és Linux támogató használható. Az Azure CLI Megnyitás. A forráskód <a href= "https://github.com/azure/azure-xplat-cli">https://github.com/azure/azure-xplat-cli</a>a GitHub van kezelése. Ez a cikk bemutatja, hogy csak az Azure CLI használata tó adattár. Az általános útmutató Azure CLI használatát, megtudhatja, [hogy miként használhatja az Azure CLI] [azure-command-line-tools].


##<a name="prerequisites"></a>Előfeltételek

Ez a cikk megkezdése előtt a következőket kell rendelkeznie:

- **Az Azure-előfizetés**. Lásd: [Ismerkedés az Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/).

- **Azure CLI** – lásd: [Telepítse és állítsa be az Azure CLI](../xplat-cli-install.md) telepítési és konfigurálási információt. Győződjön meg arról, hogy a CLI telepítése után indítsa újra a számítógépet.

## <a name="authentication"></a>Hitelesítés

Ez a cikk a egyszerűbb hitelesítési megközelítést adatok tó áruházból, amikor bejelentkezik az végfelhasználói felhasználók használja. A hozzáférési szint fiók és a fájlrendszerben majd tevékenységére a bejelentkezett felhasználó hozzáférési szintjének tó adattár. Vannak azonban olyan is, hogy a hitelesítő adatok tó áruházzal más módszer, hogy mely **végfelhasználói** vagy **szolgáltatás hitelesítés használatával**. Utasításokat és hitelesítést végezni kapcsolatos tudnivalók című témakörben talál [tó áruházzal hitelesítés használatával az Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

##<a name="login-to-your-azure-subscription"></a>Jelentkezzen be az Azure előfizetés

1. Kövesse a lépéseket a [Microsoft Azure-előfizetésbe a az Azure parancssori kezelőfelületről Azure](../xplat-cli-connect.md) dokumentált és az előfizetés használatával csatlakozhat a `azure login` módot.

2. Az előfizetések társított a fiók használatával lista a `azure account list` parancsot.

        info:    Executing command account list
        data:    Name              Id                                    Current
        data:    ----------------  ------------------------------------  -------
        data:    Azure-sub-1       ####################################  true
        data:    Azure-sub-2       ####################################  false

    A fenti eredménye az **Azure-sub-1** be van kapcsolva, és a más típusú előfizetés **Azure-sub-2**. 

3. Válassza ki a használni kívánt az előfizetést. Ha az Azure-sub-2-előfizetés a használni kívánt, használja a `azure account set` parancsot.

        azure account set Azure-sub-2


## <a name="create-an-azure-data-lake-store-account"></a>Azure tó adattár fiók létrehozása

Nyissa meg a parancssort, rendszerhéj vagy a munkamenet, és futtassa az alábbi parancsokat.

2. Váltás Azure erőforrás-kezelő módban használja a következő parancsot:

        azure config mode arm


5. Hozzon létre egy új erőforráscsoport. Az a következő parancsot adja meg a használni kívánt paraméterértékeket.

        azure group create <resourceGroup> <location>

    Ha a hely neve szóközöket tartalmaz, helyezze idézőjelek közé. Ha például "Kelet-Amerikai Egyesült Államok 2".

5. Hozza létre a tó adattár fiókot.

        azure datalake store account create <dataLakeStoreAccountName> <location> <resourceGroup>

## <a name="create-folders-in-your-data-lake-store"></a>Mappák létrehozása az adatok tó tárolása

Mappák kezeléséhez és adattárolásra Azure tó adattár fiókhoz tartozó hozhat létre. A következő parancs használatával hozzon létre egy "mynewfolder" nevű mappát a legfelső szintű az adatok tó áruházból.

    azure datalake store filesystem create <dataLakeStoreAccountName> <path> --folder

Példa:

    azure datalake store filesystem create mynewdatalakestore /mynewfolder --folder

## <a name="upload-data-to-your-data-lake-store"></a>Feltölteni az adatokat a tó adattárhoz

Az adatok is feltölthet, közvetlenül a gyökérszinten tó adattár vagy egy mappába a számla belüli létrehozott. A kódrészletek, az alábbi bemutatják, hogy miként tölthet fel mintaadatokat tartalmaz az előző szakaszban létrehozott mappát (**mynewfolder**).

Ha tölthet fel mintaadatokat is tartalmazó keres, a az [Azure adatok tó mely számjegy tárházba](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData)kattint a **Mentővel adatok** mappát. Töltse le a fájlt, és tárolja a számítógépen, például C:\sampledata helyi könyvtárában található\.

    azure datalake store filesystem import <dataLakeStoreAccountName> "<source path>" "<destination path>"

Példa:

    azure datalake store filesystem import mynewdatalakestore "C:\SampleData\AmbulanceData\vehicle1_09142014.csv" "/mynewfolder/vehicle1_09142014.csv"


## <a name="list-files-in-data-lake-store"></a>Lista fájlok tó adattárhoz

A következő paranccsal egy tó adattár fiókban a fájlok listája.

    azure datalake store filesystem list <dataLakeStoreAccountName> <path>

Példa:

    azure datalake store filesystem list mynewdatalakestore /mynewfolder

A kimenet az adott kell lennie az alábbihoz hasonló:

    info:    Executing command datalake store filesystem list
    data:    accessTime: 1446245025257
    data:    blockSize: 268435456
    data:    group: NotSupportYet
    data:    length: 1589881
    data:    modificationTime: 1446245105763
    data:    owner: NotSupportYet
    data:    pathSuffix: vehicle1_09142014.csv
    data:    permission: 777
    data:    replication: 0
    data:    type: FILE
    data:    ------------------------------------------------------------------------------------
    info:    datalake store filesystem list command OK

## <a name="rename-download-and-delete-data-from-your-data-lake-store"></a>Nevezze át, töltse le és adatok törlése az adatok tó áruházból

* **Nevezze át a fájlt**, használja az alábbi parancsot:

        azure datalake store filesystem move <dataLakeStoreAccountName> <path/old_file_name> <path/new_file_name>

    Példa:

        azure datalake store filesystem move mynewdatalakestore /mynewfolder/vehicle1_09142014.csv /mynewfolder/vehicle1_09142014_copy.csv

* **Egy fájl letöltésére**, a következő parancsot használja. Győződjön meg arról, hogy a rendeltetési hely elérési út már létezik.

        azure datalake store filesystem export <dataLakeStoreAccountName> <source_path> <destination_path>

    Példa:

        azure datalake store filesystem export mynewdatalakestore /mynewfolder/vehicle1_09142014_copy.csv "C:\mysampledata\vehicle1_09142014_copy.csv"

* **Fájl törlése**, használja az alábbi parancsot:

        azure datalake store filesystem delete <dataLakeStoreAccountName> <path>

    Példa:

        azure datalake store filesystem delete mynewdatalakestore /mynewfolder/vehicle1_09142014_copy.csv

    Amikor a rendszer kéri, adja meg az **Y** az elem törlése.

## <a name="view-the-access-control-list-for-a-folder-in-data-lake-store"></a>A hozzáférési tó-tárolóban mappákra vonatkozó lista nézet

A következő paranccsal megtekintheti a hozzáférés-vezérlési listák adatok tó tároló mappa. Az aktuális programcsomag hozzáférés-vezérlési listák beállítható, hogy csak a legfelső szintű az adatok tó tároló. Igen az alábbi elérési út paraméter értéke mindig legfelső szintű (/).

    azure datalake store permissions show <dataLakeStoreName> <path>

Példa:

    azure datalake store permissions show mynewdatalakestore /


## <a name="delete-your-data-lake-store-account"></a>A tó adattár fiók törlése

A következő paranccsal tó adattár fiók törlése.

    azure datalake store account delete <dataLakeStoreAccountName>

Példa:

    azure datalake store account delete mynewdatalakestore

Amikor a rendszer kéri, adja meg az **Y** a fiók törléséhez.


## <a name="next-steps"></a>Következő lépések

- [Biztonságos adattár tó adatok](data-lake-store-secure-data.md)
- [Azure adatok tó Analytics használata tó adattárhoz](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure hdinsight szolgáltatáshoz használata tó adattárhoz](data-lake-store-hdinsight-hadoop-use-portal.md)


[azure-command-line-tools]: ../xplat-cli-install.md
