<properties
   pageTitle="Használja a struktúra rendszerhéj HDInsight (Hadoop) |} Microsoft Azure"
   description="Megtudhatja, hogy miként Linux-alapú HDInsight fürt a struktúra rendszerhéj használata. Megtudhatja, hogy miként csatlakozzon a HDInsight fürthöz SSh használatával lesz, majd a struktúra rendszerhéj segítségével interaktív lekérdezések futtatása."
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
   ms.date="10/04/2016"
   ms.author="larryfr"/>

# <a name="use-hive-with-hadoop-in-hdinsight-with-ssh"></a>A HDInsight SSH a Hadoop struktúra használata

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

Ebben a cikkben megtanulhatja, hogyan biztonságos rendszerhéj (SSH) használatával egy hadoop Azure hdinsight szolgáltatáshoz fürt és interaktív küldje struktúra lekérdezések a struktúra parancssori kezelőfelületről használatával.

> [AZURE.IMPORTANT] Noha a struktúra parancs a HDInsight Linux-alapú fürt, érdemes megfontolni Beeline használatával. Beeline egy újabb változatával osztanak a struktúra, és a HDInsight fürt megtalálható. Ez használatáról további információt a [Használatát a Hadoop HDInsight Beeline és a struktúra](hdinsight-hadoop-use-hive-beeline.md)témakörben talál.

##<a id="prereq"></a>Előfeltételek

A jelen cikkben ismertetett lépések elvégzéséhez a következőkre lesz szüksége:

* Egy Linux-alapú Hadoop fürt hdinsight szolgáltatásból lehetőségre.

