<properties
    pageTitle="Hozzon létre, és töltse fel a egy CentOS-alapú Linux virtuális Azure-ban"
    description="Ismerje meg, hozhat létre, és töltse fel az Azure virtuális merevlemez (virtuális), amely tartalmazza a Linux CentOS-alapú operációs rendszert."
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
    ms.date="05/09/2016"
    ms.author="szarkos"/>

# <a name="prepare-a-centos-based-virtual-machine-for-azure"></a>Azure virtuális CentOS-alapú gépek előkészítése

- [Azure virtuális gép CentOS 6.x előkészítése](#centos-6x)
- [Azure virtuális CentOS 7.0-s + gép előkészítése](#centos-70)

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a>Előfeltételek ##

Ez a cikk tartalma feltételezi, hogy már telepítette a CentOS (vagy hasonló származtatott) Linux operációs rendszer egy virtuális merevlemezre. Több eszközök létezik .vhd fájlokat, például a Hyper-V például virtualizációs megoldást szeretne létrehozni. Című cikkben olvashat [a Hyper-V szerepkör és konfigurálása a virtuális gép telepítése](http://technet.microsoft.com/library/hh846766.aspx).


**CentOS telepítéssel kapcsolatos megjegyzések**

- Kérjük, lásd még: [Általános Linux telepítéssel kapcsolatos megjegyzések](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) Azure Linux Felkészülés a további tippeket.

- Az Azure, csak a **rögzített virtuális**VHDX formátuma nem támogatott.  A lemez átalakíthatja a Hyper-V kezelője vagy a konvertálás-virtuális parancsmag használatával virtuális formátum.

- A Linux rendszerben telepítésekor LVM (gyakran sok telepítés alapértelmezett), hanem szabvány partíciót használata ajánlott. LVM neve ütközik klónozott VMs, így elkerülhető, különösen akkor, ha-OS lemezre minden eddiginél kell egy másik virtuális csatolja az alábbiakkal.  [LVM](virtual-machines-linux-configure-lvm.md) vagy [RAID](virtual-machines-linux-configure-raid.md) adatok lemezen használhatók meg, ha az előnyben részesített.

- NUMA nagyobb virtuális méretek Linux kernelverziókra 2.6.37 alatt egy hiba miatt nem támogatott. Ez a probléma elsősorban hatással van a felsőbb szintű használata terjesztését piros kalap 2.6.32 kernel. Manuális telepítés az Azure Linux ügynök (waagent) a Linux kernel LÁRVAJÁRAT konfigurációjában automatikusan NUMA tiltsa le. További tudnivalók találhatók az alábbi lépéseket.

- A csere partíciót a OS lemezen nem állítja be. A Linux agent beállítható úgy, hogy az ideiglenes erőforrás lemezen felcserélése fájl létrehozása.  További tudnivalók találhatók az alábbi lépéseket.

- Az összes a VHD kell rendelkeznie, amelyek többszöröseire 1 MB méretű.


## <a name="centos-6x"></a>CentOS 6.x ##

1. A Hyper-V-kezelőben válassza ki a virtuális gépen.

2. Kattintson a **Csatlakozás** a virtuális gép konzol az ablak megnyitásához.

3. Távolítsa el a NetworkManager a következő parancs futtatásával:

        # sudo rpm -e --nodeps NetworkManager

    **Megjegyzés:** A csomag már nem települ, ha ez a parancs a megjelenő hibaüzenet sikertelen lesz. Várható.

4.  Hozzon létre egy **hálózati** nevű fájlt a `/etc/sysconfig/` címtár, amely tartalmazza a következő szöveggel:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5.  Hozzon létre egy **ifcfg-eth0** a nevű fájlt a `/etc/sysconfig/network-scripts/` címtár, amely tartalmazza a következő szöveggel:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

6.  Udev szabályok létrehozása az Ethernet viteldíjjelző illesztőegységeihez statikus szabályairól elkerülése érdekében módosítsa. Szabályok problémákat okozhatnak, amikor a Hyper-V: vagy a Microsoft Azure virtuális gép klónozhatja

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules


7. Győződjön meg róla, a hálózati szolgáltatás indul rendszerindításkor, a következő parancs futtatásával:

        # sudo chkconfig network on


8. **Csak a 6.3 centOS**: az illesztőprogramjainak telepítését a Linux Integration Services (LIS).

    **Fontos: A lépés nem csak érvényes CentOS 6.3 és korábbi verziói.**  A CentOS 6.4 + az Linux integrációs szolgáltatások érhetők el *már a szabványos rendszerhéj*.

    - Kövesse a telepítési utasításokat [LIS töltse le a lapot](https://www.microsoft.com/en-us/download/details.aspx?id=46842) , és telepítse az alakzatot a kép RPM.  


9. Telepítse a python-pyasn1 csomag a következő parancs futtatásával:

        # sudo yum install python-pyasn1

10. Ha szeretné használni a OpenLogic tükrön, amelyekhez az Azure adatközpontokkal belül, majd cserélje ki a /etc/yum.repos.d/CentOS-Base.repo fájlt a következő tárházakban.  Ez a **[openlogic]** tárházba az Azure Linux ügynök csomagok tartalmazó is hozzáadja:

        [openlogic]
        name=CentOS-$releasever - openlogic packages for $basearch
        baseurl=http://olcentgbl.trafficmanager.net/openlogic/$releasever/openlogic/$basearch/
        enabled=1
        gpgcheck=0

        [base]
        name=CentOS-$releasever - Base
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/os/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

        #released updates
        [updates]
        name=CentOS-$releasever - Updates
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/updates/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

        #additional packages that may be useful
        [extras]
        name=CentOS-$releasever - Extras
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/extras/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

        #additional packages that extend functionality of existing packages
        [centosplus]
        name=CentOS-$releasever - Plus
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/centosplus/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

        #contrib - packages by Centos Users
        [contrib]
        name=CentOS-$releasever - Contrib
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/contrib/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

    **Megjegyzés:** Ez az útmutató a többi tegyük fel, legalább az [openlogic] repó, telepítse az Azure Linux ügynök az alábbi használt használ.


11. A következő sor hozzáadása /etc/yum.conf:

        http_caching=packages

    És **csak a CentOS 6.3** adja hozzá a következő parancsot:

        exclude=kernel*

12. Tiltsa le a yum modul "fastestmirror" a fájl szerkesztésével "/ etc/yum/pluginconf.d/fastestmirror.conf", és a `[main]`, írja be a következőt:

        set enabled=0

13. Futtassa az aktuális yum metaadatokat törölje a következő parancsot:

        # yum clean all

14. **A CentOS 6.3 csak**, frissítse a kernel, a következő parancsot:

        # sudo yum --disableexcludes=all install kernel

15. Módosítsa a lárvajárat konfigurációs, amelyet fel szeretne venni további kernel paraméterek Azure kernel indítási sorára. Ehhez nyissa meg a "/ boot/grub/menu.lst" szövegszerkesztőben, és győződjön meg arról, hogy az alapértelmezett kernel tartalmazza a következő paraméterek:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300 numa=off

    Ez biztosítja az első, segítheti Azure soros portot összes konzol üzenetek fogadására problémák hibakeresési támogatás. Ezzel letiltja NUMA a CentOS 6 által használt kernelverziót egy hiba miatt.

    A fenti kívül ajánlott *távolítsa el* a következő paraméterek:

        rhgb quiet crashkernel=auto

    A grafikus és csendes indítási sem lehet hasznos, ha azt szeretné, hogy a soros porthoz küldeni a naplókat felhőalapú környezetben.

    A `crashkernel` beállítás lehet balra beállítva, ha azt szeretné, de a megjegyzést, hogy a paraméter csökkenti a rendelkezésre álló memória található a virtuális 128MB vagy több, amely lehet a virtuális kisebb méretű a hibás.


16. Győződjön meg arról, hogy a SSH-kiszolgáló telepítve és beállítva rendszerindításkor indításához.  Ez általában az alapértelmezett érték.

17. Telepítse az Azure Linux ügynök a következő parancs futtatásával:

        # sudo yum install WALinuxAgent

    Ne feledje, hogy a WALinuxAgent csomag telepítésének eltávolítása a NetworkManager NetworkManager-gnome csomagok Ha az azok nem sikerült már eltávolítani leírtak szerint a lépés: 2.

18. Nem hozhatók létre felcserélése helyet a lemezen OS.

    Az Azure Linux ügynök automatikusan konfigurálhatja felcserélése hely a helyi erőforrás lemezzel, amelyhez a Azure kiépítése után a virtuális kapcsolódik. Ne feledje, hogy a helyi erőforrás lemez *ideiglenes* lemez, és előfordulhat, hogy közben kiürítette, amikor a virtuális leépítjük. Miután telepítettem az Azure Linux ügynök (lásd az előző lépést), a következő paraméterek /etc/waagent.conf megfelelően módosítása:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

19. A következő parancsokat a virtuális gép deprovision és Azure a kiépítési előkészítése:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

20. Kattintson a Hyper-V kezelője- **művelet-Leállítás > lefelé** . A Linux virtuális készen áll a Azure fel kell tölteni.


----------


## <a name="centos-70"></a>CentOS 7.0-s + ##

**Módosítások CentOS 7 (és hasonló származtatott)**

Azure virtuális CentOS 7 gép előkészítése nagyon hasonlít, de néhány fontos különbségek vannak megjegyezni, CentOS 6:

 - A NetworkManager csomagot már nem ütközik az Azure Linux ügynök. A csomag alapértelmezés szerint telepítve van, és azt javasoljuk, hogy nem törlődik.
 - GRUB2 most szolgál az alapértelmezett rendszertöltőbe, hogy az eljárás szerkesztése paramétereinek kernel megváltozott (lásd alább).
 - XFS ettől kezdve az alapértelmezett fájlrendszer. A ext4 fájlrendszer továbbra is használható, ha szükségesnek látja.


**Konfigurálási lépéseket**

1. A Hyper-V-kezelőben válassza ki a virtuális gépen.

2. Kattintson a **Csatlakozás** a virtuális gép konzol az ablak megnyitásához.

3.  Hozzon létre egy **hálózati** nevű fájlt a `/etc/sysconfig/` címtár, amely tartalmazza a következő szöveggel:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

4.  Hozzon létre egy **ifcfg-eth0** a nevű fájlt a `/etc/sysconfig/network-scripts/` címtár, amely tartalmazza a következő szöveggel:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

6.  Udev szabályok létrehozása az Ethernet viteldíjjelző illesztőegységeihez statikus szabályairól elkerülése érdekében módosítsa. Szabályok problémákat okozhatnak, amikor a Hyper-V: vagy a Microsoft Azure virtuális gép klónozhatja

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules

6. Győződjön meg róla, a hálózati szolgáltatás indul rendszerindításkor, a következő parancs futtatásával:

        # sudo chkconfig network on

7. Telepítse a python-pyasn1 csomag a következő parancs futtatásával:

        # sudo yum install python-pyasn1

8. Ha szeretné használni a OpenLogic tükrön, amelyekhez az Azure adatközpontokkal belül, majd cserélje ki a /etc/yum.repos.d/CentOS-Base.repo fájlt a következő tárházakban.  Ez a **[openlogic]** tárházba az Azure Linux ügynök csomagok tartalmazó is hozzáadja:

        [openlogic]
        name=CentOS-$releasever - openlogic packages for $basearch
        baseurl=http://olcentgbl.trafficmanager.net/openlogic/$releasever/openlogic/$basearch/
        enabled=1
        gpgcheck=0

        [base]
        name=CentOS-$releasever - Base
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/os/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

        #released updates
        [updates]
        name=CentOS-$releasever - Updates
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/updates/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

        #additional packages that may be useful
        [extras]
        name=CentOS-$releasever - Extras
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/extras/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

        #additional packages that extend functionality of existing packages
        [centosplus]
        name=CentOS-$releasever - Plus
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/centosplus/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

        #contrib - packages by Centos Users
        [contrib]
        name=CentOS-$releasever - Contrib
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/contrib/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7



    **Megjegyzés:** Ez az útmutató a többi tegyük fel, legalább az [openlogic] repó, telepítse az Azure Linux ügynök az alábbi használt használja.

9.  Törölje a jelet a jelenlegi yum metaadatokat, és telepítse a frissítéseket a következő parancsot futtatva:

        # sudo yum clean all
        # sudo yum -y update

10. Módosítsa a lárvajárat konfigurációs, amelyet fel szeretne venni további kernel paraméterek Azure kernel indítási sorára. Ehhez a megnyitott "/ stb/alapértelmezett/lárvajárat" szövegszerkesztőben és szerkesztése a `GRUB_CMDLINE_LINUX` paraméter, például:

        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"

    Ez biztosítja az első, segítheti Azure soros portot összes konzol üzenetek fogadására problémák hibakeresési támogatás. Azt is, azzal kikapcsolja azokat a új CentOS 7 elnevezési szabályai NIC számára. A fenti kívül ajánlott *távolítsa el* a következő paraméterek:

        rhgb quiet crashkernel=auto

    A grafikus és csendes indítási sem lehet hasznos, ha azt szeretné, hogy a soros porthoz küldeni a naplókat felhőalapú környezetben.

    A `crashkernel` beállítás lehet balra beállítva, ha azt szeretné, de a megjegyzést, hogy a paraméter csökkenti a rendelkezésre álló memória található a virtuális 128MB vagy több, amely lehet a virtuális kisebb méretű a hibás.

11. Miután befejezte az képszerkesztési "/ stb/alapértelmezett/lárvajárat" / fent, a következő parancsot a lárvajárat konfiguráció újraépítéséhez:

        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

12. Győződjön meg arról, hogy a SSH-kiszolgáló telepítve és beállítva rendszerindításkor indításához.  Ez általában az alapértelmezett érték.

13. **Csak akkor, ha a kép épület VMWare, VirtualBox vagy KVM:** Adja hozzá a Hyper-V modulok initramfs be:

    Szerkesztés `/etc/dracut.conf`, tartalom hozzáadása:

        add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”

    A initramfs újraépítéséhez:

        # dracut –f -v

14. Telepítse az Azure Linux ügynök a következő parancs futtatásával:

        # sudo yum install WALinuxAgent
        # sudo systemctl enable waagent

15. Nem hozhatók létre felcserélése helyet a lemezen OS.

    Az Azure Linux ügynök automatikusan konfigurálhatja felcserélése hely a helyi erőforrás lemezzel, amelyhez a Azure kiépítése után a virtuális kapcsolódik. Ne feledje, hogy a helyi erőforrás lemez *ideiglenes* lemez, és előfordulhat, hogy közben kiürítette, amikor a virtuális leépítjük. Miután telepítettem az Azure Linux ügynök (lásd az előző lépést), a következő paraméterek /etc/waagent.conf megfelelően módosítása:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

16. A következő parancsokat a virtuális gép deprovision és Azure a kiépítési előkészítése:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

17. Kattintson a Hyper-V kezelője- **művelet-Leállítás > lefelé** . A Linux virtuális készen áll a Azure fel kell tölteni.

## <a name="next-steps"></a>Következő lépések
Most már készen áll új virtuális gépeken futó létrehozása az Azure CentOS Linux virtuális merevlemezen használatával. Ha az első alkalommal feltöltendő fel a .vhd fájl Azure, lépések 2 és 3 [létrehozása](virtual-machines-linux-classic-create-upload-vhd.md)és feltöltése a Linux operációs rendszert tartalmazó virtuális merevlemez.
