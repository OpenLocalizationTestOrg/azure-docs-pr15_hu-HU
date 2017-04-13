<properties
    pageTitle="Erőforrás-kezelő környezetben Azure tároló ügyfelek, a tárolók vagy VHD törlésekor kapcsolatos hibák elhárítása |} Microsoft Azure"
    description="Erőforrás-kezelő környezetben Azure tároló ügyfelek, a tárolók vagy VHD törlésekor kapcsolatos hibák elhárítása"
    services="storage"
    documentationCenter=""
    authors="genlin"
    manager="felixwu"
    editor="na"
    tags="storage"/>

<tags
    ms.service="storage"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="genli"/>

# <a name="troubleshoot-errors-when-you-delete-azure-storage-accounts-containers-or-vhds-in-a-resource-manager-deployment"></a>Erőforrás-kezelő környezetben Azure tároló ügyfelek, a tárolók vagy VHD törlésekor kapcsolatos hibák elhárítása

[AZURE.INCLUDE [storage-selector-cannot-delete-storage-account-container-vhd](../../includes/storage-selector-cannot-delete-storage-account-container-vhd.md)]

Hibák jelenhet meg, amikor megpróbálja Azure tárterület-fiókot, tároló vagy virtuális merevlemez (virtuális) az [Azure portál](https://portal.azure.com)törlése. Ez a cikk ismerteti hibaelhárítási egy erőforrás-kezelő Azure környezetben a probléma megoldásához.

Ha ez a cikk az Azure problémájára nem, keresse fel az Azure fórumait [MSDN és Papírhalom túlcsordulás](https://azure.microsoft.com/support/forums/). A probléma a következő fórumokon, vagy tehetnek @AzureSupport a Twitteren. Azure-támogatási kérelmet is is fájl kiválasztásával a **technikai támogatás kérése** az [Azure támogatja a](https://azure.microsoft.com/support/options/) webhelyen.

## <a name="symptoms"></a>A jelenség

### <a name="scenario-1"></a>Forgatókönyv 1

Egy virtuális egy erőforrás-kezelő környezetben tárterület-fiókban törlése használatakor az alábbi hibaüzenet jelenhet meg:

**"Vhds/BlobName.vhd" blob törlése sikertelen volt. Hiba: Jelenleg a bérleti a a blob, és nincs bérleti azonosító megadott a kérést.**

Ez a probléma oka egy virtuális gép (virtuális) a bérleti van a törölni kívánt virtuális.

### <a name="scenario-2"></a>Forgatókönyv 2

Amikor megpróbálja egy erőforrás-kezelő környezetben tárterület-fiókban tároló törlése, az alábbi hibaüzenet jelenhet meg:

**Tárterület-tároló "VHD" törlése sikertelen volt. Hiba: Jelenleg a bérleti a tárolóra, és nincs bérleti azonosító megadott a kérést.**

Ez a probléma akkor fordulhat elő, mert a tároló egy virtuális, amely a bérleti állapotban zárolva van.

### <a name="scenario-3"></a>Forgatókönyv 3

Erőforrás-kezelő környezetben tároló fiók törlése használatakor az alábbi hibaüzenet jelenhet meg:

**"StorageAccountName" tároló fiók törlése sikertelen volt. Hiba: A eltérések használatban miatt nem lehet törölni a tárterület-fiókot.**

Ez a probléma akkor fordulhat elő, mert a tárterület-fiók tartalmaz egy virtuális, hogy a bérleti állapota.

## <a name="solution"></a>Megoldás

A probléma megoldásához, meg kell adnia a virtuális a hibát okozó és a kapcsolódó virtuális. Ezután leválasztása a virtuális a virtuális (az adatok lemezt) a vagy törlése a virtuális, amely a virtuális (-OS lemez) használja. Ez a bérleti eltávolítja a virtuális, és lehetővé teszi, hogy törölhető.

### <a name="step-1-identify-the-problem-vhd-and-the-associated-vm"></a>Lépés: 1: A probléma azonosításához virtuális és a kapcsolódó virtuális


1. Jelentkezzen be az [Azure-portálon](https://portal.azure.com).
2. A **központi** menüben válassza az **összes erőforrás**. Nyissa meg a tárterület-fiókot, amely a törölni kívánt, és válassza a **BLOB** > **VHD**.

    ![Képernyőkép: a tárterület-fiók és a kiemelt "VHD" tárolóhoz a portálon](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/opencontainer.png)

3. Ellenőrizze a tárolóban lévő minden virtuális tulajdonságait. Keresse meg a virtuális, hogy a **Leased** állapota. Ezt követően, hogy melyik virtuális használja a virtuális. Általában megadhatja, hogy melyik virtuális a virtuális rendelkezik, jelölje be a nevét, valamint a virtuális:

    - Operációs rendszer lemezt általában hajtsa végre az Ez az elnevezési konvenció: VMNameYYYYMMDDHHMMSS.vhd
    - Adatok lemezt általában hajtsa végre a elnevezési konvenció: VMName-ÉÉÉÉHHNN-HHMMSS.vhd

    ![A virtuális, a "Zárolt" bérleti állapotát és a kiemelt "Leased" bérleti állapotát a nevet a tároló információ az portálon képernyőképe](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/locatevm.png)

### <a name="step-2-remove-the-lease-from-the-vhd"></a>Lépés: 2: A virtuális a bérleti eltávolítása

A virtuális a virtuális (-OS lemez) használó törlése:

1.  Jelentkezzen be az [Azure-portálon](https://portal.azure.com).
2.  A **központi** menüben jelölje ki a **virtuális gépeken futó**.
3.  Jelölje ki a virtuális egy bérleti tároló a virtuális merevlemezen.
4.  Győződjön meg arról, hogy semmi nem aktívan használja a virtuális gép és a virtuális gép már nem szükséges.
5.  A **virtuális részletek** lap tetején válassza a **Törlés**, és kattintson az **Igen gombra** kattintva erősítse meg.
6.  A virtuális törölni kell, de a virtuális kell tartani. Azonban a virtuális már nem kell tartalmaznia egy bérleti rajta. Eltarthat néhány percig, amíg a bérleti kiadandó. Ha ellenőrizni szeretné, hogy a bérleti megjelent, nyissa meg **az összes erőforrás** > **Tároló fióknév** > **BLOB** > **VHD**. A **Bérleti** állapotérték **Blob-tulajdonságok** ablaktáblájában **feloldva**kell lennie.

A virtuális a virtuális a leválasztása használó meg (az adatok lemezre):

1.  Jelentkezzen be az [Azure-portálon](https://portal.azure.com).
2.  A **központi** menüben jelölje ki a **virtuális gépeken futó**.
3.  Jelölje ki a virtuális egy bérleti tároló a virtuális merevlemezen.
4.  Jelölje ki a **lemezen** a **virtuális részletek** lap a.
5.  Jelölje ki az adatok lemezt a bérleti tároló a virtuális merevlemezen. Megadhatja, hogy melyik virtuális a csatlakozik a lemez, jelölje be az URL-címet, valamint a virtuális.
6.  Határozza meg, hogy semmi nem aktívan használja az adatok lemez biztosan.
7.  Kattintson a **Leválasztás** gombra a **lemez részletek** lap.
8.  A lemezt a virtuális, a most kell leválasztása és a virtuális már nem kell a bérleti rajta. Eltarthat néhány percig, amíg a bérleti kiadandó. Ha ellenőrizni szeretné, hogy a bérleti megjelenése, nyissa meg **az összes erőforrás** > **Tároló fióknév** > **BLOB** > **VHD**. A **Bérleti** állapotérték **Blob-tulajdonságok** ablaktáblájában **feloldva**kell lennie.

## <a name="what-is-a-lease"></a>Mi az a bérleti?

A bérleti használt blob (például egy virtuális) való hozzáférés szabályozása lakat. Ha blob van bérbe, csak a bérleti tulajdonosai a blob érheti el. A bérleti fontos az alábbi okok miatt:

-   Ha több tulajdonosok meg egyszerre a blob azonos részéhez írása megakadályozza, hogy adatsérülés.
-   Megakadályozza, hogy a blob törléséről, ha valami aktívan használja (például egy virtuális).
-   Megakadályozza, hogy a tárterület-fiók törléséről, ha valami aktívan használja (például egy virtuális).



## <a name="next-steps"></a>Következő lépések

- [Tárterület fiók törlése](storage-create-storage-account.md#delete-a-storage-account)
- [Hogyan lehet a zárolt bérleti blob-tárolóhoz a bonthatja a Microsoft Azure (PowerShell)](https://gallery.technet.microsoft.com/scriptcenter/How-to-break-the-locked-c2cd6492)
