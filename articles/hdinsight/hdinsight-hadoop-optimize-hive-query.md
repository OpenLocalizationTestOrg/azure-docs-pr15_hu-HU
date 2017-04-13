<properties
   pageTitle="A struktúra lekérdezések a HDInsight a gyorsabb végrehajtás optimalizálása |} Microsoft Azure"
   description="Megtudhatja, hogy miként optimalizálás a struktúra lekérdezések Hadoop a hdinsight szolgáltatásból lehetőségre."
   services="hdinsight"
   documentationCenter=""
   authors="rashimg"
   manager="mwinkle"
   editor="cgronlun"
   tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="07/28/2015"
   ms.author="rashimg"/>


# <a name="optimize-hive-queries-for-hadoop-in-hdinsight"></a>A HDInsight Hadoop-optimalizálhatja a struktúra lekérdezések

Alapértelmezés szerint Hadoop fürt nincsenek teljesítmény optimalizálva. Ez a cikk ismerteti azokat a leggyakoribb struktúra teljesítmény optimalizálási módszerek a lekérdezések alkalmazható néhány.

##<a name="scale-out-worker-nodes"></a>Dolgozó csomópontok méretezése

Fürt dolgozó csomópontok számának növelése is kihasználhatja további mappers és szűkítő párhuzamosan futtatásához. Kétféleképpen növelheti a skála HDInsight meg:

- A rendelkezés időben megadhatja az Azure-portálra, Azure PowerShell alrendszerrel vagy platformok parancssor dolgozó csomópontok számának.  További tudnivalókért olvassa el a [rendelkezést HDInsight fürt](hdinsight-provision-clusters.md)című témakört. Az alábbi képernyőn látható a dolgozó csomópont konfigurálása az Azure-portálon:

    ![scaleout_1][image-hdi-optimize-hive-scaleout_1]

- A futásidőben is méretezheti ki fürt újbóli létrehozása egy nélkül. Az alábbiakban látható.
![scaleout_1][image-hdi-optimize-hive-scaleout_2]

