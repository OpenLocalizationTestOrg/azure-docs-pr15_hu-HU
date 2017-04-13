<properties
    pageTitle="Az Oracle Linux virtuális tölteni |} Microsoft Azure"
    description="Ismerje meg, hozhat létre, és töltse fel az Azure virtuális merevlemez (virtuális), amely tartalmazza az Oracle Linux operációs rendszer."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="szarkos"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management,azure-resource-manager" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="szark"/>

# <a name="prepare-an-oracle-linux-virtual-machine-for-azure"></a>Azure-Oracle Linux virtuális gép előkészítése

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a>Előfeltételek ##

Ez a cikk tartalma feltételezi, hogy már telepítette az Oracle Linux operációs rendszer egy virtuális merevlemezre. Több eszközök létezik .vhd fájlokat, például a Hyper-V például virtualizációs megoldást szeretne létrehozni. Című cikkben olvashat [a Hyper-V szerepkör és konfigurálása a virtuális gép telepítése](http://technet.microsoft.com/library/hh846766.aspx).


### <a name="oracle-linux-installation-notes"></a>Az Oracle Linux telepítéssel kapcsolatos megjegyzések

- Kérjük, lásd még: [Általános Linux telepítéssel kapcsolatos megjegyzések](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) Azure Linux Felkészülés a további tippeket.

- Oracle piros kalap kompatibilis kernel és azok UEK3 (szoros vállalati Kernel) is használhatók a Hyper-V és Azure. A legjobb mindenképp frissítheti a legújabb kernelhez előkészítése az Oracle Linux virtuális.

- Oracle UEK2 nem támogatott a Hyper-V és Azure, nem tartalmazza a szükséges illesztőprogramok.

- Az Azure, csak a **rögzített virtuális**VHDX formátuma nem támogatott.  A lemez átalakíthatja a Hyper-V kezelője vagy a konvertálás-virtuális parancsmag használatával virtuális formátum.

- A Linux rendszerben telepítésekor LVM (gyakran sok telepítés alapértelmezett), hanem szabvány partíciót használata ajánlott. LVM neve ütközik klónozott VMs, így elkerülhető, különösen akkor, ha-OS lemez minden eddiginél kell egy másik virtuális csatolja az alábbiakkal. [LVM](virtual-machines-linux-configure-lvm.md) vagy [RAID](virtual-machines-linux-configure-raid.md) adatok lemezen használhatók meg, ha az előnyben részesített.

- NUMA nagyobb virtuális méretek Linux kernelverziókra 2.6.37 alatt egy hiba miatt nem támogatott. Ez a probléma elsősorban hatással van a felsőbb szintű használata terjesztését piros kalap 2.6.32 kernel. Manuális telepítés az Azure Linux ügynök (waagent) a Linux kernel LÁRVAJÁRAT konfigurációjában automatikusan NUMA tiltsa le. További tudnivalók találhatók az alábbi lépéseket.

- A csere partíciót a OS lemezen nem állítja be. A Linux agent beállítható úgy, hogy az ideiglenes erőforrás lemezen felcserélése fájl létrehozása.  További tudnivalók találhatók az alábbi lépéseket.

- Az összes a VHD kell rendelkeznie a többszörös 1 MB-os oldalméretei.

- Győződjön meg arról, hogy a `Addons` tárházba engedélyezve van. Szerkessze a fájlt `/etc/yum.repo.d/public-yum-ol6.repo`(Oracle Linux 6) vagy `/etc/yum.repo.d/public-yum-ol7.repo`(Oracle Linux), és módosítsa a sor `enabled=0` való `enabled=1` **[ol6_addons]** és **[ol7_addons]** a fájlban.


## <a name="oracle-linux-64"></a>Az Oracle Linux 6.4 + ##

A virtuális gép futtatásához Azure-ban az operációs rendszer adott konfigurációs lépéseket kell elvégeznie.

1. A Hyper-V kezelője a középső ablaktáblában jelölje ki a virtuális gépen.

2. Kattintson a **Csatlakozás** a virtuális gép az ablak megnyitásához.

3. Távolítsa el a NetworkManager a következő parancs futtatásával:

        # sudo rpm -e --nodeps NetworkManager

    **Megjegyzés:** A csomag már nem települ, ha ez a parancs a megjelenő hibaüzenet sikertelen lesz. Várható.

4.  Hozzon létre egy **hálózati** nevű fájlt a `/etc/sysconfig/` címtár, amely tartalmazza a következő szöveggel:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5.  Hozzon létre egy **ifcfg-eth0** a nevű fájlt a `/etc/sysconfig/network-scripts/` könyvtárat, amely tartalmazza a következő szöveggel:

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

        # chkconfig network on

8. Telepítse a python-pyasn1 a következő parancs futtatásával:

        # sudo yum install python-pyasn1

9.  Módosítsa a lárvajárat konfigurációs, amelyet fel szeretne venni további kernel paraméterek Azure kernel indítási sorára. A Megnyitás elvégzendő "/ boot/grub/menu.lst" szövegszerkesztőben, és győződjön meg arról, hogy az alapértelmezett kernel tartalmazza a következő paraméterek:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300 numa=off

    Ez biztosítja az első, segítheti Azure soros portot összes konzol üzenetek fogadására problémák hibakeresési támogatás. Ezzel letiltja NUMA Oracle piros kalap kompatibilis rendszerhéj egy hiba miatt.

    A fenti kívül ajánlott *távolítsa el* a következő paraméterek:

        rhgb quiet crashkernel=auto

    A grafikus és csendes indítási sem lehet hasznos, ha azt szeretné, hogy a soros porthoz küldeni a naplókat felhőalapú környezetben.

    A `crashkernel` beállítás lehet balra beállítva, ha azt szeretné, de a megjegyzést, hogy a paraméter csökkenti a rendelkezésre álló memória található a virtuális 128MB vagy több, amely lehet a virtuális kisebb méretű a hibás.


10. Győződjön meg arról, hogy a SSH-kiszolgáló telepítve és beállítva rendszerindításkor indításához.  Ez általában az alapértelmezett érték.

11. Telepítse az Azure Linux ügynök a következő parancs futtatásával. A legújabb verziója 2.0.15.

        # sudo yum install WALinuxAgent

    Ne feledje, hogy a WALinuxAgent csomag telepítésének eltávolítása a NetworkManager NetworkManager-gnome csomagok Ha az azok nem sikerült már eltávolítani leírtak szerint a lépés: 2.

12. Nem hozhatók létre felcserélése helyet a lemezen OS.

    Az Azure Linux ügynök automatikusan konfigurálhatja felcserélése hely a helyi erőforrás lemezzel, amelyhez a Azure kiépítése után a virtuális kapcsolódik. Ne feledje, hogy a helyi erőforrás lemez *ideiglenes* lemez, és előfordulhat, hogy közben kiürítette, amikor a virtuális leépítjük. Miután telepítettem az Azure Linux ügynök (lásd az előző lépést), a következő paraméterek /etc/waagent.conf megfelelően módosítása:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

13. A következő parancsokat a virtuális gép deprovision és Azure a kiépítési előkészítése:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

14. Kattintson a Hyper-V kezelője- **művelet-Leállítás > lefelé** . A Linux virtuális készen áll a Azure fel kell tölteni.


----------


## <a name="oracle-linux-70"></a>Az Oracle Linux 7.0-s + ##

**Az Oracle Linux 7 változásai**

Az Oracle Linux 7 virtuális gép előkészítése Azure az Oracle Linux 6, de néhány fontos különbségek vannak megjegyezni hasonló:

 - A piros kalap kompatibilis kernel és a Oracle UEK3 Azure-ban támogatott.  A UEK3 kernel ajánlott.
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

5.  Udev szabályok létrehozása az Ethernet viteldíjjelző illesztőegységeihez statikus szabályairól elkerülése érdekében módosítsa. Szabályok problémákat okozhatnak, amikor a Hyper-V: vagy a Microsoft Azure virtuális gép klónozhatja

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules

6. Győződjön meg róla, a hálózati szolgáltatás indul rendszerindításkor, a következő parancs futtatásával:

        # sudo chkconfig network on

7. Telepítse a python-pyasn1 csomag a következő parancs futtatásával:

        # sudo yum install python-pyasn1

8.  Törölje a jelet a jelenlegi yum metaadatokat, és telepítse a frissítéseket a következő parancsot futtatva:

        # sudo yum clean all
        # sudo yum -y update

9.  Módosítsa a lárvajárat konfigurációs, amelyet fel szeretne venni további kernel paraméterek Azure kernel indítási sorára. Ehhez nyissa meg "/ stb/alapértelmezett/lárvajárat" szövegszerkesztőben és Szerkesztés a `GRUB_CMDLINE_LINUX` paraméter, például:

        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"

    Ez biztosítja az első, segítheti Azure soros portot összes konzol üzenetek fogadására problémák hibakeresési támogatás. Azt is, azzal kikapcsolja azokat a új OEL 7 elnevezési szabályai NIC számára. A fenti kívül ajánlott *távolítsa el* a következő paraméterek:

        rhgb quiet crashkernel=auto

    A grafikus és csendes indítási sem lehet hasznos, ha azt szeretné, hogy a soros porthoz küldeni a naplókat felhőalapú környezetben.

    A `crashkernel` beállítás lehet balra beállítva, ha azt szeretné, de a megjegyzést, hogy a paraméter csökkenti a rendelkezésre álló memória található a virtuális 128MB vagy több, amely lehet a virtuális kisebb méretű a hibás.


10. Miután befejezte az képszerkesztési "/ stb/alapértelmezett/lárvajárat" / fent, a következő parancsot a lárvajárat konfiguráció újraépítéséhez:

        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

11. Győződjön meg arról, hogy a SSH-kiszolgáló telepítve és beállítva rendszerindításkor indításához.  Ez általában az alapértelmezett érték.

12. Telepítse az Azure Linux ügynök a következő parancs futtatásával:

        # sudo yum install WALinuxAgent
        # sudo systemctl enable waagent

13. Nem hozhatók létre felcserélése helyet a lemezen OS.

    Az Azure Linux ügynök automatikusan konfigurálhatja felcserélése hely a helyi erőforrás lemezzel, amelyhez a Azure kiépítése után a virtuális kapcsolódik. Ne feledje, hogy a helyi erőforrás lemez *ideiglenes* lemez, és előfordulhat, hogy közben kiürítette, amikor a virtuális leépítjük. Miután telepítettem az Azure Linux ügynök (lásd az előző lépést), a következő paraméterek /etc/waagent.conf megfelelően módosítása:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

14. A következő parancsokat a virtuális gép deprovision és Azure a kiépítési előkészítése:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

15. Kattintson a Hyper-V kezelője- **művelet-Leállítás > lefelé** . A Linux virtuális készen áll a Azure fel kell tölteni.


## <a name="next-steps"></a>Következő lépések
Most már készen áll az Oracle Linux .vhd használatával hozzon létre új virtuális gépeken futó Azure-ban. Ha az első alkalommal feltöltendő fel a .vhd fájl Azure, lépések 2 és 3 [létrehozása](virtual-machines-linux-classic-create-upload-vhd.md)és feltöltése a Linux operációs rendszert tartalmazó virtuális merevlemez.
