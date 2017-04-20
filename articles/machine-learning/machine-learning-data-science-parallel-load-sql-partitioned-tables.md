<properties 
    pageTitle="Tömeges adatok importálása SQL partíciós táblák használata párhuzamos |}] A Microsoft Azure" 
    description="Párhuzamos tömeges adatok importálása SQL partíciós táblák használata" 
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
    ms.author="bradsev" /> 

# <a name="parallel-bulk-data-import-using-sql-partition-tables"></a>Párhuzamos tömeges adatok importálása SQL partíciós táblák használata

Ez a dokumentum ismerteti, hogyan gyors párhuzamos tömeges importálási adatok SQL Server adatbázis particionált táblák létrehozásához. Nagy terhelés/adatátvitelre SQL-adatbázisba adatok importálása SQL adatbázis-lekérdezések javítani lehet _particionálni táblák és nézetek_használatával. 


## <a name="create-a-new-database-and-a-set-of-filegroups"></a>Hozzon létre egy új adatbázist és a írhatósága

- [Új adatbázis létrehozása](https://technet.microsoft.com/library/ms176061.aspx) (Ha még nem létezik)
- Adatbázis írhatósága hozzáadni az adatbázishoz, amely fizikailag particionált fájlok

  Megjegyzés: Ezt megteheti, ha az új [Adatbázis létrehozása](https://technet.microsoft.com/library/ms176061.aspx) vagy [Módosítása](https://msdn.microsoft.com/library/bb522682.aspx) az adatbázis már létezik

- Egy vagy több fájl hozzáadása (szükség szerint) minden adatbázis-tároló fájlcsoport

 > [AZURE.NOTE] Adja meg a cél fájlcsoportban, a partíció adatait betöltő és tároló fájlcsoport adatokat tároló fizikai adatbázis fájl nevét.
 
A következő példa létrehoz egy új adatbázist a három írhatósága nem az elsődleges és a napló-csoportokat tartalmazó egyetlen fizikai fájlban minden. Az adatbázis fájlok jönnek létre az alapértelmezett SQL Server Data mappában, az SQL Server-példányt konfigurálni. További információt a fájlok alapértelmezett helyét lásd: [Alapértelmezett és az SQL Server példány nevű fájl helyét](https://msdn.microsoft.com/library/ms143547.aspx).

    DECLARE @data_path nvarchar(256);
    SET @data_path = (SELECT SUBSTRING(physical_name, 1, CHARINDEX(N'master.mdf', LOWER(physical_name)) - 1)
      FROM master.sys.master_files
      WHERE database_id = 1 AND file_id = 1);
    
    EXECUTE ('
        CREATE DATABASE <database_name>
        ON  PRIMARY 
        ( NAME = ''Primary'', FILENAME = ''' + @data_path + '<primary_file_name>.mdf'', SIZE = 4096KB , FILEGROWTH = 1024KB ), 
        FILEGROUP [filegroup_1] 
        ( NAME = ''FileGroup1'', FILENAME = ''' + @data_path + '<file_name_1>.ndf'' , SIZE = 4096KB , FILEGROWTH = 1024KB ), 
        FILEGROUP [filegroup_2] 
        ( NAME = ''FileGroup1'', FILENAME = ''' + @data_path + '<file_name_2>.ndf'' , SIZE = 4096KB , FILEGROWTH = 1024KB ), 
        FILEGROUP [filegroup_3] 
        ( NAME = ''FileGroup1'', FILENAME = ''' + @data_path + '<file_name>.ndf'' , SIZE = 102400KB , FILEGROWTH = 10240KB ), 
        LOG ON 
        ( NAME = ''LogFileGroup'', FILENAME = ''' + @data_path + '<log_file_name>.ldf'' , SIZE = 1024KB , FILEGROWTH = 10%)
    ')
    
## <a name="create-a-partitioned-table"></a>Particionált tábla létrehozása

Particionált megfelelően leképezve az adatbázis írhatósága az előző lépésben létrehozott séma létrehozni. Ha adatok tömeges importálásakor a particionált táblák, rekordok közötti elosztása partíció rendszerben, a írhatósága az alább ismertetett.

**A partíciós tábla létrehozásához kell:**

- Értékek/határait minden egyes partíciós táblát, pl. szerepeltetni, havonta partíciók korlátozni meghatározzák [a partíciós függvény létrehozása](https://msdn.microsoft.com/library/ms187802.aspx) (néhány\_datetime\_mező) 2013-ben:

        CREATE PARTITION FUNCTION <DatetimeFieldPFN>(<datetime_field>)  
        AS RANGE RIGHT FOR VALUES (
            '20130201', '20130301', '20130401',
            '20130501', '20130601', '20130701', '20130801',
            '20130901', '20131001', '20131101', '20131201' )

- [Partíciós séma létrehozása](https://msdn.microsoft.com/library/ms179854.aspx) , amely minden partíció tartományt a partíciós függvény rendel egy fizikai fájlcsoportban, pl.:

        CREATE PARTITION SCHEME <DatetimeFieldPScheme> AS  
        PARTITION <DatetimeFieldPFN> TO (
        <filegroup_1>, <filegroup_2>, <filegroup_3>, <filegroup_4>,
        <filegroup_5>, <filegroup_6>, <filegroup_7>, <filegroup_8>,
        <filegroup_9>, <filegroup_10>, <filegroup_11>, <filegroup_12> )

  Tipp: A függvény séma szerint minden partíción érvényben lévő tartományok ellenőrzéséhez futtassa a következő lekérdezés:

        SELECT psch.name as PartitionScheme,
            prng.value AS ParitionValue,
            prng.boundary_id AS BoundaryID
        FROM sys.partition_functions AS pfun
        INNER JOIN sys.partition_schemes psch ON pfun.function_id = psch.function_id
        INNER JOIN sys.partition_range_values prng ON prng.function_id=pfun.function_id
        WHERE pfun.name = <DatetimeFieldPFN>

- [Particionált tábla létrehozása](https://msdn.microsoft.com/library/ms174979.aspx) (s) a következők szerint, az adatszerkezet és adja meg a tábla particionálása pl. partíció rendszer és a korlátozás mezőt:

        CREATE TABLE <table_name> ( [include schema definition here] )
        ON <TablePScheme>(<partition_field>)

További tudnivalókért lásd: [létrehozása particionálva táblák és indexek](https://msdn.microsoft.com/library/ms188730.aspx).


## <a name="bulk-import-the-data-for-each-individual-partition-table"></a>Tömeges importálási minden egyes partíciós tábla adatai

- BCP, TÖMEGES Beszúrás vagy más módszerekkel, például az [SQL kiszolgáló áttelepítése varázsló](http://sqlazuremw.codeplex.com/)használható. A példában a BCP módszert használja.

- [Az adatbázis módosításához](https://msdn.microsoft.com/library/bb522682.aspx) a minimálisra csökkentése érdekében a naplózás, pl. rezsi BULK_LOGGED tranzakció naplózási séma módosítása:

        ALTER DATABASE <database_name> SET RECOVERY BULK_LOGGED

- Adatok betöltése lebonyolításához, indítsa el a tömeges importálási műveletek párhuzamosan. Tippek meggyorsítása tömeges nagy adatok SQL Server adatbázisba történő importálását a című témakörben találhatók [kevesebb, mint 1 óra 1 TB betölteni](http://blogs.msdn.com/b/sqlcat/archive/2006/05/19/602142.aspx).

A következő PowerShell parancsfájl párhuzamos adatok betöltése a BCP segítségével példája.

    # Set database name, input data directory, and output log directory
    # This example loads comma-separated input data files
    # The example assumes the partitioned data files are named as <base_file_name>_<partition_number>.csv
    # Assumes the input data files include a header line. Loading starts at line number 2.

    $dbname = "<database_name>"
    $indir  = "<path_to_data_files>"
    $logdir = "<path_to_log_directory>"

    # Select authentication mode
    $sqlauth = 0
    
    # For SQL authentication, set the server and user credentials
    $sqlusr = "<user@server>"
    $server = "<tcp:serverdns>"
    $pass   = "<password>"

    # Set number of partitions per table - Should match the number of input data files per table
    $numofparts = <number_of_partitions>
       
    # Set table name to be loaded, basename of input data files, input format file, and number of partitions
    $tbname = "<table_name>"
    $basename = "<base_input_data_filename_no_extension>"
    $fmtfile = "<full_path_to_format_file>"
   
    # Create log directory if it does not exist
    New-Item -ErrorAction Ignore -ItemType directory -Path $logdir
      
    # BCP example using Windows authentication
    $ScriptBlock1 = {
       param($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $num)
       bcp ($dbname + ".." + $tbname) in ($indir + "\" + $basename + "_" + $num + ".csv") -o ($logdir + "\" + $tbname + "_" + $num + ".txt") -h "TABLOCK" -F 2 -C "RAW" -f ($fmtfile) -T -b 2500 -t "," -r \n
    }
    
    # BCP example using SQL authentication
    $ScriptBlock2 = {
       param($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $num, $sqlusr, $server, $pass)
       bcp ($dbname + ".." + $tbname) in ($indir + "\" + $basename + "_" + $num + ".csv") -o ($logdir + "\" + $tbname + "_" + $num + ".txt") -h "TABLOCK" -F 2 -C "RAW" -f ($fmtfile) -U $sqlusr -S $server -P $pass -b 2500 -t "," -r \n
    }
    
    # Background processing of all partitions
    for ($i=1; $i -le $numofparts; $i++)
    {
       Write-Output "Submit loading trip and fare partitions # $i"
       if ($sqlauth -eq 0) {
          # Use Windows authentication
          Start-Job -ScriptBlock $ScriptBlock1 -Arg ($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $i)
       } 
       else {
          # Use SQL authentication
          Start-Job -ScriptBlock $ScriptBlock2 -Arg ($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $i, $sqlusr, $server, $pass)
       }
    }
    
    Get-Job
    
    # Optional - Wait till all jobs complete and report date and time
    date
    While (Get-Job -State "Running") { Start-Sleep 10 }
    date


## <a name="create-indexes-to-optimize-joins-and-query-performance"></a>Illesztések és lekérdezési teljesítmény optimalizálása indexek létrehozása

- Modellezési adatok több táblából kiveszi, ha az illesztés a teljesítmény javítása érdekében az illesztés kulcsok létrehozása indexek.

- [Indexek létrehozása](https://technet.microsoft.com/library/ms188783.aspx) (fürtözött vagy nem fürtözött) a célcsoportkezelés pl. minden partíció egy fájlcsoportban:

        CREATE CLUSTERED INDEX <table_idx> ON <table_name>( [include index columns here] )
        ON <TablePScheme>(<partition)field>)
vagy,

        CREATE INDEX <table_idx> ON <table_name>( [include index columns here] )
        ON <TablePScheme>(<partition)field>)

 > [AZURE.NOTE] Választhatja az indexek tömeges az adatok importálása előtt hozza létre. Az index létrehozása a tömeges importálás előtt az adatok betöltése lelassulnak.


## <a name="advanced-analytics-process-and-technology-in-action-example"></a>Speciális Analytics folyamat és technológiai művelet példa

A Cortana Analytics folyamat segítségével nyilvános dataset végpont forgatókönyv példaként lásd [Cortana Analytics folyamatban lévő művelet: SQL Server használata](machine-learning-data-science-process-sql-walkthrough.md).
 