* Egy SSH ügyfél. Linux rendszerhez, a Unix és a Mac OS érkezik egy SSH ügyfélprogrammal. Windows felhasználók le kell töltenie a ügyfélprogram, például [gitt](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

##<a id="ssh"></a>Csatlakozás a SSH segítségével

Csatlakozás a teljes tartománynevét (FQDN) a HDInsight fürt a SSH parancs használatával. A teljesen minősített tartománynév lesz a fürt, majd megadott névnek **. azurehdinsight.net**. Ha például a következő kíván csatlakozni **myhdinsight**nevű fürt:

    ssh admin@myhdinsight-ssh.azurehdinsight.net

**Ha a megadott SSH hitelesítési tanúsítvány kulcsot** a HDInsight fürt létrehozásakor, előfordulhat, a titkos kulcs helyének megadásához az ügyfél rendszeren:

    ssh -i ~/mykey.key admin@myhdinsight-ssh.azurehdinsight.net

**Jelszó SSH hitelesítéshez amennyiben** a HDInsight fürt létrehozásakor, meg kell adja meg a jelszót, amikor a rendszer kéri.

HDInsight való SSH használatáról további tudnivalókért lásd: [Használata SSH a Linux-alapú Hadoop a HDInsight Linux rendszerhez, a OS X, és a Unix](hdinsight-hadoop-linux-use-ssh-unix.md).

###<a name="putty-windows-based-clients"></a>GITT (Windows-alapú ügyfelek)

A Windows nem nyújt a beépített SSH ügyfél. **Gitt**, amely letölthető a [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)használata ajánlott.

GITT használatával kapcsolatos további tudnivalókért lásd: [Használata SSH a Linux-alapú Hadoop meg a Windows HDInsight ](hdinsight-hadoop-linux-use-ssh-windows.md).

##<a id="hive"></a>A struktúra paranccsal

2. Miután létrejött, először a struktúra CLI a következő parancsot:

        hive

3. A CLI használ, írja be a következő kimutatások **log4jLogs** nevű a mintaadatok használatával új tábla létrehozása:

        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    Ezek a kimutatások hajtsa végre az alábbi műveleteket:

    * **Táblázat LEVÁLASZTÁSA** – a táblázat és az adatfájl törlése abban az esetben, ha a táblázat már létezik.
    * **Külső tábla létrehozása** – új táblát hoz létre külső"a struktúra. Külső táblákat a tábla definícióját csak tárolását struktúra. Az adatok az eredeti helyén marad.
    * **Sor formátum** - struktúra alapján, az adatok formázását. Ebben az esetben a mezőket az egyes naplók vannak elválasztva szóközt.
    * **AS TEXTFILE helyen tárolt** - struktúra alapján, az adatokat tárolja (a példában/adatkönyvtárának), és hogy szövegként tárolt.
    * Egy adott oszlop **t4** **[hiba]**értéket tartalmazza sorok összes számát, **VÁLASSZA a** - kijelöli. Három, ezt az értéket tartalmazó sorok szerint meg kell vissza a **3** értéket.
    * **INPUT__FILE__NAME PÉLDÁUL "%.log"** - struktúra alapján visszaadó azt kell csak adatok végződésű fájlokból. naplót. E beállítás hatására a Keresés a adatokat tartalmazza, amely megőrzi a származó adatokat visszaadó más példából, amelyek nem felelnek meg a definiált séma adatfájlok sample.log fájlt.

    > [AZURE.NOTE] Külső tábla kell használni, amikor a külső forrásból, például egy automatikus feltöltés adatfeldolgozás, vagy egy másik MapReduce művelet, amelyet frissíteni kell, de mindig szeretné használni a legfrissebb adatok struktúra lekérdezések a mögöttes adatok várt.
    >
    > Tartalmaz külső tábla húzással **nem** törlése az adatokat, csak a tábla definícióját.

4. A következő kimutatások használatával **errorLogs**nevű új "belső" tábla létrehozása:

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    Ezek a kimutatások hajtsa végre az alábbi műveleteket:

    * **Hozzon létre táblázat esetén nem létezik** - táblát hoz létre, ha még nem létezik. A **külső** kulcsszó nem használatos, mivel az egy belső tábla, amely a struktúra adatraktár tárolja, és a struktúra teljesen kezeli.
    * **TÁROLVA mint ORC** - optimalizált sor oszlopos (ORC) formátumban adatait tárolja. Ez a struktúra adatok tárolására szolgáló erősen optimalizált és hatékony formátumot.
    * Beszúrás **FELÜLÍRÁSA... VÁLASSZA a** - sorok **[hiba]**tartalmazó **log4jLogs** táblából, majd az adatok beszúrása a **errorLogs** táblázatba.

    Ha ellenőrizni szeretné, hogy csak a **[hiba]** oszlopban t4 tartalmazó sorokat a **errorLogs** táblába tárolta, az alábbi utasítás segítségével az összes sort **errorLogs**eredményül:

        SELECT * from errorLogs;

    Három sornyi adatot vissza kell, az összes tartalmazó **[hiba]** oszlopban t4.

    > [AZURE.NOTE] Külső táblázatok, eltérően egy belső táblázat leválasztása törli a mögöttes adatok.

##<a id="summary"></a>Összefoglalás

Amint látható, a struktúra parancs egyszerűvé interaktív struktúra lekérdezések futtatása egy HDInsight fürt, figyelheti a projekt állapotát, valamint a kimenet.

##<a id="nextsteps"></a>Következő lépések

Ha általános információkra HDInsight struktúra:

* [A HDInsight Hadoop struktúra használata](hdinsight-use-hive.md)

Az egyéb lehetőségeiről dolgozhat a HDInsight Hadoop:

* [A HDInsight Hadoop malac használata](hdinsight-use-pig.md)

* [A HDInsight Hadoop MapReduce használata](hdinsight-use-mapreduce.md)

Ha a struktúra Tez használja, olvassa el a hibakereséshez információt a következő dokumentumokat:

* [Windows-alapú HDInsight a Tez felhasználói felületének használata](hdinsight-debug-tez-ui.md)

* [A HDInsight Linux-alapú Ambari Tez nézet használata](hdinsight-debug-ambari-tez-view.md)

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md

[putty]: http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md



[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx


[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png

