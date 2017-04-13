<properties 
    pageTitle="Állítsa be a LVM egy virtuális gépen futó Linux |} Microsoft Azure" 
    description="Megtudhatja, hogy miként LVM konfigurálása Linux Azure-ban." 
    services="virtual-machines-linux" 
    documentationCenter="na" 
    authors="szarkos"  
    manager="timlt" 
    editor="tysonn"
    tag="azure-service-management,azure-resource-manager" />

<tags 
    ms.service="virtual-machines-linux" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/24/2016" 
    ms.author="szark"/>


# <a name="configure-lvm-on-a-linux-vm-in-azure"></a>Azure-ban egy Linux virtuális LVM konfigurálása

A dokumentum bemutatja, hogyan lehet az Azure virtuális gép logikai mennyiségi Manager (LVM) beállításáról. Lehetséges LVM konfigurálása a virtuális géphez csatlakoztatott lemezen lép, alapértelmezés szerint a legtöbb felhő képek nem lesz az operációs rendszer lemezen konfigurált LVM. Az ismétlődő mennyiségi csoportok problémák megelőzése végett, ha az operációs rendszer lemez minden eddiginél csatlakozik egy másik virtuális terjesztési és a szöveg, azaz helyreállítási példa során. Ezért csak az adatok lemezt LVM használata ajánlott.


## <a name="linear-vs-striped-logical-volumes"></a>Lineáris csíkos logikai kötet összehasonlítása

Fizikai lemezen számos egyesítése egy egyetlen tárolási mennyiségi LVM használható. Alapértelmezés szerint LVM általában létrehoznia a lineáris logikai kötet, ami azt jelenti, hogy a tárolóhely közös tömb összefűzéséből. Ebben az esetben olvasási/írási műveletek általában csak küld egyetlen lemez. Viszont azt is létrehozhat csíkos logikai kötet, ahol olvasása és írása eloszlású a mennyiségi (azaz hasonlít RAID0) csoportban lévő több lemezre. Teljesítmény okok miatt valószínűleg kívánt paritásos a logikai kötet, így olvasása és írása csatlakozást az összes csatolt adatok lemez.

A dokumentum fog ismertetik, hogyan lehet több adat lemezt egyesítése egyetlen mennyiségi csoportot, és hozza létre az csíkos logikai kötet. Az alábbi lépésekkel is némileg általános dolgozhat a legtöbb terjesztését. A legtöbb esetben a segédprogramok és Azure a LVM kezelésére szolgáló munkafolyamatok nem alapvetően ugyanaz, mint a többi környezetben. Szokás szerint is forduljon a Linux szállítójával dokumentáció és ajánlott eljárások az adott eloszláshoz LVM használatához.


## <a name="attaching-data-disks"></a>Lemezen adatok csatolása
Egy fog általában szeretne kezdeni a két vagy több üres adatok lemezen LVM használata esetén. IO igényei szerint, megadhatja, amelyeket a szabványos tárolására, a legfeljebb 500 IO/ps egy lemezről vagy a prémium tárolási találatlista 5000 DB IO/ps egy lemezen a lemezt csatolhat. Ez a cikk nem fog mappába érkeznek részletes kiépítése és adatok lemezt csatolása Linux virtuális géphez. Című a Microsoft Azure cikk [lemezen csatolása](virtual-machines-linux-add-disk.md) létrehozásának lépéseit hogyan csatolhat egy üres adatok lemezt a Azure virtuális Linux géphez.

## <a name="install-the-lvm-utilities"></a>A LVM segédprogramok telepítése

- **Ubuntu**

        # sudo apt-get update
        # sudo apt-get install lvm2

- **RHEL, CentOS & Oracle Linux**

        # sudo yum install lvm2

- **SLES 12 és openSUSE**

        # sudo zypper install lvm2

- **SLES 11**

        # sudo zypper install lvm2

    A SLES11 kell is /etc/sysconfig/lvm szerkesztése és beállítása `LVM_ACTIVATED_ON_DISCOVERED` "engedélyezése":

        LVM_ACTIVATED_ON_DISCOVERED="enable" 


