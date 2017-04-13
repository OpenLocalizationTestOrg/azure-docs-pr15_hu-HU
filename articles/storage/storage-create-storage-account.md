<properties
    pageTitle="Hogyan hozhat létre, kezelése és az Azure-portálon tároló fiók törlése |} Microsoft Azure"
    description="Hozzon létre egy új tárterület-fiókot, a fiók hívóbetűk kezelése vagy az Azure-portálon tároló fiók törlése. Ismerje meg, normál és premium tárolási fiókokkal kapcsolatban."
    services="storage"
    documentationCenter=""
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/26/2016"
    ms.author="robinsh"/>


# <a name="about-azure-storage-accounts"></a>Azure tároló fiókokról

[AZURE.INCLUDE [storage-selector-portal-create-storage-account](../../includes/storage-selector-portal-create-storage-account.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools](../../includes/storage-try-azure-tools.md)]

## <a name="overview"></a>– Áttekintés

Azure tároló fiók mentheti és nyithatja meg a Azure tároló adatobjektumok egyedi névteret tartalmaz. Van egy tároló fiókban minden objektumhoz csoportként közös számlát kapni. Alapértelmezés szerint a fiók adatai csak az Ön a tulajdonosa érhető el.

[AZURE.INCLUDE [storage-account-types-include](../../includes/storage-account-types-include.md)]

## <a name="storage-account-billing"></a>Tárterület-fiók számlázás

[AZURE.INCLUDE [storage-account-billing-include](../../includes/storage-account-billing-include.md)]

