<properties
    pageTitle="Feladat futtatása olyan Hadoop DocumentDB és HDInsight |} Microsoft Azure"
    description="Megismerheti egy egyszerű struktúra, Malac és MapReduce feladat futtatása DocumentDB és Azure hdinsight szolgáltatásból lehetőségre."
    services="documentdb"
    authors="dennyglee"
    manager="jhubbard"
    editor="mimig"
    documentationCenter=""/>


<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="denlee"/>

#<a name="DocumentDB-HDInsight"></a>Egy DocumentDB és HDInsight Hadoop feladat futtatása

Ebből az oktatóanyagból megtudhatja, hogy miként futtatása [Apache struktúra][apache-hive], [Apache malac][apache-pig], és [Apache Hadoop] [ apache-hadoop] MapReduce feladatok Azure hdinsight szolgáltatáshoz a Hadoop-összekötő DocumentDB meg. DocumentDB's Hadoop connector lehetővé teszi, hogy a használni kívánt a forrás- és a struktúra, Malac és MapReduce feladatokhoz gyűjtő DocumentDB. Ebben az oktatóanyagban használja DocumentDB az adatforrás és a cél Hadoop feladatokhoz.

Ebben az oktatóanyagban befejeztével is az alábbi kérdésekre választ:

- Hogyan adatok betöltése DocumentDB struktúra, Malac vagy MapReduce feladat használatával?
- Hogyan tárolja az adatokat a struktúra, Malac vagy MapReduce feladat használatával DocumentDB?

Azt javasoljuk, hogy az első lépések a következő videót, ahol DocumentDB és HDInsight struktúra feladat azt futtathatja.

> [AZURE.VIDEO use-azure-documentdb-hadoop-connector-with-azure-hdinsight]

Ezt követően lépjen vissza Ez a cikk hol akkor jelenik meg, hogyan futtatható analytics feladatok DocumentDB adatok a teljes részleteibe.

> [AZURE.TIP] Ebben az oktatóanyagban feltételezi, hogy nincs előzetes használatában Apache Hadoop, struktúra és/vagy malac. Ha új Apache Hadoop, struktúra és malac, azt javasoljuk, megtalálhatók az [Apache Hadoop dokumentáció][apache-hadoop-doc]. Ebben az oktatóanyagban is feltételezi, hogy DocumentDB előzetes használatát, és DocumentDB fiókkal rendelkezik. Ha még kezdő az DocumentDB, vagy egy DocumentDB fiókja nem rendelkezik, kérjük, olvassa el az [Első lépések] [ getting-started] lapot.

Nem időt vesz igénybe az oktatóprogram és egyszerűen szeretné teljes minta PowerShell-parancsfájlokat beszerzése struktúra, Malac és MapReduce? Nem probléma, el őket [az alábbi][documentdb-hdinsight-samples]. A letöltés e minták hql malac és java fájljait is tartalmazza.

## <a name="NewestVersion"></a>Legújabb verziója

<table border='1'>
    <tr><th>Összekötő Hadoop-verziója</th>
        <td>1.2.0</td></tr>
    <tr><th>Parancsfájl Uri</th>
        <td>https://portalcontent.BLOB.Core.Windows.NET/scriptaction/documentdb-hadoop-Installer-v04.ps1</td></tr>
    <tr><th>Módosítás dátuma</th>
        <td>04/26 és 2016-ban</td></tr>
    <tr><th>Támogatott HDInsight-verziókkal</th>
        <td>3.1, 3,2</td></tr>
    <tr><th>Módosítási napló</th>
        <td>Frissített DocumentDB Java SDK való 1.6.0</br>
            A forrás-és a gyűjtő particionált gyűjtemények hozzáadott támogatása</br>
        </td></tr>
</table>

## <a name="Prerequisites"></a>Előfeltételek
Ebben az oktatóanyagban utasításai előtt győződjön meg arról, hogy a következő:

