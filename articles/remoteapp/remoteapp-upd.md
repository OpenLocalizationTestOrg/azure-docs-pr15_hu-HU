
<properties 
    pageTitle="Hogyan menteni a Azure RemoteApp a felhasználói adatok és beállítások? | Microsoft Azure"
    description="Megtudhatja, hogyan Azure RemoteApp menti a felhasználói adatok a felhasználói profil lemezzel."
    services="remoteapp"
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />

# <a name="how-does-azure-remoteapp-save-user-data-and-settings"></a>Hogyan menteni a Azure RemoteApp a felhasználói adatok és beállítások?

> [AZURE.IMPORTANT]
> Azure RemoteApp megszakad alatt. Olvassa el a további információt a [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) .

Azure RemoteApp menti a felhasználói identitások és egyéni eszközök és a munkamenetek között. A felhasználói adatok felhasználónkénti webhelycsoport lemezen, vagyis a felhasználói profil lemezen (UPD) vannak tárolva. A lemez követi a felhasználó, és biztosíthatja, hogy a felhasználó rendelkezik-e egy következetes változat, függetlenül attól, ahol azok jelentkezzen be. mentése 

Felhasználói profil lemezt teljesen átlátszóak, a felhasználónak – felhasználók dokumentumok mentése a dokumentumok mappába (a helyi meghajtón látszólag) és a szokásos, az alkalmazás beállításainak módosítása. Egy időben az összes személyes beállítások bármilyen eszközről Azure RemoteApp való csatlakozáskor áll fenn. A felhasználó által látott nézetet mindössze ugyanazon a helyen adataikat.

Egyes UPD 50GB-nyi állandó tárhelyet, amelyben két felhasználói adatok és az alkalmazás beállításai. 

A részletekért a felhasználói profiladatok Olvasson tovább.

