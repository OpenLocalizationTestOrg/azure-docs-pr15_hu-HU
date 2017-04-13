<properties 
    pageTitle="Adatok áthelyezése SQL Azure-adatbázisból az Azure gépi tanulási |} Azure" 
    description="SQL-tábla és adatok betöltése SQL-táblázat létrehozása" 
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

# <a name="move-data-to-an-azure-sql-database-for-azure-machine-learning"></a>Az Azure gépi tanulási-adatok áthelyezése SQL Azure-adatbázisból

Ez a témakör ismerteti a beállításokat, az adatok áthelyezésére, strukturálatlan fájl (CSV- vagy TSV formátumok) vagy egy helyszíni SQL Server Azure SQL-adatbázishoz tárolt adatok. Ezeket a feladatokat, az adatok áthelyezése a felhőbe a csapat adatok tudományos folyamat részét képezik.

Ez a témakör ismerteti a beállításokat, az adatok áthelyezése egy helyszíni SQL Server-gépi tanulási olvassa el az [SQL Server Azure virtuális gépen-adatok áthelyezése](machine-learning-data-science-move-sql-server-virtual-machine.md)című témakört.

A következő **menü** tartalmazó témakörökre mutató hivatkozások ismertetik, hogyan lehet adatok ingest cél környezetekben, ahol az adatokat is tárolt és feldolgozása közben a csapat adatok tudományos folyamat (TDSP).

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

Az alábbi táblázat összefoglalja a beállításokat, az adatok áthelyezése SQL Azure-adatbázisból.

<b>FORRÁS</b> |<b>CÉL: Azure SQL-adatbázis</b> |
-------------- |--------------------------------|
<b>Strukturálatlan, egybesimított fájlhoz (CSV- vagy formázott TSV)</b> |<a href="#bulk-insert-sql-query">Tömeges beszúrás SQL-lekérdezés |
<b>Helyszíni SQL Server-</b> | 1. a <a href="#export-flat-file">strukturálatlan fájl exportálása<br> 2. a <a href="#insert-tables-bcp">SQL-adatbázis áttelepítése<br> 3. a <a href="#db-migration">vissza az adatbázis mentése és visszaállítása<br> 4. a <a href="#adf">azure Data Factory |


## <a name="prereqs"></a>Előfeltételek
Az itt ismertetett eljárások csak, hogy:

* **Azure előfizetés**. Ha nem rendelkezik az előfizetést, jelentkezzen az [ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/).
* **Azure tárterület-fiókot**. Az adatok tárolásának ebben az oktatóanyagban Azure tároló fiókot használ. Ha nincs Azure tárterület-fiókkal, ismertető [tároló fiók létrehozása](storage-create-storage-account.md#create-a-storage-account) . Miután létrehozta a tárterület-fiókot, kell szerezze be a fiókkulcs, amellyel elérhető a tár. Lásd: a [nézet, a másolás és a hívóbetűk újragenerálása tárhelyet](storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).
* Az **Azure SQL-adatbázis**eléréséhez. Be kell állítania egy Azure SQL-adatbázissal, ha [Az első lépések a Microsoft Azure SQL-adatbázis](../sql-database/sql-database-get-started.md) ismertetése kiépítését Azure SQL-adatbázis egy új példányát.
* Telepített és konfigurált **Azure PowerShell** helyi meghajtóra. Útmutatásért lásd: [Telepítse és állítsa be a Azure PowerShell](../powershell-install-configure.md).

**Adatok**: az áttelepítési folyamatot vannak mutatni a [következőt: Taxi adatkészlet](http://chriswhong.com/open-data/foil_nyc_taxi/)használata. A következőt: Taxi adatkészlet utazás adatok és vásárok adatait tartalmazza, és érhető el, amint azt a bejegyzést, a Azure blob-tárolóhoz: [Adatok a következőt: Taxi](http://www.andresmh.com/nyctaxitrips/). A minta és a leírás ezeket a fájlokat a [Következőt: Taxi utakat adatkészlet leírás](machine-learning-data-science-process-sql-walkthrough.md#dataset)találhatók.
 
Alkalmazkodás az itt leírt használatával egy a saját adatain eljárások, vagy kövesse a lépéseket a következőt: Taxi adatkészlet használatával leírt módon. A helyszíni SQL Server-adatbázisba a következőt: Taxi adatkészlet feltöltéséhez hajtsa végre a [Tömeges adatok beolvasása SQL Server-adatbázisba](machine-learning-data-science-process-sql-walkthrough.md#dbload)című témakörben ismertetett lépéseket. Ezeket az utasításokat egy SQL Server-Azure virtuális gép, de az eljárás a helyszíni SQL Server feltöltése nem azonos.


## <a name="file-to-azure-sql-database"></a>Adatok áthelyezése egy strukturálatlan, egybesimított fájlhoz forrásból származó Azure SQL-adatbázishoz

Egyszerű, strukturálatlan fájl (CSV- vagy TSV formázott) az adatokat áthelyezheti lekérdezéssel tömeges beszúrása SQL Azure SQL-adatbázishoz.

### <a name="bulk-insert-sql-query"></a>Tömeges beszúrás SQL-lekérdezés

A lépéseket a folyamathoz az tömeges beszúrása SQL-lekérdezés használatával hasonlók átütemezése adatok strukturálatlan fájlhoz adatforrásokból az SQL Server-Azure virtuális szakaszokban foglalkozik. A részletekért olvassa [Tömeges beszúrása SQL-lekérdezés](machine-learning-data-science-move-sql-server-virtual-machine.md#insert-tables-bulkquery).


##<a name="sql-on-prem-to-sazure-sql-database"></a>Adatok áthelyezése a helyszíni SQL Server Azure SQL-adatbázishoz

A forrásadatok egy helyszíni SQL Server tárolja, ha van különféle lehetőségek állnak rendelkezésére az adatok áthelyezése Azure SQL-adatbázisból:

1. [Egyszerű, strukturálatlan fájl exportálása](#export-flat-file) 
2. [SQL-adatbázis áttelepítése](#insert-tables-bcp)
3. [Vissza az adatbázis mentése és visszaállítása](#db-migration)
4. [Azure Data Factory](#adf)

A lépéseket az első három nagyban hasonlítanak ahhoz, hogy terjed ki ezeket a műveleteket szakaszokat az [adatok áthelyezése SQL Server Azure virtuális-gépen](machine-learning-data-science-move-sql-server-virtual-machine.md) . Témakör megfelelő szakaszai mutató hivatkozások találhatók az alábbi utasításokat.

###<a name="export-flat-file"></a>Egyszerű, strukturálatlan fájl exportálása

Exportáláshoz a strukturálatlan fájlhoz a lépések hasonlóak a [strukturálatlan fájl exportálása](machine-learning-data-science-move-sql-server-virtual-machine.md#export-flat-file)foglalt.

###<a name="insert-tables-bcp"></a>SQL-adatbázis áttelepítése

A lépéseket az SQL-adatbázis áttelepítése varázslóval hasonlók [SQL](machine-learning-data-science-move-sql-server-virtual-machine.md#sql-migration)-adatbázis áttelepítése varázslóban vonatkozik.

###<a name="db-migration"></a>Vissza az adatbázis mentése és visszaállítása

A lépéseket használatával adatbázis biztonsági mentése és visszaállítása hasonlók foglalt az [adatbázis biztonsági mentése és visszaállítása](machine-learning-data-science-move-sql-server-virtual-machine.md#sql-backup).

###<a name="adf"></a>Azure Data Factory

Az adatok áthelyezésére Azure SQL-adatbázishoz Azure adatok gyári (ADF) eljárás a témakörben [áthelyezése egy helyszíni SQL server Azure Data Factory az SQL Azure adatainak](machine-learning-data-science-move-sql-azure-adf.md)kapja. Ez a témakör mutatja adatok áthelyezése egy helyszíni SQL Server-adatbázisból Azure SQL-adatbázishoz keresztül Azure Blob-tárolóhoz ADF használatával. 

Fontolja meg inkább ADF adatokat kell a helyszíni és felhőbeli is erőforrások hozzáférő hibrid helyzetben folyamatosan áttelepített, valamint esetén az adatok feldolgozva van, vagy módosítani kell, és hozzáadja azt, amikor az áttelepítendő üzleti logikával rendelkeznek. ADF lehetővé teszi, hogy az ütemezés, és a feladatok parancsfájlokkal egyszerű JSON kezelő adatok rendszeres időközönkénti mozgásának nyomon követése. ADF szintén egyéb lehetőségek, például összetett műveletek támogatása.




