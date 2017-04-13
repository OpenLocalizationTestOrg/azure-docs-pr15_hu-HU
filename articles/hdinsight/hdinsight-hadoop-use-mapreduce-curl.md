<properties
   pageTitle="Használata MapReduce és a Hadoop a HDInsight Curl |} Microsoft Azure"
   description="Megtudhatja, hogy miként távolról MapReduce feladat futtatható a Hadoop Curl használatával hdinsight szolgáltatásból lehetőségre."
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

#<a name="run-mapreduce-jobs-with-hadoop-on-hdinsight-using-curl"></a>MapReduce feladat futtatható a Hadoop HDInsight Curl használatával

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

A jelen dokumentum megtanulhatja Curl használatáról a Hadoop HDInsight fürt MapReduce feladat futtatható.

Curl bemutatják, hogyan lehetősége nyílik az HDInsight MapReduce feladatok futtatásához nyers HTTP-kérések használatával szolgál. Ez a módszer a WebHCat REST API-t (korábbi nevén Templeton) a HDInsight fürt által biztosított használatával.

> [AZURE.NOTE] Ha már jól ismert Linux-alapú Hadoop-kiszolgálót használ, de nem HDInsight, olvassa el a [Mit kell tudni a HDInsight Linux-alapú Hadoop](hdinsight-hadoop-linux-information.md)című témakört.

##<a id="prereq"></a>Előfeltételek

A jelen cikkben ismertetett lépések elvégzéséhez a következőkre lesz szüksége:

* A Hadoop HDInsight fürt (Linux vagy Windows-alapú)

* [Curl](http://curl.haxx.se/)

* [jq](http://stedolan.github.io/jq/)

##<a id="curl"></a>Curl használatával MapReduce feladatok futtatása

> [AZURE.NOTE] Amikor az WebHCat Curl vagy bármely más többi kommunikáció használja, a HDInsight fürt rendszergazdai felhasználónév és jelszó megadásával hitelesítést végezni a kérelmeket. A csoport nevét, amely a kérést küld a kiszolgáló a URI részeként is használja.
>
> Ebben a szakaszban a parancsok a felhasználót, hogy a felhasználói fiók jelszavának a fürthöz és a **JELSZAVÁT** hitelesítéshez **felhasználónév** helyére. **CLUSTERNAME** cserélje le a csoport nevére.
>
> A REST API védett [Alapszintű hitelesítés](http://en.wikipedia.org/wiki/Basic_access_authentication)használatával. Mindig készítsen kérések HTTPS annak érdekében, hogy a hitelesítő adatok biztonságos elküldi a kiszolgáló használatával.

1. A parancssorból a következő parancs segítségével ellenőrizze, hogy tud csatlakozni a HDInsight fürthöz:

        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status

    A válasz, a következőhöz hasonló kell megjelennie:

        {"status":"ok","version":"v1"}

    Ez a parancs használt paraméterek a következők:

    * **-u**: jelzi a felhasználónevet és jelszót hitelesíti a kérelmet
    * **-G**: jelzi, hogy a GET kérésekben

    A URI, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, elejére megegyezik az összes kérelmet.

2. MapReduce feladat elküldése, használja az alábbi parancsot:

        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d jar=wasbs:///example/jars/hadoop-mapreduce-examples.jar -d class=wordcount -d arg=wasbs:///example/data/gutenberg/davinci.txt -d arg=wasbs:///example/data/CurlOut https://CLUSTERNAME.azurehdinsight.net/templeton/v1/mapreduce/jar

    A URI (/ mapreduce/üveg) végén WebHCat Ez az, hogy a kérelem MapReduce projekt indítása egy osztály üveg fájlban. Ez a parancs használt paraméterek a következők:

    * **-d**: `-G` nem használatos, így a kérelem alapértelmezés szerint a bejegyzés módszerrel. `-d`Itt adhatja meg az adatértékek küldött a kérést.

        * **User.Name**: A felhasználó, aki fut a parancs
        * **üveg**: kell osztály tartalmazó üveg-fájl helyét futtatta
        * **Class**: a MapReduce logika tartalmazó osztály
        * **argumentum**: a MapReduce feladat; átadandó argumentumok a kimenet használt ebben az esetben, a bemeneti szövegfájlt, és a címtár

    Ez a parancs térjen vissza a feladat azonosítója használt a feladat állapotának ellenőrzése:

        {"id":"job_1415651640909_0026"}

3. A feladat állapotának ellenőrzése, használja a következő parancsot. Cserélje ki a **JOBID** az előző lépésben visszaadott érték. Ha például a visszaadott érték lett `{"id":"job_1415651640909_0026"}`, akkor válassza a JOBID `job_1415651640909_0026`.

        curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state

    Ha a feladat befejezése, az állapot fog kell "sikeres".

    > [AZURE.NOTE] A Curl kérelem információkkal, a feladat; JSON dokumentum adja eredményül. csak az állapot érték beolvasása jq szolgál.

4. Ha **sikerült**a feladat állapota módosítását, az eredmények a feladat beolvasható Azure Blob-tárolóhoz. A `statusdir` a lekérdezés az átadott paraméter tartalmaz helyét a kimeneti fájl; Ebben az esetben **wasbs: / / / Példa/curl**. A cím a kimenet a feladat a **Példa/curl** könyvtár az alapértelmezett tároló tárolóban a HDInsight fürt által használt tárol.

Listára, és ezek a fájlok letöltése az [Azure CLI](../xplat-cli-install.md)használatával. Ha például a **Példa/curl**lista fájlok használja a következő parancsot:

    azure storage blob list <container-name> example/curl

Egy fájl letöltésére, használja az alábbiakat:

    azure storage blob download <container-name> <blob-name> <destination-file>

> [AZURE.NOTE] Meg kell adnia a tárhely fiók nevét, amely tartalmazza a blob használatával a `-a` és `-k` paramétereket vagy beállítása a **AZURE\_tároló\_fiók** és **AZURE\_tároló\_ACCESS\_kulcs** környezeti változók. Megtudhatja, [hogy miként tölthet fel az adatok HDInsight](hdinsight-upload-data.md) további információt.

##<a id="summary"></a>Összefoglalás

Ahogyan az ehhez a dokumentumhoz, nyers HTTP-kérelem futtatása, figyelésére és struktúra feladatok eredményeinek megtekintése a HDInsight fürt is használhatja.

A többi felület, a jelen cikkben használt kapcsolatos további tudnivalókért lásd: a [WebHCat hivatkozást](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).

##<a id="nextsteps"></a>Következő lépések

A HDInsight MapReduce feladatok általános információt:

* [A HDInsight Hadoop MapReduce használata](hdinsight-use-mapreduce.md)

További módszerek információt dolgozhat a HDInsight Hadoop:

* [A HDInsight Hadoop struktúra használata](hdinsight-use-hive.md)

* [A HDInsight Hadoop malac használata](hdinsight-use-pig.md)
