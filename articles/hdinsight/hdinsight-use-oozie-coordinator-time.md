<properties
    pageTitle="Időalapú Hadoop Oozie koordinátor használata a HDInsight |} Microsoft Azure"
    description="Időalapú Hadoop Oozie koordinátor HDInsight, nagy adatok szolgáltatás használja. Megtudhatja, hogy miként Oozie munkafolyamatok és koordinátorok határozza meg, és nyújtson be a feladatokat."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/25/2016"
    ms.author="jgao"/>


# <a name="use-time-based-oozie-coordinator-with-hadoop-in-hdinsight-to-define-workflows-and-coordinate-jobs"></a>A HDInsight Hadoop definiálása munkafolyamatok és feladatok koordinálása időalapú Oozie koordinátor használata

Ebben a cikkben megismerheti, hogy munkafolyamatok és koordinátorok megadása, és hogy miként indíthatja el a koordinátor feladatok, ideje alapján. [A HDInsight használata Oozie] folyamatát hasznos[ hdinsight-use-oozie] előtt, olvassa el a jelen cikk. Kívül Oozie akkor is ütemezhetnek Azure Data Factory használatával. Azure Data Factory című témakörben talál [használata malac és az adatok gyári struktúra](../data-factory/data-factory-data-transformation-activities.md).

> [AZURE.NOTE] Ez a cikk a Windows-alapú HDInsight fürtre szükséges. Oozie, többek között a feladatok időalapú Linux-alapú fürtre, használatáról bővebben lásd: [Használata Oozie a Hadoop meghatározása és a HDInsight Linux-alapú munkafolyamat futtatása](hdinsight-use-oozie-linux-mac.md)

##<a name="what-is-oozie"></a>Mi az Oozie

Apache Oozie munkafolyamat/effektusával rendszer kezelése a Hadoop feladatokat. A Hadoop Papírhalom integrálva van, illetve Hadoop feladatok Apache MapReduce, Apache malac, Apache struktúra és Apache Sqoop támogatja. Azt is használható a rendszer, például Java-programok vagy rendszerhéj parancsfájlok jellemző feladatok ütemezése.

Az alábbi képen látható, a munkafolyamat kívánja végrehajtani:

![Munkafolyamat-diagramokhoz.][img-workflow-diagram]

A munkafolyamat két művelet tartalmazza:

1. A struktúra művelet megszámlálására log4j naplófájlban napló szintű típusonként HiveQL parancsfájl fut. Minden egyes log4j napló kattintva jelenítse meg a típus és súlyosságát, például egy [NAPLÓZÁSI szintjének] mezőt tartalmazó mezők sor áll:

        2012-02-03 18:35:34 SampleClass6 [INFO] everything normal for id 577725851
        2012-02-03 18:35:34 SampleClass4 [FATAL] system problem at id 1991281254
        2012-02-03 18:35:34 SampleClass3 [DEBUG] detail for id 1304807656
        ...

    A struktúra eredményébe hasonlít:

        [DEBUG] 434
        [ERROR] 3
        [FATAL] 1
        [INFO]  96
        [TRACE] 816
        [WARN]  4

    Struktúra kapcsolatos további tudnivalókért lásd: a [Használata a HDInsight struktúra][hdinsight-use-hive].

2.  Egy Sqoop művelet exportálása a HiveQL művelet eredménye Azure SQL-adatbázisban. Sqoop kapcsolatos további tudnivalókért lásd: [A HDInsight használata Sqoop][hdinsight-use-sqoop].

> [AZURE.NOTE] A HDInsight fürt támogatott Oozie verziók című [HDInsight által biztosított fürt verziók újdonságai?] [hdinsight-versions].


##<a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban megkezdése előtt a következőket kell rendelkeznie:

- **Azure PowerShell munkaállomás**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

- **Egy HDInsight fürt**. Egy HDInsight fürthöz létrehozásával kapcsolatos további tudnivalókért lásd: [Hozzon létre HDInsight fürt][hdinsight-provision], vagy az [első lépések a HDInsight][hdinsight-get-started]. Szüksége lesz az oktatóprogram folyamatát, az alábbi adatokat:

    <table border = "1">
    <tr><th>Fürt tulajdonság</th><th>A Windows PowerShell változó neve</th><th>Érték</th><th>Leírás</th></tr>
    <tr><td>HDInsight fürt neve</td><td>$clusterName</td><td></td><td>A HDInsight fürt, amelyen ebben az oktatóanyagban fog futni.</td></tr>
    <tr><td>HDInsight fürt felhasználónév</td><td>$clusterUsername</td><td></td><td>A HDInsight fürt felhasználó nevét. </td></tr>
    <tr><td>HDInsight fürt felhasználó jelszavát </td><td>$clusterPassword</td><td></td><td>A HDInsight fürt felhasználó jelszavát.</td></tr>
    <tr><td>Azure tároló fióknév</td><td>$storageAccountName</td><td></td><td>A HDInsight fürthöz elérhető Azure tároló fiók. Ebben az oktatóanyagban használja az alapértelmezett tárterület-fiók a fürt rendelkezést során megadott.</td></tr>
    <tr><td>Azure Blob-tárolóhoz neve</td><td>$containerName</td><td></td><td>Ebben a példában a Azure Blob-tároló tároló, amellyel az alapértelmezett HDInsight fürt fájlrendszer használja. Alapértelmezés szerint a HDInsight fürt nevével megegyező rendelkezik.</td></tr>
    </table>

