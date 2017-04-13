<properties
   pageTitle="A HDInsight Curl Hadoop Sqoop használata |} Microsoft Azure"
   description="Megtudhatja, hogy miként távolról elküldése Curl használatával HDInsight Sqoop feladatokat."
   services="hdinsight"
   documentationCenter=""
   authors="mumian"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/21/2016"
   ms.author="jgao"/>

#<a name="run-sqoop-jobs-with-hadoop-in-hdinsight-with-curl"></a>Futtassa a Sqoop feladatok Hadoop a hdinsight szolgáltatáshoz a Curl használatával

[AZURE.INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Megtudhatja, hogy miként Curl használja a HDInsight Hadoop fürt Sqoop feladatok futtatásához.

Curl bemutatják, hogyan lehetősége nyílik az HDInsight futtatása, figyelésére és az eredmények Sqoop feladatok beolvasásához nyers HTTP-kérések használatával szolgál. Ez a módszer a WebHCat REST API-t (korábbi nevén Templeton) a HDInsight fürt által biztosított használatával.

> [AZURE.NOTE] Ha már jól ismert Linux-alapú Hadoop-kiszolgálót használ, de új HDInsight, [HDInsight Linux használatával kapcsolatos](hdinsight-hadoop-linux-information.md)információk.

##<a name="prerequisites"></a>Előfeltételek

A jelen cikkben ismertetett lépések elvégzéséhez a következőkre lesz szüksége:

* A Hadoop HDInsight fürt (Linux vagy Windows-alapú)

* [Curl](http://curl.haxx.se/)

* [jq](http://stedolan.github.io/jq/)

##<a name="submit-sqoop-jobs-by-using-curl"></a>Sqoop feladatok elküldése Curl használatával

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

    Az URL-címet, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, elejére összes kérelem azonos lesz. Az elérési út **/status/status**, azt jelzi, hogy a kérelem visszatérési WebHCat (más néven Templeton) állapotot a kiszolgálón. 

2. A következő segítségével sqoop feladat elküldése:


        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d command="export --connect jdbc:sqlserver://SQLDATABASESERVERNAME.database.windows.net;user=USERNAME@SQLDATABASESERVERNAME;password=PASSWORD;database=SQLDATABASENAME --table log4jlogs --export-dir /tutorials/usesqoop/data --input-fields-terminated-by \0x20 -m 1" -d statusdir="wasbs:///example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/sqoop

    Ez a parancs használt paraméterek a következők:

    * **-d** - óta `-G` nem használják, a bejegyzés módszer alapértelmezés szerint a kérést. `-d`Itt adhatja meg az adatértékek küldött a kérést.

        * **user.name** – a felhasználót, hogy fut a parancsot.

        * **parancs** - végrehajtani a Sqoop parancsot.

        * **statusdir** – a könyvtárat, amely a feladat állapota írandó.

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

##<a name="limitations"></a>Korlátozások

* Tömeges export - a Linux-alapú HDInsight, az adatok exportálása a Microsoft SQL Server- vagy SQL Azure-adatbázis használt Sqoop összekötő jelenleg nem támogatja a tömeges szúr be.

* Kötegelés – a HDInsight Linux-alapú használata esetén a `-batch` váltás beszúrása elvégzésekor, Sqoop több beszúrása a Beszúrás műveletek kötegelés helyett hajt végre.

##<a name="summary"></a>Összefoglalás

Ahogyan az ehhez a dokumentumhoz, nyers HTTP kérelem futtatása, figyelésére és az eredmény Sqoop feladatok megjelenítése a HDInsight fürt is használhatja.

További információt a jelen cikkben használt többi felületén lásd: az <a href="https://sqoop.apache.org/docs/1.99.3/RESTAPI.html" target="_blank">Útmutató Sqoop REST API-hoz</a>.

##<a name="next-steps"></a>Következő lépések

Ha általános információkra a HDInsight struktúra:

* [A HDInsight Hadoop Sqoop használata](hdinsight-use-sqoop.md)

Az egyéb lehetőségeiről dolgozhat a HDInsight Hadoop:

* [A HDInsight Hadoop struktúra használata](hdinsight-use-hive.md)

* [A HDInsight Hadoop malac használata](hdinsight-use-pig.md)

* [A HDInsight Hadoop MapReduce használata](hdinsight-use-mapreduce.md)

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