>[AZURE.NOTE] Tiltsa le a UPD kell? Megteheti, hogy most - nézze Pavithra a blogbejegyzést, [Letiltása felhasználói profil lemez (UPDs) az Azure RemoteApp](https://blogs.technet.microsoft.com/enterprisemobility/2015/11/11/disable-user-profile-disks-upds-in-azure-remoteapp/)további információt.


## <a name="how-can-an-admin-get-to-the-data"></a>Hogyan be az adatok rendszergazda?

Ha szeretne elérni az adatokat, a felhasználók (vagy ha a felhasználó elhagyja a vállalat vészhelyreállítás) egyik, Azure ügyfélszolgálatát, és az előfizetési adatokat nyújt a webhelycsoport és felhasználói azonosító. Az Azure RemoteApp csoport szolgáltatja a virtuális egy URL-CÍMÉT. Töltse le a virtuális és beolvasni a dokumentumok és fájlok van szüksége. Ne feledje, hogy a virtuális 50GB, hogy tart, töltse le a bit.


## <a name="is-the-data-backed-up"></a>Az adatok biztonsági másolatot?

Igen, hogy a felhasználói adatok földrajzi területenként másolatának mentéséhez. Az adatok csak olvasható, és azok webböngészőn ugyanúgy hasonlóan a normál adatok (kapcsolattartó Azure RemoteApp el), ha az elsődleges adatközpont lejjebb. Az adatokat másolja a valós idejű a biztonsági másolat helyét, és azt ne legyenek különböző verzióival másolatát. Igen adatsérülésekkel, azt nem tudja azt állítani egy korábban ismert jó verzióját, de az elsődleges adatközpont nem működik, ha Ön fogja tudni felhasználói adatok beolvasása a másik helyre.

## <a name="how-do-users-see-the-upd-on-the-server-side"></a>Hogyan felhasználók meg a UPD kiszolgálóoldalon?

Minden felhasználó rendelkezik saját címtár a kiszolgálón, amely a UPD rendeli: c:\Users\username.

## <a name="whats-the-best-way-to-use-outlook-and-upd"></a>Mi az, hogy a legjobb mód használata az Outlook és a UPD?

Azure RemoteApp Outlook állapotát (postaládák pst-fájlok) menti a munkamenetek között. Engedélyezéséhez szükség a felhasználói profil adatait tárolja a pst-fájl (c:\users\<felhasználónév >). Ez az alapértelmezett helye az adatokat, az mindaddig nem helyének módosítása, az adatok megmaradnak a munkamenetek között.

Azt is "gyorsítótárazott" üzemmód használata az Outlook alkalmazásban, és a "server/online" üzemmód használata a kereséshez javasoljuk.

Nézze meg [Ez a cikk](remoteapp-outlook.md) az Outlook és az Azure RemoteApp használatáról további információt.

## <a name="what-about-redirection"></a>Mit kell tudni az átirányítási?
Beállíthatja, hogy a felhasználók számára helyi eszközök állít be az [átirányítást](remoteapp-redirection.md)Azure RemoteApp. Helyi eszközök így tudják elérni az adatokat, kattintson a UPD.

## <a name="can-i-use-my-upd-as-a-network-share"></a>Van lehetőség a UPD hálózati megosztáson szerint?
nem. Hálózati megosztáson UPDs nem használható. Egy UPD gomb csak a felhasználó számára érhető el, ha a felhasználó csatlakozik az Azure RemoteApp aktívan.

## <a name="if-i-delete-a-user-from-a-collection-is-their-upd-deleted"></a>Ha egy felhasználó a gyűjteményből törölhetők, azok UPD törlődik?

Nem, amikor a felhasználó törlése, hogy nem törli automatikusan a UPD – Ehelyett azt tárolja az adatokat a webhelycsoport törléséig. a webhelycsoportban törlése után 90 napon azt minden UPDs törlése. 

Ha módosítani szeretné egy UPD törlés a gyűjteményből, lépjen kapcsolatba a Azure RemoteApp - azt UPD törölheti az oldalról.

## <a name="can-i-access-my-users-upds-either-current-or-deleted-users"></a>Elérhető a felhasználóim UPDs (aktuális vagy a törölt felhasználók)?

Igen, ha [Azure RemoteApp](mailto:remoteappforum@microsoft.com)kapcsolatba, azt is beállíthat, az adatok eléréséhez URL. Névjegy 10 óra kell töltse le a bármely adatok és a fájlokat a UPD az access lejárta előtt.

## <a name="are-upds-available-offline"></a>Érhetők el UPDs offline állapotban?

Most már a Microsoft nem vállal az offline hozzáférés UPDs, hogy túl a fentebb ismertetett 10 óra access-ablak jobb. Ez azt jelenti, hogy azt nincs lehetőség a szükséges, az Access-szel hosszú elég bonyolultabb feladatok elvégzése, például a UPDs vírusvédelmi szoftver fut, vagy egy naplózási adatainak elérése.

## <a name="do-registry-key-settings-persist"></a>Beállításkulcs marad tegye meg?
Igen, bármi HKEY_Current_User írt része a UPD.

## <a name="can-i-disable-upds-for-a-collection"></a>Letilthatom UPDs gyűjtemény?

Igen, megkérheti Azure RemoteApp UPDs letiltása az előfizetéshez, de nem megadható, hogy saját maga. Ez azt jelenti, hogy az előfizetés minden gyűjtemény UPDs letiltja.

Célszerű az alábbi esetekben az UPDs letiltása: 

- Szükséges fejezze be a hozzáférés és a felhasználói adatok (a jelölőnégyzetét, és tekintse át a céljából, például a pénzügyi intézmények).
- Ha 3rd külső felhasználói profil management megoldások a helyszíni és folytatja a tartományhoz tartozó Azure RemoteApp üzembe használni őket. Ehhez a profil agent kell tölteni a gold képbe van szükség. 
- Bármely helyi adattárolás fölösleges, vagy az összes adat a felhőben vagy fájl megosztása, és szeretné vezérlő az Azure RemoteApp helyben történő mentést.

[Tiltsa le felhasználói profil lemez (UPDs) az Azure RemoteApp](https://blogs.technet.microsoft.com/enterprisemobility/2015/11/11/disable-user-profile-disks-upds-in-azure-remoteapp/) talál további információt.

## <a name="can-i-restrict-users-from-saving-data-to-the-system-drive"></a>Felhasználók tiltása adatok mentésének rendszermeghajtóra?

Igen, de beállítása, hogy a sablon képe a webhelycsoport létrehozása előtt kell. Rendszermeghajtóra elérésének letiltása kövesse az alábbi lépéseket:

1. A sablon képe **gpedit.msc** futtatásával.
2. Nyissa meg azt **Felhasználó konfigurációja > Felügyeleti sablonok > Windows-összetevők > Explorer**.
3. Válassza az alábbi beállításokat:
    - **A sajátgépről fut a megadott meghajtók elrejtése**
    - **Megakadályozhatja, hogy az access meghajtók a sajátgépről fut**

## <a name="can-i-seed-upds-i-want-to-put-some-data-in-the-upd-thats-available-the-first-time-the-user-signs-in"></a>Rendező UPDs is? Szeretném elhelyezni a rendelkezésre álló az első alkalommal a felhasználó bejelentkezik a UPD néhány adatot.

Igen, a sablon képe létrehozásakor vehet információ az alapértelmezett profil. Ezeket az információkat a UPD majd kerül.

## <a name="can-i-change-the-size-of-the-upd-depending-on-how-much-data-i-want-to-store"></a>Módosíthatja a méretét a UPD attól függően, hogy mennyi adatot szeretnék tárolni?

Nem, minden UPDs van 50 GB-nyi tárhelyet. Ha szeretne másik mennyiségű adat tárolni, próbálkozzon az alábbiakkal:

1. Tiltsa le a webhelycsoport UPDs.
2. A felhasználók férhetnek hozzá a fájlmegosztás beállítása.
3. A fájlmegosztás betölteni egy indítási parancsfájl használatával. A parancsfájlok az Azure RemoteApp kapcsolatos részletekért lásd az alábbi.
4. A fájlmegosztás az összes adat mentése helyével.


## <a name="how-do-i-run-a-startup-script-in-azure-remoteapp"></a>Hogyan működik az Azure RemoteApp indítási parancsfájl?

Ha a használni kívánt indítási parancsfájl, először ütemezett feladat létrehozása a sablon kép fogja használni a gyűjteményben. (Ez *előtt* végezze el a sysprep futtatása.) 

![Rendszer-feladat létrehozása](./media/remoteapp-upd/upd1.png)

![Lefut, amikor a felhasználó bejelentkezik rendszer-feladat létrehozása](./media/remoteapp-upd/upd2.png)

Az **Általános** lapon ne felejtse el biztonsági "BUILTIN\Users.", az a **Felhasználói fiók** módosítása

![A felhasználói fiók módosítása egy csoportba](./media/remoteapp-upd/upd4.png)

Az ütemezett feladat elindítja az indítási parancsfájl, a felhasználó hitelesítő adataival. A feladat futását ütemezése minden olyan amikor egy felhasználó bejelentkezik.

![A tevékenység az eseményindító beállítása, "bejelentkezéskor"](./media/remoteapp-upd/upd3.png)

[A csoportházirend-alapú indítási parancsfájlok](https://technet.microsoft.com/library/cc779329%28v=ws.10%29.aspx)is használhatja. 

## <a name="what-about-placing-a-startup-script-in-the-start-menu-would-that-work"></a>Mit kell tudni a indítási parancsfájl elhelyezése a Start menü? Amely működő?

Más szóval is lehet config ablak parancsfájl futtató .bat fájl létrehozása és mentse a c:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp mappába, és választhat, hogy futtatni, amikor a felhasználó elindítja a RemoteApp munkamenet parancsfájl?

Nem, amely nem támogatott Azure RemoteApp, amelyet használ RDSH, amely szintén nem támogatja a indítási parancsfájlok a Start menü.

## <a name="can-i-use-mstscexe-the-remote-desktop-program-to-configure-logon-scripts"></a>Mstsc.exe (a távoli asztali program) használatával állítsa be a bejelentkezési parancsfájlok?

Nope nem támogatott Azure RemoteApp.

## <a name="can-i-store-data-on-the-vm-locally"></a>Tárolhatom a helyi meghajtóra a virtuális adatok?

NEM, a UPD az eltérő a virtuális tetszőleges tárolt adatok elvesznek. A felhasználó nagy valószínűséggel nem kap a azonos virtuális a következő van, hogy jelentkezzen be az Azure RemoteApp idő. Azt nem karbantartása: a felhasználó-virtuális adatmegőrzési, így a felhasználó nem lesz jelentkezzen be az azonos a virtuális, és az adatok el fog veszni. Emellett a webhelycsoport frissítésekor a meglévő VMs felülíródnak tartalmazó új VMs -, az azt jelenti, hogy a virtuális magát a tárolt adatokat elvész. Az ajánlási UPD, például az Azure fájlok fájlkiszolgálóra egy VNET belül, illetve a felhőbe, például a DropBox felhőalapú tárolási rendszert használ a megosztott tárolási az adatok tárolására.

## <a name="how-do-i-mount-an-azure-file-share-on-a-vm-using-powershell"></a>Hogyan csatlakoztatni egy Azure fájlmegosztás a egy virtuális PowerShell használatával?

A nettó-PSDrive parancsmag használatával csatlakoztatása az a meghajtóba, az alábbi képlettel történik:

    New-PSDrive -Name <drive-name> -PSProvider FileSystem -Root \\<storage-account-name>.file.core.windows.net\<share-name> -Credential :<storage-account-name>


A következő futtatásával is mentheti a hitelesítő adatait:

    cmdkey /add:<storage-account-name>.file.core.windows.net /user:<storage-account-name> /pass:<storage-account-key>


Lehetővé teszi, ugorja át a New-PSDrive parancsmagot a - hitelesítő adatok paramétert.
