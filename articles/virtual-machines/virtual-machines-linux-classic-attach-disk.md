<properties
    pageTitle="Lemezen csatolása egy Linux virtuális |} Microsoft Azure"
    description="Megtudhatja, hogyan csatolhat adatok lemezen egy Azure virtuális gépen futó Linux és inicializálni, így a használatra kész."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/23/2016"
    ms.author="iainfou"/>

# <a name="how-to-attach-a-data-disk-to-a-linux-virtual-machine"></a>Hogyan csatolhat adatok lemezen Linux virtuális gépen

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Lásd: hogyan [csatolhat az erőforrás-kezelő telepítési modell adatainak lemezen](virtual-machines-linux-add-disk.md).

Üres lemez és a Azure VMs adatokat tartalmazó lemezt is csatolhat. Lemezen kétféle a .vhd fájlok Azure tároló fiók tárolnak. Mint bármelyik lemezre hozzáadása egy Linux számítógépre való után a lemez csatlakoztatásához kell inicializálni, és így a használatra kész formázza. Ez a cikk részletei üres lemez és a már tartalmazó a VMs, valamint hogyan inicializálni, majd az új lemez formázása adatok csatolásával.

> [AZURE.NOTE]A legjobb megoldás egy vagy több külön lemez használata egy virtuális gép adatok tárolására. Amikor létrehoz egy Azure virtuális gép, rajta az operációs rendszer és egy ideiglenes lemezzel. **Ne használja a ideiglenes lemez állandó adatok tárolására.** Foglalja, csak az ideiglenes tárolási biztosít. Is kínál nincs redundancia vagy a biztonsági másolat mivel Azure tárhely azt nem találhatók.
> Az ideiglenes lemez általában az Azure Linux ügynök kezeli, és automatikusan csatlakoztatott **/mnt/resource** (vagy **/mnt** Ubuntu képek). Kézzel, adatok lemezen neve lehet a Linux kernel hasonló `/dev/sdc`, és meg kell partition, formázása és csatlakoztatása az erőforrás. Olvassa el az [Azure Linux ügynök útmutatója] [ Agent] további információt.

[AZURE.INCLUDE [howto-attach-disk-windows-linux](../../includes/howto-attach-disk-linux.md)]

## <a name="initialize-a-new-data-disk-in-linux"></a>Új adatok lemezen a Linux inicializálni

1. A virtuális gép SSH. További részletek megtudhatja, [hogy miként jelentkezzen be egy virtuális gép Linux operációs rendszert futtató][Logon].

2. Következő meg kell keresnie a inicializálni adatok lemez eszközazonosítót. Ennek két módja van:

    a) Grep SCSI készülékén a naplókat, ilyen például az alábbi parancsot:

            $sudo grep SCSI /var/log/messages

    A legutóbbi Ubuntu terjesztését, előfordulhat, hogy szüksége `sudo grep SCSI /var/log/syslog` , mert a naplózás `/var/log/messages` esetleg letiltott alapértelmezés szerint.

    Az utolsó adatok lemezt a megjelenített üzenetek felvett azonosítója is megkeresheti.

    ![A lemez üzenetek lekérése](./media/virtual-machines-linux-classic-attach-disk/scsidisklog.png)

    VAGY

    b) használata a `lsscsi` megtudhatja, hogy az eszközazonosítót parancsot. `lsscsi`bármelyik telepíthető `yum install lsscsi` (piros-kalap alapján terjesztését) vagy `apt-get install lsscsi` (Debian alapján terjesztését). A _logikai_ vagy **logikai egységet**keres lemezre is megkeresheti. Például a _logikai_ csatolt lemezt egyszerűen látható legyen a `azure vm disk list <virtual-machine>` szerint:

            ~$ azure vm disk list TestVM
            info:    Executing command vm disk list
            + Fetching disk images
            + Getting virtual machines
            + Getting VM disks
            data:    Lun  Size(GB)  Blob-Name                         OS
            data:    ---  --------  --------------------------------  -----
            data:         30        TestVM-2645b8030676c8f8.vhd  Linux
            data:    0    100       TestVM-76f7ee1ef0f6dddc.vhd
            info:    vm disk list command OK

    Ezeket az adatokat a kimenő összehasonlítása `lsscsi` esetében az azonos mintában virtuális gépen:

            ops@TestVM:~$ lsscsi
            [1:0:0:0]    cd/dvd  Msft     Virtual CD/ROM   1.0   /dev/sr0
            [2:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sda
            [3:0:1:0]    disk    Msft     Virtual Disk     1.0   /dev/sdb
            [5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc

    A minden sorban a rekord utolsó értéke a _logikai_. Lásd: `man lsscsi` további információt.

3. A parancssorba írja be az eszköz létrehozása a következő parancsot:

        $sudo fdisk /dev/sdc


4. Amikor a rendszer kéri, írja be az **n** létrehoz egy új partíciót.


    ![Eszköz létrehozása](./media/virtual-machines-linux-classic-attach-disk/fdisknewpartition.png)

5. Amikor a rendszer kéri, írja be a **p** , hogy a partíciót az elsődleges partíciót. Írja be az **1** , hogy az első partíciót legyen, és fogadja el az alapértelmezett érték a henger típusa írja. Egyes rendszerek akkor megjelenítheti az alapértelmezett értékeket az első és utolsó ágazatok, a henger helyett. Megadhatja, fogadja el ezeket az alapértelmezett beállításokat.


    ![Partition létrehozása](./media/virtual-machines-linux-classic-attach-disk/fdisknewpartdetails.png)



6. Írja be a **p** particionálva a van lemez adatainak megjelenítéséhez.


    ![Lista lemez adatai](./media/virtual-machines-linux-classic-attach-disk/fdiskpartitiondetails.png)



7. Írja be a **w** írni a lemez beállításait.


    ![Írja be a lemez változik](./media/virtual-machines-linux-classic-attach-disk/fdiskwritedisk.png)

8. Most már elkészítheti a fájlrendszerben az új partíciót a. Lehetőség van a számát a Eszközazonosítót (az alábbi példában `/dev/sdc1`). Az alábbi példa létrehoz egy ext4 partíciót /dev/sdc1:

        # sudo mkfs -t ext4 /dev/sdc1

    ![Hozzon létre a fájlrendszerben](./media/virtual-machines-linux-classic-attach-disk/mkfsext4.png)

    >[AZURE.NOTE] SuSE Linux vállalati 11 rendszerek ext4 fájlrendszer csak támogatása csak olvasási hozzáférést. Ezek a rendszerek ajánlott formázza az új fájlrendszer ext4, hanem ext3.


9. Végezze el az alábbi képlettel történik csatlakoztatása az új fájlrendszer könyvtár:

        # sudo mkdir /datadrive


10. Végül akkor is csatlakoztatható a meghajtóba, az alábbi képlettel történik:

        # sudo mount /dev/sdc1 /datadrive

    Az adatok lemezre készen áll a **/datadrive**használni.
    
    ![A könyvtár létrehozásához, és csatlakoztassa a lemez](./media/virtual-machines-linux-classic-attach-disk/mkdirandmount.png)


11. Adja hozzá az új meghajtó /etc/fstab:

    Annak érdekében, hogy a meghajtó újraindítása után automatikusan csatlakoztatni azt hozzá kell adnia a/stb/fstab fájlt. Ezeken kívül ajánlott szolgál, hogy a UUID (általánosan egyedi azonosító) /etc/fstab hivatkozni csak az eszköz nevét (azaz /dev/sdc1) helyett. A UUID használatával elkerülhető a helytelen lemezre alatt csatlakoztatott egy adott helyre, ha az operációs rendszer a lemez hiba észleli az indítási és bármilyen fennmaradó adatok lemezre, majd hozzárendeli ezeket eszköz azonosítók során. Keresse meg az új meghajtó UUID, használhatja a **blkid** segédprogram:

        # sudo -i blkid

    A kimeneti formátumban a következőhöz hasonló:

        /dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
        /dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
        /dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"


    >[AZURE.NOTE] Helytelenül szerkesztése a **/etc/fstab** fájlt egy diagnosztizálását rendszer eredményezhet. Ha nem tudja, hogy, keresse meg az eloszlás dokumentációjában megfelelően szerkesztése: ezt a fájlt. Ajánlott is, hogy a biztonsági másolatot a /etc/fstab fájl szerkesztése előtt jön létre.

    Ezután nyissa meg a **/etc/fstab** fájlt egy szövegszerkesztőben:

        # sudo vi /etc/fstab

    Ebben a példában az új **/dev/sdc1** eszköz az előző lépéseket, és a csatlakozási pont **/datadrive**az hozza létre azt be az UUID értéket. Adja hozzá a következő sort a **/etc/fstab** fájl végére:

        UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,nofail   1   2

    Vagy alapján SuSE Linux rendszerben szüksége lehet némileg eltérő formázással:

        /dev/disk/by-uuid/33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext3   defaults,nofail   1   2
    
    >[AZURE.NOTE] A `nofail` lehetőség biztosítja, hogy a virtuális elindul, akkor is, ha a fájlrendszer sérült, vagy a lemez rendszerindításkor nem létezik. Anélkül, hogy ez a beállítás viselkedés ismertetett módon [Nem SSH Linux virtuális gép FSTAB hibák miatt](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/)merülhetnek fel.

    Most már ellenőrizheti, hogy a a fájlrendszer megfelelően van-e csatlakoztatva leválasztása, és ezután remounting tehát a fájlrendszerben a példa használatával csatlakoztatási pontot `/datadrive` az előző lépésben létrehozott:

        # sudo umount /datadrive
        # sudo mount /datadrive

    Ha a `mount` parancs hibát eredményez, jelölje be a helyes szintaxis/stb/fstab fájlt. Ha további adatokat meghajtók vagy partíciók hozott létre, és egyéb elemek/fstab külön-külön, valamint kezdenek őket.

    Teheti a meghajtó írható ezzel a paranccsal:

        # sudo chmod go+w /datadrive

>[AZURE.NOTE] Ezt követően eltávolítása az adatok lemezen fstab módosítása nélkül okozhat a virtuális meghiúsító indításáról. Ha egy gyakori előfordulás, a legtöbb terjesztését adja meg vagy a `nofail` és/vagy `nobootwait` fstab beállításokat, amelyek lehetővé teszik a rendszert, ha a lemez nem sikerült csatlakoztatni a indítsa el időt. További információt a paramétereket a terjesztési dokumentációjában.

### <a name="trimunmap-support-for-linux-in-azure"></a>A TRIM/UNMAP támogatása Linux Azure-ban
Néhány Linux mag támogatják a nem használt blokkolja a lemezen törölni kívánja a TRIM/UNMAP műveleteket. Ezek a műveletek elsősorban hasznosak szabványos tároló tájékoztatja a Törölt lapok Azure már nem érvényesek, és figyelmen kívül hagyható. Lapok elvetésével költség mentheti, ha nagyméretű fájlok létrehozása és törölheti őket.

Vannak olyan, a Linux virtuális kétféleképpen a TRIM ahhoz, hogy támogatást. A szokásos módon olvassa el a terjesztési a javasolt megközelítés:

- Használja a `discard` csatlakozási lehetőség `/etc/fstab`, például:

        UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,discard   1   2

- Azt is megteheti, hogy futtatását is lehetővé teszi a `fstrim` a parancssorból manuálisan parancsot, vagy vegye fel a crontab rendszeresen futtatásához:

    **Ubuntu**

        # sudo apt-get install util-linux
        # sudo fstrim /datadrive

    **RHEL/CentOS**

        # sudo yum install util-linux
        # sudo fstrim /datadrive

## <a name="troubleshooting"></a>Hibaelhárítás
[AZURE.INCLUDE [virtual-machines-linux-lunzero](../../includes/virtual-machines-linux-lunzero.md)]


## <a name="next-steps"></a>Következő lépések
Erről további a Linux virtuális használatáról az alábbi cikkekben:

- [Virtuális géphez Linux operációs rendszert futtató bejelentkezés][Logon]

- [Hogyan kell leválasztani Linux virtuális számítógépről lemezen](virtual-machines-linux-classic-detach-disk.md)

- [A klasszikus telepítési modell az Azure CLI használata](../virtual-machines-command-line-tools.md)

<!--Link references-->
[Agent]: virtual-machines-linux-agent-user-guide.md
[Logon]: virtual-machines-linux-mac-create-ssh-keys.md
