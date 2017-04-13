<properties
   pageTitle="Azure tároló biztonsági – áttekintés |} Microsoft Azure"
   description=" Azure tároló a felhőalapú tárolási megoldást tartóssági, elérhetőségét és méretezhetőség az igényeknek megfelelően ügyfeleikkel támaszkodó modern alkalmazásokhoz. Ez a cikk áttekintést nyújt az alapvető Azure biztonsági funkciók Azure adathordozós használható. "
   services="security"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/16/2016"
   ms.author="terrylan"/>

# <a name="azure-storage-security-overview"></a>Azure tároló biztonsági – áttekintés

Azure tároló a felhőalapú tárolási megoldást tartóssági, elérhetőségét és méretezhetőség az igényeknek megfelelően ügyfeleikkel támaszkodó modern alkalmazásokhoz. Azure tárhelyet biztosít egy teljes biztonsági funkciók:

- A tároló fiók védhetők szerepköralapú hozzáférés-vezérlés és Azure Active Directory.
- A védett adatok között egy alkalmazást, és Azure ügyféloldali titkosítást, a HTTPS vagy a kis-és Középvállalatok 3.0 használatával.
- Adatok beállítható, hogy automatikusan titkosítása Azure tárolóhoz írásakor tároló szolgáltatást használ.
- Virtuális gépeken futó által használt operációs rendszer és az adatok lemez adhatja meg az Azure lemez titkosítással titkosítva.
- A data objects Azure-tárolóban lévő delegált eléréséhez lehet adni a megosztott Access aláírások használata.
- A mások által használt tárterület elérésekor kérni hitelesítési módszer követhetők a tárhely analytics használatával.

Azure-tárolóban lévő biztonsági részletesebb figyelmébe olvassa el az [Azure tároló biztonsági útmutató](../storage/storage-security-guide.md). Ez az útmutató mély merülési biztonsági funkciókat Azure-tárterületét például a tárhely fiók kulcsok, az adatok titkosítása a hálózaton átvitt és a többi és tároló analytics.

Ez a cikk áttekintést nyújt Azure biztonsági funkciók Azure adathordozós használható. Hivatkozások találhatók, cikkekre, adja meg adatait az egyes szolgáltatásokhoz, többet is megtudhat.

Íme az alapvető funkcióit, a jelen cikkben nem vonatkozik:

- Szerepköralapú hozzáférés-vezérlés
- Delegált hozzáférést biztosít a tárterület-objektumokhoz
- A hálózaton átvitt titkosítás:
- A többi/tároló titkosítási szolgáltatás titkosítás:
- Azure lemez titkosítás:
- Azure kulcs tárolóból elemre

## <a name="role-based-access-control-rbac"></a>Szerepköralapú hozzáférés-szerepalapú

