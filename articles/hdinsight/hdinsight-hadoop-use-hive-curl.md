<properties
   pageTitle="A HDInsight Curl Hadoop-struktúra használata |} Microsoft Azure"
   description="Megtudhatja, hogy miként távolról elküldése Curl használatával HDInsight malac feladatokat."
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
   ms.date="09/07/2016"
   ms.author="larryfr"/>

#<a name="run-hive-queries-with-hadoop-in-hdinsight-with-curl"></a>A Hadoop hdinsight szolgáltatáshoz a Curl használatával a struktúra-lekérdezések futtatása

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

A jelen dokumentum megtanulhatja, hogyan Curl segítségével struktúra lekérdezések futtatása a Hadoop fürt Azure hdinsight szolgáltatáshoz a.

Curl bemutatják, hogyan lehetősége nyílik az HDInsight futtatása, figyelésére és struktúra lekérdezés eredményének beolvasásához nyers HTTP-kérések használatával szolgál. Ez a módszer a WebHCat REST API-t (korábbi nevén Templeton) a HDInsight fürt által biztosított használatával.

> [AZURE.NOTE] Ha már jól ismert Linux-alapú Hadoop-kiszolgálót használ, de új HDInsight, olvassa el a [Mit kell tudni a Linux-alapú HDInsight Hadoop](hdinsight-hadoop-linux-information.md)című témakört.

##<a id="prereq"></a>Előfeltételek

A jelen cikkben ismertetett lépések elvégzéséhez a következőkre lesz szüksége:

* A Hadoop HDInsight fürt (Linux vagy Windows-alapú)

