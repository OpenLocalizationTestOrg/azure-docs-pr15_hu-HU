<properties
    pageTitle="Hozzon létre, és töltse fel a egy Linux virtuális Azure-ban"
    description="Ismerje meg, hozhat létre, és töltse fel az Azure virtuális merevlemez (virtuális), amely tartalmazza a Linux operációs rendszert."
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
    ms.date="09/23/2016"
    ms.author="szark"/>

# <a name="information-for-non-endorsed-distributions"></a>Nem záradékkal terjesztését adatait #

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


**Fontos**: az Azure platform SLA virtuális gépeken futó operációs rendszer használata a Linux csak akkor, amikor szolgál egyik [hitelesítettek a felosztott](virtual-machines-linux-endorsed-distros.md) vonatkozik. Az összes Linux terjesztését az Azure képgyűjtemény kapott a szükséges konfigurációs hitelesített varianciája.

- [A Azure - Linux záradékkal terjesztését](virtual-machines-linux-endorsed-distros.md)
- [A Microsoft Azure Linux képek támogatása](https://support.microsoft.com/kb/2941892)

Azure futó összes terjesztését kell, hogy a névjegyadatok megfelelően futtatható a platformon Előfeltételek számos felel meg.  Ez a cikk megegyezik semmiképpen átfogó minden más; eloszlása és igazán lehetséges, hogy akkor is, ha az alábbi minden megadott feltételeknek eleget meg is kell a Linux rendszerhez annak érdekében, hogy megfelelően futása platformon jelentősen működését.

Javasoljuk, hogy indítsa el az egyik lehetőség az [Azure záradékkal terjesztését a Linux](virtual-machines-linux-endorsed-distros.md) , ezért érdemes. Az alábbi cikkekben végigvezeti Önt a különböző hitelesített Linux terjesztését Azure támogatott előkészítése:

- **[Felosztás centOS-alapú](virtual-machines-linux-create-upload-centos.md)**
- **[Debian Linux](virtual-machines-linux-debian-create-upload-vhd.md)**
- **[Az Oracle Linux](virtual-machines-linux-oracle-create-upload-vhd.md)**
- **[Piros kalap vállalati Linux](virtual-machines-linux-redhat-create-upload-vhd.md)**
- **[SLES & openSUSE](virtual-machines-linux-suse-create-upload-vhd.md)**
- **[Ubuntu](virtual-machines-linux-create-upload-ubuntu.md)**

Ez a cikk a többi általános útmutatást a Linux terjesztési futó Azure összpontosít.


## <a name="general-linux-installation-notes"></a>Általános Linux telepítéssel kapcsolatos megjegyzések ##

- Az Azure, csak a **rögzített virtuális**VHDX formátuma nem támogatott.  A lemez átalakíthatja a Hyper-V kezelője vagy a konvertálás-virtuális parancsmag használatával virtuális formátum. VirtualBox rendszer használata esetén ez azt jelenti, nem pedig az alapértelmezett dinamikusan a lemez létrehozásakor van kiosztva a **rögzített méretű** kijelölése.

- A Linux rendszerben telepítése esetén *ajánlott* használata LVM (gyakran sok telepítés alapértelmezett), hanem szabvány partíciót. LVM neve ütközik klónozott VMs, így elkerülhető, különösen akkor, ha-OS lemez minden eddiginél kell egy másik, azonos virtuális csatolja az alábbiakkal. Adatok lemezen [LVM](virtual-machines-linux-configure-lvm.md) vagy [RAID](virtual-machines-linux-configure-raid.md) is használható.

- UDF fájlrendszer csatlakoztatása kernel támogatása szükség. A Azure első rendszerindításkor a kiépítési konfiguráció átadott a Linux virtuális UDF formátumú, vendégként van csatolva media keresztül. Az Azure Linux ügynök az UDF fájlrendszer konfigurációját olvashatók, és a virtuális kiépítése csatlakoztatni tudja kell lennie.

- Linux kernelverziókra 2.6.37 alatt nem támogatja a NUMA a Hyper-V szolgáltatást úgy nagyobb virtuális méretek együtt. Erről a problémáról elsősorban megközelítés hatásai: segítségével a felsőbb szintű régebbi terjesztését piros kalap 2.6.32 kernel, és javítását a RHEL 6.6 (kernel 2.6.32, 504). Rendszert futtató egyéni mag 2.6.37 régebbi vagy régebbi RHEL-alapú mag, mint 2.6.32-504 meg az indítási paraméter `numa=off` a parancssori grub.conf a kernel a. További információt a piros kalap [KB 436883](https://access.redhat.com/solutions/436883)témakörben talál.

- A csere partíciót a OS lemezen nem állítja be. A Linux agent beállítható úgy, hogy az ideiglenes erőforrás lemezen felcserélése fájl létrehozása.  További tudnivalók találhatók az alábbi lépéseket.

- Az összes a VHD kell rendelkeznie, amelyek többszöröseire 1 MB méretű.


### <a name="installing-linux-without-hyper-v"></a>Linux telepítése a Hyper-V nélkül ###

Bizonyos esetekben Linux telepítőfájlokat lehetséges, hogy nem tartalmazza a illesztőprogramokat a Hyper-V a kezdeti RAM-lemezen (initrd vagy initramfs) kivéve, ha azt észleli, hogy fut-a Hyper-V környezetet.  Amikor egy másik virtualizációs rendszer (azaz Virtualbox, KVM, stb.) a Linux kép előkészítése, szüksége lehet ahhoz, hogy a initrd újraépítéséhez legalább az `hv_vmbus` és `hv_storvsc` kernel modulok érhetők el a kezdeti ramdisk.  Az ismert probléma legalább rendszereken a felsőbb szintű piros kalap eloszlás alapján.

Az initrd vagy initramfs kép Újraépítés szolgáló eljárás attól függően, hogy a terjesztési változhat. A terjesztési dokumentációjában, vagy támogatása a megfelelő lépéseket.  Íme egy példa, hogy hogyan újraépítéséhez a initrd használata a `mkinitrd` segédprogram:

Első lépésként biztonsági másolatot a meglévő initrd képet:

    # cd /boot
    # sudo cp initrd-`uname -r`.img  initrd-`uname -r`.img.bak

Ezután újra a initrd a a `hv_vmbus` és `hv_storvsc` kernel testre:

    # sudo mkinitrd --preload=hv_storvsc --preload=hv_vmbus -v -f initrd-`uname -r`.img `uname -r`


### <a name="resizing-vhds"></a>VHD átméretezése ###

Azure virtuális képek rendelkeznie kell egy virtuális méret 1MB igazodik.  Általában a Hyper-V készült VHD kell már zárni megfelelően.  Ha a virtuális nem megfelelően igazodik majd jelenhet meg a következőhöz hasonló hibaüzenet amikor megpróbálja *kép* készítése a virtuális:

    "The VHD http://<mystorageaccount>.blob.core.windows.net/vhds/MyLinuxVM.vhd has an unsupported virtual size of 21475270656 bytes. The size must be a whole number (in MBs).”

Ez orvoslására átméretezheti a virtuális a Hyper-V kezelője konzol vagy az [Átméretezés-virtuális](http://technet.microsoft.com/library/hh848535.aspx) Powershell-parancsmag használatával.  Ha nem futtatja a Windows környezetben majd ajánlott qemu-img használni szeretne alakítani (ha szükséges), és méretezze át a virtuális.

> [AZURE.NOTE] Van egy ismert hibát qemu-img verzióiban > =, amelynek eredménye egy helytelenül formázott virtuális 2.2.1. A probléma megoldását QEMU 2.6. Ajánlott qemu-img 2.2.0 vagy az alsó, illetve frissítheti 2.6 vagy magasabb. Hivatkozás: https://bugs.launchpad.net/qemu/+bug/1490611.


 1. A virtuális közvetlenül eszközeivel például átméretezése `qemu-img` vagy `vbox-manage` egy diagnosztizálását virtuális vonhat.  Így ajánlott első konvertálás a virtuális nyers lemezre képpé.  Ha a virtuális kép már létrehozott nyers lemez képe (például KVM néhány Hypervisors alapértelmezett) majd kihagyhatja ezt a lépést:

        # qemu-img convert -f vpc -O raw MyLinuxVM.vhd MyLinuxVM.raw

 2. Kiszámítja annak érdekében, hogy a virtuális méret 1MB igazodik a lemez kép szükséges méretét.  A következő bash parancsprogram segítheti az adatokkal.  A parancsfájl "`qemu-img info`" határozza meg a virtuális méretet a lemez kép és a méret a következő 1MB számítja ki:

        rawdisk="MyLinuxVM.raw"
        vhddisk="MyLinuxVM.vhd"

        MB=$((1024*1024))
        size=$(qemu-img info -f raw --output json "$rawdisk" | \
               gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

        rounded_size=$((($size/$MB + 1)*$MB))
        echo "Rounded Size = $rounded_size"

 3. Méretezze át a $rounded_size használja a fenti parancsfájl beállított nyers lemez:

        # qemu-img resize MyLinuxVM.raw $rounded_size

 4. Ezután konvertálja a nyers lemez vissza az egy rögzített méretű virtuális:

        # qemu-img convert -f raw -o subformat=fixed -O vpc MyLinuxVM.raw MyLinuxVM.vhd



## <a name="linux-kernel-requirements"></a>Linux Kernel vonatkozó követelmények ##

A Hyper-V és Azure Linux Integration Services (LIS) illesztőprogramjainak közvetlenül az a felsőbb szintű Linux kernel közzé. Sok terjesztését legutóbbi Linux kernelverziót (azaz 3.x) tartalmazó e elérhető illesztőprogramot nem állított még be, vagy egyébként küldje el saját mag illesztőprogramok backported verziója.  Illesztőprogramok folyamatosan változik az új javítások és szolgáltatások, a felsőbb szintű rendszerhéj, amikor csak lehetséges ajánlott egy [terjesztési záradékkal](virtual-machines-linux-endorsed-distros.md) , amely tartalmazni fogja az alábbi javításokat és frissítéseket futtatásához.

Ha egy piros kalap vállalati Linux verziók **6.0-6.3**változatának futtatja a, majd szüksége lesz a legújabb LIS illesztőprogramjainak telepítését a Hyper-v. Illesztőprogramok [ezen a helyen](http://go.microsoft.com/fwlink/p/?LinkID=254263&clcid=0x409)található. RHEL **6.4 +** (és származtatott) kezdve az LIS illesztőprogramok már szerepel a kernel az és, nincs további telepítési csomag a Azure javításuk futtatásához szükséges.

Ha egy egyéni kernelt szükség, ajánlott kernel újabb verziójával (azaz **3.8 +**). Adott terjesztését vagy a szállítók, akik a saját kernel karbantartása néhány munkamennyiség lesz szükséges rendszeresen backport LIS illesztőprogramokat a felsőbb szintű kernelből az egyéni kernelhez.  Akkor is, ha már rendszerben viszonylag legutóbbi kernelverziót nagyon ajánlott nyomon követheti a bármely felsőbb szintű javítások a LIS illesztőprogramok és backport azokat, szükség szerint. A LIS illesztőprogramját forrásfájlok helyét a Linux kernel forrás fában [MAINTAINERS](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/tree/MAINTAINERS) fájlban érhető el:

    F:  arch/x86/include/asm/mshyperv.h
    F:  arch/x86/include/uapi/asm/hyperv.h
    F:  arch/x86/kernel/cpu/mshyperv.c
    F:  drivers/hid/hid-hyperv.c
    F:  drivers/hv/
    F:  drivers/input/serio/hyperv-keyboard.c
    F:  drivers/net/hyperv/
    F:  drivers/scsi/storvsc_drv.c
    F:  drivers/video/fbdev/hyperv_fb.c
    F:  include/linux/hyperv.h
    F:  tools/hv/

Egy nagyon minimális az alábbi javításokat hiányában a Azure problémákhoz ismert, és így ezek szerepelnie kell a rendszerhéj. Ebben a listában a teljes körű, vagy az összes terjesztését teljes semmiképpen:

- [ata_piix: a Hyper-V illesztőprogramok lemezt késleltetése alapértelmezés szerint](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/drivers/ata/ata_piix.c?id=cd006086fa5d91414d8ff9ff2b78fbb593878e3c)
- [storvsc: fiókkal a hálózaton átvitt csomagok az ALAPHELYZET útvonalban](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/drivers/scsi/storvsc_drv.c?id=5c1b10ab7f93d24f29b5630286e323d1c5802d5c)
- [storvsc: WRITE_SAME használatának kerülése](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=3e8f4f4065901c8dfc51407e1984495e1748c090)
- [storvsc: RAID és virtuális host videokártya illesztőprogramok azonos írása letiltása](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=54b2b50c20a61b51199bedb6e5d2f8ec2568fb43)
- [storvsc: NULL mutató visszakeresni kijavítása](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=b12bb60d6c350b348a4e1460cd68f97ccae9822e)
- [storvsc: csengetés pufferelési hibák vonhat I/O rögzítése](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=e86fb5e8ab95f10ec5f2e9430119d5d35020c951)
- [scsi_sysfs: __scsi_remove_device dupla végrehajtását elleni védelem](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/scsi_sysfs.c?id=be821fd8e62765de43cc4f0e2db363d0e30a7e9b)


## <a name="the-azure-linux-agent"></a>Az Azure Linux ügynök ##

Az [Azure Linux ügynök](virtual-machines-linux-agent-user-guide.md) (waagent) megfelelő módon hozhatók létre az Azure virtuális Linux gép van szükség. A [Linux ügynök GitHub repó](https://github.com/Azure/WALinuxAgent)ki kérések elküldése vagy szerezhet a legújabb verzióját, a fájl problémák.

- A Linux agent csoportban a Apache 2.0-licenc van fejlesztésének. Sok terjesztését már a szükséges RPM vagy deb csomagok a agent, és így bizonyos esetekben ez telepíthető és kis munkamennyiség frissíti.

- Az Azure Linux ügynök Python v2.6 + szükséges.

- A agent is szükség van a python-pyasn1 modulra. A legtöbb terjesztését telepíthetők külön csomagként igénybe.

- Egyes esetekben az Azure Linux ügynök előfordulhat, hogy nem lesz kompatibilis NetworkManager. A felosztott által biztosított RPM/Deb csomagok számos NetworkManager konfigurálja a waagent csomaghoz ütközés, és így eltávolítja NetworkManager a Linux ügynök csomagot telepítésekor.


## <a name="general-linux-system-requirements"></a>Általános Linux rendszerkövetelményei ##

- Módosítsa a kernel indítási sor LÁRVAJÁRAT vagy GRUB2, amelyet fel szeretne venni a következő paraméterek. Ez biztosítja az első, segítheti Azure soros portot összes konzol üzenetek fogadására problémák hibakeresési támogatása:

        console=ttyS0,115200n8 earlyprintk=ttyS0,115200 rootdelay=300

    Ez biztosítja az első, segítheti Azure soros portot összes konzol üzenetek fogadására problémák hibakeresési támogatás.

    A fenti kívül ajánlott *eltávolítása* a következő paraméterek ha vannak ilyenek:

        rhgb quiet crashkernel=auto

    A grafikus és csendes indítási sem lehet hasznos, ha azt szeretné, hogy a soros porthoz küldeni a naplókat felhőalapú környezetben.

    A `crashkernel` beállítás lehet balra beállítva, ha azt szeretné, de a megjegyzést, hogy a paraméter csökkenti a rendelkezésre álló memória található a virtuális 128MB vagy több, amely lehet a virtuális kisebb méretű a hibás.

- Az Azure Linux ügynök telepítése

    Az Azure Linux ügynök szükség a kiépítési Azure Linux képként.  Sok terjesztését a agent nyújthat, mint egy RPM vagy Deb csomag (a csomag neve általában "WALinuxAgent" vagy "walinuxagent").  A agent is telepíthető manuálisan a [Linux ügynök útmutató](virtual-machines-linux-agent-user-guide.md)lépéseit követve.

- Győződjön meg arról, hogy a SSH-kiszolgáló telepítve és beállítva rendszerindításkor indításához.  Ez általában az alapértelmezett érték.

- Ne hozzon létre felcserélése helyet a lemezen OS

    Az Azure Linux ügynök automatikusan konfigurálhatja felcserélése hely a helyi erőforrás lemezzel, amelyhez a Azure kiépítése után a virtuális kapcsolódik. Ne feledje, hogy a helyi erőforrás lemez *ideiglenes* lemez, és előfordulhat, hogy közben kiürítette, amikor a virtuális leépítjük. Miután telepítettem az Azure Linux ügynök (lásd az előző lépést), a következő paraméterek /etc/waagent.conf megfelelően módosítása:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

- Utolsó lépésként, a következő parancsokat a virtuális gép deprovision:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

    >[AZURE.NOTE] A Virtualbox jelenhetnek meg a következő hiba futtatása után "waagent-kényszerítése - deprovision": `[Errno 5] Input/output error`. Ez a hibaüzenet nem kritikus, és nyugodtan figyelmen kívül hagyható.

- Szüksége lesz majd, állítsa le a virtuális gép, és töltse fel a virtuális Azure.
