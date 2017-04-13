<properties 
    pageTitle="A HDInsight Linux-alapú struktúra nézetbeli késleltetés adatok elemzése |} Microsoft Azure" 
    description="Megtudhatja, hogy miként struktúra használatával a Linux-alapú HDInsight nézetbeli adatok elemzése, majd az SQL-adatbázisokkal Sqoop exportálja az adatokat." 
    services="hdinsight" 
    documentationCenter="" 
    authors="Blackmist" 
    manager="jhubbard" 
    editor="cgronlun"
    tags="azure-portal"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/11/2016" 
    ms.author="larryfr"/>

#<a name="analyze-flight-delay-data-by-using-hive-in-hdinsight"></a>Nézetbeli késleltetés adatok elemzése HDInsight struktúra használatával

Megtudhatja, hogy miként elemzéséhez nézetbeli késleltetés struktúra használata Linux-alapú hdinsight szolgáltatásból lehetőségre, majd exportálja az adatokat Azure SQL-adatbázis használata Sqoop.

> [AZURE.NOTE] Bár a dokumentum adott hardvereszközöket felhasználhatók a Windows-alapú HDInsight fürt (Python és struktúra, például), több lépésben kötődnek Linux-alapú fürt. A Windows-alapú fürtre működő című témakör tartalmazza [elemzés nézetbeli késési adataival HDInsight struktúra használatával](hdinsight-analyze-flight-delay-data.md)

###<a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban megkezdése előtt a következőket kell rendelkeznie:

- **Az Azure-előfizetés**. Lásd: [Ismerkedés az Azure ingyenes próbaverziót](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- __Egy HDInsight fürt__. Lásd: a [használatba Hadoop Linux HDInsight struktúra az](hdinsight-hadoop-linux-tutorial-get-started.md) új Linux-alapú HDInsight fürt létrehozásának lépéseit.

- __Azure SQL-adatbázis__. Azure SQL-adatbázishoz, használja a céltár adatok. Ha egy SQL-adatbázis már nem rendelkezik, olvassa el a [SQL-adatbázis oktatóprogram: SQL-adatbázis létrehozása percben](../sql-database/sql-database-get-started.md).

- __Azure CLI__. Ha még nem telepítette az Azure CLI, olvassa el [Telepítse és állítsa be az Azure CLI](../xplat-cli-install.md) a további lépéseket.

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]


##<a name="download-the-flight-data"></a>Töltse le a nézetbeli adatokat

1. Tallózással keresse meg azt a [kutatási és innovatív technológiák felügyeleti, pihenőhelyek statisztikai hivatala][rita-website].
2. A lapon jelölje be az alábbi értékeket:

  	| név | Érték |
  	| ---- | ---- |
  	| Szűrő év | 2013-ban |
  	| Szűrés időszak | Január |
  	| Mezők | Év FlightDate, UniqueCarrier, Carrier, FlightNum, OriginAirportID, Origin, OriginCityName, OriginState, DestAirportID, cél, DestCityName, DestState, DepDelayMinutes, ArrDelay, ArrDelayMinutes, CarrierDelay, WeatherDelay, NASDelay, SecurityDelay, LateAircraftDelay. Törölje a jelet az összes többi mező |

3. Kattintson a **Letöltés**gombra. 

##<a name="upload-the-data"></a>Az adatok feltöltése

1. A következő parancsot használja a zip-fájl feltöltése HDInsight központi csomópont:

        scp FILENAME.csv USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:

    __Fájlnév__ cserélje le a zip-fájl nevét. A HDInsight fürt a SSH bejelentkezési __felhasználónév__ cserélje ki. CLUSTERNAME cserélje le a HDInsight fürt nevére.
    
    > [AZURE.NOTE] Ha a jelszó használatával a SSH bejelentkezési hitelesítő, a rendszer kéri, a jelszót. Nyilvános kulccsal használata esetén szüksége lehet használni a `-i` paraméter, és adja meg a megfelelő titkos kulcs elérési útja. Ha például `scp -i ~/.ssh/id_rsa FILENAME.csv USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:`.

2. Ha a feltöltés befejeződött, a fürt SSH használatával csatlakozhat:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
        
    További tájékoztatást a Linux-alapú HDInsight SSH használja az alábbi cikkekben talál:
    
    * [A HDInsight Linux, Unix vagy OS X Linux-alapú Hadoop SSH használata](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [A Windows HDInsight Linux-alapú Hadoop SSH használata](hdinsight-hadoop-linux-use-ssh-windows.md)
    
3. Miután létrejött, a következő segítségével bontsa ki a .zip fájlt:

        unzip FILENAME.zip
    
    Ez egy .csv fájlt, amely nagyjából 60MB méretű kiveszi.
    
4. Segítségével a következő új címtár a WASB (a elosztott adattár HDInsight által használt), és másolja a fájlt:

    fájlrendszerhez elosztott - mkdir -p /tutorials/flightdelays/data fájlrendszerhez elosztott-FILENAME.csv/oktatóanyagok/flightdelays/adatok/helyezése
    
##<a name="create-and-run-the-hiveql"></a>Létrehozása és futtatása a HiveQL

Adatok importálása CSV-fájl __késések__nevű táblába struktúra kövesse az alábbi lépéseket.

1. A következő használatával hozhat létre és módosíthat elnevezett __flightdelays.hql__új fájl:

        nano flightdelays.hql
        
    Ez a fájl tartalmát a következő használható:
    
        DROP TABLE delays_raw;
        -- Creates an external table over the csv file
        CREATE EXTERNAL TABLE delays_raw (
            YEAR string,
            FL_DATE string,
            UNIQUE_CARRIER string,
            CARRIER string,
            FL_NUM string,
            ORIGIN_AIRPORT_ID string,
            ORIGIN string,
            ORIGIN_CITY_NAME string,
            ORIGIN_CITY_NAME_TEMP string,
            ORIGIN_STATE_ABR string,
            DEST_AIRPORT_ID string,
            DEST string,
            DEST_CITY_NAME string,
            DEST_CITY_NAME_TEMP string,
            DEST_STATE_ABR string,
            DEP_DELAY_NEW float,
            ARR_DELAY_NEW float,
            CARRIER_DELAY float,
            WEATHER_DELAY float,
            NAS_DELAY float,
            SECURITY_DELAY float,
            LATE_AIRCRAFT_DELAY float)
        -- The following lines describe the format and location of the file
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE
        LOCATION '/tutorials/flightdelays/data';
        
        -- Drop the delays table if it exists
        DROP TABLE delays;
        -- Create the delays table and populate it with data
        -- pulled in from the CSV file (via the external table defined previously)
        CREATE TABLE delays AS
        SELECT YEAR AS year,
            FL_DATE AS flight_date,
            substring(UNIQUE_CARRIER, 2, length(UNIQUE_CARRIER) -1) AS unique_carrier,
            substring(CARRIER, 2, length(CARRIER) -1) AS carrier,
            substring(FL_NUM, 2, length(FL_NUM) -1) AS flight_num,
            ORIGIN_AIRPORT_ID AS origin_airport_id,
            substring(ORIGIN, 2, length(ORIGIN) -1) AS origin_airport_code,
            substring(ORIGIN_CITY_NAME, 2) AS origin_city_name,
            substring(ORIGIN_STATE_ABR, 2, length(ORIGIN_STATE_ABR) -1)  AS origin_state_abr,
            DEST_AIRPORT_ID AS dest_airport_id,
            substring(DEST, 2, length(DEST) -1) AS dest_airport_code,
            substring(DEST_CITY_NAME,2) AS dest_city_name,
            substring(DEST_STATE_ABR, 2, length(DEST_STATE_ABR) -1) AS dest_state_abr,
            DEP_DELAY_NEW AS dep_delay_new,
            ARR_DELAY_NEW AS arr_delay_new,
            CARRIER_DELAY AS carrier_delay,
            WEATHER_DELAY AS weather_delay,
            NAS_DELAY AS nas_delay,
            SECURITY_DELAY AS security_delay,
            LATE_AIRCRAFT_DELAY AS late_aircraft_delay
        FROM delays_raw;
        
2. Használja a __Ctrl + X billentyűkombinációt__, majd az __Y__ mentse a fájlt.

3. A következő használatával indítsa el a struktúra, és futtassa a __flightdelays.hql__ fájlt:

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin -f flightdelays.hql
        
    > [AZURE.NOTE] Ebben a példában `localhost` használják, mivel a központi csomópontot a HDInsight fürt, amely HiveServer2 futtató csatlakozik.

4. A következő paranccsal nyisson meg egy interaktív Beeline munkamenetet:

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin

5. Ha értesítést kap a `jdbc:hive2://localhost:10001/>` kérdés, használja az alábbi adatok beolvasása az importált nézetbeli késési adataival.

        INSERT OVERWRITE DIRECTORY '/tutorials/flightdelays/output'
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        SELECT regexp_replace(origin_city_name, '''', ''),
            avg(weather_delay)
        FROM delays
        WHERE weather_delay IS NOT NULL
        GROUP BY origin_city_name;

    Ezzel időjárási késleltetése, a átlagos késleltetési időt, valamint tapasztalt városok listáját, és mentheti, hogy `/tutorials/flightdelays/output`. Később Sqoop ezen a helyen lévő adatok beolvasása és Azure SQL-adatbázishoz exportálhatja.

6. Való kilépéshez Beeline, írja be a `!quit` a parancssorba.

## <a name="create-a-sql-database"></a>SQL-adatbázis létrehozása

Ha már van egy SQL-adatbázissal, a kiszolgáló nevét kell kapja. Ez a található az [Azure-portálon](https://portal.azure.com) __SQL-adatbázisait__kijelölése, és kattintson a használni kívánt adatbázis nevének szűrés. A kiszolgáló nevét a __kiszolgáló__ oszlop szerepel.

Ha még nem rendelkezik SQL-adatbázishoz, használja az adatokat [SQL-adatbázis oktatóprogram: SQL-adatbázis létrehozása percben](../sql-database/sql-database-get-started.md) hozhat létre egyet. Szüksége lesz a kiszolgáló nevét, használja az adatbázist menteni.

##<a name="create-a-sql-database-table"></a>SQL-adatbázis táblázat létrehozása

> [AZURE.NOTE] Többféleképpen tábla létrehozása az SQL-adatbázishoz való csatlakozáshoz. Az alábbi lépéseket a HDInsight fürt [FreeTDS](http://www.freetds.org/) használata.

1. Csatlakozzon a Linux-alapú HDInsight fürthöz SSH használja, és futtassa az alábbi lépéseket a SSH munkamenetből.

3. A következő paranccsal FreeTDS telepítése:

        sudo apt-get --assume-yes install freetds-dev freetds-bin

4. FreeTDS telepítése után a következő paranccsal az SQL-adatbázis-kiszolgálóhoz való csatlakozáshoz. __Kiszolgálónév__ cserélje ki az SQL-adatbázis-kiszolgáló neve. Cserélje __adminLogin__ és __adminPassword__ a bejelentkezési SQL-adatbázis. __Adatbázisnév__ cserélje ki az adatbázis nevét.

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D <databaseName>

    Fog kapni a kibocsátás, az alábbihoz hasonló:

        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set to sqooptest
        1>

5. A a `1>` kéri, adja meg a következő sort:

        CREATE TABLE [dbo].[delays](
        [origin_city_name] [nvarchar](50) NOT NULL,
        [weather_delay] float,
        CONSTRAINT [PK_delays] PRIMARY KEY CLUSTERED   
        ([origin_city_name] ASC))
        GO

    Ha a `GO` utasítás van megadva, az előző kimutatások ki kell értékelni. Ezzel hoz létre egy új táblát __késések__, névvel ellátott csoportosított index létrehozása (SQL-adatbázis kötelező.)

    A következő segítségével ellenőrizze, hogy a táblázat létrehoztak:

        SELECT * FROM information_schema.tables
        GO

    Meg kell jelennie a kimeneti az alábbihoz hasonló:

        TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
        databaseName       dbo     delays      BASE TABLE

8. Adja meg `exit` , a `1>` kérdés való kilépéshez a tsql segédprogramot.
    
##<a name="export-data-with-sqoop"></a>Sqoop az adatok exportálása

2. A következő paranccsal ellenőrizze, hogy Sqoop láthatja az SQL-adatbázis:

        sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.windows.net:1433 --username <adminLogin> --password <adminPassword>

    Ez térjen vissza az adatbázisok, beleértve az adatbázist, amely a korábban létrehozott késések táblázat listáját.

3. A következő parancsot használja hivesampletable adatainak exportálása a mobiledata tábla:

        sqoop export --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=<databaseName>' --username <adminLogin> --password <adminPassword> --table 'delays' --export-dir 'wasbs:///tutorials/flightdelays/output' --fields-terminated-by '\t' -m 1

    Ez arra utasítja az SQL-adatbázis csatlakoztatása a késések táblázatot tartalmazó adatbázishoz, és az adatok exportálása a wasbs Sqoop: / / / oktatóanyagok/flightdelays/kimeneti (azt tárolási helyére a korábbi, a struktúra lekérdezés eredménye) a késések táblázatra.

4. A parancs befejeződése után csatlakozhat az alábbi TSQL az adatbázist:

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D sqooptest

    Miután létrejött, a következő kimutatások használatával ellenőrizze, hogy az adatok exportálása lett-e a mobiledata táblához:
    
        SELECT * FROM delays
        GO

    Meg kell jelennie a táblázatban lévő adatok listája. Típus `exit` való kilépéshez a tsql segédprogramot.

##<a id="nextsteps"></a>Következő lépések

Most már ismeri fájl feltöltése Azure Blob-tárolóhoz, egy struktúratáblával kitöltéséhez Azure Blob-tárolóhoz adatainak használatával hogyan, hogyan struktúra lekérdezések futtatása és Sqoop használata Fájlrendszerhez adatainak exportálása Azure SQL-adatbázishoz. További tudnivalókért lásd: az alábbi cikkekben:

* [Első lépések – hdinsight szolgáltatáshoz][hdinsight-get-started]
* [HDInsight struktúra használata][hdinsight-use-hive]
* [HDInsight Oozie használata][hdinsight-use-oozie]
* [HDInsight Sqoop használata][hdinsight-use-sqoop]
* [Malac használata hdinsight szolgáltatáshoz][hdinsight-use-pig]
* [Fejleszthet olyan Java MapReduce programok hdinsight szolgáltatáshoz][hdinsight-develop-mapreduce]
* [Adatfolyam-programok HDInsight-Python Hadoop kidolgozása][hdinsight-develop-streaming]



[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/


[rita-website]: http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time
[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[hdinsight-use-oozie]: hdinsight-use-oozie-linux-mac.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop-mac-linux.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-develop-streaming]: hdinsight-hadoop-streaming-python.md
[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md

[hadoop-hiveql]: https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx


 