- Egy DocumentDB fiókot, adatbázis és a webhelycsoport-dokumentumok belül. További tudnivalókért lásd: a [DocumentDB – első lépések][getting-started]. Mintaadatok importálása [DocumentDB importálása eszköz]DocumentDB fiókját[documentdb-import-data].
- Átviteli. Felolvassa és hdinsight írások a webhelycsoportok számára a kiosztott kérelem mennyiség felé lesznek megszámlálva. További tudnivalókért lásd: a [Provisioned átviteli, a kérelem egységek, és az adatbázis-műveletek][documentdb-manage-throughput].
- A kimeneti webhelycsoport belül minden további tárolt eljárás kapacitása. A tárolt eljárásokat az eredményül kapott dokumentumok átvitele használják. További tudnivalókért lásd: a [webhelycsoportok és kiépített átviteli][documentdb-manage-document-storage].
- Az eredményül kapott dokumentumait a struktúra, Malac vagy MapReduce feladatok kapacitása. További tudnivalókért lásd: [kezelése DocumentDB beosztását, és a teljesítmény][documentdb-manage-collections].
- [*Választható*] Egy további webhelycsoport kapacitása. További tudnivalókért lásd: a [Provisioned dokumentumok tárolása és index felsőbb][documentdb-manage-document-storage].

> [AZURE.WARNING] Egy új webhelycsoport alatt a feladatok létrehozásának elkerülése érdekében az eredményeket stdout nyomtatása, mentse a kimenet a WASB tároló, vagy adjon meg egy már meglévő webhelycsoport. Egy meglévő webhelycsoport megadása, ha új dokumentumok webhelycsoporton belüli jön létre, és már meglévő dokumentumokat csak érinti, ha nincs ütközés *azonosítók*. **Az összekötő automatikusan felülírja a meglévő dokumentumok azonosító ütközéseket**. Ez a funkció kikapcsolása a upsert beállítást megadva a hamis értékre. Upsert értéke HAMIS, és ütközés akkor történik, ha a Hadoop feladat meghiúsul; azonosító ütközés hibát jelez.


## <a name="ProvisionHDInsight"></a>Lépés: 1: Hozzon létre egy új HDInsight fürt
Ebben az oktatóanyagban az Azure portálról parancsfájl műveletet használja a HDInsight fürt testreszabása. Ebben az oktatóanyagban fogjuk használni az Azure-portálra a HDInsight fürt létrehozásához. PowerShell-parancsmagok vagy a HDInsight .NET SDK csomagjában talál utasításokat, nézze meg a [parancsfájlt művelettel testreszabása HDInsight fürt] [ hdinsight-custom-provision] cikk.

1. Jelentkezzen be az [Azure portál][azure-portal].

2. Kattintson a **+ Új** tetején a bal oldali navigációs sávján az új lap felső keresési sávján **HDInsight** keresése gombra.

3. A **Microsoft** által közzétett **HDInsight** az eredmények tetején jelenik meg. Kattintson rá, és kattintson a **Létrehozás**gombra.

4. Az új HDInsight fürtre létrehozása lap, adja meg a **Csoport nevét** , és jelölje ki az **előfizetés** hozhatók létre az erőforrás csoportban.

    <table border='1'>
        <tr><td>Csoport neve</td><td>A csoport nevét.<br/>
   DNS-névhez kell elindítani egy alfanumerikus karaktert végződjön és tartalmazhatnak kötőjeleket.<br/>
   A mező egy karakterláncot a 3 és 63 karakterei között kell lennie.</td></tr>
        <tr><td>Előfizetés neve</td>
            <td>Ha egynél több Azure-előfizetése van, jelölje ki az előfizetést, a HDInsight fürt fog tárolni. </td></tr>
    </table>

5. Kattintson a **Fürt típusának kijelölése** , majd állítsa be a következő tulajdonságokat a megadott értékeket.

    <table border='1'>
        <tr><td>Fürt típusa</td><td><strong>Hadoop</strong></td></tr>
        <tr><td>Réteg fürthöz</td><td><strong>Normál</strong></td></tr>
        <tr><td>Operációs rendszer</td><td><strong>A Windows</strong></td></tr>
        <tr><td>Verzió</td><td>legújabb verziója</td></tr>
    </table>

    Ezután kattintson a **Kijelölés**gombra.

    ![Adja meg a Hadoop HDInsight fürt kezdeti részletei][image-customprovision-page1]

6. Kattintson a **hitelesítő adatok** beállítása a bejelentkezési és távelérési hitelesítő adatait. Válassza a **fürt bejelentkezési felhasználónév** és a **fürt bejelentkezési jelszavát**.

    Ha azt szeretné, távolról a fürt be, válassza az *Igen* elemre a lap alján, és adja meg a felhasználónevet és jelszót.

