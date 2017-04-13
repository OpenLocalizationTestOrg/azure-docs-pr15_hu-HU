<properties
    pageTitle="Gyakori kérdések a Linux VMs |} Microsoft Azure"
    description="Néhány a Linux rendszerhez készült az erőforrás-kezelő modell virtuális gépeken futó kapcsolatos gyakori kérdésekre választ ad."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="cynthn"/>

# <a name="frequently-asked-question-about-linux-virtual-machines"></a>Gyakori kérdések a Linux virtuális gépeken futó

Ez a cikk címek Linux virtuális gépeken futó az erőforrás-kezelő telepítési modell Azure-ban létrehozott kapcsolatos gyakori kérdésekre. Ez a témakör a Windows verzióban című [gyakori kérdés kapcsolatos Windows virtuális gépeken futó](virtual-machines-windows-faq.md)

## <a name="what-can-i-run-on-an-azure-vm"></a>Mi futtatja az Azure virtuális?

Az összes előfizető futtatását is lehetővé teszi a kiszolgálószoftver Azure virtuális-gépen. További tudnivalókért lásd: [a Azure-Endorsed terjesztését Linux](virtual-machines-linux-endorsed-distros.md)


## <a name="how-much-storage-can-i-use-with-a-virtual-machine"></a>Mennyi tárhely használhatók a virtuális gép?

Minden adat lemezre legfeljebb 1 TB lehet. Adatok lemezt használható száma attól függ, hogy a virtuális gép méretét. Részletekért olvassa el [a virtuális gépeken futó méretét](virtual-machines-linux-sizes.md).

Azure tároló fiók tároló biztosít az operációs rendszer lemezen és minden adat lemezt. Minden egyes lemez tárolt, oldal blob-ként .vhd fájl. Az ár a részletek, a [Tárhely árak részletei](https://azure.microsoft.com/pricing/details/storage/)című részben találja.


## <a name="how-can-i-access-my-virtual-machine"></a>Hogyan érhetem el a virtuális számítógépemre?

Jelentkezzen be a virtuális gép, biztonságos rendszerhéj (SSH) használatával távoli kapcsolatot létesíteni. Nézze meg a képernyőn megjelenő utasításokat [a Windows](virtual-machines-linux-ssh-from-windows.md) vagy [Linux rendszerhez és a Mac](virtual-machines-linux-mac-create-ssh-keys.md)csatlakozni. Alapértelmezés szerint SSH lehetővé teszi, hogy legfeljebb 10 egyidejű kapcsolatot. Ennek a számnak növelheti a konfigurációs fájl szerkesztésével.


Ha problémákat tapasztal, olvassa el a [kapcsolatos hibák elhárítása biztonságos rendszerhéj (SSH) kapcsolatok](virtual-machines-linux-troubleshoot-ssh-connection.md).


## <a name="can-i-use-the-temporary-disk-devsdb1-to-store-data"></a>Az ideiglenes lemez (/ fejlesztők/sdb1) segítségével adattárolásra?

Nem használja az ideiglenes lemez (/ fejlesztők/sdb1) tárolt adatok. Még csak szerepel ott átmeneti tárolásra. Adatvesztés sem állíthatók kockáztassa meg.


## <a name="can-i-copy-or-clone-an-existing-azure-vm"></a>Másolja vagy egy meglévő Azure virtuális klónozhatja?

igen. Útmutatásért lásd: [az erőforrás-kezelő telepítési modell Linux virtuális gép másolatának létrehozása](virtual-machines-linux-copy-vm.md).


## <a name="why-am-i-not-seeing-canada-central-and-canada-east-regions-through-azure-resource-manager"></a>Miért nem jelennek meg a közép Kanadán és Kanada keleti régióban Azure erőforrás-kezelővel?

A két új térség, a közép Kanadán és Kanada keleti automatikusan nem regisztrált virtuális gép létrehozása meglévő Azure előfizetésekhez. A regisztráció automatikusan történik, amikor egy virtuális számítógépre telepítve van a Azure erőforrás-kezelővel bármely más régió az Azure portal segítségével. Miután egy virtuális számítógépre telepíti, minden egyéb Azure régió, az új régiók használható fel későbbi virtuális gépeken futó.


## <a name="can-i-add-a-nic-to-my-vm-after-its-created"></a>Vehetek fel egy hálózati kártya a virtuális létrehozása után?

nem. A hálózati kártya hozzáadása csak létrehozáskor végezheti el.


## <a name="are-there-any-computer-name-requirements"></a>Van bármilyen számítógép neve követelmények?

igen. A számítógép neve legfeljebb 64 karakter hosszúságú lehet. [Infrastruktúra elnevezési irányelveket](virtual-machines-linux-infrastructure-naming-guidelines.md) talál további információt az erőforrások elnevezési körül.


## <a name="what-are-the-username-requirements-when-creating-a-vm"></a>Mik azok a felhasználónév követelményeknek, egy virtuális létrehozásakor?

Felhasználónevét 1-64 karakter hosszúságú lehet.

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

Jelszavak kell 6-72 karakter hosszúságú és 3 kívül az alábbi 4 összetettsége követelmények felel meg:

- Alsó karaktereket
- Felső karaktereket
- Van egy számjegyet
- Speciális karaktert (Regex egyeznie [\W_])

A következő jelszavak nem megengedettek:

<table>
    <tr>
        <td style="text-align:center">abc@123</td>
        <td style="text-align:center">P@$$w0rd</td>
        <td style="text-align:center">P@ssw0rd</td>
        <td style="text-align:center">P@ssword123</td>
        <td style="text-align:center">Pa$ a $word</td>
    </tr>
    <tr>
        <td style="text-align:center">pass@word1</td>
        <td style="text-align:center">Jelszó!</td>
        <td style="text-align:center">Jelszo1</td>
        <td style="text-align:center">Password22</td>
        <td style="text-align:center">ILOVEYOU!</td>
    </tr>
</table>
