<properties
    pageTitle="Adja hozzá a lemezt Linux virtuális |} Microsoft Azure"
    description="Ismerkedjen meg az állandó lemezen hozzáadása a Linux virtuális"
    keywords="Linux virtuális gép lemez erőforrás hozzáadása"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="rickstercdn"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager" />

<tags
    ms.service="virtual-machines-linux"
    ms.topic="article"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.date="09/06/2016"
    ms.author="rclaus"/>

# <a name="add-a-disk-to-a-linux-vm"></a>Lemezen hozzáadása egy Linux virtuális

Ebből a cikkből megtudhatja, hogyan csatolhat állandó lemezen a virtuális, hogy megőrizhető az adatok – még akkor is, ha a virtuális van reprovisioned karbantartási vagy átméretezése miatt. Lemezen, szeretne felvenni [Az Azure CLI](../xplat-cli-install.md) erőforrás-kezelő módban konfigurált (`azure config mode arm`).  

## <a name="quick-commands"></a>Gyors parancsok

Az alábbi példák parancs cserélje ki az értékek közötti &lt; és &gt; saját környezetből származó értékekkel.

```bash
azure vm disk attach-new <myuniquegroupname> <myuniquevmname> <size-in-GB>
```

## <a name="attach-a-disk"></a>Lemezen csatolása

Az új lemez csatolása a gyors. Típus `azure vm disk attach-new <myuniquegroupname> <myuniquevmname> <size-in-GB>` hozhat létre, és csatolja az új GB lemezen a virtuális. Nem kifejezetten azonosítja a tárterület-fiókkal, ha bármelyik létrehozott lemezre küldi ugyanazzal a tárterület-fiókkal az operációs rendszer lemez helye.  Azt hasonlóan kell kinéznie a következőket:

```bash
azure vm disk attach-new myuniquegroupname myuniquevmname 5
```

Kimenet

```bash
info:    Executing command vm disk attach-new
+ Looking up the VM "myuniquevmname"
info:    New data disk location: https://cliexxx.blob.core.windows.net/vhds/myuniquevmname-20150526-0xxxxxxx43.vhd
+ Updating VM "myuniquevmname"
info:    vm disk attach-new command OK
```

## <a name="connect-to-the-linux-vm-to-mount-the-new-disk"></a>Az új lemezre csatlakoztatása a Linux virtuális csatlakoztatása

> [AZURE.NOTE] Ez a témakör a virtuális felhasználóneveket és jelszavakat használatával csatlakozik. Nyilvános és titkos kulcs párban használatával kommunikál a virtuális, megtudhatja, [hogy miként használhatja SSH Linux Azure a](virtual-machines-linux-mac-create-ssh-keys.md). Módosíthatja a létrehozott VMs **SSH** kapcsolatot a `azure vm quick-create` parancs használatával a `azure vm reset-access` **SSH** access teljesen alaphelyzetbe, hozzáadása vagy távolítsa el a felhasználók és biztonságos hozzáférés nyilvános kulcs fájlok hozzáadása parancsot.

Meg kell SSH az Azure virtuális partíciót a be, formázása és csatlakoztatása az új lemez, hogy a Linux virtuális felhasználhassa. Ha nem ismeri a összekapcsolása **ssh**, a parancs megnyitja az űrlapot `ssh <username>@<FQDNofAzureVM> -p <the ssh port>`, és az alábbiakhoz hasonlóan néz ki:

```bash
ssh ops@myuni-westu-1432328437727-pip.westus.cloudapp.azure.com -p 22
```

Kimenet

