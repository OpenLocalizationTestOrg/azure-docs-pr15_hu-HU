<properties
    pageTitle="Hozzon létre, és töltse fel a egy piros kalap vállalati Linux virtuális Azure-ban való használatra"
    description="Ismerje meg, hozhat létre, és töltse fel az Azure virtuális merevlemez (virtuális), amely tartalmazza a piros kalap Linux operációs rendszert."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="SuperScottz"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/17/2016"
    ms.author="mingzhan"/>


# <a name="prepare-a-red-hat-based-virtual-machine-for-azure"></a>Azure virtuális piros kalap-alapú gépek előkészítése

Ebben a cikkben megtanulhatja, hogy miként piros kalap vállalati Linux (RHEL) virtuális gép használata az Azure. Ebben a cikkben szereplő RHEL verziója, hogy a 6,7, 7.1 és 7.2. Ebben a cikkben szereplő Hypervisors előkészítésére, hogy a Hyper-V, Kernel alapú virtuális gép (KVM) és a VMware. Követelményei piros kalap Felhőelérés programban részt vevő kapcsolatos további tudnivalókért olvassa el a [piros kalap Felhőelérés webhelyet](http://www.redhat.com/en/technologies/cloud-computing/cloud-access) , és [A Azure operációs rendszert futtató RHEL](https://access.redhat.com/articles/1989673)című témakört.

[Felkészülés a RHEL 6,7 virtuális gép a Hyper-V kezelője](#rhel67hyperv)

[Felkészülés a RHEL 7.1/7.2 virtuális gép a Hyper-V kezelője](#rhel7xhyperv)

[Felkészülés a RHEL 6,7 virtuális gép a KVM](#rhel67kvm)

[Felkészülés a RHEL 7.1/7.2 virtuális gép a KVM](#rhel7xkvm)

[Felkészülés a RHEL 6,7 virtuális gép a VMware](#rhel67vmware)

[Felkészülés a RHEL 7.1/7.2 virtuális gép a VMware](#rhel7xvmware)

[Felkészülés a RHEL 7.1/7.2 virtuális gép kickstart fájlból](#rhel7xkickstart)


## <a name="prepare-a-red-hat-based-virtual-machine-from-hyper-v-manager"></a>Felkészülés a virtuális piros kalap-alapú gépek a Hyper-V kezelője
### <a name="prerequisites"></a>Előfeltételek
Ez a szakasz tartalma feltételezi, hogy már telepítette a RHEL kép (a az ISO-fájl piros kalap webhely kapott) egy virtuális merevlemezre (virtuális). Használatát a Hyper-V Manager telepítheti az operációs rendszer képet, lásd: [telepítse a Hyper-V szerepkör és konfigurálása a virtuális gép](http://technet.microsoft.com/library/hh846766.aspx).

**RHEL telepítéssel kapcsolatos megjegyzések**

- Kérjük, lásd még: [Általános Linux telepítéssel kapcsolatos megjegyzések](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) Azure Linux Felkészülés a további tippeket.

- Újabb VHDX formátumúvá Azure-ban nem támogatott. A lemez virtuális formátumba tud átalakítani a Hyper-V kezelője vagy a **Konvertálás-virtuális** PowerShell-parancsmag használatával.

- VHD kell létrehozni, a "fix" – dinamikus VHD nem támogatottak.

- Ha telepíti a Linux rendszerben, azt javasoljuk, LVM (gyakran sok telepítés alapértelmezett), hanem szabvány partíciót használható. LVM neve ütközik klónozott VMs, így elkerülhető, különösen akkor, ha-OS lemezre minden eddiginél kell egy másik virtuális csatolja az alábbiakkal. LVM vagy [RAID](virtual-machines-linux-configure-raid.md) használható adatok lemezen Ha előnyben részesített.

- A csere partíciót a OS lemezen nem állítja be. Beállíthatja, hogy a Linux agent az ideiglenes erőforrás lemezen felcserélése fájlt. További információt a érhető el az alábbi lépéseket.

- Az összes a VHD kell rendelkeznie a többszörös 1 MB-os oldalméretei.

- **Qemu-img** lemez képek konvertálása virtuális formátumba való használatakor ne feledje, hogy van egy ismert hibát qemu-img verzióiban 2.2.1 vagy újabb verziójában. Ezt a hibát eredményez egy helytelenül formázott virtuális. A probléma célja egy rövidesen qemu-img meg kell oldódnia. Most, azt javasoljuk, hogy qemu-img 2.2.0 verzióját használja, vagy korábbi verziójában.

### <a id="rhel67hyperv"> </a>RHEL 6,7 virtuális gép előkészítése a Hyper-V kezelője###


1.  A Hyper-V-kezelőben válassza ki a virtuális gépen.

2.  Kattintson a **Csatlakozás** a virtuális gép konzol az ablak megnyitásához.

3.  Távolítsa el a NetworkManager a következő parancs futtatásával:

        # sudo rpm -e --nodeps NetworkManager

    Figyelje meg, hogy a csomag már nem települ, ha ez a parancs sikertelen lesz a megjelenő hibaüzenet. Várható.

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

6.  Lépés (vagy eltávolítása) udev szabályok létrehozása az Ethernet-illesztő statikus szabályairól elkerülése érdekében. Szabályok problémákat okozhatnak, amikor, klónozhatja, a Hyper-V: vagy a Microsoft Azure virtuális gép

        # sudo mkdir -m 0700 /var/lib/waagent
        # sudo mv /lib/udev/rules.d/75-persistent-net-generator.rules /var/lib/waagent/
        # sudo mv /etc/udev/rules.d/70-persistent-net.rules /var/lib/waagent/

7.  Győződjön meg arról, hogy a hálózati szolgáltatást a rendszer indításakor elindul a következő parancs futtatásával:

        # sudo chkconfig network on

8.  Regisztráció a piros kalap-előfizetés a RHEL tárházba csomagok telepítésének engedélyezése a következő parancs futtatásával:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

9.  A WALinuxAgent csomagot `WALinuxAgent-<version>` van már nyomni az piros kalap extrák tárházba. A extrák tár engedélyezése a következő parancs futtatásával:

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

10. Módosítsa a lárvajárat konfigurációs, amelyet fel szeretne venni további kernel paraméterek Azure kernel indítási sorára. Ehhez nyissa meg a `/boot/grub/menu.lst` szövegszerkesztőben, és győződjön meg arról, hogy az alapértelmezett kernel tartalmazza a következő paraméterek:

        console=ttyS0
        earlyprintk=ttyS0
        rootdelay=300
        numa=off

    Ez biztosítja, hogy minden konzol üzenet elküldi az első, segítheti Azure soros portot problémák hibakeresési támogatás. Ezzel letiltja NUMA a RHEL 6 által használt kernelverziót egy hiba miatt.

    A fenti művelet kívül azt javasoljuk, hogy távolítsa el a következő paraméterek:

        rhgb quiet crashkernel=auto

    A grafikus és csendes indítási sem lehet hasznos, ha azt szeretné, hogy a soros porthoz küldeni a naplókat felhőalapú környezetben.

    A crashkernel beállítás lehet balra beállítva, ha azt szeretné, de a megjegyzést, hogy a paraméter csökkenti a virtuális 128 MB vagy több a rendelkezésre álló memória. Ez lehet okoz a kisebb virtuális méretét.

11. Győződjön meg arról, hogy a SSH-kiszolgáló telepítve és beállítva rendszerindításkor indításához. Ez általában az alapértelmezett érték. Ha meg szeretné jeleníteni a következő sort /etc/ssh/sshd_config módosítása:

        ClientAliveInterval 180

12. Telepítse az Azure Linux ügynök a következő parancs futtatásával:

        # sudo yum install WALinuxAgent
        # sudo chkconfig waagent on

    Ne feledje, hogy a WALinuxAgent csomag telepítésének eltávolítása a NetworkManager NetworkManager-gnome csomagok Ha az azok nem sikerült már eltávolítani leírtak szerint a lépés: 2.

13. Nem hozhatók létre felcserélése helyet a lemezen OS.
Az Azure Linux ügynök automatikusan konfigurálhatja felcserélése hely a helyi erőforrás lemez, amely a virtuális van csatolva, miután a virtuális már kiépítve Azure a segítségével. Ne feledje, hogy a helyi erőforrás lemez ideiglenes lemez, és előfordulhat, hogy közben kiürítette, amikor a virtuális leépítjük. Miután telepítette az Azure Linux ügynök (lásd az előző lépést), a következő paraméterek /etc/waagent.conf megfelelően módosítása:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

14. Az előfizetés unregister (ha szükséges) a következő parancs futtatásával:

        # sudo subscription-manager unregister

15. A következő parancsokat a virtuális gép deprovision és Azure a kiépítési előkészítése:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

16. Kattintson a **művelet > leállítása** a Hyper-V kezelője. A Linux virtuális készen áll a Azure fel kell tölteni.
 

### <a id="rhel7xhyperv"> </a>RHEL 7.1/7.2 virtuális gép előkészítése a Hyper-V kezelője###

1.  A Hyper-V-kezelőben válassza ki a virtuális gépen.

2.  Kattintson a **Csatlakozás** a virtuális gép konzol az ablak megnyitásához.

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

5.  Győződjön meg arról, hogy a hálózati szolgáltatást a rendszer indításakor elindul a következő parancs futtatásával:

        # sudo chkconfig network on

6.  Regisztráció a piros kalap-előfizetés a RHEL tárházba csomagok telepítésének engedélyezése a következő parancs futtatásával:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

7.  Módosítsa a lárvajárat konfigurációs, amelyet fel szeretne venni további kernel paraméterek Azure kernel indítási sorára. Ehhez nyissa meg a `/etc/default/grub` szövegszerkesztőben és szerkesztése a **GRUB_CMDLINE_LINUX** paramétert. Példa:

        GRUB_CMDLINE_LINUX="rootdelay=300
        console=ttyS0
        earlyprintk=ttyS0"

    Ez biztosítja, hogy minden konzol üzenet elküldi az első, segítheti Azure soros portot problémák hibakeresési támogatás. A fenti művelet kívül azt javasoljuk, hogy távolítsa el a következő paraméterek:

        rhgb quiet crashkernel=auto

    A grafikus és csendes indítási sem lehet hasznos, ha azt szeretné, hogy a soros porthoz küldeni a naplókat felhőalapú környezetben.
    A crashkernel beállítás lehet balra beállítva, ha azt szeretné, de a megjegyzést, hogy a paraméter csökkenti a virtuális 128 MB vagy több a rendelkezésre álló memória. Ez lehet okoz a kisebb virtuális méretét.

8.  Miután végzett a szerkesztési `/etc/default/grub`, futtassa a következő parancsot a lárvajárat konfiguráció újraépítéséhez:

        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

9.  Győződjön meg arról, hogy a SSH-kiszolgáló telepítve és beállítva rendszerindításkor indításához. Ez általában az alapértelmezett érték. Módosítása `/etc/ssh/sshd_config` szeretné hozzáadni a következő parancsot:

        ClientAliveInterval 180

10. A WALinuxAgent csomag `WALinuxAgent-<version>` van már tolni a piros kalap extrák tárházba. A extrák tár engedélyezése a következő parancs futtatásával:

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

11. Telepítse az Azure Linux ügynök a következő parancs futtatásával:

        # sudo yum install WALinuxAgent
        # sudo systemctl enable waagent.service

12. Nem hozhatók létre felcserélése helyet a lemezen OS. Az Azure Linux ügynök automatikusan konfigurálhatja felcserélése hely a helyi erőforrás lemez, amely a virtuális van csatolva, miután a virtuális már kiépítve Azure a segítségével. Ne feledje, hogy a helyi erőforrás lemez ideiglenes lemez, és előfordulhat, hogy közben kiürítette, amikor a virtuális leépítjük. Miután telepítette az Azure Linux ügynök (lásd az előző lépést), a következő paraméterek módosítása `/etc/waagent.conf` megfelelően:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

13. Ha az előfizetés unregister, futtassa a következő parancsot:

        # sudo subscription-manager unregister

14. A következő parancsokat a virtuális gép deprovision és Azure a kiépítési előkészítése:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

15. Kattintson a **művelet > leállítása** a Hyper-V kezelője. A Linux virtuális készen áll a Azure fel kell tölteni.
 


## <a name="prepare-a-red-hat-based-virtual-machine-from-kvm"></a>A virtuális piros kalap-alapú gépek készítsünk KVM

### <a id="rhel67kvm"> </a>Készítsünk egy RHEL 6,7 virtuális gép KVM###


1.  Töltse le a piros kalap webhely RHEL 6,7 KVM képe.

2.  Adja meg a legfelső szintű jelszót.

    A titkosított jelszót létrehozni, és másolja a parancs:

        # openssl passwd -1 changeme

    Guestfish a legfelső szintű jelszó beállítása:

        # guestfish --rw -a <image-name>
        ><fs> run
        ><fs> list-filesystems
        ><fs> mount /dev/sda1 /
        ><fs> vi /etc/shadow
        ><fs> exit

    A második mező a legfelső szintű felhasználó módosítása "!" a titkosított jelszót.

3.  Virtuális gép létrehozása a KVM a qcow2 kép, a lemez típusa **qcow2**válassza, és állítsa a virtuális hálózati kapcsolat eszköz modell **virtio**. Ezután indítsa el a virtuális gép, és jelentkezzen be a legfelső szintű.

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

6.  Áthelyezése (vagy eltávolítja) az Ethernet-illesztő statikus szabályairól létrehozásának elkerülése érdekében a udev szabályokat. Szabályok problémákat okozhatnak, amikor, klónozhatja, a Hyper-V: vagy a Microsoft Azure virtuális gép

        # mkdir -m 0700 /var/lib/waagent
        # mv /lib/udev/rules.d/75-persistent-net-generator.rules /var/lib/waagent/
        # mv /etc/udev/rules.d/70-persistent-net.rules /var/lib/waagent/

7.  Győződjön meg arról, hogy a hálózati szolgáltatást a rendszer indításakor elindul a következő parancs futtatásával:

        # chkconfig network on

8.  Regisztráció a piros kalap-előfizetés a RHEL tárházba csomagok telepítésének engedélyezése a következő parancs futtatásával:

        # subscription-manager register --auto-attach --username=XXX --password=XXX

9.  Módosítsa a lárvajárat konfigurációs, amelyet fel szeretne venni további kernel paraméterek Azure kernel indítási sorára. Ehhez nyissa meg a `/boot/grub/menu.lst` szövegszerkesztőben, és győződjön meg arról, hogy az alapértelmezett kernel tartalmazza a következő paraméterek:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300 numa=off

    Ez biztosítja, hogy minden konzol üzenet elküldi az első, segítheti Azure soros portot problémák hibakeresési támogatás. Ezzel letiltja NUMA a RHEL 6 által használt kernelverziót egy hiba miatt.

    A fenti művelet kívül azt javasoljuk, hogy távolítsa el a következő paraméterek:

        rhgb quiet crashkernel=auto

    A grafikus és csendes indítási sem lehet hasznos, ha azt szeretné, hogy a soros porthoz küldeni a naplókat felhőalapú környezetben.
    A crashkernel beállítás lehet balra beállítva, ha azt szeretné, de a megjegyzést, hogy a paraméter csökkenti a virtuális 128 MB vagy több a rendelkezésre álló memória. Ez lehet okoz a kisebb virtuális méretét.

10. Adja hozzá a Hyper-V modulok initramfs be:  

    Szerkesztés `/etc/dracut.conf` és tartalom hozzáadása: add_drivers += "hv_vmbus hv_netvsc hv_storvsc"

    Initramfs újraépítéséhez: # dracut – az f - v

11. Távolítsa el a felhőben nicializálás:

        # yum remove cloud-init

12. Győződjön meg arról, hogy a SSH-kiszolgáló telepítve és beállítva rendszerindításkor indítása:

        # chkconfig sshd on

    Vegye fel a következő sorokat szeretne /etc/ssh/sshd_config módosítása:

        PasswordAuthentication yes
        ClientAliveInterval 180

    Indítsa újra a sshd:

        # service sshd restart

13. A WALinuxAgent csomagot `WALinuxAgent-<version>` van már nyomni az piros kalap extrák tárházba. A extrák tár engedélyezése a következő parancs futtatásával:

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

14. Telepítse az Azure Linux ügynök a következő parancs futtatásával:

        # yum install WALinuxAgent
        # chkconfig waagent on

15. Az Azure Linux ügynök automatikusan konfigurálhatja felcserélése hely a helyi erőforrás lemez, amely a virtuális van csatolva, miután a virtuális már kiépítve Azure a segítségével. Ne feledje, hogy a helyi erőforrás lemez ideiglenes lemez, és előfordulhat, hogy közben kiürítette, amikor a virtuális leépítjük. Miután telepítette az Azure Linux ügynök (lásd az előző lépést), a következő paraméterek **/etc/waagent.conf** megfelelően módosítása:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

16. Az előfizetés unregister (ha szükséges) a következő parancs futtatásával:

        # subscription-manager unregister

17. A következő parancsokat a virtuális gép deprovision és Azure a kiépítési előkészítése:

        # waagent -force -deprovision
        # export HISTSIZE=0
        # logout

18. Állítsa le a virtuális a KVM.

19. A qcow2 kép átalakítása virtuális formátum.
    A kép először átalakítása nyers formázása:

         # qemu-img convert -f qcow2 –O raw rhel-6.7.qcow2 rhel-6.7.raw
    Győződjön meg arról, hogy a nyers kép méretének 1 MB igazodik. Egyéb esetben felkerekítése méretének igazítása az 1 MB: # MB = $((1024*1024)) # méret = $(qemu-img információ -f nyers – kimeneti json "rhel-6.7.raw" |} \ gawk "felel meg (0 Ft, /"virtuális méretét": ([0 – 9-es] +), / val) {val[1]}') # rounded_size nyomtatása = $((($size/$MB + 1)*$MB))

         # qemu-img resize rhel-6.7.raw $rounded_size

    A rögzített méretű virtuális konvertálja a nyers lemez:

         # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-6.7.raw rhel-6.7.vhd


### <a id="rhel7xkvm"> </a>Készítsünk egy RHEL 7.1/7.2 virtuális gép KVM###


1.  Töltse le a piros kalap webhely RHEL 7.1 (vagy 7.2) KVM képe. Az alábbi példaként RHEL 7.1 fogjuk használni.

2.  Adja meg a legfelső szintű jelszót.

    A titkosított jelszót létrehozni, és másolja a parancs:

        # openssl passwd -1 changeme

    Guestfish a legfelső szintű jelszó beállítása.

        # guestfish --rw -a <image-name>
        ><fs> run
        ><fs> list-filesystems
        ><fs> mount /dev/sda1 /
        ><fs> vi /etc/shadow
        ><fs> exit

    A második mező a legfelső szintű felhasználó módosítása "!" a titkosított jelszót.

3.  Virtuális gép létrehozása a KVM a qcow2 kép, a lemez típusa **qcow2**válassza, és állítsa a virtuális hálózati kapcsolat eszköz modell **virtio**. Ezután indítsa el a virtuális gép, és jelentkezzen be a legfelső szintű.

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

6.  Győződjön meg arról, hogy a hálózati szolgáltatást a rendszer indításakor elindul a következő parancs futtatásával:

        # chkconfig network on

7.  Regisztráció piros kalap előfizetését a RHEL tárházba csomagok telepítésével ahhoz, hogy a következő parancs futtatásával:

        # subscription-manager register --auto-attach --username=XXX --password=XXX

8.  Módosítsa a lárvajárat konfigurációs, amelyet fel szeretne venni további kernel paraméterek Azure kernel indítási sorára. Ehhez nyissa meg a `/etc/default/grub` szövegszerkesztőben és szerkesztése a **GRUB_CMDLINE_LINUX** paramétert. Példa:

        GRUB_CMDLINE_LINUX="rootdelay=300
        console=ttyS0
        earlyprintk=ttyS0"

    Ez biztosítja, hogy minden konzol üzenet elküldi az első, segítheti Azure soros portot problémák hibakeresési támogatás. A fenti művelet kívül azt javasoljuk, hogy távolítsa el a következő paraméterek:

        rhgb quiet crashkernel=auto

    A grafikus és csendes indítási sem lehet hasznos, ha azt szeretné, hogy a soros porthoz küldeni a naplókat felhőalapú környezetben.
    A crashkernel beállítás lehet balra beállítva, ha azt szeretné, de a megjegyzést, hogy a paraméter csökkenti a virtuális 128 MB vagy több a rendelkezésre álló memória. Ez lehet okoz a kisebb virtuális méretét.

9.  Miután végzett a szerkesztési `/etc/default/grub`, futtassa a következő parancsot a lárvajárat konfiguráció újraépítéséhez:

        # grub2-mkconfig -o /boot/grub2/grub.cfg

10. Adja hozzá a Hyper-V modulok initramfs be:

    Szerkesztés `/etc/dracut.conf` és tartalom hozzáadása:

        add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”

    Újraépítéséhez initramfs:

        # dracut –f -v

11. Távolítsa el a felhőben nicializálás:

        # yum remove cloud-init

12. Győződjön meg arról, hogy a SSH-kiszolgáló telepítve és beállítva rendszerindításkor indítása:

        # systemctl enable sshd

    Vegye fel a következő sorokat szeretne /etc/ssh/sshd_config módosítása:

        PasswordAuthentication yes
        ClientAliveInterval 180

    Indítsa újra a sshd:

        systemctl restart sshd

13. A WALinuxAgent csomagot `WALinuxAgent-<version>` van már nyomni az piros kalap extrák tárházba. A extrák tár engedélyezése a következő parancs futtatásával:

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

14. Telepítse az Azure Linux ügynök a következő parancs futtatásával:

        # yum install WALinuxAgent

    A waagent szolgáltatás engedélyezése:

        # systemctl enable waagent.service

15. Nem hozhatók létre felcserélése helyet a lemezen OS. Az Azure Linux ügynök automatikusan konfigurálhatja felcserélése hely a helyi erőforrás lemez, amely a virtuális van csatolva, miután a virtuális már kiépítve Azure a segítségével. Ne feledje, hogy a helyi erőforrás lemez ideiglenes lemez, és előfordulhat, hogy közben kiürítette, amikor a virtuális leépítjük. Miután telepítette az Azure Linux ügynök (lásd az előző lépést), a következő paraméterek módosítása `/etc/waagent.conf` megfelelően:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

16. Az előfizetés unregister (ha szükséges) a következő parancs futtatásával:

        # subscription-manager unregister

17. A következő parancsokat a virtuális gép deprovision és Azure a kiépítési előkészítése:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

18. Állítsa le a virtuális gép KVM.

19. A qcow2 kép átalakítása virtuális formátum.

    A kép először átalakítása nyers formázása:

         # qemu-img convert -f qcow2 –O raw rhel-7.1.qcow2 rhel-7.1.raw

    Győződjön meg arról, hogy a nyers kép méretének 1 MB igazodik. Egyéb esetben 1 MB igazodva elmozdulnak méretének felkerekítése:

         # MB=$((1024*1024))
         # size=$(qemu-img info -f raw --output json "rhel-7.1.raw" | \
                  gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')
         # rounded_size=$((($size/$MB + 1)*$MB))

         # qemu-img resize rhel-7.1.raw $rounded_size

    A rögzített méretű virtuális konvertálja a nyers lemez:

         # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-7.1.raw rhel-7.1.vhd


## <a name="prepare-a-red-hat-based-virtual-machine-from-vmware"></a>A virtuális piros kalap-alapú gépek készítsünk VMware
### <a name="prerequisites"></a>Előfeltételek
Ez a szakasz feltételezi, hogy már telepítette a RHEL virtuális gép VMware. Az operációs rendszer telepítési VMware a részletekért olvassa el [VMware vendég operációs rendszer telepítési útmutatóját](http://partnerweb.vmware.com/GOSIG/home.html).

- Ha telepíti a Linux operációs rendszer, azt javasoljuk, LVM (gyakran sok telepítés alapértelmezett), hanem szabvány partíciót használható. LVM neve ütközik klónozott VMs, így elkerülhető, különösen akkor, ha-OS lemezre minden eddiginél kell egy másik virtuális csatolja az alábbiakkal. LVM vagy RAID adatok lemezen használható meg, ha az előnyben részesített.

- A csere partíciót a OS lemezen nem állítja be. Beállíthatja, hogy a Linux agent az ideiglenes erőforrás lemezen felcserélése fájlt. Az alábbi lépéseket talál további információt a problémáról.

- A virtuális merevlemez létrehozásakor jelölje be a **tár virtuális lemez egyetlen fájlként**.



### <a id="rhel67vmware"> </a>Készítsünk egy RHEL 6,7 virtuális gép VMware###

1.  Távolítsa el a NetworkManager a következő parancs futtatásával:

         # sudo rpm -e --nodeps NetworkManager

    Figyelje meg, hogy a csomag már nem települ, ha ez a parancs sikertelen lesz a megjelenő hibaüzenet. Várható.

2.  Hozzon létre egy **hálózati** a címtárban/etc/sysconfig/a következő szöveget tartalmazó nevű fájlt:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

3.  Hozzon létre egy **ifcfg-eth0** a /etc/sysconfig/network-scripts a nevű fájlt / a következő szöveget tartalmazó könyvtár:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

4.  Áthelyezése (vagy eltávolítja) az Ethernet-illesztő statikus szabályairól létrehozásának elkerülése érdekében a udev szabályokat. Szabályok problémákat okozhatnak, amikor, klónozhatja, a Hyper-V: vagy a Microsoft Azure virtuális gép

        # sudo mkdir -m 0700 /var/lib/waagent
        # sudo mv /lib/udev/rules.d/75-persistent-net-generator.rules /var/lib/waagent/
        # sudo mv /etc/udev/rules.d/70-persistent-net.rules /var/lib/waagent/

5.  Győződjön meg arról, hogy a hálózati szolgáltatást a rendszer indításakor elindul a következő parancs futtatásával:

        # sudo chkconfig network on

6.  Regisztráció a piros kalap-előfizetés a RHEL tárházba csomagok telepítésének engedélyezése a következő parancs futtatásával:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

7.  A WALinuxAgent csomagot `WALinuxAgent-<version>` van már nyomni az piros kalap extrák tárházba. A extrák tár engedélyezése a következő parancs futtatásával:

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

8.  Módosítsa a lárvajárat konfigurációs, amelyet fel szeretne venni további kernel paraméterek Azure kernel indítási sorára. Ehhez nyissa meg a "/ boot/grub/menu.lst" szövegszerkesztőben, és győződjön meg arról, hogy az alapértelmezett kernel tartalmazza a következő paraméterek:

        console=ttyS0
        earlyprintk=ttyS0
        rootdelay=300
        numa=off

    Ez biztosítja, hogy minden konzol üzenet elküldi az első, segítheti Azure soros portot problémák hibakeresési támogatás. Ezzel letiltja NUMA a RHEL 6 által használt kernelverziót egy hiba miatt.
    A fenti művelet kívül azt javasoljuk, hogy távolítsa el a következő paraméterek:

        rhgb quiet crashkernel=auto

    A grafikus és csendes indítási sem lehet hasznos, ha azt szeretné, hogy a soros porthoz küldeni a naplókat felhőalapú környezetben.
    A crashkernel beállítás lehet balra beállítva, ha azt szeretné, de a megjegyzést, hogy a paraméter csökkenti a virtuális 128 MB vagy több a rendelkezésre álló memória. Ez lehet okoz a kisebb virtuális méretét.

9.  Adja hozzá a Hyper-V modulok initramfs be:

        Edit `/etc/dracut.conf` and add content:

            add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”

        Rebuild initramfs:

            # dracut –f -v

10. Győződjön meg arról, hogy a SSH-kiszolgáló telepítve és beállítva rendszerindításkor indításához. Ez általában az alapértelmezett érték. Módosítása `/etc/ssh/sshd_config` szeretné hozzáadni a következő parancsot:

        ClientAliveInterval 180

11. Telepítse az Azure Linux ügynök a következő parancs futtatásával:

        # sudo yum install WALinuxAgent
        # sudo chkconfig waagent on

12. Nem hozhatók létre felcserélése helyet a lemezen operációs rendszer:

    Az Azure Linux ügynök automatikusan konfigurálhatja felcserélése hely a helyi erőforrás lemez, amely a virtuális van csatolva, miután a virtuális már kiépítve Azure a segítségével. Ne feledje, hogy a helyi erőforrás lemez ideiglenes lemez, és előfordulhat, hogy közben kiürítette, amikor a virtuális leépítjük. Miután telepítette az Azure Linux ügynök (lásd az előző lépést), a következő paraméterek módosítása `/etc/waagent.conf` megfelelően:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

13. Az előfizetés unregister (ha szükséges) a következő parancs futtatásával:

        # sudo subscription-manager unregister

14. A következő parancsokat a virtuális gép deprovision és Azure a kiépítési előkészítése:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

15. Állítsa le a virtuális, és a VMDK fájl .vhd-fájllá konvertálja.

    A kép először átalakítása nyers formázása:

        # qemu-img convert -f vmdk –O raw rhel-6.7.vmdk rhel-6.7.raw

    Győződjön meg arról, hogy a nyers kép méretének 1 MB igazodik. Egyéb esetben 1 MB igazodva elmozdulnak méretének felkerekítése:

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-6.7.raw" | \
                gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')
        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-6.7.raw $rounded_size

    A rögzített méretű virtuális konvertálja a nyers lemez:

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-6.7.raw rhel-6.7.vhd


### <a id="rhel7xvmware"> </a>Készítsünk egy RHEL 7.1/7.2 virtuális gép VMware###

1.  Hozzon létre egy **hálózati** a címtárban/etc/sysconfig/a következő szöveget tartalmazó nevű fájlt:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

2.  Hozzon létre egy **ifcfg-eth0** a /etc/sysconfig/network-scripts a nevű fájlt / a következő szöveget tartalmazó könyvtár:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

3.  Győződjön meg arról, hogy a hálózati szolgáltatást a rendszer indításakor elindul a következő parancs futtatásával:

        # sudo chkconfig network on

4.  Regisztráció a piros kalap-előfizetés a RHEL tárházba csomagok telepítésének engedélyezése a következő parancs futtatásával:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

5.  Módosítsa a lárvajárat konfigurációs, amelyet fel szeretne venni további kernel paraméterek Azure kernel indítási sorára. Ehhez nyissa meg a `/etc/default/grub` szövegszerkesztőben és szerkesztése a **GRUB_CMDLINE_LINUX** paramétert. Példa:

        GRUB_CMDLINE_LINUX="rootdelay=300
        console=ttyS0
        earlyprintk=ttyS0"

    Ez biztosítja, hogy minden konzol üzenet elküldi az első, segítheti Azure soros portot problémák hibakeresési támogatás. A fenti művelet kívül azt javasoljuk, hogy távolítsa el a következő paraméterek:

        rhgb quiet crashkernel=auto

    A grafikus és csendes indítási sem lehet hasznos, ha azt szeretné, hogy a soros porthoz küldeni a naplókat felhőalapú környezetben.
    A crashkernel beállítás lehet balra beállítva, ha azt szeretné, de a megjegyzést, hogy a paraméter csökkenti a virtuális 128 MB vagy több a rendelkezésre álló memória. Ez lehet okoz a kisebb virtuális méretét.

6.  Miután végzett a szerkesztési `/etc/default/grub`, futtassa a következő parancsot a lárvajárat konfiguráció újraépítéséhez:

         # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

7.  Adja hozzá a Hyper-V modulok initramfs be:

    Szerkesztés `/etc/dracut.conf`, tartalom hozzáadása:

        add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”

    Újraépítéséhez initramfs:

        # dracut –f -v

8.  Győződjön meg arról, hogy a SSH-kiszolgáló telepítve és beállítva rendszerindításkor indításához. Ez általában az alapértelmezett érték. Módosítása `/etc/ssh/sshd_config` szeretné hozzáadni a következő parancsot:

        ClientAliveInterval 180

9.  A WALinuxAgent csomagot `WALinuxAgent-<version>` van már nyomni az piros kalap extrák tárházba. A extrák tár engedélyezése a következő parancs futtatásával:

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

10. Telepítse az Azure Linux ügynök a következő parancs futtatásával:

        # sudo yum install WALinuxAgent
        # sudo systemctl enable waagent.service

11. Nem hozhatók létre felcserélése helyet a lemezen OS. Az Azure Linux ügynök automatikusan konfigurálhatja felcserélése hely a helyi erőforrás lemez, amely a virtuális van csatolva, miután a virtuális már kiépítve Azure a segítségével. Ne feledje, hogy a helyi erőforrás lemez ideiglenes lemez, és előfordulhat, hogy közben kiürítette, amikor a virtuális leépítjük. Miután telepítette az Azure Linux ügynök (lásd az előző lépést), a következő paraméterek módosítása `/etc/waagent.conf` megfelelően:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

12. Ha az előfizetés unregister, futtassa a következő parancsot:

        # sudo subscription-manager unregister

13. A következő parancsokat a virtuális gép deprovision és Azure a kiépítési előkészítése:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

14. Állítsa le a virtuális, és a VMDK-fájl átkonvertálása virtuális formátum.

    A kép először átalakítása nyers formázása:

        # qemu-img convert -f vmdk –O raw rhel-7.1.vmdk rhel-7.1.raw

    Győződjön meg arról, hogy a nyers kép méretének 1 MB igazodik. Egyéb esetben 1 MB igazodva elmozdulnak méretének felkerekítése:

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-7.1.raw" | \
                 gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')
        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-7.1.raw $rounded_size

    A rögzített méretű virtuális konvertálja a nyers lemez:

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-7.1.raw rhel-7.1.vhd


## <a name="prepare-a-red-hat-based-virtual-machine-from-an-iso-by-using-a-kickstart-file-automatically"></a>A virtuális piros kalap-alapú gépek előkészítése az ISO automatikusan kickstart-fájl használatával


### <a id="rhel7xkickstart"> </a>Kickstart fájlból RHEL 7.1/7.2 virtuális gép előkészítése###


1.  Az alábbi tartalom hozzon létre egy kickstart fájlt, és mentse a fájlt. Kickstart telepítésével kapcsolatos részletekért lásd: a [Kickstart telepítési útmutatóját](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Installation_Guide/chap-kickstart-installations.html).



        # Kickstart for provisioning a RHEL 7 Azure VM

        # System authorization information
        auth --enableshadow --passalgo=sha512

        # Use graphical install
        text

        # Do not run the Setup Agent on first boot
        firstboot --disable

        # Keyboard layouts
        keyboard --vckeymap=us --xlayouts='us'

        # System language
        lang en_US.UTF-8

        # Network information
        network  --bootproto=dhcp

        # Root password
        rootpw --plaintext "to_be_disabled"

        # System services
        services --enabled="sshd,waagent,NetworkManager"

        # System timezone
        timezone Etc/UTC --isUtc --ntpservers 0.rhel.pool.ntp.org,1.rhel.pool.ntp.org,2.rhel.pool.ntp.org,3.rhel.pool.ntp.org

        # Partition clearing information
        clearpart --all --initlabel

        # Clear the MBR
        zerombr

        # Disk partitioning information
        part /boot --fstype="xfs" --size=500
        part / --fstyp="xfs" --size=1 --grow --asprimary

        # System bootloader configuration
        bootloader --location=mbr

        # Firewall configuration
        firewall --disabled

        # Enable SELinux
        selinux --enforcing

        # Don't configure X
        skipx

        # Power down the machine after install
        poweroff



        %packages
        @base
        @console-internet
        chrony
        sudo
        parted
        -dracut-config-rescue

        %end

        %post --log=/var/log/anaconda/post-install.log

        #!/bin/bash

        # Register Red Hat Subscription
        subscription-manager register --username=XXX --password=XXX --auto-attach --force

        # Install latest repo update
        yum update -y

        # Enable extras repo
        subscription-manager repos --enable=rhel-7-server-extras-rpms

        # Install WALinuxAgent
        yum install -y WALinuxAgent

        # Unregister Red Hat subscription
        subscription-manager unregister

        # Enable waaagent at boot-up
        systemctl enable waagent

        # Disable the root account
        usermod root -p '!!'

        # Configure swap in WALinuxAgent
        sed -i 's/^\(ResourceDisk\.EnableSwap\)=[Nn]$/\1=y/g' /etc/waagent.conf
        sed -i 's/^\(ResourceDisk\.SwapSizeMB\)=[0-9]*$/\1=2048/g' /etc/waagent.conf

        # Set the cmdline
        sed -i 's/^\(GRUB_CMDLINE_LINUX\)=".*"$/\1="console=tty1 console=ttyS0 earlyprintk=ttyS0 rootdelay=300"/g' /etc/default/grub

        # Enable SSH keepalive
        sed -i 's/^#\(ClientAliveInterval\).*$/\1 180/g' /etc/ssh/sshd_config

        # Build the grub cfg
        grub2-mkconfig -o /boot/grub2/grub.cfg

        # Configure network
        cat << EOF > /etc/sysconfig/network-scripts/ifcfg-eth0
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=yes
        EOF

        # Deprovision and prepare for Azure
        waagent -force -deprovision

        %end

2.  Helyezze a kickstart fájlt a telepítés rendszerből elérhető helyen.

3.  A Hyper-V-kezelőben hozzon létre egy új virtuális. **Kapcsolódás virtuális merevlemez** lapon jelölje ki a **virtuális merevlemez később csatolása**, és fejezze be az új virtuális gép varázslót.

4.  Nyissa meg a virtuális beállításokat:

    egy.  A virtuális új virtuális merevlemez-meghajtó csatolhat. Győződjön meg róla, hogy **Virtuális formátum** és a **Rögzített méretű**.

    b.  A telepítés ISO csatolása a DVD-meghajtóba.

    c billentyűkombinációt.  Állítsa a BIOS CD-ről.

5.  Indítsa el a virtuális. Amikor megjelenik a telepítési útmutatója, nyomja le a **lapon** az indítási beállítások konfigurálása.

6.  ENTER `inst.ks=<the location of the kickstart file>` végén található az indítási beállítások, és nyomja le az **ENTER billentyűt**.

7.  Várja meg a telepítés befejezéséhez. Amikor elkészült, a program automatikusan leállítása a virtuális. A Linux virtuális készen áll a Azure fel kell tölteni.

## <a name="known-issues"></a>Ismert problémák
Vannak ismert problémák a Hyper-V és Azure RHEL 7.1 használatakor.

### <a name="disk-io-freeze"></a>Lemezen I/O rögzítése

Ez a probléma gyakori tároló lemez I/O tevékenységeket a Hyper-V és Azure RHEL 7.1 közben előfordulhatnak.   

Reprodukálja ráta:

Ez a probléma nem folyamatos. Jó helyen jár akkor fordul elő, további gyakran során gyakran használják lemez bemeneti és kimeneti műveletet a Hyper-V és Azure.   


[AZURE.NOTE] Ismert probléma piros kalap már tárgyalt. A kapcsolódó javítások telepítéséhez futtassa a következő parancsot:

    # sudo yum update

### <a name="the-hyper-v-driver-could-not-be-included-in-the-initial-ram-disk-when-using-a-non-hyper-v-hypervisor"></a>A Hyper-V illesztőprogram sikerült nem jelenő a kezdeti RAM lemez a nem a Hyper-V hipervizor használata esetén

Bizonyos esetekben Linux telepítőfájlokat előfordulhat, hogy nem szerepel a illesztőprogramok Hyper-v a kezdeti (initrd vagy initramfs) RAM lemez kivéve, ha azt észleli, hogy fut a Hyper-V környezetben.

Készítse elő a Linux képet, hogy használja egy másik virtualizációs rendszer (azaz Virtualbox, Xen, stb.), amikor kell ahhoz, hogy legalább initrd újraépítéséhez hv_vmbus és hv_storvsc kernel modulokat a kezdeti RAM lemezen érhetők el. Az ismert probléma legalább rendszereken a felsőbb szintű piros kalap eloszlás alapján.

Ez a probléma megoldásához kell initramfs beszúrhat a Hyper-V modulok és újraépítéséhez azt:

Szerkesztés `/etc/dracut.conf` és tartalom hozzáadása:

        add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”

Újraépítéséhez initramfs:

        # dracut –f -v

További részletekért lásd [initramfs Újraépítés](https://access.redhat.com/solutions/1958)vonatkozó információkat.

## <a name="next-steps"></a>Következő lépések
Most már készen áll új virtuális gépeken futó létrehozása az Azure Red Hat Enterprise Linux virtuális merevlemezen használandó. Ha az első alkalommal feltöltendő fel a .vhd fájl Azure, lépések 2 és 3 [létrehozása](virtual-machines-linux-classic-create-upload-vhd.md)és feltöltése a Linux operációs rendszert tartalmazó virtuális merevlemez.

A piros kalap vállalati Linux futtatni, akkor olvassa el [a piros kalap webhely](https://access.redhat.com/certified-hypervisors)hitelesített hypervisors olvashat bővebben.
