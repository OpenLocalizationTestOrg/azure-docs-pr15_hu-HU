<properties
   pageTitle="A HDInsight Curl Hadoop malac használata |} Microsoft Azure"
   description="Megtudhatja, hogy miként Curl használja az Azure hdinsight szolgáltatáshoz Hadoop fürt malac Latin feladatok futtatásához."
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

#<a name="run-pig-jobs-with-hadoop-on-hdinsight-by-using-curl"></a>Malac feladat futtatható a Hadoop HDInsight Curl használatával

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

A jelen dokumentum megtanulhatja Curl használata egy Azure hdinsight szolgáltatáshoz fürt malac Latin feladatok futtatásához. Malac Latin programnyelv megadása lehetővé teszi a bemeneti adatok kapcsol a kívánt kimeneti alkalmazott átalakítások ismertetik.

Curl bemutatják, hogyan lehetősége nyílik az HDInsight futtatása, figyelésére és malac feladatok az eredmények beolvasásához nyers HTTP-kérések használatával szolgál. Ez a módszer a WebHCat REST API-t (korábbi nevén Templeton) a HDInsight fürt által biztosított használatával.

> [AZURE.NOTE] Ha már jól ismert Linux-alapú Hadoop-kiszolgálót használ, de új HDInsight, ötletek [Linux-alapú hdinsight szolgáltatásból lehetőségre](hdinsight-hadoop-linux-information.md).

##<a id="prereq"></a>Előfeltételek

A jelen cikkben ismertetett lépések elvégzéséhez a következőkre lesz szüksége:

* Az Azure hdinsight szolgáltatáshoz (Hadoop a HDInsight) fürt (Linux- vagy Windows-alapú)

* [Curl](http://curl.haxx.se/)

* [jq](http://stedolan.github.io/jq/)

##<a id="curl"></a>Malac feladat futtatása Curl használatával

> [AZURE.NOTE] Használatakor Curl vagy bármely más többi kommunikáció a WebHCat azáltal, hogy a rendszergazda felhasználónevét és jelszavát a HDInsight fürt hitelesítést végezni a kérelmeket. A csoport nevét, a egységes erőforrás azonosító (URI), amely a kérést küld a kiszolgáló részeként is használja.
>
> Ebben a szakaszban a parancsok **felhasználónév** cserélje ki a felhasználót, hogy a fürthöz hitelesítést végezni, és cserélje le **JELSZAVÁT** a felhasználói fiók jelszavának gombra. **CLUSTERNAME** cserélje le a csoport nevére.
>
> [Alapszintű hitelesítés](http://en.wikipedia.org/wiki/Basic_access_authentication)keresztül védett a REST API-t. Mindig készítsen kérések biztosíthatja, hogy a hitelesítő adatok biztonságos elküldi a kiszolgáló biztonságos HTTP (HTTPS) segítségével.

1. A parancssorból a következő parancs segítségével ellenőrizze, hogy tud csatlakozni a HDInsight fürthöz:

        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status

    A válasz, a következőhöz hasonló kell megjelennie:

        {"status":"ok","version":"v1"}

    Ez a parancs használt paraméterek a következők:

    * **-u**: A felhasználónevet és jelszót hitelesíti a kérelmet
    * **-G**: jelzi, hogy a GET kérésekben

    Az URL-címet, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, az elején az összes kérelmet azonos lesz. Az elérési út **/status/status**, azt jelzi, hogy a kérelem való visszatéréshez (más néven Templeton) WebHCat állapotát a kiszolgálón.

2. A következő kód segítségével a fürthöz malac Latin feladat elküldése:

        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="LOGS=LOAD+'wasbs:///example/data/sample.log';LEVELS=foreach+LOGS+generate+REGEX_EXTRACT($0,'(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)',1)+as+LOGLEVEL;FILTEREDLEVELS=FILTER+LEVELS+by+LOGLEVEL+is+not+null;GROUPEDLEVELS=GROUP+FILTEREDLEVELS+by+LOGLEVEL;FREQUENCIES=foreach+GROUPEDLEVELS+generate+group+as+LOGLEVEL,COUNT(FILTEREDLEVELS.LOGLEVEL)+as+count;RESULT=order+FREQUENCIES+by+COUNT+desc;DUMP+RESULT;" -d statusdir="wasbs:///example/pigcurl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/pig

    Ez a parancs használt paraméterek a következők:

    * **-d**: mivel `-G` nem használják, a bejegyzés módszer alapértelmezés szerint a kérést. `-d`Itt adhatja meg az adatértékek küldött a kérést.

        * **User.Name**: A felhasználó, aki fut a parancs
        * **végrehajtása**: A malac latin betűs utasítás végrehajtása
        * **statusdir**: a feladat állapota írt könyvtárban

    > [AZURE.NOTE] Figyelje meg, hogy a szóközök malac Latin kimutatásokban helyébe a `+` Curl együtt használva karaktert.

    Ez a parancs térjen vissza a feladat azonosítója használt például a feladat állapotának ellenőrzése:

        {"id":"job_1415651640909_0026"}

3. A feladat állapotának ellenőrzése, használja a következő parancsot. Cserélje ki az értéket adja vissza az előző lépésben **JOBID** . Ha például a visszaadott érték lett `{"id":"job_1415651640909_0026"}`, majd **JOBID** lenne `job_1415651640909_0026`.

        curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state

    Ha a feladat befejeződött, az állapot lesz **sikerült**.

    > [AZURE.NOTE] A Curl kérelem adja eredményül a feladattal kapcsolatos információkat a JavaScript objektum jelölés (JSON) dokumentumokat, és jq csak az állapot érték beolvasásához használt.

##<a id="results"></a>Eredményeinek megtekintése

Ha **sikerült**a feladat állapota módosítását, az eredmények a feladat beolvasható Azure Blob-tárolóhoz. A `statusdir` a lekérdezés az átadott paraméter tartalmaz helyét a kimeneti fájl; Ebben az esetben **wasbs: / / / Példa/pigcurl**. A cím a kimenet a feladat az alapértelmezett tároló tárolóban a HDInsight fürt által használt **Példa/pigcurl** könyvtárában tárolja.

Listára, és ezek a fájlok letöltése az [Azure CLI](../xplat-cli-install.md)használatával. Lista fájlok **Példa/pigcurl**, például a következő parancsot használja:

    azure storage blob list <container-name> example/pigcurl

Egy fájl letöltésére, használja az alábbiakat:

    azure storage blob download <container-name> <blob-name> <destination-file>

> [AZURE.NOTE] Meg kell adnia a tárhely fiók nevét, amely tartalmazza a blob használatával a `-a` és `-k` paramétereket vagy beállítása a **AZURE\_tároló\_fiók** és **AZURE\_tároló\_ACCESS\_kulcs** környezeti változók.

##<a id="summary"></a>Összefoglalás

Ahogyan az ehhez a dokumentumhoz, nyers HTTP kérelem futtatása, figyelésére és az eredmény malac feladatok megjelenítése a HDInsight fürt is használhatja.

A jelen cikkben használt többi felület kapcsolatos további tudnivalókért olvassa el a [WebHCat hivatkozás](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference)című témakört.

##<a id="nextsteps"></a>Következő lépések

A HDInsight malac általános információt:

* [A HDInsight Hadoop malac használata](hdinsight-use-pig.md)

További módszerek információt dolgozhat a HDInsight Hadoop:

* [A HDInsight Hadoop struktúra használata](hdinsight-use-hive.md)

* [A HDInsight Hadoop MapReduce használata](hdinsight-use-mapreduce.md)
