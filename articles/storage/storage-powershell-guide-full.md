<properties
    pageTitle="Azure PowerShell használata az Azure tároló |} Microsoft Azure"
    description="Megtudhatja, hogy miként Azure tárolására Azure PowerShell-parancsmagok használata; tárterület-fiókok létrehozása és kezelése BLOB, a táblázatok, a sorok és a fájlok; Állítsa be, és tároló analytics lekérdezés átengedése aláírások létrehozása."
    services="storage"
    documentationCenter="na"
    authors="robinsh"
    manager="carmonm"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="robinsh"/>

# <a name="using-azure-powershell-with-azure-storage"></a>Azure adathordozós Azure PowerShell használatával

## <a name="overview"></a>– Áttekintés

Azure PowerShell modul, amely tartalmaz kezelése Azure – a Windows PowerShell-parancsmagok. Érdemes egy feladatalapú parancssori rendszerhéj és a tervezett, különösen a rendszer felügyeleti parancsfájlok futtatásának nyelv. A PowerShell egyszerűen szabályozhatja és a Azure szolgáltatások és alkalmazások felügyeleti automatizálása. Például a parancsmagok is használhatja ugyanazt az [Azure-portálon](https://portal.azure.com)keresztül elvégezhető műveletek elvégzéséhez.

Ebből az útmutatóból mutatunk be az [Azure tároló parancsmagok](https://msdn.microsoft.com/library/azure/mt269418.aspx) használatáról a különböző Azure adathordozós fejlesztés és felügyeleti feladatok elvégzéséhez.

Ez az útmutató feltételezi, hogy előzetes élmény [Azure-tárhely](https://azure.microsoft.com/documentation/services/storage/) és [A Windows PowerShell](http://technet.microsoft.com/library/bb978526.aspx)használatával. Az útmutató többféle parancsfájlokat az Azure adathordozós PowerShell használatát mutatja be. Az Ön konfigurációjának minden parancsprogram futtatása előtt parancsfájl változók frissítenie kell.

Az útmutató az első rész Azure-tárhely és a PowerShell gyors áttekintést nyújt. Részletes tudnivalókat és az utasításokat indítsa el az [Azure PowerShell Azure adathordozós előfeltételei](#prerequisites-for-using-azure-powershell-with-azure-storage).


## <a name="getting-started-with-azure-storage-and-powershell-in-5-minutes"></a>Első lépések Azure-tárhely és a PowerShell 5 perc múlva

Ez a szakasz bemutatja, hogyan az 5 perc Azure tároló PowerShell eléréséhez.

**Azure új:** Microsoft Azure előfizetés és e-előfizetéséhez társított Microsoft-fiókkal juthat. Azure vásárlás információt lásd: [Ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/), [Vásárolja beállítások](https://azure.microsoft.com/pricing/purchase-options/)és [Tag kínál](https://azure.microsoft.com/pricing/member-offers/) (az MSDN webhelyen, a Microsoft Partner Network és BizSpark és más Microsoft-szoftverekhez tagjai).

Lásd: a [rendszergazdai szerepkörök hozzárendelése az Azure Active Directory (Azure Active Directory)](https://msdn.microsoft.com/library/azure/hh531793.aspx) Azure előfizetések kapcsolatban további tudnivalókat.

**Miután létrehozott egy Microsoft Azure-előfizetést és a fiók:**

1.  Töltse le és telepítse az [Azure PowerShell](http://go.microsoft.com/?linkid=9811175&clcid=0x409).
2.  Útmutató a Windows PowerShell integrált parancsprogram-kezelés környezet (ISE): Helyi számítógépen nyissa meg a **Start** menü. Írja be a **Felügyeleti eszközök** ikonjára, majd kattintással indítsa el. A **Felügyeleti eszközök** ablakában kattintson a jobb gombbal a **Windows PowerShell ISE**, kattintson a **Futtatás rendszergazdaként**parancsra.
3.  A **Windows PowerShell ISE**, kattintson a **fájl** > **Új** hozzon létre egy új parancsprogram-fájlt.
4.  Most fog ad egy egyszerű parancsfájl, amely mutatja az Azure tároló elérése egyszerű PowerShell-parancsait. A parancsprogram előbb kérje meg a Azure a helyi PowerShell környezetet fiók hozzáadása az Azure a fiók hitelesítő adatait. Ezután a parancsfájl az alapértelmezett Azure előfizetés, és új tárterület-fiók létrehozása az Azure-ban. Ezután a parancsfájl hozzon létre egy új tároló a tárhely új fiókba, és töltse fel a meglévő kép fájl (blob) tároló. Után a parancsfájl valamennyi BLOB-tároló a listák, akkor fog könyvtár új cél létrehozása a helyi számítógépen, majd töltse le a képfájl.
5.  A következő kódot tartalmazó szakaszban jelölje be a megjegyzések között a parancsfájl **#begin** és **#end**. Nyomja le a CTRL + C billentyűkombinációval másolja a vágólapra.

        #begin
        # Update with the name of your subscription.
        $SubscriptionName = "YourSubscriptionName"

        # Give a name to your new storage account. It must be lowercase!
        $StorageAccountName = "yourstorageaccountname"

        # Choose "West US" as an example.
        $Location = "West US"

        # Give a name to your new container.
        $ContainerName = "imagecontainer"

        # Have an image file and a source directory in your local computer.
        $ImageToUpload = "C:\Images\HelloWorld.png"

        # A destination directory in your local computer.
        $DestinationFolder = "C:\DownloadImages"

        # Add your Azure account to the local PowerShell environment.
        Add-AzureAccount

        # Set a default Azure subscription.
        Select-AzureSubscription -SubscriptionName $SubscriptionName –Default

        # Create a new storage account.
        New-AzureStorageAccount –StorageAccountName $StorageAccountName -Location $Location

        # Set a default storage account.
        Set-AzureSubscription -CurrentStorageAccountName $StorageAccountName -SubscriptionName $SubscriptionName

        # Create a new container.
        New-AzureStorageContainer -Name $ContainerName -Permission Off

        # Upload a blob into a container.
        Set-AzureStorageBlobContent -Container $ContainerName -File $ImageToUpload

        # List all blobs in a container.
        Get-AzureStorageBlob -Container $ContainerName

        # Download blobs from the container:
        # Get a reference to a list of all blobs in a container.
        $blobs = Get-AzureStorageBlob -Container $ContainerName

        # Create the destination directory.
        New-Item -Path $DestinationFolder -ItemType Directory -Force  

        # Download blobs into the local destination directory.
        $blobs | Get-AzureStorageBlobContent –Destination $DestinationFolder
        #end

5.  A **Windows PowerShell ISE**nyomja le a CTRL + V billentyűkombinációval másolja a parancsfájlt. Kattintson a **fájl** > **menteni**. A **Mentés másként** párbeszédpanelen írja be a nevét a parancsfájl, például "mystoragescript". Kattintson a **Mentés**gombra.

6.  Most módosítania kell a parancsprogram változók a konfigurációs beállítások alapján. A saját előfizetéssel rendelkező frissítenie kell a **$SubscriptionName** változó. A további változót, amint az a parancsfájl megőrzése, vagy frissíteni őket a kívánt.

    - **$SubscriptionName:** A saját előfizetés nevű frissítenie kell a változó. Hajtsa végre az alábbi három módon keresse meg a nevét, az előfizetése egyikét:

        egy. A **Windows PowerShell ISE**, kattintson a **fájl** > **Új** hozzon létre egy új parancsprogram-fájlt. Az új parancsprogram másolja a következő parancsfájlt, és kattintson a **hibakeresési** > **futtatni**. A következő parancsfájl előbb kérje meg a Azure fiók hitelesítő adatait az Azure hozzáadása a helyi PowerShell környezetet fiók és jelenjen meg az előfizetések, amelyeket a helyi PowerShell-munkamenetet csatlakoztatott. Jegyezze fel a használata közben, ebben az oktatóanyagban számított kívánt előfizetés nevére:

            Add-AzureAccount
                Get-AzureSubscription | Format-Table SubscriptionName, IsDefault, IsCurrent, CurrentStorageAccountName

        b. Keresse meg és másolja a vágólapra az előfizetése nevére az [Azure-portált](https://portal.azure.com)a központi menüben kattintson a bal oldalon kattintson az **előfizetések elemre**. Másolja a vágólapra, amely ebből az útmutatóból a parancsfájlok futtatásakor használni kívánt előfizetés nevére.

        ![Azure portálon][Image2]

        c billentyűkombinációt. Keresse meg és másolja a vágólapra az előfizetése nevére az [Azure klasszikus Portal](https://manage.windowsazure.com/), görgessen le, és kattintson a **Beállítások** a portál bal oldalán. Kattintson az **előfizetés** az előfizetések listájának megtekintéséhez. Másolja a vágólapra, amely ebből az útmutatóból a megadott parancsfájlok futtatásakor használni kívánt előfizetés nevére.

        ![Azure klasszikus portál][Image1]

    - **$StorageAccountName:** Az Utónév használata a parancsfájl, vagy adjon meg egy új nevet a tárolási fiók. **Fontos:** A tároló fiók neve Azure egyedinek kell lennie. Legyen nagybetű, túl!

    - **$Location:** Az adott "nyugati USA" használata a parancsfájlt vagy más Azure helyekre, például kelet-Amerikai Egyesült Államok, Észak-Európa, válassza ki, és így tovább.

    - **$ContainerName:** Használja a megadott névvel a parancsfájl, vagy írja be a tárolót új nevét.

    - **$ImageToUpload:** Írja be a elérési útját egy képet a helyi számítógépen, például: "C:\Images\HelloWorld.png".

    - **$DestinationFolder:** Írjon be egy elérési utat tárolni, például az Azure-tárból letöltött fájlokat helyi könyvtár: "C:\DownloadImages".

7.  Miután frissítette a "mystoragescript.ps1" fájlban a parancsprogram változók, kattintson a **fájl** > **menteni**. Kattintson a **hibakeresési** > **, vagy nyomja le az **F5** futtatása** .  

A parancsprogram futtatása után rendelkeznie kell a helyi célmappát, amely tartalmazza a letöltött képfájl. Az alábbi képernyőképen egy példa kimenetét mutatja be:

![BLOB letöltése][Image3]


> [AZURE.NOTE] Az "Első lépések az Azure tárhely és a PowerShell az 5 perc" szakasz Azure PowerShell használata az Azure-tárolóhoz adni rövid útmutatást talál. Részletes tudnivalókat és az utasításokat javasoljuk, hogy olvassa el az alábbi szakaszok.

## <a name="prerequisites-for-using-azure-powershell-with-azure-storage"></a>Azure PowerShell használata az Azure tároló előfeltételei
Egy Azure előfizetés és ebből az útmutatóból megadott PowerShell-parancsmagok futtatásához a fentebb ismertetett fiók szükséges.

Azure PowerShell modul, amely tartalmaz kezelése Azure – a Windows PowerShell-parancsmagok. Telepítse és állítsa be a Azure PowerShell olvashat megtudhatja, [hogy miként telepítheti, állíthatja Azure PowerShell](../powershell-install-configure.md). Azt javasoljuk, hogy letöltése és telepítése vagy frissítése a legújabb Azure PowerShell-modult Ez az útmutató használata előtt.

A parancsmagok futtatását is lehetővé teszi a szabványos a Windows PowerShell-konzol vagy a Windows PowerShell integrált parancsfájlok környezet (ISE). Például **A Windows PowerShell ISE**megnyitásához nyissa meg a Start menü, írja be a felügyeleti eszközök és futtatásához kattintson. A felügyeleti eszközök ablakában kattintson a jobb gombbal a Windows PowerShell ISE, kattintson a Futtatás rendszergazdaként parancsra.

## <a name="how-to-manage-storage-accounts-in-azure"></a>Azure-ban tároló fiókok kezelése

### <a name="how-to-set-a-default-azure-subscription"></a>Egy alapértelmezett Azure előfizetés megadása
Azure PowerShell használatával Azure tárhely kezelése, szüksége a ügyfél-környezet Azure az Azure Active Directory vagy tanúsítvány-alapú hitelesítés használatával keresztül hitelesítést végezni. Részletes tudnivalókat nézze [telepítése és konfigurálása az Azure PowerShell](../powershell-install-configure.md) . Ez az útmutató az Azure Active Directory-hitelesítést használ.

1.  A Windows PowerShell ISE, írja be az Azure adja hozzá a következő parancsot a helyi PowerShell környezetet fiók:

    `Add-AzureAccount`

2.  A "Jelentkezzen be a Microsoft Azure" ablakban írja be az e-mail címét és a fiókjához tartozó jelszót. Azure hitelesíti menti a hitelesítő adatait, és kattintson az ablak bezárása.

3.  Ezután a következő parancsot a helyi PowerShell környezetet az Azure fiókok megtekintése, és ellenőrizze, hogy fiókja szerepel:

    `Get-AzureAccount`

4.  Ezután, futtassa a következő parancsmagot a előfizetéseket, amelyeket a helyi PowerShell-munkamenetet csatlakoztatott megtekintéséhez, és ellenőrizze, hogy szerepel-e az előfizetés:

    `Get-AzureSubscription | Format-Table SubscriptionName, IsDefault, IsCurrent, CurrentStorageAccountName`

5.  Egy alapértelmezett Azure előfizetés beállítása, futtassa a a Select-AzureSubscription parancsmagot:

        $SubscriptionName = 'Your subscription Name'
        Select-AzureSubscription -SubscriptionName $SubscriptionName –Default

6.  Ellenőrizze az alapértelmezett előfizetés nevét a Get-AzureSubscription parancsmagot:

    `Get-AzureSubscription -Default`

7.  Ha szeretné az összes rendelkezésre álló PowerShell-parancsmagok Azure tárolására, futtassa:

    `Get-Command -Module Azure -Noun *Storage*`

### <a name="how-to-create-a-new-azure-storage-account"></a>Azure tároló új fiók létrehozása
Azure tárterületet használ, szüksége lesz egy tárterület-fiókkal. Új Azure tároló fiók hozhat létre, miután beállította a számítógépen, az előfizetés csatlakozni.

1.  Futtassa a Get-AzureLocation parancsmagot a rendelkezésre álló adatközponthoz mindenhol keresése:

    `Get-AzureLocation | Format-Table -Property Name, AvailableServices, StorageAccountTypes`

2.  Ezután futtassa a New-AzureStorageAccount parancsmag tároló új fiók létrehozása. Az alábbi példa létrehoz egy új tárterület-fiókot a "USA-beli nyugati" adatközpontban.

        $location = "West US"
        $StorageAccountName = "yourstorageaccount"
        New-AzureStorageAccount –StorageAccountName $StorageAccountName -Location $location

> [AZURE.IMPORTANT] A tároló fiók neve Azure belül egyedinek kell lennie, és kisbetűre cseréli le kell lennie. Elnevezési szabályokat és korlátozásokat című témakörben talál [Kapcsolatos Azure tárterület-fiókok](storage-create-storage-account.md) és [névhasználati és a tárolók hivatkozik, a BLOB, és a metaadat-alapú](http://msdn.microsoft.com/library/azure/dd135715.aspx).

### <a name="how-to-set-a-default-azure-storage-account"></a>Egy alapértelmezett Azure tárterület-fiók beállítása
Több tárhely fiókot az előfizetésben is. Válasszon egyet közülük, és beállítja a tárterület-parancsok az alapértelmezett tároló fiókként az azonos PowerShell-munkamenetet. Ez lehetővé teszi, hogy az Azure PowerShell tároló parancsai futtathatók az tároló környezetben kifejezetten nélkülire.

1.  Előfizetéshez tartozó tárterület alapértelmezett számla beállításához futtatását is lehetővé teszi a Set-AzureSubscription parancsmag.

        $SubscriptionName = "Your subscription name"
        $StorageAccountName = "yourstorageaccount"  
        Set-AzureSubscription -CurrentStorageAccountName $StorageAccountName -SubscriptionName $SubscriptionName

2.  Ezután futtassa a Get-AzureSubscription parancsmag bizonyosodjon meg arról, hogy a tárhely fiók alapértelmezett előfizetés fiókhoz tartozó. Ez a parancs az előfizetés tulajdonságai a jelenlegi előfizetését, az aktuális tárterület-fiókot is beleértve adja vissza.

        Get-AzureSubscription –Current

### <a name="how-to-list-all-azure-storage-accounts-in-a-subscription"></a>Hogyan kell a lista összes Azure tároló fiókok lehetőséget a előfizetés
Minden egyes Azure előfizetés beállíthatja, hogy legfeljebb 100 tárterület-fiókok. A legfrissebb információt vonatkozó korlátozások című témakörben talál [Azure-előfizetést és a szolgáltatás korlátozások, a kvótákat, és a kényszerek](../azure-subscription-service-limits.md).

Futtassa a következő parancsmagot megtudhatja, hogy a nevét, és a tárterület-fiókok az aktuális előfizetés állapotát:

    Get-AzureStorageAccount | Format-Table -Property StorageAccountName, Location, AccountType, StorageAccountStatus

### <a name="how-to-create-an-azure-storage-context"></a>Hogyan hozhat létre egy tároló Azure környezetben
Azure tároló környezetben a PowerShell használatá hitelesítő adatokat tároló beágyazására objektum. Tároló környezetben használja, amíg minden későbbi parancsmagot lehetővé teszi a kérelem hitelesíteni a tárterület-fiók és a hívóbetű kifejezetten nélkülire. Tárterület környezetben használatával tároló nevét és az access fiókkulcs, a megosztott jogkivonat aláírás (Társítások), a kapcsolati karakterlánc, például a számos módon hozhat létre vagy névtelen. További tudnivalókért olvassa el a [New-AzureStorageContext](http://msdn.microsoft.com/library/azure/dn806380.aspx)című témakört.  

Kövesse az alábbi három módon hozhat létre a tárhely környezetben egyikét:

- Futtassa a [Get-AzureStorageKey](http://msdn.microsoft.com/library/azure/dn495235.aspx) parancsmag megtudhatja, hogy az elsődleges tároló hívóbetű Azure tárterület-fiókjához. Ezután hívja fel a [New-AzureStorageContext](http://msdn.microsoft.com/library/azure/dn806380.aspx) parancsmag tároló környezetben létrehozása:

        $StorageAccountName = "yourstorageaccount"
        $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
        $Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary


- Egy megosztott aláírás jogkivonat-Azure tároló tároló készítése és tároló környezetben létrehozásához:

        $sasToken = New-AzureStorageContainerSASToken -Container abc -Permission rl
        $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -SasToken $sasToken

    További tudnivalókért lásd: a [New-AzureStorageContainerSASToken](http://msdn.microsoft.com/library/azure/dn806416.aspx) és [Használatával megosztott Access aláírások (Társítások)](storage-dotnet-shared-access-signature-part-1.md).

- Egyes esetekben előfordulhat, hogy szeretne adja meg a szolgáltatás végpontokat, amikor létrehoz egy új tároló környezetben. Ez lehet, hogy a tárterület-fiók egyéni tartománynevet regisztrálta a Blob szolgáltatással vagy átengedése aláírás használni tárolási erőforrásokat elérni kívánt. A kapcsolati karakterlánc szolgáltatás végpontokat és használatával hozzon létre egy új tároló környezetben, alább látható módon:

        $ConnectionString = "DefaultEndpointsProtocol=http;BlobEndpoint=<blobEndpoint>;QueueEndpoint=<QueueEndpoint>;TableEndpoint=<TableEndpoint>;AccountName=<AccountName>;AccountKey=<AccountKey>"
        $Ctx = New-AzureStorageContext -ConnectionString $ConnectionString

Tároló kapcsolati karakterlánc konfigurálásával kapcsolatos további tudnivalókért olvassa el a [Csatlakozási_karakterlánc beállítása](storage-configure-connection-string.md)című témakört.

Most, hogy állítsa be a számítógépet, és a tanultakhoz előfizetések és Azure PowerShell használatával tároló fiókok kezelése, ugorjon a következő szakaszban megtudhatja, hogy miként kezelheti az Azure BLOB, és blob-pillanatképek.

### <a name="how-to-retrieve-and-regenerate-azure-storage-keys"></a>Hogyan olvashat be és újbóli létrehozása az Azure tároló kulcsok

Azure tároló fiók két fiókot billentyűk megtalálható. A következő parancsmagot a billentyűk beolvasásához is használhatja.

    Get-AzureStorageKey -StorageAccountName "yourstorageaccount"

A következő parancsmagot segítségével beolvashatja egy adott billentyűt. Érvényes értékei elsődleges és másodlagos.  

    (Get-AzureStorageKey -StorageAccountName $StorageAccountName).Primary

    (Get-AzureStorageKey -StorageAccountName $StorageAccountName).Secondary

Ha azt szeretné, a billentyűk újbóli, használja a következő parancsmagot. Érvényes - KeyType értékei "Elsődleges" és "Másodlagos"

    New-AzureStorageKey -StorageAccountName $StorageAccountName -KeyType “Primary”

    New-AzureStorageKey -StorageAccountName $StorageAccountName -KeyType “Secondary”

## <a name="how-to-manage-azure-blobs"></a>Azure BLOB kezelése
Azure Blob-tárolóhoz olyan szolgáltatás, a nagy mennyiségű strukturálatlan adatokat, például a szöveg és a bináris adatokat, bárhol is elérhető a világ HTTP-és HTTPS tárolásához. Ez a szakasz tartalma feltételezi, hogy Ön már jól ismert az Azure Blob-tároló szolgáltatás fogalmak. Információ a [Blob-tárolóhoz .NET használata – első lépések](storage-dotnet-how-to-use-blobs.md) és [Blob-szolgáltatási fogalmak](http://msdn.microsoft.com/library/azure/dd179376.aspx)témakörben talál.

### <a name="how-to-create-a-container"></a>A tároló létrehozása
Azure-tárolóban lévő minden blob tároló kell lennie. A New-AzureStorageContainer parancsmaggal a magánjellegű tároló hozhat létre:

    $StorageContainerName = "yourcontainername"
    New-AzureStorageContainer -Name $StorageContainerName -Permission Off

> [AZURE.NOTE] A névtelen olvasási hozzáférést három szinten: **kikapcsolása** **Blob**és **tároló**. Név nélküli hozzáférés BLOB elkerülése a jogosultsági paraméter értéke **Kikapcsolva**. Alapértelmezés szerint a új tároló privát, és csak a fiók tulajdonosa által is elérhető. Szeretné, hogy névtelen nyilvános olvasási hozzáférést blob-erőforrásokhoz, de nem tároló metaadatok vagy a BLOB-tárolóban listájára, állítsa a jogosultsági paraméter **Blob**. Szeretné, hogy a teljes körű nyilvános olvasási hozzáférést blob-erőforrások, tároló metaadatok és BLOB-tárolóban listájának, állítsa a jogosultsági paraméter **tároló**. További tudnivalókért olvassa el a [kezelés névtelen olvasási hozzáférést tárolók és BLOB](storage-manage-access-to-resources.md)című témakört.

### <a name="how-to-upload-a-blob-into-a-container"></a>Hogyan szeretné feltölteni a tároló blob
Azure Blob-tároló letiltása BLOB és a lap BLOB támogatja. [További információ ismertetése blokk BLOB, hozzáfűző BLOB és](http://msdn.microsoft.com/library/azure/ee691964.aspx)megtekintése lapon BLOB

Töltse fel a BLOB tároló, a [Set-AzureStorageBlobContent](http://msdn.microsoft.com/library/azure/dn806379.aspx) parancsmag is használhatja. Alapértelmezés szerint ez a parancs a helyi fájlok feltölti a továbbfejlesztett fájlblokkolás blob. Adja meg a blob típusát, a - BlobType paraméter is használhatja.

Az alábbi példa a [Get-ChildItem](http://technet.microsoft.com/library/hh849800.aspx) parancsmagot a megadott mappába minden fájlok fut, és átadja őket a következő parancsmagot a folyamat operátor segítségével. A [Set-AzureStorageBlobContent](http://msdn.microsoft.com/library/azure/dn806379.aspx) parancsmagot a tároló feltölti a helyi fájlokat:

    Get-ChildItem –Path C:\Images\* | Set-AzureStorageBlobContent -Container "yourcontainername"

### <a name="how-to-download-blobs-from-a-container"></a>A tároló BLOB letöltése
A következő példa bemutatja, hogyan lehet töltse le a BLOB tároló. A példa először egy kapcsolatot létesít Azure tárhely a fiók szerepel a tárterület-fiók nevét és az access elsődleges kulcs tároló környezet használatával. Ezután a példa beolvassa a [Get-AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806392.aspx) parancsmaggal blob hivatkozást. Ezután a példában a [Get-AzureStorageBlobContent](http://msdn.microsoft.com/library/azure/dn806418.aspx) parancsmag BLOB letöltése helyi cél mappába.

    #Define the variables.
    $ContainerName = "yourcontainername"
    $DestinationFolder = "C:\DownloadImages"

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = "Storage key for yourstorageaccount ends with =="
    $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

    #List all blobs in a container.
    $blobs = Get-AzureStorageBlob -Container $ContainerName -Context $Ctx

    #Download blobs from a container.
    New-Item -Path $DestinationFolder -ItemType Directory -Force
    $blobs | Get-AzureStorageBlobContent -Destination $DestinationFolder -Context $Ctx

### <a name="how-to-copy-blobs-from-one-storage-container-to-another"></a>Több tárhely tároló között BLOB másolása
Másolhatja BLOB tároló fiókok és régiók aszinkron. A következő példa bemutatja, hogyan BLOB másolása egy tároló tároló két különböző tároló fiók között. A példa először állítja be a forrás- és céltáblák tároló fiókok változói, és ekkor létrehoz az összes fiókhoz tároló környezetben. Ezután a példa másolja BLOB a forrás tárolóból a cél tároló a [Kezdés-AzureStorageBlobCopy](http://msdn.microsoft.com/library/azure/dn806394.aspx) parancsmaggal. A példa feltételezi, hogy a forrás- és céltáblák tároló fiókok és tárolók már létezik.

    #Define the source storage account and context.
    $SourceStorageAccountName = "yoursourcestorageaccount"
    $SourceStorageAccountKey = "Storage key for yoursourcestorageaccount"
    $SrcContainerName = "yoursrccontainername"
    $SourceContext = New-AzureStorageContext -StorageAccountName $SourceStorageAccountName -StorageAccountKey $SourceStorageAccountKey

    #Define the destination storage account and context.
    $DestStorageAccountName = "yourdeststorageaccount"
    $DestStorageAccountKey = "Storage key for yourdeststorageaccount"
    $DestContainerName = "destcontainername"
    $DestContext = New-AzureStorageContext -StorageAccountName $DestStorageAccountName -StorageAccountKey $DestStorageAccountKey

    #Get a reference to blobs in the source container.
    $blobs = Get-AzureStorageBlob -Container $SrcContainerName -Context $SourceContext

    #Copy blobs from one container to another.
    $blobs| Start-AzureStorageBlobCopy -DestContainer $DestContainerName -DestContext $DestContext

Figyelje meg, hogy a ebben a példában egy aszinkron másolatot hajt végre. A [Get-AzureStorageBlobCopyState](http://msdn.microsoft.com/library/azure/dn806406.aspx) parancsmag futtatásával figyelheti példányokban állapotát.

### <a name="how-to-copy-blobs-from-a-secondary-location"></a>A másodlagos helyre BLOB másolása
A másodlagos helyről RA GRS engedélyezett fiók BLOB másolhatja.

    #define secondary storage context using a connection string constructed from secondary endpoints.
    $SrcContext = New-AzureStorageContext -ConnectionString "DefaultEndpointsProtocol=https;AccountName=***;AccountKey=***;BlobEndpoint=http://***-secondary.blob.core.windows.net;FileEndpoint=http://***-secondary.file.core.windows.net;QueueEndpoint=http://***-secondary.queue.core.windows.net; TableEndpoint=http://***-secondary.table.core.windows.net;"
    Start-AzureStorageBlobCopy –Container *** -Blob *** -Context $SrcContext –DestContainer *** -DestBlob *** -DestContext $DestContext

### <a name="how-to-delete-a-blob"></a>Blob törlése
Blob törléséhez először első blob hivatkozást, majd az Eltávolítás-AzureStorageBlob parancsmag telefonál. A következő példa egy adott tárolóban lévő valamennyi BLOB törli. A példa először állítja be a változók tárterület-fiókot, és ekkor létrehoz egy tároló környezetben. Ezután a példában a [Get-AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806392.aspx) parancsmaggal blob hivatkozást, és az fut, az [Eltávolítás-AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806399.aspx) parancsmag BLOB eltávolítása Azure-tárolóban lévő tároló.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = "Storage key for yourstorageaccount ends with =="
    $ContainerName = "containername"
    $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

    #Get a reference to all the blobs in the container.
    $blobs = Get-AzureStorageBlob -Container $ContainerName -Context $Ctx

    #Delete blobs in a specified container.
    $blobs| Remove-AzureStorageBlob

## <a name="how-to-manage-azure-blob-snapshots"></a>Azure blob-pillanatképek kezelése
Azure segítségével blob pillanatképét. Pillanatkép blob, amely egy pontján származik, az idő írásvédett verziója fut. Pillanatkép létrehozása után azt is kell olvasni, másolja, vagy törölt, de nem módosíthatók. Egy adott időpontban érvényes blob kevésbé részletes adatokat a időpillanatban megjelenő ad lehetőséget. További tudnivalókért olvassa el a [Blob-pillanatfelvétel létrehozásának](http://msdn.microsoft.com/library/azure/hh488361.aspx)című témakört.

### <a name="how-to-create-a-blob-snapshot"></a>Hogyan hozhat létre egy blob-pillanatfelvétel
Hozzon létre egy snaphot a blob, először első blob hivatkozást, és hívhatja a `ICloudBlob.CreateSnapshot` módszer rajta. A következő példa először állítja be a változók tárterület-fiókot, és ekkor létrehoz egy tároló környezetben. Ezután a példában a [Get-AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806392.aspx) parancsmaggal blob hivatkozást, és az fut, a [ICloudBlob.CreateSnapshot](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.icloudblob.aspx) módszer pillanatképet szeretne létrehozni.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = "Storage key for yourstorageaccount ends with =="
    $ContainerName = "yourcontainername"
    $BlobName = "yourblobname"
    $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

    #Get a reference to a blob.
    $blob = Get-AzureStorageBlob -Context $Ctx -Container $ContainerName -Blob $BlobName

    #Create a snapshot of the blob.
    $snap = $blob.ICloudBlob.CreateSnapshot()

### <a name="how-to-list-a-blobs-snapshots"></a>Hogyan kell egy blob pillanatképek lista
Annyi pillanatképek blob a kívánt módon hozhat létre. Egy adott időpontban érvényes, az aktuális pillanatképek nyomon követésére a blob társított listája Az alábbi példában egy előre definiált blob használ, és hívja a [Get-AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806392.aspx) parancsmag listáját, hogy a blob az egy adott időpontban érvényes.  

    #Define the blob name.
    $BlobName = "yourblobname"

    #List the snapshots of a blob.
    Get-AzureStorageBlob –Context $Ctx -Prefix $BlobName -Container $ContainerName  | Where-Object  { $_.ICloudBlob.IsSnapshot -and $_.Name -eq $BlobName }

### <a name="how-to-copy-a-snapshot-of-a-blob"></a>Blob pillanatképét másolása
A pillanatkép visszaállítása blob pillanatképét másolhatja. Részletes tudnivalókat és korlátozásokat olvassa el a [Blob-pillanatfelvétel létrehozásának](http://msdn.microsoft.com/library/azure/hh488361.aspx)című témakört. A következő példa először állítja be a változók tárterület-fiókot, és ekkor létrehoz egy tároló környezetben. Ezután a példában a tároló és blob neve változók határozza meg. A példában a [Get-AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806392.aspx) parancsmaggal blob hivatkozást, és az fut, a [ICloudBlob.CreateSnapshot](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.icloudblob.aspx) módszer pillanatképet szeretne létrehozni. A példa futtat a [Kezdés-AzureStorageBlobCopy](http://msdn.microsoft.com/library/azure/dn806394.aspx) parancsmag ICloudBlob objektumot használva a forrás blob-blob pillanatképét másolásához. Ügyeljen arra, hogy az Ön konfigurációjának a példa futtatása előtt változók frissítése. Figyelje meg, hogy a következő példa feltételezi, hogy a forrás- és cél tárolók és a forrás blob már létezik.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = "Storage key for yourstorageaccount ends with =="
    $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

    #Define the variables.
    $SrcContainerName = "yoursourcecontainername"
    $DestContainerName = "yourdestcontainername"
    $SrcBlobName = "yourblobname"
    $DestBlobName = "CopyBlobName"

    #Get a reference to a blob.
    $blob = Get-AzureStorageBlob -Context $Ctx -Container $SrcContainerName -Blob $SrcBlobName

    #Create a snapshot of a blob.
    $snap = $blob.ICloudBlob.CreateSnapshot()

    #Copy the snapshot to another container.
    Start-AzureStorageBlobCopy –Context $Ctx -ICloudBlob $snap -DestBlob $DestBlobName -DestContainer $DestContainerName

Most, hogy hogyan Azure BLOB kezelése és pillanatfelvételek a Azure PowerShell blob-van megtanulta, Ugrás a következő szakaszban megtudhatja, hogy miként kezelheti a táblázatok, a sorok és a fájlokat.

## <a name="how-to-manage-azure-tables-and-table-entities"></a>Azure táblák és a táblázat szervezetek kezelése
Azure táblázat tárhelyszolgáltatáshoz egy NoSQL adattárhoz, amelynek tárolására és strukturált, nem relációs nagyon nagy adathalmazt lekérdezés használatával. A szolgáltatás fő összetevői táblázatok, a személyek és a tulajdonságok. Táblázat szervezetek gyűjteménye. Egy személy tulajdonságait. Minden egyes entitás beállíthatja, hogy legfeljebb 252 tulajdonságait, hogy mely az összes név – érték párokká. Ez a szakasz tartalma feltételezi, hogy Ön már jól ismert az Azure-táblából Tárhelyszolgáltatáshoz fogalmak. Részletes tudnivalókat talál [a táblázat szolgáltatás-adatmodell ismertetése](http://msdn.microsoft.com/library/azure/dd179338.aspx) és [Azure táblatárolóhoz .NET használata – első lépések](storage-dotnet-how-to-use-tables.md)

A következőkben lesz megtudhatja kezelése az Azure-táblából tárhelyszolgáltatáshoz Azure PowerShell használatával. Érintett esetek **létrehozása**, **törlése**és **beolvasása** **táblázatok**, valamint **hozzáadásával**, **lekérdezése**és **törlése a táblázat szervezetek**.

### <a name="how-to-create-a-table"></a>Táblázat létrehozása
Mindegyik táblázat az Azure tárterület-fiókkal kell lennie. A következő példa bemutatja, hogyan tábla létrehozása az Azure-tárolóban lévő. A példa először egy kapcsolatot létesít Azure tárhely a fiók szerepel a tárterület-fiók nevét és annak hívóbetű tároló környezet használatával. A [New-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806417.aspx) parancsmag ezután tábla létrehozása az Azure tárterületet használ.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = "Storage key for yourstorageaccount ends with =="
    $Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey

    #Create a new table.
    $tabName = "yourtablename"
    New-AzureStorageTable –Name $tabName –Context $Ctx

### <a name="how-to-retrieve-a-table"></a>Hogyan lehet beolvasni a táblázat
A lekérdezés, és beolvashatja egy vagy összes táblák tárterület-fiókjában. A következő példa bemutatja, hogyan lehet beolvasni a [Get-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806411.aspx) parancsmaggal adott táblázat.

    #Retrieve a table.
    $tabName = "yourtablename"
    Get-AzureStorageTable –Name $tabName –Context $Ctx

Ha a paraméter nélkül a Get-AzureStorageTable parancsmag hív meg, tárterület-fiók kap minden tároló tábla.

### <a name="how-to-delete-a-table"></a>Táblázat törlése
Táblázat [Eltávolítása-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806393.aspx) parancsmag használatával törölheti tárterület-fiókból.  

    #Delete a table.
    $tabName = "yourtablename"
    Remove-AzureStorageTable –Name $tabName –Context $Ctx

### <a name="how-to-manage-table-entities"></a>Táblázat személyek kezelése
Azure PowerShell jelenleg nem nyújt parancsmagok közvetlenül a táblázat szervezetek kezeléséhez. Táblázat szervezetek műveletek hajthatók végre, az osztályok az [Azure tároló ügyfél tár a .NET rendszerhez](http://msdn.microsoft.com/library/azure/wa_storage_30_reference_home.aspx)megadott is használhatja.

#### <a name="how-to-add-table-entities"></a>Hogyan lehet hozzáadni a táblázat személyek
Egy személy hozzáadása egy táblához, először létre kell hoznia egy objektum, amely meghatározza a szervezet tulajdonságainak. Egy személy beállíthatja, hogy legfeljebb 255 tulajdonságait, például a 3 Rendszertulajdonságok: **PartitionKey** **RowKey**és **időbélyeg**. Ön a felelős beszúrása és a **PartitionKey** és **RowKey**értékek frissítése. A kiszolgáló kezeli a az érték, az **időbélyegző**, amelyeket nem lehet módosítani. Együtt a **PartitionKey** és **RowKey** egyedileg azonosító egy táblázaton belül minden személy.

-   **PartitionKey**: határozza meg, hogy a partíciók, amely a szervezet található.
-   **RowKey**: egyedileg kell azonosítania a szervezet a partíciót belül.

Legfeljebb 252 egyéni tulajdonságok entitás adhatók meg. További tudnivalókért olvassa el a [a táblázat szolgáltatás-adatmodell ismertetése](http://msdn.microsoft.com/library/azure/dd179338.aspx)című témakört.

A következő példa bemutatja, hogyan lehet személyek hozzáadása egy táblához. A példa bemutatja az alkalmazottak tábla beolvasásához, és több személyek felvétele a mikrofonba. Először is azt egy kapcsolatot létesít Azure tárhely a fiók szerepel a tárterület-fiók nevét és annak hívóbetű tároló környezet használatával. Ezután az adott táblázat a [Get-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806411.aspx) parancsmaggal azt veszi. Ha a táblázat nem létezik, a [New-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806417.aspx) parancsmagot a tábla létrehozása az Azure-tárolóban lévő használatos. Ezután a példa egy egyéni függvény hozzáadása-entitás személyek hozzáadása a táblázat minden egyes entitás partíciót és sor kulcs megadásával határozza meg. A Hozzáadás-entitás függvény a hívások a [New-objektum](http://technet.microsoft.com/library/hh849885.aspx) parancsmagot a [Microsoft.WindowsAzure.Storage.Table.DynamicTableEntity](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.dynamictableentity.aspx) osztály hozzon létre egy egyed-objektumot. A példa később a [Microsoft.WindowsAzure.Storage.Table.TableOperation.Insert](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.insert.aspx) módszer hívások a szervezet objektum fel szeretne venni egy táblázatot.

    #Function Add-Entity: Adds an employee entity to a table.
    function Add-Entity() {
        [CmdletBinding()]
        param(
           $table,
           [String]$partitionKey,
           [String]$rowKey,
           [String]$name,
           [Int]$id
        )

      $entity = New-Object -TypeName Microsoft.WindowsAzure.Storage.Table.DynamicTableEntity -ArgumentList $partitionKey, $rowKey
      $entity.Properties.Add("Name", $name)
      $entity.Properties.Add("ID", $id)

      $result = $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Insert($entity))
    }

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
    $TableName = "Employees"

    #Retrieve the table if it already exists.
    $table = Get-AzureStorageTable –Name $TableName -Context $Ctx -ErrorAction Ignore

    #Create a new table if it does not exist.
    if ($table -eq $null)
    {
       $table = New-AzureStorageTable –Name $TableName -Context $Ctx
    }

    #Add multiple entities to a table.
    Add-Entity -Table $table -PartitionKey Partition1 -RowKey Row1 -Name Chris -Id 1
    Add-Entity -Table $table -PartitionKey Partition1 -RowKey Row2 -Name Jessie -Id 2
    Add-Entity -Table $table -PartitionKey Partition2 -RowKey Row1 -Name Christine -Id 3
    Add-Entity -Table $table -PartitionKey Partition2 -RowKey Row2 -Name Steven -Id 4

#### <a name="how-to-query-table-entities"></a>Keresés a táblázat személyek
A lekérdezés egy táblázat, használja a [Microsoft.WindowsAzure.Storage.Table.TableQuery](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tablequery.aspx) osztály. A következő példa feltételezi, hogy már korábban futtatása a parancsfájlt, hogyan megadott egységek szakasz, ez az útmutató hozzáadásához. A példa először egy kapcsolatot létesít Azure tárhely a tárhely környezetben, amely magában foglalja a tárterület-fiók nevét és annak hívóbetű használatával. Ezután próbál meg a korábban létrehozott "Alkalmazottak" táblázat a [Get-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806411.aspx) parancsmaggal beolvasásához. Hívja fel a [New-objektum](http://technet.microsoft.com/library/hh849885.aspx) parancsmagot a Microsoft.WindowsAzure.Storage.Table.TableQuery osztály létrehoz egy új lekérdezést-objektumot. A példában a szervezetek, amelyek egy "Azonosító" oszlopot, amelynek az értéke 1, a karakterlánc szűrő a megadott módon keres. Részletes információt olvassa el a [lekérdezés táblák és a személyek](http://msdn.microsoft.com/library/azure/dd894031.aspx)című témakört. A lekérdezés futtatásakor az összes szervezetek, amelyek megegyeznek a szűrési feltételek adja eredményül.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
    $TableName = "Employees"

    #Get a reference to a table.
    $table = Get-AzureStorageTable –Name $TableName -Context $Ctx

    #Create a table query.
    $query = New-Object Microsoft.WindowsAzure.Storage.Table.TableQuery

    #Define columns to select.
    $list = New-Object System.Collections.Generic.List[string]
    $list.Add("RowKey")
    $list.Add("ID")
    $list.Add("Name")

    #Set query details.
    $query.FilterString = "ID gt 0"
    $query.SelectColumns = $list
    $query.TakeCount = 20

    #Execute the query.
    $entities = $table.CloudTable.ExecuteQuery($query)

    #Display entity properties with the table format.
    $entities  | Format-Table PartitionKey, RowKey, @{ Label = "Name"; Expression={$_.Properties["Name"].StringValue}}, @{ Label = "ID"; Expression={$_.Properties["ID"].Int32Value}} -AutoSize

#### <a name="how-to-delete-table-entities"></a>Táblázat szervezetek törlése
A partíciók, és a sor kulcsokkal entitás törölheti. A következő példa feltételezi, hogy már korábban futtatása a parancsfájlt, hogyan megadott egységek szakasz, ez az útmutató hozzáadásához. A példa először egy kapcsolatot létesít Azure tárhely a tárhely környezetben, amely magában foglalja a tárterület-fiók nevét és annak hívóbetű használatával. Ezután próbál meg a korábban létrehozott "Alkalmazottak" táblázat a [Get-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806411.aspx) parancsmaggal beolvasásához. Ha a tábla létezik, a példa felhívja a a [Microsoft.WindowsAzure.Storage.Table.TableOperation.Retrieve](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.retrieve.aspx) módszer az partíciót és sor kulcs értékeit alapján entitás beolvasásához. Ezután adja át a szervezet a [Microsoft.WindowsAzure.Storage.Table.TableOperation.Delete](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.delete.aspx) módszerrel törlése.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

    #Retrieve the table.
    $TableName = "Employees"
    $table = Get-AzureStorageTable -Name $TableName -Context $Ctx -ErrorAction Ignore

    #If the table exists, start deleting its entities.
    if ($table -ne $null) {
       #Together the PartitionKey and RowKey uniquely identify every  
       #entity within a table.
       $tableResult = $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Retrieve("Partition2", "Row1"))
       $entity = $tableResult.Result
    if ($entity -ne $null)
    {
       #Delete the entity.
       $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Delete($entity))
    }
    }

## <a name="how-to-manage-azure-queues-and-queue-messages"></a>Azure sorok és az üzenetek kezelése
Azure várólista-tároló olyan szolgáltatás, a nagyszámú bárhol is elérhető a világ keresztül hitelesített hívások HTTP vagy HTTPS üzenetek tárolásához. Ez a szakasz tartalma feltételezi, hogy Ön már jól ismert az Azure várólista Tárhelyszolgáltatáshoz fogalmak. Részletes tudnivalókért olvassa el a [Azure várólista-tároló .NET használata – első lépések](storage-dotnet-how-to-use-queues.md)című témakört.

Ez a szakasz bemutatja, hogyan kezelheti az Azure várólista tárhelyszolgáltatáshoz Azure PowerShell használatával. Az érintett esetek **beszúrása** és **törlése** várólista az üzenetek, valamint **létrehozása**, **törlése**és **várólista beolvasása**.

### <a name="how-to-create-a-queue"></a>Hogyan hozhat létre egy várólista
A következő példa először egy kapcsolatot létesít Azure tárhely a fiók szerepel a tárterület-fiók nevét és annak hívóbetű tároló környezet használatával. Ezután a [New-AzureStorageQueue](http://msdn.microsoft.com/library/azure/dn806382.aspx) parancsmag "Várólistanév" nevű várólista létrehozásához hívások.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
    $QueueName = "queuename"
    $Queue = New-AzureStorageQueue –Name $QueueName -Context $Ctx

Azure várólista szolgáltatást a elnevezési szabályai tudnivalókért lásd [elnevezési sorban várakozó és a metaadatokat](http://msdn.microsoft.com/library/azure/dd179349.aspx).

### <a name="how-to-retrieve-a-queue"></a>Hogyan várólista beolvasása
A lekérdezés, és egy adott várólista vagy egy tároló fiókban sorban várakozó listájának lekéréséhez. A következő példa bemutatja, hogyan lehet beolvasni a [Get-AzureStorageQueue](http://msdn.microsoft.com/library/azure/dn806377.aspx) parancsmaggal a megadott várólista.

    #Retrieve a queue.
    $QueueName = "queuename"
    $Queue = Get-AzureStorageQueue –Name $QueueName –Context $Ctx

Ha paraméter nélkül a [Get-AzureStorageQueue](http://msdn.microsoft.com/library/azure/dn806377.aspx) parancsmag hív meg, akkor a listában az összes várólista kap.

### <a name="how-to-delete-a-queue"></a>Egy várólista törlése
Várólista és a benne lévő összes üzenetet törléséhez hívja fel az Eltávolítás-AzureStorageQueue parancsmag. A következő példa bemutatja a eltávolítása-AzureStorageQueue parancsmaggal a megadott várólista törlése.

    #Delete a queue.
    $QueueName = "yourqueuename"
    Remove-AzureStorageQueue –Name $QueueName –Context $Ctx

#### <a name="how-to-insert-a-message-into-a-queue"></a>Hogyan szúrhat be egy üzenetet az várólista
Üzenet beszúrni egy meglévő várólista, először létre kell hoznia egy új példányát az [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) osztály. Ezután hívja fel a [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx) módszert. Egy karakterlánc (a formátum UTF-8) vagy egy bájt tömböt egy CloudQueueMessage hozhat létre.

A következő példa bemutatja, hogyan üzenet hozzáadása egy várólista. A példa először egy kapcsolatot létesít Azure tárhely a fiók szerepel a tárterület-fiók nevét és annak hívóbetű tároló környezet használatával. Ezután a [Get-AzureStorageQueue](https://msdn.microsoft.com/library/azure/dn806377.aspx) parancsmaggal a megadott várólista azt veszi. Ha létezik a várakozási sorban található, a a [New-objektum](http://technet.microsoft.com/library/hh849885.aspx) parancsmagot a [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) osztály egy példányának létrehozására szolgál. Később a példában a [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx) módszer az üzenet objektum fel szeretne venni egy várólista hívások. A következő kód, amely egy várólista, és az szúrja be a "MessageInfo" üzenet:

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

    #Retrieve the queue.
    $QueueName = "queuename"
    $Queue = Get-AzureStorageQueue -Name $QueueName -Context $ctx

    #If the queue exists, add a new message.
    if ($Queue -ne $null) {
       # Create a new message using a constructor of the CloudQueueMessage class.
       $QueueMessage = New-Object -TypeName Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage -ArgumentList MessageInfo

       #Add a new message to the queue.
       $Queue.CloudQueue.AddMessage($QueueMessage)
    }


#### <a name="how-to-de-queue-at-the-next-message"></a>Hogyan, majd a sorból, a következő üzenetre
A kód vonja vissza az egy sorból két lépést a üzenet várakozási sorba. A [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.GetMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.getmessage.aspx) módszer hívásakor hibaüzenet a következő egy sorban. **GetMessage** – által eredményül adott üzenet lesz nem látható az üzenetek olvasása a sorból bármely más kódot. Ha végzett, az üzenet eltávolítása a várakozási sorban található, a [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.DeleteMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.deletemessage.aspx) módszer is meg kell hívni. Az üzenet eltávolítása a két lépésből álló folyamat biztosítja, hogy a kód feldolgozása üzenet hardveres és szoftveres hiba miatt sikertelen, ha egy másik példányát a kódot is ugyanazon üzenet jelenik meg, és próbálkozzon újra. A kód hívások **DeleteMessage** jobbra, után az üzenet feldolgozása megtörtént.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

    #Retrieve the queue.
    $QueueName = "queuename"
    $Queue = Get-AzureStorageQueue -Name $QueueName -Context $ctx

    $InvisibleTimeout = [System.TimeSpan]::FromSeconds(10)

    #Get the message object from the queue.
    $QueueMessage = $Queue.CloudQueue.GetMessage($InvisibleTimeout)
    #Delete the message.
    $Queue.CloudQueue.DeleteMessage($QueueMessage)

## <a name="how-to-manage-azure-file-shares-and-files"></a>Azure fájlmegosztások és a fájlok kezelése
Azure tárhely a szabványos kis-és Középvállalatok protokollt használó alkalmazásai megosztott tárhelyet biztosít. Microsoft Azure virtuális gépeken futó és felhőszolgáltatások fájlban lévő adatok megoszthatja alkalmazásösszetevők keresztül csatlakoztatott megosztások keresztül, és helyszíni alkalmazások hozzáférhetnek a fájlt tároló API-val, illetve az Azure PowerShell megosztás fájl adatok.

Azure-tárhelyet a további tudnivalókért lásd: [első lépések a Windows Azure fájltárolás](storage-dotnet-how-to-use-files.md) és a [Fájl szolgáltatás REST API -t](http://msdn.microsoft.com/library/azure/dn167006.aspx).

## <a name="how-to-set-and-query-storage-analytics"></a>Hogyan kell beállítani és a lekérdezés tároló analytics
[Azure tároló Analytics](storage-analytics.md) gyűjt a Azure tároló fiókok és a naplóadatokat kapcsolatban a tárhely fiókjába küldött kérelmek mértékek is használhatja. Lync-állapota egy tárterület-fiók és a tárhely naplózás diagnosztizálása és a tárterület-fiókkal kapcsolatos problémák megoldása tárolási mértékek is használhatja.
Alapértelmezés szerint a tárolási mértékek a tárterület-szolgáltatás nincs engedélyezve. Lehetővé teheti az Azure portálja vagy a Windows PowerShell használatával, vagy a tárterület-ügyfél tár programozás útján használatával figyelése. Tároló naplózás kiszolgálóoldali történik, és lehetővé teszi a rekordok részletei sikertelen és sikeres kérelmeket a tárterület-fiókjában. Ezek a naplók lehetővé teszi részleteinek olvasni, írja be és törlése a táblázatok, sorok, és BLOB, valamint a sikertelen kérelmek okait elleni művelet.

Megtudhatja, hogy miként engedélyezheti és a PowerShell használatá tárolási mértékek adatok megtekintése, lásd: [hogyan engedélyezhető a tárolási mértékek PowerShell használatával](http://msdn.microsoft.com/library/azure/dn782843.aspx#HowtoenableStorageMetricsusingPowerShell).

Megtudhatja, hogy miként engedélyezheti és a PowerShell használatá tároló naplózás adatok beolvasásához, olvassa el a [hogyan engedélyezhető a tárhely naplózás a PowerShell használatával](http://msdn.microsoft.com/library/azure/dn782840.aspx#HowtoenableStorageLoggingusingPowerShell) és [a tárhely naplózás napló adatok keresése](http://msdn.microsoft.com/library/azure/dn782840.aspx#FindingyourStorageLogginglogdata)című témakört.
Részletes információt a tárolási mértékek és a tárhely naplózási használatához tároló kapcsolatos problémák elhárítása című témakörben talál [figyelés, a felismerése, és a Microsoft Azure tárolás hibaelhárítási](storage-monitoring-diagnosing-troubleshooting.md).

## <a name="how-to-manage-shared-access-signature-sas-and-stored-access-policy"></a>A megosztott Access-aláírás (Társítások) és a hozzáférési házirend tárolt kezelése
Aláírások átengedése a biztonsági modell tetszőleges Azure-tárolót használó alkalmazás fontos részei. Korlátozott engedélyek a tárterület-fiókjába, hogy nem kell a fiókkulcs ügyfelek nyújtására hasznosak. Alapértelmezés szerint a tárterület-fiókot csak a tulajdonos BLOB, táblázatok és a számla belüli sorban várakozó is hozzáférhet. Ha a szolgáltatásból vagy alkalmazásból ezek az erőforrások számára elérhetővé szeretné tenni más ügyfelek megosztása a hívóbetű nélkül, telepítette három lehetőség közül választhat:

- A tároló engedélyeket lehetővé teszi a tároló és annak BLOB névtelen olvasási hozzáférést. Táblák vagy sorban várakozó ez nem engedélyezett.
- Használjon, hogy korlátozza-e támogatás jogokkal tárolók, BLOB, sorok és táblázatok meghatározott időközönként átengedése aláírást.
- Egy tárolt access-házirend használata egy további beállítási lehetőségekre tároló vagy annak BLOB, várólista, vagy tábla esetén a megosztott access aláírások szintet juthat. A tárolt hozzáférési házirend lehetővé teszi a kezdési időpontot, lejárati idő vagy aláírás engedélyeinek módosítása, illetve vissza szeretné vonni az azt követő nem adták.

A megosztott access aláírás valamelyik két képernyőn lehet:

- **Alkalmi Társítások**: során hoz létre egy alkalmi Társítások, a kezdési időpontot, a lejárati idő, és a Társítások engedélyeinek összes adható meg a Társítások URI. Az ilyen típusú Társítások lehet létrehozni a tároló, a blob, a táblázat vagy várólista, és nem revokable.
- **A tárolt hozzáférési házirend Társítások**: egy erőforrás tároló blob-tároló, táblázat vagy várólista - definiálva egy tárolt hozzáférési házirend, és felhasználhatja azt egy vagy több átengedése aláírások korlátozásokkal kezeléséhez. A Társítások társítása egy tárolt hozzáférési házirendet, a Társítások örökölnek kényszerek – a kezdési időpontot, a lejárati idő és a engedélyek – a tárolt hozzáférési házirend. Az ilyen típusú Társítások revokable.

További tudnivalókért lásd: [Használatával megosztott Access aláírások (Társítások)](storage-dotnet-shared-access-signature-part-1.md) és a [kezelés névtelen olvasási hozzáférést tárolók és BLOB](storage-manage-access-to-resources.md).

A következő szakaszokban megtanulhatja, hogyan hozhat létre egy megosztott access aláírás jogkivonat és tárolt hozzáférési házirend Azure táblák. Azure PowerShell hasonló parancsmagok nyújt a tárolók BLOB és, valamint a sorok. A parancsfájlok futtatni ebben a részben, töltse le a [Azure PowerShell verzió 0.8.14](http://go.microsoft.com/?linkid=9811175&clcid=0x409) vagy újabb verziójában.

### <a name="how-to-create-a-policy-based-shared-access-signature-token"></a>A csoportházirend-alapú hozzáférés aláírás megosztott jogkivonat létrehozása
A New-AzureStorageTableStoredAccessPolicy parancsmag használatával hozzon létre egy új tárolt hozzáférési házirendet. Ezután hívja fel a [New-AzureStorageTableSASToken](http://msdn.microsoft.com/library/azure/dn806400.aspx) parancsmag hozhat létre egy új csoportházirend-alapú megosztott aláírás jogkivonat-Azure tároló tábla.

    $policy = "policy1"
    New-AzureStorageTableStoredAccessPolicy -Name $tableName -Policy $policy -Permission "rd" -StartTime "2015-01-01" -ExpiryTime "2016-01-01" -Context $Ctx
    New-AzureStorageTableSASToken -Name $tableName -Policy $policy -Context $Ctx

### <a name="how-to-create-an-ad-hoc-non-revokable-shared-access-signature-token"></a>Hogyan hozhat létre egy alkalmi (nem revokable) megosztott Access-aláírás jogkivonat
A [New-AzureStorageTableSASToken](http://msdn.microsoft.com/library/azure/dn806400.aspx) parancsmag használatával hozzon létre egy új alkalmi (nem revokable) megosztott Access-aláírás token az Azure tároló tábla:

    New-AzureStorageTableSASToken -Name $tableName -Permission "rqud" -StartTime "2015-01-01" -ExpiryTime "2015-02-01" -Context $Ctx

### <a name="how-to-create-a-stored-access-policy"></a>A tárolt hozzáférési házirend létrehozása
A New-AzureStorageTableStoredAccessPolicy parancsmag használatával hozzon létre egy új tárolt hozzáférési házirendet, az Azure tároló tábla:

    $policy = "policy1"
    New-AzureStorageTableStoredAccessPolicy -Name $tableName -Policy $policy -Permission "rd" -StartTime "2015-01-01" -ExpiryTime "2016-01-01" -Context $Ctx

### <a name="how-to-update-a-stored-access-policy"></a>A tárolt hozzáférési házirend frissítése
A Set-AzureStorageTableStoredAccessPolicy parancsmag használatával az Azure tároló tábla meglévő tárolt hozzáférési házirend módosítása:

    Set-AzureStorageTableStoredAccessPolicy -Policy $policy -Table $tableName -Permission "rd" -NoExpiryTime -NoStartTime -Context $Ctx

### <a name="how-to-delete-a-stored-access-policy"></a>A tárolt hozzáférési házirend törlése
Az Eltávolítás-AzureStorageTableStoredAccessPolicy parancsmag használatával tárolt hozzáférési házirend-Azure tároló tábla törlése:

    Remove-AzureStorageTableStoredAccessPolicy -Policy $policy -Table $tableName -Context $Ctx


## <a name="how-to-use-azure-storage-for-us-government-and-azure-china"></a>Azure tároló használatáról az Amerikai Egyesült Államok kormányzati és Azure Kína
Azure környezetben a Microsoft Azure, például [Az Amerikai Egyesült Államok kormányzati Azure kormányzati](https://azure.microsoft.com/features/gov/), [a globális Azure AzureCloud](https://portal.azure.com)és [az Azure Kínában a 21Vianet által üzemeltetett AzureChinaCloud](http://www.windowsazure.cn/)egy független telepítésének. Angol kormányzati és Azure Kína új Azure környezetben üzembe azt.

Azure-tároló használatára AzureChinaCloud, AzureChinaCloud társított tároló környezetben létrehozásához szükséges. Kövesse ezeket a lépéseket az első lépések:

1.  Futtassa a [Get-AzureEnvironment](https://msdn.microsoft.com/library/azure/dn790368.aspx) parancsmagot a rendelkezésre álló Azure környezetben megtekintéséhez:

    `Get-AzureEnvironment`

2.  Azure Kína-fiók felvétele a Windows PowerShell:

    `Add-AzureAccount –Environment AzureChinaCloud`

3.  Hozzon létre egy AzureChinaCloud fiók tároló környezetben:

        $Ctx = New-AzureStorageContext -StorageAccountName $AccountName -StorageAccountKey $AccountKey> -Environment AzureChinaCloud

[Amerikai Egyesült Államok Azure kormányzati](https://azure.microsoft.com/features/gov/)Azure tároló használatához kell egy új környezet meghatározása és majd hozzon létre egy új tároló környezetben e-környezettel rendelkező:

1.  Futtassa a [Get-AzureEnvironment](https://msdn.microsoft.com/library/azure/dn790368.aspx) parancsmagot a rendelkezésre álló Azure környezetben megtekintéséhez:

    `Get-AzureEnvironment`

2.  Azure US kormányzati fiók hozzáadása a Windows PowerShell:

    `Add-AzureAccount –Environment AzureUSGovernment`

3.  Hozzon létre egy AzureUSGovernment fiók tároló környezetben:

        $Ctx = New-AzureStorageContext -StorageAccountName $AccountName -StorageAccountKey $AccountKey> -Environment AzureUSGovernment

További tudnivalókért lásd:

- [Útmutató a Microsoft Azure kormányzati Fejlesztőeszközök](../azure-government-developer-guide.md).
- [Kína-szolgáltatási alkalmazás létrehozásakor különbségek áttekintése](https://msdn.microsoft.com/library/azure/dn578439.aspx)

## <a name="next-steps"></a>Következő lépések
Ebből az útmutatóból megtanulta már Azure PowerShell Azure-tároló kezelése. Íme, néhány kapcsolódó cikkek és a velük kapcsolatos további tanulási források.

- [Azure tároló dokumentáció](https://azure.microsoft.com/documentation/services/storage/)
- [Azure tároló PowerShell-parancsmagok](http://msdn.microsoft.com/library/azure/dn806401.aspx)
- [A Windows PowerShell-hivatkozás](https://msdn.microsoft.com/library/ms714469.aspx)

[Image1]: ./media/storage-powershell-guide-full/Subscription_currentportal.png
[Image2]: ./media/storage-powershell-guide-full/Subscription_Previewportal.png
[Image3]: ./media/storage-powershell-guide-full/Blobdownload.png


[Getting started with Azure Storage and PowerShell in 5 minutes]: #getstart
[Prerequisites for using Azure PowerShell with Azure Storage]: #pre
[How to manage storage accounts in Azure]: #manageaccount
[How to set a default Azure subscription]: #setdefsub
[How to create a new Azure storage account]: #createaccount
[How to set a default Azure storage account]: #defaultaccount
[How to list all Azure storage accounts in a subscription]: #listaccounts
[How to create an Azure storage context]: #createctx
[How to manage Azure blobs and blob snapshots]: #manageblobs
[How to create a container]: #container
[How to upload a blob into a container]: #uploadblob
[How to download blobs from a container]: #downblob
[How to copy blobs from one storage container to another]: #copyblob
[How to delete a blob]: #deleteblob
[How to manage Azure blob snapshots]: #manageshots
[How to create a blob snapshot]: #createshot
[How to list snapshots of a blob]: #listshot
[How to copy a snapshot of a blob]: #copyshot
[How to manage Azure tables and table entities]: #managetables
[How to create a table]: #createtable
[How to retrieve a table]: #gettable
[How to delete a table]: #remtable
[How to manage table entities]: #mngentity
[How to add table entities]: #addentity
[How to query table entities]: #queryentity
[How to delete table entities]: #deleteentity
[How to manage Azure queues and queue messages]: #managequeues
[How to create a queue]: #createqueue
[How to retrieve a queue]: #getqueue
[How to delete a queue]: #remqueue
[How to manage queue messages]: #mngqueuemsg
[How to insert a message into a queue]: #addqueuemsg
[How to de-queue at the next message]: #dequeuemsg
[How to manage Azure file shares and files]: #files
[How to set and query storage analytics]: #stganalytics
[How to manage Shared Access Signature (SAS) and Stored Access Policy]: #sas
[How to use Azure Storage for U.S. government and Azure China]: #gov
[Next Steps]: #next
