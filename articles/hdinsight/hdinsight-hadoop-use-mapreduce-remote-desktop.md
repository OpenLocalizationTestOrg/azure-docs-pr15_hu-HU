<properties
   pageTitle="MapReduce és a HDInsight Hadoop távoli asztali |} Microsoft Azure"
   description="Megtudhatja, hogy miként távoli asztali használatával a HDInsight hadoop és MapReduce feladatok futtatása."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/27/2016"
   ms.author="larryfr"/>

# <a name="use-mapreduce-in-hadoop-on-hdinsight-with-remote-desktop"></a>A távoli asztal HDInsight Hadoop MapReduce használata

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

Ebben a cikkben megtanulhatja a Hadoop HDInsight fürt csatlakozás távoli asztali használatával, és futtassa a Hadoop-parancs használatával MapReduce feladatokat.

##<a id="prereq"></a>Előfeltételek

A jelen cikkben ismertetett lépések elvégzéséhez a következőkre lesz szüksége:

* A Windows-alapú HDInsight (Hadoop a HDInsight) fürtre

* A Windows 10, Windows 8 vagy Windows 7 rendszert futtató ügyfélszámítógép

##<a id="connect"></a>Csatlakozás távoli asztali

A HDInsight fürt engedélyezése a távoli asztali, majd a [Csatlakozás RDP segítségével HDInsight fürt](hdinsight-administer-use-management-portal.md#rdp)utasításait követve csatlakozni.

##<a id="hadoop"></a>A Hadoop paranccsal

Ha rendelkezik az asztali a HDInsight fürt, kövesse az alábbi lépéseket a MapReduce feladat futtatása a Hadoop-parancs használatával:

1. A HDInsight asztalról indítsa el a **Hadoop parancssori**. Ekkor megnyílik egy új parancssort az a **c:\apps\dist\hadoop-&lt;verziószáma >** directory.

    > [AZURE.NOTE] A verziószám Hadoop frissül változik. Keresse meg az elérési utat a **HADOOP_HOME** környezeti változó használható. Ha például `cd %HADOOP_HOME%` könyvtárak anélkül, hogy a verziószám, hogy a Hadoop könyvtár változik.

2. A **Hadoop** paranccsal egy példa MapReduce feladat futtatása, használja az alábbi parancsot:

        hadoop jar hadoop-mapreduce-examples.jar wordcount wasbs:///example/data/gutenberg/davinci.txt wasbs:///example/data/WordCountOutput

    Ez elindítja az **wordcount** osztály, amelyek az aktuális mappában **hadoop-mapreduce-examples.jar** fájlban található. Bemeneteként, akkor használja a **wasbs://example/data/gutenberg/davinci.txt** dokumentumot, és a kimeneti van tárolva: **wasbs: / / / Példa/adatok/WordCountOutput**.

    > [AZURE.NOTE] a MapReduce feladat és a mintaadatok kapcsolatos további tudnivalókért lásd: <a href="hdinsight-use-mapreduce.md">A HDInsight Hadoop használata MapReduce</a>.

2. A feladat részletei bocsát ki, a program dolgozza fel, és visszaküldi információk az alábbihoz hasonló a feladat befejezésekor:

        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623

3. Ha befejezte a feladatot, a következő parancs segítségével listája a kimeneti fájl **wasbs://example/data/WordCountOutput**tárolt:

        hadoop fs -ls wasbs:///example/data/WordCountOutput

    Ez megjelenjen-e két fájlokat, a **_SUCCESS** és a **része-r-00000**. A **része-r-00000** fájlt tartalmazza a kimenet ehhez a feladathoz.

    > [AZURE.NOTE] Egyes MapReduce feladatok előfordulhat, hogy felosztás az eredmények több **kijelzőt-r-###** fájlok között. Ha igen, használja a ### utótag, hogy a fájlok sorrendjét.

4. A kimenet megtekintéséhez használja az alábbi parancsot:

        hadoop fs -cat wasbs:///example/data/WordCountOutput/part-r-00000

    Ez a szavakat, amelyek **wasbs://example/data/gutenberg/davinci.txt** fájlban minden szó történt hányszor együtt listáját jeleníti meg. Az alábbi képen az adatok az a fájlt tartalmazó:

        wreathed        3
        wreathing       1
        wreaths         1
        wrecked         3
        wrenching       1
        wretched        6
        wriggling       1

##<a id="summary"></a>Összefoglalás

Amint látható, a Hadoop parancs egyszerűvé egy HDInsight fürthöz MapReduce feladat futtatható, és tekintse meg a projekt eredménye.

##<a id="nextsteps"></a>Következő lépések

A HDInsight MapReduce feladatok általános információt:

* [HDInsight Hadoop MapReduce használata](hdinsight-use-mapreduce.md)

További módszerek információt dolgozhat a HDInsight Hadoop:

* [A HDInsight Hadoop struktúra használata](hdinsight-use-hive.md)

* [A HDInsight Hadoop malac használata](hdinsight-use-pig.md)
