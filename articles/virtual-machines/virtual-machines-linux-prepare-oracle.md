<properties
pageTitle="Az Oracle Linux virtuális gép előkészítése az Azure |} Microsoft Azure"
description="Oracle Linux fut a Microsoft Azure virtuális gépet konfiguráció lépésről lépésre."
services="virtual-machines-linux"
authors="bbenz"
manager="timlt"
documentationCenter="virtual-machines"
tags="azure-service-management,azure-resource-manager"
/>

<tags
ms.service="virtual-machines-linux"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="vm-linux"
ms.workload="infrastructure-services"
ms.date="06/22/2015"
ms.author="bbenz" />

# <a name="prepare-an-oracle-linux-virtual-machine-for-azure"></a>Azure-Oracle Linux virtuális gép előkészítése

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


-   [Azure-Oracle Linux 6.4 + virtuális gép előkészítése](virtual-machines-linux-oracle-create-upload-vhd.md)

-   [Azure-Oracle Linux 7.0-s + virtuális gép előkészítése](virtual-machines-linux-oracle-create-upload-vhd.md)

## <a name="prerequisites"></a>Előfeltételek
Ez a cikk tartalma feltételezi, hogy már telepítette az Oracle Linux operációs rendszer egy virtuális merevlemezre. Több eszközök létezik .vhd fájlokat, például a Hyper-V például virtualizációs megoldást szeretne létrehozni. Című cikkben olvashat [telepítése a Hyper-V, és hozzon létre egy virtuális gép](http://technet.microsoft.com/library/hh846766.aspx).

**Az Oracle Linux telepítéssel kapcsolatos megjegyzések**

- Oracle piros kalap kompatibilis kernel és annak UEK3 (szoros vállalati Kernel) is használhatók a Hyper-V és Azure. A legjobb eredmény érdekében ügyeljen arra, hogy a legújabb kernelhez frissítheti a előkészítése az Oracle Linux virtuális.

- Oracle UEK2 nem támogatott a Hyper-V és Azure, nem tartalmazza a szükséges illesztőprogramok.

- Újabb VHDX formátumúvá Azure-ban nem támogatott. A lemez virtuális formátumba tud átalakítani a Hyper-V kezelője vagy a konvertálás-virtuális parancsmag használatával.

- Ha telepíti a Linux rendszerben, azt javasoljuk, LVM (gyakran sok telepítés alapértelmezett), hanem szabvány partíciót használható. LVM neve ütközik klónozott VMs, így elkerülhető, különösen akkor, ha-OS lemez minden eddiginél kell egy másik virtuális csatolja az alábbiakkal. LVM vagy [RAID](virtual-machines-linux-configure-raid.md) adatok lemezen használhatók meg, ha az előnyben részesített.

- NUMA nagyobb virtuális méretek Linux kernelverziókra 2.6.37 alatt egy hiba miatt nem támogatott. Ez a probléma elsősorban hatással van a felsőbb szintű használó terjesztését piros kalap 2.6.32 kernel. Manuális telepítés az Azure Linux ügynök (waagent) a Linux kernel LÁRVAJÁRAT konfigurációjában automatikusan NUMA tiltsa le. További tudnivalók találhatók az alábbi lépéseket.

- A csere partíciót a OS lemezen nem állítja be. A Linux agent beállítható úgy, hogy az ideiglenes erőforrás lemezen felcserélése fájl létrehozása. További tudnivalók találhatók az alábbi lépéseket.

- Az összes a VHD kell rendelkeznie a többszörös 1 MB-os oldalméretei.

- Győződjön meg arról, hogy a `Addons` tárházba engedélyezve van. Válassza a fájl szerkesztése `/etc/yum.repo.d/public-yum-ol6.repo`(Oracle Linux 6) vagy `/etc/yum.repo.d/public-yum-ol7.repo`(Oracle Linux), és módosítsa a sor `enabled=0` való `enabled=1` **[ol6_addons]** és **[ol7_addons]** a fájlban.


## <a name="oracle-linux-64"></a>Az Oracle Linux 6.4 +
A virtuális gép futtatásához Azure-ban az operációs rendszer adott konfigurációs lépéseket kell elvégeznie.

1. A Hyper-V kezelője a középső ablaktáblában jelölje ki a virtuális gépen.

2. Kattintson a **Csatlakozás** a virtuális gép az ablak megnyitásához.

3. Távolítsa el a NetworkManager a következő parancs futtatásával:

        # sudo rpm -e --nodeps NetworkManager

    >[AZURE.NOTE] A csomag már nem települ, ha ez a parancs a megjelenő hibaüzenet sikertelen lesz. Várható.

4. Hozzon létre egy **hálózati** a címtárban/etc/sysconfig/a következő szöveget tartalmazó nevű fájlt:

    `NETWORKING=yes`  
    `HOSTNAME=localhost.localdomain`

5.  Hozzon létre egy **ifcfg-eth0** a /etc/sysconfig/network-scripts a nevű fájlt / a következő szöveget tartalmazó könyvtár:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
            TYPE=Ethernet
            USERCTL=no
            PEERDNS=yes
        IPV6INIT=no

6.  Lépés (vagy eltávolítása) udev szabályok létrehozása az Ethernet-illesztő statikus szabályairól elkerülése érdekében. Szabályok problémákat okozhatnak, amikor éppen klónozhatja a virtuális gép az Azure vagy a Hyper-V:

        # sudo mkdir -m 0700 /var/lib/waagent
        # sudo mv /lib/udev/rules.d/75-persistent-net-generator.rules /var/lib/waagent/ 2\>/dev/null
        # sudo mv /etc/udev/rules.d/70-persistent-net.rules /var/lib/waagent/ 2\>/dev/null

7.  Győződjön meg arról, hogy a hálózati szolgáltatást a rendszer indításakor elindul a következő parancs futtatásával:

        # chkconfig network on

8.  Telepítse a python-pyasn1 a következő parancs futtatásával:

        # sudo yum install python-pyasn1

9.  Módosítsa a lárvajárat konfigurációs, amelyet fel szeretne venni további kernel paraméterek Azure kernel indítási sorára. Ehhez nyissa meg a "/ boot/grub/menu.lst" szövegszerkesztőben, és győződjön meg arról, hogy az alapértelmezett kernel tartalmazza a következő paraméterek:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300 numa=off

    Ez biztosítja, hogy minden konzol üzenet elküldi az első, segítheti Azure soros portot problémák hibakeresési támogatás. Ezzel letiltja NUMA Oracle piros kalap kompatibilis rendszerhéj egy hiba miatt.

    A fenti kívül akkor javasoljuk, hogy *távolítsa el* a következő paraméterek:

        rhgb quiet crashkernel=auto

    A grafikus és csendes indítási sem lehet hasznos, ha azt szeretné, hogy a soros porthoz küldeni a naplókat felhőalapú környezetben.

    A `crashkernel` beállítás lehet balra beállítva, ha azt szeretné, de a megjegyzést, hogy a paraméter csökkenti a virtuális 128 MB vagy több a rendelkezésre álló memória. Lehet, hogy ez a virtuális kisebb méretű a problémát.

10.  Győződjön meg arról, hogy a SSH-kiszolgáló telepítve és beállítva rendszerindításkor indításához. Ez általában az alapértelmezett érték.

11.  Telepítse az Azure Linux ügynök a következő parancs futtatásával:

        # <a name="sudo-yum-install-walinuxagent"></a>sudo yum telepítés WALinuxAgent

    Ne feledje, hogy a WALinuxAgent csomag telepítésének eltávolítása a NetworkManager NetworkManager-gnome csomagok Ha az azok nem sikerült már eltávolítani leírtak szerint a lépés: 2.

12.  Nem hozhatók létre felcserélése helyet a lemezen OS.

    Az Azure Linux ügynök automatikusan konfigurálhatja felcserélése hely a helyi erőforrás lemez a Azure kiépítése után a virtuális csatolt segítségével. Figyelje meg, hogy a helyi erőforrás egy *ideiglenes* lemez, és előfordulhat, hogy kell kiürül, amikor a virtuális leépítjük. Miután telepítette az Azure Linux ügynök (lásd az előző lépést), a következő paraméterek /etc/waagent.conf megfelelően módosítása:

        ResourceDisk.Format=y

        ResourceDisk.Filesystem=ext4

        ResourceDisk.MountPoint=/mnt/resource

        ResourceDisk.EnableSwap=y

        ResourceDisk.SwapSizeMB=2048 ## Megjegyzés: bármilyen Ha szüksége lenne rá kell beállítani.

13.  A következő parancsokat a virtuális gép deprovision és Azure a kiépítési előkészítése:

        # <a name="sudo-waagent--force--deprovision"></a>sudo waagent-kényszerítése - deprovision
        # <a name="export-histsize0"></a>Exportálás HISTSIZE = 0
        # <a name="logout"></a>Kijelentkezés

14.  Kattintson a **művelet -\> leállítása** a Hyper-V kezelője. A Linux virtuális készen áll a Azure fel kell tölteni.

## <a name="oracle-linux-70"></a>Az Oracle Linux 7.0-s +
**Az Oracle Linux 7 változásai**

Az Oracle Linux 7 virtuális gép előkészítése Azure nagyon hasonlít a folyamat az Oracle Linux 6. Vannak azonban olyan megjegyezni több fontos különbség:

-   A piros kalap kompatibilis kernel és a Oracle UEK3 Azure-ban támogatott. Azt javasoljuk, hogy a UEK3 kernel.

-   A NetworkManager csomagot már nem ütközik az Azure Linux ügynök. A csomag alapértelmezés szerint telepítve van, és azt javasoljuk, hogy nem törlődik.

-   GRUB2 most szolgál az alapértelmezett rendszertöltőbe, hogy az eljárás szerkesztése paramétereinek kernel megváltozott (lásd alább).

-   XFS ettől kezdve az alapértelmezett fájlrendszer. A ext4 fájlrendszer továbbra is használható, ha szükségesnek látja.

**Konfigurálási lépéseket**

1.  A Hyper-V-kezelőben válassza ki a virtuális gépen.

2.  Kattintson a **Csatlakozás** a virtuális gép konzol az ablak megnyitásához.

3.  Hozzon létre egy **hálózati** a címtárban/etc/sysconfig/a következő szöveget tartalmazó nevű fájlt:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

4.  Hozzon létre egy **ifcfg-eth0** a /etc/sysconfig/network-scripts a nevű fájlt / a következő szöveget tartalmazó könyvtár:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
            PEERDNS=yes
        IPV6INIT=no

5.  Lépés (vagy eltávolítása) udev szabályok létrehozása az Ethernet-illesztő statikus szabályairól elkerülése érdekében. Szabályok problémákat okozhatnak, amikor éppen klónozhatja a Hyper-V vagy a Microsoft Azure virtuális géphez.

        # sudo mkdir -m 0700 /var/lib/waagent
        # sudo mv /lib/udev/rules.d/75-persistent-net-generator.rules /var/lib/waagent/ 2>/dev/null
        # sudo mv /etc/udev/rules.d/70-persistent-net.rules /var/lib/waagent/ 2>/dev/null

6.  Győződjön meg arról, hogy a hálózati szolgáltatást a rendszer indításakor elindul a következő parancs futtatásával:

        # sudo chkconfig network on

7.  Telepítse a python-pyasn1 csomag a következő parancs futtatásával:

        # sudo yum install python-pyasn1

8.  Törölje a jelet a jelenlegi yum metaadatokat, és telepítse a frissítéseket a következő parancsot futtatva:

        # sudo yum clean all
        # sudo yum -y update

9.  Módosítsa a lárvajárat konfigurációs, amelyet fel szeretne venni további kernel paraméterek Azure kernel indítási sorára. Ehhez a megnyitott "/ stb/alapértelmezett/lárvajárat" szövegszerkesztőben és szerkesztése a LÁRVAJÁRAT\_parancssor\_LINUX paraméter, például:

        GRUB\_CMDLINE\_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0"

    Ez biztosítja az első, segítheti Azure soros portot összes konzol üzenetek fogadására problémák hibakeresési támogatás. A fenti kívül akkor javasoljuk, hogy *távolítsa el* a következő paraméterek:

        rhgb quiet crashkernel=auto

    A grafikus és csendes indítási sem lehet hasznos, ha azt szeretné, hogy a soros porthoz küldeni a naplókat felhőalapú környezetben.

    A `crashkernel` beállítás lehet balra beállítva, ha azt szeretné, de a megjegyzést, hogy a paraméter csökkenti a virtuális 128 MB vagy több a rendelkezésre álló memória. Lehet, hogy ez a virtuális kisebb méretű a problémát.

10.  Miután befejezte az képszerkesztési "/ stb/alapértelmezett/lárvajárat", a következő parancsot a lárvajárat konfiguráció újraépítéséhez:

        # <a name="sudo-grub2-mkconfig--o-bootgrub2grubcfg"></a>sudo grub2-mkconfig - o /boot/grub2/grub.cfg

11.  Győződjön meg arról, hogy a SSH-kiszolgáló telepítve és beállítva rendszerindításkor indításához. Ez általában az alapértelmezett érték.

12.  Telepítse az Azure Linux ügynök a következő parancs futtatásával:

        # <a name="sudo-yum-install-walinuxagent"></a>sudo yum telepítés WALinuxAgent

13.  Nem hozhatók létre felcserélése helyet a lemezen OS.

    Az Azure Linux ügynök automatikusan konfigurálhatja felcserélése hely a helyi erőforrás lemez a Azure kiépítése után a virtuális csatolt segítségével. Figyelje meg, hogy a helyi erőforrás egy *ideiglenes* lemez, és előfordulhat, hogy kell kiürül, amikor a virtuális leépítjük. Miután telepítette az Azure Linux ügynök (lásd az előző lépést), a következő paraméterek /etc/waagent.conf megfelelően módosítása:

        ResourceDisk.Format=y ResourceDisk.Filesystem=ext4 ResourceDisk.MountPoint=/mnt/resource ResourceDisk.EnableSwap=y ResourceDisk.SwapSizeMB=2048 ## Megjegyzés: bármilyen Ha szüksége lenne rá kell beállítani.

14.  A következő parancsokat a virtuális gép deprovision és Azure a kiépítési előkészítése:

        # <a name="sudo-waagent--force--deprovision"></a>sudo waagent-kényszerítése - deprovision
        # <a name="export-histsize0"></a>Exportálás HISTSIZE = 0
        # <a name="logout"></a>Kijelentkezés

15.  Kattintson a **művelet -\> leállítása** a Hyper-V kezelője. A Linux virtuális készen áll a Azure fel kell tölteni.
