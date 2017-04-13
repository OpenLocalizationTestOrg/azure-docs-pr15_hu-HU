<properties
    pageTitle="Hadoop Oozie munkafolyamatok használata Linux-alapú HDInsight |} Microsoft Azure"
    description="Hadoop Oozie ismertetett Linux-alapú hdinsight szolgáltatásból lehetőségre. Megtudhatja, hogy miként egy Oozie munkafolyamat megadása, és küldjön egy Oozie feladatot."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/11/2016"
    ms.author="larryfr"/>


# <a name="use-oozie-with-hadoop-to-define-and-run-a-workflow-on-linux-based-hdinsight"></a>Hadoop meghatározása és a munkafolyamatot futtatni Linux-alapú HDInsight Oozie használata

[AZURE.INCLUDE [oozie-selector](../../includes/hdinsight-oozie-selector.md)]

Megtudhatja, hogy miként Apache Oozie használja a struktúra, és Sqoop használó munkafolyamat megadása, és futtassa a munkafolyamatot Linux-alapú HDInsight fürtre.

Apache Oozie munkafolyamat/effektusával rendszer kezelése a Hadoop feladatokat. A Hadoop Papírhalom integrálva van, illetve Hadoop feladatok Apache MapReduce, Apache malac, Apache struktúra és Apache Sqoop támogatja. Azt is használható a rendszer, például Java-programok vagy rendszerhéj parancsfájlok jellemző feladatok ütemezése

> [AZURE.NOTE] A HDInsight munkafolyamatok definiálása alternatíva Azure Data Factory. Azure Data Factory kapcsolatos további tudnivalókért lásd: a [használatának malac és az adatok gyári struktúra][azure-data-factory-pig-hive].

##<a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban megkezdése előtt a következőket kell rendelkeznie:

- **Az Azure-előfizetés**: lásd: [Ismerkedés az Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/).

- **Azure CLI**: [Telepítse és állítsa be az Azure CLI](../xplat-cli-install.md) lásd:
    
    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

- **Egy HDInsight fürt**: lásd a [HDInsight Linux – első lépések](hdinsight-hadoop-linux-tutorial-get-started.md)

- **Az Azure SQL-adatbázishoz**: Ez létrejön a lépésekkel a dokumentumban

##<a name="example-workflow"></a>Példa munkafolyamat

A munkafolyamat alkalmazhat a dokumentumban az utasításokat követve két művelet tartalmazza. Műveletek definiálják tevékenységek, például a struktúra, Sqoop, MapReduce vagy más folyamat futtatása:

![Munkafolyamat-diagramokhoz.][img-workflow-diagram]

1. A struktúra művelet rekordok kinyerése HDInsight részét képező **hivesampletable** HiveQL parancsfájl fut. Az adatok minden egyes sor egy adott mobileszközéről látogatás ismerteti. A rekord formátum jelenik meg az alábbihoz hasonló:

        8       18:54:20        en-US   Android Samsung SCH-i500        California     United States    13.9204007      0       0
        23      19:19:44        en-US   Android HTC     Incredible      Pennsylvania   United States    NULL    0       0
        23      19:19:46        en-US   Android HTC     Incredible      Pennsylvania   United States    1.4757422       0       1

    A struktúra parancsfájl, a dokumentumban használt számolja meg (például az Android- és iPhone-alapú) platformokon a teljes tartománynevére, és tárolja az új struktúra táblázattá száma.

    Struktúra kapcsolatos további tudnivalókért lásd: a [Használata a HDInsight struktúra][hdinsight-use-hive].

2.  Egy Sqoop műveletet az új struktúratáblával tartalmának exportálása egy Azure SQL-adatbázis táblájához. Sqoop kapcsolatos további tudnivalókért lásd: [Használata Hadoop Sqoop a HDInsight][hdinsight-use-sqoop].

> [AZURE.NOTE] A HDInsight fürt támogatott Oozie verziók című [HDInsight által biztosított Hadoop fürt verziók újdonságai?] [hdinsight-versions].

##<a name="create-the-working-directory"></a>A munkakönyvtár létrehozása

Oozie vár a feladat ugyanabban a mappában tárolt lehet szükséges források. Ez a példa **wasbs: / / / oktatóanyagok/useoozie**. A következő paranccsal hozhat létre, ez, és az adatok könyvtárat, amely tartalmazni fogja az új struktúratáblával hozta létre a munkafolyamatot:

    hdfs dfs -mkdir -p /tutorials/useoozie/data

> [AZURE.NOTE] A `-p` paraméter összes könyvtárak okozott elérési útját jön létre, ha az azok nem létezik. **A adatkönyvtárának** használandó tartsa lenyomva az ujját a **useooziewf.hql** parancsfájl által használt adatok.

Szintén a következő parancsot, amely biztosítja, hogy fut-struktúra és Sqoop feladatok Oozie is megszemélyesítés a felhasználói fiókját. **USERNAME** cserélje ki a login name:

    sudo adduser USERNAME users

Ha hibaüzenet jelenik meg, hogy a felhasználó már tagja a felhasználók, csak figyelmen kívül hagyhatja azt.

##<a name="add-a-database-driver"></a>Egy adatbázis-illesztőprogram hozzáadása

Mivel ez a munkafolyamat SQL-adatbázissal adatexportálás Sqoop használ, meg kell adnia a használt SQL-adatbázishoz beszélgetni JDBC illesztőprogram másolatát. A következő paranccsal másolja a vágólapra a munkakönyvtár:

    hdfs dfs -copyFromLocal /usr/share/java/sqljdbc_4.1/enu/sqljdbc*.jar /tutorials/useoozie/

Ha a munkafolyamat egyéb erőforrást, például egy olyan MapReduce alkalmazást tartalmazó üveg, kellene, valamint adja hozzá ezeket.

##<a name="define-the-hive-query"></a>Struktúra lekérdezés megadása

Egy HiveQL parancsprogramot, amely definiálja, a jelen dokumentum egy Oozie munkafolyamatban használt lekérdezés létrehozásához kövesse az alábbi lépéseket.

