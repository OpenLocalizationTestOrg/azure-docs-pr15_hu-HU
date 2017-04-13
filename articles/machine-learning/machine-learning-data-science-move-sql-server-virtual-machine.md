<properties 
    pageTitle="Adatok áthelyezése SQL Server Azure virtuális-gépen |} Azure" 
    description="Helyezze át az adatokat az egyszerű fájlok vagy egy helyszíni SQL Server SQL Server Azure virtuális." 
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
    ms.date="09/14/2016" 
    ms.author="bradsev" /> 

# <a name="move-data-to-sql-server-on-an-azure-virtual-machine"></a>Adatok áthelyezése SQL Server Azure virtuális-gépen

Ez a témakör ismerteti a beállításokat, az adatok áthelyezése a strukturálatlan fájl (CSV- vagy TSV formátumok) vagy egy helyszíni SQL Server SQL Server Azure virtuális-gépen. Ezeket a feladatokat, az adatok áthelyezése a felhőbe a csapat adatok tudományos folyamat részét képezik.

Ez a témakör a beállításokat, az adatok áthelyezése SQL Azure-adatbázisból a gépi tanulási ismertet [az Azure SQL-adatbázishoz Azure gépi tanulási az adatok áthelyezése](machine-learning-data-science-move-sql-azure.md)című témakör tartalmaz.

A **menü** alatti ismertetik, hogyan lehet adatok ingest más cél környezetekben, ahol tárolt és feldolgozása közben a csapat adatok tudományos folyamat (TDSP) a az adatokat tartalmazó témakörökre mutató hivatkozások.

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]


Az alábbi táblázat összefoglalja az adatok áthelyezése SQL Server Azure virtuális-gépen lehetőségeit.

<b>FORRÁS</b> |<b>CÉL: SQL Server Azure virtuális</b> |
------------------ |-------------------- |
<b>Strukturálatlan fájlhoz</b> |1. <a href="#insert-tables-bcp">a tömeges parancssori másolás segédprogram (BCP)</a><br> 2. <a href="#insert-tables-bulkquery">a tömeges beszúrás SQL-lekérdezés</a><br> 3. <a href="#sql-builtin-utilities">a grafikus beépített segédprogramok az SQL Server</a>
<b>A helyszíni SQL Server</b> | 1. <a href="#deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard">a Deploy a Microsoft Azure virtuális varázsló SQL Server-adatbázishoz</a><br> 2. <a href="#export-flat-file">a strukturálatlan fájl exportálása</a><br> 3. <a href="#sql-migration">SQL-adatbázis áttelepítése</a> <br> 4. <a href="#sql-backup">vissza az adatbázis mentése és visszaállítása</a><br>

Figyelje meg, hogy a dokumentum tartalma feltételezi, hogy SQL-parancsok végrehajtása az SQL Server Management Studio vagy Visual Studio adatbázis Explorer.

> [AZURE.TIP] Alternatív megoldásként [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) hozhat létre, és a folyamat, amely fog helyezze át az adatokat egy SQL Server virtuális gép Azure ütemezése is használhatja. További tudnivalókért olvassa el a [Azure Data Factory (Másolás tevékenység) az adatok másolása](../data-factory/data-factory-data-movement-activities.md)című témakört.


## <a name="prereqs"></a>Előfeltételek
Ebben az oktatóanyagban feltételezi, hogy van:

