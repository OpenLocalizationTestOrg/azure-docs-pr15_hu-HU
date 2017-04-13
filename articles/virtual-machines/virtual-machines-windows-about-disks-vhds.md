<properties
    pageTitle="A lemez és a Windows VMs VHD |} Microsoft Azure"
    description="Tudjon meg többet a lemez és a Windows VHD virtuális gépeken futó Azure-ban alapjait."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="cynthn"/>

# <a name="about-disks-and-vhds-for-azure-virtual-machines"></a>Tudnivalók a lemez és VHD az Azure virtuális gépeken futó

Bármely olyan számítógépen, mint az Azure virtuális gépeken futó használva lemezt helyként az operációs rendszer, az alkalmazások és az adatok. Az összes Azure virtuális gépeken futó legalább két lemez – a Windows operációs rendszer és egy ideiglenes lemezzel van. Az operációs rendszer lemezen létrejön a képet, és az operációs rendszer lemezen, mind a kép virtuális merevlemez (VHD) tárolt Azure tárterület-fiókkal. Virtuális gépeken futó is beállíthatja, hogy egy vagy több adatok lemezre, mint VHD is találhatók. Ez a cikk olyan [Linux virtuális gépeken futó](virtual-machines-linux-about-disks-vhds.md)is érhető el.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]



## <a name="operating-system-disk"></a>Operációs rendszer lemez

Minden virtuális gép több csatolt operációs rendszer lemez tartalmaz. Van egy SATA meghajtó azonosítóként regisztrált és alapértelmezés szerint a c meghajtó felirata. Ez a lemez maximális kapacitása 1023 gigabájt (GB) nem. 

##<a name="temporary-disk"></a>Ideiglenes lemez

Az ideiglenes lemez automatikusan létrejön. Az ideiglenes lemez felirata a d meghajtó alapértelmezés szerint és azt pagefile.sys tárolására szolgál. 

Az ideiglenes lemezre méretének függően változnak a virtuális gép méretét. További tudnivalókért lásd: [Windows méretű virtuális gépeken futó](virtual-machines-windows-sizes.md).

>[AZURE.WARNING] Ne tárolja az adatokat az ideiglenes lemezen. Átmeneti tárolására szolgál az alkalmazások és folyamatok és a célja, hogy csak tárolja az adatokat, például az oldal vagy felcserélése fájlokat. A lemez egy másik meghajtóra levél rendelnie, lásd: [a Windows ideiglenes lemez betűjele módosítása](virtual-machines-windows-classic-change-drive-letter.md).

További tájékoztatást a hogyan Azure használja-e az ideiglenes lemezre [ismertetése](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/) című cikk nyújt az ideiglenes meghajtó a Microsoft Azure virtuális gépeken futó

## <a name="data-disk"></a>Adatok lemez

Adatok lemezen egy virtuális tárolni alkalmazás adatokat, illetve egyéb adatokat szeretne rögzíteni virtuális géphez csatlakoztatott. Adatok lemez SCSI meghajtók nyilvántartott, és a választott betűvel fel van tüntetve.  Minden adat lemezre van maximális kapacitása 1023 GB-ot. A virtuális gép méretének határozza meg, hogy hány, csatolása és tároló típusú adatok lemezt használhatja a lemez tárolni.

>[AZURE.NOTE] Virtuális gépeken futó kapacitás kapcsolatos további tudnivalókért olvassa el a [Windows méretű virtuális gépeken futó](virtual-machines-windows-sizes.md)című témakört.

Azure az operációs rendszer lemez hoz létre, a képről egy virtuális gép létrehozásakor. Adatok lemezt tartalmazó kép használata esetén Azure is hoz létre az adatok lemezt amikor létrehozza a virtuális gépen. Egyéb esetben hozzáadása adatok lemezt a virtuális gép létrehozása után.

Is hozzáadhat adatok lemezt virtuális géphez bármikor **csatolása** a lemezt a virtuális géphez. A virtuális, amely az Ön által feltöltött vagy másolni a tárterület-fiókot, illetve egy Azure létrehoz, is használhatja. A virtuális feltüntetésével lemezen adatok társít a virtuális fájl "bérleti" helyezze a virtuális merevlemezen, ezért nem törölhető tárhelyről miközben továbbra is csatlakozik.

## <a name="about-vhds"></a>Tudnivalók a VHD

Az Azure-ban használt VHD .vhd fájlok tárolása, oldal BLOB egy szabványos vagy magasabb szintű tároló fiókban Azure-ban. Lap BLOB kapcsolatos további tudnivalókért lásd: a [ismertetése blokk BLOB és lap BLOB](https://msdn.microsoft.com/library/ee691964.aspx). Prémium tároló kapcsolatos további tudnivalókért lásd [prémium tároló: Azure virtuális gép munkaterhelésekből nagy teljesítményű tárterület](../storage/storage-premium-storage.md).

Azure támogatja a merevlemez virtuális formátumot. A rögzített formátumú megállapítja a logikai lemez lineárisan belül a fájlt, úgy, hogy a lemez eltolás X blob eltolás X van tárolva. Kis élőlábba a blob végén a virtuális merevlemez-tulajdonságokat ismerteti. A rögzített formátumú hulladékokat gyakran, hely, mert a legtöbb lemez van-e még nem használt tartomány nagy őket. Azonban Azure tárolja, .vhd fájlok ritka formátumban, így a rögzített és dinamikus lemezre előnyei kap egy időben. További tudnivalókért olvassa el a [virtuális merevlemezeken – első lépések](https://technet.microsoft.com/library/dd979539.aspx)című témakört.

Az összes .vhd lemezen vagy képek létrehozása adatforrásként használni kívánt Azure-ban fájlok csak olvasható. A lemez vagy kép létrehozásakor Azure lemásolja a .vhd fájlokat. Ezek a másolatok lehet írásvédett vagy és-írási és olvasási, attól függően, hogy hogyan használhatja a virtuális.

Ha egy virtuális gép képből, Azure hoz létre, amely a forrás .vhd fájl egy példányát a virtuális gép lemezen. És vírusvédelmet véletlen törlését, Azure helyez el egy bérleti bármely .vhd forrásfájl, amely a képet, az operációs rendszer lemez vagy adatok lemezen létrehozására szolgál.

A forrásfájl .vhd törlése előtt kell a bérleti eltávolítása törli a lemez vagy a képet. A virtuális gép, az operációs rendszer lemezen, törölheti, és a forrásfájl .vhd egyszerre virtuális gépen törlése, illetve törlésével, az összes kapcsolódó lemezt. Azonban tárolt adatok lemezen forrás .vhd fájlok törlése több lépésből beállítása sorrendben. Először a lemezt a virtuális számítógépről leválasztása, majd törölje a lemez, majd törölje a .vhd fájlt.

>[AZURE.WARNING] Törölje a forrásfájl .vhd tárból, vagy törlése a tárterület-fiókját, akkor az a Microsoft nem állíthatók helyre az adatokat.



## <a name="next-steps"></a>Következő lépések
-  [Lemezen csatolása](virtual-machines-windows-attach-disk-portal.md) további tárterület hozzáadásához a virtuális.
-  [A virtuális Windows Azure képet feltölteni](virtual-machines-windows-upload-image.md) használni egy új virtuális létrehozásakor.
-  [A Windows ideiglenes lemez betűjele módosítása](virtual-machines-windows-classic-change-drive-letter.md) úgy, hogy az alkalmazás adatainak használhat a d meghajtó.
