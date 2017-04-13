<properties
    pageTitle="Felkészülés a Debian Linux virtuális |} Microsoft Azure"
    description="Megtudhatja, hogyan hozhat létre Debian 7 és 8 virtuális fájlok telepítéshez Azure-ban."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="szarkos"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="szark"/>



# <a name="prepare-a-debian-vhd-for-azure"></a>Azure virtuális Debian merevlemez előkészítése

## <a name="prerequisites"></a>Előfeltételek
Ez a szakasz tartalma feltételezi, hogy már telepítette a Debian Linux operációs rendszert virtuális merevlemez [Debian webhely](https://www.debian.org/distrib/) letöltött fájlból. Több eszközök léteznek .vhd fájlok; létrehozása A Hyper-V csak egy példa. A Hyper-V használata című cikkben olvashat [telepítheti a Hyper-V szerepkört, és a virtuális gép konfigurálása](https://technet.microsoft.com/library/hh846766.aspx).


## <a name="installation-notes"></a>A telepítéssel kapcsolatos megjegyzések

- Kérjük, lásd még: [Általános Linux telepítéssel kapcsolatos megjegyzések](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) Azure Linux Felkészülés a további tippeket.
- Az új VHDX formátumot Azure-ban nem támogatott. A lemez átalakíthatja a Hyper-V kezelője vagy a **Konvertálás-virtuális** parancsmag használatával virtuális formátum.
- A Linux rendszerben telepítésekor LVM (gyakran sok telepítés alapértelmezett), hanem szabvány partíciót használata ajánlott. LVM neve ütközik klónozott VMs, így elkerülhető, különösen akkor, ha-OS lemezre minden eddiginél kell egy másik virtuális csatolja az alábbiakkal. [LVM](virtual-machines-linux-configure-lvm.md) vagy [RAID](virtual-machines-linux-configure-raid.md) adatok lemezen használhatók meg, ha az előnyben részesített.
- A csere partíciót a OS lemezen nem állítja be. Az Azure Linux ügynök beállítható úgy, hogy az ideiglenes erőforrás lemezen felcserélése fájl létrehozása. További tudnivalók találhatók az alábbi lépéseket.
- Az összes a VHD kell rendelkeznie, amelyek többszöröseire 1 MB méretű.


## <a name="use-azure-manage-to-create-debian-vhds"></a>Debian VHD létrehozása az Azure kezelése segítségével

Eszközök is elérhetők az Azure-Debian VHD előállításához az [azure-kezelése](https://github.com/credativ/azure-manage) [credativ](http://www.credativ.com/)parancsfájlok. Ez a kép létrehozása a nulláról és a javasolt megközelítés. Ha például hozhat létre egy Debian 8 virtuális letöltése a következő parancsokat azure-kezelése (és függőségek), és futtassa a azure_build_image:

    # sudo apt-get update
    # sudo apt-get install git qemu-utils mbr kpartx debootstrap

    # sudo apt-get install python3-pip python3-dateutil python3-cryptography
    # sudo pip3 install azure-storage azure-servicemanagement-legacy azure-common pytest pyyaml
    # git clone https://github.com/credativ/azure-manage.git
    # cd azure-manage
    # sudo pip3 install .

    # sudo azure_build_image --option release=jessie --option image_size_gb=30 --option image_prefix=debian-jessie-azure section


## <a name="manually-prepare-a-debian-vhd"></a>Egy Debian virtuális manuálisan előkészítése

1. A Hyper-V-kezelőben válassza ki a virtuális gépen.

2. Kattintson a **Csatlakozás** a virtuális gép konzol az ablak megnyitásához.

3. Megjegyzés a **deb CD-ROM** a sort, `/etc/apt/source.list` Ha úgy állítja be a virtuális az ISO-fájl szemben.

4. Szerkesztés a `/etc/default/grub` fájl, és módosítsa a **GRUB_CMDLINE_LINUX** paramétert a következőképpen amelyet fel szeretne venni további kernel paraméterek Azure.

        GRUB_CMDLINE_LINUX="console=tty0 console=ttyS0,115200 earlyprintk=ttyS0,115200 rootdelay=30"

5. A lárvajárat újbóli létrehozása és futtatása:

        # sudo update-grub

6. /Etc/apt/sources.list Debian 7 vagy 8 Debian's Azure tárházakban hozzáadása:

    **Debian 7.x "Wheezy"**

        deb http://debian-archive.trafficmanager.net/debian wheezy-backports main
        deb-src http://debian-archive.trafficmanager.net/debian wheezy-backports main
        deb http://debian-archive.trafficmanager.net/debian-azure wheezy main
        deb-src http://debian-archive.trafficmanager.net/debian-azure wheezy main


    **Debian 8.x "Jessie"**

        deb http://debian-archive.trafficmanager.net/debian jessie-backports main
        deb-src http://debian-archive.trafficmanager.net/debian jessie-backports main
        deb http://debian-archive.trafficmanager.net/debian-azure jessie main
        deb-src http://debian-archive.trafficmanager.net/debian-azure jessie main


7. Telepítse az Azure Linux ügynök:

        # sudo apt-get update
        # sudo apt-get install waagent

8. Debian 7 nem szükséges a 3.16-alapú kernel lebonyolítása a wheezy backports tárat. Először létre kell hoznia egy /etc/apt/preferences.d/linux.pref a következő tartalommal nevű fájlt:

        Package: linux-image-amd64 initramfs-tools
        Pin: release n=wheezy-backports
        Pin-Priority: 500

    Az új kernelt telepítéséhez majd futtassa "sudo apt get-telepítés linux-kép-amd64"

8. A virtuális gép deprovision és Azure a kiépítési előkészítése és futtatása:

        # sudo waagent –force -deprovision
        # export HISTSIZE=0
        # logout

9. Kattintson a **művelet** a Hyper-V kezelője lefelé-Leállítás >. A Linux virtuális készen áll a Azure fel kell tölteni.


## <a name="next-steps"></a>Következő lépések

Most már készen áll új virtuális gépeken futó létrehozása az Azure virtuális Debian merevlemezen használatával. Ha az első alkalommal feltöltendő fel a .vhd fájl Azure, lépések 2 és 3 [létrehozása](virtual-machines-linux-classic-create-upload-vhd.md)és feltöltése a Linux operációs rendszert tartalmazó virtuális merevlemez.
