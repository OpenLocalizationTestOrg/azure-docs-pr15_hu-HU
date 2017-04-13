<properties
    pageTitle="Gyakori kérdések a Windows VMs |} Microsoft Azure"
    description="Néhány a Windows és az erőforrás-kezelő modellt létre virtuális gépeken futó kapcsolatos gyakori kérdésekre választ ad."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="cynthn"/>

# <a name="frequently-asked-question-about-windows-virtual-machines"></a>Gyakori kérdések a Windows virtuális gépeken futó 


Ez a cikk a Windows Azure az erőforrás-kezelő telepítési modell használatával létrehozott virtuális gépeken futó kapcsolatos gyakori kérdésekre foglalkozik. Ez a témakör Linux verziójához talál a [gyakori kérdés kapcsolatos Linux virtuális gépeken futó](virtual-machines-linux-faq.md)

## <a name="what-can-i-run-on-an-azure-vm"></a>Mi futtatja az Azure virtuális?

Az összes előfizető futtatását is lehetővé teszi a kiszolgálószoftver Azure virtuális-gépen. A a Microsoft szoftverlicenc-kiszolgálón futó Azure-ban szabályzata információkért olvassa el a [Microsoft kiszolgálószoftver támogatja az Azure virtuális gépeken futó](https://support.microsoft.com/kb/2721672) című témakört.

A Windows 7 és Windows 8.1 egyes verzióiban MSDN Azure kedvezménye előfizetők és MSDN fejlesztők és tesztelése kirovó előfizetők fejlesztés és tesztelje a tevékenységek érhetők el. Adatait, beleértve az utasításokat és korlátozásokat olvassa el a [Windows-ügyfél képeket találhat az MSDN előfizetők számára](http://azure.microsoft.com/blog/2014/05/29/windows-client-images-on-azure/)című témakört. 


## <a name="how-much-storage-can-i-use-with-a-virtual-machine"></a>Mennyi tárhely használhatók a virtuális gép?

Minden adat lemezre legfeljebb 1 TB lehet. Adatok lemezt használható száma attól függ, hogy a virtuális gép méretét. Részletekért olvassa el [a virtuális gépeken futó méretét](virtual-machines-windows-sizes.md).

Azure tároló fiók tároló biztosít az operációs rendszer lemezen és minden adat lemezt. Minden egyes lemez tárolt, oldal blob-ként .vhd fájl. Az ár a részletek, a [Tárhely árak részletei](https://azure.microsoft.com/pricing/details/storage/)című részben találja.


## <a name="how-can-i-access-my-virtual-machine"></a>Hogyan érhetem el a virtuális számítógépemre?

Kapcsolat létrehozása távoli asztali kapcsolat (RDP) használata a Windows virtuális. További tudnivalókért megtudhatja, [hogy miként csatlakozhat, és jelentkezzen be az Azure virtuális gép Windows operációs rendszert futtató](virtual-machines-windows-connect-logon.md). Két egyidejű kapcsolatok legfeljebb támogatottak, kivéve, ha a kiszolgáló egy távoli asztali szolgáltatásokat munkamenetgazda van beállítva.  


Ha a netán problémákat tapasztal a távoli asztal, olvassa el a [kapcsolatos hibák elhárítása távoli asztali kapcsolat a Windows-alapú Azure virtuális géphez](virtual-machines-windows-troubleshoot-rdp-connection.md). 

Ha ismeri a Hyper-V szolgáltatást úgy, előfordulhat, hogy kell a keresett VMConnect hasonló eszköz. Azure hasonló eszköz nem kínál, mert egy virtuális géphez konzol access a nem támogatott.

## <a name="can-i-use-the-temporary-disk-the-d-drive-by-default-to-store-data"></a>Az ideiglenes lemez (a d meghajtó alapértelmezés szerint) segítségével adattárolásra?

Nem használja az ideiglenes lemez adatait tárolja. Gomb csak akkor átmeneti tárolására, így volna kockázat a sem állíthatók adatvesztés. Az adatok elvesztését akkor fordulhat elő, amikor a virtuális gép egy másik állomás lép. Virtuális gép átméretezés, olyan kapni a fogadó vagy az állomáson hardver hiba néhány lehetséges oka egy virtuális gép lehet áthelyezni.

Ha olyan alkalmazás, amely a d meghajtó betű van szüksége, a meghajtó betűket, hogy az ideiglenes lemez használja, ne D: rendelheti. Című cikkben olvashat [a Windows ideiglenes lemez betűjele módosítása](virtual-machines-windows-classic-change-drive-letter.md).

## <a name="how-can-i-change-the-drive-letter-of-the-temporary-disk"></a>Hogyan változtathatom meg a ideiglenes lemez betűjele?

Módosíthatja a betűjele áthelyezése a lapon fájlt, és újbóli hozzárendelése meghajtó betűket, de akkor győződjön meg arról, hogy egy adott sorrendben végezze. Című cikkben olvashat [a Windows ideiglenes lemez betűjele módosítása](virtual-machines-windows-classic-change-drive-letter.md).

## <a name="can-i-add-an-existing-vm-to-an-availability-set"></a>Vehetek fel egy meglévő virtuális egy elérhetőségének beállítása?

nem. Ha azt szeretné, hogy a virtuális az elérhetőség része lesz, a virtuális meghatározott belül létrehozásához szükséges. Jelenleg nem kínál egy virtuális egy elérhetővé után lett létrehozva.

## <a name="can-i-upload-a-virtual-machine-to-azure"></a>E feltölthet egy virtuális gép Azure?

igen. Című cikkben olvashat [a virtuális Windows Azure kép feltöltése](virtual-machines-windows-upload-image.md)

## <a name="can-i-resize-the-os-disk"></a>Át tudom méretezni a OS lemez?

igen. Útmutatásért lásd: [bontsa ki a virtuális gép Azure-erőforrás csoportban a OS meghajtót](virtual-machines-windows-expand-os-disk.md).

## <a name="can-i-copy-or-clone-an-existing-azure-vm"></a>Másolja vagy egy meglévő Azure virtuális klónozhatja?

igen. Útmutatásért lásd: [az erőforrás-kezelő telepítési modell Windows virtuális gép másolatának létrehozása](virtual-machines-windows-vhd-copy.md).

## <a name="why-am-i-not-seeing-canada-central-and-canada-east-regions-through-azure-resource-manager"></a>Miért nem jelennek meg a közép Kanadán és Kanada keleti régióban Azure erőforrás-kezelővel?

A két új térség, a közép Kanadán és Kanada keleti automatikusan nem regisztrált virtuális gép létrehozása meglévő Azure előfizetésekhez. A regisztráció automatikusan történik, amikor egy virtuális számítógépre telepítve van a Azure erőforrás-kezelővel bármely más régió az Azure portal segítségével. Miután egy virtuális számítógépre telepíti, minden egyéb Azure régió, az új régiók használható fel későbbi virtuális gépeken futó.

## <a name="does-azure-support-linux-vms"></a>Azure támogatja a Linux VMs?

igen. Egy Linux virtuális kipróbálására gyors létrehozásához lásd: a [Create a portálon Azure virtuális egy Linux gép](virtual-machines-linux-quick-create-portal.md).

## <a name="can-i-add-a-nic-to-my-vm-after-its-created"></a>Vehetek fel egy hálózati kártya a virtuális létrehozása után?

nem. A hálózati kártya hozzáadása csak létrehozáskor végezheti el.

## <a name="are-there-any-computer-name-requirements"></a>Van bármilyen számítógép neve követelmények?

igen. A számítógép neve legfeljebb 15 karakter hosszúságú lehet. [Infrastruktúra elnevezési irányelveket](virtual-machines-windows-infrastructure-naming-guidelines.md) talál további információt az erőforrások elnevezési körül.

## <a name="what-are-the-username-requirements-when-creating-a-vm"></a>Mik azok a felhasználónév követelményeknek, egy virtuális létrehozásakor?

Legfeljebb 20 karakter hosszúságú lehet felhasználónevét, és nem végződhet ponttal ("."). 

A következő felhasználónevét nem megengedettek:

<table>
    <tr>
        <td style="text-align:center">rendszergazda </td><td style="text-align:center"> rendszergazda </td><td style="text-align:center"> felhasználói </td><td style="text-align:center"> Felhasználó1</td>
    </tr>
    <tr>
        <td style="text-align:center">teszt </td><td style="text-align:center"> Felhasználó2 </td><td style="text-align:center"> Teszt1 </td><td style="text-align:center"> Felhasználó3</td>
    </tr>
    <tr>
        <td style="text-align:center">rendszergazda1 </td><td style="text-align:center"> 1 </td><td style="text-align:center"> 123 </td><td style="text-align:center"> egy</td>
    </tr>
    <tr>
        <td style="text-align:center">actuser  </td><td style="text-align:center"> ADM </td><td style="text-align:center"> admin2 </td><td style="text-align:center"> ASPNET</td>
    </tr>
    <tr>
        <td style="text-align:center">biztonsági másolat </td><td style="text-align:center"> konzol </td><td style="text-align:center"> Belinszki </td><td style="text-align:center"> a vendégként való bekapcsolódáshoz</td>
    </tr>
    <tr>
        <td style="text-align:center">János </td><td style="text-align:center"> tulajdonos </td><td style="text-align:center"> legfelső szintű </td><td style="text-align:center"> kiszolgáló</td>
    </tr>
    <tr>
        <td style="text-align:center">SQL </td><td style="text-align:center"> támogatás </td><td style="text-align:center"> support_388945a0 </td><td style="text-align:center"> sys</td>
    </tr>
    <tr>
        <td style="text-align:center">Teszt2 </td><td style="text-align:center"> Teszt3 </td><td style="text-align:center"> Felhasználó4 </td><td style="text-align:center"> user5</td>
    </tr>
</table>

## <a name="what-are-the-password-requirements-when-creating-a-vm"></a>Mik azok a jelszókövetelmények, egy virtuális létrehozásakor?

Jelszavak kell a 8-123 karakter hosszúságú és 3 kívül az alábbi 4 összetettsége követelmények felel meg:

- Alsó karaktereket
- Felső karaktereket
- Van egy számjegyet
- Speciális karaktert (Regex egyeznie [\W_])

A következő jelszavak nem megengedettek:

<table>
    <tr>
        <td style="text-align:center">abc@123</td><td style="text-align:center">P@$$w0rd</td><td style="text-align:center">P@ssw0rd</td><td style="text-align:center">P@ssword123</td><td style="text-align:center">Pa$ a $word</td>
    </tr>
    <tr>
        <td style="text-align:center">pass@word1</td><td style="text-align:center">Jelszó!</td><td style="text-align:center">Jelszo1</td><td style="text-align:center">Password22</td><td style="text-align:center">ILOVEYOU!</td>
    </tr>
</table>
