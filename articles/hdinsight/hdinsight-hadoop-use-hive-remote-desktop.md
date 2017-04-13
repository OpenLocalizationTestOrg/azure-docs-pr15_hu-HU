<properties
   pageTitle="Használja Hadoop-struktúra és a távoli asztali HDInsight |} Microsoft Azure"
   description="Megtudhatja, hogy miként csatlakozhat HDInsight Hadoop-fürt távoli asztali változatában, és kattintson a struktúra parancssori felületén a struktúra lekérdezések futtatása."
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
   ms.date="09/06/2016"
   ms.author="larryfr"/>

# <a name="use-hive-with-hadoop-on-hdinsight-with-remote-desktop"></a>Hadoop a HDInsight a távoli asztali struktúrát használata

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

Ebben a cikkben használni megtudhatja, hogy miként csatlakozhat egy HDInsight fürthöz távoli asztali változatában, és futtassa a struktúra lekérdezések struktúra parancssori felület (CLI) használatával.

> [AZURE.NOTE] A dokumentum nem biztosít a példákban használt HiveQL kimutatások mire részletes leírását. Az ebben a példában használt HiveQL tudni olvassa el a [Hadoop a HDInsight a struktúra használata](hdinsight-use-hive.md)című témakört.

##<a id="prereq"></a>Előfeltételek

A jelen cikkben ismertetett lépések elvégzéséhez a következőkre lesz szüksége:

* A Windows-alapú HDInsight (Hadoop a HDInsight) fürtre

* A Windows 10, ablak 8 vagy Windows 7 rendszert futtató ügyfélszámítógép

##<a id="connect"></a>Csatlakozás távoli asztali

A HDInsight fürt engedélyezése a távoli asztali, majd a [Csatlakozás RDP segítségével HDInsight fürt](hdinsight-administer-use-management-portal.md#rdp)utasításait követve csatlakozni.

##<a id="hive"></a>A struktúra paranccsal

Ha csatlakozott a HDInsight fürt az asztalon, kövesse az alábbi lépéseket struktúra dolgozhat:

1. A HDInsight asztalról indítsa el a **Hadoop parancssori**.

2. Írja be a struktúra CLI indítsa el a következő parancsot:

        %hive_home%\bin\hive

    A CLI indításakor látni fogja a struktúra CLI kérdés: `hive>`.

3. A CLI használ, írja be a következő kimutatások **log4jLogs** példaadatok használatával nevű új tábla létrehozása:

        set hive.execution.engine=tez;
        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    Ezek a kimutatások hajtsa végre az alábbi műveleteket:

    * **Táblázat LEVÁLASZTÁSA**: a táblázat és az adatfájl törli, ha a táblázat már létezik.

    * **Külső tábla létrehozása**: új táblát hoz létre külső"a struktúra. Külső tábla csak a táblázat definition (az adatok balra az eredeti helyén) struktúra tárolja.

        > [AZURE.NOTE] Külső táblák kell használni, amikor a mögöttes adatok frissítése külső forrásból (például egy automatikus feltöltés adatfeldolgozás) vagy egy másik MapReduce művelet várt, de mindig kívánt struktúra lekérdezések használata a legfrissebb adatokat.
        >
        > Tartalmaz külső tábla húzással **nem** törlése az adatokat, csak a tábla definícióját.

    * **Sor FORMAT**: Ez az információ struktúra, az adatok formázását. Ebben az esetben a mezőket az egyes naplók vannak elválasztva szóközt.

    * **AS TEXTFILE helyen tárolt**: Ez az információ struktúra az adatokat tárolja (a példában/adatkönyvtárának), és szövegként tárolt.

    * **Jelölje ki**: a száma, ahol oszlop **t4** **[ERROR]**értéket tartalmazza az összes sor kijelölése. Ez kell visszaadni értéke **3** , mert a három, ezt az értéket tartalmazó sorok.

    * **INPUT__FILE__NAME PÉLDÁUL "%.log"** - struktúra alapján visszaadó azt kell csak adatok végződésű fájlokból. naplót. E beállítás hatására a Keresés a adatokat tartalmazza, amely megőrzi a származó adatokat visszaadó más példából, amelyek nem felelnek meg a definiált séma adatfájlok sample.log fájlt.


4. A következő kimutatások használatával **errorLogs**nevű új "belső" tábla létrehozása:

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    Ezek a kimutatások hajtsa végre az alábbi műveleteket:

    * **Hozzon létre táblázat esetén nem létezik**: létrehoz egy táblázatot, ha még nem létezik. A **külső** kulcsszó nem használatos, mert az egy belső tábla, amely a struktúra adatraktár tárolja, és a struktúra teljesen kezeli.

        > [AZURE.NOTE] **Külső** táblázatok, eltérően egy belső tábla húzással is törli a mögöttes adatok.

    * **TÁROLVA mint ORC**: optimalizált sor oszlopos formátumban (ORC) adatait tárolja. Ez a struktúra adatok tárolására szolgáló erősen optimalizált és hatékony formátumot.

    * Beszúrás **FELÜLÍRÁSA... VÁLASSZA a**: kiválasztja az **log4jLogs** értékeket tartalmazó sorok **[hiba]**, majd beilleszti az adatokat a **errorLogs** táblázatba.

    Ha ellenőrizni szeretné, hogy csak az a oszlop t4 **[ERROR]** tartalmazó sorokat a **errorLogs** táblába tárolta, az alábbi utasítás segítségével **errorLogs**az összes sort eredményül:

        SELECT * from errorLogs;

    Három sornyi adatot vissza kell, az összes tartalmazó **[ERROR]** oszlopban t4 a.

##<a id="summary"></a>Összefoglalás

Amint látható, az a struktúra parancs egyszerűvé interaktív struktúra lekérdezések futtatása egy HDInsight fürt, figyelheti a projekt állapotát, valamint a kimenet.

##<a id="nextsteps"></a>Következő lépések

A HDInsight struktúra általános információt:

* [A HDInsight Hadoop struktúra használata](hdinsight-use-hive.md)

További módszerek információt dolgozhat a HDInsight Hadoop:

* [A HDInsight Hadoop malac használata](hdinsight-use-pig.md)

* [A HDInsight Hadoop MapReduce használata](hdinsight-use-mapreduce.md)

Ha a struktúra Tez használja, olvassa el a hibakereséshez információt a következő dokumentumokat:

* [Windows-alapú HDInsight a Tez felhasználói felületének használata](hdinsight-debug-tez-ui.md)

* [A HDInsight Linux-alapú Ambari Tez nézet használata](hdinsight-debug-ambari-tez-view.md)

[1]: ../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md

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





[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md


[Powershell-install-configure]: ../powershell-install-configure.md
[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx

