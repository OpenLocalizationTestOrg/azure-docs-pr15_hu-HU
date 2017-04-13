<properties
    pageTitle="Optimalizálás a Azure virtuális Linux gép |} Microsoft Azure"
    description="Ismerje meg, bizonyos teljesítményoptimalizálási tippeket, hogy a Linux virtuális az optimális teljesítmény eléréséhez a Azure be van állítva"
    keywords="Linux virtuális gép virtuális gép linux, ubuntu virtuális gépen" 
    services="virtual-machines-linux"
    documentationCenter=""
    authors="rickstercdn"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="rclaus"/>

# <a name="optimize-your-linux-vm-on-azure"></a>Az Azure virtuális Linux gép optimalizálása

Linux virtuális gép (virtuális) létrehozása nagyon egyszerűen végezze el a parancssorból vagy a portálon. Ebből az oktatóanyagból megtudhatja, hogy miként, annak érdekében, hogy be van állítva, optimalizálhatja a a Microsoft Azure platform teljesítményét. Ez a témakör használja egy Ubuntu kiszolgáló virtuális, de [saját képeket, sablonok](virtual-machines-linux-create-upload-generic.md)használata Linux virtuális gép is létrehozhat.  

## <a name="prerequisites"></a>Előfeltételek

Ez a témakör azt feltételezi, hogy már rendelkezik működő [telepítve van az Azure CLI](../xplat-cli-install.md) Azure-előfizetés ([ingyenes próba-előfizetés](https://azure.microsoft.com/pricing/free-trial/)), és van már kiépítve egy virtuális az Azure-előfizetését. Mielőtt bármilyen fájlok az Azure - ezzel van-előfizetéséhez hitelesítést végezni. Ehhez az Azure CLI, egyszerűen írja be a `azure login` az interaktív megkezdéséhez. 

## <a name="azure-os-disk"></a>Azure OS lemez

Miután létrehozott egy Linux virtuális Azure-ban, két lemez társítva van. /dev/SDA OS lemezterületet, a /dev/sdb az ideiglenes lemez.  Ne használja a fő OS lemez (/ fejlesztők/sda) az összes, kivéve az operációs rendszer van optimalizálva gyors virtuális rendszer indításakor, és nem nyújt a munkaterhelésekből a jó teljesítményt. Egy vagy több lemezre csatolása a virtuális állandó lekérdezni kívánt és az adatok tárterület optimalizált. 

## <a name="adding-disks-for-size-and-performance-targets"></a>Lemezen mérete és teljesítménye célok felvétele 

Virtuális mérete alapján, csatolhat-A-sorozat, a D-sorozat 32 lemezre a lemezt legfeljebb 16, és a 64 lemezt egy G sorozat a számítógép - minden legfeljebb 1 TB méretű. Akkor további lemezt szükség szerint adjon meg egy IOps követelmények és a terület. Minden egyes lemez van egy célul 500 IOps a szabványos tárolására és felfelé egy lemezen 5000 IOps prémium tárolására.  Prémium tároló lemezt kapcsolatos további tudnivalókért tekintse át [prémium tároló: Azure VMs nagy teljesítményű tárterület](../storage/storage-premium-storage.md)

A legnagyobb IOps, ahol a gyorsítótár-beállítások állította "Írásvédett" vagy "Nincs" prémium tároló lemezen eléréséhez a "korlátok" tiltsa le a fájlrendszer Linux csatlakoztatása közben. Mivel a biztonsági prémium tároló lemezre írása az alábbi gyorsítótár beállítások tartós szükségtelen korlátok.

- Ha **reiserFS**, tiltsa le a korlátok csatlakoztatási parancs "korlát = nincs" (engedélyezése a korlátok, használja a "korlát ürítése =")
- Ha **ext3/ext4**, tiltsa le a korlátok csatlakoztatási parancs "korlát = 0" (engedélyezése a korlátok, használja a "korlát = 1")
- Ha **XFS**, tiltsa le a korlátok csatlakoztatási parancs "nobarrier" (engedélyezése a korlátok, használja a beállítás "korlát")

## <a name="storage-account-considerations"></a>Tárterület fiókkal kapcsolatos szempontok

A Linux virtuális Azure-ban létrehozott, gondoskodnia kell arról lemezre csatol ugyanabban a régióban, mint a virtuális közelében biztosítása, és minimalizálhatja hálózati késés tartózkodó tárterület-fiókból.  Minden egyes szabványos tárterület-fióknak legfeljebb 20 k IOps és mérete 500 TB kapacitása.  Ez a módszer körülbelül 40 erősen használt lemezre, beleértve az operációs rendszer lemez és a bármilyen adat lemezt hoz létre. Prémium tárterület-fiókok esetén nem maximális IOps korlátozott, de 32 TB méret korlátozott. 

Kezelésének magas IOps munkaterhelésének és azt választotta, hogy szabványos tárhely a lemezen, amikor szükség lehet a lemez felosztása több tárterületet fiókot, hogy a nem gombra kattint a szabványos tároló fiókok 20 000 IOps korlátját keresztül. A virtuális tartalmazhat kombinálja a lemezt a különböző tároló fiókok és tároló fióktípusok az optimális konfigurációt eléréséhez. 

## <a name="your-vm-temporary-drive"></a>A virtuális ideiglenes meghajtóra

Alapértelmezés szerint egy virtuális gép létrehozásakor Azure biztosít-OS (/ fejlesztők/sda) és egy ideiglenes lemezzel (/ fejlesztők/sdb).  Az összes további lemez hozzáadása megjelennek majd /dev/sdc, /dev/sdd, /dev/sde és így tovább. Az ideiglenes lemez (/ fejlesztők/sdb) lévő összes adatot nem tartós, és elveszhet, ha bizonyos eseményeket, például virtuális gép átméretezés, újratelepítés, vagy a karbantartás újraindítja a virtuális.  A méret és az ideiglenes lemez típusú kapcsolódó kiválasztott telepítési egyszerre virtuális méretét. A prémium mérete VMs (DS, G és DS_V2 adatsor) közül az esetében az ideiglenes meghajtó biztonsági egy helyi SSD legfeljebb 48 k további teljesítmény IOps. 

## <a name="linux-swap-file"></a>Linux lapozó fájl

Ha az Azure virtuális Ubuntu vagy CoreOS képet, majd küldje el a felhőben config felhő nicializálás CustomData is használhatja. Ha [egy egyéni Linux képet feltölteni](virtual-machines-linux-upload-vhd.md) felhő nicializálás használó, beállíthatja a is felcserélése partíciót felhő nicializálás használatával.

Ubuntu felhő képek, a felhőben nicializálás konfigurálása a felcserélése partíciót kell használnia. A megjelenő lap lássanak további részleteket a Ubuntu wiki: [AzureSwapPartitions](https://wiki.ubuntu.com/AzureSwapPartitions).

Képek felhő nicializálás támogatás nélkül virtuális telepítették a Microsoft Azure piactéren lévő képeknek egy virtuális Linux ügynök az operációs rendszer integrálva. Ez az ügynök lehetővé teszi, hogy a különböző Azure szolgáltatások vezérléséhez virtuális. Feltételezve, hogy egy szokásos képet ábrázoló a Microsoft Azure piactéren lévő telepítette, kimutatásadatokat megfelelően konfigurálása a Linux felcserélése beállításai a következőképpen:

Keresse meg és módosíthatja a **/etc/waagent.conf** fájlban két bejegyzések. Azok az ellenőrzés dedikált felcserélése fájl megléte és az ideiglenes fájl méretét. A paraméterek módosításának keresi `ResourceDisk.EnableSwap=N` és`ResourceDisk.SwapSizeMB=0` 

Kell a következőképpen módosíthatja:

* ResourceDisk.EnableSwap=Y
* Az igényeknek MB ResourceDisk.SwapSizeMB={size} 

Után a változtatások saját, meg kell indítsa újra a waagent vagy a Linux virtuális a változások követése érdekében.  Tudja, hogy a módosítások végrehajtását és felcserélése fájl használatakor jött létre a `free` szabad terület megjelenítése parancsot. Az alábbi példában a waagent.conf fájl módosítása eredményeként létrehozott 512MB felcserélése fájl tartalmaz.

    admin@mylinuxvm:~$ free
                total       used       free     shared    buffers     cached
    Mem:       3525156     804168    2720988        408       8428     633192
    -/+ buffers/cache:     162548    3362608
    Swap:       524284          0     524284
    admin@mylinuxvm:~$
 
## <a name="io-scheduling-algorithm-for-premium-storage"></a>I/O ütemezési algoritmus prémium tárolására

Az a 2.6.18 Linux kernel, az alapértelmezett I/O algoritmus ütemezése módosult a határidőt, a CFQ (teljesen valós várólista algoritmus). Elérésű I/O mintázatok a CFQ és a határidő teljesítmény eltérések a elhanyagolható különbség van.  A hol főleg egymást követő-e a lemez I/O minta SSD-alapú lemez esetében átirányítása a NOOP vagy határidő algoritmus érhet el jobb I/O teljesítményt.

### <a name="view-the-current-io-scheduler"></a>Az aktuális I/O ütemező megtekintése

A következő parancsot használja:  

    admin@mylinuxvm:~# cat /sys/block/sda/queue/scheduler

Kimenet, amely jelzi az aktuális ütemező követően jelenik meg.  

    noop [deadline] cfq

###<a name="change-the-current-device-devsda-of-io-scheduling-algorithm"></a>Az aktuális eszköz (/ fejlesztők/sda) i/o-algoritmus ütemezésének módosítása

Az alábbi parancsokat használhatja:  

    azureuser@mylinuxvm:~$ sudo su -
    root@mylinuxvm:~# echo "noop" >/sys/block/sda/queue/scheduler
    root@mylinuxvm:~# sed -i 's/GRUB_CMDLINE_LINUX=""/GRUB_CMDLINE_LINUX_DEFAULT="quiet splash elevator=noop"/g' /etc/default/grub
    root@mylinuxvm:~# update-grub

>[AZURE.NOTE] Nem lehet hasznos a beállítást a /dev/sda egyedül áll. Szeretné állítani az összes adat lemez hol egymás után következő I/O uralja-e a I/O minta szüksége van.  

Meg kell jelennie a következő eredményt, jelezve, hogy grub.cfg újraépíti sikeresen és, hogy az alapértelmezett ütemező NOOP frissült.  

    Generating grub configuration file ...
    Found linux image: /boot/vmlinuz-3.13.0-34-generic
    Found initrd image: /boot/initrd.img-3.13.0-34-generic
    Found linux image: /boot/vmlinuz-3.13.0-32-generic
    Found initrd image: /boot/initrd.img-3.13.0-32-generic
    Found memtest86+ image: /memtest86+.elf
    Found memtest86+ image: /memtest86+.bin
    done

A Redhat terjesztési család számára csak akkor van szüksége a következő parancsot:   

    echo 'echo noop >/sys/block/sda/queue/scheduler' >> /etc/rc.local

## <a name="using-software-raid-to-achieve-higher-iops"></a>Szoftver RAID használata eléréséhez magasabb lehet / Ops

Ha a feladatok további IOps egyetlen lemez nyújthat, mint kér, akkor több lemezre szoftver RAID konfiguráció használata. Azure már a helyi háló rétegben lemezre tűrőképessége hajt végre, mert a RAID-0 csíkozást konfiguráció a teljesítmény a legmagasabb szintű érhető el.  Kell rendelkezni és új lemez létrehozása az Azure környezetben és csatolása őket a Linux virtuális szétválasztás, formázása és a meghajtókon csatlakoztatása előtt.  További részleteket a RAID Szoftvertelepítés konfigurálása a Linux virtuális azure-ban a **[Szoftver RAID konfigurálása Linux](virtual-machines-linux-configure-raid.md)** dokumentumban található.


## <a name="next-steps"></a>Következő lépések

Ne feledje, az összes optimalizálási vitafórum, az ellenőrzések végrehajtásához előtt és után az ütközés áll elő, a változást mérésére egyes módosítások szükség szerint.  Optimalizálás a következő, eltérő eredményeket fog rendelkezni az környezetben egyes számítógépek közötti lépésenkénti áll.  Mi működik, az egyik konfigurációs mások számára nem működnek.

Néhány hasznos további forrásokra mutató hivatkozásokat: 

- [Prémium tárterület: Azure virtuális gép Munkaterhelésekből nagy teljesítményű tárterület](../storage/storage-premium-storage.md)
- [Azure Linux ügynök útmutatója](virtual-machines-linux-agent-user-guide.md)
- [Azure Linux VMs a MySQL-teljesítmény optimalizálása](virtual-machines-linux-classic-optimize-mysql.md)
- [A szoftver Linux RAID konfigurálása](virtual-machines-linux-configure-raid.md)
