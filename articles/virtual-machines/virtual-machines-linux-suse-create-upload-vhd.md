<properties
    pageTitle="Hozzon létre, és töltse fel a egy SUSE Linux virtuális Azure-ban"
    description="Ismerje meg, hozhat létre, és töltse fel az Azure virtuális merevlemez (virtuális), amely tartalmazza a SUSE Linux operációs rendszert."
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

# <a name="prepare-a-sles-or-opensuse-virtual-machine-for-azure"></a>Azure virtuális gép SLES vagy openSUSE előkészítése

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a>Előfeltételek ##

Ez a cikk tartalma feltételezi, hogy már telepítette a SUSE vagy Linux operációs rendszer openSUSE egy virtuális merevlemezre. Több eszközök létezik .vhd fájlokat, például a Hyper-V például virtualizációs megoldást szeretne létrehozni. Című cikkben olvashat [a Hyper-V szerepkör és konfigurálása a virtuális gép telepítése](http://technet.microsoft.com/library/hh846766.aspx).

### <a name="sles--opensuse-installation-notes"></a>SLES / openSUSE telepítéssel kapcsolatos megjegyzések

- Kérjük, lásd még: [Általános Linux telepítéssel kapcsolatos megjegyzések](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) Azure Linux Felkészülés a további tippeket.

- Az Azure, csak a **rögzített virtuális**VHDX formátuma nem támogatott.  A lemez átalakíthatja a Hyper-V kezelője vagy a konvertálás-virtuális parancsmag használatával virtuális formátum.

- A Linux rendszerben telepítésekor LVM (gyakran sok telepítés alapértelmezett), hanem szabvány partíciót használata ajánlott. LVM neve ütközik klónozott VMs, így elkerülhető, különösen akkor, ha-OS lemezre minden eddiginél kell egy másik virtuális csatolja az alábbiakkal. [LVM](virtual-machines-linux-configure-lvm.md) vagy [RAID](virtual-machines-linux-configure-raid.md) adatok lemezen használhatók meg, ha az előnyben részesített.

- A csere partíciót a OS lemezen nem állítja be. A Linux agent beállítható úgy, hogy az ideiglenes erőforrás lemezen felcserélése fájl létrehozása.  További tudnivalók találhatók az alábbi lépéseket.

- Az összes a VHD kell rendelkeznie, amelyek többszöröseire 1 MB méretű.


## <a name="use-suse-studio"></a>Használja a SUSE Studio
[SUSE Studio](http://www.susestudio.com) egyszerűen létrehozhat és kezelheti a SLES és openSUSE képek Azure és a Hyper-V. Ez a javasolt megközelítés saját SLES és openSUSE képek testreszabásához.

Alternatívájaként saját virtuális létrehozásába SUSE is teszi közzé a BYOS (hozása: A saját előfizetési) képek esetében a [VMDepot](https://vmdepot.msopentech.com/User/Show?user=1007)SLES.


## <a name="prepare-suse-linux-enterprise-server-11-sp4"></a>SUSE Linux vállalati kiszolgáló 11 SP4 előkészítése ##

1. A Hyper-V kezelője a középső ablaktáblában jelölje ki a virtuális gépen.

2. Kattintson a **Csatlakozás** a virtuális gép az ablak megnyitásához.

3. Regisztráljon az SUSE Linux vállalati rendszer teszik lehetővé a frissítések letöltéséhez és telepítéséhez a csomagok.

4. Frissítse a rendszer a legújabb javítások:

        # sudo zypper update

5. Telepítse az Azure Linux ügynök a SLES tárat a:

        # sudo zypper install WALinuxAgent

6. Ha waagent értéke "a" beadása chkconfig, és ha nem, kapcsolhatja be az automatikus indítása:
               
        # sudo chkconfig waagent on

7. Annak ellenőrzése, ha waagent szolgáltatás fut, és ha nem, indítsa el: 

        # sudo service waagent start
                
8. Módosítsa a lárvajárat konfigurációs, amelyet fel szeretne venni további kernel paraméterek Azure kernel indítási sorára. A Megnyitás elvégzendő "/ boot/grub/menu.lst" szövegszerkesztőben, és győződjön meg arról, hogy az alapértelmezett kernel tartalmazza a következő paraméterek:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300

    Ezzel biztosíthatja az első, segítheti Azure soros portot összes konzol üzenetek fogadására problémák hibakeresési támogatás.

9. Győződjön meg arról, hogy /boot/grub/menu.lst /etc/fstab mindkét hivatkozási és a lemezt a UUID (által-uuid) használata a merevlemez-azonosító (által-azonosító) helyett. 

    UUID cikkre
    
        # ls /dev/disk/by-uuid/

    Ha /dev/disk/by-id / van /boot/grub/menu.lst és is/stb/fstab használt, frissítse a megfelelő által-uuid érték

    Módosítás előtt
    
        root=/dev/disk/by-id/SCSI-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxx-part1

    Módosítás után
    
        root=/dev/disk/by-uuid/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

10. Udev szabályok létrehozása az Ethernet viteldíjjelző illesztőegységeihez statikus szabályairól elkerülése érdekében módosítsa. Szabályok problémákat okozhatnak, amikor a Hyper-V: vagy a Microsoft Azure virtuális gép klónozhatja

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules

11. A fájl szerkesztéséhez ajánlott "/ dhcp stb/sysconfig/hálózati", és módosítsa a `DHCLIENT_SET_HOSTNAME` paraméter nélkül a következő:

        DHCLIENT_SET_HOSTNAME="no"

12. A "/ stb/sudoers" megjegyzést fűzni, vagy távolítsa el a következő sorokat, ha vannak ilyenek:

        Defaults targetpw   # ask for the password of the target user i.e. root
        ALL    ALL=(ALL) ALL   # WARNING! Only use this together with 'Defaults targetpw'!

13. Győződjön meg arról, hogy a SSH-kiszolgáló telepítve és beállítva rendszerindításkor indításához.  Ez általában az alapértelmezett érték.

14. Nem hozhatók létre felcserélése helyet a lemezen OS.

    Az Azure Linux ügynök automatikusan konfigurálhatja felcserélése hely a helyi erőforrás lemezzel, amelyhez a Azure kiépítése után a virtuális kapcsolódik. Ne feledje, hogy a helyi erőforrás lemez *ideiglenes* lemez, és előfordulhat, hogy közben kiürítette, amikor a virtuális leépítjük. Miután telepítettem az Azure Linux ügynök (lásd az előző lépést), a következő paraméterek /etc/waagent.conf megfelelően módosítása:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

15. A következő parancsokat a virtuális gép deprovision és Azure a kiépítési előkészítése:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

16. Kattintson a Hyper-V kezelője- **művelet-Leállítás > lefelé** . A Linux virtuális készen áll a Azure fel kell tölteni.


----------

## <a name="prepare-opensuse-131"></a>OpenSUSE 13.1 + előkészítése ##

1. A Hyper-V kezelője a középső ablaktáblában jelölje ki a virtuális gépen.

2. Kattintson a **Csatlakozás** a virtuális gép az ablak megnyitásához.

3. A felületén a parancsot "`zypper lr`". Ha ez a parancs függvény eredménye az alábbihoz hasonló, akkor a tárházakban meg a várt módon – nincs módosítás szükség beállítva (figyelje meg, hogy verziószámai változhat):

        # | Alias                 | Name                  | Enabled | Refresh
        --+-----------------------+-----------------------+---------+--------
        1 | Cloud:Tools_13.1      | Cloud:Tools_13.1      | Yes     | Yes
        2 | openSUSE_13.1_OSS     | openSUSE_13.1_OSS     | Yes     | Yes
        3 | openSUSE_13.1_Updates | openSUSE_13.1_Updates | Yes     | Yes

    Ha a parancs eredménye "Nincs tárházakban definiált..." majd az alábbi parancsokkal ezek repók hozzáadása:

        # sudo zypper ar -f http://download.opensuse.org/repositories/Cloud:Tools/openSUSE_13.1 Cloud:Tools_13.1
        # sudo zypper ar -f http://download.opensuse.org/distribution/13.1/repo/oss openSUSE_13.1_OSS
        # sudo zypper ar -f http://download.opensuse.org/update/13.1 openSUSE_13.1_Updates

    Majd ellenőrizze, hogy a tárházakban van hozzáadva a parancs futtatásával "`zypper lr`" újra. Abban az esetben, ha a megfelelő frissítés tárházakban található nincs engedélyezve, engedélyezze a következő parancsot:

        # sudo zypper mr -e [NUMBER OF REPOSITORY]


4. A legújabb verzióra frissíteni az a kernel:

        # sudo zypper up kernel-default

    Vagy frissítse a rendszer a legújabb javítások:

        # sudo zypper update

5.  Telepítse az Azure Linux ügynök.

        # sudo zypper install WALinuxAgent

6.  Módosítsa a lárvajárat konfigurációs, amelyet fel szeretne venni további kernel paraméterek Azure kernel indítási sorára. Ehhez nyissa meg a "/ boot/grub/menu.lst" szövegszerkesztőben, és győződjön meg arról, hogy az alapértelmezett kernel tartalmazza a következő paraméterek:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300

    Ezzel biztosíthatja az első, segítheti Azure soros portot összes konzol üzenetek fogadására problémák hibakeresési támogatás. A következő paraméterek ezenkívül eltávolítása a kernel indítási sor, ha vannak ilyenek:

        libata.atapi_enabled=0 reserve=0x1f0,0x8

7.  A fájl szerkesztéséhez ajánlott "/ dhcp stb/sysconfig/hálózati", és módosítsa a `DHCLIENT_SET_HOSTNAME` paraméter nélkül a következő:

        DHCLIENT_SET_HOSTNAME="no"

8.  **Fontos:** A "/ stb/sudoers" megjegyzést fűzni, vagy távolítsa el a következő sorokat, ha vannak ilyenek:

        Defaults targetpw   # ask for the password of the target user i.e. root
        ALL    ALL=(ALL) ALL   # WARNING! Only use this together with 'Defaults targetpw'!

9.  Győződjön meg arról, hogy a SSH-kiszolgáló telepítve és beállítva rendszerindításkor indításához.  Ez általában az alapértelmezett érték.

10. Nem hozhatók létre felcserélése helyet a lemezen OS.

    Az Azure Linux ügynök automatikusan konfigurálhatja felcserélése hely a helyi erőforrás lemezzel, amelyhez a Azure kiépítése után a virtuális kapcsolódik. Ne feledje, hogy a helyi erőforrás lemez *ideiglenes* lemez, és előfordulhat, hogy közben kiürítette, amikor a virtuális leépítjük. Miután telepítettem az Azure Linux ügynök (lásd az előző lépést), a következő paraméterek /etc/waagent.conf megfelelően módosítása:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

11. A következő parancsokat a virtuális gép deprovision és Azure a kiépítési előkészítése:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

12. Győződjön meg róla, az Azure Linux ügynök lefut az indításkor:

        # sudo systemctl enable waagent.service

13. Kattintson a Hyper-V kezelője- **művelet-Leállítás > lefelé** . A Linux virtuális készen áll a Azure fel kell tölteni.

## <a name="next-steps"></a>Következő lépések
Most már készen áll új virtuális gépeken futó létrehozása az Azure SUSE Linux virtuális merevlemezen használatával. Ha az első alkalommal feltöltendő fel a .vhd fájl Azure, lépések 2 és 3 [létrehozása](virtual-machines-linux-classic-create-upload-vhd.md)és feltöltése a Linux operációs rendszert tartalmazó virtuális merevlemez.
