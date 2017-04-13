<properties
    pageTitle="A csoportwebhely adatok tudományos folyamat működését: használata SQL adatraktár |} Microsoft Azure"
    description="Fejlett analitikai folyamat és technológia működésben"  
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
    ms.date="06/24/2016"
    ms.author="bradsev;hangzh;weig"/>


# <a name="the-team-data-science-process-in-action-using-sql-data-warehouse"></a>A csoportwebhely adatok tudományos folyamat működését: SQL adatraktár használatával

Ebben az oktatóanyagban ismerni összeállítását, és üzembe helyezése a gépi tanulási modell használata SQL adatraktár (SQL DW) keresztül a nyilvánosan elérhető adatkészlet – a [Következőt: Taxi utakat](http://www.andresmh.com/nyctaxitrips/) adatkészlet számára. A helyes összeállítás bináris besorolás modell = esetben előrejelzése, vagy sem ezt a tippet kifizetett utazása és multiclass osztályozás és a regressziós modellek is ismertetik, hogy a terjesztési előre kifizetett tipp összegek.

Az eljárás a [Csapat adatok tudományos folyamat (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) munkafolyamat követi. Bemutatjuk, hogy egy adatok tudományos környezet beállítása az adatok betöltése SQL DW, és használatával hogyan SQL DW vagy egy IPython jegyzetfüzet ismerje meg az adatok és adatbázismodellbe modellel szolgáltatásait. Ezután bemutatjuk módját, létre és helyezhetnek üzembe a Azure gépi tanulási modell.


## <a name="dataset"></a>A következőt: Taxi utakat adatkészlet

A következőt: Taxi utazás adatok körülbelül 20GB-nyi tömörített CSV-fájlok (nem tömörített ~ 48GB) áll, minden út kifizetett felvételek 173 milliónál egyes utakat és a díjakat. Utazás rekordokat tartalmazza a felvétel és Gyűjtőtár helyek és időpontok, anonimizált támadó szoftver (illesztőprogram) licencszámmal, és a medallion (taxi tartozó egyedi azonosító) számát. Az adatok minden utakat bemutatja a 2013-ben és az egyes az a következő két adatkészleteket megadva:

1. A **trip_data.csv** út részleteket, például a utasok, felvétel és dropoff pontok, út időtartama és az utazás hossza tartalmaz. Íme néhány példa rekordot:

        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868

2. A **trip_fare.csv** fájlt tartalmazza a jegy ára kifizetett minden út, például a fizetés típusát, jegy ára összege, emelt díjas és adót, tippek és autópályadíjak, és a végösszeget kifizetett részleteit. Íme néhány példa rekordot:

        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

A csatlakozáshoz üzenetváltási használt **egyedi kulcs** \_adatokat, és utazás\_jegy ára tevődik össze a következő három elem látható:

- medallion,
- ellophatja\_licenc és
- Felvétel\_datetime.

## <a name="mltasks"></a>Három típusú előrejelzési feladatokat cím

Azt megfogalmazásához alapján három előrejelzési problémák a *Tipp\_összeg* szemléltetésére tevékenységek modellezése három típusú:

1. **Bináris besorolás**: előrejelzésére engedélyezi vagy tiltja a tipp fizettek utazása, azaz egy *Tipp\_összeg* nagyobb, mint 0 Ft értéke egy szám pozitív példa, miközben egy *Tipp\_összeg* 0 Ft van a negatív példa.

2. **Multiclass besorolás**: előre kifizetett az utazás tipp a cellatartományt. Azt osztása a *Tipp\_összeg* öt intervallumokat vagy osztályok:

        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20

3. **Regressziós tevékenység**: előre kifizetett utazása tipp mennyiségét.  


## <a name="setup"></a>Fejlett analitikai Azure adatok tudományos környezet beállítása

A Azure adatok tudományos környezet beállításához kövesse az alábbi lépéseket.

**A saját Azure blob-tároló fiók létrehozása**

- A saját Azure blob-tárolóhoz kiépítése meg, amikor válassza ki a Azure blob-tárolóban lévő geo-helyét, vagy **Központi Dél-amerikai**, amely a lehető legközelebb a következőt: Taxi adatokat tároló. Az adatokat a program átmásolja tárolóhoz a nyilvános blob-tároló tárolóból AzCopy használata a saját tárterület-fiókját. A közelebb az Azure blob-tárolóhoz Dél központi Nekünk, ha minél gyorsabban (lépés: 4) ebben a feladatban befejeződik.
- A saját Azure tárterület-fiók létrehozásához kövesse a tagolt [kapcsolatos Azure tároló fiókok](../storage/storage-create-storage-account.md)elemre. Ügyeljen arra, hogy az értékeket az alábbi tároló fiók hitelesítő adatait a jegyzetek, azokat a jelen útmutató lesz szükség szerint.

  - **Tárterület-fiók neve**
  - **Tárterület Fiókkulcs**
  - **Tároló neve** (Ez azt szeretné, hogy tárolja az Azure blob-tárolóban lévő adatok)

**Az Azure SQL-DW példány kiépítése.**
Kövesse a dokumentációt a [egy SQL adatraktár létrehozása](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md) egy SQL adatraktár példány hozhatók létre. Ellenőrizze, hogy jelölések kinagyíthatja a következő SQL adatraktár hitelesítő adatok későbbi lépéseiben használt.

  - **Kiszolgáló neve**: <server Name>. database.windows.net
  - **Név SQLDW (adatbázis)**
  - **Felhasználónév**
  - **Jelszó**

**Telepítse a Visual Studio 2015 és az SQL Server Data Tools.** Útmutatásért lásd: a [Visual Studio 2015 telepítése és/vagy SSDT (SQL Server Data Tools) az SQL adatraktár](../sql-data-warehouse/sql-data-warehouse-install-visual-studio.md).

**Az Azure SQL-DW a Visual Studio csatlakozni.** Útmutatásért lásd: az 1 és a [Microsoft Azure SQL-adatraktár a Visual Studio szeretne](../sql-data-warehouse/sql-data-warehouse-connect-overview.md)2 lépéseket.

>[AZURE.NOTE] Az adatbázisban, az SQL adatraktár (helyett a lekérdezést a csatlakozás témakör 3 megadva) létrehozott az alábbi SQL-lekérdezés futtatásával **Hozzon létre egy minta kulcsot**.

    BEGIN TRY
           --Try to create the master key
        CREATE MASTER KEY
    END TRY
    BEGIN CATCH
           --If the master key exists, do nothing
    END CATCH;

**Hozzon létre egy Azure gépi tanulási munkaterületet csoportban az Azure előfizetés.** Útmutatásért lásd: [az Azure gépi tanulási munkaterület létrehozása](machine-learning-create-workspace.md).

## <a name="getdata"></a>Adatok betöltése SQL adatraktár

Nyissa meg a Windows PowerShell-parancsot konzolt. Futtassa az alábbi PowerShell szolgáló parancsok a példa SQL letöltése parancsfájlok, amely azt megosztva Önnel a Github a paraméterrel megadott helyi könyvtár *- DestDir*. Módosíthatja a paraméter értéke *-DestDir* bármely helyi könyvtár. Ha *- DestDir* nem létezik, azt hozza létre a PowerShell-parancsprogramot.

>[AZURE.NOTE] Szükség lehet a **Futtatás rendszergazdaként** az alábbi PowerShell parancsprogramot végrehajtásakor, ha a *DestDir* címtárban rendszergazdai jogosultságot szeretne létrehozni, vagy megírhatja azt.

    $source = "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/SQLDW/Download_Scripts_SQLDW_Walkthrough.ps1"
    $ps1_dest = "$pwd\Download_Scripts_SQLDW_Walkthrough.ps1"
    $wc = New-Object System.Net.WebClient
    $wc.DownloadFile($source, $ps1_dest)
    .\Download_Scripts_SQLDW_Walkthrough.ps1 –DestDir 'C:\tempSQLDW'

A sikeres végrehajtás, után az aktuális munkakönyvtár *- DestDir*változik. Látnia kell képernyő jelenik meg, hogy például az alábbi:

![][19]

A *- DestDir*hajtsa végre az alábbi PowerShell parancsprogramot rendszergazdai módban:

    ./SQLDW_Data_Import.ps1

A PowerShell-parancsprogramot, az első alkalommal fut, amikor a program megkéri a szövegbeviteli az Azure SQL-DW és Azure blob-tároló fiókja adatait. A PowerShell-parancsprogramot befejezése után a hitelesítő adatok első alkalommal fut, beviteli fog készült a bemutató munkakönyvtár konfigurációs fájl SQLDW.conf. A PowerShell parancsprogramot fájl jövőbeli futtatás esetén minden szükséges konfigurációs fájl paraméterek olvasható lehetősége van. Ha meg kell néhány paramétereinek módosítása, megadhatja a figyelmeztető üzenet esetén a képernyőn a paraméterek a konfigurációs fájl törlése és a paraméterek értékét bevitel, gombra, vagy a paraméterértékeket módosítása a SQLDW.conf fájl *– DestDir* címtárában szerkesztésével.

>[AZURE.NOTE] Séma neve ütközik azokat, amelyek már megtalálható az Azure SQL-DW paraméterek közvetlenül a SQLDW.conf fájlból olvasásakor elkerülése érdekében 3 jegyű véletlen számot bekerül a séma nevét a SQLDW.conf fájlból minden futtatásakor alapértelmezett séma neveként. A PowerShell-parancsprogramot, kérheti egy séma neve: felhasználói megítélése szerint adható meg a nevét.

A **PowerShell-parancsprogramot** fájl befejeződik, az alábbi műveleteket:

- **Letölti és telepíti az AzCopy**, ha nem telepítette AzCopy

        $AzCopy_path = SearchAzCopy
        if ($AzCopy_path -eq $null){
            Write-Host "AzCopy.exe is not found in C:\Program Files*. Now, start installing AzCopy..." -ForegroundColor "Yellow"
            InstallAzCopy
            $AzCopy_path = SearchAzCopy
        }
            $env_path = $env:Path
            for ($i=0; $i -lt $AzCopy_path.count; $i++){
                if ($AzCopy_path.count -eq 1){
                    $AzCopy_path_i = $AzCopy_path
                } else {
                    $AzCopy_path_i = $AzCopy_path[$i]
                }
                if ($env_path -notlike '*' +$AzCopy_path_i+'*'){
                    Write-Host $AzCopy_path_i 'not in system path, add it...'
                    [Environment]::SetEnvironmentVariable("Path", "$AzCopy_path_i;$env_path", "Machine")
                    $env:Path = [System.Environment]::GetEnvironmentVariable("Path","Machine")
                    $env_path = $env:Path
                }

- **Másolatok adatok a magánjellegű blob tárterület-fiókjába** a nyilvános blob AzCopy együtt

        Write-Host "AzCopy is copying data from public blob to yo storage account. It may take a while..." -ForegroundColor "Yellow"
        $start_time = Get-Date
        AzCopy.exe /Source:$Source /Dest:$DestURL /DestKey:$StorageAccountKey /S
        $end_time = Get-Date
        $time_span = $end_time - $start_time
        $total_seconds = [math]::Round($time_span.TotalSeconds,2)
        Write-Host "AzCopy finished copying data. Please check your storage account to verify." -ForegroundColor "Yellow"
        Write-Host "This step (copying data from public blob to your storage account) takes $total_seconds seconds." -ForegroundColor "Green"


- **Terhelések adatok Polybase (által LoadDataToSQLDW.sql végrehajtása) használatával az Azure SQL-DW** magánjellegű blob tároló fiókját, az alábbi parancsokat.

    - A séma

            EXEC (''CREATE SCHEMA {schemaname};'');

    - Hozzon létre egy adatbázis hatóköre hitelesítő adatok

            CREATE DATABASE SCOPED CREDENTIAL {KeyAlias}
            WITH IDENTITY = ''asbkey'' ,
            Secret = ''{StorageAccountKey}''

    - Hozzon létre egy külső adatforrás-tároló Azure blob-

            CREATE EXTERNAL DATA SOURCE {nyctaxi_trip_storage}
            WITH
            (
                TYPE = HADOOP,
                LOCATION =''wasbs://{ContainerName}@{StorageAccountName}.blob.core.windows.net'',
                CREDENTIAL = {KeyAlias}
            )
            ;

            CREATE EXTERNAL DATA SOURCE {nyctaxi_fare_storage}
            WITH
            (
                TYPE = HADOOP,
                LOCATION =''wasbs://{ContainerName}@{StorageAccountName}.blob.core.windows.net'',
                CREDENTIAL = {KeyAlias}
            )
            ;

    - Hozzon létre egy külső fájlformátum csv-fájlba. Adatok nem tömörített és mezők választják el a függőleges vonal karakter.

            CREATE EXTERNAL FILE FORMAT {csv_file_format}
            WITH
            (   
                FORMAT_TYPE = DELIMITEDTEXT,
                FORMAT_OPTIONS  
                (
                    FIELD_TERMINATOR ='','',
                    USE_TYPE_DEFAULT = TRUE
                )
            )
            ;

    - Külső jegy ára és a következőt: taxi adatkészlet utazás táblákat létrehozása az Azure blob-tárolóhoz.

            CREATE EXTERNAL TABLE {external_nyctaxi_fare}
            (
                medallion varchar(50) not null,
                hack_license varchar(50) not null,
                vendor_id char(3),
                pickup_datetime datetime not null,
                payment_type char(3),
                fare_amount float,
                surcharge float,
                mta_tax float,
                tip_amount float,
                tolls_amount float,
                total_amount float
            )
            with (
                LOCATION    = ''/nyctaxifare/'',
                DATA_SOURCE = {nyctaxi_fare_storage},
                FILE_FORMAT = {csv_file_format},
                REJECT_TYPE = VALUE,
                REJECT_VALUE = 12     
            )  


            CREATE EXTERNAL TABLE {external_nyctaxi_trip}
            (
                medallion varchar(50) not null,
                hack_license varchar(50)  not null,
                vendor_id char(3),
                rate_code char(3),
                store_and_fwd_flag char(3),
                pickup_datetime datetime  not null,
                dropoff_datetime datetime,
                passenger_count int,
                trip_time_in_secs bigint,
                trip_distance float,
                pickup_longitude varchar(30),
                pickup_latitude varchar(30),
                dropoff_longitude varchar(30),
                dropoff_latitude varchar(30)
            )
            with (
                LOCATION    = ''/nyctaxitrip/'',
                DATA_SOURCE = {nyctaxi_trip_storage},
                FILE_FORMAT = {csv_file_format},
                REJECT_TYPE = VALUE,
                REJECT_VALUE = 12         
            )

    - Azure blob-tárolóban lévő külső táblákat az SQL adatraktár adatok betöltése

            CREATE TABLE {schemaname}.{nyctaxi_fare}
            WITH
            (   
                CLUSTERED COLUMNSTORE INDEX,
                DISTRIBUTION = HASH(medallion)
            )
            AS
            SELECT *
            FROM   {external_nyctaxi_fare}
            ;

            CREATE TABLE {schemaname}.{nyctaxi_trip}
            WITH
            (   
                CLUSTERED COLUMNSTORE INDEX,
                DISTRIBUTION = HASH(medallion)
            )
            AS
            SELECT *
            FROM   {external_nyctaxi_trip}
            ;

    - (NYCTaxi_Sample) minta adattábla készítése, és szúrja be a adatok azt az út- és jegy ára táblázatokban SQL-lekérdezések válasszanak. (Az útmutató a néhány lépést kell használnia a mintatáblázat.)

            CREATE TABLE {schemaname}.{nyctaxi_sample}
            WITH
            (   
                CLUSTERED COLUMNSTORE INDEX,
                DISTRIBUTION = HASH(medallion)
            )
            AS
            (
                SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount, f.total_amount, f.tip_amount,
                tipped = CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END,
                tip_class = CASE
                        WHEN (tip_amount = 0) THEN 0
                        WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
                        WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
                        WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
                        ELSE 4
                    END
                FROM {schemaname}.{nyctaxi_trip} t, {schemaname}.{nyctaxi_fare} f
                WHERE datepart("mi",t.pickup_datetime) = 1
                AND t.medallion = f.medallion
                AND   t.hack_license = f.hack_license
                AND   t.pickup_datetime = f.pickup_datetime
                AND   pickup_longitude <> ''0''
                AND   dropoff_longitude <> ''0''
            )
            ;

A tárhely fiókok földrajzi helye betöltésével van hatással.

>[AZURE.NOTE] Attól függően, hogy a földrajzi hely a személyes blob-tároló fiók, nyilvános blob adatok másolása a személyes tárterület-fiókjába a folyamat tehet körülbelül 15 percet, vagy akár hosszabb, és az adatok betöltésének a tárterület-fiókjából szeretne az Azure SQL-DW folyamat 20 perc is eltelhet vagy hosszú.  

Be kell milyen döntse el, ha ismétlődő forrás- és fájljait.

>[AZURE.NOTE] Ha a .csv-fájljait, ahol a másolni kívánt a magánjellegű blob tárterület-fiókjába a nyilvános blob-tárolóhoz már létezik a magánjellegű blob-tároló fiókjában, akkor AzCopy rákérdez, hogy szeretné-e felülírni. Ha nem szeretne felülírni, a bemeneti **n** üzenet megjelenésekor. Ha azt szeretné, felülírja az **összes** őket, a bemeneti **egy** üzenet megjelenésekor. Beviteli **y** felülírja a .csv fájl egyenként is.

![#21 ábrázolása][21]

A saját adatain is használhatja. Ha az adatok helyszíni számítógépén a valós életben alkalmazásban, töltse fel a helyszíni adatok a magánjellegű Azure blob-tárolóhoz továbbra is használhatja a AzCopy. Csak akkor van szüksége az **adatforrás** helyének megváltoztatásához `$Source = "http://getgoing.blob.core.windows.net/public/nyctaxidataset"`, a AzCopy parancs a helyi könyvtár származó adatot tartalmazó PowerShell parancsprogramot fájlt.


>[AZURE.TIP] Ha az adatok a valós életben alkalmazásban már a magánjellegű Azure blob-tárolóban lévő, a PowerShell-parancsprogramot AzCopy hagyja, és közvetlenül Azure SQL-DW töltse fel az adatokat. Ehhez a parancsfájl megfelelően alakíthatja ki a formátumot, az adatok további módosításokat.


A Powershell-parancsprogramot is csatlakoztatja az Azure SQL-DW információt az adatok feltárása példa fájlokba SQLDW_Explorations.sql SQLDW_Explorations.ipynb és SQLDW_Explorations_Scripts.py, hogy ezek a fájlok készen próbálkozik azonnali PowerShell-parancsprogramot befejeződése után.

A sikeres végrehajtás után látni fogja a képernyő alatti hasonlóan:

![][20]

## <a name="dbexplore"></a>Adatok feltárása és az Azure SQL-adatraktár szolgáltatás műszaki

Ebben a részben azt adatok feltárása és szolgáltatás generációs által végzett futó lekérdezések SQL Azure SQL-DW közvetlenül a **Visual Studio Adateszközök**használatával. Ebben a részben használt összes SQL-lekérdezések a *SQLDW_Explorations.sql*nevű mintaparancsfájl találhatók. Ez a fájl már letöltött a helyi könyvtár által a PowerShell-parancsprogramot. Akkor is megtalálja a [Github](https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/SQLDW/SQLDW_Explorations.sql). De Github fájlban nem szerepelnek az Azure SQL-DW dugva.

Csatlakozás az Azure SQL-DW Visual Studio használata SQL DW bejelentkezési nevét és jelszavát, és nyissa meg az **SQL-objektum Explorer** kattintva erősítse meg, az adatbázis és tábla importálása. Lekérdezi a *SQLDW_Explorations.sql* fájlt.

>[AZURE.NOTE] A párhuzamos raktári (PDW) a Lekérdezésszerkesztő megnyitása, a paranccsal **Új lekérdezést** miközben ki van jelölve a párhuzamos Adatraktárhoz az **SQL-objektum Explorer**. A szokásos SQL Lekérdezésszerkesztő PDW által nem támogatott.

Milyen típusú adatokat az alábbiakban feltárására és szolgáltatás generációs műveletek a szakasz tartalma:

- Ismerkedjen meg néhány mezők változó idő Windows adatok terjesztését.
- Vizsgálja meg a hosszúság és a szélesség mező adatok minőségének.
- Bináris és multiclass besorolás címkét alapján a **Tipp\_összeg**.
- Szolgáltatások készítése és a számítási/hasonlítsa utazás távolságokat.
- A két tábla csatlakozhat, és bontsa ki a modellek létrehozásához használt véletlenszerűen.

### <a name="data-import-verification"></a>Adatok importálása ellenőrzése

Ezeket a lekérdezéseket, adja meg a sorok száma egy rövid ellenőrzése és a korábbi használatával Polybase a párhuzamos tömeges töltve táblaoszlopok importálni,

    -- Report number of rows in table <nyctaxi_trip> without table scan
    SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_trip>')

    -- Report number of columns in table <nyctaxi_trip>
    SELECT COUNT(*) FROM information_schema.columns WHERE table_name = '<nyctaxi_trip>' AND table_schema = '<schemaname>'

**Kimenet:** 173,179,759 sorok és oszlopok 14 szerezheti be.

### <a name="exploration-trip-distribution-by-medallion"></a>Feltárása: Utazás terjesztési medallion szerint

Ez a példa lekérdezés azonosítja a 100-nál több utakat befejeződött a megadott időszakon belül medallions (taxi számok). A lekérdezés előnyös particionált táblázat elérése óta azt van tárolni a partíciót rendszerben **Felvétel\_datetime**. A teljes adatkészlet lekérdezése is láthatóvá válik a particionált táblázat használja, illetve indexelni a beolvasott képet.

    SELECT medallion, COUNT(*)
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    GROUP BY medallion
    HAVING COUNT(*) > 100

**Kimenet:** A lekérdezés megadása a 13,369 medallions (taxikra) és az utazás őket a 2013-ban elvégeznie sort tartalmazó táblázat vissza. Az utolsó oszlop befejeződött utakat számát tartalmazza.

### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a>Feltárása: Utazás terjesztési medallion és hack_license

Ez a példa azonosítja a medallions (taxi számok), és hack_license számok (illesztőprogramok), amely a lefolytatottak legfeljebb 100 utakat adott időszakon belül.

    SELECT medallion, hack_license, COUNT(*)
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130131'
    GROUP BY medallion, hack_license
    HAVING COUNT(*) > 100

**Kimenet:** A lekérdezés megadása a 13,369 autós/illesztőprogram további adott 100 utakat a 2013-ban végrehajtott azonosítók 13,369 sort tartalmazó táblázat vissza. Az utolsó oszlop befejeződött utakat számát tartalmazza.

### <a name="data-quality-assessment-verify-records-with-incorrect-longitude-andor-latitude"></a>Adatok vizsgálatának: helytelen hosszúság és/vagy a szélesség rekordok ellenőrzése

Ez a példa megvizsgálja, ha egyetlen hosszúság és/vagy a szélesség mező vagy érvénytelen értéket tartalmaz (radián fokban -90 és 90 között kell lennie), vagy nem (0; 0) koordinátáit.

    SELECT COUNT(*) FROM <schemaname>.<nyctaxi_trip>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(pickup_latitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_latitude AS float) NOT BETWEEN -90 AND 90
    OR    (pickup_longitude = '0' AND pickup_latitude = '0')
    OR    (dropoff_longitude = '0' AND dropoff_latitude = '0'))

**Kimenet:** A lekérdezés 837,467 utakat, amelyek érvénytelen hosszúság és/vagy a szélesség mező adja vissza.

### <a name="exploration-tipped-vs-not-tipped-trips-distribution"></a>És magyarázat: Összehasonlítás nem Formabontó utakat terjesztési Formabontó

Ez a példa utakat, hogy azok Formabontó összehasonlítása a szám, amely nem voltak Formabontó megadott időszak (vagy a teljes adatkészlet, ha a teljes év kiterjedő van beállítva az alábbi) számát találja meg. A terjesztési a bináris címke eloszlás később használható bináris besorolás modellezési tükrözi.

    SELECT tipped, COUNT(*) AS tip_freq FROM (
      SELECT CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped, tip_amount
      FROM <schemaname>.<nyctaxi_fare>
      WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tipped

**Kimenet:** A lekérdezés a következő tipp gyakoriságok 2013: 90,447,622 évre Formabontó és 82,264,709 nem-Formabontó térjen vissza.

### <a name="exploration-tip-classrange-distribution"></a>És magyarázat: Ezt a tippet osztály vagy tartomány ki.

Ez a példa tipp tartományok egy adott időszakban (vagy a teljes adatkészlet, ha a teljes évre vonatkozó) eloszlását számítja ki. Ez az a címke osztályok, hogy később használandó multiclass besorolás modellezési eloszlását.

    SELECT tip_class, COUNT(*) AS tip_freq FROM (
        SELECT CASE
            WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tip_class

**Kimenet:**

|tip_class  | tip_freq |
| --------- | -------|
|1  | 82230915 |
|2  | 6198803 |
|3  | 1932223 |
|0  | 82264625 |
|4  | 85765 |

### <a name="exploration-compute-and-compare-trip-distance"></a>Feltárása: Számítja ki, és utazás távolság összehasonlítása

Ebben a példában a felvétel és Gyűjtőtár hosszúság alakítja, és SQL földrajzi szélesség mutat, függvény használata SQL földrajzi pontok különbség utazás távolság ki, és az eredmények összehasonlításra véletlen minta adja eredményül. A példában az eredmények csak lekérdezéssel az adatok minőség értékelési tartozó korábbi érvényes koordináták korlátozza.

    /****** Object:  UserDefinedFunction [dbo].[fnCalculateDistance] ******/
    SET ANSI_NULLS ON
    GO

    SET QUOTED_IDENTIFIER ON
    GO

    IF EXISTS (SELECT * FROM sys.objects WHERE type IN ('FN', 'IF') AND name = 'fnCalculateDistance')
      DROP FUNCTION fnCalculateDistance
    GO

    -- User-defined function to calculate the direct distance  in mile between two geographical coordinates.
    CREATE FUNCTION [dbo].[fnCalculateDistance] (@Lat1 float, @Long1 float, @Lat2 float, @Long2 float)

    RETURNS float
    AS
    BEGIN
        DECLARE @distance decimal(28, 10)
        -- Convert to radians
        SET @Lat1 = @Lat1 / 57.2958
        SET @Long1 = @Long1 / 57.2958
        SET @Lat2 = @Lat2 / 57.2958
        SET @Long2 = @Long2 / 57.2958
        -- Calculate distance
        SET @distance = (SIN(@Lat1) * SIN(@Lat2)) + (COS(@Lat1) * COS(@Lat2) * COS(@Long2 - @Long1))
        --Convert to miles
        IF @distance <> 0
        BEGIN
            SET @distance = 3958.75 * ATAN(SQRT(1 - POWER(@distance, 2)) / @distance);
        END
        RETURN @distance
    END
    GO

    SELECT pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude,
    dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) AS DirectDistance
    FROM <schemaname>.<nyctaxi_trip>
    WHERE datepart("mi",pickup_datetime)=1
    AND CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND pickup_longitude != '0' AND dropoff_longitude != '0'

### <a name="feature-engineering-using-sql-functions"></a>A szolgáltatás műszaki SQL-függvény használata

SQL-függvényekkel előfordul, hogy egy hatékony funkció szolgáltatás műszaki is lehet. Ez a forgatókönyv a definiált egy SQL-függvényben a felvétel és dropoff helyeken közvetlen távolságának számítja ki. A **Visual Studio Adateszközök**futtatását is lehetővé teszi a következő SQL-parancsfájlokat.

Az alábbiakban az SQL-parancsfájlt, amely definiálja az távolság függvény.

    SET ANSI_NULLS ON
    GO

    SET QUOTED_IDENTIFIER ON
    GO

    IF EXISTS (SELECT * FROM sys.objects WHERE type IN ('FN', 'IF') AND name = 'fnCalculateDistance')
      DROP FUNCTION fnCalculateDistance
    GO

    -- User-defined function calculate the direct distance between two geographical coordinates.
    CREATE FUNCTION [dbo].[fnCalculateDistance] (@Lat1 float, @Long1 float, @Lat2 float, @Long2 float)

    RETURNS float
    AS
    BEGIN
        DECLARE @distance decimal(28, 10)
        -- Convert to radians
        SET @Lat1 = @Lat1 / 57.2958
        SET @Long1 = @Long1 / 57.2958
        SET @Lat2 = @Lat2 / 57.2958
        SET @Long2 = @Long2 / 57.2958
        -- Calculate distance
        SET @distance = (SIN(@Lat1) * SIN(@Lat2)) + (COS(@Lat1) * COS(@Lat2) * COS(@Long2 - @Long1))
        --Convert to miles
        IF @distance <> 0
        BEGIN
            SET @distance = 3958.75 * ATAN(SQRT(1 - POWER(@distance, 2)) / @distance);
        END
        RETURN @distance
    END
    GO

Íme egy példa hívja fel az SQL-lekérdezés szolgáltatásokat szeretne függvény:

    -- Sample query to call the function to create features
    SELECT pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude,
    dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) AS DirectDistance
    FROM <schemaname>.<nyctaxi_trip>
    WHERE datepart("mi",pickup_datetime)=1
    AND CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND pickup_longitude != '0' AND dropoff_longitude != '0'

**Kimenet:** A lekérdezés (2,803,538 adatsorokkal) táblázat felvétel és dropoff földrajzi szélesség és hosszúság és a megfelelő közvetlen távolságokat mérföld a hoz létre. Az alábbiakban először 3 sorból az eredményeket:

||pickup_latitude | pickup_longitude    | dropoff_latitude |dropoff_longitude | DirectDistance |
|---| --------- | -------|-------| --------- | -------|
|1  | 40.731804 | -74.001083 | 40.736622 | -73.988953 | .7169601222 |
|2  | 40.715794 | -74,010635 | 40.725338 | -74.00399 | .7448343721 |
|3  | 40.761456 | -73.999886 | 40.766544 | -73.988228 | 0.7037227967 |



### <a name="prepare-data-for-model-building"></a>Az épület adatmodell adatainak előkészítése

A következő lekérdezés illesztések a **nyctaxi\_utazás** és **nyctaxi\_jegy ára** táblázatok, létrehoz egy bináris besorolás címke **Formabontó**, több osztály besorolás címke **Tipp\_osztály**, és a minta olvas a teljes illesztett adatkészlet. A felvételi idő alapján utakat csak egy részhalmazát beolvasása a mintavételnél végre.  A lekérdezés másolja, majd beillesztettek közvetlenül az [Azure gépi tanulási Studio](https://studio.azureml.net) [Adatok importálása] [ import-data] modul közvetlen adatok szempontjából Azure SQL-adatbázis példányból. A lekérdezés kizárja a helytelen rekordot (0; 0) koordinátáit.

    SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount,  f.total_amount, f.tip_amount,
        CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped,
        CASE WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM <schemaname>.<nyctaxi_trip> t, <schemaname>.<nyctaxi_fare> f
    WHERE datepart("mi",t.pickup_datetime) = 1
    AND   t.medallion = f.medallion
    AND   t.hack_license = f.hack_license
    AND   t.pickup_datetime = f.pickup_datetime
    AND   pickup_longitude != '0' AND dropoff_longitude != '0'

Ha készen áll arra, hogy folytassa az Azure gépi tanulási, akkor előfordulhat, hogy vagy:  

1. Az utolsó SQL-lekérdezés kibontása és példa az adatok és a Másolás a Beillesztés a lekérdezés közvetlenül [Az adatok importálása párbeszédpanel] Mentés[ import-data] modul Azure gépi tanulási, vagy
2. A probléma továbbra is fennáll a mintában szereplő visszafejtett adatok és szeretné használni az új SQL DW táblázat épület adatmodell-, és az új táblába az [Adatok importálása] [ import-data] Azure gépi tanulási moduljának. Az előző lépésben a PowerShell-parancsprogramot megállapított ezt meg. Közvetlenül az alábbi táblázat az adatok importálása modul erről.


## <a name="ipnb"></a>Adatok feltárása és szolgáltatás műszaki IPython jegyzetfüzetben

Ebben a részben azt végezze el a adatok feltárása és szolgáltatás generációs mindkét Python használatával, és az SQL-DW SQL-lekérdezéseket korábban létrehozott. A helyi könyvtár letöltött minta IPython Jegyzetfüzet **SQLDW_Explorations.ipynb** és Python parancsfájl **SQLDW_Explorations_Scripts.py** nevű. Azok is elérhetők [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/SQLDW). A két fájlok azonosak Python parancsfájlokat. A Python parancsprogram kapja meg, akkor abban az esetben, ha nem rendelkezik IPython jegyzetfüzetet. E két Python mintafájlok **Python 2.7**alapján készült.

A szükséges Azure SQL-DW adatokat a mintában a helyi számítógépre letöltött IPython jegyzetfüzet és a Python parancsprogram van már csatlakoztatva a PowerShell-parancsprogramot által korábban. Azok a végrehajtható módosítás nélkül.

Ha már beállított egy AzureML munkaterület, közvetlenül a minta IPython jegyzetfüzet feltöltheti a a AzureML IPython jegyzetfüzet szolgáltatás és futtatásával indítása. Az alábbiakban a lépéseket követve töltse fel a AzureML IPython jegyzetfüzet szolgáltatás:

1. Jelentkezzen be a AzureML munkaterület, a képernyő tetején kattintson a "Studio" és "JEGYZETFÜZETEK" a weblap bal oldalán kattintson.

    ![#22 ábrázolása][22]

2. Kattintson az "Új" a weblap bal alsó sarkában lévő gombra, és válassza a "2" Python. Ezután adja meg a jegyzetfüzet nevét, és kattintson az új, üres IPython jegyzetfüzet létrehozása a pipára.

    ![#23 ábrázolása][23]

3. Kattintson az új IPython jegyzetfüzet bal felső sarokban a "Jupyter" szimbólumra.

    ![#24 ábrázolása][24]

4. Húzása a minta IPython jegyzetfüzet a AzureML IPython jegyzetfüzet szolgáltatásban **fa** lapjára, és kattintson a **Feltöltés**elemre. Ezután a IPython jegyzetfüzet feltöltődnek a AzureML IPython jegyzetfüzet szolgáltatás minta.

    ![#25 ábrázolása][25]

A minta futtatásához IPython jegyzetfüzet vagy a Python parancsfájl-fájlt, az alábbi Python csomagok van szükség. A AzureML IPython jegyzetfüzet szolgáltatás használatakor ezekről a csomagokról volna előre telepítve van.

    - pandas
    - numpy
    - matplotlib
    - pyodbc
    - PyTables

Ha fejlett analitikai megoldások fejlesztésére AzureML a nagy adatokkal ajánlott sorrendje a következőket:

- Olvassa el az adatok kis mintában egy a memóriában adatok keretbe.
- Végezze el az egyes megjelenítések és explorations a mintában szereplő adatok felhasználásával.
- Kísérletezés a szolgáltatás műszaki a mintában szereplő adatok felhasználásával.
- Nagyobb adatok feltárása, adatfeldolgozás és szolgáltatás műszaki Python segítségével közvetlenül az SQL-DW szemben SQL-lekérdezést.
- Döntse el kell, hogy megfelelő az Azure gépi tanulási modell épület a minta mérete.

A követő táblázat néhány adatok feltárása, adatmegjelenítés és mérnöki példák szolgáltatás. A minta IPython jegyzetfüzet és a minta Python parancsprogram további adatok explorations is található.

### <a name="initialize-database-credentials"></a>Adatbázis hitelesítő adatai inicializálni

Az adatbázis kapcsolatbeállításokat az alábbi változók a inicializálni:

    SERVER_NAME=<server name>
    DATABASE_NAME=<database name>
    USERID=<user name>
    PASSWORD=<password>
    DB_DRIVER = <database driver>

### <a name="create-database-connection"></a>Az adatbázis-kapcsolat létrehozása

Az alábbiakban a kapcsolati karakterlánc, amely az adatbázishoz való csatlakozással hoz létre.

    CONNECTION_STRING = 'DRIVER={'+DRIVER+'};SERVER='+SERVER_NAME+';DATABASE='+DATABASE_NAME+';UID='+USERID+';PWD='+PASSWORD
    conn = pyodbc.connect(CONNECTION_STRING)

### <a name="report-number-of-rows-and-columns-in-table-nyctaxitrip"></a>Sorok és oszlopok < nyctaxi_trip > táblázat jelentés száma

    nrows = pd.read_sql('''
        SELECT SUM(rows) FROM sys.partitions
        WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_trip>')
    ''', conn)

    print 'Total number of rows = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''
        SELECT COUNT(*) FROM information_schema.columns
        WHERE table_name = ('<nyctaxi_trip>') AND table_schema = ('<schemaname>')
    ''', conn)

    print 'Total number of columns = %d' % ncols.iloc[0,0]

- A sorok száma = 173179759  
- Az oszlopok száma = 14

### <a name="report-number-of-rows-and-columns-in-table-nyctaxifare"></a>Sorok és oszlopok < nyctaxi_fare > táblázat jelentés száma

    nrows = pd.read_sql('''
        SELECT SUM(rows) FROM sys.partitions
        WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_fare>')
    ''', conn)

    print 'Total number of rows = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''
        SELECT COUNT(*) FROM information_schema.columns
        WHERE table_name = ('<nyctaxi_fare>') AND table_schema = ('<schemaname>')
    ''', conn)

    print 'Total number of columns = %d' % ncols.iloc[0,0]

- A sorok száma = 173179759  
- Az oszlopok száma = 11

### <a name="read-in-a-small-data-sample-from-the-sql-data-warehouse-database"></a>Olvasás a kis adatok mintája az SQL raktári-adatbázis

    t0 = time.time()

    query = '''
        SELECT TOP 10000 t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax,
            f.tolls_amount, f.total_amount, f.tip_amount
        FROM <schemaname>.<nyctaxi_trip> t, <schemaname>.<nyctaxi_fare> f
        WHERE datepart("mi",t.pickup_datetime) = 1
        AND   t.medallion = f.medallion
        AND   t.hack_license = f.hack_license
        AND   t.pickup_datetime = f.pickup_datetime
    '''

    df1 = pd.read_sql(query, conn)

    t1 = time.time()
    print 'Time to read the sample table is %f seconds' % (t1-t0)

    print 'Number of rows and columns retrieved = (%d, %d)' % (df1.shape[0], df1.shape[1])

A mintatáblázat érték 14.096495 másodperc olvasható ideje.  
Sorok és oszlopok számát beolvasott = (1000-21).

### <a name="descriptive-statistics"></a>Leíró statisztika

Most már készen áll a mintában szereplő adatait. Azt kezdje az egyes leíró statisztika megjeleníti a **út\_távolság** (vagy bármely más olyan mezők, úgy dönt, hogy adja meg).

    df1['trip_distance'].describe()

### <a name="visualization-box-plot-example"></a>Megjelenítés: Mezőben ábra példa

Azt vizsgálja meg a Dobozdiagram az út távolságot a quantiles ábrázolásához.

    df1.boxplot(column='trip_distance',return_type='dict')

![#1 ábrázolása][1]

### <a name="visualization-distribution-plot-example"></a>Megjelenítés: Terjesztési ábra példa

Az felvételt, amely az eloszlás és a mintában szereplő utazás távolságokat a hisztogram ábrázolása.

    fig = plt.figure()
    ax1 = fig.add_subplot(1,2,1)
    ax2 = fig.add_subplot(1,2,2)
    df1['trip_distance'].plot(ax=ax1,kind='kde', style='b-')
    df1['trip_distance'].hist(ax=ax2, bins=100, color='k')

![#2 ábrázolása][2]

### <a name="visualization-bar-and-line-plots"></a>Megjelenítés: Sáv- és vonal ábra

Ebben a példában azt be öt intervallumokat utazás távolság bin és ábrázolása az binning eredményeket.

    trip_dist_bins = [0, 1, 2, 4, 10, 1000]
    df1['trip_distance']
    trip_dist_bin_id = pd.cut(df1['trip_distance'], trip_dist_bins)
    trip_dist_bin_id

Azt is rajzolja meg a fenti bin eloszlás egy sáv vagy az ábra sor:

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='bar')

![#3 ábrázolása][3]

és

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='line')

![#4 ábrázolása][4]

### <a name="visualization-scatterplot-examples"></a>Megjelenítés: Scatterplot példák

Pont ábra közötti bemutatjuk **út\_idő\_a\_másodperc** és **út\_távolság** van-e bármilyen korrelációs

    plt.scatter(df1['trip_time_in_secs'], df1['trip_distance'])

![#6 ábrázolása][6]

Hasonlóképpen azt ellenőrizheti a közötti kapcsolatot **ráta\_kód** és **út\_távolság**.

    plt.scatter(df1['passenger_count'], df1['trip_distance'])

![#8 ábrázolása][8]


### <a name="data-exploration-on-sampled-data-using-sql-queries-in-ipython-notebook"></a>Adatok feltárása a mintában szereplő adatok SQL lekérdezésekkel IPython jegyzetfüzetben

Ebben a részben mutatunk adatok terjesztését a mintában szereplő adatokat az új tábla feletti létrehozott megőrződnek használatával. Figyelje meg, hogy hasonló explorations végezhető el az eredeti tábla.

#### <a name="exploration-report-number-of-rows-and-columns-in-the-sampled-table"></a>Feltárása: Jelentés számú sort és oszlopot a mintában szereplő táblázatban

    nrows = pd.read_sql('''SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_sample>')''', conn)
    print 'Number of rows in sample = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''SELECT count(*) FROM information_schema.columns WHERE table_name = ('<nyctaxi_sample>') AND table_schema = '<schemaname>'''', conn)
    print 'Number of columns in sample = %d' % ncols.iloc[0,0]

#### <a name="exploration-tippednot-tripped-distribution"></a>Feltárása: Formabontó/nem azokat kioldják ki.

    query = '''
        SELECT tipped, count(*) AS tip_freq
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY tipped
        '''

    pd.read_sql(query, conn)

#### <a name="exploration-tip-class-distribution"></a>Feltárása: Ezt a tippet osztály ki.

    query = '''
        SELECT tip_class, count(*) AS tip_freq
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY tip_class
    '''

    tip_class_dist = pd.read_sql(query, conn)

#### <a name="exploration-plot-the-tip-distribution-by-class"></a>És magyarázat: A tipp eloszlás ábrázolni osztály

    tip_class_dist['tip_freq'].plot(kind='bar')

![#26 ábrázolása][26]


#### <a name="exploration-daily-distribution-of-trips"></a>És magyarázat: Utakat napi terjesztése

    query = '''
        SELECT CONVERT(date, dropoff_datetime) AS date, COUNT(*) AS c
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY CONVERT(date, dropoff_datetime)
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-per-medallion"></a>Feltárása: Utazás medallion egy terjesztési

    query = '''
        SELECT medallion,count(*) AS c
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY medallion
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-by-medallion-and-hack-license"></a>Feltárása: Utazás terjesztési medallion és támadó szoftver licenc szerint

    query = '''select medallion, hack_license,count(*) from <schemaname>.<nyctaxi_sample> group by medallion, hack_license'''
    pd.read_sql(query,conn)


#### <a name="exploration-trip-time-distribution"></a>És magyarázat: Üzenetváltási időt ki.

    query = '''select trip_time_in_secs, count(*) from <schemaname>.<nyctaxi_sample> group by trip_time_in_secs order by count(*) desc'''
    pd.read_sql(query,conn)

#### <a name="exploration-trip-distance-distribution"></a>Feltárása: Utazás távolság ki.

    query = '''select floor(trip_distance/5)*5 as tripbin, count(*) from <schemaname>.<nyctaxi_sample> group by floor(trip_distance/5)*5 order by count(*) desc'''
    pd.read_sql(query,conn)

#### <a name="exploration-payment-type-distribution"></a>Feltárása: Fizetési típus ki.

    query = '''select payment_type,count(*) from <schemaname>.<nyctaxi_sample> group by payment_type'''
    pd.read_sql(query,conn)

#### <a name="verify-the-final-form-of-the-featurized-table"></a>Ellenőrizze a featurized táblázat a végleges formában

    query = '''SELECT TOP 100 * FROM <schemaname>.<nyctaxi_sample>'''
    pd.read_sql(query,conn)

## <a name="mlmodel"></a>Az Azure gépi tanulási levő összeállítása

Azt most már készen áll a következő lépésben épület adatmodell és a modell telepítési az [Azure gépi tanulási](https://studio.azureml.net). A szövegbevitel problémáknak korábbi, azaz az használatra kész adatai:

1. **Bináris besorolás**: előrejelzésére engedélyezi vagy tiltja a tipp fizettek utazása.

2. **Multiclass besorolás**: előre kifizetett, az előzőleg definiált osztályok szerint tipp a cellatartományt.

3. **Regressziós tevékenység**: előre kifizetett utazása tipp mennyiségét.  



Modellezési gyakorlásához megkezdéséhez jelentkezzen be az **Azure gépi tanulási** munkaterület. Ha még nem hozott létre a gépi tanulási munkaterület, olvassa el [az Azure Machine Learning munkaterület létrehozása](machine-learning-create-workspace.md)című témakört.

1. Lásd: Ismerkedés az Azure gépi tanulási, [Mi az Azure gépi tanulási Studio?](machine-learning-what-is-ml-studio.md)

2. Jelentkezzen be az [Azure gépi tanulási Studio](https://studio.azureml.net).

3. A Studio kezdőlap információk, videókat, oktatóanyagok, hivatkozások különböző modulok hivatkozást, és más erőforrások: biztosít. Azure gépi tanulási kapcsolatos további tudnivalókért olvassa el az [Azure Machine Learning dokumentáció Center](https://azure.microsoft.com/documentation/services/machine-learning/).

Egy tipikus oktatás kísérlet a következő lépésekből áll:

1. Hozzon létre egy **+ Új** kísérlet.
2. A adatok lekérése az Azure Machine Learning.
3. Előre feldolgozása, átalakítás, és az adatok módosítására, szükség szerint.
4. Funkciók miatt, szükség szerint.
5. Ossza szét az adatok érvényesítése, képzés és tesztelése adatkészleteket (külön adatkészleteket, vagy az egyes).
6. Jelölje ki egy vagy több gépi tanulási algoritmusok attól függően, hogy a tanulási probléma megoldására. Például bináris besorolás multiclass besorolás, regressziós.
7. Az oktatóanyag adatkészlet használata egy vagy több modellek képzése.
8. Pontszám az érvényesítési adatkészlet képzett a modellek használatával.
9. Mérje fel a modellek számítja ki a megfelelő mérési módja miatt a tanulási probléma.
10. Finom finomhangolása a modellek, és jelölje ki a telepítendő legjobb modellt.

A következő gyakorlatban azt van már megismerkedett és megtervezett SQL adatraktár adatait, és kattintson a minta mérete úgy döntött, az Azure Machine Learning ingest. Az alábbiakban az eljárás szerint egy vagy több előrejelzési modellek létrehozása:

1. Az adatok beolvasása az Azure Machine Learning a az [Adatok importálása] [ import-data] modul érhető el a **bevitt adatok és a kimeneti** szakaszban. További tudnivalókért lásd: az [Adatok importálása] [ import-data] modul hivatkozás lap.

    ![Azure Machine Learning adatimportálás][17]

2. Jelölje be a **Tulajdonságok** panelen az **adatforrás** **Azure SQL-adatbázis** .

3. Az **adatbázis-kiszolgáló neve** mezőjében adja meg a DNS-adatbázis nevét. Formátum:`tcp:<your_virtual_machine_DNS_name>,1433`

4. A megfelelő mezőbe írja be az **adatbázis nevét** .

5. Írja be a *felhasználónevét az SQL* **Server felhasználói fiók nevét**, és a *jelszót* a **kiszolgáló felhasználói fiók jelszavának**.

6. Jelölje be a **bármely kiszolgálói tanúsítvány elfogadása** lehetőséget.

7. Az **adatbázis-lekérdezés** szerkesztése betűtípusú beillesztése a lekérdezést, amely kinyeri az ehhez szükséges adatbázismezők (például a címkék számított mezőket is beleértve), és lefelé minták az adatokat a kívánt minta mérete.

Példa egy bináris besorolás kísérlet közvetlenül a adatraktár SQL-adatbázisból adatok beolvasása az alábbi ábrán van (ne feledje, hogy a táblázat neve nyctaxi_trip és nyctaxi_fare helyettesíteni a séma neve és a táblázatok neve az útmutató használt). Hasonló kísérletek multiclass osztályozás és a regressziós problémákról is alakítható ki.

![Azure Machine Learning vonaton][10]

> [AZURE.IMPORTANT] Modellezési adatok kivonása és a lekérdezés példák mintavételi adni, az előző szakaszokban, **az összes címkét a három modellezési gyakorlatok szerepelnek a lekérdezést**. Egy fontos (kötelező) a modellezési gyakorlatok mindegyikében lépésként **kizárja** a szükségtelen címkék két problémákat, és minden más **cél elveszít**. Például bináris besorolás használatakor, használja a címke **Formabontó** és kizárja a mezők **Tipp\_osztály**, **Tipp\_összeg**, és **teljes\_összeg**. Az utóbbi cél szivárgást mivel azok szemléltetnek a tipp kifizetett.
>
> Bármely szükségtelen oszlopok kizárása vagy szivárgást cél, az [Oszlopok kiválasztása az adatkészlet] használhat[ select-columns] modul vagy a [Metaadatok szerkesztése][edit-metadata]. További tudnivalókért lásd: az [Oszlopok kiválasztása az adatkészlet] [ select-columns] és a [Metaadatok szerkesztése] [ edit-metadata] lapok hivatkozást.

## <a name="mldeploy"></a>Az Azure gépi tanulási levő terjesztése

Amikor készen áll a modell, egyszerűen telepítheti azt, közvetlenül a kísérlet a webszolgáltatás. Azure Machine Learning webes szolgáltatások telepítésével kapcsolatos további tudnivalókért olvassa el a [Deploy az Azure gépi tanulási webszolgáltatás](machine-learning-publish-a-machine-learning-web-service.md)című témakört.

Új webes szolgáltatás üzembe helyezéséhez kell:

1. Hozzon létre egy pontozási kísérlet.
2. A webszolgáltatás üzembe.

Egy **Befejezett** oktatás kísérlet egy pontozási kísérlet létrehozásához kattintson **Létrehozása pontozási KÍSÉRLETEZHET** alsó Műveletsáv.

![Azure pontozás][18]

Azure gépi tanulási megpróbálja hozzon létre egy pontozási kísérlet a oktatás kísérlet összetevői alapján. Mindenekelőtt lesz:

1. Mentse a képzett modellt, és távolítsa el a modell képzési modulok.
2. Egy logikai **bemeneti port** jelenítik meg a várt bemeneti adatok séma azonosításához.
3. Egy logikai **kimeneti port** jelenítik meg a várt web service kimeneti séma azonosításához.

A pontozási kísérlet létrehozásakor, tekintse át, és szükség szerint módosításával. Egy tipikus kiigazítás lecserélése a bemeneti adatkészlet és/vagy a lekérdezés egyik kizárja a címke mezők, mivel ezek nem használható, ha a szolgáltatás neve. Érdemes emellett a bemeneti adatkészlet és/vagy a lekérdezés méretének csökkentése néhány rekordokhoz javasolt csak elegendő, a bemeneti séma jelzése. A kimeneti port célszerű az összes beviteli mezők kihagyása, és csak a **Pontszáma címkéket** és a **Valószínűség pontszáma** szerepeltetni a kimenet az [Oszlopok kiválasztása az adatkészlet] használata közös[ select-columns] modulra.

Az alábbi ábrán egy kísérlet pontozási minta kapja. Ha készen áll a telepítéshez használni, kattintson az alsó műveletsávon a **WEBSZOLGÁLTATÁS közzététele** gombra.

![Azure Machine Learning közzététele][11]


## <a name="summary"></a>Összefoglalás
Recap mi azt befejezte az útmutató oktatóprogram, a létrehozott Azure adatok tudományos környezetben időadatok használata egy nagy nyilvános adatkészlet véve a folyamatát csapat adatok tudományos, teljes mértékben a modell oktatás adatbeszerzési, majd a az Azure gépi tanulási webszolgáltatás-példányhoz.

### <a name="license-information"></a>Licenc információ

Példa az útmutató és a hozzá tartozó parancsfájlok és IPython notebook(s) által megosztott Microsoft MIT-licenc csoportban. Ellenőrizze a LICENSE.txt fájl könyvtárában található további részleteket GitHub minta kódot.

## <a name="references"></a>Hivatkozások

• [Andrés Monroy a következőt: Taxi utakat letöltési oldalon](http://www.andresmh.com/nyctaxitrips/)  
• [a következőt: Taxi utazás adatok Chris Whong fOILing](http://chriswhong.com/open-data/foil_nyc_taxi/)   
• [Taxi a következőt: és Limousine jutalék kutatási és statisztika](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)


[1]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_26_1.png
[2]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_28_1.png
[3]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_35_1.png
[4]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_36_1.png
[5]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_39_1.png
[6]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_42_1.png
[7]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_44_1.png
[8]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_46_1.png
[9]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_71_1.png
[10]: ./media/machine-learning-data-science-process-sqldw-walkthrough/azuremltrain.png
[11]: ./media/machine-learning-data-science-process-sqldw-walkthrough/azuremlpublish.png
[12]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ssmsconnect.png
[13]: ./media/machine-learning-data-science-process-sqldw-walkthrough/executescript.png
[14]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sqlserverproperties.png
[15]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sqldefaultdirs.png
[16]: ./media/machine-learning-data-science-process-sqldw-walkthrough/bulkimport.png
[17]: ./media/machine-learning-data-science-process-sqldw-walkthrough/amlreader.png
[18]: ./media/machine-learning-data-science-process-sqldw-walkthrough/amlscoring.png
[19]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ps_download_scripts.png
[20]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ps_load_data.png
[21]: ./media/machine-learning-data-science-process-sqldw-walkthrough/azcopy-overwrite.png
[22]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-1.png
[23]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-2.png
[24]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-3.png
[25]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-4.png
[26]: ./media/machine-learning-data-science-process-sqldw-walkthrough/tip_class_hist_1.png


<!-- Module References -->
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
