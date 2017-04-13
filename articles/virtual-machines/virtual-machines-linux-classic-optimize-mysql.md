<properties
    pageTitle="Optimalizálhatja Linux VMs MySQL teljesítményét |} Microsoft Azure"
    description="Megtudhatja, hogyan optimalizálhatja a MySQL-gépen futó az Azure virtuális (virtuális) Linux operációs rendszert futtató."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="NingKuang"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/15/2015"
    ms.author="ningk"/>

#<a name="optimizing-mysql-performance-on-azure-linux-vms"></a>Azure Linux VMs a MySQL-teljesítmény optimalizálása

Számos tényező, amely a MySQL-teljesítményt Azure, kattintson a virtuális kiválasztása és a szoftver hardverkonfiguráció egyaránt. Ez a cikk a tárhely, a rendszer és az adatbázis konfigurációk – optimalizálás teljesítmény koncentrál.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


##<a name="utilizing-raid-on-an-azure-virtual-machine"></a>Az Azure virtuális gépen RAID hasznosítása
Tároló a kulcsfontosságú tényező, amely hatással van a felhőalapú környezetben adatbázis teljesítményét.  Egyetlen lemez képest, RAID feldolgozási gyorsabb hozzáférésének lehet nyújtani.  Olvassa el [Szabványos szintjei](http://en.wikipedia.org/wiki/Standard_RAID_levels) részletesen.   

Lemeztevékenység átviteli és Azure-ban I/O válaszidő jelentősen javítható RAID. A laboratóriumi vizsgálat jelenjen meg lemezen I/O kapacitása meg kell duplázni I/O válaszidő csökkenthető a fele átlagosan amikor kétszeres RAID lemezre száma (4-es, 4-8 2 stb.). [Mintája](#AppendixA) talál további információt.  

Lemeztevékenység, mellett a MySQL-teljesítmény javítja a Ha RAID növelése.  [B függelék](#AppendixB) talál további információt.  

Is érdemes figyelembe adattömb méretét. Az általános adattömb nagyobb méretű esetén kaphat alsó felsőbb, különösen a nagy írások. A adattömb mérete túl nagy, amikor adhat további terhelést, és a RAID kihasználhatja nem. Az aktuális alapértelmezett mérete 512KB, ami a legáltalánosabb gyártási környezetekben az optimális igazolt van. [Útmutatásai](#AppendixC) talál további információt.   

Felhívjuk a figyelmét arra, hogy nincsenek-e határértékeket is hozzáadhat típusai eltérő virtuális gép hány lemezen. Ezek a korlátok [virtuális gép és felhőalapú szolgáltatást méretű az Azure](http://msdn.microsoft.com/library/azure/dn197896.aspx)részletezi. 4 csatolt adatok lemezt kövesse az ebben a cikkben a RAID példa szüksége lesz, bár úgy lehetett dönt, hogy kevesebb lemezt RAID állítva.  

Ez a cikk azt feltételezi, hogy már létrehozott egy Linux virtuális gép és MYSQL telepítette és beállította. További információt az első lépések hivatkozzon az Azure MySQL telepítése.  

###<a name="setting-up-raid-on-azure"></a>A Azure RAID beállítása
A következő lépések bemutatják a Azure RAID létrehozása az Azure klasszikus portálon. Azt is beállíthatja RAID használata a Windows PowerShell-parancsfájlokat.
Ez a példa azt fog konfigurálni RAID 0 és 4 lemezt.  

####<a name="step-1-add-a-data-disk-to-your-virtual-machine"></a>Lépés: 1: A virtuális gép adatok lemezen hozzáadása  

Az Azure klasszikus portál virtuális gépeken futó lapján kattintson a virtuális gép adatok lemezen hozzáadni kívánt. Ebben a példában a virtuális gép mysqlnode1.  

![][1]

A virtuális gép lapján kattintson az **Irányítópult**.  

![][2]


A tevékenység sávon kattintson a **Csatolás**gombra.

![][3]

És kattintson a **Csatolás üres lemez**gombra.  

![][4]

Az adatok lemezt a **Host gyorsítótár preferencia** nincs értékre **állításával**meg kell.  

Ez egy üres lemez beszúrhat a virtuális gépet. Ismételje meg ezt a lépést három többször, hogy a 4 adatok lemezt RAID telepítve van.  

Megjeleníti az üzenetek kernelnaplót megjelenik a hozzáadott meghajtók a virtuális gépen. A Ubuntu jelenik meg, például használja a következő parancsot:  

    sudo grep SCSI /var/log/dmesg

####<a name="step-2-create-raid-with-the-additional-disks"></a>Lépés: 2: További lemezt RAID létrehozása
Kövesse az ebben a cikkben részletes RAID beállítási lépéseket:  

[A szoftver Linux RAID konfigurálása](virtual-machines-linux-configure-raid.md)

>[AZURE.NOTE] A XFS fájlrendszer használatakor RAID létrehozása után kövesse az alábbi lépéseket.

Debian, Ubuntu vagy Linux menta XFS telepítéséhez használja az alábbi parancsot:  

    apt-get -y install xfsprogs  

XFS a Fedora, CentOS vagy RHEL telepítéséhez használja az alábbi parancsot:  

    yum -y install xfsprogs  xfsdump


####<a name="step-3-set-up-a-new-storage-path"></a>Lépés 3: Állítsa be az új tároló elérési út
A következő parancsot használja:  

    root@mysqlnode1:~# mkdir -p /RAID0/mysql

####<a name="step-4-copy-the-original-data-to-the-new-storage-path"></a>Lépés: 4: Az új tároló elérési út az eredeti adatok másolása
A következő parancsot használja:  

    root@mysqlnode1:~# cp -rp /var/lib/mysql/* /RAID0/mysql/

####<a name="step-5-modify-permissions-so-mysql-can-access-read-and-write-the-data-disk"></a>Lépés az 5: Kapcsolatos engedélyek módosítása, MySQL hozzáférhet (olvasási és írási) az adatok lemez
A következő parancsot használja:  

    root@mysqlnode1:~# chown -R mysql.mysql /RAID0/mysql && chmod -R 755 /RAID0/mysql


##<a name="adjust-the-disk-io-scheduling-algorithm"></a>A lemez I/O ütemezési algoritmus módosítása
Linux négyféle i/o-algoritmusok ütemezési hajtja végre:  

-   NOOP algoritmus (nincs művelet)
-   Határidő módszerével (határidő)
-   Teljesen valós várólista módszerével (CFQ)
-   Tervezett időszak módszerével (Anticipatory)  

A teljesítmény optimalizálása a különböző forgatókönyvek különböző I/O bejegyzéstípusait is választhat. Teljesen elérésű környezetben nem áll egy nagy különbsége a CFQ és a határidő algoritmusok a teljesítmény elérése érdekében. A MySQL-adatbázis környezet beállításához stabilitás határidő általában javasolt. Ha egymást követő i/o-sok, CFQ csökkentheti a lemez I/O teljesítménye.   

SSD és más berendezések NOOP vagy határidő érhet el az alapértelmezett ütemező-nál nagyobb teljesítmény.   

A kernelből 2,5 az alapértelmezett I/O ütemezési algoritmus határidő. A kernelből 2.6.18 kezdve, CFQ vált, az alapértelmezett I/O ütemezési algoritmus.  Adja meg ezt a beállítást, a rendszerhéj-indítási idő, illetve módosítására ezt a beállítást, amikor a rendszer fut.  

A következő példa bemutatja, hogyan ellenőrizheti és az alapértelmezett ütemező beállíthatja a NOOP módszerével.  

Az eloszlás Debian család számára:

###<a name="step-1view-the-current-io-scheduler"></a>1. nézet az aktuális I/O ütemező lépés
A következő parancsot használja:  

    root@mysqlnode1:~# cat /sys/block/sda/queue/scheduler

Kimenet, amely jelzi az aktuális ütemező követően jelenik meg.  

    noop [deadline] cfq


###<a name="step-2-change-the-current-device-devsda-of-io-scheduling-algorithm"></a>Lépés: 2. Az aktuális eszköz (/ fejlesztők/sda) i/o-algoritmus ütemezésének módosítása
Az alábbi parancsokat használhatja:  

    azureuser@mysqlnode1:~$ sudo su -
    root@mysqlnode1:~# echo "noop" >/sys/block/sda/queue/scheduler
    root@mysqlnode1:~# sed -i 's/GRUB_CMDLINE_LINUX=""/GRUB_CMDLINE_LINUX_DEFAULT="quiet splash elevator=noop"/g' /etc/default/grub
    root@mysqlnode1:~# update-grub

>[AZURE.NOTE] Nem lehet hasznos a beállítást a /dev/sda egyedül áll. Szeretné állítani az összes adat lemez az adatbázist tároló szüksége van.  

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

##<a name="configure-system-file-operations-settings"></a>Rendszer fájl műveletek beállításainak konfigurálása
Több szempontból is a fájlrendszerben atime naplózási szolgáltatás letiltása. Atime az utolsó fájl access időre szükség. Egy fájl érhető el, amikor a fájlrendszerben az időbélyegző rögzíti a naplóban. Jó helyen jár ez az információ ritkán használatos. Ha már nincs szükség, amely csökkenti a teljes lemez access idő letilthatja.  

Tiltsa le a naplózás atime, szüksége fájl rendszer konfigurációs fájl /etc/ fstab módosíthatja, és adja hozzá a **noatime** lehetőséget.  

Ha például szerkesztése a vim /etc/fstab fájlt, az alább látható módon noatime hozzáadása.  

    # CLOUD_IMG: This file was created/modified by the Cloud Image build process
    UUID=3cc98c06-d649-432d-81df-6dcd2a584d41       /        ext4   defaults,discard        0 0
    #Add the “noatime” option below to disable atime logging
    UUID="431b1e78-8226-43ec-9460-514a9adf060e"     /RAID0   xfs   defaults,nobootwait, noatime 0 0
    /dev/sdb1       /mnt    auto    defaults,nobootwait,comment=cloudconfig 0       2

Ezután csatlakoztassa a fájlrendszer és a következő parancsot:  

    mount -o remount /RAID0

Tesztelje a módosított eredményét. Figyelje meg, hogy módosítja a próba-fájlt, az access idő nem frissül.  

Példa: előtt     

![][5]

Példa: után

![][6]

##<a name="increase-the-maximum-number-of-system-handles-for-high-concurrency"></a>Növelje a magas feldolgozási kezeli maximális száma
MySQL egy nagy feldolgozási adatbázis. Az alapértelmezett egyidejű fogópontok egy 1024 Linux, amely kifolyólag sem elegendő. **Kövesse az alábbi lépéseket a rendszer támogatja a MySQL verzió magas-ellenőrzési maximális egyidejű fogópontok növeléséhez**.

###<a name="step-1-modify-the-limitsconf-file"></a>Lépés: 1: A limits.conf fájl módosítása
Az alábbi négy sor hozzáadása az maximális engedélyezett egyidejű fogópontok növeléséhez /etc/security/limits.conf fájlban. Ne feledje, hogy 65536 a rendszer támogató maximális számát.   

    * finom nofile 65536
    * merevlemez nofile 65536
    * finom nproc 65536
    * merevlemez nproc 65536

###<a name="step-2-update-the-system-for-the-new-limits"></a>Lépés: 2: Frissítse a rendszer az új korlátozásairól
Futtassa az alábbi parancsokat:  

    ulimit -SHn 65536
    ulimit -SHu 65536

###<a name="step-3-ensure-that-the-limits-are-updated-at-boot-time"></a>Lépés 3: Győződjön meg arról, hogy a rendszerindításkor frissüljenek a korlátai
A következő indítási parancsok akkor a /etc/rc.local fájlt, akkor lép érvénybe, hogy minden indítás során.  

    echo “ulimit -SHn 65536” >>/etc/rc.local
    echo “ulimit -SHu 65536” >>/etc/rc.local

##<a name="mysql-database-optimization"></a>MySQL-adatbázis optimalizálása
Az azonos teljesítmény eszközbeállítási stratégia MySQL tartományi Azure, mint egy helyszíni számítógépen is használhatja.  

A fő I/O optimalizálási szabályok a következők:   

-   Növelje a gyorsítótár méretét.
-   Csökkentse a I/O válaszidő.  

-Történő optimalizálásához MySQL-kiszolgáló beállításainak frissítheti a my.cnf-fájllal, ez az alapértelmezett konfigurációs fájl-kiszolgáló és az ügyfélszámítógépek.  

Az alábbi konfigurációs elemeket MySQL teljesítményt befolyásoló főbb tényezők:  

-   **innodb_buffer_pool_size**: A pufferelési készlet pufferelt adatok és az index tartalmazza. Ez általában értéke fizikai memória 70 %-át.
-   **innodb_log_file_size**: Ez a művelet ismétlése napló méretét. Művelet ismétlése naplók annak érdekében, hogy írható gyors, a megbízható és a helyreállítható e az összeomlást után használhatja. 512MB-képet ad elég hely az írási műveletek naplózási értéke.
-   **max_connections**: néha alkalmazások, ne zárja be kapcsolatok megfelelően. Nagyobb értéket ad a kiszolgáló idled kapcsolatok Lomtár több időt. A maximális kapcsolatok 10000 számot, de az ajánlott maximális 5000.
-   **Innodb_file_per_table**: Ez a beállítás engedélyezése vagy letiltása a InnoDB lehetőségét, táblák tárolása a külön fájlokat. Kattintson a beállítás bekapcsolása biztosítja több speciális felügyeleti műveleteket hatékonyan alkalmazza. A teljesítmény szempontjából azt a táblázat terület Microsoft gyorsítása és tekintetében management optimalizálásához. Így ahhoz, hogy ez a javasolt beállítás be Kapcsolva.</br>
    A MySQL-5.6 az alapértelmezett beállítás be Kapcsolva. Ezért nincs további teendő szükség. Más verzióiban, amely 5.6 legkorábban, az alapértelmezett beállítások ki KAPCSOLVA. Ez tovább bekapcsolásával szükség. És kell alkalmazása előtt adatok betöltése, mert csak az újonnan létrehozott táblák érinti.
-   **innodb_flush_log_at_trx_commit**: alapértelmezett értéke 1, a hatókörrel értéke 0 ~ 2. Az alapértelmezett értéke a különálló MySQL-adatbázis leginkább alkalmas beállítást. A beállítás a 2 lehetővé teszi, hogy a legtöbb adatintegritás és MySQL fürt fő alkalmas. A 0-beállítással az adatok elvesztését, amely hatással lehetnek a megbízhatóságot, egyes esetekben az jobb teljesítményt, és a MySQL-fürt kisegítő alkalmas.
-   **Innodb_log_buffer_size**: A naplózási puffer lehetővé teszi, hogy a tranzakciók anélkül, hogy a tranzakciók véglegesítése előtt ürítse ki a napló lemez futtatásához. Ha nagy bináris objektum vagy szöveg mezőbe, a gyorsítótár nagyon gyorsan felhasználandó, és gyakori lemez I/O aktiválódik. Még jobban pufferelési méretének növelése, ha Innodb_log_waits állam változó nem 0.
-   **query_cache_size**: letiltáshoz kezdettől fogva nyújthatja a legjobb megoldást. Query_cache_size értéke 0 (Ez a most az alapértelmezett beállítás a MySQL-5.6), és más módszerekkel Lekérdezések gyorsítására.  

Lásd: [D függelékében](#AppendixD) után az optimalizálás teljesítmény összehasonlítása.


##<a name="turn-on-the-mysql-slow-query-log-for-analyzing-the-performance-bottleneck"></a>Kapcsolja be a MySQL-lassú lekérdezés naplójában szűk teljesítmény elemzése
A MySQL-lassú lekérdezés napló könnyen Észreveheti a lassú lekérdezések MySQL. Miután engedélyezte a MySQL-lassú lekérdezés napló, például **mysqldumpslow** MySQL-eszközök a teljesítmény szűk azonosítása is használhatja.  

Felhívjuk a figyelmét arra, hogy alapértelmezés szerint ez nincs engedélyezve. A lekérdezés lassú napló bekapcsolása foglalhat bizonyos Processzor erőforrásokat. Ajánlott tehát, hogy engedélyezze a ezzel ideiglenesen teljesítmény szűk hibaelhárítási.

###<a name="step-1-modify-mycnf-file-by-adding-the-following-lines-to-the-end"></a>Lépés: 1: My.cnf fájl módosításához a végén a következő sorok hozzáadásával   

    long_query_time = 2
    slow_query_log = 1
    slow_query_log_file = /RAID0/mysql/mysql-slow.log

###<a name="step-2-restart-mysql-server"></a>Lépés: 2: Indítsa újra a mysql-kiszolgáló
    service  mysql  restart

###<a name="step-3-check-whether-the-setting-is-taking-effect-using-the-show-command"></a>3 lépés: Ellenőrizze, hogy a beállítás tart az effektust a "megjelenítése" paranccsal

![][7]   

![][8]

Ebben a példában látható, hogy a lekérdezés lassú szolgáltatás ki van kapcsolva. Azután használhatja a **mysqldumpslow** eszköz teljesítmény szűk megállapítja, és a teljesítmény, például az indexek hozzáadásával optimalizálása.





##<a name="appendix"></a>Függelék

Az alábbi minta próba teljesítményadatokat célzott tesztkörnyezetben kattintson előállított, biztosítanak általános háttér a teljesítmény adatok trendjét különböző teljesítmény javítása, az alábbi módszerek azonban az eredményeket a különböző környezet vagy a termék verziója változhat.

<a name="AppendixA"></a>A: függelék  
**Különböző szintjei lemezre teljesítmény (IOPS)**


![][9]

**Próba-parancsok:**  

    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=5G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite

>AZURE. Megjegyzés: A terhelést a vizsgálat használja a 64 szál, próbálja a felső határ RAID eléréséhez.

<a name="AppendixB"></a>B: függelék  
**Különböző szintjei MySQL-teljesítmény (sebesség) összehasonlítása**   
(XFS fájlrendszer)


![][10]  
![][11]

**Próba-parancsok:**

    mysqlslap -p0ps.123 --concurrency=2 --iterations=1 --number-int-cols=10 --number-char-cols=10 -a --auto-generate-sql-guid-primary --number-of-queries=10000 --auto-generate-sql-load-type=write –engine=innodb

**Különböző szintjei MySQL-teljesítmény (OLTP) összehasonlítása**  
![][12]

**Próba-parancsok:**

    time sysbench --test=oltp --db-driver=mysql --mysql-user=root --mysql-password=0ps.123  --mysql-table-engine=innodb --mysql-host=127.0.0.1 --mysql-port=3306 --mysql-socket=/var/run/mysqld/mysqld.sock --mysql-db=test --oltp-table-size=1000000 prepare

<a name="AppendixC"></a>C: függelék   
**Különböző adattömb méretű lemezre teljesítmény (IOPS) összehasonlítása**  
(XFS fájlrendszer)


![][13]

**Próba-parancsok:**  

    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=30G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite
    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=1G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite  

Megjegyzés: a fájl mérete a teszteléshez használt 30GB és 1GB rendre, a RAID 0(4 disks) XFS mezők rendszer.


<a name="AppendixD"></a>D: függelék  
**MySQL teljesítmény (sebesség) összehasonlító előtt és után optimalizálása**  
(XFS File System)


![][14]

**Próba-parancsok:**

    mysqlslap -p0ps.123 --concurrency=2 --iterations=1 --number-int-cols=10 --number-char-cols=10 -a --auto-generate-sql-guid-primary --number-of-queries=10000 --auto-generate-sql-load-type=write –engine=innodb,misam

**A konfiguráció optmization és alapértelmezett értéke az alábbi képlettel történik:**

|Paraméterek |Alapértelmezett    |optmization
|-----------|-----------|-----------
|**innodb_buffer_pool_size**    |Nincs lehetőség   |7G
|**innodb_log_file_size**   |5M |512M
|**max_connections**    |100    |5000
|**innodb_file_per_table**  |0  |1
|**innodb_flush_log_at_trx_commit** |1  |2
|**innodb_log_buffer_size** |8M |128M
|**query_cache_size**   |16M    |0


Több részletes optimalizálási konfigurációs paraméterekkel, olvassa el a mysql hivatalos képernyőn megjelenő utasításokat.  

[http://dev.MySQL.com/doc/refman/5.6/en/innodb-Configuration.HTML](http://dev.mysql.com/doc/refman/5.6/en/innodb-configuration.html)  

[http://dev.MySQL.com/doc/refman/5.6/en/innodb-Parameters.HTML#sysvar_innodb_flush_method](http://dev.mysql.com/doc/refman/5.6/en/innodb-parameters.html#sysvar_innodb_flush_method)

**Tesztkörnyezetben**  

|Hardveres   |Részletek
|-----------|-------
|Processzor    |AMD Opteron(tm) processzor 4171 HE / 4 magmintákat
|A memória |14G
|merevlemez   |10G vagy lemez
|operációs rendszer |Ubuntu 14.04.1 LTS



[1]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-01.png
[2]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-02.png
[3]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-03.png
[4]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-04.png
[5]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-05.png
[6]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-06.png
[7]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-07.png
[8]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-08.png
[9]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-09.png
[10]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-10.png
[11]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-11.png
[12]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-12.png
[13]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-13.png
[14]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-14.png
