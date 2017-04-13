<properties
    pageTitle="Az Azure CLI használata Azure tároló |} Microsoft Azure"
    description="Megtudhatja, hogy miként használhatja az Azure parancssori kezelőfelületről Azure Azure adathordozós és hozhat létre és tároló fiókokat Azure BLOB- és a fájlok használata. Az Azure CLI platformok segédprogram "
    services="storage"
    documentationCenter="na"
    authors="micurd"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="micurd"/>

# <a name="using-the-azure-cli-with-azure-storage"></a>Azure tároló az Azure CLI használata

## <a name="overview"></a>– Áttekintés

Az Azure CLI biztosít a forrás megnyitása platformok parancsok az Azure Platform használatához. Nagy, az [Azure portál](https://portal.azure.com) , valamint a rich-adatok access működésére található ugyanazokat a szolgáltatásokat biztosít.

Ebből az útmutatóból mutatunk be az [Azure parancssori kezelőfelületről Azure](../xplat-cli-install.md) használatáról a különböző Azure adathordozós fejlesztés és felügyeleti feladatok elvégzéséhez. Azt javasoljuk, hogy letöltése és telepítése vagy frissítése a legújabb Azure CLI Ez az útmutató használata előtt.

Ez az útmutató feltételezi, hogy megismeri a alapfogalmak Azure-tárterületét. Az útmutató többféle parancsfájlokat az Azure adathordozós az Azure CLI használatát mutatja be. Ügyeljen arra, hogy az Ön konfigurációjának minden parancsprogram futtatása előtt parancsfájl változók frissítése.

> [AZURE.NOTE] Az útmutató a példákat az Azure CLI parancs és a parancsprogram klasszikus tároló fiókok. Lásd: az erőforrás-kezelő tárterület-fiókok parancsok [használata az Azure CLI for Mac, Linux, és a Windows Azure az erőforrás-kezelés és](../virtual-machines/azure-cli-arm-commands.md#azure-storage-commands-to-manage-your-storage-objects) Azure CLI.

## <a name="get-started-with-azure-storage-and-the-azure-cli-in-5-minutes"></a>Első lépések az Azure-tárhely és a Azure CLI az 5 perc

Ez az útmutató Ubuntu használja a példákat, de más operációs rendszerek hasonlóan kell végeznie.

**Azure új:** Microsoft Azure előfizetés és e-előfizetéséhez társított Microsoft-fiókkal juthat. Azure vásárlás információt lásd: [Ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/), [Vásárolja beállítások](https://azure.microsoft.com/pricing/purchase-options/)és [Tag kínál](https://azure.microsoft.com/pricing/member-offers/) (az MSDN webhelyen, a Microsoft Partner Network és BizSpark és más Microsoft-szoftverekhez tagjai).

Lásd: a [rendszergazdai szerepkörök hozzárendelése az Azure Active Directory (Azure Active Directory)](https://msdn.microsoft.com/library/azure/hh531793.aspx) Azure előfizetések kapcsolatban további tudnivalókat.

**Miután létrehozott egy Microsoft Azure-előfizetést és a fiók:**

1. Töltse le és telepítse az Azure CLI, [telepítse az Azure CLI](../xplat-cli-install.md)ismertetett lépéseket követve.
2. Az Azure CLI telepítése után fogja használni a parancssor (Bash, Terminálszolgáltatások, parancssor) az azure parancs az Azure CLI parancsok elérése. Típus `azure` parancsot, és meg kell jelennie a következő eredményt.

    ![Azure parancs][Image1]

3. Írja be a parancssor `azure storage` ki az azure tárterület-parancsok listára, és kérjen egy benyomást a funkciók a Azure CLI biztosít. Beírhatja a parancs neve **-h** paraméterrel (például `azure storage share create -h`) parancs szintaxisának részleteinek.
4. Most fog ad egy egyszerű parancsfájl, amely mutatja az alapvető Azure CLI parancsok Azure tároló eléréséhez. A parancsprogram először kérni fogja a tárhely és a kulcs két változó beállítása. Ezután a parancsfájl hozzon létre egy új tároló a tárhely új fiókba, és töltse fel a meglévő kép fájl (blob) tároló. Után a parancsfájl valamennyi BLOB-tároló a listák, akkor a képfájlt letölti a cél könyvtárra, amely megtalálható a helyi számítógépen.

        #!/bin/bash
        # A simple Azure storage example

        export AZURE_STORAGE_ACCOUNT=<storage_account_name>
        export AZURE_STORAGE_ACCESS_KEY=<storage_account_key>

        export container_name=<container_name>
        export blob_name=<blob_name>
        export image_to_upload=<image_to_upload>
        export destination_folder=<destination_folder>

        echo "Creating the container..."
        azure storage container create $container_name

        echo "Uploading the image..."
        azure storage blob upload $image_to_upload $container_name $blob_name

        echo "Listing the blobs..."
        azure storage blob list $container_name

        echo "Downloading the image..."
        azure storage blob download $container_name $blob_name $destination_folder

        echo "Done"

5. A helyi számítógépen nyissa meg a használni kívánt szövegszerkesztőben (például vim). A fenti parancsfájlt beírja a szövegszerkesztőben.

6. Most módosítania kell a parancsprogram változók a konfigurációs beállítások alapján.

    - **< storage_account_name >** Az Utónév használata a parancsfájl, vagy adjon meg egy új nevet a tárolási fiók. **Fontos:** A tároló fiók neve Azure egyedinek kell lennie. Legyen nagybetű, túl!

    - **< storage_account_key >** A tároló fiók hívóbetű.

    - **< container_name >** Használja a megadott névvel a parancsfájl, vagy írja be a tárolót új nevét.

    - **< image_to_upload >** Írjon be egy elérési utat kép a helyi számítógépen, például: "~ / images/HelloWorld.png".

    - **< destination_folder >** Írjon be egy elérési utat tárolni, például az Azure-tárból letöltött fájlokat helyi könyvtár: "~/downloadImages".

7. A szükséges változók vim frissítése után nyomja le az egyes billentyűparancsok fő használati "Esc,:, wq!" szeretné menteni a parancsfájl.

8. Futtassa a parancsfájlt, egyszerűen írja be a parancsfájl nevét a bash konzolhoz. Ez a parancsfájl futtatása után egy helyi célmappára, amely tartalmazza a letöltött képfájl kell rendelkeznie. Az alábbi képernyőképen egy példa kimenetét mutatja be:

A parancsprogram futtatása után rendelkeznie kell a helyi célmappát, amely tartalmazza a letöltött képfájl.

## <a name="manage-storage-accounts-with-the-azure-cli"></a>Az Azure CLI fiókok tárhely kezelése

### <a name="connect-to-your-azure-subscription"></a>Csatlakozás az Azure előfizetés

Miközben a legtöbb tároló parancs az Azure előfizetéssel nélkül fog működni, javasoljuk, hogy az Azure CLI az előfizetés csatlakozni. Az előfizetése készült Azure CLI beállításához kövesse a [Csatlakozás az Azure CLI az Azure-előfizetésbe](../xplat-cli-connect.md).

### <a name="create-a-new-storage-account"></a>Tárterület új fiók létrehozása

Azure tárterületet használ, szüksége lesz egy tárterület-fiókkal. Új Azure tároló fiók hozhat létre, miután beállította a számítógépen, az előfizetés csatlakozni.

        azure storage account create <account_name>

Tárterület-fiókja nevét közötti 3 és 24 karakter hosszúságú legyen, és használja a számokat és csak a kisbetűk betűket.

### <a name="set-a-default-azure-storage-account-in-environment-variables"></a>Egy alapértelmezett Azure tárterület-fiók beállítása a környezeti változók

Több tárhely fiókot az előfizetésben is. Válasszon egyet közülük, és az adott munkamenetben beállítja a tárterület-parancsok környezet változói. Lehetővé teszi, hogy a parancsokat Azure CLI tároló megadása a tárterület-fiók nélkül, és a főbb explicit módon.

        export AZURE_STORAGE_ACCOUNT=<account_name>
        export AZURE_STORAGE_ACCESS_KEY=<key>

Kapcsolati karakterlánc úgy is, hogy egy alapértelmezett tároló fiókot beállítani használ. Először ismerkedjen parancs a kapcsolati karakterláncot:

        azure storage account connectionstring show <account_name>

Ezután másolja a kimeneti kapcsolati karakterláncot, és állítsa be a környezeti:

        export AZURE_STORAGE_CONNECTION_STRING=<connection_string>

## <a name="create-and-manage-blobs"></a>Létrehozhatja és kezelheti a BLOB

Azure Blob-tárolóhoz olyan szolgáltatás, a nagy mennyiségű strukturálatlan adatokat, például a szöveg és a bináris adatokat, bárhol is elérhető a világ HTTP-és HTTPS tárolásához. Ez a szakasz tartalma feltételezi, hogy Ön már jól ismert az Azure Blob-tároló fogalmak. Információ a [Azure Blob-tárolóhoz .NET használata – első lépések](storage-dotnet-how-to-use-blobs.md) és [Blob-szolgáltatási fogalmak](http://msdn.microsoft.com/library/azure/dd179376.aspx)témakörben talál.

### <a name="create-a-container"></a>A tároló létrehozása

Azure-tárolóban lévő minden blob tároló kell lennie. A magánjellegű tároló használatával hozhat létre a `azure storage container create` parancsot:

        azure storage container create mycontainer

> [AZURE.NOTE] A névtelen olvasási hozzáférést három szinten: **kikapcsolása** **Blob**és **tároló**. Név nélküli hozzáférés BLOB elkerülése a jogosultsági paraméter értéke **Kikapcsolva**. Alapértelmezés szerint a új tároló privát, és csak a fiók tulajdonosa által is elérhető. Szeretné, hogy névtelen nyilvános olvasási hozzáférést blob-erőforrásokhoz, de nem tároló metaadatok vagy a BLOB-tárolóban listájára, állítsa a jogosultsági paraméter **Blob**. Szeretné, hogy a teljes körű nyilvános olvasási hozzáférést blob-erőforrások, tároló metaadatok és BLOB-tárolóban listájának, állítsa a jogosultsági paraméter **tároló**. További tudnivalókért olvassa el a [kezelés névtelen olvasási hozzáférést tárolók és BLOB](storage-manage-access-to-resources.md)című témakört.

### <a name="upload-a-blob-into-a-container"></a>A tároló blob feltölteni

Azure Blob-tároló letiltása BLOB és a lap BLOB támogatja. [További információ ismertetése blokk BLOB, hozzáfűző BLOB és](http://msdn.microsoft.com/library/azure/ee691964.aspx)megtekintése lapon BLOB

Töltse fel a BLOB tároló, használja a `azure storage blob upload`. Alapértelmezés szerint ez a parancs a helyi fájlok feltölti a továbbfejlesztett fájlblokkolás blob. Adja meg a blob típusát, használja a `--blobtype` paraméter.

        azure storage blob upload '~/images/HelloWorld.png' mycontainer myBlockBlob

### <a name="download-blobs-from-a-container"></a>Töltse le a BLOB tároló

A következő példa bemutatja, hogyan lehet töltse le a BLOB tároló.

        azure storage blob download mycontainer myBlockBlob '~/downloadImages/downloaded.png'

### <a name="copy-blobs"></a>BLOB másolása

Aszinkron másolhatja BLOB belül, illetve a tárhely fiókok és régiók.

A következő példa bemutatja, hogyan BLOB másolása egyik tárterület-fiókból egy másikra. Ez a példa a hol találhatók BLOB nyilvánosan, tároló hozzunk létre névtelenül érhető el.

    azure storage container create mycontainer2 -a <accountName2> -k <accountKey2> -p Blob

    azure storage blob upload '~/Images/HelloWorld.png' mycontainer2 myBlockBlob2 -a <accountName2> -k <accountKey2>

    azure storage blob copy start 'https://<accountname2>.blob.core.windows.net/mycontainer2/myBlockBlob2' mycontainer

Ez a példa egy aszinkron másolatot hajt végre. Minden egyes fájlmásolás állapotának futtatásával figyelheti a `azure storage blob copy show` művelet.

Megjegyzendő, hogy a forrás URL-címe megadva a Másolás kell nyilvánosan hozzáférhető vagy Társítások (átengedése aláírás) jogkivonat tartalmazza.

### <a name="delete-a-blob"></a>Blob törlése

Blob törléséhez használja az alábbi parancsot:

        azure storage blob delete mycontainer myBlockBlob2

## <a name="create-and-manage-file-shares"></a>Létrehozhatja és kezelheti a megosztott fájl

Azure tárhely a szabványos kis-és Középvállalatok protokollt használó alkalmazásai megosztott tárhelyet biztosít. Microsoft Azure virtuális gépeken futó és felhőszolgáltatások, valamint a helyszíni környezetbe applications, megoszthatja a fájlban lévő adatok csatlakoztatott megosztások keresztül. Fájlmegosztások és az Azure CLI keresztül fájlban lévő adatok kezelése Azure-tárhelyet a további tudnivalókért lásd: [Ismerkedés a Windows Azure fájltároló](storage-dotnet-how-to-use-files.md) vagy [Azure fájltároló Linux használatáról](storage-how-to-use-files-linux.md).

### <a name="create-a-file-share"></a>Fájlmegosztás létrehozása

Egy Azure fájlmegosztás egy kis-és Középvállalatok fájlmegosztás Azure-ban. Könyvtárak és a fájlok fájlmegosztás kell létrehozni. Fiók megosztások tetszőleges számú is tartalmazhat, és a megosztás tárolhatja a fájlokat, a tárhely fiók kapacitás kereteken belül tetszőleges számú. Az alábbi példa létrehoz egy **megosztás**nevű fájlmegosztás.

        azure storage share create myshare

### <a name="create-a-directory"></a>Könyvtár létrehozása

A címtár-választható hierarchikus nyújt a egy Azure fájlmegosztás. Az alábbi példa létrehoz egy **könyvtárnév** fájl megosztása nevű könyvtárában.

        azure storage directory create myshare myDir

Megjegyzés: az elérési út címtár tartalmazhat több szinten, *például* **a és b**. Jó helyen jár győződjön meg arról, hogy az összes szülő könyvtárak létezik. Például az elérési út **a és b**, kell létrehozása címtár **egy** első, majd **b**könyvtár létrehozása.

### <a name="upload-a-local-file-to-directory"></a>Helyi fájl feltöltése címtár

Az alábbi példa a **könyvtárnév** címtár- **~/temp/samplefile.txt** fájl feltölti. A fájl elérési útja szerkesztése, fölé egy érvényes fájlba, a helyi számítógépen:

        azure storage file upload '~/temp/samplefile.txt' myshare myDir

Ne feledje, hogy a fájl megosztása is mérete legfeljebb 1 TB.

### <a name="list-the-files-in-the-share-root-or-directory"></a>A megosztás legfelső szintű vagy könyvtár a fájlok listája

A fájlok és a legfelső szintű megosztás vagy a következő paranccsal könyvtár alkönyvtárak jeleníthetők meg:

        azure storage file list myshare myDir

Ne feledje, hogy a könyvtár nevét a nevére a partnerlistában művelet nem kötelező. Adja meg, ha a parancs a megosztás a legfelső szintű könyvtár tartalmát jeleníti meg.

### <a name="copy-files"></a>Fájlok másolása

Azure CLI 0.9.8 verziójának kezdve, is fájl másolása egy másik fájlt, és blob blob-fájlba. Az alábbi azt szemléltetik feladatokat hajtanak végre ezeket másolás CLI parancsaival. Az új könyvtár másolni egy fájlt:

    azure storage file copy start --source-share srcshare --source-path srcdir/hello.txt --dest-share destshare --dest-path destdir/hellocopy.txt --connection-string $srcConnectionString --dest-connection-string $destConnectionString

Másolandó blob fájl könyvtár:

    azure storage file copy start --source-container srcctn --source-blob hello2.txt --dest-share hello --dest-path hellodir/hello2copy.txt --connection-string $srcConnectionString --dest-connection-string $destConnectionString

## <a name="next-steps"></a>Következő lépések

Íme, néhány kapcsolódó cikkek és forrásokat is tartalmaz az Azure tároló többet.

- [Azure tároló dokumentáció](https://azure.microsoft.com/documentation/services/storage/)
- [Azure tároló REST API-hivatkozás](https://msdn.microsoft.com/library/azure/dd179355.aspx)


[Image1]: ./media/storage-azure-cli/azure_command.png
