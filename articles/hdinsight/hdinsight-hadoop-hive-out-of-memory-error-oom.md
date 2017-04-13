<properties
    pageTitle="Kijelentkezés memóriában hiba (memória) - struktúra beállítások |} Microsoft Azure"
    description=""Házon kívül" memória hiba (memória) struktúra lekérdezés javítsa Hadoop a hdinsight szolgáltatásból lehetőségre. A forgatókönyv az a lekérdezés sok nagy táblázatok között."
    keywords="Kijelentkezés a memória hiba, memória, struktúra beállításai"
    services="hdinsight"
    documentationCenter=""
    authors="rashimg"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="09/02/2016"
    ms.author="rashimg;jgao"/>

# <a name="fix-an-out-of-memory-oom-error-with-hive-memory-settings-in-hadoop-in-azure-hdinsight"></a>"Házon kívül" memória (memória) hiba megoldása Hadoop az Azure hdinsight szolgáltatáshoz a struktúra memória beállításokkal

A gyakori problémák közül az ügyfelek arc van "Házon kívül" memória (memória) hibát kapott struktúra használatával. Ez a cikk ismerteti a példa az ügyfél és a struktúra beállításokat, ajánlott a probléma megoldásához.

## <a name="scenario-hive-query-across-large-tables"></a>Alkalmazási helyzetek: Struktúra lekérdezés nagy táblázatok között

