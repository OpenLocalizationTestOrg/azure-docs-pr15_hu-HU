<properties
    pageTitle="Kattintson egy Linux virtuális MySQL beállítása |} Microsoft Azure "
    description="Megtudhatja, hogyan telepítheti a MySQL-Papírhalom Linux virtuális gépen (Ubuntu vagy RedHat család OS) Azure-ban"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="SuperScottz"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/01/2016"
    ms.author="mingzhan"/>


#<a name="how-to-install-mysql-on-azure"></a>Azure MySQL telepítése


Ebben a cikkben megtanulhatja, hogyan telepítheti, állíthatja a MySQL-Azure virtuális gépen futó Linux.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


##<a name="install-mysql-on-your-virtual-machine"></a>A virtuális gép MySQL telepítése

> [AZURE.NOTE] Ebben az oktatóanyagban befejezéséhez Linux operációs rendszert futtató Microsoft Azure virtuális gép már kell rendelkeznie. Tanulmányozza az [Azure Linux virtuális oktatóprogram](virtual-machines-linux-quick-create-cli.md) hozhat létre, és állítsa be az egy Linux virtuális `mysqlnode` virtuális neveként és `azureuser` felhasználóként a folytatás előtt.

Ebben az esetben 3306 portot használja a MySQL-port.  

Csatlakozás a Linux gitt keresztül létrehozott virtuális. Ha ez az első alkalommal Azure Linux virtuális használnia, olvassa el a gitt használata egy Linux virtuális csatlakoztatása [ide](virtual-machines-linux-mac-create-ssh-keys.md).

