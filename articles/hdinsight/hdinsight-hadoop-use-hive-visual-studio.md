<properties
   pageTitle="Lekérdezés, amelynek Hadoop eszközök struktúra for Visual Studio |} Microsoft Azure"
   description="Megtudhatja, hogy miként struktúra használata a Visual Studio Hadoop eszközeivel HDInsight Hadoop."
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

#<a name="run-hive-queries-using-the-hdinsight-tools-for-visual-studio"></a>A HDInsight eszközökkel for Visual Studio struktúra lekérdezések futtatása

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

Ebben a cikkben megtanulhatja a struktúra lekérdezések for Visual Studio HDInsight eszközeivel HDInsight fürthöz módját.

> [AZURE.NOTE] A dokumentum nem biztosít a HiveQL kimutatások példáiban szereplő mire részletes leírását. Az ebben a példában használt HiveQL tudni olvassa el a [Hadoop a HDInsight a struktúra használata](hdinsight-use-hive.md)című témakört.

##<a id="prereq"></a>Előfeltételek

A jelen cikkben ismertetett lépések elvégzéséhez az alábbi lesz szüksége.

* Az Azure hdinsight szolgáltatáshoz (Hadoop a HDInsight) fürt (Linux vagy Windows-alapú)

* Visual Studio (következő verzióira egyike):

    Visual Studio 2013 közösségi/Professional/prémium/Ultimate a [4-es frissítése](https://www.microsoft.com/download/details.aspx?id=44921)

    Visual Studio 2015 (közösségi vagy Enterprise)

- HDInsight tools for Visual studio. Az [első lépések](hdinsight-hadoop-visual-studio-tools-get-started.md) című for HDInsight Visual Studio Hadoop eszközeivel való telepítéséről és konfigurálásáról az eszközök olvashat.

##<a id="run"></a>A HDInsight eszközökkel for Visual Studio struktúra lekérdezések futtatása

1. Nyissa meg a **Visual Studióban** , és válassza az **Új** > **Project** > **HDInsight** > **Struktúra alkalmazást**. Adja meg a projekt nevét.

2. Nyissa meg az ehhez a projekthez létrehozott **Script.hql** fájlt, és illessze be a következő HiveQL utasításokat:

        set hive.execution.engine=tez;
        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    Ezek a kimutatások hajtsa végre az alábbi műveleteket:

    * **Táblázat LEVÁLASZTÁSA**: a táblázat és az adatfájl törli, ha a táblázat már létezik.
    * **Külső tábla létrehozása**: új táblát hoz létre külső"a struktúra. Külső táblákat a tábla definícióját csak tárolását struktúra (az eredeti helyén az adatok balra).

        > [AZURE.NOTE] Külső táblák kell használni, amikor a mögöttes adatok frissítése külső forrásból (például egy automatikus feltöltés adatfeldolgozás) vagy egy másik MapReduce művelet várt, de mindig kívánt struktúra lekérdezések használata a legfrissebb adatokat.
        >
        > Tartalmaz külső tábla húzással **nem** törlése az adatokat, csak a tábla definícióját.

    * **Sor FORMAT**: Ez az információ struktúra, az adatok formázását. Ebben az esetben a mezőket az egyes naplók vannak elválasztva szóközt.
    * **AS TEXTFILE helyen tárolt**: Ez az információ struktúra az adatokat tárolja (a példában/adatkönyvtárának), és szövegként tárolt.
    * **VÁLASSZA a**: jelölje be a hol oszlop **t4** **[ERROR]**értéket tartalmazza, sorok összes számát. Ez kell visszaadni értéke **3** , mert a három, ezt az értéket tartalmazó sorok.
    * **INPUT__FILE__NAME PÉLDÁUL "%.log"** - struktúra alapján visszaadó azt kell csak adatok végződésű fájlokból. naplót. E beállítás hatására a Keresés a adatokat tartalmazza, amely megőrzi a származó adatokat visszaadó más példából, amelyek nem felelnek meg a definiált séma adatfájlok sample.log fájlt.

3. Az eszköztáron jelölje ki a **HDInsight fürt** , amelyet használni ezt a lekérdezést, és válassza a **Küldés gombot WebHCat** a kimutatások használatával WebHCat struktúra feladatként futtatásához. A feladat- __végrehajtás HiveServer2 keresztül__ gomb segítségével, ha elérhető a verzióhoz fürt HiveServer2 is küldhet. A **Projekt-összefoglaló struktúra** , és megjeleníti a futó feladattal kapcsolatos információkat. A **frissítési** hivatkozást használva frissítheti a a projekt adatait, amíg át nem változik a **Feladat állapota** **befejeződött**.

4. Használja a **Feladat kimeneti** hivatkozásra a kimenet a feladat megtekintéséhez. Jelenítsék meg `[ERROR] 3`, amely olyan, a SELECT utasítás által visszaadott értéket.

5. Struktúra lekérdezések létrehozása a projekt nélkül is futtathatók. A **Kiszolgáló Explorer**bontsa ki a **Azure** > **HDInsight**, kattintson a jobb gombbal a HDInsight-kiszolgálóhoz, és válassza a **struktúra lekérdezés írni**.

6. A dokumentumban **temp.hql** megjelenő adja hozzá a következő HiveQL utasításokat:

        set hive.execution.engine=tez;
        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    Ezek a kimutatások hajtsa végre az alábbi műveleteket:

    * **Hozzon létre táblázat esetén nem létezik**: létrehoz egy táblázatot, ha még nem létezik. A **külső** kulcsszó nem használatos, mert az egy belső tábla, amely a struktúra adatraktár tárolja, és a struktúra teljesen kezeli.

        > [AZURE.NOTE] **Külső** táblázatok, eltérően egy belső tábla húzással is törli a mögöttes adatok.

    * **TÁROLVA mint ORC**: optimalizált sor oszlopos formátumban (ORC) adatait tárolja. Ez a struktúra adatok tárolására szolgáló erősen optimalizált és hatékony formátumot.
    * Beszúrás **FELÜLÍRÁSA... VÁLASSZA a**: kiválasztja az **log4jLogs** értékeket tartalmazó sorok **[hiba]**, majd beilleszti az adatokat a **errorLogs** táblázatba.

7. Az eszköztáron válassza a **Küldés** a feladat futtatása. A **Feladat állapota** segítségével határozza meg, hogy a feladat sikeresen befejeződött.

8. Ellenőrizze, hogy a feladat befejezése és létrehozott egy új táblát, **Kiszolgáló Explorert** használ, és bontsa ki az **Azure** > **HDInsight** > a HDInsight fürt > **Struktúra adatbázisok** > és az **alapértelmezett**. Meg kell jelennie a **errorLogs** és a **log4jLogs** táblát.

##<a id="summary"></a>Összefoglalás

Amint látható, az a HDInsight tools for Visual Studio segítségével egyszerűen egy HDInsight fürt struktúra-lekérdezések futtatása, figyelheti a projekt állapotát, valamint a kimenet.

##<a id="nextsteps"></a>Következő lépések

A HDInsight struktúra általános információt:

* [A HDInsight Hadoop struktúra használata](hdinsight-use-hive.md)

További módszerek információt dolgozhat a HDInsight Hadoop:

* [A HDInsight Hadoop malac használata](hdinsight-use-pig.md)

* [A HDInsight Hadoop MapReduce használata](hdinsight-use-mapreduce.md)

A HDInsight eszközök további információt a Visual Studio:

* [Első lépések HDInsight eszközök Visual Studio](../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md)


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



[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx

[image-hdi-hive-powershell]: ./media/hdinsight-use-hive/HDI.HIVE.PowerShell.png
[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png
[image-hdi-hive-architecture]: ./media/hdinsight-use-hive/HDI.Hive.Architecture.png