- **Az Azure SQL-adatbázishoz**. Meg kell adnia az SQL-adatbázis kiszolgálója engedélyezze, hogy hozzáférjen a munkaállomásról tűzfal szabályt. A utasítások létrehozása az Azure SQL-adatbázis, és konfigurálása a tűzfalat, az [első lépések]című Azure SQL-adatbázis használata[sqldatabase-get-started]. Ez a cikk a Windows PowerShell-parancsprogramot, ebben az oktatóanyagban van szüksége Azure SQL-adatbázis táblázat létrehozásához biztosít.

    <table border = "1">
    <tr><th>SQL-adatbázis tulajdonság</th><th>A Windows PowerShell változó neve</th><th>Érték</th><th>Leírás</th></tr>
    <tr><td>Az SQL server adatbázisnév</td><td>$sqlDatabaseServer</td><td></td><td>Az SQL-adatbáziskiszolgálóhoz, amelyhez Sqoop exportálja az adatokat. </td></tr>
    <tr><td>SQL-adatbázis bejelentkezési neve</td><td>$sqlDatabaseLogin</td><td></td><td>SQL-adatbázis bejelentkezési nevét.</td></tr>
    <tr><td>SQL-adatbázis bejelentkezési jelszavát</td><td>$sqlDatabaseLoginPassword</td><td></td><td>SQL-adatbázis bejelentkezési jelszavát.</td></tr>
    <tr><td>SQL-adatbázis neve</td><td>$sqlDatabaseName</td><td></td><td>Az Azure SQL-adatbázissal, amelyhez Sqoop exportálja az adatokat. </td></tr>
    </table>

    > [AZURE.NOTE] Alapértelmezés szerint az Azure SQL-adatbázis Azure szolgáltatásokból, például az Azure hdinsight szolgáltatáshoz engedélyezi a csatlakozást. Ha a tűzfal beállítás be van kapcsolva, engedélyeznie kell azt az Azure portálról. SQL-adatbázis létrehozása és konfigurálása a tűzfalszabályokat kapcsolatos útmutatás, [létrehozása és SQL-adatbázis konfigurálása]című[sqldatabase-get-started].


> [AZURE.NOTE] Fill-in az értékeket a táblázatokban. Ebben az oktatóanyagban keresztül hasznos lesz.


##<a name="define-oozie-workflow-and-the-related-hiveql-script"></a>Oozie munkafolyamat és a kapcsolódó HiveQL parancsfájl meghatározása

Munkafolyamat-definíciók Oozie írt hPDL (egy XML folyamat definition language). Az alapértelmezett munkafolyamat fájl neve *workflow.xml*.  Használni mentése helyi meghajtóra a munkafolyamat-fájlt, és ezután beállítaná őket a HDInsight fürt később az oktatóprogram Azure PowerShell használatával.

A munkafolyamat a struktúra műveletet HiveQL parancsfájl hívások. A parancsprogram három HiveQL kimutatások tartalmazza:

1. **Húzza a TABLE utasítás** a log4j struktúratáblával törli, ha van ilyen.
2. **A CREATE TABLE utasítás** létrehoz egy log4j külső struktúratáblával, amely azt a helyet, a naplófájl log4j; mutat
3.  **A log4j naplófájl helyét**. A mező határolójel ",". Az alapértelmezett sor "\n" határolójel. Egy külső struktúratáblával elkerülése érdekében az adatfájl eltávolítása az eredeti helyre a csoportból, abban az esetben, ha többször is a Oozie munkafolyamatot futtatni szeretné használják.
3. **A BESZÚRÁSA FELÜLÍRJA a kimutatás** a log4j struktúratáblával napló szintű típusonként előfordulását megszámolja, és a kimeneti egy Azure Blob-tároló helyre menti.

**Megjegyzés**: a struktúra elérési út ismert probléma. Ha egy Oozie feladat elküldése futtatható ezt a problémát. A probléma az az utasításokat a TechNet Wikijében található: [HDInsight-struktúra hiba: nem lehet átnevezni][technetwiki-hive-error].

**A munkafolyamat meghívása a HiveQL parancsfájl meghatározása**

