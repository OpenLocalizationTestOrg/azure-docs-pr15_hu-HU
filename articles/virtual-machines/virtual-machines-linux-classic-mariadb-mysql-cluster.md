<properties
    pageTitle="Azure MariaDB (MySQL) fürt fut"
    description="Hozzon létre egy MariaDB + Galera MySQL fürt a Azure virtuális gépeken futó"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="sabbour"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="04/15/2015"
    ms.author="v-ahsab"/>

# <a name="mariadb-mysql-cluster---azure-tutorial"></a>MariaDB (MySQL) fürt – Azure oktatóprogram

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

> [AZURE.NOTE]  MariaDB vállalati fürt most érhető el a Microsoft Azure piactéren található.  Az új felületek fog automatikusan üzembe felhő MariaDB Galera fürt. Az új ajánlja fel a https://azure.microsoft.com/en-us/marketplace/partners/mariadb/cluster-maxscale/ kell használni. 

Több diaminta [Galera](http://galeracluster.com/products/) fürt [MariaDBs](https://mariadb.org/en/about/), robusztus, méretezhető és megbízható Esőcsepp helyettesítheti a MySQL a munkát a Azure virtuális gépeken futó könnyen hozzáférhető környezetben készít azt.

## <a name="architecture-overview"></a>Architektúra – áttekintés

Ez a témakör hajtja végre az alábbi lépéseket:

1. 3-csomópont fürt létrehozása
2. Az adatok lemez külön az operációs rendszer lemezről
3. Az adatok RAID 0 és amelyben beállítással növelheti IOPS létrehozásához
4. Az Azure betöltése terheléselosztó használja a 3 csomópontok terhelését
5. Ismétlődő munka minimalizálásához létrehozása MariaDB + Galera tartalmazó virtuális képe, és a többi fürt VMs létrehozásához.

![Architektúra](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Setup.png)

> [AZURE.NOTE]  Ez a témakör az [Azure CLI](../xplat-cli-install.md) eszközöket használja, így feltétlenül töltse le és csatlakozás őket az Azure előfizetés útmutatása szerint. Ha az Azure CLI használható parancsok hivatkozás van szüksége, olvassa el ezt a hivatkozást a [Azure CLI parancs hivatkozást](../virtual-machines-command-line-tools.md). Lesz [-SSH kulcs hitelesítéshez létrehozásához] szükséges, és jegyezze fel a **.pem fájl helyét**.


## <a name="creating-the-template"></a>A sablon létrehozása

### <a name="infrastructure"></a>Infrastruktúra

1. Csoport együtt tartása az erőforrások affinitás létrehozása

        azure account affinity-group create mariadbcluster --location "North Europe" --label "MariaDB Cluster"

2. Hozzon létre egy virtuális hálózaton

        azure network vnet create --address-space 10.0.0.0 --cidr 8 --subnet-name mariadb --subnet-start-ip 10.0.0.0 --subnet-cidr 24 --affinity-group mariadbcluster mariadbvnet

3. Az összes a lemez tárolni tárterület-fiók létrehozása. Jegyzet, akkor ne kell úgy, hogy a több mint 40 erősen szerezze meg a 20 000 IOPS fiók tárterületkorlát elkerülése érdekében lemez használt ugyanazzal a tárterület-fiókkal. Ebben az esetben, hogy meddig kikapcsolt száma, azt fog tárolni mindent az egyszerűség ugyanazzal a fiókkal

        azure storage account create mariadbstorage --label mariadbstorage --affinity-group mariadbcluster

3. Keresse meg a nevét a CentOS 7 virtuális gép képe

        azure vm image list | findstr CentOS
Ez lesz hasonló kimeneti `5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20140926`. A következő lépésben nevét használja.

4. A tárolási helyére a létrehozott .pem SSH kulcs elérési **/path/to/key.pem** lecserélve virtuális sablon létrehozása

        azure vm create --virtual-network-name mariadbvnet --subnet-names mariadb --blob-url "http://mariadbstorage.blob.core.windows.net/vhds/mariadbhatemplate-os.vhd"  --vm-size Medium --ssh 22 --ssh-cert "/path/to/key.pem" --no-ssh-password mariadbtemplate 5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20140926 azureuser

5. 4 x 500GB adatot lemezre csatolása a virtuális a RAID konfiguráció használata

        FOR /L %d IN (1,1,4) DO azure vm disk attach-new mariadbhatemplate 512 http://mariadbstorage.blob.core.windows.net/vhds/mariadbhatemplate-data-%d.vhd

6. SSH **mariadbhatemplate.cloudapp.net:22** hozza létre és a titkos kulcs csatlakozhat virtuális sablonba.

### <a name="software"></a>Szoftver

1. Szerezze be a legfelső szintű

        sudo su

2. Telepítse a RAID támogatása:

     - Mdadm telepítése

                yum install mdadm

     - Hozzon létre egy EXT4 fájlrendszer a RAID0/csíkok konfiguráció

                mdadm --create --verbose /dev/md0 --level=stripe --raid-devices=4 /dev/sdc /dev/sdd /dev/sde /dev/sdf
                mdadm --detail --scan >> /etc/mdadm.conf
                mkfs -t ext4 /dev/md0

     - A csatlakozási pont könyvtár létrehozása

                mkdir /mnt/data

     - Az újonnan létrehozott RAID eszköz UUID beolvasása

                blkid | grep /dev/md0

     - /Etc/fstab szerkesztése

                vi /etc/fstab

     - Az ott ahhoz, hogy az automatikus mouting újraindításkor az UUID helyettesítve a **blkid** parancs, mielőtt a kapott értéket adja hozzá a eszközt

                UUID=<UUID FROM PREVIOUS>   /mnt/data ext4   defaults,noatime   1 2

     - Az új partíciót csatlakoztatása

                mount /mnt/data

3. MariaDB telepítése:

     - A MariaDB.repo fájl létrehozása:

                vi /etc/yum.repos.d/MariaDB.repo

     - Töltse ki az az alábbi tartalom

                [mariadb]
                name = MariaDB
                baseurl = http://yum.mariadb.org/10.0/centos7-amd64
                gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
                gpgcheck=1

     - Távolítsa el a meglévő utólagos javítás és a mariadb-könyvtárral lenne ütközések elkerülése

            yum remove postfix mariadb-libs-*

     - Galera MariaDB telepíteni

            yum install MariaDB-Galera-server MariaDB-client galera

4. A MySQL-adatkönyvtárának áthelyezése a RAID blokk eszköz

     - Az új helyre másolja be az aktuális MySQL könyvtár és a régi könyvtár eltávolítása

            cp -avr /var/lib/mysql /mnt/data  
            rm -rf /var/lib/mysql

     - Ennek megfelelően az új könyvtár vonatkozó engedélyek beállítása

            chown -R mysql:mysql /mnt/data && chmod -R 755 /mnt/data/  

     - Hozzon létre egy a régi könyvtár mutasson az új helyre a RAID partíciót a symlink

            ln -s /mnt/data/mysql /var/lib/mysql

5. Mivel [a fürt műveletek zavarja SELinux](http://galeracluster.com/documentation-webpages/configuration.html#selinux), az aktuális munkamenet (megjelenéséig kompatibilis verziója) letiltáshoz szükséges. Szerkesztés `/etc/selinux/config` tiltsa le azt a későbbi újraindítása:

            setenforce 0

       then editing `/etc/selinux/config` to set `SELINUX=permissive`

6. MySQL fut ellenőrzése

    - Indítsa el a MySQL

            service mysql start

    - Biztonságos a MySQL-telepítést, a legfelső szintű jelszó beállítása, távolítsa el a névtelen felhasználók távoli legfelső szintű bejelentkezési letiltása és eltávolítása a próba-adatbázis

            mysql_secure_installation

    - Felhasználó létrehozása az adatbázisban fürt műveletekhez, és ha szükséges, az alkalmazások

            mysql -u root -p
            GRANT ALL PRIVILEGES ON *.* TO 'cluster'@'%' IDENTIFIED BY 'p@ssw0rd' WITH GRANT OPTION; FLUSH PRIVILEGES;
            exit

   - MySQL leállítása

            service mysql stop

7. Konfigurációs helyőrző létrehozása

    - Hozzon létre egy helyőrző fürt beállításainak az MySQL-beállítások szerkesztése. Ne cserélje le a **`<Vairables>`** , vagy vegye ki a megjegyzésjeleket most. Amely lép fel, miután hozzunk létre egy virtuális, a sablonból.

            vi /etc/my.cnf.d/server.cnf

    - A **[galera]** szakasz szerkesztése, és törölje a jelet azoknak

    - A **[mariadb]** csoport szerkesztése

            wsrep_provider=/usr/lib64/galera/libgalera_smm.so
            binlog_format=ROW
            wsrep_sst_method=rsync
            bind-address=0.0.0.0 # When set to 0.0.0.0, the server listens to remote connections
            default_storage_engine=InnoDB
            innodb_autoinc_lock_mode=2

            wsrep_sst_auth=cluster:p@ssw0rd # CHANGE: Username and password you created for the SST cluster MySQL user
            #wsrep_cluster_name='mariadbcluster' # CHANGE: Uncomment and set your desired cluster name
            #wsrep_cluster_address="gcomm://mariadb1,mariadb2,mariadb3" # CHANGE: Uncomment and Add all your servers
            #wsrep_node_address='<ServerIP>' # CHANGE: Uncomment and set IP address of this server
            #wsrep_node_name='<NodeName>' # CHANGE: Uncomment and set the node name of this server

8. Nyissa meg a szükséges portok a tűzfal (FirewallD használata CentOS 7)

    - MySQL:`firewall-cmd --zone=public --add-port=3306/tcp --permanent`
    - GALERA:`firewall-cmd --zone=public --add-port=4567/tcp --permanent`
    - GALERA ITT:`firewall-cmd --zone=public --add-port=4568/tcp --permanent`
    - RSYNC:`firewall-cmd --zone=public --add-port=4444/tcp --permanent`
    - Frissítse a tűzfalon keresztül:`firewall-cmd --reload`

9.  Optimalizálás a rendszert a teljesítmény elérése érdekében. Ez a cikk a [Teljesítmény eszközbeállítási stratégia](virtual-machines-linux-classic-optimize-mysql.md) további részleteket hivatkozik

    - Ismét szerkeszteni a MySQL-konfigurációs fájl

            vi /etc/my.cnf.d/server.cnf

    - A **[mariadb]** szakasz szerkesztése és a Hozzáfűzés a alatt

    > [AZURE.NOTE] Azt ajánljuk, hogy **innodb\_pufferelési\_pool_size** 70 %-a virtuális memória. Van beállítva az alábbi 2.45 GB 3,5 GB RAM a közepes Azure virtuális.

            innodb_buffer_pool_size = 2508M # The buffer pool contains buffered data and the index. This is usually set to 70% of physical memory.
            innodb_log_file_size = 512M #  Redo logs ensure that write operations are fast, reliable, and recoverable after a crash
            max_connections = 5000 # A larger value will give the server more time to recycle idled connections
            innodb_file_per_table = 1 # Speed up the table space transmission and optimize the debris management performance
            innodb_log_buffer_size = 128M # The log buffer allows transactions to run without having to flush the log to disk before the transactions commit
            innodb_flush_log_at_trx_commit = 2 # The setting of 2 enables the most data integrity and is suitable for Master in MySQL cluster
            query_cache_size = 0

10. Állítsa le a MySQL, nem futtathatnak indítási rontja el a fürt, egy új csomópont hozzáadásakor elkerülése érdekében a MySQL-szolgáltatás kikapcsolása és a gép deprovision.

        service mysql stop
        chkconfig mysql off
        waagent -deprovision

11. Rögzítés a virtuális a portálon keresztül. (Jelenleg [a probléma az Azure CLI a #1268] eszközök mutatja be arra, hogy az Azure CLI eszközök által rögzített képek nem rögzítése a csatlakoztatott adatok lemez.)

    - A gép a portálon keresztül leállítása
    - Kattintson a rögzítés, és adja meg a kép neve nevet **mariadb-galera -** képe, és adjon meg egy leírást, és jelölje be a "Waagent kell futtatni".
    ![A virtuális gép rögzítése](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Capture.png)
    ![a virtuális gép rögzítése](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Capture2.PNG)

## <a name="creating-the-cluster"></a>A fürt létrehozása

Kijelentkezés a sablont, akkor csak létrehozott és majd konfigurálhatja és megkezdheti a fürt 3 VMs létrehozása.

1. A első CentOS 7 virtuális a **mariadb-galera-kép** létrehozott képből készítéséhez nyújtó a virtuális hálózat nevét **mariadbvnet** és alhálózat **mariadb**gép méretét, **Közepes**, a felhőszolgáltatásában áthaladó **mariadbha** kell nevet (vagy bármely nevet szeretne mariadbha.cloudapp.net keresztül érhető el), a nevét, az adott beállítás **mariadb1** és felhasználónév **azureuser**kell lennie a gép és SSH hozzáférés és a SSH áthaladó tanúsítvány .pem fájl- és cseréje **/path/to/key.pem** útvonala hol, a létrehozott .pem SSH kulcs tárolja.

    > [AZURE.NOTE] A parancsok az alábbi két részre áttekinthetővé több sort fölé, de egyes egy sorba kell megadnia.

        azure vm create
        --virtual-network-name mariadbvnet
        --subnet-names mariadb
        --availability-set clusteravset
        --vm-size Medium
        --ssh-cert "/path/to/key.pem"
        --no-ssh-password
        --ssh 22
        --vm-name mariadb1
        mariadbha mariadb-galera-image azureuser

2. 2 további virtuális gépeken futó _összekapcsolásával_ létrehozni az éppen létrehozott **mariadbha** felhőalapú szolgáltatás, a **virtuális nevét** , valamint a **SSH port** módosítása nem ütközik-e az egyéb VMs az azonos felhőszolgáltatásában egyedi porthoz őket.

        azure vm create
        --virtual-network-name mariadbvnet
        --subnet-names mariadb
        --availability-set clusteravset
        --vm-size Medium
        --ssh-cert "/path/to/key.pem"
        --no-ssh-password
        --ssh 23
        --vm-name mariadb2
        --connect mariadbha mariadb-galera-image azureuser
és MariaDB3

        azure vm create
        --virtual-network-name mariadbvnet
        --subnet-names mariadb
        --availability-set clusteravset
        --vm-size Medium
        --ssh-cert "/path/to/key.pem"
        --no-ssh-password
        --ssh 24
        --vm-name mariadb3
        --connect mariadbha mariadb-galera-image azureuser

3. Meg kell a belső IP-címét a 3 VMs mindegyike beszerzése a következő lépés:

    ![Az első IP-cím](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/IP.png)

4. A 3 VMs be SSH és és a konfigurációs fájl minden szerkesztése

        sudo vi /etc/my.cnf.d/server.cnf

    uncommenting **`wsrep_cluster_name`** és **`wsrep_cluster_address`** eltávolításával a **#** elején és érvényességi azok valóban kapcsolódó.
    Ezenkívül cseréje **`<ServerIP>`** a **`wsrep_node_address`** és **`<NodeName>`** a **`wsrep_node_name`** a virtuális IP-cím, és nevezze el a kurzor, és vegye ki a megjegyzésjeleket, valamint azokat a sorokat.

5. A fürt kezdése MariaDB1, és várja meg a Futtatás indításakor

        sudo service mysql bootstrap
        chkconfig mysql on

6. Indítsa el a MySQL MariaDB2 és MariaDB3, és várja meg a Futtatás indításakor

        sudo service mysql start
        chkconfig mysql on

## <a name="load-balancing-the-cluster"></a>A fürt terheléselosztás
A csoportosított VMs létrehozásakor, adja őket hozzá egy Availablity beállítása **clusteravset** nevű annak biztosítására, a különböző hiba kerülnek, és frissítse a tartományok és, hogy Azure soha nem minden gépeken karbantartási egyszerre be. Ebben a konfigurációban megfelel az adott Azure szolgáltatás szint szerződést (SLA) véve.

Most már segítségével az Azure terheléselosztó egyenleg kérelmek a 3 csomópontok között.

Futtassa a a számítógépen, használja az Azure CLI parancsai alatt.
Parancs paraméter struktúra van:`azure vm endpoint create-multiple <MachineName> <PublicPort>:<VMPort>:<Protocol>:<EnableDirectServerReturn>:<Load Balanced Set Name>:<ProbeProtocol>:<ProbePort>`

    azure vm endpoint create-multiple mariadb1 3306:3306:tcp:false:MySQL:tcp:3306
    azure vm endpoint create-multiple mariadb2 3306:3306:tcp:false:MySQL:tcp:3306
    azure vm endpoint create-multiple mariadb3 3306:3306:tcp:false:MySQL:tcp:3306

Végül a CLI a terheléselosztó vizsgálati gyakoriság (amely lehet egy kicsit túl hosszú) 15 másodperc beállítása, mivel az módosítja a portálon, a **Végpontok** bármely a VMs

![Végpont szerkesztése](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Endpoint.PNG)

Reconfigure The Load-Balanced beállításai gombra, majd lépjen tovább

![Konfigurálja a kiegyensúlyozott betöltése](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Endpoint2.PNG)

Ezután 5 másodpercre Probe intervallum módosításához és mentése

![Módosítás vizsgálati intervallum](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Endpoint3.PNG)

## <a name="validating-the-cluster"></a>A fürt érvényesítése

A merevlemez munkát. A fürt elérhető kell, hogy most már a `mariadbha.cloudapp.net:3306` lesz a terheléselosztó találati és a 3 VMs közötti kérelmek irányítása, hatékonyan és zavartalanul.

Csatlakozás vagy csak a VMs ellenőrizze annak működését a fürt a csatlakozás a kedvenc MySQL-ügyfél használata

     mysql -u cluster -h mariadbha.cloudapp.net -p

Ezután hozzon létre új adatbázist, és töltse ki az adatokat

    CREATE DATABASE TestDB;
    USE TestDB;
    CREATE TABLE TestTable (id INT NOT NULL AUTO_INCREMENT PRIMARY KEY, value VARCHAR(255));
    INSERT INTO TestTable (value)  VALUES ('Value1');
    INSERT INTO TestTable (value)  VALUES ('Value2');
    SELECT * FROM TestTable;

Az alábbi táblázat eredményez

    +----+--------+
  	| id | value  |
    +----+--------+
  	|  1 | Value1 |
  	|  4 | Value2 |
    +----+--------+
    2 rows in set (0.00 sec)

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Következő lépések

Ebben a cikkben a 3 létrehozott csomópont MariaDB + Galera könnyen hozzáférhető fürt a Azure virtuális gépeken futó CentOS 7 rendszert futtató. A VMs kerül az Azure betöltése terheléselosztó együtt.

Ajánljuk figyelmébe az [úgy is fürt Linux MySQL](virtual-machines-linux-classic-mysql-cluster.md) és [optimalizálása, és tesztelje a MySQL teljesítmény Azure Linux VMs](virtual-machines-linux-classic-optimize-mysql.md)módjai érdemes.

<!--Anchors-->
[Architecture overview]: #architecture-overview
[Creating the template]: #creating-the-template
[Creating the cluster]: #creating-the-cluster
[Load balancing the cluster]: #load-balancing-the-cluster
[Validating the cluster]: #validating-the-cluster
[Next steps]: #next-steps

<!--Image references-->

<!--Link references-->
[Galera]: http://galeracluster.com/products/
[MariaDBs]: https://mariadb.org/en/about/
[Hozzon létre egy SSH kulcs hitelesítéshez]:http://www.jeff.wilcox.name/2013/06/secure-linux-vms-with-ssh-certificates/
[az Azure CLI #1268 problémát]:https://github.com/Azure/azure-xplat-cli/issues/1268