Egy ügyfél adódott a lekérdezés használatával struktúra alatt.

    SELECT
        COUNT (T1.COLUMN1) as DisplayColumn1,
        …
        …
        ….
    FROM
        TABLE1 T1,
        TABLE2 T2,
        TABLE3 T3,
        TABLE5 T4,
        TABLE6 T5,
        TABLE7 T6
    where (T1.KEY1 = T2.KEY1….
        …
        …

A lekérdezés néhány részleteiről:

* T1 nagy táblázattá tábla1, amely rengeteg karakterlánc oszlopban található információ típusa alias.
* Más táblákat, amelyek nem nagy, de rendelkező oszlopok nagyszámú.
* Minden olyan tábla csatlakozásáról egymás között, bizonyos esetekben a TABLE1 és mások több oszlopra.

A struktúra használata MapReduce 24 csomóponton A3 fürt lekérdezés futtatásakor a az ügyfelet a lekérdezés futtatásakor körülbelül 26 perc alatt. Az ügyfelet a következő figyelmeztető üzenetek láthatta, futtatásakor a lekérdezés MapReduce struktúra használata:

    Warning: Map Join MAPJOIN[428][bigTable=?] in task 'Stage-21:MAPRED' is a cross product
    Warning: Shuffle Join JOIN[8][tables = [t1933775, t1932766]] in Stage 'Stage-4:MAPRED' is a cross product

A lekérdezés végzett körülbelül 26 perces menjen végbe, mert az ügyfél figyelmen kívül hagyja az alábbi figyelmeztetéseket, és hogyan javítható a kiemelése inkább lépések további lekérdezés teljesítményét.

Az ügyfél egyeztetni [a HDInsight Hadoop lekérdezések optimalizálása struktúra](hdinsight-hadoop-optimize-hive-query.md), és úgy döntött, hogy Tez végrehajtási motor használja. Miután ugyanazon lekérdezés futtatásának a Tez beállítás engedélyezése a lekérdezés-15 percet, és a majd okozott a következő hiba:

    Status: Failed
    Vertex failed, vertexName=Map 5, vertexId=vertex_1443634917922_0008_1_05, diagnostics=[Task failed, taskId=task_1443634917922_0008_1_05_000006, diagnostics=[TaskAttempt 0 failed, info=[Error: Failure while running task:java.lang.RuntimeException: java.lang.OutOfMemoryError: Java heap space
        at
    org.apache.hadoop.hive.ql.exec.tez.TezProcessor.initializeAndRunProcessor(TezProcessor.java:172)
        at org.apache.hadoop.hive.ql.exec.tez.TezProcessor.run(TezProcessor.java:138)
        at
    org.apache.tez.runtime.LogicalIOProcessorRuntimeTask.run(LogicalIOProcessorRuntimeTask.java:324)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable$1.run(TezTaskRunner.java:176)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable$1.run(TezTaskRunner.java:168)
        at java.security.AccessController.doPrivileged(Native Method)
        at javax.security.auth.Subject.doAs(Subject.java:415)
        at org.apache.hadoop.security.UserGroupInformation.doAs(UserGroupInformation.java:1628)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable.call(TezTaskRunner.java:168)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable.call(TezTaskRunner.java:163)
        at java.util.concurrent.FutureTask.run(FutureTask.java:262)
        at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
        at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
        at java.lang.Thread.run(Thread.java:745)
    Caused by: java.lang.OutOfMemoryError: Java heap space

Az ügyfél majd úgy döntött, használja a nagyobb virtuális (azaz D12) egy nagyobb virtuális gondolok szeretné, hogy több halommemória helyet. Majd még akkor is az ügyfél továbbra is hibaüzenet jelenik meg. Az ügyfelet a HDInsight csapatnak segíteni a probléma hibakeresési elérése után.

## <a name="debug-the-out-of-memory-oom-error"></a>A "Házon kívül" memória (memória) hiba hibakeresése

A támogatási és mérnöki csapatok közös található a "Házon kívül" memória (memória) hibát okozó problémák volt [ismert problémák a Apache JIRA leírtak](https://issues.apache.org/jira/browse/HIVE-8306). A leírás a JIRA:

    When hive.auto.convert.join.noconditionaltask = true we check noconditionaltask.size and if the sum  of tables sizes in the map join is less than noconditionaltask.size the plan would generate a Map join, the issue with this is that the calculation doesnt take into account the overhead introduced by different HashTable implementation as results if the sum of input sizes is smaller than the noconditionaltask size by a small margin queries will hit OOM.

Azt arról, hogy **hive.auto.convert.join.noconditionaltask** valóban beállítása óta **true** megjeleníti a struktúra-site.xml fájl:

    <property>
        <name>hive.auto.convert.join.noconditionaltask</name>
        <value>true</value>
        <description>
            Whether Hive enables the optimization about converting common join into mapjoin based on the input file size.
            If this parameter is on, and the sum of size for n-1 of the tables/partitions for a n-way join is smaller than the
            specified size, the join is directly converted to a mapjoin (there is no conditional task).
        </description>
    </property>

A figyelmeztetés és a JIRA alapján, a hipotézis volt, térkép Bekapcsolódás volt a Java halommemória terület memória hiba oka. Így azt ásott mélyebb be ezt a problémát.

A blogbejegyzés [Hadoop fonal memória-beállításokat a HDInsight](http://blogs.msdn.com/b/shanyu/archive/2014/07/31/hadoop-yarn-memory-settings-in-hdinsigh.aspx)leírtak használt halommemória terület használatakor az adatvégrehajtás motor Tez ténylegesen tartozik a Tez tároló. Lásd: a kép alatt a Tez tároló memória leíró.

![Tez tároló memória diagram: ki a memória hiba memória struktúra](./media/hdinsight-hadoop-hive-out-of-memory-error-oom/hive-out-of-memory-error-oom-tez-container-memory.png)


A blogbejegyzés javaslatokat tesz, mint az alábbi két memória-beállítások megadása a halom a tároló memória: **hive.tez.container.size** és **hive.tez.java.opts**. A saját változat a memória kivételt nem jelent a tároló mérete túl kicsi. Ez azt jelenti, hogy a Java felhalmozódott (hive.tez.java.opts) túl kicsi. Így memória jelenik meg, valahányszor megpróbálhatja **hive.tez.java.opts**növeléséhez. Ha szükséges lehet, hogy **hive.tez.container.size**növeléséhez. A **java.opts** beállítás körülbelül **container.size**80 %-át kell lennie.

> [AZURE.NOTE]  A beállítás **hive.tez.java.opts** mindig **hive.tez.container.size**kisebbnek kell lennie.

Mivel az egy D12 számítógépre 28GB memóriát, azt a tároló méretét 10GB (10240MB) és 80 %-os hozzárendelése java.opts úgy döntött. Ez a struktúra konzolban, az alábbi beállításokkal végzett:

    SET hive.tez.container.size=10240
    SET hive.tez.java.opts=-Xmx8192m

Ezeket a beállításokat alapján, a lekérdezés sikeresen futtatásakor a tíz perccel.

## <a name="conclusion-oom-errors-and-container-size"></a>Elfogadásáról: Memória hibák és tároló mérete

Memória hiba nem feltétlenül jelenti tároló mérete túl kicsi. Ehelyett konfigurálnia kell a memória beállításokat, hogy a felhalmozódott növekszik, és a tároló memóriaméret legalább 80 %-át.