```bash
The authenticity of host 'myuni-westu-1432328437727-pip.westus.cloudapp.azure.com (191.239.51.1)' can't be established.
ECDSA key fingerprint is bx:xx:xx:xx:xx:xx:xx:xx:xx:x:x:x:x:x:x:xx.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'myuni-westu-1432328437727-pip.westus.cloudapp.azure.com,191.239.51.1' (ECDSA) to the list of known hosts.
ops@myuni-westu-1432328437727-pip.westus.cloudapp.azure.com's password:
Welcome to Ubuntu 14.04.2 LTS (GNU/Linux 3.16.0-37-generic x86_64)

* Documentation:  https://help.ubuntu.com/

System information as of Fri May 22 21:02:32 UTC 2015

System load: 0.37              Memory usage: 2%   Processes:       207
Usage of /:  41.4% of 1.94GB   Swap usage:   0%   Users logged in: 0

Graph this data and manage this system at:
  https://landscape.canonical.com/

Get cloud support with Ubuntu Advantage Cloud Guest:
  http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

ops@myuniquevmname:~$
```

Most, hogy a virtuális hálózathoz kapcsolódik, készen áll a lemez csatolni.  Először nyissa meg a lemezt, használatával `dmesg | grep SCSI` (a módszerrel is az új lemez felfedezése változhat). Ebben az esetben az alábbihoz hasonló:

```bash
dmesg | grep SCSI
```

Kimenet

```bash
[    0.294784] SCSI subsystem initialized
[    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
[    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
[    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
[ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
```

és ez a témakör a `sdc` lemez szeretnénk, amelyiket. Most már partition rendelkező lemez `sudo fdisk /dev/sdc` --feltételezve, hogy a lemez volt az eset `sdc`, legyen elsődleges lemezen a partition 1 és fogadja el az alapértelmezett beállításokat.

```bash
sudo fdisk /dev/sdc
```

Kimenet

```bash
Device contains neither a valid DOS partition table, nor Sun, SGI or OSF disklabel
Building a new DOS disklabel with disk identifier 0x2a59b123.
Changes will remain in memory only, until you decide to write them.
After that, of course, the previous content won't be recoverable.

Warning: invalid flag 0x0000 of partition table 4 will be corrected by w(rite)

Command (m for help): n
Partition type:
   p   primary (0 primary, 0 extended, 4 free)
   e   extended
Select (default p): p
Partition number (1-4, default 1): 1
First sector (2048-10485759, default 2048):
Using default value 2048
Last sector, +sectors or +size{K,M,G} (2048-10485759, default 10485759):
Using default value 10485759
```

Hozza létre a partíciót beírásával `p` a parancssorba:

```bash
Command (m for help): p

Disk /dev/sdc: 5368 MB, 5368709120 bytes
255 heads, 63 sectors/track, 652 cylinders, total 10485760 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x2a59b123

   Device Boot      Start         End      Blocks   Id  System
/dev/sdc1            2048    10485759     5241856   83  Linux

Command (m for help): w
The partition table has been altered!

Calling ioctl() to re-read partition table.
Syncing disks.
```

És a partíciót a fájlrendszer írási parancsával **mkfs** , a fájlrendszer típusát, és az eszköz nevét. Ez a témakör a programmal mutatjuk be `ext4` és `/dev/sdc1` a fentiektől:

```bash
sudo mkfs -t ext4 /dev/sdc1
```

Kimenet

```bash
mke2fs 1.42.9 (4-Feb-2014)
Discarding device blocks: done
Filesystem label=
OS type: Linux
Block size=4096 (log=2)
Fragment size=4096 (log=2)
Stride=0 blocks, Stripe width=0 blocks
327680 inodes, 1310464 blocks
65523 blocks (5.00%) reserved for the super user
First data block=0
Maximum filesystem blocks=1342177280
40 block groups
32768 blocks per group, 32768 fragments per group
8192 inodes per group
Superblock backups stored on blocks:
    32768, 98304, 163840, 229376, 294912, 819200, 884736
Allocating group tables: done
Writing inode tables: done
Creating journal (32768 blocks): done
Writing superblocks and filesystem accounting information: done
```

Most hozzunk létre használatával fájlt rendszer csatlakoztatásához könyvtárában `mkdir`:

```bash
sudo mkdir /datadrive
```

És a címtár-használatával csatlakoztatása `mount`:

```bash
sudo mount /dev/sdc1 /datadrive
```