7. Kattintson az **Adatforrás** a fő tartózkodási helyén az adatokhoz való hozzáférés megadását. Válassza a **Kijelölés módszer** , és adjon meg egy már meglévő tárterület-fiókot vagy hozzon létre egy újat.

8. Adja meg a ugyanazt a lap **Alapértelmezett tároló** és **helyét**. És kattintson a **Kijelölés**gombra.

    > [AZURE.NOTE] Jelöljön ki egy helyet a DocumentDB fiók terület jobb teljesítmény közelébe

8. Kattintson a **árak** , jelölje be a szám és a csomópontok típusát. Az alapértelmezett beállítások megőrzése, és később méretezni dolgozó csomópontok számának.

9. Kattintson a **választható beállítási**, majd a választható beállítási lap **Script műveletek** .

    Parancsfájl-műveletek írja be a következő információkat a HDInsight fürt testreszabásához.

    <table border='1'>
        <tr><th>A tulajdonság</th><th>Érték</th></tr>
        <tr><td>név</td>
            <td>Adja meg a parancsfájlt beavatkozásra.</td></tr>
        <tr><td>Parancsfájl URI</td>
            <td>Adja meg a parancsfájlt, ha testre szeretné szabni a fürt hív meg a URI.</br></br>
   Írja be: </br> <strong>https://portalcontent.blob.core.windows.net/scriptaction/documentdb-hadoop-installer-v04.ps1</strong>.</td></tr>
        <tr><td>Címsor</td>
            <td>Jelölje be az alakzatot a címsor csomópontot a PowerShell parancsprogramot futtatásához.</br></br>
            <strong>Ezt a jelölőnégyzetet</strong>.</td></tr>
        <tr><td>Dolgozói</td>
            <td>Jelölje be az alakzatot a dolgozó csomópontot a PowerShell parancsprogramot futtatásához.</br></br>
            <strong>Ezt a jelölőnégyzetet</strong>.</td></tr>
        <tr><td>Zookeeper</td>
            <td>Jelölje be az alakzatot a Zookeeper az PowerShell parancsprogramot futtatásához.</br></br>
            <strong>Nincs szükség</strong>.
            </td></tr>
        <tr><td>Paraméterek</td>
            <td>Szükség szerint a parancsfájlt, adja meg a paraméterek.</br></br>
   A          <strong>paraméter nincs szükség</strong>.</td></tr>
    </table>

10. Hozzon létre vagy egy új **Erőforráscsoport** , vagy használjon az Azure-előfizetés a meglévő erőforráscsoport.

11. Most megtudhatja **PIN-kód irányítópult** nyomon követheti a telepítés, és kattintson a **Létrehozás**gombra.

## <a name="InstallCmdlets"></a>Lépés: 2: Telepítse és állítsa be a Azure PowerShell

1. Azure PowerShell telepítése. Utasítások megtalálható [Itt][powershell-install-configure].

    > [AZURE.NOTE] Azt is megteheti csak a struktúra lekérdezések, használhatja HDInsight meg online struktúra szerkesztő. Kéri, jelentkezzen be az [Azure-portálon][azure-portal], a bal oldali ablaktáblában a HDInsight fürt listájának megtekintéséhez kattintson a **hdinsight szolgáltatásból lehetőségre** . Kattintson a struktúra lekérdezések futtatása kívánt fürtre, és kattintson a **Lekérdezés konzolt**.

2. Nyissa meg az Azure PowerShell integrált parancsfájlok környezetben:
    - Windows 8 vagy Windows Server 2012 vagy újabb rendszert futtató számítógépen a beépített keresési is használhatja. A kezdőképernyőn írja be a **powershell ise** , és kattintson az **ENTER billentyűt**.
    - Windows 8 vagy Windows Server 2012-nél korábbi verzióját futtató számítógépen a Start menü használata. A Start menüből írja be a **parancssor parancsot** a Keresés mezőbe, majd a listában az eredmények, kattintson a **parancssor parancsot**. A parancssorba írja be a **powershell_ise** , és kattintson az **ENTER billentyűt**.

