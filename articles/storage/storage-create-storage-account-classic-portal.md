<properties
    pageTitle="Hogyan hozhat létre, kezelése és az Azure klasszikus portálon tároló fiók törlése |} Microsoft Azure"
    description="Hozzon létre egy új tárterület-fiókot, a fiók hívóbetűk kezelése vagy az Azure-portálon tároló fiók törlése. Tudjon meg többet a normál és premium tároló fiókok."
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

Azure tároló fiók férhet hozzá az Azure-tárolóban lévő Azure Blob, várólista, táblázat vagy fájl szolgáltatások. Tárterület-fiókját az egyedi névtér biztosít az Azure tároló adatobjektumok. Alapértelmezés szerint a fiók adatai csak az Ön a tulajdonosa érhető el.

Tárterület-fiókok két típusa van:

- Egy szabványos tároló fiókja biztosan tartalmazza-Blob, tábla, várólista és fájlt tároló.
- Egy prémium tároló fiók jelenleg csak az Azure virtuális gép lemez támogatja. Lásd: [prémium tároló: Azure virtuális gép Munkaterhelésekből nagy teljesítményű tárterület](storage-premium-storage.md) prémium tároló részletes áttekintést.

## <a name="storage-account-billing"></a>Tárterület-fiók számlázás

Vannak az Azure tárterület-használat a tárterület-fiók alapján számlát kapni. Tárolási költségeket négy tényezők alapján: tárolókapacitással rendelkezik, az Office-téma replikációs, tároló tranzakciók és adatok kilépési.

- Mennyi a tárterület-fiók lekötés Ön hivatkozik tárolókapacitású adattárolásra használatával. A költség egyszerűen tárolása az adatok tárolására, hogy mennyi adatot, és hogyan replikált határozza meg.
- Replikációs határozza meg, hogy hány példányt az adatok megmaradnak egyszerre, és milyen helyeken.
- Az összes írási és olvasási műveletek Azure-tárolóhoz hivatkozhat tranzakciók.
- Kilépés az Azure területről átvitt adatok adatok kilépési hivatkozik. Amikor a tárterület-fiókja adatait egy ugyanabban a régióban nem futó alkalmazás által használt-e az alkalmazást egy felhőalapú szolgáltatásba, vagy valamilyen más típusú alkalmazás, majd az előfizetést terhelő adatok kilépési. (Az Azure-szolgáltatások készíthet lépéseket követve csoportosíthatja az adatokat, és a szolgáltatások csökkentése vagy adatok kilépési díjak kiküszöbölése érdekében az azonos adatközpontokban.)  