1. Hozzon létre egy szövegfájlt, az alábbi tartalom:

        DROP TABLE ${hiveTableName};
        CREATE EXTERNAL TABLE ${hiveTableName}(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
        INSERT OVERWRITE DIRECTORY '${hiveOutputFolder}' SELECT t4 AS sev, COUNT(*) AS cnt FROM ${hiveTableName} WHERE t4 LIKE '[%' GROUP BY t4;

    Létezik a parancsfájl használt három változóval:

    - ${hiveTableName}
    - ${hiveDataFolder}
    - ${hiveOutputFolder}

    A munkafolyamat-definíciós fájl (ebben az oktatóanyagban workflow.xml) továbbítja ezeket az értékeket ez HiveQL parancsfájl futásidőben.

2. ANSI (ASCII) kódolással **C:\Tutorials\UseOozie\useooziewf.hql** mentse a fájlt. (Jegyzettömb használata, ha a szövegszerkesztőben nem nyújt ezt a lehetőséget.) A parancsprogram belül az oktatóprogram a HDInsight fürthöz fog telepítését.



**Munkafolyamat meghatározása**

1. Hozzon létre egy szövegfájlt, az alábbi tartalom:

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
                    <param>hiveOutputFolder=${hiveOutputFolder}</param>
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
                <arg>${hiveOutputFolder}</arg>
                <arg>-m</arg>
                <arg>1</arg>
                <arg>--input-fields-terminated-by</arg>
                <arg>"\001"</arg>
                </sqoop>
                <ok to="end"/>
                <error to="fail"/>
            </action>

            <kill name="fail">
                <message>Job failed, error message[${wf:errorMessage(wf:lastErrorNode())}] </message>
            </kill>

           <end name="end"/>
        </workflow-app>

    Vannak olyan definiált az munkafolyamatokban két művelet. A kezdő művelet *RunHiveScript*. A művelet fut, *az OK gombra*, a következő műveletet esetén *RunSqoopExport*.

    A RunHiveScript többváltozós tartalmaz. Az értékeket a elküldésekor a Oozie feladat a munkaállomásról Azure PowerShell használatával továbbítja.

    <table border = "1">
    <tr><th>Munkafolyamat-változó</th><th>Leírás</th></tr>
    <tr><td>${jobTracker}</td><td>Adja meg a Hadoop feladat nyilvántartása URL-CÍMÉT. Használja a <strong>jobtrackerhost:9010</strong> HDInsight fürt verzió 3.0-s és 2.0-s verziója.</td></tr>
    <tr><td>${nameNode}</td><td>Adja meg az URL-CÍMÉT a Hadoop neve csomópontot. Használja az alapértelmezett fájl rendszer wasbs: / / cím, például <i>wasbs: / /&lt;containerName&gt;@&lt;storageAccountName&gt;. blob.core.windows.net</i>.</td></tr>
    <tr><td>${Várólistanév}</td><td>Megadja, hogy a feladat terjeszteni várólista nevét. Használja az <strong>alapértelmezett</strong>.</td></tr>
    </table>


    <table border = "1">
    <tr><th>Művelet változó struktúra</th><th>Leírás</th></tr>
    <tr><td>${hiveDataFolder}</td><td>A forrás könyvtár a Struktúratáblát létrehozása parancsot.</td></tr>
    <tr><td>${hiveOutputFolder}</td><td>A Beszúrás FELÜLÍRJA a kimutatás kimeneti mappa.</td></tr>
    <tr><td>${hiveTableName}</td><td>Az adatfájlok log4j hivatkozó struktúratáblával neve.</td></tr>
    </table>


    <table border = "1">
    <tr><th>Sqoop művelet változó</th><th>Leírás</th></tr>
    <tr><td>${sqlDatabaseConnectionString}</td><td>SQL-adatbázis-kapcsolati karakterláncot.</td></tr>
    <tr><td>${sqlDatabaseTableName}</td><td>Az Azure SQL adatbázistáblát, ahol az adatok exportálásának módját.</td></tr>
    <tr><td>${hiveOutputFolder}</td><td>A kimenet mappát a struktúra BESZÚRÁSA FELÜLÍRJA a kimutatáshoz. Ez az ugyanabban a mappában Sqoop exportálásához (export-dir).</td></tr>
    </table>

    Oozie munkafolyamat és a munkafolyamat-műveletek használatával kapcsolatos további tudnivalókért lásd: [Apache Oozie 4.0 dokumentáció] [ apache-oozie-400] (a HDInsight fürt 3.0-s verziója) vagy [Apache Oozie 3.3.2 dokumentáció] [ apache-oozie-332] (a HDInsight fürt 2.1-es verzió).

2. ANSI (ASCII) kódolással **C:\Tutorials\UseOozie\workflow.xml** mentse a fájlt. (Jegyzettömb használata, ha a szövegszerkesztőben nem nyújt ezt a lehetőséget.)

**Koordinátor meghatározása**

1. Hozzon létre egy szövegfájlt, az alábbi tartalom:

        <coordinator-app name="my_coord_app" frequency="${coordFrequency}" start="${coordStart}" end="${coordEnd}" timezone="${coordTimezone}" xmlns="uri:oozie:coordinator:0.4">
           <action>
              <workflow>
                 <app-path>${wfPath}</app-path>
              </workflow>
           </action>
        </coordinator-app>

    Vannak az definíciós fájl használt öt változók:

  	| Változó          | Leírás |
  	| ------------------|------------ |
  	| ${coordFrequency} | Feladat szünet ideje. Gyakoriság mindig perc van megadva. |
  	| ${coordStart}     | Projekt kezdési ideje. |
  	| ${coordEnd}       | Projekt a befejezési idő. |
  	| ${coordTimezone}  | Oozie dolgozza rögzített időzónájának koordinátor feladatok nincs nyári mentés ideje (UTC használatával általában jeleníti meg). Ez az időzóna van néven "Oozie feldolgozás időzóna." |
  	| ${wfPath}         | A workflow.xml elérési útját.  Ha a munkafolyamat-fájl neve nem az alapértelmezett fájlnevet (workflow.xml), meg kell adnia. |

2. Mentse a fájlt az ANSI (ASCII) kódolással **C:\Tutorials\UseOozie\coordinator.xml** . (Jegyzettömb használata, ha a szövegszerkesztőben nem nyújt ezt a lehetőséget.)

##<a name="deploy-the-oozie-project-and-prepare-the-tutorial"></a>Telepítse az Oozie projektet, és Felkészülés az oktatóprogram

Futtatható az egy Azure PowerShell-parancsprogramot, hajtsa végre a következő:

- Másolja a vágólapra a HiveQL parancsfájl (useoozie.hql) Azure Blob-tárolóhoz wasbs:///tutorials/useoozie/useoozie.hql.
- Másolja a workflow.xml wasbs:///tutorials/useoozie/workflow.xml.
- Másolja a coordinator.xml wasbs:///tutorials/useoozie/coordinator.xml.
- Másolja az adatokat tartalmazó fájl (/ example/data/sample.log) való wasbs:///tutorials/useoozie/data/sample.log.
- Hozzon létre egy Azure SQL-adatbázis táblájának Sqoop exportálás adatok tárolására szolgáló. A táblanév nem *log4jLogCount*.

**HDInsight tároló ismertetése**

Adattárolás Azure Blob-tárolóhoz HDInsight használja. wasbs: / / a Microsoft Azure Blob-tárolóhoz a Hadoop elosztott fájlrendszer (Fájlrendszerhez) megvalósítása. További tudnivalókért lásd: [Azure blobtárolóhoz használata a HDInsight][hdinsight-storage].

Egy HDInsight fürthöz kiépítése meg, amikor Azure Blob-tároló fiók és az adott fiókból egy adott tároló van kijelölve alapértelmezett fájlrendszer, például a Fájlrendszerhez. A tároló fiók mellett felveheti további tárterület-fiókok ugyanabban az Azure-előfizetésben, vagy egy másik Azure előfizetésből a kiépítési folyamat során. További tárterület fiókok, felvétele témaköröket olvassa el [a rendelkezés HDInsight fürt][hdinsight-provision]. Az Azure PowerShell-parancsprogramot, ebben az oktatóanyagban használt leegyszerűsítése összes fájlt tárolja az alapértelmezett fájl rendszer tárolóban található */tutorials/useoozie*. Alapértelmezés szerint ez a tároló rendelkezik neve megegyezik a HDInsight fürt nevét.
Szintaxisa a következő:

    wasb[s]://<ContainerName>@<StorageAccountName>.blob.core.windows.net/<path>/<filename>

> [AZURE.NOTE] Csak a *wasb: / /* szintaxis HDInsight fürt 3.0-s verziója támogatott. A régebbi *asv: / /* szintaxis HDInsight 2.1-es és 1,6 fürt támogatott, de nem támogatja a HDInsight 3.0 fürt.

> [AZURE.NOTE] A wasb: / / elérési út virtuális. További információt talál az [Azure blobtárolóhoz használata a HDInsight][hdinsight-storage].

Az alapértelmezett fájlrendszer-tárolót tárolt fájl webböngészőn keresztül elérhetők HDInsight használatával az alábbi URL-címe (használok workflow.xml példaként) közül:

    wasbs://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie/workflow.xml
    wasbs:///tutorials/useoozie/workflow.xml
    /tutorials/useoozie/workflow.xml

A fájl elérésére, közvetlenül a tárterület-fiókot, a fájl nevét a blob van szükség:

    tutorials/useoozie/workflow.xml

**Belső és külső táblák struktúra megértése**

Néhány dolgot kell tudnia struktúra belső és külső tábláiból:

- A táblázat létrehozása parancs létrehoz egy belső tábla, más néven egy felügyelt. Az alapértelmezett tárolóban az adatfájl kell elhelyezni.
- A táblázat létrehozása parancs helyezi át az adatfájl /hive/raktári/<TableName> az alapértelmezett tároló mappájában.
- A külső tábla létrehozása parancs egy külső táblázatot hoz létre. Az alapértelmezett tároló kívül az adatfájl is található.
- A külső tábla létrehozása parancs ne helyezze át az adatokat tartalmazó fájl.
- A külső tábla létrehozása parancsot nem teszi lehetővé a mappán, a hely záradékban megadott almappákat is. Ez az az oka, miért teszi az oktatóanyagot, a sample.log fájl egy példányát.

További tudnivalókért lásd: [HDInsight: struktúra belső és külső táblák – bevezetés][cindygross-hive-tables].

**Az oktatóprogram előkészítése**

1. Nyissa meg a Windows PowerShell ISE (Windows 8 Start képernyője **PowerShell_ISE**írja be, és kattintson a **Windows PowerShell ISE**parancsra. További tudnivalókért lásd: a [Windows PowerShell indítása a Windows 8 vagy Windows][powershell-start]).
2. Az alsó ablaktáblában a következő parancsot az Azure előfizetés csatlakozhat:

        Add-AzureAccount

    Az Azure-fiók hitelesítő adatok megadását kéri. Ez a módszer előfizetés kapcsolat hozzáadása időtúllépés történik, és 12 óra elteltével be kell futtassa újra.

    > [AZURE.NOTE] Ha több Azure előfizetéssel rendelkezik, és az alapértelmezett előfizetés nem az adott szeretné használni, a <strong>Kijelölés-AzureSubscription</strong> parancsmag használatával jelöljön ki egy előfizetést.

3. A parancsprogram-ablakban másolja be a következőt, és állítsa az első hat változók:

        # WASB variables
        $storageAccountName = "<StorageAccountName>"
        $containerName = "<BlobStorageContainerName>"

        # SQL database variables
        $sqlDatabaseServer = "<SQLDatabaseServerName>"  
        $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
        $sqlDatabaseLoginPassword = "SQLDatabaseLoginPassword>"
        $sqlDatabaseName = "<SQLDatabaseName>"  
        $sqlDatabaseTableName = "log4jLogsCount"

        # Oozie files for the tutorial
        $hiveQLScript = "C:\Tutorials\UseOozie\useooziewf.hql"
        $workflowDefinition = "C:\Tutorials\UseOozie\workflow.xml"
        $coordDefinition =  "C:\Tutorials\UseOozie\coordinator.xml"

        # WASB folder for storing the Oozie tutorial files.
        $destFolder = "tutorials/useoozie"  # Do NOT use the long path here


    További leírását a változók című [Előfeltételek](#prerequisites) ebben az oktatóanyagban.

3. A következő hozzáfűzése a parancsfájl a parancsprogram-ablakban:

        # Create a storage context object
        $storageaccountkey = get-azurestoragekey $storageAccountName | %{$_.Primary}
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey

        function uploadOozieFiles()
        {
            Write-Host "Copy HiveQL script, workflow definition and coordinator definition ..." -ForegroundColor Green
            Set-AzureStorageBlobContent -File $hiveQLScript -Container $containerName -Blob "$destFolder/useooziewf.hql" -Context $destContext
            Set-AzureStorageBlobContent -File $workflowDefinition -Container $containerName -Blob "$destFolder/workflow.xml" -Context $destContext
            Set-AzureStorageBlobContent -File $coordDefinition -Container $containerName -Blob "$destFolder/coordinator.xml" -Context $destContext
        }

        function prepareHiveDataFile()
        {
            Write-Host "Make a copy of the sample.log file ... " -ForegroundColor Green
            Start-CopyAzureStorageBlob -SrcContainer $containerName -SrcBlob "example/data/sample.log" -Context $destContext -DestContainer $containerName -destBlob "$destFolder/data/sample.log" -DestContext $destContext
        }

        function prepareSQLDatabase()
        {
            # SQL query string for creating log4jLogsCount table
            $cmdCreateLog4jCountTable = " CREATE TABLE [dbo].[$sqlDatabaseTableName](
                    [Level] [nvarchar](10) NOT NULL,
                    [Total] float,
                CONSTRAINT [PK_$sqlDatabaseTableName] PRIMARY KEY CLUSTERED
                (
                [Level] ASC
                )
                )"

            #Create the log4jLogsCount table
            Write-Host "Create Log4jLogsCount table ..." -ForegroundColor Green
            $conn = New-Object System.Data.SqlClient.SqlConnection
            $conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabaseLoginPassword;Encrypt=true;Trusted_Connection=false;"
            $conn.open()
            $cmd = New-Object System.Data.SqlClient.SqlCommand
            $cmd.connection = $conn
            $cmd.commandtext = $cmdCreateLog4jCountTable
            $cmd.executenonquery()

            $conn.close()
        }

        # upload workflow.xml, coordinator.xml, and ooziewf.hql
        uploadOozieFiles;

        # make a copy of example/data/sample.log to example/data/log4j/sample.log
        prepareHiveDataFile;

        # create log4jlogsCount table on SQL database
        prepareSQLDatabase;

4. Kattintson a **Futtatás parancsfájl** , vagy nyomja le az **F5** futtatása. A kimenet lesz hasonló:

    ![A kimenet oktatóanyag előkészítése][img-preparation-output]

##<a name="run-the-oozie-project"></a>Futtassa a Oozie projekt

Azure PowerShell jelenleg nem nyújt bármely parancsmagok definiáló Oozie feladatokat. A **Invoke-RestMethod** parancsmag Oozie webszolgáltatásokhoz meghívásához is használhatja. A Oozie web services API a HTTP REST API-val JSON. A Oozie webszolgáltatásokhoz API-val kapcsolatos további tudnivalókért lásd: [Apache Oozie 4.0 dokumentáció] [ apache-oozie-400] (a HDInsight fürt 3.0-s verziója) vagy [Apache Oozie 3.3.2 dokumentáció] [ apache-oozie-332] (a HDInsight fürt 2.1-es verzió).

**Egy Oozie feladat elküldése**

1. Nyissa meg a Windows PowerShell ISE (Windows 8 Start képernyője, írja be a **PowerShell_ISE**, és kattintson a **Windows PowerShell ISE**parancsra. További tudnivalókért lásd: a [Windows PowerShell indítása a Windows 8 vagy Windows][powershell-start]).

3. A parancsprogram-ablakban másolja be a következőt, és állítsa az első 14 változók (azonban kihagyja **$storageUri**).

        #HDInsight cluster variables
        $clusterName = "<HDInsightClusterName>"
        $clusterUsername = "<HDInsightClusterUsername>"
        $clusterPassword = "<HDInsightClusterUserPassword>"

        #Azure Blob storage (WASB) variables
        $storageAccountName = "<StorageAccountName>"
        $storageContainerName = "<BlobContainerName>"
        $storageUri="wasbs://$storageContainerName@$storageAccountName.blob.core.windows.net"

        #Azure SQL database variables
        $sqlDatabaseServer = "<SQLDatabaseServerName>"
        $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
        $sqlDatabaseLoginPassword = "<SQLDatabaseloginPassword>"
        $sqlDatabaseName = "<SQLDatabaseName>"  

        #Oozie WF/coordinator variables
        $coordStart = "2014-03-21T13:45Z"
        $coordEnd = "2014-03-21T13:45Z"
        $coordFrequency = "1440"    # in minutes, 24h x 60m = 1440m
        $coordTimezone = "UTC"  #UTC/GMT

        $oozieWFPath="$storageUri/tutorials/useoozie"  # The default name is workflow.xml. And you don't need to specify the file name.
        $waitTimeBetweenOozieJobStatusCheck=10

        #Hive action variables
        $hiveScript = "$storageUri/tutorials/useoozie/useooziewf.hql"
        $hiveTableName = "log4jlogs"
        $hiveDataFolder = "$storageUri/tutorials/useoozie/data"
        $hiveOutputFolder = "$storageUri/tutorials/useoozie/output"

        #Sqoop action variables
        $sqlDatabaseConnectionString = "jdbc:sqlserver://$sqlDatabaseServer.database.windows.net;user=$sqlDatabaseLogin@$sqlDatabaseServer;password=$sqlDatabaseLoginPassword;database=$sqlDatabaseName"
        $sqlDatabaseTableName = "log4jLogsCount"

        $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
        $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    További leírását a változók című [Előfeltételek](#prerequisites) ebben az oktatóanyagban.

    $coordstart és $coordend a munkafolyamatot, kezdési és befejezési idő. Keressen "utc idő" a Bing.com-on, Egyezményes/GMT ki. A $coordFrequency (perc) azt szeretné, hogy futtassa a munkafolyamatot gyakoriságának van.

3. A következő hozzáfűzése a parancsfájl. Ebben a részben a Oozie tartalom határozza meg:

        #OoziePayload used for Oozie web service submission
        $OoziePayload =  @"
        <?xml version="1.0" encoding="UTF-8"?>
        <configuration>

           <property>
               <name>nameNode</name>
               <value>$storageUrI</value>
           </property>

           <property>
               <name>jobTracker</name>
               <value>jobtrackerhost:9010</value>
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
               <name>oozie.coord.application.path</name>
               <value>$oozieWFPath</value>
           </property>

           <property>
               <name>wfPath</name>
               <value>$oozieWFPath</value>
           </property>

           <property>
               <name>coordStart</name>
               <value>$coordStart</value>
           </property>

           <property>
               <name>coordEnd</name>
               <value>$coordEnd</value>
           </property>

           <property>
               <name>coordFrequency</name>
               <value>$coordFrequency</value>
           </property>

           <property>
               <name>coordTimezone</name>
               <value>$coordTimezone</value>
           </property>

           <property>
               <name>hiveScript</name>
               <value>$hiveScript</value>
           </property>

           <property>
               <name>hiveTableName</name>
               <value>$hiveTableName</value>
           </property>

           <property>
               <name>hiveDataFolder</name>
               <value>$hiveDataFolder</value>
           </property>

           <property>
               <name>hiveOutputFolder</name>
               <value>$hiveOutputFolder</value>
           </property>

           <property>
               <name>sqlDatabaseConnectionString</name>
               <value>&quot;$sqlDatabaseConnectionString&quot;</value>
           </property>

           <property>
               <name>sqlDatabaseTableName</name>
               <value>$SQLDatabaseTableName</value>
           </property>

           <property>
               <name>user.name</name>
               <value>admin</value>
           </property>

        </configuration>
        "@

    >[AZURE.NOTE] A fő és összehasonlítása az munkafolyamat Beküldési adatfájlban különbség a változó **oozie.coord.application.path**. Munkafolyamat-feladat elküldésekor használhatja **oozie.wf.application.path** helyette.

4. A következő hozzáfűzése a parancsfájl. Ebben a részben ellenőrzi a Oozie webes szolgáltatás állapota:

        function checkOozieServerStatus()
        {
            Write-Host "Checking Oozie server status..." -ForegroundColor Green
            $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/admin/status"
            $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds -OutVariable $OozieServerStatus

            $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
            $oozieServerSatus = $jsonResponse[0].("systemMode")
            Write-Host "Oozie server status is $oozieServerSatus..."

            if($oozieServerSatus -notmatch "NORMAL")
            {
                Write-Host "Oozie server status is $oozieServerSatus...cannot submit Oozie jobs. Check the server status and re-run the job."
                exit 1
            }
        }

5. A következő hozzáfűzése a parancsfájl. Ebben a részben egy Oozie projektet hoz létre:

        function createOozieJob()
        {
            # create Oozie job
            Write-Host "Sending the following Payload to the cluster:" -ForegroundColor Green
            Write-Host "`n--------`n$OoziePayload`n--------"
            $clusterUriCreateJob = "https://$clusterName.azurehdinsight.net:443/oozie/v2/jobs"
            $response = Invoke-RestMethod -Method Post -Uri $clusterUriCreateJob -Credential $creds -Body $OoziePayload -ContentType "application/xml" -OutVariable $OozieJobName -debug -Verbose

            $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
            $oozieJobId = $jsonResponse[0].("id")
            Write-Host "Oozie job id is $oozieJobId..."

            return $oozieJobId
        }

    > [AZURE.NOTE] Munkafolyamat-feladat elküldésekor hívás indítása a feladatot, a projekt létrehozását követően egy másik webszolgáltatás kell végeznie. Ebben az esetben a koordinátor feladat idő váltja ki. A feladat automatikusan elindul.

6. A következő hozzáfűzése a parancsfájl. Ebben a részben a Oozie feladat állapotának ellenőrzése:

        function checkOozieJobStatus($oozieJobId)
        {
            # get job status
            Write-Host "Sleeping for $waitTimeBetweenOozieJobStatusCheck seconds until the job metadata is populated in the Oozie metastore..." -ForegroundColor Green
            Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck

            Write-Host "Getting job status and waiting for the job to complete..." -ForegroundColor Green
            $clusterUriGetJobStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?show=info"
            $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $creds
            $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
            $JobStatus = $jsonResponse[0].("status")

            while($JobStatus -notmatch "SUCCEEDED|KILLED")
            {
                Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state...waiting $waitTimeBetweenOozieJobStatusCheck seconds for the job to complete..."
                Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck
                $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $creds
                $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
                $JobStatus = $jsonResponse[0].("status")
            }

            Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state!"
            if($JobStatus -notmatch "SUCCEEDED")
            {
                Write-Host "Check logs at http://headnode0:9014/cluster for detais."
                exit -1
            }
        }

7. (Nem kötelező) A következő hozzáfűzése a parancsfájl.

        function listOozieJobs()
        {
            Write-Host "Listing Oozie jobs..." -ForegroundColor Green
            $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/jobs"
            $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds

            write-host "Job ID                                   App Name        Status      Started                         Ended"
            write-host "----------------------------------------------------------------------------------------------------------------------------------"
            foreach($job in $response.workflows)
            {
                Write-Host $job.id "`t" $job.appName "`t" $job.status "`t" $job.startTime "`t" $job.endTime
            }
        }

        function ShowOozieJobLog($oozieJobId)
        {
            Write-Host "Showing Oozie job info..." -ForegroundColor Green
            $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/$oozieJobId" + "?show=log"
            $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds
            write-host $response
        }

        function killOozieJob($oozieJobId)
        {
            Write-Host "Killing the Oozie job $oozieJobId..." -ForegroundColor Green
            $clusterUriStartJob = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?action=kill" #Valid values for the 'action' parameter are 'start', 'suspend', 'resume', 'kill', 'dryrun', 'rerun', and 'change'.
            $response = Invoke-RestMethod -Method Put -Uri $clusterUriStartJob -Credential $creds | Format-Table -HideTableHeaders -debug
        }

7. A következő hozzáfűzése a parancsfájl:

        checkOozieServerStatus
        # listOozieJobs
        $oozieJobId = createOozieJob($oozieJobId)
        checkOozieJobStatus($oozieJobId)
        # ShowOozieJobLog($oozieJobId)
        # killOozieJob($oozieJobId)

    Ha szeretné futtatni, a további függvények, távolítsa el a # jelek.

7. Ha a HDinsight fürt 2.1-es verzió, a "https://$clusterName.azurehdinsight.net:443/oozie/v2/" a "https://$clusterName.azurehdinsight.net:443/oozie/v1/" helyére. HDInsight fürt 2.1-es verzió nem támogatja a 2-es verziójú webes szolgáltatások nem.

7. Kattintson a **Futtatás parancsfájl** , vagy nyomja le az **F5** futtatása. A kimenet lesz hasonló:

    ![Oktatóprogram munkafolyamat kimeneti futtatása][img-runworkflow-output]

8. Az SQL-adatbázis lásd: az exportált adatokat szeretne csatlakozni.

**A feladat hibanaplójának ellenőrzése**

A munkafolyamat elhárításához a Oozie naplófájl találhatók C:\apps\dist\oozie-3.3.2.1.3.2.0-05\oozie-win-distro\logs\Oozie.log a fürt headnode a. RDP a további tudnivalókért lásd [az Azure portálon felügyelete HDInsight fürt][hdinsight-admin-portal].

**Az oktatóprogram futtatnia.**

Futtassa a munkafolyamatot, hajtsa végre az alábbi műveleteket:

- A struktúra parancsfájl kimeneti fájl törlése
- A log4jLogsCount táblában lévő adatok törlése

Íme egy minta Windows PowerShell-parancsprogramot, amelyek segítségével használhatja:

    $storageAccountName = "<AzureStorageAccountName>"
    $containerName = "<ContainerName>"

    #SQL database variables
    $sqlDatabaseServer = "<SQLDatabaseServerName>"
    $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
    $sqlDatabaseLoginPassword = "<SQLDatabaseLoginPassword>"
    $sqlDatabaseName = "<SQLDatabaseName>"
    $sqlDatabaseTableName = "log4jLogsCount"

    Write-host "Delete the Hive script output file ..." -ForegroundColor Green
    $storageaccountkey = get-azurestoragekey $storageAccountName | %{$_.Primary}
    $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey
    Remove-AzureStorageBlob -Context $destContext -Blob "tutorials/useoozie/output/000000_0" -Container $containerName

    Write-host "Delete all the records from the log4jLogsCount table ..." -ForegroundColor Green
    $conn = New-Object System.Data.SqlClient.SqlConnection
    $conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabaseLoginPassword;Encrypt=true;Trusted_Connection=false;"
    $conn.open()
    $cmd = New-Object System.Data.SqlClient.SqlCommand
    $cmd.connection = $conn
    $cmd.commandtext = "delete from $sqlDatabaseTableName"
    $cmd.executenonquery()

    $conn.close()


##<a name="next-steps"></a>Következő lépések
Ebben az oktatóanyagban megtanulta azt egy Oozie munkafolyamatot, és egy Oozie koordinátornak megadása, és hogyan tehető függővé egy Oozie koordinátor feladat Azure PowerShell használatával. További tudnivalókért lásd: az alábbi cikkekben:

- [Első lépések a hdinsight szolgáltatáshoz][hdinsight-get-started]
- [Azure Blob-tárolóhoz használata hdinsight szolgáltatáshoz][hdinsight-storage]
- [HDInsight felügyelete Azure PowerShell használatával][hdinsight-admin-powershell]
- [Töltse fel az adatok hdinsight szolgáltatáshoz][hdinsight-upload-data]
- [HDInsight Sqoop használata][hdinsight-use-sqoop]
- [HDInsight struktúra használata][hdinsight-use-hive]
- [Malac használata hdinsight szolgáltatáshoz][hdinsight-use-pig]
- [Fejleszthet olyan Java MapReduce programok hdinsight szolgáltatáshoz][hdinsight-develop-java-mapreduce]



[hdinsight-cmdlets-download]: http://go.microsoft.com/fwlink/?LinkID=325563


[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md


[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-develop-java-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md

[sqldatabase-get-started]: ../sql-database/sql-database-get-started.md

[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]: ../storage-create-storage-account.md

[apache-hadoop]: http://hadoop.apache.org/
[apache-oozie-400]: http://oozie.apache.org/docs/4.0.0/
[apache-oozie-332]: http://oozie.apache.org/docs/3.3.2/

[powershell-download]: http://azure.microsoft.com/downloads/
[powershell-about-profiles]: http://go.microsoft.com/fwlink/?LinkID=113729
[powershell-install-configure]: ../powershell-install-configure.md
[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[img-workflow-diagram]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.Workflow.Diagram.png
[img-preparation-output]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.Preparation.Output1.png  
[img-runworkflow-output]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.RunCoord.Output.png  

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx
