<properties
    pageTitle="Azure tároló szolgáltatás titkosítási az adatokat a többi |} Microsoft Azure"
    description="Titkosítás szolgáltatás oldalán az Azure Blob-tárolóhoz, ha az adatok tárolása az Azure tároló szolgáltatás titkosítási szolgáltatást használja, és visszafejteni az adatok beolvasása közben."
    services="storage"
    documentationCenter=".net"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="robinsh"/>

# <a name="azure-storage-service-encryption-for-data-at-rest"></a>Azure tároló szolgáltatás titkosítási az adatokat a többi

Azure tároló szolgáltatás titkosítási (SSE) az adatokat a többi segít és az adatok felel meg a szervezeti biztonság és megfelelőség kötelezettségek megvédeni. Ezzel a szolgáltatással a Azure tároló automatikusan titkosítja előtt tárolóhoz pályától tartós az adatok, és használatával visszafejti a lekérés előtt. A titkosítási visszafejtés és kulcskezelő átlátszóak teljesen felhasználók számára.

A következő szakaszokban a tárhely szolgáltatás titkosítási szolgáltatások, valamint a támogatott esetek és a felhasználó használatát, részletes útmutatás találkozik.

## <a name="overview"></a>– Áttekintés

Biztonsági funkciók, amelyek közös engedélyezése a fejlesztők számára biztonságos alkalmazások átfogó Azure tárhelyet biztosít. A védett adatok között egy alkalmazást, és Azure [Ügyféloldali titkosítást](storage-client-side-encryption.md), a HTTPs vagy a kis-és Középvállalatok 3.0 használatával. Tároló szolgáltatás titkosítási titkosítási a többi, kezelési titkosítás, visszafejtés és teljes mértékben átlátszó módon kulcskezelő biztosít. Az összes adat titkosított 256 bites [AES titkosítást](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), a legmagasabb szintű blokkolása titkosítási közül.

SSE működik, hogy titkosítja a az adatok Azure-tárolóhoz írott és blokk BLOB, oldal BLOB használhatók és BLOB Hozzáfűzés. Az alábbi működik:

-   Általános célú tárhely és a Blob-tároló fiókok
-   Szabványos tároló és támogatási szolgáltatások 
-   Az összes redundancia szintek (LRS, ZRS, GRS, TS-GRS)
-   Azure erőforrás-kezelő tároló accounts (de nem klasszikus) 
-   Az összes terület

Engedélyezése, és tiltsa le a tárhely szolgáltatás titkosítást tárterület-fiókot, jelentkezzen be az [Azure-portálra](https://azure.portal.com) , és válassza a tárterület-fiók. Kattintson a beállítások lap a Blob-szolgáltatás szakaszban keresse meg a képernyőképen látható módon, majd kattintson a titkosítást.

![Portál képernyőképe megjelenítő titkosítási beállítás](./media/storage-service-encryption/image1.png)

A titkosítási beállítás gombra kattintás után engedélyezése, és tiltsa le a tárhely szolgáltatás titkosítást.

![Portál képernyőképe megjelenítő titkosítási tulajdonságai](./media/storage-service-encryption/image2.png)

##<a name="encryption-scenarios"></a>Titkosítási felhasználási területei

Tárterület szolgáltatás titkosítási engedélyezhető a tárhely fiók szinten. Támogatja az ügyfélnek alábbi jelenik meg:

-   Blokkokból álló BLOB, titkosítása hozzáfűző BLOB és lap BLOB.

-   Archivált VHD és a helyszíni Azure tudomására sablonokat titkosítást.

-   A VHD használatával létrehozott IaaS VMs alapjául szolgáló adatok és OS lemez titkosítás.

SSE rendelkezik az alábbi korlátozások vonatkoznak:

-   Titkosítási klasszikus tárterület-fiókok nem támogatott.

-   Titkosítási klasszikus tárolási fiókok átkerül az erőforrás-kezelő tárterület-fiókok nem támogatott.

-   Meglévő adatok - SSE csak újonnan létrehozott adatok titkosítására, miután a titkosítási engedélyezve van. Ha például hozzon létre egy új erőforrás-kezelő tárterület-fiókot, de ne kapcsolja be a titkosítást, és ezután BLOB- vagy archivált VHD feltöltése, a tárterület-fiók, és kapcsolja be a SSE, kivéve, ha az azok újraírásra vagy másolt e BLOB nincs titkosítva.

-   Piactér támogatása – VMs engedélyezése titkosítása létrehozni, a az [Azure portálon](https://portal.azure.com), a PowerShell és a Azure CLI Marketplace webhelyről. A virtuális alap kép titkosított; marad azonban bármelyik végrehajtani, miután a virtuális fel van is írások titkosítja.

-   Táblázat, sorok, és a fájlok adatok nem lesznek titkosítva.

##<a name="getting-started"></a>Első lépések

###<a name="step-1-create-a-new-storage-accountstorage-create-storage-accountmd"></a>Lépés: 1: [tároló új fiók létrehozása](storage-create-storage-account.md).

###<a name="step-2-enable-encryption"></a>Lépés: 2: Engedélyezni a titkosítást.

Lehetősége van engedélyezni a titkosítási az [Azure-portálon](https://portal.azure.com).

> [AZURE.NOTE] Ha szeretne programozás útján engedélyezése vagy letiltása a tárhely titkosítását tárterület-fiókjában, az [Azure tároló erőforrás szolgáltató REST API -val](https://msdn.microsoft.com/library/azure/mt163683.aspx), a [Tárhely erőforrás szolgáltató ügyfél tár a .NET rendszerhez](https://msdn.microsoft.com/library/azure/mt131037.aspx), [Azure PowerShell](../powershell-install-configure.md)vagy az [Azure CLI](storage-azure-cli.md)is használhatja.

###<a name="step-3-copy-data-to-storage-account"></a>3 lépés: Az adatok másolása tárterület-fiókhoz

Ha engedélyezi a SSE tárterület-fiókjában, és írja be a BLOB szeretné a fiókhoz társított tárhely, a BLOB titkosítja. Csak azok is újraírásra bármely BLOB-tároló fiók már található nem lesz titkosítva. Másolja az adatokat egy tárterület-fiókból egy titkosított, SSE vagy is SSE engedélyezése és a BLOB másolása egy tároló a másikra való meg arról, hogy az előző adatok titkosítva van-e. A következő eszközök bármelyikét használhatja a végrehajtásához.

#### <a name="using-azcopy"></a>AzCopy használata

AzCopy az adatok másolása, és az optimális teljesítmény egyszerű parancsok használata a Microsoft Azure Blob, a fájl és a táblázat tárhelyről készült Windows parancssori segédprogramot. Használhatja ezt a BLOB másolása egyik tárterület-fiókból SSE engedélyezve van egy másikra. 

További információért látogasson el [a AzCopy parancssori segédprogram az adatok átvitele](storage-use-azcopy.md).

#### <a name="using-the-storage-client-libraries"></a>A tároló ügyfél tárak használata

Blob-adatokat is másolhatja, és blob-tárhelyről vagy a rich készletével tároló ügyfél tárai .NET, C++, Java, Android, Node.js, PHP, Python és fonetikus tároló fiókok között.

További információért látogasson el az [első lépések az Azure Blob-tárolóhoz .NET használatával](storage-dotnet-how-to-use-blobs.md).

#### <a name="using-a-storage-explorer"></a>A tároló Intézővel

A tároló Intézővel tárterület-fiókok létrehozása, feltöltése és letöltése az adatok, BLOB tartalmának megtekintése és navigálhat a könyvtárak. Használhatja az alábbiak egyike tölthet fel BLOB szereplő adatoknak titkosítással engedélyezve van a tárterület-fiókjába. Az egyes tároló kezelők is másolhatja adatok a meglévő blob-tárolóhoz a tárhely vagy új tárterület-fiókkal SSE engedélyezve van a különböző tárolóhoz.

További információért látogasson el [Azure tároló kezelők](storage-explorers.md).

###<a name="step-4-query-the-status-of-the-encrypted-data"></a>Lépés: 4: A lekérdezés a titkosított adatok állapotának

Telepítve a tárhely ügyfél tárak frissített verziójának, amely lehetővé teszi, hogy a lekérdezés határozza meg, ha titkosítva van-e egy objektum állapota. Példák a közeljövőben bekerül a dokumentumhoz.

Időközben felhívhatja a [Fiók tulajdonságok beolvasása](https://msdn.microsoft.com/library/azure/mt163553.aspx) ellenőrizze, hogy a tárterület-fiók titkosítás engedélyezve van-e, illetve a tárhely fiók tulajdonságai megtekintheti az Azure-portálon.

##<a name="encryption-and-decryption-workflow"></a>Titkosítás és visszafejtés munkafolyamat

Íme egy rövid leírást a titkosítás/visszafejtés munkafolyamat:

-   Az ügyfél lehetővé teszi, hogy a titkosítási a tárterület-fiók.

-   Ha az ügyfélnek új adatot ír (Blob HELYEZI el, HELYEZI el a továbbfejlesztett fájlblokkolás, oldal HELYEZI, stb.); Blob-tárolóhoz minden írási titkosított 256 bites [AES titkosítást](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), a legmagasabb szintű blokkolása titkosítási közül.

-   Ha az ügyfélnek (első Blob, stb.) adatok eléréséhez szükséges, adatai automatikusan visszafejtett Visszatérés a felhasználó előtt.

-   Titkosítás le van tiltva, ha új írások már nem védett, és a meglévő titkosított adatok megmaradnak titkosított, amíg a felhasználó által újraírásra. Titkosítás engedélyezve, amíg Blob-tárolóhoz írások titkosítja. Adatok állapotának váltása a tárterület-fiókom titkosítás engedélyezése vagy letiltása a felhasználóval nem változik.

-   Minden titkosítási kulcs tárolt, titkosítva, és a Microsoft kezeli.

##<a name="frequently-asked-questions-about-storage-service-encryption-for-data-at-rest"></a>Gyakori kérdések a tárhely szolgáltatás titkosítási az adatokat a többi

**Kérdés: van egy meglévő klasszikus tárterület-fiókból. Is engedélyezni a SSE rajta?**

A: SSE erőforrás-kezelő tárterület-fiókok nem csak támogatja.

**Kérdés: hogyan is titkosítása a klasszikus tárterület-fiókom adatait?**

A: hozzon létre egy új erőforrás-kezelő tárterület-fiókot, és másolja a vágólapra az adatokat [AzCopy](storage-use-azcopy.md) használatával a meglévő klasszikus tárterület-fiókjából az újonnan létrehozott erőforrás-kezelő tárterület-fiókjába. 

Egy másik, hogy a klasszikus tároló fiók áttelepítése az egy erőforrás kezelése tárterület-fiókkal. [További információért Platform támogatott áttelepítési a IaaS forrásokban](https://azure.microsoft.com/blog/iaas-migration-classic-resource-manager/)talál az erőforrás-kezelő klasszikus.

**Kérdés: van egy meglévő erőforrás-kezelő tárterület-fiókból. Is engedélyezni a SSE rajta?**

Válasz: Igen, de csak az újonnan írt BLOB titkosítja. Térjen vissza, és nem már meglévő adatok titkosításához. 

**Probléma: szeretné titkosítsa az aktuális adatokat, a meglévő erőforrás-kezelő tároló fiók?**

A: engedélyezheti SSE egy erőforrás-kezelő tároló fiókban bármikor. BLOB, amelyekre már bemutató azonban nem lesz titkosítva. Titkosítsa az adott BLOB, a másolása egy másik nevet vagy egy másik tároló, és távolítsa el a titkosítatlan verziók.

**Kérdés: használok prémium tároló; van-e lehetőség SSE?**

Válasz: Igen, SSE szabványos tárhely és a prémium tároló támogatott.

**K: Ha e hozzon létre egy új tárterület-fiókot SSE engedélyezése, majd hozzon létre egy új virtuális fiókba való tárolására, amely jelent a virtuális titkosítva van?**

V: Igen. Bármelyik létrehozott lemezt, az új tárterület-fiókhoz használt titkosított mindaddig, amíg a létrehozásuk után SSE engedélyezve van. Ha a virtuális készült Azure piactér használ, a virtuális alap kép marad titkosított; azonban bármelyik végrehajtani, miután a virtuális fel van is írások titkosítja.

**Kérdés: hozhatok létre új tárterület-fiókokat az engedélyezett az Azure PowerShell és Azure CLI SSE?**

V: Igen.

**Kérdés: hogyan sokkal kerülni a tárhely Azure a Ha engedélyezve van a SSE?**

V: nem külön költség nélkül.

**Kérdés: ki kezeli a titkosítási kulcs?**

A: a billentyűparancsok a Microsoft kezeli.

**Kérdés: van lehetőség a saját a titkosítási kulcs?**

A: dolgozunk a vevők ahhoz, hogy a saját titkosítási kulcs funkciók kezeléséről.

**K: visszavonni a hozzáférést a titkosítási kulcs?**

V: nem adott időben; a billentyűparancsok a Microsoft teljesen kezelhetők.

**Kérdés: alapértelmezés szerint engedélyezve SSE esetén hozhatok létre egy új tárterület-fiókba?**

A: SSE; alapértelmezés szerint nincs engedélyezve az Azure portal segítségével kapcsolhatja be. Ez a funkció a tárhely erőforrás szolgáltató REST API programozás útján is engedélyezheti.

**Kérdés: Miben különbözik ez az Azure meghajtó titkosítás?**

Megoldás: Ez a funkció Azure Blob-tárolóban lévő adatok titkosítása használják. Az Azure lemez titkosítást IaaS VMs operációs rendszer és az adatok lemezt titkosítása használják. További információra kíváncsi látogasson el a [Tárhely biztonsági útmutatóját](storage-security-guide.md).

**Kérdés: Mi a teendő, ha e engedélyezése SSE, majd go és Azure lemez titkosítás engedélyezése a lemezen?**

Válasz: Ez problémamentesen működik. Az adatok mindkét módszer szerint titkosítja.

**Probléma: a tárterület-fiókot kell geo redundantly replikált van beállítva. Ha SSE engedélyezéséhez a felesleges másolása is titkosítva lesznek?**

Válasz: Igen, a titkosított összes másolatát a tárterület-fiókot, és minden redundancia beállítások – helyileg felesleges tároló (LRS), zóna-redundáns tároló (ZRS), Geo-redundáns tároló (GRS) és olvasási hozzáférést Geo-redundáns tároló (TS-GRS) – támogatottak.

**Probléma: nem lehet engedélyezni a titkosítási tárterület-fiókom.**

A: egy erőforrás-kezelő tárterület-fiókot is ez? Klasszikus tárterület-fiókok nem támogatottak. 

**Kérdés: SSE csak engedélyezve van az adott régióban?**

A: a SSE minden régióban érhető el. 

**Kérdés: hogyan érhető el valaki az, ha van kapcsolatos problémák megoldásához, vagy visszajelzést szeretne?**

Válasz: kérje a [ssediscussions@microsoft.com](mailto:ssediscussions@microsoft.com) a tárhely szolgáltatás titkosítási kapcsolatos problémák megoldásához.

##<a name="next-steps"></a>Következő lépések

Biztonsági funkciók, amelyek közös engedélyezése a fejlesztők számára biztonságos alkalmazások átfogó Azure tárhelyet biztosít. További információra kíváncsi keresse fel a [Tárhely biztonsági útmutató](storage-security-guide.md).