3. Az Azure-fiók felvétele.
    1. Konzol mezőre írja be a **Hozzáadása-AzureAccount** , és kattintson az **ENTER billentyűt**.
    2. Írja be az e-mail címet az Azure-előfizetéséhez társított, és kattintson a **Tovább**gombra.
    3. Írja be a jelszót az Azure előfizetéshez.
    4. Kattintson a **Bejelentkezés**gombra.

4. Az alábbi ábra azonosítja a Azure PowerShell-parancsfájlokat környezet fontos részeit.

    ![Azure PowerShell-diagram][azure-powershell-diagram]

## <a name="RunHive"></a>Lépés 3: DocumentDB és HDInsight struktúra feladat futtatása

> [AZURE.IMPORTANT] Összes változó < > jelölt meg kell adni a megadott beállításokat használja.

1. A következő változók beállítása a PowerShell-parancsprogramot ablaktáblán.

        # Provide Azure subscription name, the Azure Storage account and container that is used for the default HDInsight file system.
        $subscriptionName = "<SubscriptionName>"
        $storageAccountName = "<AzureStorageAccountName>"
        $containerName = "<AzureStorageContainerName>"

        # Provide the HDInsight cluster name where you want to run the Hive job.
        $clusterName = "<HDInsightClusterName>"

2. <p>Lássunk hozzá a lekérdezési karakterlánc megépítése. Fogja írja be azt, hogy mi minden dokumentum rendszer által létrehozott időbélyegeket (_ts) struktúra lekérdezés, és egyedi azonosítói (_rid) a DocumentDB gyűjteményből minden dokumentum összeadja perc, majd tárolja az eredmények vissza egy új DocumentDB gyűjteménybe.</p>

    <p>Első lépésként struktúra táblázat létrehozása a DocumentDB gyűjteményből. A következő kódrészletet felvétele a PowerShell-parancsprogramot ablaktábla <strong>után</strong> a kódrészletet # 1. Felejtse el beírni a választható DocumentDB.query paraméter t kimetsz csak _ts a dokumentumok és _rid.</p>

    > [AZURE.NOTE]**Elnevezési DocumentDB.inputCollections nem volt hibát.** Igen, akkor engedélyezése több gyűjtemények hozzáadása egy bemeneteként: </br>

        '*DocumentDB.inputCollections*' = '*\<DocumentDB Input Collection Name 1\>*,*\<DocumentDB Input Collection Name 2\>*' A1A</br> The collection names are separated without spaces, using only a single comma.


        # Create a Hive table using data from DocumentDB. Pass DocumentDB the query to filter transferred data to _rid and _ts.
        $queryStringPart1 = "drop table DocumentDB_timestamps; "  +
                            "create external table DocumentDB_timestamps(id string, ts BIGINT) "  +
                            "stored by 'com.microsoft.azure.documentdb.hive.DocumentDBStorageHandler' "  +
                            "tblproperties ( " +
                                "'DocumentDB.endpoint' = '<DocumentDB Endpoint>', " +
                                "'DocumentDB.key' = '<DocumentDB Primary Key>', " +
                                "'DocumentDB.db' = '<DocumentDB Database Name>', " +
                                "'DocumentDB.inputCollections' = '<DocumentDB Input Collection Name>', " +
                                "'DocumentDB.query' = 'SELECT r._rid AS id, r._ts AS ts FROM root r' ); "

3.  Ezután a kimeneti vonatkozóan struktúra táblázat létrehozása. A kimenet dokumentumtulajdonságok lesz, a hónap, nap, órát, percet és előfordulásainak száma.

    > [AZURE.NOTE]**Aztán ismét DocumentDB.outputCollections elnevezési nem volt hibát.** Igen, hogy engedélyezése hozzáadása több gyűjtemények olyan eredménye: </br>
"*DocumentDB.outputCollections*"="*\<DocumentDB kimeneti webhelycsoport neve 1\>*,*\<DocumentDB kimeneti webhelycsoport neve 2\>*" </br> A webhelycsoport nevét egymástól vesszővel csak egyetlen szóközök nélkül. </br></br>
Dokumentumok lesz elosztott ciklikus több gyűjtemények keresztül. Dokumentumok tétele egy webhelycsoport szeretne tárolni, majd a dokumentumok egy második tételt szeretne tárolni a következő webhelycsoport és stb.

        # Create a Hive table for the output data to DocumentDB.
        $queryStringPart2 = "drop table DocumentDB_analytics; " +
                              "create external table DocumentDB_analytics(Month INT, Day INT, Hour INT, Minute INT, Total INT) " +
                              "stored by 'com.microsoft.azure.documentdb.hive.DocumentDBStorageHandler' " +
                              "tblproperties ( " +
                                  "'DocumentDB.endpoint' = '<DocumentDB Endpoint>', " +
                                  "'DocumentDB.key' = '<DocumentDB Primary Key>', " +  
                                  "'DocumentDB.db' = '<DocumentDB Database Name>', " +
                                  "'DocumentDB.outputCollections' = '<DocumentDB Output Collection Name>' ); "

4. Végül vegyük megegyeznek a dokumentumok hónap, nap, óra és perc, és az eredmények vissza beillesztése a kimenet struktúratáblával.

        # GROUP BY minute, COUNT entries for each, INSERT INTO output Hive table.
        $queryStringPart3 = "INSERT INTO table DocumentDB_analytics " +
                              "SELECT month(from_unixtime(ts)) as Month, day(from_unixtime(ts)) as Day, " +
                              "hour(from_unixtime(ts)) as Hour, minute(from_unixtime(ts)) as Minute, " +
                              "COUNT(*) AS Total " +
                              "FROM DocumentDB_timestamps " +
                              "GROUP BY month(from_unixtime(ts)), day(from_unixtime(ts)), " +
                              "hour(from_unixtime(ts)) , minute(from_unixtime(ts)); "

5. Adja hozzá a következő parancsfájl kódtöredékének hozhat létre egy feladat struktúra definíciója az előző lekérdezésből.

        # Create a Hive job definition.
        $queryString = $queryStringPart1 + $queryStringPart2 + $queryStringPart3
        $hiveJobDefinition = New-AzureHDInsightHiveJobDefinition -Query $queryString

    Is használhatja az - fájl kapcsoló a Fájlrendszerhez HiveQL parancsfájl.

6. Adja meg a kezdő időpont és a struktúra feladat elküldése az alábbi kódtöredékének.

        # Save the start time and submit the job to the cluster.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $hiveJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $hiveJobDefinition

7. Adja hozzá az alábbi módon megvárja, amíg a struktúra feladat elvégzéséhez.

        # Wait for the Hive job to complete.
        Wait-AzureHDInsightJob -Job $hiveJob -WaitTimeoutInSeconds 3600

8. Adja hozzá az alábbiak szerint a szabványos kimeneti és a kezdő és záró időpont nyomtatása.

        # Print the standard error, the standard output of the Hive job, and the start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $hiveJob.JobId -StandardOutput
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green

9. **Futtatásához** használt új! **Kattintson** a zöld végrehajtás gombra.

10. Ellenőrizze az eredményt. Jelentkezzen be az [Azure portál][azure-portal].
    1. A bal oldali panelen kattintson a <strong>Tallózás gombra</strong> . </br>
    2. Kattintson a <strong>minden</strong> tetején a Tallózás panel a jobb oldali. </br>
    3. Keresse meg és <strong>DocumentDB fiókok</strong>parancsára. </br>
    4. Ezután find <strong>DocumentDB fiók</strong>, majd a <strong>DocumentDB adatbázis</strong> és a <strong>Webhelycsoport DocumentDB</strong> társított a kimeneti gyűjtemény a struktúra lekérdezés megadott.</br>
    5. Végül kattintson a <strong>Dokumentum Explorer</strong> <strong>Fejlesztőeszközök</strong>alatt.</br></p>

    Ekkor megjelenik a struktúra lekérdezés eredményét.

    ![Lekérdezés eredményének struktúra][image-hive-query-results]

## <a name="RunPig"></a>Lépés: 4: DocumentDB és HDInsight malac feladat futtatása

> [AZURE.IMPORTANT] Összes változó < > jelölt meg kell adni a megadott beállításokat használja.

1. A következő változók beállítása a PowerShell-parancsprogramot ablaktáblán.

        # Provide Azure subscription name.
        $subscriptionName = "Azure Subscription Name"

        # Provide HDInsight cluster name where you want to run the Pig job.
        $clusterName = "Azure HDInsight Cluster Name"

2. <p>Lássunk hozzá a lekérdezési karakterlánc megépítése. Fogja írja be azt, hogy mi minden dokumentum rendszer által létrehozott időbélyegeket (_ts) malac lekérdezés, és egyedi azonosítói (_rid) a DocumentDB gyűjteményből minden dokumentum összeadja perc, majd tárolja az eredmények vissza egy új DocumentDB gyűjteménybe.</p>
    <p>Első lépésként betöltése HDInsight DocumentDB a dokumentumokat. A következő kódrészletet felvétele a PowerShell-parancsprogramot ablaktábla <strong>után</strong> a kódrészletet # 1. Ügyeljen arra, hogy a választható DocumentDB lekérdezési paraméter levághatja a dokumentumokat csak _ts DocumentDB lekérdezés hozzáadása és _rid.</p>

    > [AZURE.NOTE]Igen, akkor engedélyezése több gyűjtemények hozzáadása egy bemeneteként: </br>
"*\<DocumentDB beviteli gyűjtemény neve 1\>*,*\<DocumentDB beviteli webhelycsoport neve 2\>*"</br> A webhelycsoport nevét egymástól vesszővel csak egyetlen szóközök nélkül. </b>

    Dokumentumok lesz elosztott ciklikus több gyűjtemények keresztül. Dokumentumok tétele egy webhelycsoport szeretne tárolni, majd a dokumentumok egy második tételt szeretne tárolni a következő webhelycsoport és stb.

        # Load data from DocumentDB. Pass DocumentDB query to filter transferred data to _rid and _ts.
        $queryStringPart1 = "DocumentDB_timestamps = LOAD '<DocumentDB Endpoint>' USING com.microsoft.azure.documentdb.pig.DocumentDBLoader( " +
                                                        "'<DocumentDB Primary Key>', " +
                                                        "'<DocumentDB Database Name>', " +
                                                        "'<DocumentDB Input Collection Name>', " +
                                                        "'SELECT r._rid AS id, r._ts AS ts FROM root r' ); "

3.  Ezután vegyük megegyeznek a dokumentumokat a hónap, nap, órát, percet és előfordulásainak száma.

        # GROUP BY minute and COUNT entries for each.
        $queryStringPart2 = "timestamp_record = FOREACH DocumentDB_timestamps GENERATE `$0#'id' as id:int, ToDate((long)(`$0#'ts') * 1000) as timestamp:datetime; " +
                            "by_minute = GROUP timestamp_record BY (GetYear(timestamp), GetMonth(timestamp), GetDay(timestamp), GetHour(timestamp), GetMinute(timestamp)); " +
                            "by_minute_count = FOREACH by_minute GENERATE FLATTEN(group) as (Year:int, Month:int, Day:int, Hour:int, Minute:int), COUNT(timestamp_record) as Total:int; "

4. Végezetül tegyük a eredményeinek tárolása az új kimeneti gyűjteménybe.

    > [AZURE.NOTE]Igen, hogy engedélyezése hozzáadása több gyűjtemények olyan eredménye: </br>
"\<DocumentDB kimeneti webhelycsoport neve 1\>,\<DocumentDB kimeneti webhelycsoport neve 2\>"</br> A webhelycsoport nevét egymástól vesszővel csak egyetlen szóközök nélkül.</br>
Dokumentumok lesz elosztott ciklikus több gyűjtemények keresztül. Dokumentumok tétele egy webhelycsoport szeretne tárolni, majd a dokumentumok egy második tételt szeretne tárolni a következő webhelycsoport és stb.

        # Store output data to DocumentDB.
        $queryStringPart3 = "STORE by_minute_count INTO '<DocumentDB Endpoint>' " +
                            "USING com.microsoft.azure.documentdb.pig.DocumentDBStorage( " +
                                "'<DocumentDB Primary Key>', " +
                                "'<DocumentDB Database Name>', " +
                                "'<DocumentDB Output Collection Name>'); "

5. Adja hozzá a következő parancsfájl kódtöredékének hozhat létre egy malac feladat definíciója az előző lekérdezésből.

        # Create a Pig job definition.
        $queryString = $queryStringPart1 + $queryStringPart2 + $queryStringPart3
        $pigJobDefinition = New-AzureHDInsightPigJobDefinition -Query $queryString -StatusFolder $statusFolder

    Is használhatja az - fájl kapcsoló a Fájlrendszerhez malac parancsfájl.

6. Adja meg a kezdő időpont és a malac feladat elküldése az alábbi kódtöredékének.

        # Save the start time and submit the job to the cluster.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $pigJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $pigJobDefinition

7. Adja hozzá az alábbi módon megvárja, amíg a malac feladat elvégzéséhez.

        # Wait for the Pig job to complete.
        Wait-AzureHDInsightJob -Job $pigJob -WaitTimeoutInSeconds 3600

8. Adja hozzá az alábbiak szerint a szabványos kimeneti és a kezdő és záró időpont nyomtatása.

        # Print the standard error, the standard output of the Hive job, and the start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $pigJob.JobId -StandardOutput
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green

9. **Futtatásához** használt új! **Kattintson** a zöld végrehajtás gombra.

10. Ellenőrizze az eredményt. Jelentkezzen be az [Azure portál][azure-portal].
    1. A bal oldali panelen kattintson a <strong>Tallózás gombra</strong> . </br>
    2. Kattintson a <strong>minden</strong> tetején a Tallózás panel a jobb oldali. </br>
    3. Keresse meg és <strong>DocumentDB fiókok</strong>parancsára. </br>
    4. Ezután find <strong>DocumentDB fiók</strong>, majd a <strong>DocumentDB adatbázis</strong> és a <strong>Webhelycsoport DocumentDB</strong> társított a kimeneti gyűjtemény a malac lekérdezés megadott.</br>
    5. Végül kattintson a <strong>Dokumentum Explorer</strong> <strong>Fejlesztőeszközök</strong>alatt.</br></p>

    Ekkor megjelenik a malac lekérdezés eredményét.

    ![Malac lekérdezés eredménye][image-pig-query-results]

## <a name="RunMapReduce"></a>Lépés 5: DocumentDB és HDInsight MapReduce feladat futtatása

1. A következő változók beállítása a PowerShell-parancsprogramot ablaktáblán.

        $subscriptionName = "<SubscriptionName>"   # Azure subscription name
        $clusterName = "<ClusterName>"             # HDInsight cluster name

2. Azt fogja végrehajtása megszámolja az egyes dokumentumtulajdonság a DocumentDB gyűjteményből előfordulásainak MapReduce feladatot. Adja hozzá a parancsprogram kódtöredékének **után** a fenti kódtöredékének.

        # Define the MapReduce job.
        $TallyPropertiesJobDefinition = New-AzureHDInsightMapReduceJobDefinition -JarFile "wasb:///example/jars/TallyProperties-v01.jar" -ClassName "TallyProperties" -Arguments "<DocumentDB Endpoint>","<DocumentDB Primary Key>", "<DocumentDB Database Name>","<DocumentDB Input Collection Name>","<DocumentDB Output Collection Name>","<[Optional] DocumentDB Query>"

    > [AZURE.NOTE] Az egyéni telepítési az DocumentDB Hadoop összekötő TallyProperties-v01.jar megtalálható.

3. Adja hozzá a következő parancsot a MapReduce feladat elküldése.

        # Save the start time and submit the job.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $TallyPropertiesJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $TallyPropertiesJobDefinition | Wait-AzureHDInsightJob -WaitTimeoutInSeconds 3600  

    A MapReduce feladat definíciója mellett is megadnia a HDInsight fürt nevére, amelyhez a MapReduce feladatot, és a hitelesítő adatok futtatásához. A kezdő AzureHDInsightJob aszinkron módjára való átkapcsolás kezdeményezése. Ellenőrizze, hogy a feladat befejezésekor, használja a *Várakozás-AzureHDInsightJob* parancsmag.

4. Adja hozzá a következő parancsot, jelölje be a MapReduce feladat futtatásával kapcsolatos hibákat.

        # Get the job output and print the start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $TallyPropertiesJob.JobId -StandardError
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green

5. **Futtatásához** használt új! **Kattintson** a zöld végrehajtás gombra.

