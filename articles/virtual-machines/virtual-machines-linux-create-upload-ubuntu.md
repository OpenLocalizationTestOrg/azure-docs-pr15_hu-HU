<properties
    pageTitle="Hozzon létre, és töltse fel a egy Ubuntu Linux virtuális Azure-ban"
    description="Ismerje meg, hozhat létre, és töltse fel az Azure virtuális merevlemez (virtuális) Ubuntu Linux operációs rendszert tartalmazó."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="szarkos"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="szark"/>

# <a name="prepare-an-ubuntu-virtual-machine-for-azure"></a>Azure-Ubuntu virtuális gép előkészítése

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="official-ubuntu-cloud-images"></a>Hivatalos Ubuntu felhő képek
Ubuntu most teszi közzé a hivatalos Azure VHD [http://cloud-images.ubuntu.com/](http://cloud-images.ubuntu.com/)címen. Ha saját speciális Ubuntu kép készítése Azure kell inkább, mint az alábbi eljárással kézi ajánlott kezdje azzal, hogy ezek az ismert VHD dolgozik, és szükség szerint testre szabhatja. Kép Újdonságok mindig találhatók a következő helyeken:

 - Ubuntu 12.04/pontos: [ubuntu-12.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/precise/release/ubuntu-12.04-server-cloudimg-amd64-disk1.vhd.zip)
 - Ubuntu 14.04/Trusty: [ubuntu-14.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/trusty/release/ubuntu-14.04-server-cloudimg-amd64-disk1.vhd.zip)
 - Ubuntu 16.04/Xenial: [ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/xenial/release/ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip)


## <a name="prerequisites"></a>Előfeltételek

Ez a cikk tartalma feltételezi, hogy virtuális merevlemez-Ubuntu Linux operációs rendszer már telepítette. Több eszközök létezik .vhd fájlokat, például a Hyper-V például virtualizációs megoldást szeretne létrehozni. Című cikkben olvashat [a Hyper-V szerepkör és konfigurálása a virtuális gép telepítése](http://technet.microsoft.com/library/hh846766.aspx).

**Ubuntu telepítéssel kapcsolatos megjegyzések**

- Kérjük, lásd még: [Általános Linux telepítéssel kapcsolatos megjegyzések](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) Azure Linux Felkészülés a további tippeket.
- Az Azure, csak a **rögzített virtuális**VHDX formátuma nem támogatott.  A lemez átalakíthatja a Hyper-V kezelője vagy a konvertálás-virtuális parancsmag használatával virtuális formátum.
- A Linux rendszerben telepítésekor LVM (gyakran sok telepítés alapértelmezett), hanem szabvány partíciót használata ajánlott. LVM neve ütközik klónozott VMs, így elkerülhető, különösen akkor, ha-OS lemezre minden eddiginél kell egy másik virtuális csatolja az alábbiakkal. [LVM](virtual-machines-linux-configure-lvm.md) vagy [RAID](virtual-machines-linux-configure-raid.md) adatok lemezen használhatók meg, ha az előnyben részesített.
- A csere partíciót a OS lemezen nem állítja be. A Linux agent beállítható úgy, hogy az ideiglenes erőforrás lemezen felcserélése fájl létrehozása.  További tudnivalók találhatók az alábbi lépéseket.
- Az összes a VHD kell rendelkeznie, amelyek többszöröseire 1 MB méretű.


## <a name="manual-steps"></a>Manuális

> [AZURE.NOTE] Mielőtt hoz létre saját egyéni Ubuntu képe az Azure, fontolja meg az [http://cloud-images.ubuntu.com/](http://cloud-images.ubuntu.com/) képeinek használja.


1. A Hyper-V kezelője a középső ablaktáblában jelölje ki a virtuális gépen.

2. Kattintson a **Csatlakozás** a virtuális gép az ablak megnyitásához.

3.  Az aktuális tárházakban Azure repók Ubuntu meg használni a kép helyére. A lépések némileg Ubuntu függően változnak.

    Mielőtt szerkeszti /etc/apt/sources.list, ajánlott biztonsági másolatot készíteni:

        # sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak

    Ubuntu 12.04:

        # sudo sed -i "s/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g" /etc/apt/sources.list
        # sudo apt-get update

    Ubuntu 14.04:

        # sudo sed -i "s/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g" /etc/apt/sources.list
        # sudo apt-get update

4. A Ubuntu Azure képek Mostantól követi a *hardver bevezetés* (HWE) kernel. Az operációs rendszer frissítése a legújabb kernelhez a következő parancs futtatásával:

    Ubuntu 12.04:

        # sudo apt-get update
        # sudo apt-get install linux-image-generic-lts-trusty linux-cloud-tools-generic-lts-trusty
        # sudo apt-get install hv-kvp-daemon-init
        (recommended) sudo apt-get dist-upgrade

        # sudo reboot

    Ubuntu 14.04:

        # sudo apt-get update
        # sudo apt-get install linux-image-virtual-lts-vivid linux-lts-vivid-tools-common
        # sudo apt-get install hv-kvp-daemon-init
        (recommended) sudo apt-get dist-upgrade

        # sudo reboot


5. Módosítsa a lárvajárat, amelyet fel szeretne venni további kernel paraméterek Azure kernel indítási sort. Ehhez nyissa meg "/ stb/alapértelmezett/lárvajárat" szövegszerkesztőben, keresse meg a változó nevű `GRUB_CMDLINE_LINUX_DEFAULT` (vagy hozzáadása, ha szükséges), és módosítsa a következő paraméterek felvenni:

        GRUB_CMDLINE_LINUX_DEFAULT="console=tty1 console=ttyS0,115200n8 earlyprintk=ttyS0,115200 rootdelay=300"

    Mentse és zárja be a fájlt, majd futtatásával '`sudo update-grub`'. Ezzel biztosíthatja üzenetekhez konzol elküldi az első, segítheti a problémák hibakeresés Azure technikai támogatási soros portot.

6.  Győződjön meg arról, hogy a SSH-kiszolgáló telepítve és beállítva rendszerindításkor indításához.  Ez általában az alapértelmezett érték.

7.  Telepítse az Azure Linux ügynök:

        # sudo apt-get update
        # sudo apt-get install walinuxagent

    Figyelje meg, hogy telepítése a `walinuxagent` csomag eltávolítja a `NetworkManager` és `NetworkManager-gnome` csomagokat, ha telepítve van.

8.  A következő parancsokat a virtuális gép deprovision és Azure a kiépítési előkészítése:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

9. Kattintson a Hyper-V kezelője- **művelet-Leállítás > lefelé** . A Linux virtuális készen áll a Azure fel kell tölteni.


## <a name="next-steps"></a>Következő lépések
Most már készen áll új virtuális gépeken futó létrehozása az Azure Ubuntu Linux virtuális merevlemezen használatával. Ha az első alkalommal feltöltendő fel a .vhd fájl Azure, lépések 2 és 3 [létrehozása](virtual-machines-linux-classic-create-upload-vhd.md)és feltöltése a Linux operációs rendszert tartalmazó virtuális merevlemez.

## <a name="references"></a>Hivatkozások ##

Ubuntu hardver bevezetés (HWE) kernel:

- [http://blog.utlemming.org/2015/01/ubuntu-1404-Azure-Images-Now-Tracking.HTML](http://blog.utlemming.org/2015/01/ubuntu-1404-azure-images-now-tracking.html)
- [http://blog.utlemming.org/2015/02/1204-Azure-cloud-Images-Now-Using-hwe.HTML](http://blog.utlemming.org/2015/02/1204-azure-cloud-images-now-using-hwe.html)
