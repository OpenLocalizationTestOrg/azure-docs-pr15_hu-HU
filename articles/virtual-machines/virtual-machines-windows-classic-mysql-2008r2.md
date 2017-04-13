<properties
    pageTitle="Hozzon létre egy virtuális MySQL futó |} Microsoft Azure"
    description="Hozzon létre egy Azure virtuális gépen futó Windows Server 2012 R2 és a MySQL-adatbázis használata a klasszikus telepítési modell."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/25/2016"
    ms.author="cynthn"/>


# <a name="install-mysql-on-a-virtual-machine-created-with-the-classic-deployment-model-running-windows-server-2012-r2"></a>MySQL telepítése a Windows Server 2012 R2 rendszer futtatása klasszikus telepítési modell készült virtuális gépen

[MySQL](http://www.mysql.com) , a népszerű Megnyitás, az SQL-adatbázishoz. Ebből az oktatóanyagból megtudhatja, hogy miként telepítheti és futtathatja a MySQL 5.6.23 közösségi változatát, mint a MySQL-kiszolgáló egy virtuális gépen futó Windows Server 2012 R2. MySQL telepít Linux útmutatást találhat: [Azure a MySQL telepítése](virtual-machines-linux-mysql-install.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

## <a name="create-a-virtual-machine-running-windows-server-2012-r2"></a>A Windows Server 2012 R2 futó virtuális gép létrehozása

Ha még nem rendelkezik egy virtuális Windows Server 2012 R2 rendszer fut, a virtuális gép létrehozása ebből az [oktatóanyagból](virtual-machines-windows-classic-tutorial.md) is használhatja. 

## <a name="attach-a-data-disk"></a>Lemezen adatok csatolása

A virtuális gép létrehozását követően tetszés szerint egy további adatokat lemez csatolhat. Ez akkor javasolt gyártási munkaterhelésekből és kevés a hely a meghajtón OS (c), amely tartalmazza az operációs rendszer elkerülése érdekében.

Megtudhatja, [hogy miként csatolni virtuális Windows géphez adatok lemezen](virtual-machines-windows-classic-attach-disk.md) , és kövesse az utasításokat üres lemez csatolása. Állítsa be a host gyorsítótár **nincs** vagy **csak olvasható**.

## <a name="log-on-to-the-virtual-machine"></a>Jelentkezzen be a virtuális gépen

Ezután akkor [Jelentkezzen be a virtuális gép](virtual-machines-windows-classic-connect-logon.md) így MySQL telepítheti.

##<a name="install-and-run-mysql-community-server-on-the-virtual-machine"></a>Telepítése és futtatása a MySQL-közösségi kiszolgáló a virtuális gépen

Kövesse ezeket a lépéseket követve telepítése, beállítása és a közösségi MySQL-kiszolgáló verziója:

> [AZURE.NOTE] Ezeket a lépéseket a 5.6.23.0 valók MySQL és a Windows Server 2012 R2 közösségi változatát. A felhasználói felület eltérhetnek az MySQL- vagy Windows Server különböző verzióira vonatkozóan.

1.  Miután felvette a virtuális gép távoli asztali változatában, kattintson az **Internet Explorer** a kezdőképernyőről.
2.  Jelölje ki az **eszközök** gombra a jobb felső sarokban (a cogged ikon) ikonra, és válassza az **Internetbeállítások parancsra**. Kattintson a **Biztonság** fülre, kattintson a **Megbízható helyek** ikonra, és kattintson a **helyek** gombra. Adja hozzá a http://*. mysql.com a megbízható helyek listájához. Kattintson a * *Bezárás**, és kattintson a * *OK**.
3.  Az a címet az Internet Explorer címsorában, írja be a http://dev.mysql.com/downloads/mysql/.
4.  Töltse le a MySQL-a Windows Installer legújabb verzióját, és keresse meg a MySQL-webhely használata A MySQL-telepítő kiválasztásakor töltse le a verziót, amelynek az adja meg a fájl megadása (például a mysql-installer-Közösség – 5.6.23.0.msi egy 282.4 MB fájlméretű), a telepítő mentése.
5.  Befejeződése a telepítő letöltése, kattintson a beállítás indítása **futtatása** .
6.  A **Licencszerződés** lapon fogadja el a licencszerződést, és kattintson a **Tovább**gombra.
7.  A **telepítés típusának kiválasztása** lapon kattintson a telepítés, amelyet, és kattintson a **Tovább gombra**. A következő lépések feltételezik, **csak** a telepítő kiszolgálótípus listáját.
8.  Kattintson a **telepítés** lap **végrehajtás**. Amikor befejeződött a telepítés, kattintson a **Tovább**gombra.
9.  A **Termék beállítása** lapon kattintson a **Tovább**gombra.
10. A **típus és a hálózati** lapon adja meg a kívánt konfiguráció típusa és kapcsolódási beállítást, például a portot szükség esetén. Válassza a **Speciális beállítások megjelenítése**elemre, és kattintson a **Tovább gombra**.

    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_TypeNetworking.png)

11. A **fiókok és szerepkörök** lapon adja meg a MySQL legfelső szintű erős jelszóval. Szükség szerint adjon meg további MySQL-fiókokkal, és kattintson a **Tovább**gombra.

    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_AccountsRoles_Filled.png)

12. A **Windows-alapú szolgáltatást** lapon adja meg az alapértelmezett beállítások módosítása a MySQL-kiszolgáló futtatott Windows-szolgáltatás igény szerint, és kattintson a **Tovább gombra**.

    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_WindowsService.png)