6. Ellenőrizze az eredményt. Jelentkezzen be az [Azure portál][azure-portal].
    1. A bal oldali panelen kattintson a <strong>Tallózás gombra</strong> .
    2. Kattintson a <strong>minden</strong> tetején a Tallózás panel a jobb oldali.
    3. Keresse meg és <strong>DocumentDB fiókok</strong>parancsára.
    4. Ezt követően keresse a <strong>DocumentDB fiók</strong>, majd a kimenet gyűjtemény a MapReduce feladatok megadott társított <strong>DocumentDB adatbázis</strong> és a <strong>Webhelycsoport DocumentDB</strong> .
    5. Végül kattintson a <strong>Dokumentum Explorer</strong> <strong>Fejlesztőeszközök</strong>alatt.

    Ekkor megjelenik a MapReduce feladatok eredményét.

    ![MapReduce lekérdezés eredménye][image-mapreduce-query-results]

## <a name="NextSteps"></a>Következő lépések

Gratulálok! Csak az első struktúra, Malac és MapReduce feladatok az Azure DocumentDB és HDInsight futtatta.

Van még nyitva a Hadoop-összekötő kifejezéskészletébe. Ha érdekli, akkor a [GitHub]hozzájárulhat[documentdb-github].

További tudnivalókért lásd: az alábbi cikkekben:

- [Java-alkalmazások Documentdb kidolgozása][documentdb-java-application]
- [A HDInsight Hadoop fejleszthet olyan Java MapReduce programok][hdinsight-develop-deploy-java-mapreduce]
- [Elkezdeni használni Hadoop HDInsight struktúra mobil kézibeszélőt használati elemzéséhez][hdinsight-get-started]
- [HDInsight MapReduce használata][hdinsight-use-mapreduce]
- [HDInsight struktúra használata][hdinsight-use-hive]
- [Malac használata hdinsight szolgáltatáshoz][hdinsight-use-pig]
- [Parancsfájl művelettel HDInsight fürt testreszabása][hdinsight-hadoop-customize-cluster]

[apache-hadoop]: http://hadoop.apache.org/
[apache-hadoop-doc]: http://hadoop.apache.org/docs/current/
[apache-hive]: http://hive.apache.org/
[apache-pig]: http://pig.apache.org/
[getting-started]: documentdb-get-started.md

[azure-portal]: https://portal.azure.com/
[azure-powershell-diagram]: ./media/documentdb-run-hadoop-with-hdinsight/azurepowershell-diagram-med.png

[documentdb-hdinsight-samples]: http://portalcontent.blob.core.windows.net/samples/documentdb-hdinsight-samples.zip
[documentdb-github]: https://github.com/Azure/azure-documentdb-hadoop
[documentdb-java-application]: documentdb-java-application.md
[documentdb-manage-collections]: documentdb-manage.md#Collections
[documentdb-manage-document-storage]: documentdb-manage.md#IndexOverhead
[documentdb-manage-throughput]: documentdb-manage.md#ProvThroughput
[documentdb-import-data]: documentdb-import-data.md

[hdinsight-custom-provision]: ../hdinsight/hdinsight-provision-clusters.md#powershell
[hdinsight-develop-deploy-java-mapreduce]: ../hdinsight/hdinsight-develop-deploy-java-mapreduce-linux.md
[hdinsight-hadoop-customize-cluster]: ../hdinsight/hdinsight-hadoop-customize-cluster.md
[hdinsight-get-started]: ../hdinsight/hdinsight-hadoop-tutorial-get-started-windows.md
[hdinsight-storage]: ../hdinsight/hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-hive]: ../hdinsight/hdinsight-use-hive.md
[hdinsight-use-mapreduce]: ../hdinsight/hdinsight-use-mapreduce.md
[hdinsight-use-pig]: ../hdinsight/hdinsight-use-pig.md

[image-customprovision-page1]: ./media/documentdb-run-hadoop-with-hdinsight/customprovision-page1.png
[image-hive-query-results]: ./media/documentdb-run-hadoop-with-hdinsight/hivequeryresults.PNG
[image-mapreduce-query-results]: ./media/documentdb-run-hadoop-with-hdinsight/mapreducequeryresults.PNG
[image-pig-query-results]: ./media/documentdb-run-hadoop-with-hdinsight/pigqueryresults.PNG

[powershell-install-configure]: ../powershell-install-configure.md