Az adatok lemez készen áll a használja az `/datadrive`.

```bash
ls
```

Kimenet

```bash
bin   datadrive  etc   initrd.img  lib64       media  opt   root  sbin  sys  usr  vmlinuz
boot  dev        home  lib         lost+found  mnt    proc  run   srv   tmp  var
```

Annak érdekében, hogy a meghajtó újraindítása után automatikusan csatlakoztatni azt hozzá kell adnia a/stb/fstab fájlt. Ezeken kívül erősen ajánlott szolgál, hogy a UUID (általánosan egyedi azonosító) / stb/fstab csak az eszköz neve helyett hivatkozik (például: `/dev/sdc1`). Az operációs rendszer a lemez hiba rendszerindításkor azt észleli, ha UUID használatával elkerülhető a helytelen lemez, egy adott helyre folyamatban van. Adatok lemezt hátralévő szeretné majd kiosztani e azonos eszköz azonosítóját. Az új meghajtó UUID megkereséséhez használja a **blkid** segédprogram:

```bash
sudo -i blkid
```

A kimeneti formátumban a következőhöz hasonló:

```bash
/dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
/dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
/dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
```

>[AZURE.NOTE] Helytelenül szerkesztése a **/etc/fstab** fájlt egy diagnosztizálását rendszer eredményezhet. Ha nem tudja, hogy, keresse meg az eloszlás dokumentációjában megfelelően szerkesztése: ezt a fájlt. Ajánlott is, hogy a biztonsági másolatot a /etc/fstab fájl szerkesztése előtt jön létre.

Ezután nyissa meg a **/etc/fstab** fájlt egy szövegszerkesztőben:

```bash
sudo vi /etc/fstab
```

Ebben a példában az új **/dev/sdc1** eszköz az előző lépéseket, és a csatlakozási pont **/datadrive**az hozza létre azt be az UUID értéket. Adja hozzá a következő sort a **/etc/fstab** fájl végére:

```bash
UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,nofail   1   2
```

>[AZURE.NOTE] Később eltávolítása az adatok lemezen fstab módosítása nélkül okozhat a virtuális indításáról sikertelen lesz. A legtöbb terjesztését adja meg, vagy a `nofail` és/vagy `nobootwait` fstab beállítások. Ezeket a beállításokat a rendszert, ha a lemez nem sikerült csatlakoztatni rendszerindításkor lehetővé. További információt a paramétereket a terjesztési dokumentációjában.

>[AZURE.NOTE] A **nofail** beállítás biztosítja, hogy a virtuális elindul, akkor is, ha a fájlrendszer sérült, vagy a lemez rendszerindításkor nem létezik. Anélkül, hogy ezt a lehetőséget előfordulhatnak viselkedés ismertetett módon [Nem SSH Linux virtuális gép FSTAB hibák miatt](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/)


### <a name="trimunmap-support-for-linux-in-azure"></a>A TRIM/UNMAP támogatása Linux Azure-ban
Néhány Linux mag támogatják a nem használt blokkolja a lemezen törölni kívánja a TRIM/UNMAP műveleteket. Ez akkor elsősorban hasznos szabványos tárolója tájékoztatja a Törölt lapok Azure már nem érvényesek, és figyelmen kívül hagyható. A költség mentheti, ha nagyméretű fájlok létrehozása és törölheti őket.

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

- Ne feledje, hogy, hogy az új lemez nem érhető el a virtuális gép, ha azt újraindul, kivéve, ha ezeket az információkat, írja be a [fstab](http://en.wikipedia.org/wiki/Fstab) fájlhoz.
- Annak érdekében, hogy helyesen van beállítva a Linux virtuális, olvassa el az [Optimize Linux gépi teljesítményének](virtual-machines-linux-optimization.md) ajánlások.
- Bontsa ki a tárolókapacitású hozzáadásával további lemez és [RAID konfigurálása](virtual-machines-linux-configure-raid.md) a további teljesítmény elérése érdekében.