Szerepköralapú hozzáférés szerepalapú tároló fiókját biztonságát. A [tudnivalók](https://en.wikipedia.org/wiki/Need_to_know) és a [legalacsonyabb jogosultság](https://en.wikipedia.org/wiki/Principle_of_least_privilege) biztonsági elvek alapján hozzáférés korlátozása elengedhetetlen a szervezeteknek szól, amelyek szeretné az adatokat az access biztonsági házirendek részesíteni. Ezeket az engedélyeket a megfelelő RBAC szerepkör hozzárendelése a csoportok és az egyes hatókör alkalmazások által nyújtott. [Beépített RBAC szerepkörök](../active-directory/role-based-access-built-in-roles.md)tárterület-fiók közös munka, például segítségével jogosultságok hozzárendelése a felhasználókhoz.

tudj meg többet:

- [Azure Active Directory-szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-configure.md)

## <a name="delegated-access-to-storage-objects"></a>Delegált hozzáférést biztosít a tárterület-objektumokhoz

Megosztott access aláírás (Társítások) a tárterület-fiókjában erőforrások delegált hozzáférést biztosít. A Társítások, az azt jelenti, hogy adhat az ügyfél korlátozott engedélyek objektumok a tárterület-fiókjában időt és a megadott engedélyeket tartalmazó adott időszakra vonatkozóan. Korlátozott engedélyek anélkül, hogy a fiók hívóbetűk megosztása adhat meg. A Társítások, amely magában foglalja a lekérdezés paraméterei tároló erőforráshoz hitelesített hozzáférés szükséges minden információt URI. A Társítások tároló erőforrások eléréséhez az ügyfél csak a megfelelő konstruktor vagy módszer a Társítások átadni van szüksége.

tudj meg többet:

- [A Társítások modell ismertetése](../storage/storage-dotnet-shared-access-signature-part-1.md)
- [Hozzon létre, és egy Társítások használata Blob-tárolóhoz](../storage/storage-dotnet-shared-access-signature-part-2.md)

## <a name="encryption-in-transit"></a>A hálózaton átvitt titkosítás:
Titkosítás a hálózaton átvitt adatok védelme, mind a hálózaton keresztül továbbításra mechanizmusa. Azure adathordozós biztonságát adatok használata:

- [Átviteli szintű titkosítás](../storage/storage-security-guide.md#encryption-in-transit), például az adatátvitel esetén és Azure tároló elhalványítása HTTPS.
- [Átutalásra vonatkozó titkosítási](../storage/storage-security-guide.md#using-encryption-during-transit-with-azure-file-shares), például a kis-és Középvállalatok 3.0 titkosítás Azure fájlmegosztások.
- [Ügyféloldali titkosítás](../storage/storage-security-guide.md#using-client-side-encryption-to-secure-data-that-you-send-to-storage), az adatok titkosítása előtt tárolóba és visszafejteni az adatokat, után átkerül tároló ki.

További információ az ügyféloldali titkosítás:

- [A Microsoft Azure-tárterületre ügyféloldali titkosítás:](https://blogs.msdn.microsoft.com/windowsazurestorage/2015/04/28/client-side-encryption-for-microsoft-azure-storage-preview/)
- [Cloud security meghatározza a sorozat: hálózaton átvitt adatok titkosítása](http://blogs.microsoft.com/cybertrust/2015/08/10/cloud-security-controls-series-encrypting-data-in-transit/)

## <a name="encryption-at-rest"></a>A többi titkosítás:

Sok szervezetek [a többi adatot titkosítási](https://blogs.microsoft.com/cybertrust/2015/09/10/cloud-security-controls-series-encrypting-data-at-rest/) adatok védelmét, megfelelőség és adatszuverenitás felé kötelező lépés. Vannak, amelyekkel "a többi"-adatok titkosítása három Azure szolgáltatás:

- [Tárterület szolgáltatás titkosítás](../storage/storage-security-guide.md#encryption-at-rest) lehetővé teszi, hogy az a tároló szolgáltatás automatikusan titkosíthatja a adatokat ír be Azure tárolóhoz kérése.
- [Ügyféloldali titkosítás](../storage/storage-security-guide.md#client-side-encryption) is tartalmaz a többi a titkosítási szolgáltatás.
- [Azure lemez titkosítás](../storage/storage-security-guide.md#using-azure-disk-encryption-to-encrypt-disks-used-by-your-virtual-machines) lehetővé teszi az operációs rendszer lemez és adatok lemezt egy IaaS virtuális gép által használt titkosítása.

További információ a tárhely szolgáltatás titkosítás:

- [Azure tároló szolgáltatás titkosítási](https://azure.microsoft.com/services/storage/) [Azure Blob-tárolóhoz](https://azure.microsoft.com/services/storage/blobs/)érhető el. A más Azure tároló típusú részletekért lásd a [fájlt](https://azure.microsoft.com/services/storage/files/), a [lemez (prémium tároló)](https://azure.microsoft.com/services/storage/premium-storage/), a [táblázat](https://azure.microsoft.com/services/storage/tables/)és a [várólista](https://azure.microsoft.com/services/storage/queues/).
- [Azure tároló szolgáltatás titkosítási az adatokat a többi](../storage/storage-service-encryption.md)

## <a name="azure-disk-encryption"></a>Azure lemez titkosítás:

Azure virtuális gépeken futó (VMs) lemez titkosítás segít cím szervezeti biztonság és a megfelelőségi követelmények hogy titkosítja a házirendek szabályozhatja az [Azure kulcs tárolóból elemre](https://azure.microsoft.com/services/key-vault/), és a billentyűk virtuális lemezt (beleértve az indítási és az adatok lemezt).

Lemezen titkosítás VMs Linux és a Windows operációs rendszerek működik. Segít megóvása, kezelése és a lemez titkosítási kulcsok használatának naplózása kulcs tárolóból elemre is használja. A virtuális merevlemezeken lévő összes adatot titkosítja a többi segítségével szabványos titkosítási technológiára az Azure tárterület-fiókjában. A lemez titkosítási megoldást a Windows [Microsoft BitLocker meghajtó használatára](https://technet.microsoft.com/library/cc732774.aspx)alapul, és a Linux megoldást [a titkosítási dm](https://en.wikipedia.org/wiki/Dm-crypt)alapul.

tudj meg többet:

- [A Windows és Linux IaaS virtuális gépeken futó Azure lemez titkosítás](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0)

## <a name="azure-key-vault"></a>Azure kulcs tárolóból elemre

Azure lemez titkosítás szabályozhatja, és kezelheti a lemez titkosítási kulcs és titkos kulcsok kulcs tárolóból elemre az előfizetésben, biztosítva, hogy a virtuális gép lemezen az összes adat a Azure-tárolóban lévő többi a titkosított használja [Azure kulcs tárolóból elemre](https://azure.microsoft.com/services/key-vault/) . Kulcs tárolóra kulcsok és a házirend-használati naplózandó kell használni.

tudj meg többet:

- [Mi az Azure kulcs tárolóra?](../key-vault/key-vault-whatis.md)
- [Első lépések az Azure kulcs tárolóból elemre](../key-vault/key-vault-get-started.md)