* [Curl](http://curl.haxx.se/)

* [jq](http://stedolan.github.io/jq/)

##<a id="curl"></a>Struktúra lekérdezések futtatása Curl használatával

> [AZURE.NOTE] Használatakor Curl vagy bármely más többi kommunikáció a WebHCat, a felhasználónév és jelszó megadásával a HDInsight fürt rendszergazda hitelesítést végezni a kérelmeket. A csoport nevét is: az egységes erőforrás azonosító (URI) használja a kérelem küld a kiszolgáló részeként kell használnia.
>
> Ebben a szakaszban a parancsok **felhasználónév** cserélje ki a felhasználót, hogy a fürthöz hitelesítést végezni, és **jelszó** cserélje ki a felhasználói fiók jelszava. **CLUSTERNAME** cserélje le a csoport nevére.
>
> [Alapszintű hitelesítés](http://en.wikipedia.org/wiki/Basic_access_authentication)keresztül védett a REST API-t. Mindig készítsen kérések biztosíthatja, hogy a hitelesítő adatok biztonságos elküldi a kiszolgáló biztonságos HTTP (HTTPS) segítségével.

1. A parancssorból a következő parancs segítségével ellenőrizze, hogy tud csatlakozni a HDInsight fürthöz:

        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status

    A válasz, a következőhöz hasonló kell megjelennie:

        {"status":"ok","version":"v1"}

    Ez a parancs használt paraméterek a következők:

    * **-u** – a felhasználónevet és jelszót hitelesíti a kérést.
    * **-G** - jelzi, hogy a GET kérésekben.

    Az URL-címet, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, elejére összes kérelem azonos lesz. Az elérési út **/status/status**, azt jelzi, hogy a kérelem visszatérési WebHCat (más néven Templeton) állapotot a kiszolgálón. A következő parancs használatával is kérhet struktúra verziója:

        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/version/hive

    Ez térjen vissza a válasz a következőhöz hasonló:

        {"module":"hive","version":"0.13.0.2.1.6.0-2103"}

2. A következő segítségével **log4jLogs**nevű új tábla létrehozása:

        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="set+hive.execution.engine=tez;DROP+TABLE+log4jLogs;CREATE+EXTERNAL+TABLE+log4jLogs(t1+string,t2+string,t3+string,t4+string,t5+string,t6+string,t7+string)+ROW+FORMAT+DELIMITED+FIELDS+TERMINATED+BY+' '+STORED+AS+TEXTFILE+LOCATION+'wasbs:///example/data/';SELECT+t4+AS+sev,COUNT(*)+AS+count+FROM+log4jLogs+WHERE+t4+=+'[ERROR]'+AND+INPUT__FILE__NAME+LIKE+'%25.log'+GROUP+BY+t4;" -d statusdir="wasbs:///example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/hive

    Ez a parancs használt paraméterek a következők:

    * **-d** - óta `-G` nem használják, a bejegyzés módszer alapértelmezés szerint a kérést. `-d`Itt adhatja meg az adatértékek küldött a kérést.

        * **user.name** – a felhasználót, hogy fut a parancsot.

        * **végrehajtás** - végrehajtani a HiveQL kimutatások.

        * **statusdir** – a könyvtárat, amely a feladat állapota írandó.

    Ezek a kimutatások hajtsa végre az alábbi műveleteket:

    * **DROP TABLE** - törli a táblázat és az adatfájl, ha a táblázat már létezik.

    * **Külső tábla létrehozása** - táblát hoz létre egy új "külső" struktúra. Külső tábla csak a táblázat definíció struktúra tárolja. Az adatok az eredeti helyén marad.

        > [AZURE.NOTE] Külső tábla kell használni, amikor a külső forrásból, például egy automatizált adatok feltöltése folyamatban, vagy egy másik MapReduce művelet, amelyet frissíteni kell, de mindig szeretné használni a legfrissebb adatok struktúra lekérdezések a mögöttes adatok várt.
        >
        > Tartalmaz külső tábla húzással **nem** törlése az adatokat, csak a tábla definícióját.

    * **Sor formátum** - struktúra alapján, az adatok formázását. Ebben az esetben a mezőket az egyes naplók a szóközzel vannak elválasztva.

    * **AS TEXTFILE helyen tárolt** - struktúra alapján, az adatokat tárolja (a példában/adatkönyvtárának), és hogy szövegként tárolt.

    * Egy adott oszlop **t4** **[ERROR]**értéket tartalmazza sorok összes számát, **VÁLASSZA a** - kijelöli. Három, ezt az értéket tartalmazó sorok szerint meg kell vissza a **3** értéket.

    > [AZURE.NOTE] Figyelje meg, hogy HiveQL kimutatások közötti térközök helyébe a `+` Curl megadásakor karaktert. Nem tartalmazhat szóközt, például az elválasztó karakterrel jegyzett értékeket kell helyettesíteni `+`.

    * **INPUT__FILE__NAME PÉLDÁUL "% 25.log"** – Ez csak a keresés végződésű fájlok használata korlátai. naplót. Ha ez nem szerepel, a struktúra megkísérli az összes fájl keresni ezt könyvtárat és alkönyvtárait, beleértve a fájlokat, amelyek nem felelnek meg az alábbi táblázatban meghatározott oszlop séma.

    > [AZURE.NOTE] Megjegyzés: a % 25 az URL-cím %-os űrlap kódolva, így a tényleges feltétel `like '%.log'`. A % rendelkezik URL-címként kódolandó, akkor azt az URL-speciális karakter számít.

    Ez a parancs térjen vissza a feladat azonosítója, a feladat állapotának ellenőrzése használható.

        {"id":"job_1415651640909_0026"}

3. A feladat állapotának ellenőrzése, használja a következő parancsot. Cserélje ki az értéket adja vissza az előző lépésben **JOBID** . Ha például a visszaadott érték lett `{"id":"job_1415651640909_0026"}`, majd **JOBID** lenne `job_1415651640909_0026`.

        curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state

    Ha a feladat befejeződött, az állapot lesz **sikerült**.

    > [AZURE.NOTE] A Curl kérelem információkkal, a feladat; JavaScript objektum jelölés (JSON) dokumentum adja eredményül. csak az állapot érték beolvasása jq szolgál.

4. Miután **sikerült**a feladat állapota módosítását, az eredmények a feladat beolvasható Azure Blob-tárolóhoz. A `statusdir` a lekérdezés az átadott paraméter tartalmaz helyét a kimeneti fájl; Ebben az esetben **wasbs: / / / Példa/curl**. A cím a kimenet a feladat tárolja a HDInsight fürt által használt alapértelmezett tároló tárolóját **Példa/curl** könyvtárában található.

    Listára, és ezek a fájlok letöltése az [Azure CLI](../xplat-cli-install.md)használatával. Lista fájlok **Példa/curl**, például a következő parancsot használja:

        azure storage blob list <container-name> example/curl

    Egy fájl letöltésére, használja az alábbiakat:

        azure storage blob download <container-name> <blob-name> <destination-file>

    > [AZURE.NOTE] Vagy meg kell adnia a tárhely fiók nevét, amely tartalmazza a blob használatával a `-a` és `-k` paramétereket vagy beállítása a **AZURE\_tároló\_fiók** és **AZURE\_tároló\_ACCESS\_kulcs** környezeti változók. Lásd: < egy href = "hdinsight-feltöltés-data.md" target = "_blank" További információt.

6. A következő kimutatások használatával **errorLogs**nevű új "belső" tábla létrehozása:

        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="set+hive.execution.engine=tez;CREATE+TABLE+IF+NOT+EXISTS+errorLogs(t1+string,t2+string,t3+string,t4+string,t5+string,t6+string,t7+string)+STORED+AS+ORC;INSERT+OVERWRITE+TABLE+errorLogs+SELECT+t1,t2,t3,t4,t5,t6,t7+FROM+log4jLogs+WHERE+t4+=+'[ERROR]'+AND+INPUT__FILE__NAME+LIKE+'%25.log';SELECT+*+from+errorLogs;" -d statusdir="wasbs:///example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/hive

    Ezek a kimutatások hajtsa végre az alábbi műveleteket:

    * **Hozzon létre táblázat IF NOT EXISTS** - táblát hoz létre, ha még nem létezik. A **külső** kulcsszó nem használatos, mivel az egy belső tábla, amely a struktúra adatraktár tárolja, és a struktúra teljesen kezeli.

        > [AZURE.NOTE] Külső táblázatok, eltérően egy belső táblázat leválasztása törli a mögöttes adatok.

    * **TÁROLVA mint ORC** - optimalizált sor oszlopos (ORC) formátumban adatait tárolja. Ez a struktúra adatok tárolására szolgáló erősen optimalizált és hatékony formátumot.
    * Beszúrás **FELÜLÍRÁSA... VÁLASSZA a** - sorok **[hiba]**tartalmazó **log4jLogs** táblából, majd az adatok beszúrása a **errorLogs** táblázatba.
    * Minden sor, **Jelölje be** - kijelöli az új **errorLogs** értékeket.

7. Használja a feladat állapotának ellenőrzése vissza feladat Azonosítóját. Miután sikerült, használatával Azure CLI for Mac, Linux és leírtak szerint a Windows korábban töltse le és az eredmény megjelenítése. A kimenet három vonalat, ami a **[ERROR]**tartalmaznia kell tartalmaznia.


##<a id="summary"></a>Összefoglalás

Ahogyan az ehhez a dokumentumhoz, nyers HTTP kérelem futtatása, figyelésére és az eredmény struktúra feladatok megjelenítése a HDInsight fürt is használhatja.

További információt a jelen cikkben használt többi felületén lásd: a <a href="https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference" target="_blank">WebHCat hivatkozást</a>.

##<a id="nextsteps"></a>Következő lépések

Ha általános információkra a HDInsight struktúra:

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




[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md

[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx


