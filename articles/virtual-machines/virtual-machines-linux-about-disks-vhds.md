<properties
    pageTitle="Lemez és a Linux VMs VHD |} Microsoft Azure"
    description="Tudjon meg többet a lemez és VHD alapjai Linux virtuális gépeken futó Azure-ban."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/16/2016"
    ms.author="cynthn"/>

# <a name="about-disks-and-vhds-for-azure-virtual-machines"></a>Tudnivalók a lemez és VHD az Azure virtuális gépeken futó

Bármely olyan számítógépen, mint az Azure virtuális gépeken futó használva lemezt helyként az operációs rendszer, az alkalmazások és az adatok. Az összes Azure virtuális gépeken futó legalább két lemez – egy Linux operációs rendszer (Ha egy Linux virtuális) és egy ideiglenes lemezzel van. Az operációs rendszer lemezen létrejön a képet, és az operációs rendszer lemezen, mind a kép ténylegesen virtuális merevlemez (VHD) tárolt Azure tárterület-fiókkal. Virtuális gépeken futó is beállíthatja, hogy egy vagy több adatok lemezre, mint VHD is találhatók. Ez a cikk a [Windows virtuális gépeken futó](virtual-machines-windows-about-disks-vhds.md)is érhető el.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

> [AZURE.NOTE] Ha egy darabig, kérjük, segítse a szolgáltatás javítható az Azure Linux virtuális át ezt a [rövid felmérés](https://aka.ms/linuxdocsurvey) a tapasztalok vételével. Minden válasz segít a munka elvégzéséhez kaphat segítséget.

## <a name="operating-system-disk"></a>Operációs rendszer lemez

Minden virtuális gép több csatolt operációs rendszer lemez tartalmaz. Van egy SATA meghajtó azonosítóként regisztrált és alapértelmezés szerint a c meghajtó felirata. Ez a lemez maximális kapacitása 1023 gigabájt (GB) nem. 

##<a name="temporary-disk"></a>Ideiglenes lemez

Az ideiglenes lemez automatikusan létrejön. Linux virtuális gépeken a lemez általában /dev/sdb és van formázva és az Azure Linux ügynök /mnt/resource szeretne csatlakoztatni.

Az ideiglenes lemezre méretének függően változnak a virtuális gép méretét. További tudnivalókért lásd: [Linux virtuális gépeken futó méretét](virtual-machines-linux-sizes.md).

>[AZURE.WARNING] Ne tárolja az adatokat az ideiglenes lemezen. Átmeneti tárolására szolgál az alkalmazások és folyamatok és a célja, hogy csak tárolja az adatokat, például az oldal vagy felcserélése fájlokat. 

További tájékoztatást a hogyan Azure használja-e az ideiglenes lemezre [ismertetése](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/) című cikk nyújt az ideiglenes meghajtó a Microsoft Azure virtuális gépeken futó

## <a name="data-disk"></a>Adatok lemez

Adatok lemezen egy virtuális tárolni alkalmazás adatokat, illetve egyéb adatokat szeretne rögzíteni virtuális géphez csatlakoztatott. Adatok lemez SCSI meghajtók nyilvántartott, és a választott betűvel fel van tüntetve.  Minden adat lemezre van maximális kapacitása 1023 GB-ot. A virtuális gép méretének határozza meg, hogy hány, csatolása és tároló típusú adatok lemezt használhatja a lemez tárolni.

>[AZURE.NOTE] A virtuális gépeken futó kapacitás, lásd: [Linux virtuális gépeken futó méretét](virtual-machines-linux-sizes.md).

Azure az operációs rendszer lemez hoz létre, a képről egy virtuális gép létrehozásakor. Adatok lemezt tartalmazó kép használata esetén Azure is hoz létre az adatok lemezt amikor létrehozza a virtuális gépen. Egyéb esetben hozzáadása adatok lemezt a virtuális gép létrehozása után.

Is hozzáadhat adatok lemezt virtuális géphez bármikor **csatolása** a lemezt a virtuális géphez. A virtuális, amely az Ön által feltöltött vagy másolni a tárterület-fiókot, illetve egy Azure létrehoz, is használhatja. A virtuális gép feltüntetésével lemezen adatok társít a virtuális fájlt tároló fiókjából "bérleti" helyezze be a virtuális, ezért nem törölhető tárhelyről miközben továbbra is csatlakozik.

## <a name="about-vhds"></a>Tudnivalók a VHD

Az Azure-ban használt VHD .vhd fájlok tárolása, oldal BLOB egy szabványos vagy magasabb szintű tároló fiókban Azure-ban. Lap BLOB kapcsolatos részletekért lásd: [ismertetése blokk BLOB és lap BLOB](https://msdn.microsoft.com/library/ee691964.aspx). Prémium tároló kapcsolatos részletekért lásd: [prémium tároló: Azure virtuális gép munkaterhelésekből nagy teljesítményű tárterület](../storage/storage-premium-storage.md).

Azure támogatja a merevlemez virtuális formátumot. A rögzített formátum megállapítja a logikai lemez lineárisan belül a fájlt úgy, hogy a lemez eltolás X blob eltolás X van tárolva. Kis élőlábba a blob végén a virtuális merevlemez-tulajdonságokat ismerteti. Gyakran a rögzített formátum Helypazarlás, mert a legtöbb lemez van-e még nem használt tartomány nagy őket. Azonban Azure tárolja, .vhd fájlok ritka formátumban, így a rögzített és dinamikus lemezre előnyei kap egy időben. További információra kíváncsi olvassa el a [virtuális merevlemezeken – első lépések](https://technet.microsoft.com/library/dd979539.aspx)című témakört.

Az összes .vhd lemezen vagy képek létrehozása adatforrásként használni kívánt Azure-ban fájlok csak olvasható. A lemez vagy kép létrehozásakor Azure lemásolja a .vhd fájlokat. Ezek a másolatok lehet írásvédett vagy és-írási és olvasási, attól függően, hogy hogyan használhatja a virtuális.

Ha egy virtuális gép képből, Azure hoz létre, amely a forrás .vhd fájl egy példányát a virtuális gép lemezen. És vírusvédelmet véletlen törlését, Azure helyez el egy bérleti bármely .vhd forrásfájl, amely a képet, az operációs rendszer lemez vagy adatok lemezen létrehozására szolgál.

A forrásfájl .vhd törlése előtt kell a bérleti eltávolítása törli a lemez vagy a képet. Ha törölni szeretne egy .vhd fájlt, mint az operációs rendszer lemez egy virtuális gép által használt, törölheti a virtuális gép, az operációs rendszer lemezen, és a forrásfájl .vhd egyszerre virtuális gépen törlése, illetve törlésével, az összes kapcsolódó lemezt. Azonban több lépésből tárolt adatok lemezen forrás .vhd fájlok törlése beállítása sorrendben--leválasztása a lemezt a virtuális számítógépről, a lemez törlése, és törölje a .vhd fájlt.

>[AZURE.WARNING] Törölje a forrásfájl .vhd tárból, vagy törlése a tárterület-fiókját, akkor az a Microsoft nem állíthatók helyre az adatokat.


## <a name="troubleshooting"></a>Hibaelhárítás
[AZURE.INCLUDE [virtual-machines-linux-lunzero](../../includes/virtual-machines-linux-lunzero.md)]

## <a name="next-steps"></a>Következő lépések

-  [Lemezen csatolása](virtual-machines-linux-add-disk.md) további tárterület hozzáadásához a virtuális.
-  [Konfigurálása szoftveres RAID](virtual-machines-linux-configure-raid.md) redundancia.
-  A [Linux virtuális gép rögzítése](virtual-machines-linux-classic-capture-image.md) , így gyorsan alkalmazhat további VMs.


