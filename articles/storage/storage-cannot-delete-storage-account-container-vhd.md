<properties
    pageTitle="Azure tároló ügyfelek, a tárolók vagy VHD törlése klasszikus környezetben elhárítása |} Microsoft Azure"
    description="Azure tároló ügyfelek, a tárolók vagy VHD törlése klasszikus környezetben hibaelhárítása"
    services="storage"
    documentationCenter=""
    authors="genlin"
    manager="felixwu"
    editor="tysonn"
    tags="storage"/>

<tags
    ms.service="storage"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="genli"/>

# <a name="troubleshoot-deleting-azure-storage-accounts-containers-or-vhds-in-a-classic-deployment"></a>Azure tároló ügyfelek, a tárolók vagy VHD törlése klasszikus környezetben hibaelhárítása

[AZURE.INCLUDE [storage-selector-cannot-delete-storage-account-container-vhd](../../includes/storage-selector-cannot-delete-storage-account-container-vhd.md)]

Hibák jelenhet meg, amikor megpróbálja az Azure tárterület-fiókot, tároló vagy az [Azure portál](https://portal.azure.com/) vagy az [Azure klasszikus portál](https://manage.windowsazure.com/)virtuális törlése. Az alábbi esetekben a problémákat oka lehet:

-   Amikor töröl egy virtuális, a lemez és a virtuális nem automatikusan törlődnek. Előfordulhat, hogy tárterületet fiók törlése a hiba okát. A lemez azt nem törölheti, hogy a lemez csatlakoztatása egy másik virtuális használható.

-   Továbbra is fennáll a bérleti a lemezen vagy a blob, amely a lemez van társítva.

Ha az Azure nem kérdést az Ez a cikk, keresse fel a Azure fórumait [MSDN és a túlcsordulás Papírhalom](https://azure.microsoft.com/support/forums/). A problémát a következő fórumokon, vagy tehetnek @AzureSupport a Twitteren. Azure támogatási kérelmet is is fájl kiválasztásával a **technikai támogatás kérése** az [Azure támogatja a](https://azure.microsoft.com/support/options/) webhelyen.

## <a name="symptoms"></a>A jelenség

A következő szakasz kaphat, ha megpróbál törölni az Azure tároló partnerek, a tárolók vagy a VHD kapcsolatos gyakori hibákra sorolja fel.

### <a name="scenario-1-unable-to-delete-a-storage-account"></a>Alkalmazási helyzetek 1: Nem lehet tároló fiók törlése

Nyissa meg a tárterület-fiókjába, az [Azure portál](https://portal.azure.com/) vagy [Azure klasszikus portál](https://manage.windowsazure.com/) és **törlése**, ha az alábbi hibaüzenet jelenhet meg:

*Tárterület-fiók StorageAccountName virtuális képeket is tartalmaz. Győződjön meg róla, ezeket a képeket az virtuális törlődik a tárterület-fiók törlése előtt.*

Ez a hiba is jelenhetnek meg:

**Az Azure-portálon**:

*Tárterület-fiók < virtuális-tárterület-fiók-neve > törlése sikertelen volt. Nem lehet törölni a tárterület-fiók < virtuális-tárterület-fiók-neve >: "tároló < virtuális-tárterület-fiók-neve > fióknak néhány aktív kép(ek) és/vagy lemez(ek). Győződjön meg róla, ezek kép(ek) és/vagy lemez(ek) törlődik a tárterület-fiók törlése előtt. ".*

**Az Azure klasszikus portálon**:

*Tárterület < virtuális-tárterület-fiók-neve > fióknak néhány aktív kép(ek) és/vagy lemez(ek), például xxxxxxxxx-xxxxxxxxx-O – 209490240936090599. Győződjön meg róla, ezek kép(ek) és/vagy lemez(ek) törlődik a tárterület-fiók törlése előtt.*

Vagy

**Az Azure-portálon**:

*Tároló fiók < virtuális-tárterület-fiók-neve > 1 container(s) aktív kép és/vagy a lemez eltérések rendelkező rendelkezik. Győződjön meg róla, ilyen eltéréseket a kép tárházba törlődjenek a tárterület-fiók törlése előtt*.

**Az Azure klasszikus portálon**:

*Nem sikerült tároló elküldése fiók < virtuális-tárterület-fiók-neve > 1 container(s) aktív kép és/vagy a lemez eltérések rendelkező rendelkezik. Gondoskodjon arról, hogy ilyen eltéréseket a kép tárházba törlődjenek a tárterület-fiók törlése előtt. Amikor megpróbálja tároló fiók törlése és vannak társítva, aktív lemezt, látni fogja a egy üzenet jelenik meg, amely lehet törölni kell az aktív lemez*.

### <a name="scenario-2-unable-to-delete-a-container"></a>2 alkalmazási helyzetek: Nem lehet törölni a tároló

Törölje a tárhely tároló próbál, a következő hiba jelenhet meg:

*Tároló tároló törlése sikertelen volt <container name>. Hiba: "jelenleg a bérleti a tároló, és nincs bérleti azonosító megadott a kérelem*.

### <a name="scenario-3-unable-to-delete-a-vhd"></a>Alkalmazási helyzetek 3: Nem lehet törölni egy virtuális

Után egy virtuális törlése, és próbálkozzon újra a számítógépet a BLOB törlése a társított rendszerindításának, előfordulhat, hogy a következő hibaüzenet:

*Nem sikerült blob törlése "elérési út/XXXXXX-XXXXXX-os-1447379084699.vhd'. Hiba: "jelenleg a bérleti a blob, és nincs bérleti azonosító megadott a kérést.*

## <a name="solution"></a>Megoldás
A leggyakoribb problémák megoldásához próbálkozzon az alábbi módon:

### <a name="step-1-delete-any-os-disks-that-are-preventing-deletion-of-the-storage-account-container-or-vhd"></a>Lépés: 1: Minden olyan OS lemezt, amelyek megakadályozzák a tárterület-fiókot, a tároló vagy a virtuális törlésének törlése

1. Váltás az [Azure klasszikus portálon](https://manage.windowsazure.com/).
2. Jelölje ki a **virtuális gép** > **lemezt**.

    ![A klasszikus portálon Azure virtuális gépeken lemez képe.](./media/storage-cannot-delete-storage-account-container-vhd/VMUI.png)

3. Keresse meg a lemezt társított a tárterület-fiókot, tároló vagy virtuális, amelyeket törölni szeretne. Ha bejelöli a lemez helyét, a társított tárterület-fiókot, a tároló vagy a virtuális kifejezésre keresve.

    ![Azure klasszikus portálon helyére vonatkozó adatok a lemezt megjelenítő kép](./media/storage-cannot-delete-storage-account-container-vhd/DiskLocation.png)

4. Győződjön meg arról, hogy nincs virtuális megjelenik-e a lemezt **csatolt** mezőre, és törölje a lemezt.

    > [AZURE.NOTE] Lemezen egy virtuális van csatolva, ha nem tudja törölni. Lemezen vannak le egy törölt virtuális aszinkron. A virtuális az ebben a mezőben törölje a törlése után néhány percig is eltarthat.

### <a name="step-2-delete-any-vm-images-that-are-preventing-deletion-of-the-storage-account-or-container"></a>Lépés: 2: A tárterület-fiókja vagy a tároló törlés meggátolják virtuális képet törlése

1. Váltás az [Azure klasszikus portálon](https://manage.windowsazure.com/).
2. Jelölje ki a **virtuális gép** > **KÉPEKET**, és törölje a képek társított a tárterület-fiókot, a tároló vagy a virtuális.

    Ezután próbálja meg újból a tárterület-fiókot, a tároló vagy a virtuális törölni.

> [AZURE.WARNING] Feltétlenül készítsen biztonsági másolatot bármi szeretné menteni a fiók törlése előtt. Még nem lehet állítani egy törölt tárterület-fiókot vagy beolvasni a tartalom törlés előtt tartalmazó közül. Ez is igaznak az erőforrások fiók: virtuális, blob, tábla, várólista vagy fájl törlése után végleg törlődik. Győződjön meg arról, hogy az erőforrás még nincs használatban.

## <a name="about-the-stopped-deallocated-status"></a>Leállítva (felszabadítása) állapotával

A klasszikus telepítési modell létrehozott és, amely megőrzi VMs [Azure portál](https://portal.azure.com/) vagy [Azure klasszikus portál](https://manage.windowsazure.com/) **Leállítva (felszabadítása)** állapotát lesz.

**Azure klasszikus portálon**:

![Leállítva (Deallocated) állapot VMs az Azure portálon.](./media/storage-cannot-delete-storage-account-container-vhd/moreinfo2.png)


**Azure portálon**:

![Leállítva (felszabadítása) állapota VMs Azure klasszikus portálon.](./media/storage-cannot-delete-storage-account-container-vhd/moreinfo1.png)

"Leállítva (felszabadítása)" állapotú a számítógép erőforrások, például a Processzor, a memória és a hálózati elengedi. A lemezt, azonban továbbra is megmarad, hogy gyorsan újra létrehozhatja a virtuális szükség esetén. Ezeket a lemezt VHD, amely Azure tároló készül fölött jön létre. A tárterület-fióknak ezek VHD, és a lemez bérletek arról e VHD.

## <a name="next-steps"></a>Következő lépések

- [Tárterület fiók törlése](storage-create-storage-account.md#delete-a-storage-account)
- [Hogyan lehet a zárolt bérleti blob-tárolóhoz a bonthatja a Microsoft Azure (PowerShell)](https://gallery.technet.microsoft.com/scriptcenter/How-to-break-the-locked-c2cd6492)