13. A **Speciális beállítások** lapon szükség szerint adja meg a naplózási beállítások módosításait, és kattintson a **Tovább gombra**.

    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_AdvOptions.png)

14. A **Kiszolgáló beállításainak alkalmazása** lapon kattintson a **végrehajtás**. Beállítási lépések elkészült, kattintson a **Befejezés gombra**.
15. A **Termék beállítása** lapon kattintson a **Tovább**gombra.
16. A **Telepítés befejeződött** lapon kattintson a **Log másolása a vágólapra** Ha azt szeretné, hogy vizsgálja meg később, és kattintson a **Befejezés gombra**.
17. A kezdőképernyőről írja be a **mysql**, és válassza a **MySQL 5.6 parancssori ügyfél**.
18. Adja meg a legfelső szintű 11 lépésben megadott jelszót, és meg fog be kell mutatni a rákérdező hol állíthatnak parancsok MySQL konfigurálása. A parancsok és a szintaxist, című cikkben olvashat [MySQL dokumentációjának](http://dev.mysql.com/doc/refman/5.6/en/server-configuration-defaults.html).

    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_CommandPrompt.png)

19. Server alapértelmezett beállításai, például a alap és az adatok mappák és meghajtók, beállíthatja a C:\Program Files (x86) \MySQL\MySQL Server 5.6\my-default.ini fájlban bejegyzésekkel is. További tudnivalókért lásd: [5.1.2 Server konfigurációs alapértelmezés szerint](http://dev.mysql.com/doc/refman/5.6/en/server-configuration-defaults.html).

## <a name="configure-endpoints"></a>Állítsa be a végpontok

Ha azt szeretné, hogy a MySQL-kiszolgáló szolgáltatás MySQL ügyfélszámítógépek az interneten keresztül elérhetővé szeretné tenni, kell a olyan portot, amelyet a MySQL-kiszolgáló szolgáltatás figyel végpont beállítása és a Windows tűzfal további szabály létrehozása. Ez a TCP-port 3306, hacsak nincs megadva egy másik porthoz **Írja be, és a hálózat** lapon (10 / lépés az előző eljárás).


> [AZURE.NOTE] Biztonsággal kapcsolatos ezzel, mert így a MySQL-kiszolgáló szolgáltatás elérhető lesz az interneten azokon a számítógépeken gondosan figyelembe. Definiálhat a forrás IP-címtartományokat használhatják az végpontot, egy Access vezérlő lista (vezérlés). További információért megtudhatja, [hogy miként állítsa be végpontok virtuális géphez](virtual-machines-windows-classic-setup-endpoints.md).


Zárólap a MySQL-kiszolgáló szolgáltatás beállítása:

1.  Az Azure klasszikus portálon kattintson a **virtuális gépeken futó**, kattintson a nevére a MySQL-virtuális gép, és válassza a **Végpontok**.
2.  A parancssáv kattintson a **Hozzáadás**gombra.
3.  A **virtuális géphez végpont hozzáadása** lapon kattintson a jobbra mutató nyílra.
4.  Ha használja az alapértelmezett 3306 portot MySQL, **MySQL** a **név**gombra, és kattintson a pipára.
5.  Különböző TCP-port használatakor adjon egy egyedi nevet a **név**. Jelölje be a **TCP** Protocol (protokoll), írja be a port száma egyaránt **Nyilvános** és **Titkos portot**, és kattintson a pipára.

## <a name="add-a-windows-firewall-rule-to-allow-mysql-traffic"></a>A Windows tűzfal MySQL forgalmának engedélyezésére szabály hozzáadása

A Windows tűzfal szabályt, amely lehetővé teszi a MySQL-forgalom az internetről hozzáadásához a következő parancsot a Windows PowerShell rendszergazda jogú parancssort a MySQL-kiszolgáló virtuális gépen.

    New-NetFirewallRule -DisplayName "MySQL56" -Direction Inbound –Protocol TCP –LocalPort 3306 -Action Allow -Profile Public


    
## <a name="test-your-remote-connection"></a>A távoli a kapcsolat tesztelése


A MySQL-kiszolgáló szolgáltatás az Azure virtuális gépen futó a távoli kapcsolat teszteléséhez meg kell határoznia a felhőbeli szolgáltatástól, amely tartalmazza a virtuális gépen futó MySQL-kiszolgáló megfelelő DNS-nevét.

1.  Az Azure klasszikus portálon kattintson a **virtuális gépeken futó**, kattintson a nevére a MySQL-kiszolgáló virtuális gép, és válassza az **Irányítópult**.
2.  A virtuális gép irányítópultról jegyezze fel a **DNS-név** oszlopban látható értéket a **Quick Glance** szakaszban. Lássunk egy példát:

    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_DNSName.png)

3.  A helyi számítógépről MySQL vagy a MySQL-ügyfél futtassa a következő parancsot a MySQL-felhasználó, jelentkezzen be.

        mysql -u <yourMysqlUsername> -p -h <yourDNSname>

    A MySQL-felhasználó neve dbadmin3 és a virtuális gép testmysql.cloudapp.net DNS-neve, például a következő parancsot használja.

        mysql -u dbadmin3 -p -h testmysql.cloudapp.net


## <a name="next-steps"></a>Következő lépések

MySQL futtatásával kapcsolatos további tudnivalókért lásd: a [MySQL-dokumentációt](http://dev.mysql.com/doc/).