> [AZURE.NOTE] Amikor létrehoz egy Azure virtuális gép, a tárterület-fiók létrejön automatikusan telepítési hely Ha még nem rendelkezik a tárterület-fiók az adott helyen. Így nem kell a virtuális gép merevlemezeken tárterület-fiók létrehozása az alábbi lépésekkel. A tároló fiók nevét a virtuális számítógépnév épül. További információt a [Azure virtuális gépeken futó dokumentációjában](https://azure.microsoft.com/documentation/services/virtual-machines/) találhat.

## <a name="storage-account-endpoints"></a>Tárterület-fiók végpontok

Minden a Azure tároló objektumnak egy egyedi URL-címet. A tároló fióknév űrlapok az adott cím altartomány. Altartomány és a tartomány neve, amely az egyes szolgáltatásokhoz, kombinációját forms tároló fiókjának *végpontot* .

Ha a tárterület-fiók neve *mystorageaccount*, majd az alapértelmezett végpontok tároló fiókjának mindegyike:

- BLOB-szolgáltatás: http://*mystorageaccount*. blob.core.windows.net

- A tábla szolgáltatás: http://*mystorageaccount*. table.core.windows.net

- Várólista szolgáltatást: http://*mystorageaccount*. queue.core.windows.net

- Szolgáltatás: http://*mystorageaccount*. file.core.windows.net

> [AZURE.NOTE] Egy Blob-tároló fiókot csak közzététele a Blob-szolgáltatás végpontjának.

Tárterület-fiókjában objektum elérése URL-CÍMÉT az objektum helyet tároló fiók hozzáfűzi az végpontot, amelyet épül. Például, hogy van egy blob-címet az ebben a formátumban: http://*mystorageaccount*.blob.core.windows.net/*mycontainer*/*myblob*.

Egyéni tartománynév használata a tárterület-fiók is beállítható. Klasszikus tárterület-fiókok esetén lásd: a [Configure egyéni tartományt a Blob-tároló végpont neve](storage-custom-domain-name.md) további információt. Az erőforrás-kezelő tárterület-fiókok ezt a lehetőséget, még nem vette fel az [Azure portál](https://portal.azure.com) , de a PowerShell konfigurálása. További tudnivalókért lásd: a [Set-AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607146.aspx) parancsmag.  

## <a name="create-a-storage-account"></a>Tárterület-fiók létrehozása

1. Jelentkezzen be az [Azure-portálon](https://portal.azure.com).

2. A központi menüben válassza az **Új** -> **adatok + tárhely** -> **tárterület-fiókot**.

3. Írja be a tárterület-fiókja nevét. Kapcsolatos további tudnivalók: [tárolási fiók végpontok](#storage-account-endpoints) hogyan a tárhely fióknevet használandó cím az Azure-tárolóban lévő objektumokat.

    > [AZURE.NOTE] Tárterület fióknevét 3 és 24 karakter hosszúságú között kell lennie, és számokat, és csak a kisbetűket is tartalmazhat.
    >  
    > A tároló fióknév Azure belül egyedinek kell lennie. Az Azure portál azt jelzik, ha a tárhely fióknév ki már használatban van.

4. Adja meg a telepítési modellt használandó: **Az erőforrás-kezelő** vagy **Klasszikus**. **Erőforrás-kezelő** a javasolt telepítési modell található. További tudnivalókért lásd: [erőforrás-kezelő Pivotbeli üzembe és a klasszikus telepítési](../resource-manager-deployment-model.md).

    > [AZURE.NOTE] Csak létrehozható BLOB-tároló fiókokat az erőforrás-kezelő telepítési modell alapján.

5. A fiók típusa szerint tároló: **Általános célú** vagy **Blob-tárolóhoz**. **Általános célú** az alapértelmezett érték.

    Ha **Általános célú** választotta, majd adja meg a teljesítmény réteg: **szabványos** vagy **Premium**. Az alapértelmezett érték **szabványos**. Normál és premium tároló fiókok, lásd: [Microsoft Azure tároló – bevezetés](storage-introduction.md) és [prémium tároló: Azure virtuális gép Munkaterhelésekből nagy teljesítményű tárterület](storage-premium-storage.md).

    Ha **Blob-tárolóhoz** lehetőséget választotta, majd adja meg az access réteg: **interaktív** vagy **izgalmas**. Az alapértelmezett érték **interaktív**. Lásd: [Azure Blob-tárolóhoz: lehűlni és melegvíz rétegek](storage-blob-storage-tiers.md) további információt.

6. Jelölje ki a replikáció lehetőséget a tárterület-fiókhoz: **LRS**, **GRS**, **TS-GRS**vagy **ZRS**. Az alapértelmezett érték **TS-GRS**. Azure tárolási replikációs beállításai, lásd: [Azure tárolási replikáció](storage-redundancy.md).

7. Jelölje ki azt az előfizetést, amelyben létre szeretne hozni az új tárterület-fiók.

8. Adja meg az új erőforráscsoport, vagy válassza ki a meglévő erőforráscsoport. Az erőforrás csoportok kapcsolatos további tudnivalókért [Azure erőforrás-kezelő áttekintése](../azure-resource-manager/resource-group-overview.md)című témakörben találhat.

9. Válassza ki a tárterület-fiók földrajzi helyét. Lásd: [Azure régiókban](https://azure.microsoft.com/regions/#services) milyen szolgáltatások érhetők el melyik régióban kapcsolatban további tudnivalókat.

10. Kattintson a **Létrehozás** a tárterület-fiók létrehozása gombra.

## <a name="manage-your-storage-account"></a>A tárterület-fiók kezelése

### <a name="change-your-account-configuration"></a>A fiók beállításainak módosítása

Miután létrehozta a tárterület-fiókját, módosíthatja a konfigurációja, például a fiókjához használt replikációs beállítást vagy a hozzáférési szint Blob-tároló fiók módosítása. Az [Azure portálon](https://portal.azure.com)nyissa meg a tárterület-fiókjába, **az összes beállításai** parancsra, és kattintson a **konfigurációs** megtekintése, illetve a fiók beállításainak módosítása gombra.

> [AZURE.NOTE] Attól függően, hogy a teljesítmény réteg úgy döntött, hogy a tárterület-fiók létrehozásakor néhány replikációs beállításai nem feltétlenül vehető igénybe.

A replikáció beállítás módosítása az ár változik. További tájékoztatásért lásd: [Azure tároló árak](https://azure.microsoft.com/pricing/details/storage/) lapot.

Blob-tároló fiókok esetén az access réteg módosítása előfordulhat, hogy merülnek fel költségek, a változtatások mellett a árak módosítása. Olvassa el a [Blob tároló fiókok – árak, és a számlázási](storage-blob-storage-tiers.md#pricing-and-billing) további információt.

### <a name="manage-your-storage-access-keys"></a>A tároló hívóbetűk kezelése

Amikor létrehoz egy tárterület-fiókkal, az Azure két 512 bites tároló hívóbetűk, a tárterület-fiók megnyitásakor hitelesítéshez használt hoz létre. Két tároló hívóbetűk megadásával Azure lehetővé teszi, hogy a megszakítás nélkül a tárhelyszolgáltatáshoz vagy a szolgáltatás a hozzáférést a billentyűk újragenerálása.

> [AZURE.NOTE] Azt javasoljuk, hogy ne a tárhely hívóbetűk megosztását beállításaira. Lehetővé teszi a tárhely erőforrásokhoz anélkül, hogy erről a hívóbetűk meg, a *megosztott access aláírás*is használhatja. Megosztott access aláírás fiókban erőforrás hozzáférést biztosít, intervallumhoz, amely csak és a megadott feltételeknek engedélyekkel rendelkező. További információt talál [Használatával megosztott Access aláírások (Társítások)](storage-dotnet-shared-access-signature-part-1.md) .

#### <a name="view-and-copy-storage-access-keys"></a>Megtekintése és tároló hívóbetűk másolása

Az [Azure portálon](https://portal.azure.com)nyissa meg a tárterület-fiókjába, **az összes beállításai** parancsra, és kattintson a **hívóbetűk** megtekintheti, másolja a vágólapra, és a fiók hívóbetűk újragenerálása gombra. A **Hívóbetűk** lap az előre beállított csatlakozási_karakterlánc a másolhatja az alkalmazásokban használandó elsődleges és másodlagos-kulcsokkal is tartalmaz.

#### <a name="regenerate-storage-access-keys"></a>Tárterület hívóbetűk újragenerálása

Azt javasoljuk, hogy módosítsa a hívóbetűk, a tárterület-fiókjába rendszeres a tárterület-kapcsolatokat biztonsága érdekében. Két hívóbetűk kapnak, hogy egy access-billentyű használatával, miközben a többi hívóbetű követező létrehozásakor karbantarthatja a kapcsolatok a tárterület-fiókjába.

> [AZURE.WARNING] A hívóbetűk újragenerálása hatással lehet a szolgáltatások Azure, valamint a saját alkalmazások, amelyek a tárterület-fiók típusától függően változnak. Az összes ügyfélprogramokat sorolja fel a hívóbetű a tárterület-fiók eléréséhez használt kell frissíteni az új termékkulccsal.

**Media services** – Ha media services függő tárterület-fiókja van, kell újra szinkronizálja a hívóbetűk a media szolgáltatás után a billentyűk követező létrehozásakor.

**Alkalmazások** – webalkalmazások vagy felhőszolgáltatások, amely a tárterület-fiók használata esetén, a kapcsolatok Ha elvesznek, újragenerálása hívóbetűk, kivéve, ha a billentyűk forgassa.

**Tárterület-kezelők** – Ha a [tárhely Intéző alkalmazást](storage-explorers.md)használja, valószínűleg be kell frissíteni a az alkalmazások által használt tárterület billentyűt.

A tároló hívóbetűk elforgatása folyamata a következő:

1. Frissítse a kapcsolat karakterláncok az alkalmazás kódja a tárterület-fiók a másodlagos hívóbetű hivatkozni szeretne.

2. Az elsődleges hívóbetű tároló fiókjának újragenerálása. A **Hívóbetűk** lap a **Újragenerálása azonosító1**gombra, és kattintson az **Igen gombra** kattintva erősítse meg, hogy szeretne-e előállítani új kulcsot.

3. Frissítse a kapcsolat karakterláncok kódban az új access elsődleges kulcs hivatkozni szeretne.

4. A másodlagos hívóbetű újragenerálása azonos módon.

## <a name="delete-a-storage-account"></a>Tárterület fiók törlése

Ha el szeretné távolítani a tárterület-fiókot, amelyet már nem használ, nyissa meg azt a tárterület-fiókot az [Azure portál](https://portal.azure.com), és kattintson a **Törlés**gombra. A tároló fiók törlése a teljes fiókot, beleértve az összes adat fiók.

> [AZURE.WARNING] Még nem lehet állítani egy törölt tárterület-fiókot vagy beolvasni a tartalom törlés előtt tartalmazó közül. Feltétlenül készítsen biztonsági másolatot bármi szeretné menteni a fiók törlése előtt. Ez is igaznak az erőforrások fiók – Ha töröl egy blob, tábla, várólista vagy fájl, véglegesen törlődik.

Társított az Azure virtuális géphez tárolási fiók törlése, meg kell győződnie, hogy minden olyan virtuális géphez lemezre törölték. Ha először nem törli a virtuális géphez lemezre, majd amikor megpróbálja a tárhely fiók törlése jelennek meg hasonló hibaüzenet:

    Failed to delete storage account <vm-storage-account-name>. Unable to delete storage account <vm-storage-account-name>: 'Storage account <vm-storage-account-name> has some active image(s) and/or disk(s). Ensure these image(s) and/or disk(s) are removed before deleting this storage account.'.

Ha a tárterület-fiókot használ, a klasszikus telepítési modell, az [Azure portál](https://manage.windowsazure.com)módon eltávolíthatja a virtuális gép lemez:

1. Nyissa meg a [Klasszikus Azure portálon](https://manage.windowsazure.com).
2. Nyissa meg a virtuális gépeken futó fülre.
3. Kattintson a lemez fülre.
4. Jelölje ki az adatok lemezt, majd kattintson a lemez törlése.
5. Lemezen képek törléséhez nyissa meg azt a képek fülre, és törölje a fiókot a tárolt képet.

További tudnivalókért lásd: [Azure virtuális géphez dokumentációt](http://azure.microsoft.com/documentation/services/virtual-machines/).

## <a name="next-steps"></a>Következő lépések

- [Azure Blob-tárolóhoz: Látványos, interaktív és rétegek](storage-blob-storage-tiers.md)
- [Azure tároló replikációs](storage-redundancy.md)
- [Azure tárolási Csatlakozási_karakterlánc konfigurálása](storage-configure-connection-string.md)
- [A AzCopy parancssori segédprogram az adatok átvitele](storage-use-azcopy.md)
- Keresse fel az [Azure tároló csapatának blogja](http://blogs.msdn.com/b/windowsazurestorage/).
