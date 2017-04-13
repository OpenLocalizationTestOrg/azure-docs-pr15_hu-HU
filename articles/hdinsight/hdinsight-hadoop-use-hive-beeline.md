<properties
   pageTitle="Beeline használata HDInsight (Hadoop) a struktúra |} Microsoft Azure"
   description="Megtudhatja, hogy miként SSH használatával a HDInsight Hadoop fürtre, és interaktív küldje struktúra lekérdezések Beeline használatával. Beeline segédprogram, amely fölé JDBC HiveServer2 használata."
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
   ms.date="10/10/2016"
   ms.author="larryfr"/>

#<a name="use-hive-with-hadoop-in-hdinsight-with-beeline"></a>A HDInsight Beeline a Hadoop struktúra használata

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

Ebben a cikkben megtanulhatja, hogyan csatlakozás Linux-alapú HDInsight fürthöz és interaktív küldje struktúra lekérdezések az [Beeline](https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Clients#HiveServer2Clients-Beeline–NewCommandLineShell) parancssori eszközzel biztonságos rendszerhéj (SSH) segítségével.

> [AZURE.NOTE] Beeline JDBC használja a struktúra csatlakozni. További tájékoztatást a struktúra JDBC használja olvassa el a [Csatlakozás az Azure hdinsight szolgáltatáshoz a struktúra JDBC illesztőprogram használata a struktúra](hdinsight-connect-hive-jdbc-driver.md).

##<a id="prereq"></a>Előfeltételek

A jelen cikkben ismertetett lépések elvégzéséhez a következőkre lesz szüksége:

* Egy Linux-alapú Hadoop fürt hdinsight szolgáltatásból lehetőségre.

* Egy SSH ügyfél. Linux rendszerhez, a Unix és a Mac OS érkezik egy SSH ügyfélprogrammal. Windows felhasználók le kell töltenie a ügyfélprogram, például [gitt](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

##<a id="ssh"></a>Csatlakozás a SSH segítségével

Csatlakozás a teljes tartománynevét (FQDN) a HDInsight fürt a SSH parancs használatával. A teljesen minősített tartománynév lesz a fürt, majd megadott névnek **. azurehdinsight.net**. Ha például a következő kíván csatlakozni **myhdinsight**nevű fürt:

    ssh admin@myhdinsight-ssh.azurehdinsight.net

**Ha a megadott SSH hitelesítési tanúsítvány kulcsot** a HDInsight fürt létrehozásakor, előfordulhat, a titkos kulcs helyének megadásához az ügyfél rendszeren:

    ssh admin@myhdinsight-ssh.azurehdinsight.net -i ~/mykey.key

**Jelszó SSH hitelesítéshez amennyiben** a HDInsight fürt létrehozásakor, meg kell adja meg a jelszót, amikor a rendszer kéri.

HDInsight való SSH használatáról további tudnivalókért lásd: [Használata SSH a Linux-alapú Hadoop a HDInsight Linux rendszerhez, a OS X, és a Unix](hdinsight-hadoop-linux-use-ssh-unix.md).

###<a name="putty-windows-based-clients"></a>GITT (Windows-alapú ügyfelek)

A Windows nem nyújt a beépített SSH ügyfél. **Gitt**, amely letölthető a [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)használata ajánlott.

GITT használatával kapcsolatos további tudnivalókért lásd: [Használata SSH a Linux-alapú Hadoop meg a Windows HDInsight ](hdinsight-hadoop-linux-use-ssh-windows.md).

##<a id="beeline"></a>A Beeline paranccsal

1. Miután létrejött, a következő segítségével Beeline indítása:

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin

    Ezzel a Beeline ügyfélprogram indítása, és csatlakozás JDBC URL-címét. Ebben az esetben `localhost` HiveServer2 futtatja a fürt mindkét központi csomóponton, és közvetlenül a az elsődleges headnode Beeline futtatja azt használja.
    
    Ha befejeződött a parancs, akkor a érkezik egy `jdbc:hive2://localhost:10001/>` kérdés.

3. Általában kezdődő beeline parancsok a `!` karakter, például `!help` megjeleníti a súgót. Azonban a `!` gyakran hagyható. Ha például `help` is működnek.

    Ha meg súgó, megfigyelheti `!sql`, mellyel HiveQL utasítás végrehajtása. Azonban HiveQL így általában arra használják, hogy az előző kihagyhatja `!sql`. Az alábbi két utasítás van pontosan ugyanazt az eredményt; a táblák jelenleg elérhető struktúra megjelenítése:
    
        !sql show tables;
        show tables;
    
    Egy új fürtre, csak egy táblából kell szerepel a felsorolásban: __hivesampletable__.

4. A következő segítségével megjeleníteni sémája a hivesampletable:

        describe hivesampletable;
        
    Ez ad eredményül a következő adatokat:
    
        +-----------------------+------------+----------+--+
        |       col_name        | data_type  | comment  |
        +-----------------------+------------+----------+--+
        | clientid              | string     |          |
        | querytime             | string     |          |
        | market                | string     |          |
        | deviceplatform        | string     |          |
        | devicemake            | string     |          |
        | devicemodel           | string     |          |
        | state                 | string     |          |
        | country               | string     |          |
        | querydwelltime        | double     |          |
        | sessionid             | bigint     |          |
        | sessionpagevieworder  | bigint     |          |
        +-----------------------+------------+----------+--+

    Az oszlopok megjelenik a táblázatban. Miközben azt végrehajthat bizonyos adatok lekérdezéseket, helyette vadonatúj táblázat létrehozása keresztül mutatjuk struktúra adatok betöltése és alkalmazása a séma.
    
5. Írja be a következő kimutatások **log4jLogs** nevű mintaadatokból a HDInsight fürthöz használatával új tábla létrehozása:

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
    * **INPUT__FILE__NAME PÉLDÁUL "%.log"** - struktúra alapján visszaadó azt kell csak adatok végződésű fájlokból. naplót. Általában azt szeretné csak, hogy ugyanarra a sémára adatok ugyanabban a mappában lekérdezés a struktúra, ha azonban ez a Példa naplófájl egyéb adatok formátumokkal tárolja.

    > [AZURE.NOTE] Külső tábla kell használni, amikor a külső forrásból, például egy automatikus feltöltés adatfeldolgozás, vagy egy másik MapReduce művelet, amelyet frissíteni kell, de mindig szeretné használni a legfrissebb adatok struktúra lekérdezések a mögöttes adatok várt.
    >
    > Tartalmaz külső tábla húzással **nem** törlése az adatokat, csak a tábla definícióját.
    
    Ez a parancs a következőképpen fog kinézni a következőket:
    
        INFO  : Tez session hasn't been created yet. Opening session
        INFO  :
        
        INFO  : Status: Running (Executing on YARN cluster with App id application_1443698635933_0001)
        
        INFO  : Map 1: -/-      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0(+1)/1  Reducer 2: 0/1
        INFO  : Map 1: 0(+1)/1  Reducer 2: 0/1
        INFO  : Map 1: 1/1      Reducer 2: 0/1
        INFO  : Map 1: 1/1      Reducer 2: 0(+1)/1
        INFO  : Map 1: 1/1      Reducer 2: 1/1
        +----------+--------+--+
        |   sev    | count  |
        +----------+--------+--+
        | [ERROR]  | 3      |
        +----------+--------+--+
        1 row selected (47.351 seconds)

4. Beeline elhagyásához használata `!quit`.

##<a id="file"></a>HiveQL fájl futtatása

Beeline is használható HiveQL utasításokat tartalmazó fájl futtatására. Az alábbi lépésekkel használatával hozzon létre egy fájlt, majd a Futtatás Beeline használatával.

1. A következő paranccsal __query.hql__nevű új fájl létrehozása:

        nano query.hql
        
2. Miután megnyílt a szerkesztő, használja az alábbi a fájl tartalmát. Ezt a lekérdezést hoz létre egy új "belső" táblázat **errorLogs**:

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    Ezek a kimutatások hajtsa végre az alábbi műveleteket:

    * **Hozzon létre táblázat esetén nem létezik** - táblát hoz létre, ha még nem létezik. A **külső** kulcsszó nem használatos, mivel az egy belső tábla, amely a struktúra adatraktár tárolja, és a struktúra teljesen kezeli.
    * **TÁROLVA mint ORC** - optimalizált sor oszlopos (ORC) formátumban adatait tárolja. Ez a struktúra adatok tárolására szolgáló erősen optimalizált és hatékony formátumot.
    * Beszúrás **FELÜLÍRÁSA... VÁLASSZA a** - sorok **[hiba]**tartalmazó **log4jLogs** táblából, majd az adatok beszúrása a **errorLogs** táblázatba.
    
    > [AZURE.NOTE] Külső táblázatok, eltérően egy belső táblázat leválasztása törli a mögöttes adatok.
    
3. Mentse a fájlt, használja a __Ctrl__+ ___X__, majd adja meg az __Y__, végül az __ENTER billentyűt__.

4. A következő segítségével a fájlt a Beeline futtathatja. __Hostname (állomásnév)__ cserélje ki a korábban kapott a központi csomópontot, és a __JELSZÓT__ a rendszergazdai fiók jelszava neve:

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin -i query.hql

    > [AZURE.NOTE] A `-i` paraméter Beeline elindul, a kimutatások fut a query.hql fájlt, és a Beeline marad a `jdbc:hive2://localhost:10001/>` kérdés. A fájl használatával is futtathatja a `-f` paramétert, amely visszatér a Bash, miután feldolgozása megtörtént a fájlt.

5. Ha ellenőrizni szeretné, hogy a **errorLogs** tábla jött létre, a következő utasítás segítségével **errorLogs**az összes sort eredményül:

        SELECT * from errorLogs;

    Három sornyi adatot vissza kell, az összes tartalmazó **[hiba]** oszlopban t4:
    
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | errorlogs.t1  | errorlogs.t2  | errorlogs.t3  | errorlogs.t4  | errorlogs.t5  | errorlogs.t6  | errorlogs.t7  |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | 2012-02-03    | 18:35:34      | SampleClass0  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 18:55:54      | SampleClass1  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 19:25:27      | SampleClass4  | [ERROR]       | incorrect     | id            |               |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        3 rows selected (1.538 seconds)

## <a name="more-about-beeline-connectivity"></a>További információ: kapcsolat Beeline

Ennek lépéseit a dokumentum használata `localhost` HiveServer2 csatlakozni a fürt headnode futó. Miközben is használhatja a hostname (állomásnév) vagy a tartománynevét, a headnode azokat, további lépéseket a folyamathoz (a lépéseket követve megtudhatja, a hostname (állomásnév) vagy a teljes Tartománynevet) szükség. Használatával `localhost` is elegendő a headnode-Beeline használatakor.

Ha egy él csomópont a fürt, a telepített Beeline, szüksége lesz a hostname (állomásnév) vagy a teljes Tartománynevét az headnode használata.

Ha telepítve van a kívül a fürt ügyfél Beeline, csatlakoztathatja a következő parancsot. __CLUSTERNAME__ cserélje le a HDInsight fürt nevét. __Jelszó__ cserélje le a (HTTP bejelentkezési) rendszergazdai fiók jelszava.

    beeline -u 'jdbc:hive2://CLUSTERNAME.azurehdinsight.net:443/default;ssl=true?hive.server2.transport.mode=http;hive.server2.thrift.http.path=hive2' -n admin -p PASSWORD

Ne feledje, hogy a paraméterek URI ugyanaz, mint amikor operációs rendszert futtató közvetlenül egy headnode vagy egy él csomópontra a fürt belül. Ennek oka az, a fürthöz az internetről kapcsolódó használ egy nyilvános átjárót, amelyek a forgalom 443-as port fölé. Is számos más szolgáltatás elérhető nyilvános az átjáró 443-as port keresztül, a URI nem ugyanaz, mint amikor közvetlenül csatlakozik. Az internetről csatlakozáskor is kell hitelesíteni a munkamenet jelszava megadásával.

##<a id="summary"></a><a id="nextsteps"></a>Következő lépések

Amint látható, a Beeline parancs egyszerűvé egy HDInsight fürt struktúra lekérdezések interaktív futtatásához.

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

