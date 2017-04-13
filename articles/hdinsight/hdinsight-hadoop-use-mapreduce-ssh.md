<properties
   pageTitle="A HDInsight Hadoop MapReduce és SSH kapcsolattal |} Microsoft Azure"
   description="Megtudhatja, hogy miként SSH Hadoop használata HDInsight MapReduce feladatok futtatásához használt."
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
   ms.date="08/23/2016"
   ms.author="larryfr"/>

# <a name="use-mapreduce-with-hadoop-on-hdinsight-with-ssh"></a>A HDInsight SSH a Hadoop MapReduce használata

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

Ebben a cikkben megtanulhatja biztonságos rendszerhéj (SSH) használata a hadoop HDInsight fürt csatlakozhat, és küldje MapReduce feladatok Hadoop-parancsaival.

> [AZURE.NOTE] Ha már jól ismert Linux-alapú Hadoop-kiszolgálót használ, de nem HDInsight, olvassa el a [Linux-alapú HDInsight tippek](hdinsight-hadoop-linux-information.md)című témakört.

##<a id="prereq"></a>Előfeltételek

A jelen cikkben ismertetett lépések elvégzéséhez a következőkre lesz szüksége:

* A HDInsight Linux-alapú (Hadoop a HDInsight) fürtre

* Egy SSH ügyfél. Linux rendszerhez, a Unix és a Mac operációs rendszer érkezik egy SSH ügyfélprogrammal. Windows felhasználók le kell töltenie a ügyfélprogram, például [gitt](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

##<a id="ssh"></a>Csatlakozás a SSH segítségével

Csatlakozás a teljes tartománynevét (FQDN) a HDInsight fürt a SSH parancs használatával. A teljesen minősített tartománynév lesz a fürt követ megadott névnek **. azurehdinsight.net**. Ha például a következő kíván csatlakozni **myhdinsight**nevű fürt:

    ssh admin@myhdinsight-ssh.azurehdinsight.net

**Ha megadott egy SSH hitelesítési tanúsítvány kulcsot** a HDInsight fürt létrehozásakor, lehet, adja meg a titkos kulcs helyét a ügyfél rendszeren:

    ssh -i ~/mykey.key admin@myhdinsight-ssh.azurehdinsight.net

**Jelszó SSH hitelesítéshez amennyiben** a HDInsight fürt létrehozásakor, meg kell adja meg a jelszót, amikor a rendszer kéri.

HDInsight való SSH használatáról további tudnivalókért lásd: [Használata SSH a Linux-alapú Hadoop a HDInsight Linux rendszerhez, a OS X, és a Unix](hdinsight-hadoop-linux-use-ssh-unix.md).

###<a name="putty-windows-clients"></a>GITT (Windows-ügyfelek)

A Windows nem nyújt a beépített SSH ügyfél. **Gitt**, amely letölthető a [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)használata ajánlott.

GITT használatával kapcsolatos további tudnivalókért lásd: [Használata SSH a Linux-alapú Hadoop meg a Windows HDInsight ](hdinsight-hadoop-linux-use-ssh-windows.md).

##<a id="hadoop"></a>Hadoop-parancsok használata

1. Miután a HDInsight fürthöz kapcsolódik, a következő paranccsal **Hadoop** MapReduce feladatok indításához:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount wasb:///example/data/gutenberg/davinci.txt wasb:///example/data/WordCountOutput

    Ez elindítja az **wordcount** osztály, amely a **hadoop-mapreduce-examples.jar** fájlban lévő. Bemeneteként, akkor használja a **wasbs://example/data/gutenberg/davinci.txt** dokumentumot, és a kimeneti van tárolva **wasbs: / / / Példa/adatok/WordCountOutput**.

    > [AZURE.NOTE] Többet szeretne tudni a MapReduce feladat és a mintaadatokat [A Hadoop a HDInsight használata MapReduce](hdinsight-use-mapreduce.md)témakörben talál.

2. A feladat részletei bocsát, feldolgozásával, és visszaküldi információk az alábbihoz hasonló a feladat befejezése után:

        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623

3. A feladat befejezése után, a következő parancs segítségével a kimeneti **wasbs://example/data/WordCountOutput**-on tárolt fájlok listája:

        hdfs dfs -ls wasbs:///example/data/WordCountOutput

    Ez megjelenjen-e két fájlokat, a **_SUCCESS** és a **része-r-00000**. A **kijelző-r-00000** fájlt a kimenet ehhez a feladathoz tartalmazza.

    > [AZURE.NOTE] Egyes MapReduce feladatok előfordulhat, hogy felosztása az eredmények több **kijelzőt-r-###** fájlok között Ha igen, használja a ### utótag, hogy a fájlok sorrendjét.

4. A kimenet megtekintéséhez használja az alábbi parancsot:

        hdfs dfs -cat wasbs:///example/data/WordCountOutput/part-r-00000

    Ez a **wasbs://example/data/gutenberg/davinci.txt** fájl, és minden szó történt hányszor szereplő szavak listáját jeleníti meg. Az alábbi képen az adatok az a fájlt tartalmazó:

        wreathed        3
        wreathing       1
        wreaths         1
        wrecked         3
        wrenching       1
        wretched        6
        wriggling       1

##<a id="summary"></a>Összefoglalás

Amint látható, a Hadoop-parancsok MapReduce feladatok futtatása HDInsight fürt, és megtekintheti a feladat kimenetét ugyanígy adja meg.

##<a id="nextsteps"></a>Következő lépések

A HDInsight MapReduce feladatok általános információt:

* [HDInsight Hadoop MapReduce használata](hdinsight-use-mapreduce.md)

További módszerek információt dolgozhat a HDInsight Hadoop:

* [A HDInsight Hadoop struktúra használata](hdinsight-use-hive.md)

* [A HDInsight Hadoop malac használata](hdinsight-use-pig.md)