Az [Azure tároló árak](https://azure.microsoft.com/pricing/details/storage) oldal részletes ismertetése árak tárolókapacitással rendelkezik, a replikáció és a tranzakciók. Az [Adatok átvitelek árak részletei](https://azure.microsoft.com/pricing/details/data-transfers/) lapon részletes ismertetése árak az adatok kilépési.

Tárterület fiók beosztását, és a teljesítmény célok kapcsolatos részletekért lásd: [Azure tároló méretezhetőség és a teljesítmény célok](storage-scalability-targets.md).

> [AZURE.NOTE] Amikor létrehoz egy Azure virtuális gép, a tárterület-fiók létrejön automatikusan telepítési hely Ha még nem rendelkezik a tárterület-fiók az adott helyen. Így nem kell a virtuális gép merevlemezeken tárterület-fiók létrehozása az alábbi lépésekkel. A tároló fiók nevét a virtuális számítógépnév épül. További információt a [Azure virtuális gépeken futó dokumentációjában](https://azure.microsoft.com/documentation/services/virtual-machines/) találhat.

## <a name="create-a-storage-account"></a>Tárterület-fiók létrehozása

1. Jelentkezzen be az [Azure klasszikus portálon](https://manage.windowsazure.com).

2. A tálcán a lap alján kattintson az **Új** gombra. Válassza a **Data Services** | **tárhely**, és kattintson a **Gyors létrehozásához**.

    ![NewStorageAccount](./media/storage-create-storage-account-classic-portal/storage_NewStorageAccount.png)

3. Az **URL-CÍMÉT**írja be a tárterület-fiókja nevét.

    > [AZURE.NOTE] Tárterület fióknevét 3 és 24 karakter hosszúságú között kell lennie, és számokat, és csak a kisbetűket is tartalmazhat.
    >  
    > A tároló fióknév Azure belül egyedinek kell lennie. Az Azure klasszikus portál azt jelzik, ha már vett-e a tárhely fióknév választja.

    Kapcsolatos további tudnivalók: [tároló fiók végpontok](#storage-account-endpoints) alatti hogyan a tárhely fióknév használandó cím az Azure-tárolóban lévő objektumokhoz.

4. **Hely/affinitás csoport**közelébe, vagy az ügyfelekkel tároló fiókjának helyének kiválasztása. Ha az adatokat tároló fiókban elérhető az Azure virtuális gép vagy felhőszolgáltatásba, például egy másik Azure szolgáltatás érdemes affinitás csoport kiválasztása a listában, és csoportosíthatja az azonos adatközpont más Azure szolgáltatásokkal javítható teljesítményének és alsó költségek használni kívánt tárterület-fiókjába.

    Figyelje meg, hogy ki kell választania egy affinitás csoport, a tárhely fiók létrehozásakor. Meglévő fiók affinitás alkalmazás nem aktiválhatók. Affinitás csoportok a további tudnivalókért lásd: az alábbi [közös SRV-affinitás csoporttal](#service-co-location-with-an-affinity-group) .

    >[AZURE.IMPORTANT] Annak megállapításához, hogy melyik helyen érhető el az előfizetéséhez, a [lista összes erőforrás-szolgáltató](https://msdn.microsoft.com/library/azure/dn790524.aspx) művelet felhívhatja. A PowerShell szolgáltatók lista, hívja fel a [Get-AzureLocation](https://msdn.microsoft.com/library/azure/dn757693.aspx). A .NET használja a ProviderOperationsExtensions osztály [lista](https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.provideroperationsextensions.list.aspx) metódusát.
    >
    >Ezenkívül lásd: [Azure régiókban](https://azure.microsoft.com/regions/#services) milyen szolgáltatások érhetők el melyik régióban kapcsolatban további tudnivalókat.


5. Ha egynél több Azure előfizetést, majd az **előfizetés** mező jelenik meg. **Előfizetés**írja be az Azure előfizetést, amelyet a tárterület-fiókot használ.

6. **Replikációs**válassza ki a kívánt replikációs a tárterület-fiókjához. A javasolt replikációs beállítás maximális tartóssági biztosít az adatok geo felesleges replikáció. Azure tároló replikációs beállításai, lásd: [Azure tároló replikáció](storage-redundancy.md).

6. Kattintson a **tárterület-fiók létrehozása**gombra.

    Eltarthat néhány percig a tárterület-fiók létrehozása. Az állapot ellenőrzése, hogy figyelheti a az értesítés az Azure klasszikus portál alján. A tároló fiók létrehozását követően a tárterület-fiókja **Online** állapotú, és használatra kész.

![StoragePage](./media/storage-create-storage-account-classic-portal/Storage_StoragePage.png)


### <a name="storage-account-endpoints"></a>Tárterület-fiók végpontok

Minden a Azure tároló objektumnak egy egyedi URL-címet. A tároló fióknév űrlapok az adott cím altartomány. Altartomány és a tartomány neve, amely az egyes szolgáltatásokhoz, kombinációját forms tároló fiókjának *végpontot* .

Ha a tárterület-fiók neve *mystorageaccount*, majd az alapértelmezett végpontok tároló fiókjának mindegyike:

- BLOB-szolgáltatás: http://*mystorageaccount*. blob.core.windows.net

- A tábla szolgáltatás: http://*mystorageaccount*. table.core.windows.net

- Várólista szolgáltatást: http://*mystorageaccount*. queue.core.windows.net

- Szolgáltatás: http://*mystorageaccount*. file.core.windows.net

Az tárterület-Irányítópulton az [Azure klasszikus portál](https://manage.windowsazure.com) tároló fiókjának végpontjait megtekintheti, a fiók létrehozása után.

Tárterület-fiókjában objektum elérése URL-CÍMÉT az objektum helyet tároló fiók hozzáfűzi az végpontot, amelyet épül. Például, hogy van egy blob-címet az ebben a formátumban: http://*mystorageaccount*.blob.core.windows.net/*mycontainer*/*myblob*.

Egyéni tartománynév használata a tárterület-fiók is beállítható. A részletekért [konfigurálása a blob-tároló végpont egyéni tartománynevet](storage-custom-domain-name.md) című témakört.

### <a name="service-co-location-with-an-affinity-group"></a>Dokumentumok közös SRV-affinitás csoporttal

Egy *Affinitás csoport* földrajzi csoportosítás a Azure szolgáltatások és VMs Azure tárterület-fiókjával. Egy affinitás csoport megkeresése a számítógépen munkaterhelésekből, az azonos adatközpont vagy annak közelében a célközönség felhasználói szolgáltatás teljesítménye növelhető. Még nem számlázási költségeket felmerült kilépési más szolgáltatásból az azonos affinitás csoport részét képező tárterület-fiókjában adatok elérésekor.

> [AZURE.NOTE]  Egy affinitás csoport létrehozásához nyissa meg a <b>Beállítások</b> területen a [Klasszikus Azure-portálon](https://manage.windowsazure.com), <b>Affinitás csoportok</b>elemre, és kattintson a <b>egy affinitás csoport hozzáadása</b> vagy a <b>Hozzáadás</b> gombra. Is létrehozhat és affinitás csoportok kezelése az Azure szolgáltatás felügyeleti API segítségével. További információt talál <a href="http://msdn.microsoft.com/library/azure/ee460798.aspx">Affinitás csoportok műveleteket</a> .

## <a name="view-copy-and-regenerate-storage-access-keys"></a>Nézet, a másolás és a hívóbetűk újragenerálása tárhely

Amikor létrehoz egy tárterület-fiókkal, az Azure két 512 bites tároló hívóbetűk, a tárterület-fiók megnyitásakor hitelesítéshez használt hoz létre. Két tároló hívóbetűk megadásával Azure lehetővé teszi, hogy a megszakítás nélkül a tárhelyszolgáltatáshoz vagy a szolgáltatás a hozzáférést a billentyűk újragenerálása.

> [AZURE.NOTE] Azt javasoljuk, hogy ne a tárhely hívóbetűk megosztását beállításaira. Lehetővé teszi a tárhely erőforrásokhoz anélkül, hogy erről a hívóbetűk meg, a *megosztott access aláírás*is használhatja. Megosztott access aláírás fiókban erőforrás hozzáférést biztosít, intervallumhoz, amely csak és a megadott feltételeknek engedélyekkel rendelkező. További információt talál [Használatával megosztott Access aláírások (Társítások)](storage-dotnet-shared-access-signature-part-1.md) .

Az [Azure klasszikus portál](https://manage.windowsazure.com)használata **Kulcsok kezelése** az irányítópulton vagy a **tárterület** -lap megtekintése, másolja a vágólapra, és a tárhely hívóbetűk a Blob, a táblázat és a várólista szolgáltatások eléréséhez használt újragenerálása.

### <a name="copy-a-storage-access-key"></a>Másolja a tárhely hívóbetű  

**Kezelése billentyűk** segítségével másolja a tárhely hívóbetű használata a kapcsolati karakterlánc. A kapcsolati karakterlánc a tárterület-fiók nevét, és egy kulcsot a hitelesítés szükséges. További információt a csatlakozási_karakterlánc Azure tároló szolgáltatások elérése, akkor olvassa el az [Azure tárhely a Csatlakozási_karakterlánc konfigurálása](storage-configure-connection-string.md)konfigurálása.

1. Az [Azure klasszikus portálon](https://manage.windowsazure.com)kattintson a **tároló**elemre, és kattintson a nevére kattintva nyissa meg az irányítópult tárterület-fiók.

2. Kattintson a **billentyűk kezelése**elemre.

    Ekkor megnyílik a **Hívóbetűk kezelése** .

    ![Managekeys](./media/storage-create-storage-account-classic-portal/Storage_ManageKeys.png)


3. A tároló hívóbetű másolásához jelölje ki a fő szöveget. Majd kattintson a jobb gombbal, és kattintson a **Másolás**parancsra.

### <a name="regenerate-storage-access-keys"></a>Tárterület hívóbetűk újragenerálása
Azt javasoljuk, hogy módosítsa a hívóbetűk, a tárterület-fiókjába rendszeres a tárterület-kapcsolatokat biztonsága érdekében. Két hívóbetűk kapnak, hogy egy access-billentyű használatával, miközben a többi hívóbetű követező létrehozásakor karbantarthatja a kapcsolatok a tárterület-fiókjába.

> [AZURE.WARNING] A hívóbetűk újragenerálása hatással lehet a szolgáltatások Azure, valamint a saját alkalmazások, amelyek a tárterület-fiók típusától függően változnak. Az összes ügyfélprogramokat sorolja fel a hívóbetű a tárterület-fiók eléréséhez használt kell frissíteni az új termékkulccsal.

**Media services** – Ha media services függő tárterület-fiókja van, kell újra szinkronizálja a hívóbetűk a media szolgáltatás után a billentyűk követező létrehozásakor.

**Alkalmazások** – webalkalmazások vagy felhőszolgáltatások, amely a tárterület-fiók használata esetén, a kapcsolatok Ha elvesznek, újragenerálása hívóbetűk, kivéve, ha a billentyűk forgassa. 

**Tárterület-kezelők** – Ha a [tárhely Intéző alkalmazást](storage-explorers.md)használja, valószínűleg be kell frissíteni a az alkalmazások által használt tárterület billentyűt.

A tároló hívóbetűk elforgatása folyamata a következő:

1. Frissítse a kapcsolat karakterláncok az alkalmazás kódja a tárterület-fiók a másodlagos hívóbetű hivatkozni szeretne.

2. Az elsődleges hívóbetű tároló fiókjának újragenerálása. Az [Azure klasszikus portál](https://manage.windowsazure.com)az irányítópulton vagy **a lap** **Kulcsok kezelése**gombra. Kattintson a **újragenerálása** access elsődleges kulcs alatt, és kattintson az **Igen gombra** kattintva erősítse meg, hogy szeretne-e előállítani új kulcsot.

3. Frissítse a kapcsolat karakterláncok a kód az új access elsődleges kulcs hivatkozni szeretne.

4. A másodlagos hívóbetű újragenerálása.

## <a name="delete-a-storage-account"></a>Tárterület fiók törlése

Ha el szeretné távolítani a tárterület-fiókot, amelyet már nem használ, használata **törlése** az irányítópulton vagy **a lap** . A teljes tárterület-fiókot, beleértve a BLOB, a táblák és a sorok minden fiók **törlése** törli.

> [AZURE.WARNING] Még nem lehet állítani egy törölt tárterület-fiókot vagy beolvasni a tartalom törlés előtt tartalmazó közül. Feltétlenül készítsen biztonsági másolatot bármi szeretné menteni a fiók törlése előtt. Ez is igaznak az erőforrások fiók – Ha töröl egy blob, tábla, várólista vagy fájl, véglegesen törlődik.
>
> Ha a tárterület-fiók tartalmaz virtuális az Azure virtuális gép, majd törölnie kell bármilyen képet vagy lemezen használják ezeket a virtuális fájlokat a tárterület-fiók törlése előtt. Első lépésként leállítása a virtuális gépen fut, és törölje azt. Lemezen törléséhez nyissa meg azt a **lemezt** fülre, és törölje a bármely lemez van. Képek törléséhez nyissa meg azt a **képek** fülre, és törölje a fiókot a tárolt képet.

1. Az [Azure klasszikus portálon](https://manage.windowsazure.com)kattintson a **tárolás**.

2. Kattintson bárhová a tárhely fiók bejegyzés kivételével a nevét, és kattintson a **Törlés**gombra.

     – Vagy –

    Kattintson a nevére kattintva nyissa meg az irányítópult tárterület-fiók, és kattintson a **Törlés**gombra.

3. **Válassza az Igen lehetőséget** választva erősítse meg, hogy a tárterület-fiók törlése gombra.

## <a name="next-steps"></a>Következő lépések

- Azure tároló kapcsolatos további információért lásd: [Azure tároló dokumentáció](https://azure.microsoft.com/documentation/services/storage/).
- Keresse fel az [Azure tároló csapatának blogja](http://blogs.msdn.com/b/windowsazurestorage/).
- [A AzCopy parancssori segédprogram az adatok átvitele](storage-use-azcopy.md)