1. Csatlakozás a HDInsight Linux-alapú fürt SSH használatával:

    * **Linux, Unix vagy OS X ügyfelek**: lásd: [Használata SSH a Linux-alapú Hadoop a HDInsight Linux, az OS X vagy a Unix](hdinsight-hadoop-linux-use-ssh-unix.md)

    * **Windows-ügyfelek**: lásd: [Használata SSH a Linux-alapú Hadoop a HDInsight a Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

2. A következő paranccsal új fájl létrehozása:

        nano useooziewf.hql

1. Miután megnyílt a nano szerkesztő, a fájl tartalmát használható a következő:

        DROP TABLE ${hiveTableName};
        CREATE EXTERNAL TABLE ${hiveTableName}(deviceplatform string, count string) ROW FORMAT DELIMITED
        FIELDS TERMINATED BY '\t' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
        INSERT OVERWRITE TABLE ${hiveTableName} SELECT deviceplatform, COUNT(*) as count FROM hivesampletable GROUP BY deviceplatform;

    Létezik a parancsfájl használt két változó:

    - **{hiveTableName} $**: fogja tartalmazni a hozható létre táblázat neve
    - **{hiveDataFolder} $**: a táblázat adatfájlok tárolási helyét fog tartalmazni.

    A munkafolyamat-definíciós fájl (ebben az oktatóanyagban workflow.xml) ezeket az értékeket ez HiveQL parancsfájl futásidőben továbbítja.

2. Nyomja le a Ctrl-X lépjen ki a szerkesztő. Amikor a rendszer kéri, válassza ki az **Y** mentse a fájlt, majd az **ENTER billentyű** lenyomásával **useooziewf.hql** fájl nevét használja.

3. Az alábbi parancsokkal **useooziewf.hql** másolása **wasbs:///tutorials/useoozie/useooziewf.hql**:

        hdfs dfs -copyFromLocal useooziewf.hql /tutorials/useoozie/useooziewf.hql

    Ezek a parancsok tárolja a **useooziewf.hql** fájlt a fürt, amely megőrzi a fájlt, még akkor is, ha a program törli a fürt társított Azure tároló fiók. Lehetővé teszi, hogy pénzt fürt törlésével, ha nem használja, a feladatok és a munkafolyamatok megőrzésével.

##<a name="define-the-workflow"></a>A munkafolyamat megadása

Munkafolyamat-definíciók Oozie írt hPDL (egy XML folyamat Definition Language). A munkafolyamat megadásához kövesse az alábbi lépéseket:

1. Az alábbi utasítás használatával hozhat létre és módosíthat egy új fájlt:

        nano workflow.xml

1. Miután megnyílt a nano szerkesztő, a fájl tartalmának adja meg a következő:

        <workflow-app name="useooziewf" xmlns="uri:oozie:workflow:0.2">
            <start to = "RunHiveScript"/>
            <action name="RunHiveScript">
            <hive xmlns="uri:oozie:hive-action:0.2">
                <job-tracker>${jobTracker}</job-tracker>
                <name-node>${nameNode}</name-node>
                <configuration>
                <property>
                    <name>mapred.job.queue.name</name>
                    <value>${queueName}</value>
                </property>
                </configuration>
                <script>${hiveScript}</script>
                <param>hiveTableName=${hiveTableName}</param>
                <param>hiveDataFolder=${hiveDataFolder}</param>
            </hive>
            <ok to="RunSqoopExport"/>
            <error to="fail"/>
            </action>
            <action name="RunSqoopExport">
            <sqoop xmlns="uri:oozie:sqoop-action:0.2">
                <job-tracker>${jobTracker}</job-tracker>
                <name-node>${nameNode}</name-node>
                <configuration>
                <property>
                    <name>mapred.compress.map.output</name>
                    <value>true</value>
                </property>
                </configuration>
                <arg>export</arg>
                <arg>--connect</arg>
                <arg>${sqlDatabaseConnectionString}</arg>
                <arg>--table</arg>
                <arg>${sqlDatabaseTableName}</arg>
                <arg>--export-dir</arg>
                <arg>${hiveDataFolder}</arg>
                <arg>-m</arg>
                <arg>1</arg>
                <arg>--input-fields-terminated-by</arg>
                <arg>"\t"</arg>
                <archive>sqljdbc41.jar</archive>
                </sqoop>
            <ok to="end"/>
            <error to="fail"/>
            </action>
            <kill name="fail">
            <message>Job failed, error message[${wf:errorMessage(wf:lastErrorNode())}] </message>
            </kill>
            <end name="end"/>
        </workflow-app>

    Vannak az munkafolyamatokban definiált két műveleteket:

    - **RunHiveScript**: Ez a indítása műveletével, és **useooziewf.hql** struktúra parancsfájl fut.

    - **RunSqoopExport**: exportálja az adatokat a struktúra parancsfájl készített Sqoop használata SQL-adatbázishoz. Ez csak hajtja végre, ha sikeres az **RunHiveScript** művelet.

        > [AZURE.NOTE] Oozie munkafolyamat és a munkafolyamat-műveletek használatával kapcsolatos további tudnivalókért lásd: [Apache Oozie 4.0 dokumentáció] [ apache-oozie-400] (a HDInsight 3.0-s verziója) vagy [Apache Oozie 3.3.2 dokumentáció] [ apache-oozie-332] (a HDInsight 2.1-es verzió).

    Megjegyzés:, hogy a munkafolyamat például van-e több tételt, `${jobTracker}`, amely a feladat meghatározása a dokumentumon belül használhatja értékek cseréli.

    Tartsa szem előtt a `<archive>sqljdbc4.jar</arcive>` bejegyzést a Sqoop szakaszban. Ez a tulajdonság utasítja Oozie kíván-e a archív elérhető Sqoop Ez a művelet futtatásakor.

2. Használja a Ctrl-X, **Y** és, majd a **meg az ENTER billentyűt** a fájl mentéséhez.

3. A következő parancs használatával másolhatja a **workflow.xml** **wasbs:///tutorials/useoozie/workflow.xml**:

        hdfs dfs -copyFromLocal workflow.xml /tutorials/useoozie/workflow.xml

##<a name="create-the-database"></a>Az adatbázis létrehozása

Kövesse a dokumentumban a az [SQL-adatbázis létrehozása](../sql-database/sql-database-get-started.md) új adatbázis létrehozása. Az adatbázis létrehozásakor __oozietest__ használja az adatbázis nevét. Is jegyezze le a használható az adatbázis-kiszolgáló nevét, ez a következő szakaszban lesz szükség.

###<a name="create-the-table"></a>A tábla létrehozása

> [AZURE.NOTE] Többféleképpen tábla létrehozása az SQL-adatbázishoz való csatlakozáshoz. Az alábbi lépéseket a HDInsight fürt [FreeTDS](http://www.freetds.org/) használata.

3. A következő parancsot használja a HDInsight fürt FreeTDS telepítése:

        sudo apt-get --assume-yes install freetds-dev freetds-bin

4. Egyszer FreeTDS telepítve van, használja a következő parancsot a korábban létrehozott SQL-adatbázis-kiszolgálóhoz való csatlakozáshoz:

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <sqlLogin> -P <sqlPassword> -p 1433 -D oozietest

    Fog kapni a kibocsátás, az alábbihoz hasonló:

        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set to oozietest
        1>

5. A a `1>` kéri, adja meg a következő sort:

        CREATE TABLE [dbo].[mobiledata](
        [deviceplatform] [nvarchar](50),
        [count] [bigint])
        GO
        CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(deviceplatform)
        GO

    Ha a `GO` utasítás van megadva, az előző kimutatások ki kell értékelni. Ez hoz létre írandó által Sqoop **mobiledata** nevű új táblát.

    A következő segítségével ellenőrizze, hogy a táblázat létrehoztak:

        SELECT * FROM information_schema.tables
        GO

    Meg kell jelennie a kimeneti az alábbihoz hasonló:

        TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
        oozietest       dbo     mobiledata      BASE TABLE

8. Adja meg `exit` , a `1>` kérdés való kilépéshez a tsql segédprogramot.

##<a name="create-the-job-definition"></a>A feladat definíciója létrehozása

A feladat definíciója ismerteti a workflow.xml, valamint más (például useooziewf.hql.) a munkafolyamat által használt fájlok megkeresése Is megadja az értékeket, tulajdonságok használni a munkafolyamat belül, és kapcsolódó fájlok.

1. A következő paranccsal alapértelmezett tárolóhoz teljes WASB címét. Ez lesz használatban a konfigurációs fájl később:

        sed -n '/<name>fs.default/,/<\/value>/p' /etc/hadoop/conf/core-site.xml

    Meg kell adatokat adja vissza az alábbihoz hasonló:

        <name>fs.defaultFS</name>
        <value>wasbs://mycontainer@mystorageaccount.blob.core.windows.net</value>

    Mentse a **wasbs://mycontainer@mystorageaccount.blob.core.windows.net** az az érték, mint fog szerepelni a következő lépésekkel.

2. A következő paranccsal beszerzése a fürt headnode teljes Tartománynevét. Ez a fürt JobTracker címének lesz. Ez lesz használatban a konfigurációs fájl később:

        hostname -f

    Ez ad vissza információk az alábbihoz hasonló:

        hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net

    A használható a JobTracker portja 8050, tehát a teljes címet írja a JobTracker használni fog **hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:8050**.

1. A következő segítségével a Oozie feladat definition konfiguráció létrehozása:

        nano job.xml

2. Miután megnyílt a nano szerkesztő, a fájl tartalmát használható a következő:

        <?xml version="1.0" encoding="UTF-8"?>
        <configuration>

          <property>
            <name>nameNode</name>
            <value>wasbs://mycontainer@mystorageaccount.blob.core.windows.net</value>
          </property>

          <property>
            <name>jobTracker</name>
            <value>JOBTRACKERADDRESS</value>
          </property>

          <property>
            <name>queueName</name>
            <value>default</value>
          </property>

          <property>
            <name>oozie.use.system.libpath</name>
            <value>true</value>
          </property>

          <property>
            <name>hiveScript</name>
            <value>wasbs://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie/useooziewf.hql</value>
          </property>

          <property>
            <name>hiveTableName</name>
            <value>mobilecount</value>
          </property>

          <property>
            <name>hiveDataFolder</name>
            <value>wasbs://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie/data</value>
          </property>

          <property>
            <name>sqlDatabaseConnectionString</name>
            <value>"jdbc:sqlserver://serverName.database.windows.net;user=adminLogin;password=adminPassword;database=oozietest"</value>
          </property>

          <property>
            <name>sqlDatabaseTableName</name>
            <value>mobiledata</value>
          </property>

          <property>
            <name>user.name</name>
            <value>YourName</value>
          </property>

          <property>
            <name>oozie.wf.application.path</name>
            <value>wasbs://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie</value>
          </property>
        </configuration>

    * Az összes előfordulását cserélni **wasbs://mycontainer@mystorageaccount.blob.core.windows.net** a korábban kapott értéket.

    > [AZURE.WARNING] A teljes WASB elérési útját, az elérési út részeként tároló és tároló fiókkal kell használnia. A rövid formátumban (wasbs: / / /) okoz a RunHiveScript művelet sikertelen, ha a feladat azonnal elindul.

    * **JOBTRACKERADDRESS** cserélje le a korábban kapott JobTracker/erőforrás-kezelő címet.

    * A HDInsight fürt azonosítóját a login name **Saját_név** cserélje ki.

    * **Kiszolgálónév** **adminLogin**és **adminPassword** cserélje az Azure SQL-adatbázis adatait.

    A legtöbb információ a fájlban lévő használják (például ${nameNode}.) workflow.xml vagy ooziewf.hql fájlokban használt értékek kitöltéséhez

    > [AZURE.NOTE] A **oozie.wf.application.path** bejegyzés határozza meg, hol találhatók a workflow.xml fájl a feladat, amely tartalmazza a munkafolyamat futtatta.

2. Használja a Ctrl-X, **Y** és, majd a **meg az ENTER billentyűt** a fájl mentéséhez.

##<a name="submit-and-manage-the-job"></a>Űrlapadatok elküldése és a feladat kezelése

Az alábbi lépéseket a Oozie paranccsal elküldéséhez és a fürt Oozie-munkafolyamatok kezelése. A Oozie parancs nem a böngészőbarát felhasználói felülete fölé a [Oozie REST API -t](https://oozie.apache.org/docs/4.1.0/WebServicesAPI.html).

> [AZURE.IMPORTANT] A Oozie parancs használatakor a teljesen minősített tartománynév a HDInsight headnode kell használnia. A teljesen minősített tartománynév csak érhető el a fürt, vagy ha a fürt ugyanazon a hálózaton más számítógépről egy Azure virtuális hálózaton.

1. Szerezze be az URL-címet a Oozie szolgáltatás használja a következő:

        sed -n '/<name>oozie.base.url/,/<\/value>/p' /etc/oozie/conf/oozie-site.xml

    Ez értéket ad eredményül a következőhöz hasonló:

        <name>oozie.base.url</name>
        <value>http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:11000/oozie</value>

    A **http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:11000/oozie** rész a Oozie paranccsal URL-címe.

2. Hozzon létre egy környezeti változóba az URL-t, így nem kell írja be a minden parancs használata a következő:

        export OOZIE_URL=http://HOSTNAMEt:11000/oozie

    Cserélje ki a korábban kapott egy URL-CÍMÉT.

3. A következő segítségével a feladat elküldése:

        oozie job -config job.xml -submit

    Ez a feladat adatainak betölti az **job.xml** és elküldi Oozie, de nem futtatható.

    Ha befejeződött a parancsot, akkor térjen vissza a feladat azonosítója. Ha például `0000005-150622124850154-oozie-oozi-W`. Ez a feladat kezelése lesz.

4. A következő parancsot a feladat állapotának megtekintése Adja meg a projekt Azonosítóját adja vissza az előző parancs:

        oozie job -info <JOBID>

    Ez által visszaadható információk az alábbihoz hasonló.

        Job ID : 0000005-150622124850154-oozie-oozi-W
        ------------------------------------------------------------------------------------------------------------------------------------
        Workflow Name : useooziewf
        App Path      : wasbs:///tutorials/useoozie
        Status        : PREP
        Run           : 0
        User          : USERNAME
        Group         : -
        Created       : 2015-06-22 15:06 GMT
        Started       : -
        Last Modified : 2015-06-22 15:06 GMT
        Ended         : -
        CoordAction ID: -
        ------------------------------------------------------------------------------------------------------------------------------------

    A feladat állapota `PREP`, ami azt jelenti, hogy azt elindították, de még nem kezdődött.

4. A következő segítségével a projekt indítása:

        oozie job -start JOBID

    Ez a parancs után ellenőrizze az állapotot, ha futó állapotban legyen, és információt vissza kell belül a projekt a tevékenységek.

5. Ha a feladat sikeresen befejeződött, ellenőrizheti, hogy az adatok hozza létre, és az SQL-adatbázis táblázat exportálása a következő parancsok használatával:

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D oozietest

    A a `1>` kéri, írja be a következőt:

        SELECT * FROM mobiledata
        GO

    A következőhöz hasonló adatokat kell megjelennie:

        deviceplatform  count
        Android 31591
        iPhone OS       22731
        proprietary development 3
        RIM OS  3464
        Unknown 213
        Windows Phone   1791
        (6 rows affected)

További információt a Oozie parancs olvassa el a [Oozie parancssori eszköz](https://oozie.apache.org/docs/4.1.0/DG_CommandLineTool.html)című témakört.

##<a name="oozie-rest-api"></a>Oozie REST API-val

A Oozie REST API-t a saját Oozie verzióval használható eszközök összeállítása teszi lehetővé. Az alábbiakban HDInsight a Oozie REST API kapcsolatos információk:

* **URI**: A REST API webböngészőn keresztül elérhetők a fürthöz kívül`https://CLUSTERNAME.azurehdinsight.net/oozie`

* **Hitelesítés**: az fürt HTTP-fiókot (rendszergazda), és jelszóval API-nak kell hitelesítést végezni. Példa:

        curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/oozie/versions

Oozie REST API-t használatával kapcsolatos további tudnivalókért olvassa el a [Oozie Web Services API](https://oozie.apache.org/docs/4.1.0/WebServicesAPI.html)című témakört.

##<a name="oozie-web-ui"></a>Oozie webes felhasználói felület

A Oozie webes felület a fürt Oozie feladatok állapotának webes betekintést biztosít. Lehetővé teszi a feladat állapotát, a feladat definíciója, konfigurációs, egy diagramon a műveletek a feladatot, és a feladat naplók megtekintéséhez. A részletezése feladat műveletekhez.

A Oozie webes felület elérésére, kövesse az alábbi lépéseket:

1. Hozzon létre egy SSH alagutas a HDInsight fürthöz. Ennek módjáról a további tudnivalókért lásd [Használata SSH Tunneling Ambari webes felület, erőforrás-kezelő, JobHistory, NameNode, Oozie, és más webes Felhasználóifelület-féle eléréséhez](hdinsight-linux-ambari-ssh-tunnel.md).

2. Egy alagutas létrehozása után a webböngészőben nyissa meg a Ambari webes felhasználói felület. A URI-Ambari webhely **https://CLUSTERNAME.azurehdinsight.net**. **CLUSTERNAME** cserélje le a HDInsight Linux-alapú fürt nevét.

3. Válassza a lap bal szélén, **Oozie**, majd a **Tartalom**, és végül **Oozie webes felületének**.

    ![a menük képe](./media/hdinsight-use-oozie-linux-mac/ooziewebuisteps.png)

4. A Oozie webes felhasználói felületének alapértelmezett futó munkafolyamat-feladatok megjelenítése. Munkafolyamat-feladatok megtekintéséhez kattintson az **Összes feladatot**.

    ![Az összes feladat jelenik meg](./media/hdinsight-use-oozie-linux-mac/ooziejobs.png)

5. Jelölje be a feladattal kapcsolatos további információk megtekintése egy feladatot.

    ![Projekt adatai](./media/hdinsight-use-oozie-linux-mac/jobinfo.png)

6. A projekt adatai lapon megtekintheti az egyszerű feladat információk, valamint a feladat belül az egyes műveletek. A lapok használata a felső részén található meg a feladat definíciója, feladat konfigurálása access a feladat napló vagy egy irányított aciklikus Graph (DAG) a feladat megtekintése.

    * **Feladat napló**: a **GetLogs** gombbal minden naplók beszerzése a feladatot, vagy a naplók szűréséhez használni a **Adja meg a keresési szűrő** mező

        ![Feladat napló](./media/hdinsight-use-oozie-linux-mac/joblog.png)

    * **JobDAG**: A DAG egy grafikus az adatok elérési utak venni a munkafolyamaton keresztül – áttekintés

        ![Feladat DAG](./media/hdinsight-use-oozie-linux-mac/jobdag.png)

7. A **Projekt adatai** lapon válassza a műveletek egyikét a művelet adatainak állapotba kerül. Válassza például a **RunHiveScript** műveletet.

    ![Művelet információ](./media/hdinsight-use-oozie-linux-mac/action.png)

8. Megtekintheti a részleteket a művelet, beleértve a **Konzol URL-CÍMÉT**, használt tekintheti meg a projektre vonatkozóan JobTracker információkat mutató hivatkozást.

##<a name="scheduling-jobs"></a>Feladatok ütemezése

A koordinátor lehetővé teszi, hogy adja meg a Kezdés, a befejezési és a feladatok előfordulást gyakorisági, hogy azok az egyes alkalommal ütemezhető.

A munkafolyamat ütemezéséről, kövesse az alábbi lépéseket:

1. A következő segítségével **coordinator.xml**nevű új fájl létrehozása:

        nano coordinator.xml

    A fájl tartalmát a következő használható:

        <coordinator-app name="my_coord_app" frequency="${coordFrequency}" start="${coordStart}" end="${coordEnd}" timezone="${coordTimezone}" xmlns="uri:oozie:coordinator:0.4">
          <action>
            <workflow>
              <app-path>${workflowPath}</app-path>
            </workflow>
          </action>
        </coordinator-app>

    Megjegyzés: Ez a használó `${...}` változót, amely a feladat definíciója értékek váltja fel. A változók a következők:

    * **{coordFrequency} $**: a feladat példánya fut között eltelt idő
    * **{coordStart} $**: A projekt kezdési ideje
    * **{coordEnd} $**: A projekt befejezési idő
    * **{coordTimezone} $**: koordinátor feladatok vannak a rögzített időzónájának nincs nyári megtakarítási ideje (UTC használatával általában jeleníti meg). Ez az időzóna van néven "Oozie feldolgozás időzóna"
    * **{wfPath} $**: a workflow.xml elérési útja

2. Használja a Ctrl-X, **Y** és, majd a **meg az ENTER billentyűt** a fájl mentéséhez.

3. Másolja a vágólapra a munkakönyvtár ehhez a feladathoz használja a következő:

        hadoop fs -copyFromLocal coordinator.xml /tutorials/useoozie/coordinator.xml

4. A következő használja a **job.xml** fájl módosításához:

        nano job.xml

    A következő módosításokat:

    * Módosítás `<name>oozie.wf.application.path</name>` való `<name>oozie.coord.application.path</name>`. Ez a tulajdonság utasítja Oozie a koordinátor fájl futtatására helyett a munkafolyamat-fájl

    * A következő lesz beállítja a coordinator.xml használt mutasson arra a helyre, a workflow.xml változó hozzáadása:

            <property>
              <name>workflowPath</name>
              <value>wasbs://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie</value>
            </property>

        **Mycontainer** és **mystorageaccount** értékeket cserélje más bejegyzések a job.xml fájlban lévő értékekben.

    * Adja hozzá a következő, amelyek határozzák meg a Kezdés, a befejezési és a gyakoriság a coordinator.xml fájl használható:

            <property>
              <name>coordStart</name>
              <value>2015-06-25T12:00Z</value>
            </property>

            <property>
              <name>coordEnd</name>
              <value>2015-06-27T12:00Z</value>
            </property>

            <property>
              <name>coordFrequency</name>
              <value>1440</value>
            </property>

            <property>
              <name>coordTimezone</name>
              <value>UTC</value>
            </property>

        Ezek a 12:00 PM 2015 június 25 a kezdési idő, a június 27th, 2015, záró időpontja és a napi operációs rendszert futtató a feladat időköze beállítása (a gyakoriság argumentum, perc, így a 24 óra x 60 perc = 1440 perc.) Végezetül UTC az időzóna van beállítva.

5. Használja a Ctrl-X, **Y** és, majd a **meg az ENTER billentyűt** a fájl mentéséhez.

6. A feladat futtatása, használja az alábbi parancsot:

        oozie job -config job.xml -run

    A küldés, és indítsa el a feladat.

7. Ha keresse fel a Oozie webes felület, és válassza ki a **Koordinátor feladatok** lap, a következőhöz hasonló adatokat kell tennie:

    ![koordinátor feladatok lapon](./media/hdinsight-use-oozie-linux-mac/coordinatorjob.png)

    Megjegyzés: a **Következő biztosítási esemény** bejegyzés; Ez akkor, ha a feladat következő fog futni.

8. Hasonlóan, mint a korábbi munkafolyamat feladata, kijelöli a projekt tétele a webhely felhasználói felület jelenik meg információ a projektre vonatkozóan:

    ![Koordinátor projekt adatai](./media/hdinsight-use-oozie-linux-mac/coordinatorjobinfo.png)

    Ne feledje, hogy ez csak látható sikeres futtatja a feladatot, nem pedig az egyes műveletek az ütemezett munkafolyamaton belül. Megjelenítése, jelölje ki azt a **művelet** bejegyzéseket. Ekkor megjelenik a korábbi munkafolyamat-feladat eredményező hasonló adatokat.

    ![Művelet információ](./media/hdinsight-use-oozie-linux-mac/coordinatoractionjob.png)

##<a name="troubleshooting"></a>Hibaelhárítás

Üzenetátvitel hibáit próbálják felderíteni Oozie feladatok, amikor a felhasználói felület Oozie lehetővé teszi, hogy egyszerűen tekintheti meg mindkét Oozie naplók megegyezik nagyon hasznos, valamint a JobTracker naplók MapReduce feladatokhoz, például a struktúra lekérdezések mutató hivatkozásokat. Az Általános hibaelhárítás mintájának kell:

1. Oozie webes felhasználói felületen, tekintse meg a feladatot.

2. Ha egy hiba vagy az adott tevékenység hiba, jelölje ki tekintheti meg, ha a **Hibaüzenet** mező további információt tartalmaz a hibáról.

3. Érhető el, ha az URL-cím használata a művelet (például JobTracker a naplók) további részletes adatainak megjelenítéséhez a műveletet.

A következő is előfordulhatnak meghatározott hibákról, és azok megoldását.

###<a name="ja009-cannot-initialize-cluster"></a>JA009: Nem sikerült inicializálni fürthöz

**A jelenség**: A feladat állapota **Felfüggesztve**változik. A feladat részletei RunHiveScript állapotát jelenik meg: **START_MANUAL**. Jelölje ki a műveletet az mutatni fogja azokat a következő hibaüzenet:

    JA009: Cannot initialize Cluster. Please check your configuration for map

**OK**: az WASB címekről a **job.xml** fájl nem tartalmaz a tárhely tároló vagy tároló fiók nevére. A WASB cím formátumot kell `wasbs://containername@storageaccountname.blob.core.windows.net`.

**Megoldás**: módosítsa a feladat WASB címekről.

###<a name="ja002-oozie-is-not-allowed-to-impersonate-ltuser"></a>JA002: Oozie nem engedélyezett megszemélyesítés &lt;felhasználói >

**A jelenség**: A feladat állapota **Felfüggesztve**változik. A feladat részletei RunHiveScript állapotát jelenik meg: **START_MANUAL**. Jelölje ki a műveletet az mutatni fogja azokat a következő hibaüzenet:

    JA002: User: oozie is not allowed to impersonate <USER>

**OK**: jelenlegi engedélybeállítások esetén nem engedélyezik a megadott felhasználói fiók való Oozie.

**Megoldás**: Oozie megszemélyesítés, a **felhasználók** csoportban lévő felhasználók számára engedélyezett. Használja a `groups USERNAME` , amelyek a felhasználói fiók tagja csoport megjelenítéséhez. Ha a felhasználó nem tagja a **felhasználó** csoportnak, vegye fel a felhasználót a csoport a következő parancs segítségével:

    sudo adduser USERNAME users

> [AZURE.NOTE] Eltarthat néhány percig, amíg előtt HDInsight ismer fel, hogy a felhasználó a csoport lett hozzáadva.

###<a name="launcher-error-sqoop"></a>Alkalmazásindító hiba (Sqoop)

**A jelenség**: A feladat állapota fog **KILLED**változik. A feladat részletei **hibaként**jelennek meg a RunSqoopExport állapotát. Jelölje ki a műveletet az mutatni fogja azokat a következő hibaüzenet:

    Launcher ERROR, reason: Main class [org.apache.oozie.action.hadoop.SqoopMain], exit code [1]

**OK**: Sqoop nem tudja töltse be az adatbázis-illesztőprogram szükséges a hozzáférést az adatbázishoz.

**Megoldás**: amikor Sqoop egy Oozie feldolgozás, meg kell adnia az adatbázis-illesztőprogram az a más forrásokból (például a workflow.xml,) a projekt által használt.

Is hivatkoznia kell az adatbázis-illesztőprogram tartalmazó az archiválás a `<sqoop>...</sqoop>` a workflow.xml szakaszát.

Például a feladat a dokumentumban, használja az alábbi lépéseket:

1. Másolja a sqljdbc4.1.jar fájlt a /tutorials/useoozie könyvtár:

         hadoop fs -copyFromLocal /usr/share/java/sqljdbc_4.1/enu/sqljdbc41.jar /tutorials/useoozie/sqljdbc41.jar

2. Módosítsa a következő fenti új sor hozzáadása a workflow.xml `</sqoop>`:

        <archive>sqljdbc41.jar</archive>

##<a name="next-steps"></a>Következő lépések
Ebben az oktatóanyagban szüksége a tanultakhoz megadása egy Oozie munkafolyamatot, és hogyan tehető függővé egy Oozie feladatot. További tájékoztatás a HDInsight, az alábbi cikkekben talál:

- [Időalapú Oozie koordinátor használata hdinsight szolgáltatáshoz][hdinsight-oozie-coordinator-time]
- [Töltse fel az adatok HDInsight a Hadoop feladatokhoz][hdinsight-upload-data]
- [A HDInsight Hadoop Sqoop használata][hdinsight-use-sqoop]
- [A HDInsight Hadoop struktúra használata][hdinsight-use-hive]
- [A HDInsight Hadoop malac használata][hdinsight-use-pig]
- [Fejleszthet olyan Java MapReduce programok hdinsight szolgáltatáshoz][hdinsight-develop-mapreduce]


[hdinsight-cmdlets-download]: http://go.microsoft.com/fwlink/?LinkID=325563



[azure-data-factory-pig-hive]: ../data-factory/data-factory-data-transformation-activities.md
[hdinsight-oozie-coordinator-time]: hdinsight-use-oozie-coordinator-time.md
[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-storage]: hdinsight-use-blob-storage.md
[hdinsight-get-started]: hdinsight-get-started.md


[hdinsight-use-sqoop]: hdinsight-use-sqoop-mac-linux.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-storage]: hdinsight-use-blob-storage.md
[hdinsight-get-started-emulator]: hdinsight-get-started-emulator.md

[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md

[sqldatabase-create-configue]: sql-database-create-configure.md
[sqldatabase-get-started]: sql-database-get-started.md

[azure-create-storageaccount]: storage-create-storage-account.md

[apache-hadoop]: http://hadoop.apache.org/
[apache-oozie-400]: http://oozie.apache.org/docs/4.0.0/
[apache-oozie-332]: http://oozie.apache.org/docs/3.3.2/

[powershell-download]: http://azure.microsoft.com/downloads/
[powershell-about-profiles]: http://go.microsoft.com/fwlink/?LinkID=113729
[powershell-install-configure]: powershell-install-configure.md
[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-script]: https://technet.microsoft.com/en-us/library/ee176961.aspx

[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[img-workflow-diagram]: ./media/hdinsight-use-oozie/HDI.UseOozie.Workflow.Diagram.png
[img-preparation-output]: ./media/hdinsight-use-oozie/HDI.UseOozie.Preparation.Output1.png
[img-runworkflow-output]: ./media/hdinsight-use-oozie/HDI.UseOozie.RunWF.Output.png

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx
