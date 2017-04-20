<properties
    pageTitle="Az Team tudományos folyamat a művelet: SQL Server használata |}] A Microsoft Azure"
    description="Speciális Analytics folyamat és a művelet technológia"  
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
    ms.author="fashah;bradsev"/>


# <a name="the-team-data-science-process-in-action-using-sql-server"></a>Az Team tudományos folyamat a művelet: SQL Server használata

Oktatóanyag, akkor útmutató létrehozása és telepítése az SQL Server és a nyilvánosan elérhető dataset – a [Következőt: Taxi Trips](http://www.andresmh.com/nyctaxitrips/) dataset gép tanulási modell. Az eljárást követi a szokásos adatok tudományos munkafolyamat: ingest, és Fedezze fel az adatokat, elősegíti a tanulást, majd elkészítheti és üzembe helyezheti a modell mérnök szolgáltatásokat.


## <a name="dataset"></a>A Turistautak Dataset leírás Taxi a következőt:

A következőt: Taxi utazás adatokat CSV fájlok (~ 48GB tömörítetlen) körülbelül 20GB, minden út fizetett több mint 173 millió egyedi utakat és a viteldíjak. Minden út bejegyzést tartalmaz a felvételi és a gyűjtőtár helye és ideje, anonimizált betörés (illesztőprogram) licenc száma és medallion (taxi's egyedi azonosító). Az adatok 2013 évben minden útra vonatkozik, és minden hónapban nyújtják be az alábbi két adatkört:

1. "Trip_data" CSV utazás részleteit, például az utasok, felvételi és dropoff pontokat, út időtartama és út hossza számát tartalmazza. Az alábbiakban néhány mintarekord:

        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868

2. A "trip_fare" CSV részleteit, valamint a jegy ára fizetendő fizetéstípus, viteldíj összegét, pótdíj és adók, tippek és autópályadíj, minden út kifizetett teljes összeget tartalmazza. Az alábbiakban néhány mintarekord:

        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

Utazás csatlakozni egyedi kulcs\_adatokat és az út\_viteldíj áll a mezők: medallion betörés\_engedély és -felvétel\_datetime.

## <a name="mltasks"></a>Példák az előrejelzési tevékenységek

Három előrejelzési problémák alapján azt fogalmaz meg a *tip\_összeg*, nevezetesen:

1. Bináris osztályozás:-e egy tipp fizették az útnak, azaz előre egy *Tipp\_összeg* nagyobb mint 0 Ft pozitív példa, amikor egy *Tipp\_összeg* negatív példa is 0 Ft.

2. Multiclass besorolása: előre fizetett az utazás tipp körét. Ezt osszuk el a *tip\_összeg* öt raktárhelyek vagy osztályok:

        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20

3. Regressziós feladat: tipp utazásra fizetett összeg előre.  


## <a name="setup"></a>Speciális analytics beállítás fel az Azure adatok tudományos környezet

Amint láthatja, a [Terv a környezetvédelmi](machine-learning-data-science-plan-your-environment.md) útmutató, több opció a következőt: Taxi Trips dataset Azure dolgozni:

- Azure BLOB adatait, majd az Azure gép tanulási modell használata
- Az adatok betöltése egy SQL Server adatbázishoz, majd Azure gép tanulási modell

Ez az oktatóanyag azt igazolják párhuzamos tömeges importálása az adatokat egy SQL Server adatok feltárása, a szolgáltatás mérnöki vonatkozó mintavételi használata az SQL Server Management Studio, valamint IPython Jegyzetfüzet használatával. [A példaként közölt parancsfájlokat](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) és [IPython jegyzetfüzet](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks) GitHub vannak osztva. Az adatok az Azure BLOB minta IPython jegyzetfüzet is rendelkezésre áll ugyanazon a helyen.

Az Azure adatok tudományos környezet beállítása:

1. [Tároló fiók létrehozása](../storage/storage-create-storage-account.md)

2. [Azure gép tanulás-munkaterület létrehozása](machine-learning-create-workspace.md)

3. [Rendelkezés tudományos adatok virtuális gép](machine-learning-data-science-setup-sql-server-virtual-machine.md), amelyek SQL Server is szolgálnak az IPython jegyzetfüzet-kiszolgáló.

    > [AZURE.NOTE] A példaként közölt parancsfájlokat és IPython jegyzetfüzet automatikusan letöltődik a tudományos adatok virtuális gép a telepítés során. A virtuális gép telepítés utáni parancsfájl lefutása, a minták lesz a virtuális Gépet a Dokumentumok könyvtár:  
    > - Parancsfájlpéldák:`C:\Users\<user_name>\Documents\Data Science Scripts`  
    > - Minta IPython jegyzetfüzet:`C:\Users\<user_name>\Documents\IPython Notebooks\DataScienceSamples`  
    > Ha `<user_name>` van a virtuális Gépet a Windows bejelentkezési név. A **Példaként közölt parancsfájlokat** és a **Minta IPython jegyzetfüzet**minta mappák hivatkozik.


A dataset méret, adatforrás helyének és a kijelölt Azure célkörnyezetben alapján ez a forgatókönyv hasonlít a [eset \#5: a helyi fájlok nagy adathalmaz érintett Azure VM az SQL Server](../machine-learning-data-science-plan-sample-scenarios.md#largelocaltodb).

## <a name="getdata"></a>Az adatok letöltése állami forrásból

A nyilvános helyről a [Következőt: Taxi Trips](http://www.andresmh.com/nyctaxitrips/) dataset beszerzéséhez az adatok átmásolása az új virtuális gép használhat az [Adatok áthelyezése, és az Azure Blob-tároló](machine-learning-data-science-move-azure-blob.md) ismertetett módszerekkel.

AzCopy használata az adatok másolásához:

1. Jelentkezzen be a virtuális gép (VM)

2. Hozzon létre egy új könyvtárat a virtuális gép adatainak lemez (Megjegyzés: ne használja a virtuális gép, mint egy adat lemez részét képező ideiglenes lemez).

3. A parancssori ablakban futtassa a következő Azcopy parancsot, a (2) a létrehozott data mappában < path_to_data_folder > cseréje:

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:https://nyctaxitrips.blob.core.windows.net/data /Dest:<path_to_data_folder> /S

    Ha befejezte a AzCopy, összesen 24 CSV-fájlok ZIP (út 12-\_adatok és az út 12-\_viteldíj) a data mappában kell lennie.

4. Bontsa ki a letöltött fájlokat. Megjegyzés: a a nem tömörített fájlokat tároló mappa. Ezt a mappát a program a továbbiakban: a < elérési út\_,\_adatok\_fájlok\>.

## <a name="dbload"></a>A tömeges importálási adatok SQL Server adatbázisba

Berakodás/átvitel, nagy mennyiségű adat SQL adatbázis-lekérdezések teljesítményét javítani lehet _particionálni táblák és nézetek_használatával. Ebben a részben azt követi a [Párhuzamos tömeges importálás segítségével SQL partíció adattáblák](machine-learning-data-science-parallel-load-sql-partitioned-tables.md) hozzon létre egy új adatbázist, és az adatok betöltése a párhuzamos particionált táblák utasításait.

1. Bejelentkezve a virtuális Gépet, indítsa el az **SQL Server Management Studio**.

2. Csatlakoztassa a Windows-hitelesítés használata.

    ![SSMS csatlakozás][12]

3. Ha még nem változott az SQL Server hitelesítési mód, és létrehozott egy új SQL bejelentkező felhasználó, nyissa meg a parancsfájl neve **módosítsa\_auth.sql** a **Mintaparancsfájlok** mappában. Módosítsa az alapértelmezett felhasználói nevet és jelszót. Kattintson a **! Végre** az eszköztáron a parancsfájl futtatásához.

    ![Parancsfájl végrehajtása][13]

4. Ellenőrizheti és módosíthatja az SQL Server alapértelmezett adatbázis és napló mappáit annak biztosítása érdekében, hogy az újonnan létrehozott adatbázisok lemez adatokat fogja tárolni. Az SQL Server VM kép datawarehousing terhelések optimalizált előre beállított lemezek adat- és naplófájlok. Ha a virtuális Gépet nem tartalmazott adatok lemezre és a virtuális gép telepítés során hozzáadott új virtuális merevlemezek, módosítása az alapértelmezett mappák az alábbiak szerint:

    - Kattintson a jobb gombbal az SQL Server nevét a bal oldali panelen, és kattintson a **Tulajdonságok**parancsra.

        ![SQL-kiszolgáló tulajdonságai][14]

    - **Lap kijelölése** a listából válassza ki az **Adatbázis-beállítások** balra.

    - Ellenőrizze, és/vagy módosítsa az **adatbázis alapértelmezett helye** a választott **Adatok lemezre** helyekre. Ez akkor, ha az alapértelmezett helyre beállításokkal létre új adatbázisokat tároló.

        ![SQL adatbázis alapértelmezett beállítások][15]  

5. Új adatbázis és a táblák particionált tároló írhatósága létrehozásához nyissa meg a minta parancsfájl **létrehozása\_db\_default.sql**. A parancsfájl a **TaxiNYC** és az alapértelmezett adatok helyen 12 írhatósága nevű új adatbázist hoz létre. Minden fájlcsoportban ráfér egy hónappal az út\_adatokat és az út\_adatok díjszabás. Módosítsa az adatbázis neve, szükség esetén. Kattintson a **! Végre** a parancsfájl futtatásához.

6. Ezután hozzon létre egy út két partíciós táblák\_adatokat és az utazás egy másik\_viteldíj. Nyissa meg a minta parancsfájl **létrehozása\_particionált\_table.sql**, amely:

    - Az adatokat havi bontásban felosztása partíció függvény létrehozása.
    - Minden hónapban adatok leképezése más fájlcsoportba partíciós séma létrehozása.
    - Partíció rendszerhez csatlakoztatott két particionált táblák létrehozása: **nyctaxi\_utazás** akkor érvényes, ha az utazás\_adatok és **nyctaxi\_viteldíj** akkor érvényes, ha az utazás\_adatok díjszabás.

    Kattintson a **! Végre** a parancsfájl futtatása és a particionált táblák létrehozása.

7. A **Mintaparancsfájlok** mappában olyan bizonyítására párhuzamos tömeges behozatalának adatok SQL Server táblák meghatározott két minta PowerShell parancsfájl.

    - **bcp\_párhuzamos\_generic.ps1** van egy általános célú parancsfájl párhuzamos tömeges importálási adatok táblába. Ezt a parancsfájlt a bemeneti és a cél változók értékét a a parancsfájlban a Megjegyzés sorok módosítása.
    - **bcp\_párhuzamos\_nyctaxi.ps1** az általános parancsfájl egy előre konfigurált változatát és a használható betölteni a következőt: Taxi Trips adatok mindkét táblában.  

8. Kattintson a jobb gombbal a **bcp\_párhuzamos\_nyctaxi.ps1** parancsfájl nevét, majd kattintson a **Szerkesztés** PowerShell nyithatja meg. Az előre definiált változók áttekintheti és módosíthatja a kijelölt adatbázis neve, a bevitt adatok mappa, a célmappa napló és a elérési útját a minta formátumú fájlok **nyctaxi_trip.xml** és **nyctaxi\_fare.xml** (amennyiben a **Minta Scripts** mappában).

    ![A tömeges importálási adatok][16]

    A hitelesítési mód is jelölhet, alapértelmezett Windows-hitelesítést. Kattintson az eszköztár Futtatás a zöld nyílra. A parancsfájl elindítja 24 tömeges importálási műveletek párhuzamos 12 particionált mindegyikhez. Az adatok importálása folyamatban fenti meg az SQL Server alapértelmezett data mappa megnyitásával is figyelheti.

9. A PowerShell parancsfájl jelentések, a kezdési és befejezési idejét. Az összes tömeges behozatal teljes, ha a befejezési időpontot kell jelenteni. Ellenőrizze, hogy a tömeges behozatal célmappa napló ellenőrzése sikeres volt, azaz nincs hiba a célmappa napló.

10. Az adatbázis készen áll a feltárása, mérnöki szolgáltatás és egyéb műveletek. Mivel a táblák vannak particionálva a következők szerint a **Felvétel\_datetime** mezőben, a lekérdezések, amelyek magukban foglalják **Felvétel\_datetime** feltétel a **WHERE** záradékban fog részesülni a partíció rendszer.

11. Az **SQL Server Management Studio**, Fedezze fel a megadott minta parancsfájl **minta\_queries.sql**. A minta lekérdezések futtatásához jelölje ki a lekérdezési sorok, majd kattintson a **! Végre** az eszköztáron.

12. A következőt: Taxi Trips adatok két különböző táblákban betöltődik. Javíthatjuk az illesztési műveleteket, erősen ajánlott indexelje a táblákat. A mintaparancsfájl **létrehozása\_particionált\_index.sql** particionált indexet hoz összetett összekapcsolási kulcs **medallion betörés\_licenc és a felvétel\_datetime**.

## <a name="dbexplore"></a>Adatok feltárása és az SQL Server szolgáltatás mérnöki

Ebben a részben azt végez adatok feltárása és a szolgáltatás létrehozása közvetlenül az **SQL Server Management Studio** segítségével a korábban létrehozott SQL Server adatbázis SQL-lekérdezések futtatásával. A mintaparancsfájl nevű **minta\_queries.sql** **Minta Scripts** mappában található. A parancsfájl adatbázis nevének módosításához módosíthatja, ha eltér az alapértelmezett: **TaxiNYC**.

Ebben a gyakorlatban a következő történik:

- Csatlakozzon az **SQL Server Management Studio** vagy a Windows-hitelesítés vagy SQL-hitelesítés és az SQL bejelentkezési név és jelszó használatával.
- Fedezze fel az adatok elosztása eltérő időkeretek néhány mezője.
- A hosszúság és szélesség mezők adatait minőségének vizsgálatára.
- Bináris és multiclass besorolás címkék alapján készítése a **tip\_összeg**.
- Szolgáltatások létrehozása és utazás távolságok compute/összehasonlítás céljából.
- Illesszük a két táblát, és vonjuk ki a modellek létrehozására használt véletlenszerű mintát.

Amikor készen Azure gép tanulás folytassa, akkor is vagy:  

1. A kibontása és a mintavételezési és a másolás-beillesztés a lekérdezés [Adatainak importálása] közvetlenül a végső SQL lekérdezés mentése[ import-data] moduljának Azure gépi tanulás, vagy
2. A probléma továbbra is fennáll a mintában szereplő és visszafejtett adatokat azt tervezi, hogy a modell egy új adatbázistábla létrehozásához használja, és az [Adatok importálása] az új táblában használni[ import-data] Azure gép tanulási modul.

Ebben a szakaszban és a mintavételezési végső lekérdezés menti azt. A második módszer az [adatok feltárása és IPython jegyzetfüzet mérnöki szolgáltatás](#ipnb) részben bizonyított.

A sorok és oszlopok a tábla számát egy gyors ellenőrzés feltöltve eszközzel korábbi párhuzamos tömeges importálás,

    -- Report number of rows in table nyctaxi_trip without table scan
    SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('nyctaxi_trip')

    -- Report number of columns in table nyctaxi_trip
    SELECT COUNT(*) FROM information_schema.columns WHERE table_name = 'nyctaxi_trip'

#### <a name="exploration-trip-distribution-by-medallion"></a>Feltárása: Utazás elosztás alapján medallion

Ebben a példában a medallion (taxi számokat) több mint 100 utak egy adott időszakon belül azonosítja. A lekérdezés előnyös a particionált tábla hozzáférést, mivel azt a rendszer partíció van kondicionálni **Felvétel\_datetime**. A teljes adatkészlet lekérdezése is tesz a particionált tábla használata és/vagy a keresési index.

    SELECT medallion, COUNT(*)
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    GROUP BY medallion
    HAVING COUNT(*) > 100

#### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a>Feltárása: Utazás terjesztési medallion és hack_license

    SELECT medallion, hack_license, COUNT(*)
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20130131'
    GROUP BY medallion, hack_license
    HAVING COUNT(*) > 100

#### <a name="data-quality-assessment-verify-records-with-incorrect-longitude-andor-latitude"></a>Adatok minőségének értékelését: Ellenőrzése nem megfelelő hosszúság és/vagy szélesség

Ez a példa megvizsgálja, ha a hosszúság és/vagy szélesség mezők bármelyike vagy érvénytelen értéket tartalmaz (radián fokban kell lennie 90-től 90), vagy (0, 0) koordinátáit.

    SELECT COUNT(*) FROM nyctaxi_trip
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(pickup_latitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_latitude AS float) NOT BETWEEN -90 AND 90
    OR    (pickup_longitude = '0' AND pickup_latitude = '0')
    OR    (dropoff_longitude = '0' AND dropoff_latitude = '0'))

#### <a name="exploration-tipped-vs-not-tipped-trips-distribution"></a>Feltárása: Tipped és nem Formabontó Trips terjesztési

Ebben a példában, mintha Formabontó és időszak (vagy ha a teljes évre vonatkozó teljes adathalmazban) egy adott idő alatt nem Formabontó utak számát találja meg. Ezt az eloszlást a bináris felirat terjesztési később használt bináris osztályozás modellezési tükrözi.

    SELECT tipped, COUNT(*) AS tip_freq FROM (
      SELECT CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped, tip_amount
      FROM nyctaxi_fare
      WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tipped

#### <a name="exploration-tip-classrange-distribution"></a>Feltárása: Tipp telepítési osztály/tartomány

Ez a példa tipp tartományok terjesztési egy adott időszakban (vagy ha a teljes évre vonatkozó teljes adathalmazban) számítja ki. Ez a címke osztályok később használandó multiclass osztályozás modellezési megoszlása.

    SELECT tip_class, COUNT(*) AS tip_freq FROM (
        SELECT CASE
            WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tip_class

#### <a name="exploration-compute-and-compare-trip-distance"></a>Feltárása: Számítása és összehasonlítása utazás távolság

Ebben a példában a felvétel és a gyűjtőtár hosszúsági alakítja át, és SQL földrajzi szélesség mutat, segítségével SQL földrajzi pontok különbség utazás távolság kiszámítja és szúrópróbaszerűen kiválasztott összehasonlítás eredményét adja vissza. A példa csak lekérdezéssel az adatok minőségi értékelés hatálya alá tartozó korábbi érvényes koordináták az eredményekre.

    SELECT
    pickup_location=geography::STPointFromText('POINT(' + pickup_longitude + ' ' + pickup_latitude + ')', 4326)
    ,dropoff_location=geography::STPointFromText('POINT(' + dropoff_longitude + ' ' + dropoff_latitude + ')', 4326)
    ,trip_distance
    ,computedist=round(geography::STPointFromText('POINT(' + pickup_longitude + ' ' + pickup_latitude + ')', 4326).STDistance(geography::STPointFromText('POINT(' + dropoff_longitude + ' ' + dropoff_latitude + ')', 4326))/1000, 2)
    FROM nyctaxi_trip
    tablesample(0.01 percent)
    WHERE CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND   CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND   pickup_longitude != '0' AND dropoff_longitude != '0'

#### <a name="feature-engineering-in-sql-queries"></a>Az SQL-lekérdezések szolgáltatás mérnöki

Címke létrehozása és a földrajzi átváltási feltárása lekérdezéseket is a számlálási részének eltávolításával címkék/szolgáltatások létrehozására használható. További szolgáltatás mérnöki SQL példák szerepelnek az [adatok feltárása és IPython jegyzetfüzet mérnöki szolgáltatás](#ipnb) szakasz. Sokkal hatékonyabb, ha a szolgáltatás létrehozása lekérdezések futtatása a teljes adatkészletet vagy futtassa az SQL Server-adatbázispéldány közvetlenül az SQL-lekérdezések segítségével egy nagy részét. A lekérdezések **Az SQL Server Management Studio**, IPython jegyzetfüzet vagy bármely fejlesztési eszköz és a környezetre, amely az adatbázishoz hozzáférjen helyben vagy távolról is végrehajtható.

#### <a name="preparing-data-for-model-building"></a>Az adatok előkészítése a modell létrehozása

A következő lekérdezés illesztést a **nyctaxi\_utazás** és **nyctaxi\_viteldíj** táblák, létrehoz egy bináris besorolási címke **Formabontó**, több osztály besorolási címke **tip\_osztály**, és 1 %-át véletlenszerűen kivonatok illesztett teljes adathalmazban. Ez a lekérdezés másolja, majd az [Azure gép tanulás Studio](https://studio.azureml.net) [Adatok importálása] közvetlenül a beillesztett[ import-data] modul közvetlen adatok szempontjából Azure az SQL Server adatbázis példányból. A lekérdezés nem tartalmazza a hibás rekordok (0, 0) koordinátáit.

    SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount,  f.total_amount, f.tip_amount,
        CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped,
        CASE WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM nyctaxi_trip t, nyctaxi_fare f
    TABLESAMPLE (1 percent)
    WHERE t.medallion = f.medallion
    AND   t.hack_license = f.hack_license
    AND   t.pickup_datetime = f.pickup_datetime
    AND   pickup_longitude != '0' AND dropoff_longitude != '0'


## <a name="ipnb"></a>Adatok feltárása és szolgáltatás mérnöki IPython jegyzetfüzet

Ebben a szakaszban azt fogja végrehajtani az adatok feltárása és a Python és az SQL lekérdezésekkel a korábban létrehozott SQL Server adatbázisban a szolgáltatás létrehozása. A **Minta IPython jegyzetfüzet** mappában egy minta IPython notebook nevű **machine-Learning-data-science-process-sql-story.ipynb** biztosítja. A jegyzetfüzet a [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks)is megtalálható.

Ajánlott sorrendje a következő nagy adatok használata esetén:

- Olvassa el az adatok egy kis minta egy memóriában lévő adatok keretbe.
- Bizonyos képi megjelenítések és explorations a mintában szereplő adatokkal végez.
- A mintában szereplő adatokkal szolgáltatás mérnöki kísérletezni.
- Nagyobb adatok feltárása, adatfeldolgozás és mérnöki szolgáltatás a Python segítségével SQL-lekérdezések kiadja a Azure virtuális SQL Server adatbázison.
- Azure gép tanulási modell épület használandó a minta mérete határozza meg.

Ha készen áll a gép tanulás Azure folytassa, akkor előfordulhat, hogy vagy:  

1. A kibontása és a mintavételezési és a másolás-beillesztés a lekérdezés [Adatainak importálása] közvetlenül a végső SQL lekérdezés mentése[ import-data] Azure gép tanulási modul. Ez a módszer az [Épület Azure gép tanulási modellek](#mlmodel) részben bizonyított.    
2. Továbbra is fennáll a mintában szereplő és visszafejtett adatokat az adatbázistábla új épület modell használatát tervezi, majd az [Adatok importálása] az új táblát használja[ import-data] modul.

Az alábbi táblázat néhány adatok feltárása, adatok képi megjelenítés és példák mérnöki szolgáltatás. További példákat talál a minta SQL IPython jegyzetfüzet a **Minta IPython jegyzetfüzet** mappában.

#### <a name="initialize-database-credentials"></a>Adatbázis hitelesítő adatainak inicializálása

A következő változók az adatbázis kapcsolati beállítások inicializálása:

    SERVER_NAME=<server name>
    DATABASE_NAME=<database name>
    USERID=<user name>
    PASSWORD=<password>
    DB_DRIVER = <database server>

#### <a name="create-database-connection"></a>Adatbázis-kapcsolat létrehozása

    CONNECTION_STRING = 'DRIVER={'+DRIVER+'};SERVER='+SERVER_NAME+';DATABASE='+DATABASE_NAME+';UID='+USERID+';PWD='+PASSWORD
    conn = pyodbc.connect(CONNECTION_STRING)

#### <a name="report-number-of-rows-and-columns-in-table-nyctaxitrip"></a>Sorok és oszlopok a tábla nyctaxi_trip jelentés száma

    nrows = pd.read_sql('''
        SELECT SUM(rows) FROM sys.partitions
        WHERE object_id = OBJECT_ID('nyctaxi_trip')
    ''', conn)

    print 'Total number of rows = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''
        SELECT COUNT(*) FROM information_schema.columns
        WHERE table_name = ('nyctaxi_trip')
    ''', conn)

    print 'Total number of columns = %d' % ncols.iloc[0,0]

- Sorok száma = 173179759  
- Oszlopok száma összesen = 14

#### <a name="read-in-a-small-data-sample-from-the-sql-server-database"></a>Olvasás a minta kis adatokat az SQL Server adatbázisból

    t0 = time.time()

    query = '''
        SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax,
            f.tolls_amount, f.total_amount, f.tip_amount
        FROM nyctaxi_trip t, nyctaxi_fare f
        TABLESAMPLE (0.05 PERCENT)
        WHERE t.medallion = f.medallion
        AND   t.hack_license = f.hack_license
        AND   t.pickup_datetime = f.pickup_datetime
    '''

    df1 = pd.read_sql(query, conn)

    t1 = time.time()
    print 'Time to read the sample table is %f seconds' % (t1-t0)

    print 'Number of rows and columns retrieved = (%d, %d)' % (df1.shape[0], df1.shape[1])

Olvassa el a mintaasztal pedig 6.492000 másodperc időt  
Beolvasott sorok és oszlopok száma = (84952, 21)

#### <a name="descriptive-statistics"></a>Leíró statisztika

Most már készen áll a mintában szereplő adatok feltárása. Azt a leíró statisztika megnézzük indítsa el a **út\_távolság** (vagy bármely más) (k) höz:

    df1['trip_distance'].describe()

#### <a name="visualization-box-plot-example"></a>Képi megjelenítés: Mintaterület – példa

A Microsoft vizsgálja meg a képi megjelenítés a quantiles út távolsága a Dobozdiagram

    df1.boxplot(column='trip_distance',return_type='dict')

![#1 ábrázolása][1]

#### <a name="visualization-distribution-plot-example"></a>Képi megjelenítés: Terjesztési ábra példa

    fig = plt.figure()
    ax1 = fig.add_subplot(1,2,1)
    ax2 = fig.add_subplot(1,2,2)
    df1['trip_distance'].plot(ax=ax1,kind='kde', style='b-')
    df1['trip_distance'].hist(ax=ax2, bins=100, color='k')

![#2 ábrázolása][2]

#### <a name="visualization-bar-and-line-plots"></a>Képi megjelenítés: Sáv és vonal parcellák

Ebben a példában azt bin öt raktárhelyekre utazás távolság és a binning eredmények megjelenítéséhez.

    trip_dist_bins = [0, 1, 2, 4, 10, 1000]
    df1['trip_distance']
    trip_dist_bin_id = pd.cut(df1['trip_distance'], trip_dist_bins)
    trip_dist_bin_id

A Microsoft rajzoljuk meg a fenti raktárhely eloszlása a sáv vagy vonal az alábbi ábra

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='bar')

![#3 ábrázolása][3]

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='line')

![#4 ábrázolása][4]

#### <a name="visualization-scatterplot-example"></a>Képi megjelenítések: Például Scatterplot

Azt megjelenítése között xy mintaterület **út\_idő\_a\_mp** és **út\_távolság** van-e bármilyen korrelációs

    plt.scatter(df1['trip_time_in_secs'], df1['trip_distance'])

![#6 ábrázolása][6]

Hasonlóképpen azt közötti kapcsolat ellenőrzése **sebesség\_kód** és **út\_távolság**.

    plt.scatter(df1['passenger_count'], df1['trip_distance'])

![#8 ábrázolása][8]

### <a name="sub-sampling-the-data-in-sql"></a>Az SQL-adatok Alminta vétele

Előkészítése adatok minta [Azure gép tanulás](https://studio.azureml.net)Studio épület, amikor is vagy az **SQL-lekérdezést közvetlenül az adatok importálása modul használata** mellett dönt, illetve továbbra is fennáll a visszafejtett és mintában szereplő adatokat, amelyek felhasználhatók az [Adatok importálása] új táblát[ import-data] nevű egyszerű * *VÁLASSZA a* FROM < a\_új\_tábla\_neve >**.

Ebben a részben azt a mintában szereplő és a visszafejtett adatok tárolására új táblát hoz létre. Modell építése közvetlen SQL lekérdezés például biztosítja az [adatok feltárása és az SQL Server mérnöki szolgáltatás](#dbexplore) szakaszban.

#### <a name="create-a-sample-table-and-populate-with-1-of-the-joined-tables-drop-table-first-if-it-exists"></a>Hozzon létre egy minta táblázat és 1 %-os illesztett tábla feltöltése. Húzza a táblázat első, ha létezik.

Ebben a részben azt a táblák illesztése **nyctaxi\_utazás** és **nyctaxi\_viteldíj**, 1 %-át véletlenszerűen kibontása, és a probléma továbbra is fennáll a mintában szereplő adatok az új tábla nevét **nyctaxi\_egy\_%**:

    cursor = conn.cursor()

    drop_table_if_exists = '''
        IF OBJECT_ID('nyctaxi_one_percent', 'U') IS NOT NULL DROP TABLE nyctaxi_one_percent
    '''

    nyctaxi_one_percent_insert = '''
        SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount, f.total_amount, f.tip_amount
        INTO nyctaxi_one_percent
        FROM nyctaxi_trip t, nyctaxi_fare f
        TABLESAMPLE (1 PERCENT)
        WHERE t.medallion = f.medallion
        AND   t.hack_license = f.hack_license
        AND   t.pickup_datetime = f.pickup_datetime
        AND   pickup_longitude <> '0' AND dropoff_longitude <> '0'
    '''

    cursor.execute(drop_table_if_exists)
    cursor.execute(nyctaxi_one_percent_insert)
    cursor.commit()

### <a name="data-exploration-using-sql-queries-in-ipython-notebook"></a>SQL-lekérdezések segítségével IPython jegyzetfüzet adatok feltárása

Ebben a részben azt használja az 1 %-os mintát adatok megőrződnek az új táblában, azt az előbb létrehozott adatok felosztások Fedezze fel. Fontos megjegyezni, hogy hasonló explorations használatával végezheti el az eredeti táblák, tetszés szerint **TABLESAMPLE** segítségével feltárás minta vagy korlátozza az eredményeket egy adott idő alatt időszak használatával korlátozzák a **Felvétel\_datetime** partíciók, az [adatok feltárása és az SQL Server mérnöki szolgáltatás](#dbexplore) szakaszban bemutatott.

#### <a name="exploration-daily-distribution-of-trips"></a>Feltárása: Utak napi megoszlása

    query = '''
        SELECT CONVERT(date, dropoff_datetime) AS date, COUNT(*) AS c
        FROM nyctaxi_one_percent
        GROUP BY CONVERT(date, dropoff_datetime)
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-per-medallion"></a>Feltárása: Utazás terjesztési medallion száma

    query = '''
        SELECT medallion,count(*) AS c
        FROM nyctaxi_one_percent
        GROUP BY medallion
    '''

    pd.read_sql(query,conn)

### <a name="feature-generation-using-sql-queries-in-ipython-notebook"></a>SQL-lekérdezések segítségével IPython jegyzetfüzet szolgáltatás létrehozása

Ebben a szakaszban azt hoz létre új címkéket, és a szolgáltatásokat közvetlenül az SQL-lekérdezések használatával, a minta 1 %-os tábla működési létrehozása az előző szakaszban.

#### <a name="label-generation-generate-class-labels"></a>Címke létrehozása: Osztály címkék készítése

A következő példa azt készítése levélcímkéket nyomtathat modellezési két csoportja:

1. Bináris címkék osztály **Formabontó** (előrejelzésére, ha kapnak egy tipp)
2. Multiclass címke **tip\_osztály** (előrejelzésére, a tip raktárhely vagy tartomány)

        nyctaxi_one_percent_add_col = '''
            ALTER TABLE nyctaxi_one_percent ADD tipped bit, tip_class int
        '''

        cursor.execute(nyctaxi_one_percent_add_col)
        cursor.commit()

        nyctaxi_one_percent_update_col = '''
            UPDATE nyctaxi_one_percent
            SET
               tipped = CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END,
               tip_class = CASE WHEN (tip_amount = 0) THEN 0
                                WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
                                WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
                                WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
                                ELSE 4
                            END
        '''

        cursor.execute(nyctaxi_one_percent_update_col)
        cursor.commit()

#### <a name="feature-engineering-count-features-for-categorical-columns"></a>Mérnöki szolgáltatás: A kockák oszlopok száma szolgáltatások

Ebben a példában a kockák mező értelmezhetjük azáltal, hogy minden egyes kategória az adatok az előfordulások számát a numerikus mezőben.

    nyctaxi_one_percent_insert_col = '''
        ALTER TABLE nyctaxi_one_percent ADD cmt_count int, vts_count int
    '''

    cursor.execute(nyctaxi_one_percent_insert_col)
    cursor.commit()

    nyctaxi_one_percent_update_col = '''
        WITH B AS
        (
            SELECT medallion, hack_license,
                SUM(CASE WHEN vendor_id = 'cmt' THEN 1 ELSE 0 END) AS cmt_count,
                SUM(CASE WHEN vendor_id = 'vts' THEN 1 ELSE 0 END) AS vts_count
            FROM nyctaxi_one_percent
            GROUP BY medallion, hack_license
        )

        UPDATE nyctaxi_one_percent
        SET nyctaxi_one_percent.cmt_count = B.cmt_count,
            nyctaxi_one_percent.vts_count = B.vts_count
        FROM nyctaxi_one_percent A INNER JOIN B
        ON A.medallion = B.medallion AND A.hack_license = B.hack_license
    '''

    cursor.execute(nyctaxi_one_percent_update_col)
    cursor.commit()

#### <a name="feature-engineering-bin-features-for-numerical-columns"></a>Mérnöki szolgáltatás: Numerikus oszlopokon raktárhely szolgáltatások

Ebben a példában egy folyamatos numerikus mező értelmezhetjük előre definiált kategória tartományok, azaz átalakító numerikus mező a kockák mezőkbe.

    nyctaxi_one_percent_insert_col = '''
        ALTER TABLE nyctaxi_one_percent ADD trip_time_bin int
    '''

    cursor.execute(nyctaxi_one_percent_insert_col)
    cursor.commit()

    nyctaxi_one_percent_update_col = '''
        WITH B(medallion,hack_license,pickup_datetime,trip_time_in_secs, BinNumber ) AS
        (
            SELECT medallion,hack_license,pickup_datetime,trip_time_in_secs,
            NTILE(5) OVER (ORDER BY trip_time_in_secs) AS BinNumber from nyctaxi_one_percent
        )

        UPDATE nyctaxi_one_percent
        SET trip_time_bin = B.BinNumber
        FROM nyctaxi_one_percent A INNER JOIN B
        ON A.medallion = B.medallion
        AND A.hack_license = B.hack_license
        AND A.pickup_datetime = B.pickup_datetime
    '''

    cursor.execute(nyctaxi_one_percent_update_col)
    cursor.commit()

#### <a name="feature-engineering-extract-location-features-from-decimal-latitudelongitude"></a>Mérnöki szolgáltatás: Hely szolgáltatások kinyerni decimális szélesség/hosszúság

Ez a példa bontja le a szélesség és/vagy a hosszúság mezőben decimális ábrázolása különböző engedélyeknek több régió mezőt például ország, város, község, blokk, stb. Fontos megjegyezni, hogy az új geo-mező nincs leképezve tényleges helyét. A hozzárendelés geocode helyek információ: [Bing Maps többi szolgáltatások](https://msdn.microsoft.com/library/ff701710.aspx).

    nyctaxi_one_percent_insert_col = '''
        ALTER TABLE nyctaxi_one_percent
        ADD l1 varchar(6), l2 varchar(3), l3 varchar(3), l4 varchar(3),
            l5 varchar(3), l6 varchar(3), l7 varchar(3)
    '''

    cursor.execute(nyctaxi_one_percent_insert_col)
    cursor.commit()

    nyctaxi_one_percent_update_col = '''
        UPDATE nyctaxi_one_percent
        SET l1=round(pickup_longitude,0)
            , l2 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 1 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),1,1) ELSE '0' END     
            , l3 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 2 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),2,1) ELSE '0' END     
            , l4 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 3 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),3,1) ELSE '0' END     
            , l5 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 4 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),4,1) ELSE '0' END     
            , l6 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 5 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),5,1) ELSE '0' END     
            , l7 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 6 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),6,1) ELSE '0' END
    '''

    cursor.execute(nyctaxi_one_percent_update_col)
    cursor.commit()

#### <a name="verify-the-final-form-of-the-featurized-table"></a>Ellenőrizze a végső formája a featurized tábla

    query = '''SELECT TOP 100 * FROM nyctaxi_one_percent'''
    pd.read_sql(query,conn)

Most vagyunk készen áll a modell épület és [Azure gép tanulási](https://studio.azureml.net)modell telepítését. A korábban, nevezetesen azonosított előrejelző probléma készen áll az adatok:

1. Bináris osztályozás: előre-e a tip fizették az útnak.

2. Multiclass besorolása: előre fizetett, a korábban definiált osztályok szerinti tipp körét.

3. Regressziós feladat: tipp utazásra fizetett összeg előre.  


## <a name="mlmodel"></a>Épület Azure gép tanulási modellek

Modellezési gyakorlására megkezdéséhez jelentkezzen be a gép tanulás Azure munkaterület. Ha még nem hozott létre a munkaterület tanulás gép, lásd: [Create egy Azure gép tanulás munkaterülethez](machine-learning-create-workspace.md).

1. Azure gép tanulásra való ismerkedés, lásd: [Mi az Azure gép tanulás Studio?](machine-learning-what-is-ml-studio.md)

2. Jelentkezzen be az [Azure gép tanulás Studio](https://studio.azureml.net).

3. Studio honlap nagyfokú információk, videók, oktatóanyagok, hivatkozások a modulok hivatkozás és más erőforrásokhoz biztosít. Azure gép tanulás kapcsolatban további információt a [Dokumentáció tanulás Azure gépcsoport](https://azure.microsoft.com/documentation/services/machine-learning/)folytat.

Egy tipikus képzési kísérletben a következőkből áll:

1. Hozzon létre egy **Új +** kísérletben.
2. A lekérdezés Azure gép tanulás.
3. Előzetes feldolgozását, átalakítás és kezelhetik az adatokat szükség szerint.
4. Szolgáltatások létrehozása, szükség szerint.
5. Az adatok felosztása képzés, ellenőrzési/tesztelési adatkészletek (vagy különálló adathalmazok minden).
6. Jelöljön ki egy vagy több gépi tanulási algoritmusok attól függően, hogy a tanulási probléma megoldására. Pl. bináris osztályozás multiclass osztályozás, regresszió.
7. A vonat egy vagy több modellt, a képzés a dataset használata.
8. A játékos az érvényesítési adatkészlet képzett a modellek segítségével.
9. A modellek számítja ki a megfelelő jogosultságok a tanulási probléma értékeli.
10. Finom hangolás a modellek, és válassza ki a legjobb típusú.

A következő gyakorlatban azt már megismerkedett és fejthetők vissza az adatokat az SQL Server, és be a minta mérete úgy határozott, hogy a gép tanulás Azure ingest. Egy vagy több döntöttünk előrejelzési modellek létrehozása:

1. Azure gép tanulás segítségével az [Adatok importálása] az adatok[ import-data] modul **adat bemenet és kimenet** szakaszában. További tudnivalókért tanulmányozza az [Adatok beolvasása] [ import-data] modul tájékoztató oldalt.

    ![Borzas gép tanulás adatok importálása][17]

2. A **Tulajdonságok** panelen az **adatforrás** **SQL Azure-adatbázis** kiválasztása

3. Az **adatbázis-kiszolgáló neve** mezőben adja meg a DNS-adatbázis nevét. Formátum:`tcp:<your_virtual_machine_DNS_name>,1433`

4. A megfelelő mezőben adja meg az **adatbázis nevét** .

5. Adja meg az **SQL-felhasználónevet** a **Server aqccount felhasználónevet és a jelszót a **kiszolgáló felhasználói fiók jelszavát **.

6. Jelölje be a beállítást **a kiszolgáló tanúsítványát fogadja el** .

7. Az **adatbázis-lekérdezés** szerkesztése szöveg területen beillesztése a lekérdezést, amely kibontja a szükséges adatbázis-mezők (például a számított mezőket is beleértve), és le a kívánt minta mérete az adatok minták.

Az alábbi ábra lehet például egy bináris osztályozás kísérlet közvetlenül az SQL Server adatbázisba történő olvasáskor. Hasonló kísérletek multiclass osztályozáshoz és regressziós problémák is kell kialakítani.

![Borzas gép tanulás vonat][10]

> [AZURE.IMPORTANT] A modellezési adatok kinyerésére és a lekérdezés-példák a mintavételi biztosítani, az előző szakaszok **a három modellezési gyakorlatok az összes címke a lekérdezésben szereplő**. (Kötelező) fontos lépés a modellezési gyakorlatok minden **kizárni** két problémákat, és bármely más **érintett memóriavesztést**szükségtelen címkéit. Pl. bináris osztályozás használatakor **Formabontó** címke használata és kizárása a mezők **tip\_osztály**, **tip\_összeg**, és **teljes\_összeg**. Az utóbbi cél szivárgás, mivel azt tartalmazzák, a tip fizetett.
>
> Kizárja a felesleges oszlopokat, és/vagy érintett szivárgás, használhatja az [Oszlopok kiválasztása a Dataset] [ select-columns] modul vagy a [Metaadatok szerkesztése][edit-metadata]. További tudnivalókért lásd az [Oszlopok kiválasztása a Dataset] [ select-columns] és a [Metaadatok szerkesztése] [ edit-metadata] lapok hivatkoznak.

## <a name="mldeploy"></a>Modellek a tanulási Azure gép üzembe helyezése

Amikor a modell készen áll, így könnyen telepíthető azt közvetlenül a kísérlet a webes szolgáltatás. Azure gép tanulás webszolgáltatások telepítésével kapcsolatos további tudnivalókért lásd: [Deploy az Azure gép tanulás webszolgáltatás](machine-learning-publish-a-machine-learning-web-service.md).

Egy új webes szolgáltatás telepítéséhez kell:

1. Hozzon létre egy pontozási kísérletben.
2. A webszolgáltatás telepítése.

A **Befejezett** képzés kísérlet pontozási kísérlet létrehozásához kattintson a **Létrehozása a PONTOZÁS KÍSÉRLETEZHET** a művelet alsó sávon.

![Borzas pontozás][18]

Azure gép tanulás megpróbálja létrehozni egy pontozási kísérletben a képzés kísérlet az összetevők alapján. Különösen a program:

1. A képzett modell mentése és a modell képzési modulok eltávolítása.
2. Egy logikai **bemeneti port** képviseli a várható bemeneti adatok séma azonosítása.
3. Logikai **kimeneti port** , amely képviseli a várható webes szolgáltatás kimeneti séma azonosítása.

A pontozási kísérlet létrehozásakor ellenőrzésre, majd szükség szerint módosítsa. Egy tipikus kiigazítás helyébe lép a bemeneti dataset és/vagy a lekérdezés egy, amely kizárja a címke mezőkben, ezek nem lesznek elérhetők, ha a szolgáltatás neve. Egyben a helyes gyakorlat néhány rekordot, hogy a bemeneti dataset és/vagy a lekérdezés méretének csökkentéséhez csak elég jelzi a beviteli sémából. A kimeneti port közös, kizárja az összes beviteli mezőt, és csak a **Címkék pontszáma** és **Valószínűségek pontszáma** bevonni az [Oszlopok kiválasztása a Dataset] használatával a kimenet[ select-columns] modul.

Kísérlet pontozási minta szerepel az ábrán. Ha készen áll a központi telepítése, a **WEBES közzététel szolgáltatás** gombra a művelet alsó sávon.

![Borzas gép tanulás közzététele][11]

Recap, ez a forgatókönyv oktatóprogram a Borzas adatok tudományos környezetben, egészen a képzést és az Azure gép tanulás webszolgáltatás telepítése modellezésére adatgyűjtést nagy nyilvános dataset együttműködik hozott létre.

### <a name="license-information"></a>A Licencadatok

A minta forgatókönyv és a mellékelt parancsfájlok és IPython notebook(s) által megosztott Microsoft a MIT licence alapján. Ellenőrizze a LICENSE.txt fájl a könyvtárban a példakód további részleteket a GitHub.

### <a name="references"></a>Hivatkozások

• [Andrés Monroy a következőt: Taxi Trips lap letöltése](http://www.andresmh.com/nyctaxitrips/)  
• [FOILing a következőt: Taxi utazás adatok szerint Chris Whong](http://chriswhong.com/open-data/foil_nyc_taxi/)   
• a [következőt: Taxi és Limousine Bizottság kutatás és statisztika](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)


[1]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_26_1.png
[2]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_28_1.png
[3]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_35_1.png
[4]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_36_1.png
[5]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_39_1.png
[6]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_42_1.png
[7]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_44_1.png
[8]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_46_1.png
[9]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_71_1.png
[10]: ./media/machine-learning-data-science-process-sql-walkthrough/azuremltrain.png
[11]: ./media/machine-learning-data-science-process-sql-walkthrough/azuremlpublish.png
[12]: ./media/machine-learning-data-science-process-sql-walkthrough/ssmsconnect.png
[13]: ./media/machine-learning-data-science-process-sql-walkthrough/executescript.png
[14]: ./media/machine-learning-data-science-process-sql-walkthrough/sqlserverproperties.png
[15]: ./media/machine-learning-data-science-process-sql-walkthrough/sqldefaultdirs.png
[16]: ./media/machine-learning-data-science-process-sql-walkthrough/bulkimport.png
[17]: ./media/machine-learning-data-science-process-sql-walkthrough/amlreader.png
[18]: ./media/machine-learning-data-science-process-sql-walkthrough/amlscoring.png


<!-- Module References -->
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