## <a name="configure-lvm"></a>LVM konfigurálása
Ebből az útmutatóból fog feltételezzük, három adatok lemezre, amelyek másként fogja hivatkozunk csatolta `/dev/sdc`, `/dev/sdd` és `/dev/sde`. Előfordulhat, hogy ezek nem mindig ugyanazt a virtuális elérési út nevek. Futtatását is lehetővé teszi "`sudo fdisk -l`" vagy a listában a rendelkezésre álló merevlemezeken hasonló parancsot.

1. A fizikai kötet előkészítése:

        # sudo pvcreate /dev/sd[cde]
          Physical volume "/dev/sdc" successfully created
          Physical volume "/dev/sdd" successfully created
          Physical volume "/dev/sde" successfully created


2.  Hozzon létre egy mennyiségi csoportot. Ebben a példában a mennyiségi csoport "adat-vg01" hívott azt:

        # sudo vgcreate data-vg01 /dev/sd[cde]
          Volume group "data-vg01" successfully created


3. Hozzon létre logikai jelenti. Alatt, hogy a parancs neve "adat-lv01" a teljes mennyiségi csoport időtartamát, de figyelje meg, hogy az is lehetséges hozhat létre több logikai kötet mennyiségi csoportjának egyetlen logikai kötet hoz létre.

        # sudo lvcreate --extents 100%FREE --stripes 3 --name data-lv01 data-vg01
          Logical volume "data-lv01" created.


4. A logikai kötet formázása

        # sudo mkfs -t ext4 /dev/data-vg01/data-lv01

  >[AZURE.NOTE] SLES11 használatával "-t ext3" ext4 helyett. SLES11 csak olvasási hozzáférést ext4 filesystems támogatja.


## <a name="add-the-new-file-system-to-etcfstab"></a>Az új fájlrendszer hozzáadása /etc/fstab

**Figyelmeztetés:** Helytelenül szerkesztése a /etc/fstab fájlt egy diagnosztizálását rendszer eredményezhet. Ha nem tudja, hogy, olvassa el a terjesztési dokumentációjában megfelelően szerkesztése: ezt a fájlt. Ajánlott is, hogy a biztonsági másolatot a /etc/fstab fájl szerkesztése előtt jön létre.

1. Létrehozhat például a kívánt csatlakozási pont, az új fájlrendszer:

        # sudo mkdir /data


2. Keresse meg a logikai kötet elérési útja

        # lvdisplay
        --- Logical volume ---
        LV Path                /dev/data-vg01/data-lv01
        ....


3. Nyissa meg a /etc/fstab szövegszerkesztőben, és vegyen fel egy új fájlrendszer, például:

        /dev/data-vg01/data-lv01  /data  ext4  defaults  0  2

    Ezután mentse, és zárja be a /etc/fstab.


4. Ellenőrizze, hogy helyesek-e a/stb/fstab bejegyzés:

        # sudo mount -a

    Ha ez a parancs eredményez hibaüzenet, ellenőrizze a fájlban és egyéb elemek/fstab az alábbi.

    Ezután futtassa a `mount` annak érdekében, hogy a fájlrendszer fel van szerelve parancsot:

        # mount
        ......
        /dev/mapper/data--vg01-data--lv01 on /data type ext4 (rw)


5. (Nem kötelező) /Etc/fstab FailSafe indítási paraméterek

    Sok terjesztését valamelyikét tartalmazza a `nobootwait` vagy `nofail` paramétereket, előfordulhat, hogy adható hozzá a/stb/fstab fájl csatlakoztatni. Ezek a paraméterek engedélyezése a hibák, ha egy adott fájlrendszer csatlakoztatása, és lehetővé teszi a Linux rendszer továbbra is az indításhoz, még akkor is, ha nem tudja a RAID fájlrendszer megfelelően csatlakoztatásához. További információt a következő paraméterek a terjesztési dokumentációjában találhat.

    Példa (Ubuntu):

        /dev/data-vg01/data-lv01  /data  ext4  defaults,nobootwait  0  2