A különböző virtuális gépeken HDInsight által támogatott, lásd: [árak hdinsight szolgáltatásból lehetőségre](https://azure.microsoft.com/pricing/details/hdinsight/).

##<a name="enable-tez"></a>Tez engedélyezése

[Apache Tez](http://hortonworks.com/hadoop/tez/) egy alternatív végrehajtás motor a MapReduce motor:

![tez_1][image-hdi-optimize-hive-tez_1]


Tez gyorsabb azért, mert:

- Irányított aciklikus Graph (DAG) végrehajtása a MapReduce motor egyetlen feladatként, a DAG fejezi ki, hogy igényel minden mappers szűkítő az egyik készlete kell követnie. Ennek hatására ki az egyes struktúra lekérdezés percenként több MapReduce feladatokat. Tez nincs ilyen korlátozás, és dolgozhat a összetett DAG, így a feladat indítási terhelést minimalizálása egy feladatot.
- **A szükségtelen Avoids ír** Több feladat a MapReduce Engine ugyanazt a struktúra lekérdezést az éppen is, hogy minden feladat kimenetét írása Fájlrendszerhez köztes adatokhoz. Mivel a Tez kis méretűre állítása mindegyik struktúra lekérdezéshez feladatok száma található felesleges írási fejhallgatót.
- **Minimizes indítási késések** Tez jobban el tudja minimalizálhatja az indítási késleltetés indításához szükséges mappers számának csökkentése és is javítása az egész optimalizálás.
- **Tárolók használja.** Jelenik meg, ha lehetséges Tez el tudja újrafelhasználása tárolók annak érdekében, hogy késés miatt tárolók indítását csökken.
- **Folytonos optimalizálási technikákkal** Hagyományos optimalizálási végezték fordítási fázisban. Azonban további információt a bemenetben érhető el, amely lehetővé teszi jobb optimalizálási futtatókörnyezet során. Tez folyamatos optimalizálási technikákat alkalmaz, amely lehetővé teszi, hogy a terv további futtatókörnyezet fázisba történő optimalizálásához használja.

E fogalmak részletekért kattintson [ide](http://hortonworks.com/hadoop/tez/)

Bármilyen struktúra lekérdezést az alábbi beállítással a lekérdezés illesztésével engedélyezett Tez is folytathat:

    set hive.execution.engine=tez;

A Windows-alapú HDInsight fürt Tez engedélyezve kell lennie rendelkezést időben. Az alábbiakban a Hadoop fürt kiépítéséhez Tez engedélyezve van az Azure PowerShell mintaparancsfájl:

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    $clusterName = "[HDInsightClusterName]"
    $location = "[AzureDataCenter]" #i.e. West US
    $dataNodes = 32 # number of worker nodes in the cluster

    $defaultStorageAccountName = "[DefaultStorageAccountName]"
    $defaultStorageContainerName = "[DefaultBlobContainerName]"
    $defaultStorageAccountKey = $defaultStorageAccountKey = Get-AzureStorageKey $defaultStorageAccountName.ToLower() | %{ $_.Primary }

    $hdiUserName = "[HTTPUserName]"
    $hdiPassword = "[HTTPUserPassword]"

    $hdiSecurePassword = ConvertTo-SecureString $hdiPassword -AsPlainText -Force
    $hdiCredential = New-Object System.Management.Automation.PSCredential($hdiUserName, $hdiSecurePassword)

    $hiveConfig = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightHiveConfiguration'
    $hiveConfig.Configuration = @{ "hive.execution.engine"="tez" }

    New-AzureHDInsightClusterConfig -ClusterSizeInNodes $dataNodes -HeadNodeVMSize Standard_D14 -DataNodeVMSize Standard_D14 |
    Set-AzureHDInsightDefaultStorage -StorageAccountName "$defaultStorageAccountName.blob.core.windows.net" -StorageAccountKey $defaultStorageAccountKey -StorageContainerName $defaultStorageContainerName |
    Add-AzureHDInsightConfigValues -Hive $hiveConfig |
    New-AzureHDInsightCluster -Name $clusterName -Location $location -Credential $hdiCredential

    
> [AZURE.NOTE] Linux-alapú HDInsight fürt Tez alapértelmezés szerint engedélyezve van.
    

## <a name="hive-partitioning"></a>Szétválasztás struktúra

I/O művelet struktúra lekérdezések futtatásához jelentős teljesítménybeli szűk. A teljesítmény javítható, ha csökkenthető a hogy mennyi adatot kell olvasni. Alapértelmezés szerint struktúra lekérdezések tekintse át a teljes struktúra táblákat. Ez a lekérdezések nagyszerű, mint a tábla megvizsgálja, azonban csak kell egy kis mennyiségű adat (például a szűréssel lekérdezések) beolvasása lekérdezések, ezzel kapcsolatot hozott létre a szükségtelen terhelést. Struktúra szétválasztás lehetővé teszi, hogy a struktúra lekérdezések csak szükséges mennyiségét struktúra táblában található adatok eléréséhez.

Struktúra szétválasztás valósítható átrendezése a a nyers adatokból új könyvtárak be az egyes partíciók saját címtár - hol partíciót a felhasználó által definiált problémákat. A következő ábra bemutatja, hogy egy struktúratáblával szétválasztás az *év*oszlop szerint. Egy új könyvtár az egyes jön létre.

![szétválasztás][image-hdi-optimize-hive-partitioning_1]

Néhány particionálási szempontok:

- **Ne nem hiány partíciót** - csak néhány értékeket tartalmazó oszlop szétválasztás nagyon kevés partíciót okozhatja. Például a gender szétválasztás fog csak hozzon létre két partíciók (Férfi és női) hozható létre, így csak csökkentése a késleltetés legfeljebb fele.

- **Nem túlságosan partition** - a rendkívüli, a partíciót egy egyedi érték (például felhasználóazonosító) oszlop létrehozásához okoz több partíciót terhelési sok okoz a fürt namenode a módon kezelje a könyvtárak nagy mennyiségű lesz.

- **Ne adatok ferdeség** – a particionáló használatával bölcs kiválasztása, hogy az összes partíciót még méretét. Példa a rendszer a szétválasztás *állam* okozhat a kaliforniai szinte 30 kell a rekordok számát x, Vermont sokaság eltérése miatt.

Partition táblázatot létrehozni, használja a *Particionálva By* záradékban:

    CREATE TABLE lineitem_part
        (L_ORDERKEY INT, L_PARTKEY INT, L_SUPPKEY INT,L_LINENUMBER INT,
         L_QUANTITY DOUBLE, L_EXTENDEDPRICE DOUBLE, L_DISCOUNT DOUBLE,
         L_TAX DOUBLE, L_RETURNFLAG STRING, L_LINESTATUS STRING,
         L_SHIPDATE_PS STRING, L_COMMITDATE STRING, L_RECEIPTDATE        STRING, L_SHIPINSTRUCT STRING, L_SHIPMODE STRING,
         L_COMMENT STRING)
    PARTITIONED BY(L_SHIPDATE STRING)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    STORED AS TEXTFILE;

Miután a particionált táblázatot hoz létre, vagy létrehozhat statikus szétválasztás vagy dinamikus szétválasztás.

- **Statikus szétválasztás** azt jelenti, hogy a megfelelő könyvtárban van már sharded adatok, és megkérheti a struktúra partíciót manuálisan címtár helye alapján. Ez az alábbi kódrészletet látható.

        INSERT OVERWRITE TABLE lineitem_part
        PARTITION (L_SHIPDATE = ‘5/23/1996 12:00:00 AM’)
        SELECT * FROM lineitem 
        WHERE lineitem.L_SHIPDATE = ‘5/23/1996 12:00:00 AM’

        ALTER TABLE lineitem_part ADD PARTITION (L_SHIPDATE = ‘5/23/1996 12:00:00 AM’))
        LOCATION ‘wasbs://sampledata@ignitedemo.blob.core.windows.net/partitions/5_23_1996/'

- **Dinamikus szétválasztás** , az azt jelenti, hogy szeretné-e meg automatikusan létrehozza a partíciók struktúra. Mivel azt már korábban elkészült a particionáló táblázat az átmeneti tárolásra szolgáló értékeket, végezze el szükség az adatokat a particionált táblázat beszúrása a alább látható módon:

        SET hive.exec.dynamic.partition = true;
        SET hive.exec.dynamic.partition.mode = nonstrict;
        INSERT INTO TABLE lineitem_part
        PARTITION (L_SHIPDATE)
        SELECT L_ORDERKEY as L_ORDERKEY, L_PARTKEY as L_PARTKEY , 
             L_SUPPKEY as L_SUPPKEY, L_LINENUMBER as L_LINENUMBER,
             L_QUANTITY as L_QUANTITY, L_EXTENDEDPRICE as L_EXTENDEDPRICE,
             L_DISCOUNT as L_DISCOUNT, L_TAX as L_TAX, L_RETURNFLAG as       L_RETURNFLAG, L_LINESTATUS as L_LINESTATUS, L_SHIPDATE as       L_SHIPDATE_PS, L_COMMITDATE as L_COMMITDATE, L_RECEIPTDATE as   L_RECEIPTDATE, L_SHIPINSTRUCT as L_SHIPINSTRUCT, L_SHIPMODE as      L_SHIPMODE, L_COMMENT as L_COMMENT, L_SHIPDATE as L_SHIPDATE FROM lineitem;

További információra kíváncsi olvassa el a [Particionálva táblázatok](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL#LanguageManualDDL-PartitionedTables)című témakört.

##<a name="use-the-orcfile-format"></a>A ORCFile formátum használata

Struktúra más fájlformátumokban támogatja. Példa:

- **Szöveg**: Ez az alapértelmezett fájlformátum és működik-e a találatokkal
- **Avro**: kompatibilitási forgatókönyvek jól működik
- **ORC/Parquet**: a teljesítmény elérése érdekében a legmegfelelőbb

ORC (optimalizált sor oszlopos) formátuma struktúra adattárolásra erősen hatékonyan. Összehasonlítás más formátumokba, ORC magában a következő előnyökkel jár:

- összetett típusú, többek között a DateTime és a bonyolult és a félig strukturált támogatása
- legfeljebb 70 %-os tömörítés
- indexek minden 10 000 sort, amely lehetővé teszi a sorok kihagyása
- futási idejű végrehajtás jelentős csökkenése

Ahhoz, hogy a ORC formátumban, először hozzon létre egy táblázatot a záradék *tárolt ORC szerint*:

    CREATE TABLE lineitem_orc_part
        (L_ORDERKEY INT, L_PARTKEY INT,L_SUPPKEY INT, L_LINENUMBER INT,
         L_QUANTITY DOUBLE, L_EXTENDEDPRICE DOUBLE, L_DISCOUNT DOUBLE,
         L_TAX DOUBLE, L_RETURNFLAG STRING, L_LINESTATUS STRING,
         L_SHIPDATE_PS STRING, L_COMMITDATE STRING, L_RECEIPTDATE STRING,
         L_SHIPINSTRUCT STRING, L_SHIPMODE STRING, L_COMMENT     STRING)
    PARTITIONED BY(L_SHIPDATE STRING)
    STORED AS ORC;

Ezután szúr be adatokat a ORC táblához az átmeneti tárolásra szolgáló értékeket. Példa:

    INSERT INTO TABLE lineitem_orc
    SELECT L_ORDERKEY as L_ORDERKEY, 
           L_PARTKEY as L_PARTKEY , 
           L_SUPPKEY as L_SUPPKEY,
           L_LINENUMBER as L_LINENUMBER,
           L_QUANTITY as L_QUANTITY, 
           L_EXTENDEDPRICE as L_EXTENDEDPRICE,
           L_DISCOUNT as L_DISCOUNT,
           L_TAX as L_TAX,
           L_RETURNFLAG as L_RETURNFLAG,
           L_LINESTATUS as L_LINESTATUS,
           L_SHIPDATE as L_SHIPDATE,
           L_COMMITDATE as L_COMMITDATE,
           L_RECEIPTDATE as L_RECEIPTDATE, 
           L_SHIPINSTRUCT as L_SHIPINSTRUCT,
           L_SHIPMODE as L_SHIPMODE,
           L_COMMENT as L_COMMENT
    FROM lineitem;

Erről további ORC formátumát [Itt](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC).

##<a name="vectorization"></a>Vectorization

Vectorization lehetővé teszi, hogy egy köteg 1024 sorok helyett közös feldolgozásának egy sor egyszerre feldolgozása struktúra. Ez azt jelenti, hogy egyszerű műveletek végzett gyorsabb, mert kevesebb belső kód futtatásához szükséges rendszerkövetelmények.

Ahhoz, hogy vectorization előtag a struktúra lekérdezést az alábbi beállításokat:

    set hive.vectorized.execution.enabled = true;

További tudnivalókért olvassa el a [Vectorized lekérdezés-végrehajtási](https://cwiki.apache.org/confluence/display/Hive/Vectorized+Query+Execution)című témakört.


##<a name="other-optimization-methods"></a>Más optimalizálási módszerek

Nincsenek további optimalizálási módszerek, amelyet akkor is, például:

- **Struktúra bucketing:** olyan módszer, amely lehetővé teszi, hogy a fürt vagy oszthatja fel nagyméretű adatkészletek optimalizálhatja a lekérdezési teljesítmény.
- **Bekapcsolódás optimalizálás:** struktúrájának lekérdezés-végrehajtási illesztések hatékonyságának javítása és a felhasználó útmutatók kell csökkentése tervezési optimalizálása. További tudnivalókért lásd: a [Bekapcsolódás optimalizálási](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+JoinOptimization#LanguageManualJoinOptimization-JoinOptimization).
- **szűkítő növelése**

##<a id="nextsteps"></a>Következő lépések
Ebben a cikkben struktúra lekérdezési néhány gyakori optimalizálási módszerek megtanulta azt van. További tudnivalókért lásd: az alábbi cikkekben:

- [A HDInsight Apache struktúra](hdinsight-use-hive.md)
- [Nézetbeli késleltetés adatok elemzése HDInsight struktúra használatával](hdinsight-analyze-flight-delay-data.md)
- [Adatelemzés Twitter segítségével HDInsight struktúra](hdinsight-analyze-twitter-data.md)
- [A lekérdezés struktúra konzol használata a HDInsight Hadoop érzékelő adatok elemzése](hdinsight-hive-analyze-sensor-data.md)
- [A webhelyekre naplók elemzéséhez HDInsight struktúra használata](hdinsight-hive-analyze-website-log.md)


[image-hdi-optimize-hive-scaleout_1]: ./media/hdinsight-hadoop-optimize-hive-query/scaleout_1.png
[image-hdi-optimize-hive-scaleout_2]: ./media/hdinsight-hadoop-optimize-hive-query/scaleout_2.png
[image-hdi-optimize-hive-tez_1]: ./media/hdinsight-hadoop-optimize-hive-query/tez_1.png
[image-hdi-optimize-hive-partitioning_1]: ./media/hdinsight-hadoop-optimize-hive-query/partitioning_1.png