Tárházba csomag telepítéséhez MySQL5.6 szerepel példaként, a jelen cikkben fogjuk használni. MySQL5.6 valójában további fokozása mint MySQL5.5 teljesítményét.  További információt [Itt](http://www.mysqlperformanceblog.com/2013/02/18/is-mysql-5-6-slower-than-mysql-5-5/).


###<a name="how-to-install-mysql56-on-ubuntu"></a>Ubuntu MySQL5.6 telepítése
Fogjuk használni Linux virtuális az Azure Ubuntu az alábbi.

- Lépés: 1: Telepítés MySQL kiszolgáló 5.6 Váltás `root` felhasználói:

            #[azureuser@mysqlnode:~]sudo su -

    Mysql-kiszolgáló 5.6 telepítése:

            #[root@mysqlnode ~]# apt-get update
            #[root@mysqlnode ~]# apt-get -y install mysql-server-5.6

    A telepítés során megjelenik egy párbeszédpanel ablak poping, kérje meg a MySQL legfelső szintű jelszót, és meg kell a jelszót Itt adhatja meg.

    ![kép](./media/virtual-machines-linux-mysql-install/virtual-machines-linux-install-mysql-p1.png)


    Adja meg a jelszót újra.

    ![kép](./media/virtual-machines-linux-mysql-install/virtual-machines-linux-install-mysql-p2.png)

- Lépés: 2: Bejelentkezési MySQL-kiszolgáló

    Amikor befejezte a MySQL-kiszolgáló telepítési, MySQL-szolgáltatás automatikusan elindul. Bejelentkezés a MySQL-kiszolgáló is `root` felhasználói.
    Használja az alatt a bejelentkezési adatok és a bemeneti jelszó parancsot.

             #[root@mysqlnode ~]# mysql -uroot -p

- 3 lépés: Az futó MySQL-szolgáltatás kezelése

    (a) első MySQL-szolgáltatás állapota

             #[root@mysqlnode ~]# service mysql status

    (b) MySQL-szolgáltatás elindítása

             #[root@mysqlnode ~]# service mysql start

    (c) MySQL-szolgáltatás leállítása

             #[root@mysqlnode ~]# service mysql stop

    d indítsa újra a MySQL-szolgáltatás

             #[root@mysqlnode ~]# service mysql restart


###<a name="how-to-install-mysql-on-red-hat-os-family-like-centos-oracle-linux"></a>Piros kalap operációs rendszer termékcsaládon kívüli, például CentOS, Oracle Linux MySQL telepítése
Fogjuk használni Linux virtuális CentOS vagy Oracle Linux itt.

- Lépés: 1: Adja hozzá a MySQL-Yum kapcsoló tárházba való `root` felhasználói:

            #[azureuser@mysqlnode:~]sudo su -

    Töltse le és telepítse a MySQL-megjelenés csomagot:

            #[root@mysqlnode ~]# wget http://repo.mysql.com/mysql-community-release-el6-5.noarch.rpm
            #[root@mysqlnode ~]# yum localinstall -y mysql-community-release-el6-5.noarch.rpm

- Lépés: 2: Alatti ahhoz, hogy a MySQL5.6 csomag letöltése a MySQL tárháza fájl szerkesztése.

            #[root@mysqlnode ~]# vim /etc/yum.repos.d/mysql-community.repo

    Az alábbi fájl egyes értékek frissítése:

        \# *Enable to use MySQL 5.6*

        [mysql56-community]
        name=MySQL 5.6 Community Server

        baseurl=http://repo.mysql.com/yum/mysql-5.6-community/el/6/$basearch/

        enabled=1

        gpgcheck=1

        gpgkey=file:/etc/pki/rpm-gpg/RPM-GPG-KEY-mysql

- 3 lépés: A MySQL-tárházba telepítése MySQL a telepítés MySQL:

           #[root@mysqlnode ~]#yum install mysql-community-server

    MySQL RPM csomag és az összes kapcsolódó csomagok telepíti.

- Lépés: 4: A futó MySQL-szolgáltatás kezelése

    (a) a MySQL-kiszolgáló szolgáltatás állapotának ellenőrzése:

           #[root@mysqlnode ~]#service mysqld status

    (b) ellenőrizze, hogy fut-e az alapértelmezett port a MySQL-kiszolgáló:

           #[root@mysqlnode ~]#netstat  –tunlp|grep 3306


    (c) a MySQL-kiszolgáló indítása:

           #[root@mysqlnode ~]#service mysqld start

    d megakadályozhatja, hogy a MySQL-kiszolgáló:

           #[root@mysqlnode ~]#service mysqld stop

    (e) beállítása MySQL indítása, amikor a rendszer indítási fel:

           #[root@mysqlnode ~]#chkconfig mysqld on


###<a name="how-to-install-mysql-on-suse-linux"></a>MySQL telepítése SUSE Linux
Fogjuk használni Linux virtuális OpenSUSE az alábbi.

- Lépés: 1: Töltse le és telepítse a MySQL-kiszolgáló

    Váltás `root` felhasználó keresztül alatt parancsot:  

           #sudo su -

    Töltse le és telepítse a MySQL-csomag:

           #[root@mysqlnode ~]# zypper update

           #[root@mysqlnode ~]# zypper install mysql-server mysql-devel mysql

- Lépés: 2: Az futó MySQL-szolgáltatás kezelése

    (a) a MySQL-kiszolgáló állapotának ellenőrzése:

           #[root@mysqlnode ~]# rcmysql status

    (b) jelölőnégyzet e alapértelmezett portja a MySQL-kiszolgáló:

           #[root@mysqlnode ~]# netstat  –tunlp|grep 3306


    (c) a MySQL-kiszolgáló indítása:

           #[root@mysqlnode ~]# rcmysql start

    d megakadályozhatja, hogy a MySQL-kiszolgáló:

           #[root@mysqlnode ~]# rcmysql stop

    (e) beállítása MySQL indítása, amikor a rendszer indítási fel:

           #[root@mysqlnode ~]# insserv mysql

###<a name="next-step"></a>Következő lépés
Keresse meg a további használatát és MySQL [Itt](https://www.mysql.com/)vonatkozó információkat.
