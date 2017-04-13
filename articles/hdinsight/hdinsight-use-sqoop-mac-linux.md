<properties
    pageTitle="Hadoop Sqoop használata Linux-alapú HDInsight |} Microsoft Azure"
    description="Megtudhatja, hogy miként futtatása a Sqoop importálása és exportálása egy Linux-alapú Hadoop HDInsight fürt és Azure SQL-adatbázishoz között."
    editor="cgronlun"
    manager="jhubbard"
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="larryfr"/>

#<a name="use-sqoop-with-hadoop-in-hdinsight-ssh"></a>A HDInsight (SSH) Hadoop Sqoop használata

[AZURE.INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Megtudhatja, hogyan Sqoop segítségével importálása és exportálása egy Linux-alapú HDInsight fürt és Azure SQL-adatbázis vagy az SQL Server-adatbázis között.

> [AZURE.NOTE] A cikkben leírt lépésekkel csatlakozhat SSH Linux-alapú HDInsight fürt. Windows-ügyfelek segítségével is Azure PowerShell és HDInsight .NET SDK Linux-alapú fürt Sqoop használata. Ezek a cikkek tabulátorválasztó használja.

##<a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban megkezdése előtt a következőket kell rendelkeznie:

- **A Hadoop-fürt HDInsight** és __Azure SQL-adatbázis__: A lépéseket a dokumentumban a fürt és a [fürt létrehozása és SQL-adatbázis](hdinsight-use-sqoop.md#create-cluster-and-sql-database) dokumentum használatával létrehozott adatbázis alapján. Ha már van egy HDInsight fürt és SQL-adatbázissal, azok az értékek a dokumentumban használt helyettesítheti.
- **Munkaállomás**: A számítógép egy SSH ügyfélprogrammal.

##<a name="install-freetds"></a>FreeTDS telepítése

1. A HDInsight Linux-alapú fürthöz kapcsolatfelvétel SSH használatával. A cím kapcsolódáskor használandó `CLUSTERNAME-ssh.azurehdinsight.net` és a port `22`.

    SSH használatával csatlakozhat HDInsight további tájékoztatást talál a következő dokumentumokat:

    * **Linux, Unix vagy OS X ügyfelek**: olvassa el a [Csatlakozás Linux, az OS X vagy a Unix Linux-alapú HDInsight fürthöz](hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster)

    * **Windows-ügyfelek**: olvassa el a [Csatlakozás a Windows Linux-alapú HDInsight fürthöz](hdinsight-hadoop-linux-use-ssh-windows.md#connect-to-a-linux-based-hdinsight-cluster)

3. A következő paranccsal FreeTDS telepítése:

        sudo apt-get --assume-yes install freetds-dev freetds-bin

    Néhány lépést SQL-adatbázishoz való csatlakozáshoz FreeTDS lesz használható.

##<a name="create-the-table-in-sql-database"></a>A tábla létrehozása az SQL-adatbázis

> [AZURE.IMPORTANT] Egy HDInsight fürt és a lépéseket a [fürt létrehozása](hdinsight-use-sqoop.md)és SQL-adatbázis készült SQL-adatbázis esetén, ha ugorja át az ebben a részben, az adatbázis és tábla létrehozott dokumentum lépéseit részeként.

1. HDInsight SSH csatlakozni csatlakozhat az SQL-adatbázis kiszolgálója és kréta be ezeket a lépéseket a többi használt alkalmazás a következő parancsot:

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D sqooptest

    Fog kapni a kibocsátás, az alábbihoz hasonló:

        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set to sqooptest
        1>

5. A a `1>` kéri, adja meg a következő sort:

        CREATE TABLE [dbo].[mobiledata](
        [clientid] [nvarchar](50),
        [querytime] [nvarchar](50),
        [market] [nvarchar](50),
        [deviceplatform] [nvarchar](50),
        [devicemake] [nvarchar](50),
        [devicemodel] [nvarchar](50),
        [state] [nvarchar](50),
        [country] [nvarchar](50),
        [querydwelltime] [float],
        [sessionid] [bigint],
        [sessionpagevieworder] [bigint])
        GO
        CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(clientid)
        GO

    Ha a `GO` utasítás van megadva, az előző kimutatások ki kell értékelni. Először a **mobiledata** táblázatot hoz létre, majd csoportosított index létrehozása hozzáadni (SQL-adatbázis kötelező.)

    A következő segítségével ellenőrizze, hogy a táblázat létrehoztak:

        SELECT * FROM information_schema.tables
        GO

    Meg kell jelennie a kimeneti az alábbihoz hasonló:

        TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
        sqooptest       dbo     mobiledata      BASE TABLE

8. Adja meg `exit` , a `1>` kérdés való kilépéshez a tsql segédprogramot.

##<a name="sqoop-export"></a>Sqoop exportálása

3. A SSH kapcsolatról hdinsight szolgáltatáshoz, se, ellenőrizze, hogy Sqoop láthatja az SQL-adatbázis a következő parancsot:

        sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.windows.net:1433 --username <adminLogin> --password <adminPassword>

    Ez térjen vissza az adatbázisok, beleértve a **sqooptest** -adatbázishoz, amely a korábban létrehozott listáját.

4. A következő parancsot használja **hivesampletable** adatainak exportálása a **mobiledata** tábla:

        sqoop export --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --export-dir 'wasbs:///hive/warehouse/hivesampletable' --fields-terminated-by '\t' -m 1

    Ez a tulajdonság utasítja Sqoop SQL-adatbázis csatlakoztatása a **sqooptest** adatbázishoz, és az adatok exportálása a **wasbs: / / / struktúra/raktári/hivesampletable** (a *hivesampletable*fizikai fájlokat) a **mobiledata** táblázatra.

5. A parancs befejeződése után csatlakozhat az alábbi TSQL az adatbázist:

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D sqooptest

    Miután létrejött, a következő kimutatások használatával ellenőrizze, hogy az adatok exportálása lett-e a **mobiledata** táblához:

        SELECT * FROM mobiledata
        GO

    Meg kell jelennie a táblázatban lévő adatok listája. Típus `exit` a tsql segédprogram lépni.

##<a name="sqoop-import"></a>Sqoop importálása

1. A következő használatával importálhatja az adatokat a **mobiledata** tábla SQL-adatbázisban, hogy a **wasbs: / / / oktatóanyagok/usesqoop/importeddata** könyvtár a HDInsight:

        sqoop import --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --target-dir 'wasbs:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1

    Az importált adatok lesz mezőket, amelyeket tabulátorkarakter vannak elválasztva, és a sorok megszakít egy új sor karaktert.

2. Az importálás befejeződése után a következő parancsot a listában, az adatok használata az új könyvtár:

        hadoop fs -text wasbs:///tutorials/usesqoop/importeddata/part-m-00000

##<a name="using-sql-server"></a>Az SQL Server használatával

Sqoop az adatok importálása és exportálása az SQL Server Azure-ban tárolt virtuális gépen vagy az Adatközpont is használhatja. A használt SQL-adatbázis és az SQL Server közötti különbségek a következők:

* HDInsight, mind az SQL Server Azure virtuális hálózaton kell lennie

    > [AZURE.NOTE] HDInsight támogatja csak helyfüggő virtuális hálózatok, és azt jelenleg nem működik a virtuális hálózatok affinitás csoport-alapú.

    SQL Server használatakor a adatközpontban meg kell adnia a virtuális hálózati *hely közötti* vagy a *webhely-pont*.

    > [AZURE.NOTE] **Webhely-pont** virtuális hálózatok SQL Server futnia kell a VPN-ügyfél configuration alkalmazást, amely érhető el az **Irányítópult** az Azure virtuális hálózati konfiguráció.

    Azure virtuális hálózati további tudnivalókért olvassa el a [Virtuális hálózati – áttekintés](../virtual-network/virtual-networks-overview.md)című témakört.

* Az SQL Server SQL-hitelesítéshez kell beállítania. További tudnivalókért lásd: [a hitelesítési mód kiválasztása](https://msdn.microsoft.com/ms144284.aspx)

* SQL Server-fogadja el a távoli kapcsolatok konfigurálása is. További információt talál [az SQL Server adatbázismotor kapcsolódás hibák elhárítása](http://social.technet.microsoft.com/wiki/contents/articles/2102.how-to-troubleshoot-connecting-to-the-sql-server-database-engine.aspx)

* Az SQL Server rendszerben olyan segédprogrammal például **SQL Server Management Studio** vagy **tsql** létre kell hoznia az **sqooptest** adatbázis - a lépések az Azure CLI csak Azure SQL-adatbázis használata

    Hasonlítanak a TSQL kimutatások a **mobiledata** táblázat létrehozásához használt SQL-adatbázis létrehozása egy clusterd kivételével index - ez nem kötelező, az SQL Server:

        CREATE TABLE [dbo].[mobiledata](
        [clientid] [nvarchar](50),
        [querytime] [nvarchar](50),
        [market] [nvarchar](50),
        [deviceplatform] [nvarchar](50),
        [devicemake] [nvarchar](50),
        [devicemodel] [nvarchar](50),
        [state] [nvarchar](50),
        [country] [nvarchar](50),
        [querydwelltime] [float],
        [sessionid] [bigint],
        [sessionpagevieworder] [bigint])

* Amikor csatlakozik az SQL Server hdinsight, előfordulhat, az IP-címet az SQL Server használja, ha nem rendelkezik egy (Tartománynévrendszer) nevek az Azure virtuális hálózaton. Példa:

        sqoop import --connect 'jdbc:sqlserver://10.0.1.1:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --target-dir 'wasbs:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1

##<a name="limitations"></a>Korlátozások

* Tömeges export - a Linux-alapú HDInsight, az adatok exportálása a Microsoft SQL Server- vagy SQL Azure-adatbázis használt Sqoop összekötő jelenleg nem támogatja a tömeges szúr be.

* Kötegelés – a HDInsight Linux-alapú használata esetén a `-batch` beszúrása végrehajtásakor váltás, Sqoop több beszúrása a Beszúrás műveletek kötegelés helyett hajt végre.

##<a name="next-steps"></a>Következő lépések

Most már megtanulta azt is van Sqoop használatáról. További tudnivalókért lásd:

- [Oozie használata HDInsight][hdinsight-use-oozie]: használata Sqoop művelet Oozie-munkafolyamatokban.
- [Adatelemzés nézetbeli késleltetés segítségével HDInsight][hdinsight-analyze-flight-data]: nézetbeli elemzéséhez használható struktúra adatok késleltetése, majd Sqoop adatainak exportálása Azure SQL-adatbázishoz.
- [Töltse fel az adatok hdinsight szolgáltatáshoz][hdinsight-upload-data]: más módszerek a feltöltéshez HDInsight/Azure Blob-tárolóhoz találja.



[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md

[sqldatabase-get-started]: ../sql-database-get-started.md
[sqldatabase-create-configue]: ../sql-database-create-configure.md

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: powershell-install-configure.md
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[sqoop-user-guide-1.4.4]: https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html
