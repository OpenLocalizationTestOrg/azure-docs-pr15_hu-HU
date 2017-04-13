<properties
    pageTitle="Az Azure adatok tó méretezhető adatok tudományos: egy végpont – útmutató |} Microsoft Azure"
    description="Hogyan használható Azure adatok tó végezze el az adatok feltárása és bináris besorolás feladatok megjelenítése egy adatkészletet."  
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="bradsev;weig"/>


# <a name="scalable-data-science-in-azure-data-lake-an-end-to-end-walkthrough"></a>Az Azure adatok tó méretezhető adatok tudományos: egy végpont – útmutató

Ez a forgatókönyv megtudhatja, hogy hogyan Azure adatok tó adatok feltárása és a következőt: taxi utazás mintájából bináris besorolás feladatok és fizetési adatkészlet előre, vagy sem ezt a tippet által egy jegy ára kell fizetni. Azt végigvezeti a [Csapat adatok tudományos folyamat](http://aka.ms/datascienceprocess)-végpont, a modell oktatás adatbeszerzési, majd a tesz közzé a modell webszolgáltatás-példányhoz.


### <a name="azure-data-lake-analytics"></a>Azure adatok tó Analytics

A [Microsoft Azure adatok tó](https://azure.microsoft.com/solutions/data-lake/) összes funkcióját megkönnyítik az adatok tudósok szükséges méretét, alakzat és sebességének adatok tárolására és adatfeldolgozási fejlett analitikai és gépi tanulási modellezési magas méretezhetőség a költséghatékony elvégzéséhez tartalmaz.   Fizet Feladatonkénti alapon, csak akkor, ha a ténylegesen feldolgozott adatok. Azure adatok tó Analytics U-SQL nyelvben tartalmazza, méretezhető megadására, C# kifejező power az SQL deklaráció jellegét színkeverés nyelvnek elosztott lekérdezés lehetőséget. Lehetővé teszi, hogy strukturálatlan adatfeldolgozás olvasható séma alkalmazásával, egyéni logika és a felhasználó által definiált függvények (UDF) beszúrása parancsra, és ahhoz, hogy miként nyomtatott szabályozni kell végrehajtani a méretezés hogyan bővítési tartalmazza. Ha többet szeretne tudni a tervezés filozófiai mögött U-SQL nyelvben, tekintse meg [Visual Studio közzé](https://blogs.msdn.microsoft.com/visualstudio/2015/09/28/introducing-u-sql-a-language-that-makes-big-data-processing-easy/).

Adatok tó Analytics egyben Cortana Analytics-rendszerben, és az együttműködik az Azure SQL-adatraktár, a Power BI és az adatok gyári fő része. Ezzel kap egy teljes felhő nagy adatok és a fejlett analitikai platform.

Ez a forgatókönyv a Előfeltételek és információforrások, amelyek a tevékenységeket, amelyek az adatok tudományos folyamat, és hogy miként telepítheti őket alkotó adatok tó Analytics befejezéséhez szükséges leíró első lépése. U – SQL használatával adatfeldolgozás lépéseit ismerteti, és a Python és struktúra használatával megjelenítésével köt, majd az Azure gépi tanulási Studio létre és helyezhetnek üzembe a cserélendő modellek. 


### <a name="u-sql-and-visual-studio"></a>Az SQL-U és a Visual Studio

Ez a forgatókönyv javasolja Visual Studio és feldolgozása az adatkészlet U-SQL nyelvben parancsfájlok módosíthatja. A U-SQL nyelvben parancsfájlok itt leírt, és a megadott egy külön fájlba. A folyamat ingesting, feltárása és az adatok mintavételi tartalmazza. Azt is megtudhatja, hogy miként feladat futtatása olyan parancsfájl alapú U SQL Azure a portálon. Az adatokat egy társított HDInsight fürt épület és Azure gépi tanulási Studio bináris besorolás modell telepítési megkönnyítése struktúra tábla jön létre.  


### <a name="python"></a>Python

Ez a forgatókönyv is egy szakaszt tartalmaz, amely bemutatja, hogyan létre és helyezhetnek üzembe a Python használata Azure gépi tanulási Studio prediktív modell.  Elvégezheti a szükséges Jupyter jegyzetfüzet Python parancsfájlokat az alábbi lépéseket a folyamathoz. A jegyzetfüzet tartalmaz néhány további szolgáltatás mérnöki lépések és az adatmodellek építés például multiclass osztályozás és modellezése mellett az itt ismertetett bináris besorolás modell regressziós kódját. Az egyéb tipp jellemzők alapján tipp mennyiségét előrejelzésére, akkor a regressziós feladatot. 


### <a name="azure-machine-learning"></a>Azure gépi tanulási
Azure gépi tanulási Studio létre és helyezhetnek üzembe a cserélendő modellek használják. Ehhez használja a két megközelítés: először Python parancsfájlok, majd egy HDInsight (Hadoop) fürt struktúra táblázatokkal.


### <a name="scripts"></a>Parancsfájlok

Csak a fő lépéseit az útmutató mutatja. A teljes **U-SQL nyelvben parancsfájl** és a **Jegyzetfüzet Jupyter** letölthető [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough).


## <a name="prerequisites"></a>Előfeltételek

Az alábbi témakörök megkezdése előtt a következőket kell rendelkeznie:

- Egy Azure-előfizetést. Ha a nem már rendelkezik egy, olvassa el az [első Azure ingyenes próbaverziót](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- [Ajánlott] Visual Studio 2013 vagy a Skype 2015. Ha már nincs ilyen verziójú van telepítve, melyet letölthet egy ingyenes közösségi edition [Itt](https://www.visualstudio.com/visual-studio-homepage-vs.aspx). Kattintson a **Közösség 2015 letöltése** gombra a Visual Studio szakaszban. 

>[AZURE.NOTE] Helyett a Visual Studióban akkor is használhatja az Azure-portálon küldhetik Azure adatok tó lekérdezések. Utasítások módszerről is a Visual Studio és a **folyamat adatainak U – SQL**című szakaszt a portálon lesz elérhető. 

- Azure Adatvillámnézet tó regisztrációs képernyője

>[AZURE.NOTE] Kell használni az Azure adatok tó áruházból (ADLS) és Azure adatok tó Analytics (ADLA) az alábbi szolgáltatások vannak az előzetes jóváhagyás beszerzése. Jelentkezzen az első ADLS vagy ADLA létrehozásakor kéri. Sigh be, kattintson a **regisztráció megtekintése**, olvassa el a szerződést, és kattintson az **OK gombra**. Ebben az esetben, tehát a ADLS jelentkezési lap:

 ![2](./media/machine-learning-data-science-process-data-lake-walkthrough/2-ADLA-preview-signup.PNG)


## <a name="prepare-data-science-environment-for-azure-data-lake"></a>Adatok tudományos környezet Azure adatok tó előkészítése
Készítse elő az útmutató adatok tudományos környezet, hozzon létre az alábbi forrásokat:

- Azure tó adattár (ADLS) 
- Azure adatok tó Analytics (ADLA)
- Azure Blob-tároló fiók
- Azure gépi tanulási Studio-fiók
- Azure adatok tó Tools for Visual Studio (ajánlott)

Ez a szakasz útmutatás minden, az alábbi források létrehozásához. Ha úgy dönt, hogy az Azure gépi tanulási helyett Python, használja a struktúra táblák egy a modellben össze is szüksége lesz egy HDInsight (Hadoop) fürthöz hozhatók létre. Az alternatív eljárás a megfelelő részben ismertetett.
<br/>
>AZURE. Megjegyzés: az **Azure tó adattár** hozható létre vagy külön-külön, vagy az **Azure adatok tó Analytics** az alapértelmezett tárolására létrehozásakor. Hivatkozott hozhat létre minden, az alábbi források külön-külön az alábbi utasításokat, de az adatok tó tároló fiók nem szükséges külön-külön létre.
<br/>
### <a name="create-an-azure-data-lake-store"></a>Azure adatok tó tároló létrehozása

Hozzon létre egy ADLS az [Azure-portálon](http://portal.azure.com). A részletekért olvassa [létrehozása egy HDInsight fürthöz tó áruházzal Azure portál használatával](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md). Ügyeljen arra, hogy az **adatforrás** lap, a **Választható beállítási** lap van leírt fürt AAD identitás beállítása. 

 ![3](./media/machine-learning-data-science-process-data-lake-walkthrough/3-create-ADLS.PNG)


### <a name="create-an-azure-data-lake-analytics-account"></a>Azure adatok tó Analytics-fiók létrehozása
ADLA fiók létrehozása az [Azure-portálon](http://portal.azure.com). A részletekért olvassa [oktatóprogram: első lépések az Azure adatok tó Analytics Azure portálon](../data-lake-analytics/data-lake-analytics-get-started-portal.md). 

 ![4](./media/machine-learning-data-science-process-data-lake-walkthrough/4-create-ADLA-new.PNG)


### <a name="create-an-azure-blob-storage-account"></a>Azure Blob-tároló fiók létrehozása
Azure Blob-tároló fiók létrehozása az [Azure-portálon](http://portal.azure.com). Részletekért olvassa el a tároló szakaszt [kapcsolatos Azure tárterület-fiókok](../storage/storage-create-storage-account.md)létrehozása.
    
 ![5](./media/machine-learning-data-science-process-data-lake-walkthrough/5-Create-Azure-Blob.PNG)


### <a name="set-up-an-azure-machine-learning-studio-account"></a>Azure gépi tanulási Studio fiók beállítása
Jelentkezzen be a feljebb/Azure gépi tanulási Studio az [Azure gépi tanulási](https://azure.microsoft.com/services/machine-learning/) lapra. Kattintson az **első** gombra, és válassza a "Ingyenes munkaterület" vagy "Normál munkaterület". Ezt követően fogja tudni kísérletek létrehozása az Azure Machine Learning Studio.  

### <a name="install-azure-data-lake-tools-recommended"></a>Telepítse az Azure tó Adateszközök [ajánlott]
Telepítse az Azure tó Adateszközök a Visual Studio az [Azure adatok tó Tools for Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504)-verziójának.

 ![6](./media/machine-learning-data-science-process-data-lake-walkthrough/6-install-ADL-tools-VS.PNG)

A telepítés befejezésekor sikeresen, nyissa meg a Visual Studio be. Meg kell jelennie a lap tetején a menü adatok tó. Az Azure erőforrások kell jelennek meg a bal oldali ablaktáblában, amikor bejelentkezik az Azure-fiókjába.

 ![7](./media/machine-learning-data-science-process-data-lake-walkthrough/7-install-ADL-tools-VS-done.PNG)


## <a name="the-nyc-taxi-trips-dataset"></a>A következőt: Taxi utakat adatkészlet
Akkor használja az alábbi adatkészlet egy nyilvánosan elérhető adatkészlet – a [következőt: Taxi utakat adatkészlet](http://www.andresmh.com/nyctaxitrips/). A következőt: Taxi utazás adatok körülbelül 20GB-nyi tömörített CSV-fájlok (nem tömörített ~ 48GB) áll, minden út kifizetett felvételek 173 milliónál egyes utakat és a díjakat. Utazás rekordokat tartalmazza a felvétel és Gyűjtőtár helyek és időpontok, anonimizált támadó szoftver (illesztőprogram) licencszámmal, és a medallion (taxi tartozó egyedi azonosító) számát. Az adatok minden utakat bemutatja a 2013-ben és az egyes az a következő két adatkészleteket megadva:

 - A "trip_data" CSV utazás részleteket, például a utasok, felvétel és dropoff pontok, út időtartama és az utazás hossza tartalmazza. Íme néhány példa rekordot:

        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count, trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868

 - A "trip_fare" CSV tartalmazza, a jegy ára kifizetett minden út, például a fizetés típusát, jegy ára összege, emelt díjas és adót, tippek és autópályadíjak, és a végösszeget kifizetett részleteit. Íme néhány példa rekordot:

        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

Bekapcsolódás utazás egyedi billentyűvel\_adatokat, és utazás\_jegy ára tevődik össze a következő három elem látható: medallion, támadó szoftver\_licenc, és a felvétel\_datetime. Egy nyilvános Azure tároló blob nyers CSV-fájlokból elérheti. Ez az illesztés U a(z) [utazás és jegy ára táblákat](#join) szakaszában van.

## <a name="process-data-with-u-sql"></a>Az SQL-U folyamat adatok

Az ebben a részben illusztrált adatkezelési feladatok közé tartozik a ingesting, minőség-ellenőrzés, feltárása és az adatok mintavételi. Akkor jelenik meg csatlakozás azonnali üzenetváltási és jegy ára tábla is. A végleges részben futtatása az Azure portálról U-SQL nyelvben parancsfájl feladatot. Az alábbiakban az egyes alszakasz mutató hivatkozások:

- [Adatok bevitel: a nyilvános blob adatainak olvasása](#ingest)
- [Adatok minőség-ellenőrzés](#quality)
- [Adatok feltárása](#explore)
- [Bekapcsolódás az út- és jegy ára tábla](#join)
- [Adatok mintavételnél](#sample)
- [Az SQL-U feladatok futtatása](#run)

A U-SQL nyelvben parancsfájlok itt leírt, és a megadott egy külön fájlba. A teljes **U – SQL-parancsfájlok** letölthető [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough).

Végrehajtás U SQL, nyissa meg a Visual Studióban, kattintson a **Fájl--> Új projekt-->**, válassza **Az SQL-U projekt**nevét, és mentse azt egy mappát.

![8](./media/machine-learning-data-science-process-data-lake-walkthrough/8-create-USQL-project.PNG)

>[AZURE.NOTE] Ajánlatos az Azure Portal segítségével U-SQL nyelvben helyett a Visual Studio végrehajtása. Nyissa meg azt az Azure adatok tó Analytics erőforrás a portálon, és közvetlenül, mint az alábbi ábrán illusztrált lekérdezéseket küldhet.

![9](./media/machine-learning-data-science-process-data-lake-walkthrough/9-portal-submit-job.PNG)

### <a name="ingest"></a>Adatok bevitel: Olvassa el a nyilvános blob adatainak

Az Azure blob-az adatok helyét hivatkozott **wasb://container_name@blob_storage_account_name.blob.core.windows.net/blob_name** és **Extractors.Csv()**használatával is kibontása. Mappa helyett a saját tároló és tároló fiók nevét, az alábbi parancsfájlok container_name@blob_storage_account_name wasb webcímét. A fájl nevét is ugyanazt a formátumot, hogy a rendelkezésére **út\_data_ {\*\}.csv** az összes 12 utazás fájlok olvasható. 

    ///Read in Trip data
    @trip0 =
        EXTRACT 
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        passenger_count string,
        trip_time_in_secs string,
        trip_distance string,
        pickup_longitude string,
        pickup_latitude string,
        dropoff_longitude string,
        dropoff_latitude string
    // This is reading 12 trip data from blob
    FROM "wasb://container_name@blob_storage_account_name.blob.core.windows.net/nyctaxitrip/trip_data_{*}.csv"
    USING Extractors.Csv();

Mivel az első sor fejlécek, szükség az Élőfej eltávolítása és oszloptípusok átalakítása megfelelő is. Azt is, vagy a Mentés az Azure tó adattárolás **swebhdfs://data_lake_storage_name.azuredatalakestorage.net/folder_name/file_name**_ használatával vagy Azure Blob-tárolóhoz feldolgozott adatok fiók használatával **wasb://container_name@blob_storage_account_name.blob.core.windows.net/blob_name**. 

    // change data types
    @trip =
        SELECT 
        medallion,
        hack_license,
        vendor_id,
        rate_code,
        store_and_fwd_flag,
        DateTime.Parse(pickup_datetime) AS pickup_datetime,
        DateTime.Parse(dropoff_datetime) AS dropoff_datetime,
        Int32.Parse(passenger_count) AS passenger_count,
        Double.Parse(trip_time_in_secs) AS trip_time_in_secs,
        Double.Parse(trip_distance) AS trip_distance,
        (pickup_longitude==string.Empty ? 0: float.Parse(pickup_longitude)) AS pickup_longitude,
        (pickup_latitude==string.Empty ? 0: float.Parse(pickup_latitude)) AS pickup_latitude,
        (dropoff_longitude==string.Empty ? 0: float.Parse(dropoff_longitude)) AS dropoff_longitude,
        (dropoff_latitude==string.Empty ? 0: float.Parse(dropoff_latitude)) AS dropoff_latitude
    FROM @trip0
    WHERE medallion != "medallion";

    ////output data to ADL
    OUTPUT @trip   
    TO "swebhdfs://data_lake_storage_name.azuredatalakestore.net/nyctaxi_folder/demo_trip.csv"
    USING Outputters.Csv(); 

    ////Output data to blob
    OUTPUT @trip   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_trip.csv"
    USING Outputters.Csv();  

Hasonlóképpen a jegy ára adathalmaz erről azt. Kattintson a jobb gombbal az Azure tó adattár, lehetősége van arra, hogy tekintse meg az adatok **Azure portál--> adatok Explorer** vagy a **Fájlkezelőben** Visual Studio belül. 

 ![10](./media/machine-learning-data-science-process-data-lake-walkthrough/10-data-in-ADL-VS.PNG)

 ![11](./media/machine-learning-data-science-process-data-lake-walkthrough/11-data-in-ADL.PNG)


### <a name="quality"></a>Adatok minőség-ellenőrzés

Utazás és jegy ára tábla a olvasottak, miután adatok minőség-ellenőrzés az alábbi módon történik. Az eredményül kapott CSV-fájlok Azure Blob-tárolóhoz vagy Azure tó adattár kimeneti lehet. 

Keresse meg a medallions medallions száma és egyedi:

    ///check the number of medallions and unique number of medallions
    @trip2 =
        SELECT
        medallion,
        vendor_id,
        pickup_datetime.Month AS pickup_month
        FROM @trip;
    
    @ex_1 =
        SELECT
        pickup_month, 
        COUNT(medallion) AS cnt_medallion,
        COUNT(DISTINCT(medallion)) AS unique_medallion
        FROM @trip2
        GROUP BY pickup_month;
        OUTPUT @ex_1   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_1.csv"
    USING Outputters.Csv(); 

Keresse meg azokat, amelyek egy 100-nál több utakat medallions:

    ///find those medallions that had more than 100 trips
    @ex_2 =
        SELECT medallion,
               COUNT(medallion) AS cnt_medallion
        FROM @trip2
        //where pickup_datetime >= "2013-01-01t00:00:00.0000000" and pickup_datetime <= "2013-04-01t00:00:00.0000000"
        GROUP BY medallion
        HAVING COUNT(medallion) > 100;
        OUTPUT @ex_2   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_2.csv"
    USING Outputters.Csv(); 

Keresse meg a pickup_longitude értelmez érvénytelen rekordjait:

    ///find those invalid records in terms of pickup_longitude
    @ex_3 =
        SELECT COUNT(medallion) AS cnt_invalid_pickup_longitude
        FROM @trip
        WHERE
        pickup_longitude <- 90 OR pickup_longitude > 90;
        OUTPUT @ex_3   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_3.csv"
    USING Outputters.Csv(); 

Hiányzó értékek megkeresése az egyes változók:

    //check missing values
    @res =
        SELECT *,
               (medallion == null? 1 : 0) AS missing_medallion
        FROM @trip;
    
    @trip_summary6 =
        SELECT 
            vendor_id,
        SUM(missing_medallion) AS medallion_empty, 
        COUNT(medallion) AS medallion_total,
        COUNT(DISTINCT(medallion)) AS medallion_total_unique  
        FROM @res
        GROUP BY vendor_id;
    OUTPUT @trip_summary6
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_16.csv"
    USING Outputters.Csv();



### <a name="explore"></a>Adatok feltárása

Bizonyos adatok feltárása az adatok megértéséhez megszerezni azt teheti meg.

Az eloszlás Formabontó, és nem Formabontó utak megkeresése:

    ///tipped vs. not tipped distribution
    @tip_or_not =
        SELECT *,
               (tip_amount > 0 ? 1: 0) AS tipped
        FROM @fare;
    
    @ex_4 =
        SELECT tipped,
               COUNT(*) AS tip_freq
        FROM @tip_or_not
        GROUP BY tipped;
        OUTPUT @ex_4   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_4.csv"
    USING Outputters.Csv(); 

Tipp: összeg lezárási értékű eloszlását keresése: 0,5,10 és 20 dollárban.

    //tip class/range distribution
    @tip_class =
        SELECT *,
               (tip_amount >20? 4: (tip_amount >10? 3:(tip_amount >5 ? 2:(tip_amount > 0 ? 1: 0)))) AS tip_class
        FROM @fare;
    @ex_5 =
        SELECT tip_class,
               COUNT(*) AS tip_freq
        FROM @tip_class
        GROUP BY tip_class;
        OUTPUT @ex_5   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_5.csv"
    USING Outputters.Csv(); 

Egyszerű statisztika utazás távolság megkeresése:

    // find basic statistics for trip_distance
    @trip_summary4 =
        SELECT 
            vendor_id,
            COUNT(*) AS cnt_row,
            MIN(trip_distance) AS min_trip_distance,
            MAX(trip_distance) AS max_trip_distance,
            AVG(trip_distance) AS avg_trip_distance 
        FROM @trip
        GROUP BY vendor_id;
    OUTPUT @trip_summary4
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_14.csv"
    USING Outputters.Csv();

Keresse meg a percentilisek út távolság:

    // find percentiles of trip_distance
    @trip_summary3 =
        SELECT DISTINCT vendor_id AS vendor,
                        PERCENTILE_DISC(0.25) WITHIN GROUP(ORDER BY trip_distance) OVER(PARTITION BY vendor_id) AS median_trip_distance_disc,
                        PERCENTILE_DISC(0.5) WITHIN GROUP(ORDER BY trip_distance) OVER(PARTITION BY vendor_id) AS median_trip_distance_disc,
                        PERCENTILE_DISC(0.75) WITHIN GROUP(ORDER BY trip_distance) OVER(PARTITION BY vendor_id) AS median_trip_distance_disc
        FROM @trip;
       // group by vendor_id;
    OUTPUT @trip_summary3
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_13.csv"
    USING Outputters.Csv(); 


### <a name="join"></a>Bekapcsolódás az út- és jegy ára tábla

Utazás és jegy ára tábla medallion, hack_license és pickup_time illeszthető.

    //join trip and fare table

    @model_data_full =
    SELECT t.*, 
    f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount,  f.total_amount, f.tip_amount,
    (f.tip_amount > 0 ? 1: 0) AS tipped,
    (f.tip_amount >20? 4: (f.tip_amount >10? 3:(f.tip_amount >5 ? 2:(f.tip_amount > 0 ? 1: 0)))) AS tip_class
    FROM @trip AS t JOIN  @fare AS f
    ON   (t.medallion == f.medallion AND t.hack_license == f.hack_license AND t.pickup_datetime == f.pickup_datetime)
    WHERE   (pickup_longitude != 0 AND dropoff_longitude != 0 );

    //// output to blob
    OUTPUT @model_data_full   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_7_full_data.csv"
    USING Outputters.Csv(); 

    ////output data to ADL
    OUTPUT @model_data_full   
    TO "swebhdfs://data_lake_storage_name.azuredatalakestore.net/nyctaxi_folder/demo_ex_7_full_data.csv"
    USING Outputters.Csv(); 


Szintenként személy száma a rekordok, az átlag tipp: összeg, a tipp összeg varianciája, a százalékos aránya Formabontó utakat számának kiszámítása.

    // contigency table
    @trip_summary8 =
        SELECT passenger_count,
               COUNT(*) AS cnt,
               AVG(tip_amount) AS avg_tip_amount,
               VAR(tip_amount) AS var_tip_amount,
               SUM(tipped) AS cnt_tipped,
               (float)SUM(tipped)/COUNT(*) AS pct_tipped
        FROM @model_data_full
        GROUP BY passenger_count;
        OUTPUT @trip_summary8
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_17.csv"
    USING Outputters.Csv();


### <a name="sample"></a>Adatok mintavételnél

Először azt véletlenszerűen válassza az adatok 0,1 % illesztett táblákban:

    //random select 1/1000 data for modeling purpose
    @addrownumberres_randomsample =
    SELECT *,
            ROW_NUMBER() OVER() AS rownum
    FROM @model_data_full;
    
    @model_data_random_sample_1_1000 =
    SELECT *
    FROM @addrownumberres_randomsample
    WHERE rownum % 1000 == 0;
    
    OUTPUT @model_data_random_sample_1_1000   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_7_random_1_1000.csv"
    USING Outputters.Csv(); 

Azt végezze el rétegzett mintavételnél bináris változó tip_class szerint:

    //stratified random select 1/1000 data for modeling purpose
    @addrownumberres_stratifiedsample =
    SELECT *,
            ROW_NUMBER() OVER(PARTITION BY tip_class) AS rownum
    FROM @model_data_full;
    
    @model_data_stratified_sample_1_1000 =
    SELECT *
    FROM @addrownumberres_stratifiedsample
    WHERE rownum % 1000 == 0;
    //// output to blob
    OUTPUT @model_data_stratified_sample_1_1000   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_9_stratified_1_1000.csv"
    USING Outputters.Csv(); 
    ////output data to ADL
    OUTPUT @model_data_stratified_sample_1_1000   
    TO "swebhdfs://data_lake_storage_name.azuredatalakestore.net/nyctaxi_folder/demo_ex_9_stratified_1_1000.csv"
    USING Outputters.Csv(); 


### <a name="run"></a>Az SQL-U feladatok futtatása

Amikor elkészült, az SQL-U parancsfájlok szerkesztése, elküldheti őket a kiszolgálóra, a Azure adatok tó Analytics-fiókjával. Kattintson az **Adatok tó**, **Feladat elküldése**, jelölje ki a **Analytics-fiókot**, válassza a **párhuzamos**és kattintson a **Küldés** gombra.  

 ![12](./media/machine-learning-data-science-process-data-lake-walkthrough/12-submit-USQL.PNG)

Ha a feladat sikeresen teljesül, a feladat állapotának megjelenik a Visual Studióban figyelemmel kísérésére. A feladat befejezésekor is ismétlődő lejátszásra van állítva a feladat-végrehajtás folyamat, és meg, hogy a feladat hatékonyságának javítása a szűk lépéseket. A U-SQL nyelvben feladat állapotának ellenőrzése Azure-portálon is megnyitásához.

 ![13-mal](./media/machine-learning-data-science-process-data-lake-walkthrough/13-USQL-running-v2.PNG)


 ![14](./media/machine-learning-data-science-process-data-lake-walkthrough/14-USQL-jobs-portal.PNG)


Most már érdemes a kimeneti fájlok Azure Blob-tárolóhoz vagy a Azure-portálon. A következő lépés a modellezési rétegzett mintaadatokat azt fogja használni.

 ![15](./media/machine-learning-data-science-process-data-lake-walkthrough/15-U-SQL-output-csv.PNG)

 ![16](./media/machine-learning-data-science-process-data-lake-walkthrough/16-U-SQL-output-csv-portal.PNG)


## <a name="build-and-deploy-models-in-azure-machine-learning"></a>Építse fel és telepítse az Azure gépi tanulási levő

Azt mutatja be a két lehetőség érhető el az, hogy adatokat olvashat be Azure gépi tanulási létrehozásához és 

- Az első lehetőséget használja a mintában szereplő adatokat (a fenti **adatok mintavételnél** lépés) az Azure Blob írt, és építse fel és telepítse az Azure gépi tanulási modellek Python használatával. 
- A második lehetőség az, a lekérdezést az Azure adatok tó közvetlenül a struktúra lekérdezés használatával. Ehhez a lehetőséghez, hogy hozzon létre egy új HDInsight fürt, vagy használja a meglévő HDInsight fürthöz hol a struktúra táblák mutasson az Azure tó adattárolás NY Taxi adatait.  Mindkét beállításokkal az alábbi témákat ölelik fel. 

## <a name="option-1-use-python-to-build-and-deploy-machine-learning-models"></a>1 beállítást: Használata Python létre és helyezhetnek üzembe a gép tanulási modellek

Létre, és helyezhetnek üzembe a gépi tanulási modellek Python használatával, Jupyter jegyzetfüzet létrehozása a helyi számítógépen, vagy az Azure gépi tanulási Studio eszközben. A szolgáltatott [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough) Jupyter jegyzetfüzet feltárása, ábrázolása adatok, szolgáltatás műszaki, modellezési és üzembe teljes kódot tartalmazza. Ez a cikk bemutatjuk, csak a modellezési és a telepítés. 

### <a name="import-python-libraries"></a>Importálás Python tárak

A minta futtatásához Jupyter jegyzetfüzet vagy a Python parancsfájl-fájlt, az alábbi Python csomagok van szükség. Ha a jegyzetfüzet AzureML szolgáltatást használ, ezekről a csomagokról előre telepítve volt.

    import pandas as pd
    from pandas import Series, DataFrame
    import numpy as np
    import matplotlib.pyplot as plt
    from time import time
    import pyodbc
    import os
    from azure.storage.blob import BlobService
    import tables
    import time
    import zipfile
    import random
    import sklearn
    from sklearn.linear_model import LogisticRegression
    from sklearn.cross_validation import train_test_split
    from sklearn import metrics
    from __future__ import division
    from sklearn import linear_model
    from azureml import services


### <a name="read-in-the-data-from-blob"></a>A blob adatainak olvasása

- Kapcsolati karakterlánc   

        CONTAINERNAME = 'test1'
        STORAGEACCOUNTNAME = 'XXXXXXXXX'
        STORAGEACCOUNTKEY = 'YYYYYYYYYYYYYYYYYYYYYYYYYYYY'
        BLOBNAME = 'demo_ex_9_stratified_1_1000_copy.csv'
        blob_service = BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
    
- A szövegként olvasása

        t1 = time.time()
        data = blob_service.get_blob_to_text(CONTAINERNAME,BLOBNAME).split("\n")
        t2 = time.time()
        print(("It takes %s seconds to read in "+BLOBNAME) % (t2 - t1))

 ![17](./media/machine-learning-data-science-process-data-lake-walkthrough/17-python_readin_csv.PNG)    
 
- Oszlopnevek hozzáadása és külön oszlopokban

        colnames = ['medallion','hack_license','vendor_id','rate_code','store_and_fwd_flag','pickup_datetime','dropoff_datetime',
        'passenger_count','trip_time_in_secs','trip_distance','pickup_longitude','pickup_latitude','dropoff_longitude','dropoff_latitude',
        'payment_type', 'fare_amount', 'surcharge', 'mta_tax', 'tolls_amount',  'total_amount', 'tip_amount', 'tipped', 'tip_class', 'rownum']
        df1 = pd.DataFrame([sub.split(",") for sub in data], columns = colnames)
    


- Numerikus oszlopok módosítása

        cols_2_float = ['trip_time_in_secs','pickup_longitude','pickup_latitude','dropoff_longitude','dropoff_latitude',
        'fare_amount', 'surcharge','mta_tax','tolls_amount','total_amount','tip_amount', 'passenger_count','trip_distance'
        ,'tipped','tip_class','rownum']
        for col in cols_2_float:
            df1[col] = df1[col].astype(float)

### <a name="build-machine-learning-models"></a>Gépi tanulási modellek összeállítása

Itt azt hozhat létre egy bináris besorolás modell előre, hogy utazás Formabontó-e, vagy sem. A Jupyter jegyzetfüzet más két modell található: multiclass osztályozás és a regressziós modellek.

- Első üres változók használható scikit létrehozásához szükséges – ismerje meg, az adatmodellek

        df1_payment_type_dummy = pd.get_dummies(df1['payment_type'], prefix='payment_type_dummy')
        df1_vendor_id_dummy = pd.get_dummies(df1['vendor_id'], prefix='vendor_id_dummy')

- Adatok kerete modellezési létrehozása

        cols_to_keep = ['tipped', 'trip_distance', 'passenger_count']
        data = df1[cols_to_keep].join([df1_payment_type_dummy,df1_vendor_id_dummy])
        
        X = data.iloc[:,1:]
        Y = data.tipped

- Tanfolyamok, és a 60-40 osztott vizsgálata

        X_train, X_test, Y_train, Y_test = train_test_split(X, Y, test_size=0.4, random_state=0)

- Logisztikus regresszió oktatás beállítása

        model = LogisticRegression()
        logit_fit = model.fit(X_train, Y_train)
        print ('Coefficients: \n', logit_fit.coef_)
        Y_train_pred = logit_fit.predict(X_train)

       ![c1](./media/machine-learning-data-science-process-data-lake-walkthrough/c1-py-logit-coefficient.PNG)

- Tesztelés adatkészlet pontszám

        Y_test_pred = logit_fit.predict(X_test)

- Kiértékelés mértékek kiszámítása

        fpr_train, tpr_train, thresholds_train = metrics.roc_curve(Y_train, Y_train_pred)
        print fpr_train, tpr_train, thresholds_train
        
        fpr_test, tpr_test, thresholds_test = metrics.roc_curve(Y_test, Y_test_pred) 
        print fpr_test, tpr_test, thresholds_test
        
        #AUC
        print metrics.auc(fpr_train,tpr_train)
        print metrics.auc(fpr_test,tpr_test)
        
        #Confusion Matrix
        print metrics.confusion_matrix(Y_train,Y_train_pred)
        print metrics.confusion_matrix(Y_test,Y_test_pred)

       ![c2](./media/machine-learning-data-science-process-data-lake-walkthrough/c2-py-logit-evaluation.PNG)


 
### <a name="build-web-service-api-and-consume-it-in-python"></a>Webes szolgáltatáshoz tartozó API összeállítása és felhasználja azt a Python

A gépi tanulási a modellt, után beépített lett üzemeltető szeretnénk. A bináris logisztikus modell az alábbi példában használjuk. Győződjön meg arról, hogy a scikit – ismerje meg, a helyi számítógép zónában verziója 0.15.1. Nem kell aggódnia amiatt, hogy ez Azure Machine Learning studio szolgáltatás használatakor.

- Azure Machine Learning studio beállításokból keresse meg a munkaterület hitelesítő adatait. Az Azure gépi tanulási Studio, kattintson a **Beállítások** --> **neve** --> **Engedélyezési tokenek**. 

    ![C3](./media/machine-learning-data-science-process-data-lake-walkthrough/c3-workspace-id.PNG)


        workspaceid = 'xxxxxxxxxxxxxxxxxxxxxxxxxxx'
        auth_token = 'xxxxxxxxxxxxxxxxxxxxxxxxxxx'

- Webszolgáltatás létrehozása

        @services.publish(workspaceid, auth_token) 
        @services.types(trip_distance = float, passenger_count = float, payment_type_dummy_CRD = float, payment_type_dummy_CSH=float, payment_type_dummy_DIS = float, payment_type_dummy_NOC = float, payment_type_dummy_UNK = float, vendor_id_dummy_CMT = float, vendor_id_dummy_VTS = float)
        @services.returns(int) #0, or 1
        def predictNYCTAXI(trip_distance, passenger_count, payment_type_dummy_CRD, payment_type_dummy_CSH,payment_type_dummy_DIS, payment_type_dummy_NOC, payment_type_dummy_UNK, vendor_id_dummy_CMT, vendor_id_dummy_VTS ):
            inputArray = [trip_distance, passenger_count, payment_type_dummy_CRD, payment_type_dummy_CSH, payment_type_dummy_DIS, payment_type_dummy_NOC, payment_type_dummy_UNK, vendor_id_dummy_CMT, vendor_id_dummy_VTS]
            return logit_fit.predict(inputArray)

- Webes szolgáltatás hitelesítő adatok beszerzése

        url = predictNYCTAXI.service.url
        api_key =  predictNYCTAXI.service.api_key
        
        print url
        print api_key

        @services.service(url, api_key)
        @services.types(trip_distance = float, passenger_count = float, payment_type_dummy_CRD = float, payment_type_dummy_CSH=float,payment_type_dummy_DIS = float, payment_type_dummy_NOC = float, payment_type_dummy_UNK = float, vendor_id_dummy_CMT = float, vendor_id_dummy_VTS = float)
        @services.returns(float)
        def NYCTAXIPredictor(trip_distance, passenger_count, payment_type_dummy_CRD, payment_type_dummy_CSH,payment_type_dummy_DIS, payment_type_dummy_NOC, payment_type_dummy_UNK, vendor_id_dummy_CMT, vendor_id_dummy_VTS ):
            pass

- Hívja fel a webes API-szolgáltatás. Meg kell várnia az 5-10 másodperc után az előző lépésben.

        NYCTAXIPredictor(1,2,1,0,0,0,0,0,1)

       ![c4](./media/machine-learning-data-science-process-data-lake-walkthrough/c4-call-API.PNG)


## <a name="option-2-create-and-deploy-models-directly-in-azure-machine-learning"></a>2 lehetőség: Létrehozása, és közvetlenül az Azure gépi tanulási levő telepítése

Azure gépi tanulási Studio tudja olvasni az adatokat közvetlenül az Azure tó adattár majd használhatók és készíthetnek és helyezhetnek üzembe a modellek. Ezt a módszert használja egy struktúratáblával, amely az Azure tó Store áruházban hivatkozásra mutat. Ehhez az, hogy egy külön Azure hdinsight szolgáltatáshoz fürt kell építenie, a struktúratáblával visszajelzéseket. Az alábbi szakaszok bemutatják, hogyan ehhez. 

### <a name="create-an-hdinsight-linux-cluster"></a>Hozzon létre egy HDInsight Linux fürthöz

Hozzon létre egy HDInsight fürthöz (Linux) az [Azure-portálon](http://portal.azure.com). További információ című **létrehozása egy HDInsight fürthöz Azure tó adattár hozzáférést** a [egy HDInsight fürthöz tó áruházzal létrehozása az Azure portál használatával](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).

 ![18](./media/machine-learning-data-science-process-data-lake-walkthrough/18-create_HDI_cluster.PNG)

### <a name="create-hive-table-in-hdinsight"></a>HDInsight struktúra táblázat létrehozása

Most már hozzunk létre struktúra táblákat az Azure gépi tanulási Studio a HDInsight fürt az előző lépésben Azure adatok tó tárban tárolt adatok felhasználásával. Nyissa meg az imént létrehozott HDInsight fürt. Kattintson a **Beállítások** --> **Tulajdonságok** --> **Fürt AAD identitás** --> **ADLS Access**, ellenőrizze, hogy a tó adattár Azure-fiók hozzáadása a listában, a további, írja be és hajtsa végre a a tartalomvédelmi. 

 ![19](./media/machine-learning-data-science-process-data-lake-walkthrough/19-HDI-cluster-add-ADLS.PNG)


Kattintson a **Beállítások** gomb melletti az **Irányítópult** elemre, és egy ablakban jelenik meg. Kattintson a **Nézet struktúra** a felső sarkában a lapot, és megjelenik a **Lekérdezésszerkesztőben**.

 ![20](./media/machine-learning-data-science-process-data-lake-walkthrough/20-HDI-dashboard.PNG)

 ![21](./media/machine-learning-data-science-process-data-lake-walkthrough/21-Hive-Query-Editor-v2.PNG)


Illessze be az alábbi struktúra parancsfájlok hozzon létre egy táblázatot. Adatforrás helye Azure tó adattár hivatkozás ilyen: **adl://data_lake_store_name.azuredatalakestore.net:443/mappanév/fájlnév**.

    CREATE EXTERNAL TABLE nyc_stratified_sample
    (
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        passenger_count string,
        trip_time_in_secs string,
        trip_distance string,
        pickup_longitude string,
        pickup_latitude string,
        dropoff_longitude string,
        dropoff_latitude string,
      payment_type string,
      fare_amount string,
      surcharge string,
      mta_tax string,
      tolls_amount string,
      total_amount string,
      tip_amount string,
      tipped string,
      tip_class string,
      rownum string
      )
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\n'
    LOCATION 'adl://data_lake_storage_name.azuredatalakestore.net:443/nyctaxi_folder/demo_ex_9_stratified_1_1000_copy.csv';


A lekérdezés befejeződésekor jelennek meg az eredmények jelennek meg:

 ![22-es hibakód](./media/machine-learning-data-science-process-data-lake-walkthrough/22-Hive-Query-results.PNG)



### <a name="build-and-deploy-models-in-azure-machine-learning-studio"></a>Építse fel és telepítse az Azure gépi tanulási Studio levő

Azt létre és helyezhetnek üzembe a modell, amely = esetben előrejelzése, vagy sem a tipp az Azure gépi tanulási kifizetett készen állnak. Ez a bináris minősítés használható készen áll a rétegzett mintaadatokat (vagy ne tipp) probléma. A cserélendő modellek multiclass besorolás (tip_class) és a regressziós (tip_amount) segítségével is kell beépített és Azure gépi tanulási Studio rendszerbe, de az alábbi csak bemutatjuk az esetet, használja a bináris besorolás modell kezelése.

1. A adatok lekérése az Azure Machine Learning modulról az **Adatok importálása** , a **bevitt adatok és a kimeneti** csoport érhető el. További tudnivalókért lásd: az [adatok importálása modul](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) hivatkozás lapon.
2. Válassza ki a **Struktúra lekérdezést** az **adatforrás** a **Tulajdonságok** panelen.
3. Az **adatbázis-lekérdezés struktúra** szerkesztő illessze be a következő struktúra parancsfájl

        select * from nyc_stratified_sample;

4. Adja meg a hdinsight szolgáltatáshoz URI fürt (Ez található az Azure-portálon), Hadoop hitelesítő adatait, kimeneti adatai, és az Azure tároló fiók nevét, a billentyű és a tároló nevét.

 ![23](./media/machine-learning-data-science-process-data-lake-walkthrough/23-reader-module-v3.PNG)  

Egy bináris besorolás kísérlet adatok olvasásakor struktúratáblával példa az alábbi ábrán.

 ![24](./media/machine-learning-data-science-process-data-lake-walkthrough/24-AML-exp.PNG)

A kísérlet létrehozását követően kattintson a **Web szolgáltatás beállítása** --> **Prediktív webszolgáltatás**

 ![25](./media/machine-learning-data-science-process-data-lake-walkthrough/25-AML-exp-deploy.PNG)

Automatikusan létrehozott futtatása pontozási kísérlet, amikor befejezte, kattintson a **Webes szolgáltatás telepítése**

 ![26](./media/machine-learning-data-science-process-data-lake-walkthrough/26-AML-exp-deploy-web.PNG)

A webes szolgáltatás irányítópult hamarosan fog megjelenni:

 ![27](./media/machine-learning-data-science-process-data-lake-walkthrough/27-AML-web-api.PNG)


## <a name="summary"></a>Összefoglalás

Ez a forgatókönyv kitöltésével egy adatok tudományos környezet az Azure adatok tó méretezhető végpont megoldások fejlesztésére hozott létre. Ez a környezet véve a az adatok tudományos folyamatán, a modell oktatás –, majd a webes szolgáltatásként a modell példányhoz adatbeszerzési kanonikus 5.7.1-es nagy nyilvános adatkészlet elemzéséhez használták. U-SQL nyelvben használt folyamat, és az adatok minta. Python és struktúra használták Azure gépi tanulási Studio létre és helyezhetnek üzembe a cserélendő modellek.

## <a name="whats-next"></a>Mi az következő?

A tanulási javaslat az a [Csapat adatok tudományos folyamat (TDSP)](http://aka.ms/datascienceprocess) a fejlett analitikai folyamat egyes lépéseiben ismertető témakörök tartalmaz. Létezik a [csapat adatok tudományos folyamat forgatókönyvek](data-science-process-walkthroughs.md) lapon részletezve forgatókönyvek, erőforrások és a szolgáltatások használatához a különböző prediktív analytics forgatókönyvekben dinamikus sorozata:

- [A csoportwebhely adatok tudományos folyamat működését: SQL adatraktár használatával](machine-learning-data-science-process-sqldw-walkthrough.md)
- [A csoportwebhely adatok tudományos folyamat működését: HDInsight Hadoop fürt használatával](machine-learning-data-science-process-hive-walkthrough.md)
- [A csoportwebhely tudományos-folyamat: az SQL Server használatával](machine-learning-data-science-process-sql-walkthrough.md)
- [A külső használata Azure hdinsight szolgáltatáshoz adatok tudományos folyamat áttekintése](machine-learning-data-science-spark-overview.md)