* **Azure előfizetés**. Ha nem rendelkezik az előfizetést, jelentkezzen az [ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/).
* **Azure tárterület-fiókot**. Azure tároló fiók használni az adatok tárolása ebben az oktatóanyagban. Ha nincs Azure tárterület-fiókkal, ismertető [tároló fiók létrehozása](../storage/storage-create-storage-account.md#create-a-storage-account) . Miután létrehozta a tárterület-fiókot, szüksége lesz szerezze be a fiókkulcs, amellyel elérhető a tár. Lásd: a [nézet, a másolás és a hívóbetűk újragenerálása tárhelyet](../storage/storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).
* **SQL Server-Azure virtuális**kiépített. Című cikkben olvashat [az Azure SQL Server virtuális gép az fejlett analitikai IPython jegyzetfüzet kiszolgáló beállítása](machine-learning-data-science-setup-sql-server-virtual-machine.md).
* Telepített és konfigurált **Azure PowerShell** helyi meghajtóra. Útmutatásért lásd: [Telepítse és állítsa be a Azure PowerShell](../powershell-install-configure.md).


## <a name="filesource_to_sqlonazurevm"></a>Adatok áthelyezése a strukturálatlan fájlhoz forrásból származó SQL Server-Azure virtuális a

Ha az adatok egy strukturálatlan, egybesimított fájlhoz (sor/oszlop formátumban vannak rendezve) van, azt áthelyezhető SQL Server virtuális Azure a keresztül az alábbi módszereket:

1. [Parancssori tömeges másolás segédprogram (BCP)](#insert-tables-bcp) 
2. [Tömeges beszúrás SQL-lekérdezés](#insert-tables-bulkquery)
3. [A grafikus beépített segédprogramok az SQL Server (importálás/exportálás, SSIS)](#sql-builtin-utilities)


### <a name="insert-tables-bcp"></a>Parancssori tömeges másolás segédprogram (BCP)

BCP telepíteni az SQL Server-parancssori segédprogram, és annak a leggyorsabb módja az adatok áthelyezése. Az összes három SQL Server változat (helyszíni SQL Server, az SQL Azure és az SQL Server Azure virtuális gép) keresztül működik. 

> [AZURE.NOTE]**Hol az adatok legyen az BCP?**  
> Bár nem szükséges, a cél SQL server ugyanazon a gépen található adatforrás-adatokat tartalmazó fájlok lehetővé, hogy gyorsabban átvitelek (hálózat sebességétől viewben helyi lemez IO sebesség). Áthelyezheti a strukturálatlan fájlokat a számítógépen adatokat tartalmazó SQL Server különböző fájl másolása eszközök, például [AZCopy](../storage/storage-use-azcopy.md)használatával is telepítve van, ahol [Azure tároló Explorer](http://storageexplorer.com/) vagy a windows beillesztéséhez keresztül távoli asztali Protocol (RDP).

1. Győződjön meg arról, hogy az adatbázist és táblákat létre a cél SQL Server-adatbázishoz. Íme egy példa bemutatja, hogyan végezze el az adott használatával a `Create Database` és `Create Table` parancsokat:

        CREATE DATABASE <database_name>
    
        CREATE TABLE <tablename>
        (
            <columnname1> <datatype> <constraint>,
            <columnname2> <datatype> <constraint>,
            <columnname3> <datatype> <constraint>
        ) 
    
2. A formátum fájl létrehozása a következő parancsot a parancssorból a gép a séma el a táblázatot leíró hol bcp telepítve van.

    `bcp dbname..tablename format nul -c -x -f exportformatfilename.xml -S servername\sqlinstance -T -t \t -r \n` 

3. Az adatok beillesztése az adatbázist a bcp parancs használatával az alábbiak szerint. Feltételezve, hogy az SQL Server ugyanarra a gépre van telepítve a kell működniük a parancssorból:

    `bcp dbname..tablename in datafilename.tsv -f exportformatfilename.xml -S servername\sqlinstancename -U username -P password -b block_size_to_move_in_single_attemp -t \t -r \n`

> **Optimalizálás BCP beszúrása** Olvassa el a következő cikk ["Optimalizálása tömeges importálás útmutatás"](https://technet.microsoft.com/library/ms177445%28v=sql.105%29.aspx) optimalizálhatja az ilyen szúr be.


### <a name="insert-tables-bulkquery-parallel"></a>Gyorsabb adatok mozgását parallelizing a beszúrása

Ha az áthelyezni kívánt adatok túl nagy, gyorsabban bekapcsolódhat dolog, amit egyszerre egy PowerShell-parancsprogramot, párhuzamosan hajtja végre több BCP parancsot.

> [AZURE.NOTE]**Nagy adatok bevitel** Optimalizálhatja az adatok betöltésének nagy és nagyméretű adatkészletek, partition fizikai, logikai és adatbázistáblákban több írhatósága és partíciót tábláinak használatával. Létrehozása és adatok partíciót táblák betöltése kapcsolatos további tudnivalókért olvassa el a [Párhuzamos betöltés SQL partíciót táblák](machine-learning-data-science-parallel-load-sql-partitioned-tables.md)című témakört.


Az alábbi példa PowerShell parancsprogramot bemutatják a párhuzamos beszúrása bcp használata:
    
    $NO_OF_PARALLEL_JOBS=2

     Set-ExecutionPolicy RemoteSigned #set execution policy for the script to execute
     # Define what each job does
       $ScriptBlock = {
           param($partitionnumber)

           #Explictly using SQL username password
           bcp database..tablename in datafile_path.csv -F 2 -f format_file_path.xml -U username@servername -S tcp:servername -P password -b block_size_to_move_in_single_attempt -t "," -r \n -o path_to_outputfile.$partitionnumber.txt

            #Trusted connection w.o username password (if you are using windows auth and are signed in with that credentials)
            #bcp database..tablename in datafile_path.csv -o path_to_outputfile.$partitionnumber.txt -h "TABLOCK" -F 2 -f format_file_path.xml  -T -b block_size_to_move_in_single_attempt -t "," -r \n 
      }
    

    # Background processing of all partitions
    for ($i=1; $i -le $NO_OF_PARALLEL_JOBS; $i++)
    {
      Write-Debug "Submit loading partition # $i"
      Start-Job $ScriptBlock -Arg $i      
    }
    

    # Wait for it all to complete
    While (Get-Job -State "Running")
    {
      Start-Sleep 10
      Get-Job
    }
    
    # Getting the information back from the jobs
    Get-Job | Receive-Job
    Set-ExecutionPolicy Restricted #reset the execution policy


### <a name="insert-tables-bulkquery"></a>Tömeges beszúrás SQL-lekérdezés

[Tömeges beszúrása SQL-lekérdezés](https://msdn.microsoft.com/library/ms188365) is használható, amellyel adatokat importál az adatbázist a sor/oszlop alapján fájlokat (a támogatott fájltípusok[Adatok előkészítése tömeges exportálása vagy importálása (SQL Server)](https://msdn.microsoft.com/library/ms188609)tartoznak) témakört. 

Íme néhány példa parancsok tömeges beszúrása alábbi is:  

1. Az adatok elemzése, és adja az egyéni beállításokat előtt győződjön meg arról, hogy az SQL Server-adatbázis feltételezi, hogy ugyanazt a formátumot bármely speciális mezők – például a dátumok az importálás. Íme egy példa, hogy miként állítható be a dátumformátumot év napos hónap szerint, (ha az adatok tartalmaz, a dátum év, hónap-nap formátumban):

        SET DATEFORMAT ymd; 
    
2. Adatok importálása a tömeges használata kimutatások importálása:

        BULK INSERT <tablename>
        FROM    
        '<datafilename>'
        WITH 
        (
        FirstRow=2,
        FIELDTERMINATOR =',', --this should be column separator in your data
        ROWTERMINATOR ='\n'   --this should be the row separator in your data
        )
      

### <a name="sql-builtin-utilities"></a>Az SQL Server beépített segédprogramok

Adatok importálása az SQL Server Azure strukturálatlan fájlhoz a virtuális gép használhatja az SQL Server integrációs Services (SSIS). Az SSIS két studio környezetben érhető el. A részletekért olvassa [Integration Services (SSIS) és a Studio környezetben](https://technet.microsoft.com/library/ms140028.aspx):

- Az SQL Server Data Tools a részletekért olvassa el [A Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/tools.aspx)  
- Az importálás és exportálás varázslót a részletekért olvassa el [az SQL Server Importálás és exportálás varázslóban](https://msdn.microsoft.com/library/ms141209.aspx)


## <a name="sqlonprem_to_sqlonazurevm"></a>Adatok áthelyezése a helyszíni SQL Server SQL Server-Azure virtuális a

A következő áttelepítési stratégiák is használhatja:

1. [Telepítse a Microsoft Azure virtuális varázsló SQL Server-adatbázishoz](#deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard)
2. [Egyszerű, strukturálatlan fájl exportálása](#export-flat-file) 
3. [SQL-adatbázis áttelepítése](#sql-migration)
4. [Vissza az adatbázis mentése és visszaállítása](#sql-backup)

Azt ismertetik az alábbiak mindegyike, az alábbi:

### <a name="deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard"></a>Telepítse a Microsoft Azure virtuális varázsló SQL Server-adatbázishoz

Az **SQL Server-adatbázishoz a Microsoft Azure virtuális varázsló Deploy** módja egy egyszerű és ajánlott áthelyezése adatok egy helyszíni SQL Server-példányt az SQL Server-Azure virtuális. A lépések részletes leírását, valamint az egyéb alternatívák vitafórum [SQL Server-Azure virtuális az adatbázis áttelepítése](../virtual-machines/virtual-machines-windows-migrate-sql.md)talál.

### <a name="export-flat-file"></a>Egyszerű, strukturálatlan fájl exportálása

Különböző módszerek az adatok exportálása egy helyszíni SQL Server tömegesen, a [tömeges adatok importálása és exportálása a (SQL Server)](https://msdn.microsoft.com/library/ms175937.aspx) témakörben ismertetett módon használható. A dokumentum példaként a tömeges másolás Program (BCP) terjed ki. Miután az adatokat exportálni szeretné strukturálatlan fájlhoz, importálhatók másik SQL server tömeges importálással. 

1. Az adatok exportálása a helyszíni SQL Server egy fájlt, az alábbi képlettel történik a bcp segédprogrammal

    `bcp dbname..tablename out datafile.tsv -S  servername\sqlinstancename -T -t \t -t \n -c`

2. Az adatbázis és a tábla létrehozása az SQL Server virtuális a Azure használatáról a `create database` és `create table` a táblázat séma exportált lépés: 1.

3. Az adatok exportálása és importálása táblázat sémájának leíró formátumú fájl létrehozása. Az fájlformátumának részleteit [(SQL Server) formátumú fájl létrehozása](https://msdn.microsoft.com/library/ms191516.aspx)témakörben olvashat.

    Fájl létrehozása formázása amikor fut BCP az SQL Server számítógépről 

        bcp dbname..tablename format nul -c -x -f exportformatfilename.xml -S servername\sqlinstance -T -t \t -r \n 

    Fájl létrehozása formázása BCP Ha távolról futó SQL Server 

        bcp dbname..tablename format nul -c -x -f  exportformatfilename.xml  -U username@servername.database.windows.net -S tcp:servername -P password  --t \t -r \n
        
    
4. Az [Áthelyezés adatforrásból származó fájl](#filesource_to_sqlonazurevm) szakaszban ismertetett eljárások valamelyikét segítségével helyezze át az adatokat egy SQL Server strukturálatlan fájlokban.

### <a name="sql-migration"></a>SQL-adatbázis áttelepítése

[SQL Server-adatbázis áttelepítése varázsló](http://sqlazuremw.codeplex.com/) felhasználóbarát biztosítja az adatok áthelyezése két SQL server-példányok között. A felhasználó feleltesse meg az adatok séma források és céltáblák között, válassza az oszlopban található információ típusa és számos más funkciókkal. Akkor használja a tömeges másolása (BCP) csoportban a vonatkozik. Az üdvözlőképernyő az SQL-adatbázis áttelepítése varázsló Képernyőkép az alábbiakban látható.  

![SQL Server áttelepítési varázsló][2]

### <a name="sql-backup"></a>Vissza az adatbázis mentése és visszaállítása

Az SQL Server támogatja: 

1. [Vissza az adatbázis mentése és visszaállítása funkciók](https://msdn.microsoft.com/library/ms187048.aspx) (helyi fájl vagy bacpac mindkét exportálása blob) és az [Adatok réteg alkalmazások](https://msdn.microsoft.com/library/ee210546.aspx) (bacpac). 
2. Azt jelenti, hogy közvetlenül létrehozása az SQL Server VMs Azure a másolt adatbázis vagy másolása a meglévő SQL Azure-adatbázisokhoz. További részletekért olvassa el [a varázslóval másolás adatbázis](https://msdn.microsoft.com/library/ms188664.aspx). 

Az adatbázis biztonsági képernyőkép felfelé/visszaállítása az SQL Server Management Studio beállítások alább látható módon.

![Az SQL Server importáló eszköz][1]

## <a name="resources"></a>Erőforrások

[Adatbázis áttelepítése az SQL Server-Azure virtuális](../virtual-machines/virtual-machines-windows-migrate-sql.md)

[SQL Server Azure virtuális gépeken futó – áttekintés](../virtual-machines/virtual-machines-windows-sql-server-iaas-overview.md)

[1]: ./media/machine-learning-data-science-move-sql-server-virtual-machine/sqlserver_builtin_utilities.png
[2]: ./media/machine-learning-data-science-move-sql-server-virtual-machine/database_migration_wizard.png
