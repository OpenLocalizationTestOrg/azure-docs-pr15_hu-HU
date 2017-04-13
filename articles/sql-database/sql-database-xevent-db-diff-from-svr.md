<properties
    pageTitle="SQL-adatbázisban események bővített |} Microsoft Azure"
    description="Bővített események (XEvents) Azure SQL-adatbázisban, és hogyan esemény munkamenetek némileg eltérő Microsoft SQL Server-alapú esemény munkamenetek ismerteti."
    services="sql-database"
    documentationCenter=""
    authors="MightyPen"
    manager="jhubbard"
    editor=""
    tags=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/23/2016"
    ms.author="genemi"/>


# <a name="extended-events-in-sql-database"></a>Bővített események SQL-adatbázisban

[AZURE.INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

Ebből a témakörből megtudhatja mi a bővített események Azure SQL-adatbázisban végrehajtása az némileg eltérő Microsoft SQL Server-alapú bővített események képest.


- SQL-adatbázis V12 szerzett a bővített események funkció a második fele 2015-ös naptár.
- Az SQL Server 2008 óta bővített események volt.
- Az SQL Server funkciók robusztus az SQL-adatbázis bővített eseményeiről szolgáltatáskészlet része.


*XEvents* blogok és más kötetlen helyekre egy kötetlen becenév néha használt "bővített események" szerepel.


> [AZURE.NOTE] Október 2015, kezdve a bővített esemény munkamenet funkció Azure SQL-adatbázis előzetes szintre aktiválva van. Az általános elérhetőségű kiadás dátum még nem állította.
>
> Az Azure [Szolgáltatásfrissítések](https://azure.microsoft.com/updates/?service=sql-database) lap bejegyzéseket tartalmaz, ám hirdetmények végrehajtásakor.


További információ a bővített eseményeket, és a Microsoft SQL Server Azure SQL-adatbázis érhető el:

- [Rövid útmutató: Az SQL Server bővített események](http://msdn.microsoft.com/library/mt733217.aspx)
- [Bővített események](http://msdn.microsoft.com/library/bb630282.aspx)


## <a name="prerequisites"></a>Előfeltételek


Ez a témakör tartalma feltételezi, hogy már van néhány ismerete:


- [Azure SQL-adatbázis szolgáltatás](https://azure.microsoft.com/services/sql-database/).


- Microsoft SQL Server [kiterjesztett eseményeket](http://msdn.microsoft.com/library/bb630282.aspx) .
 - A bővített eseményekkel kapcsolatos dokumentációt tömeges SQL Server és a SQL-adatbázis vonatkozik.


Az alábbi elemek előzetes kitéve akkor hasznos, ha a [cél](#AzureXEventsTargets)az esemény fájl kiválasztása:


- [Azure tárhelyszolgáltatáshoz](https://azure.microsoft.com/services/storage/)


- A PowerShell
 - [Azure PowerShell használatá Azure adathordozós](../storage/storage-powershell-guide-full.md) - PowerShell és Azure tárhelyszolgáltatáshoz átfogó információt tartalmaz.


## <a name="code-samples"></a>Mintakódok


Kapcsolódó témakörök nyújtanak két mintakódok:


- [Csengetés, bővített események SQL-adatbázisban pufferelési cél kódját](sql-database-xevent-code-ring-buffer.md)
 - Rövid egyszerű Transact-SQL nyelvben parancsfájl.
 - Azt, hogy ha be szeretné fejezni a csengetés pufferelési cél, érdemes felengedi erőforrásait egy alter legördülő hajtja végre, a kód minta témakörben hangsúlyozni `ALTER EVENT SESSION ... ON DATABASE DROP TARGET ...;` utasítás. Később egy másik példányát csengetés pufferelési szerint hozzáadhat `ALTER EVENT SESSION ... ON DATABASE ADD TARGET ...`.


- [Fájl cél eseménykód SQL-adatbázisban bővített eseményekre vonatkozóan:](sql-database-xevent-code-event-file.md)
 - 1 fázis létrehozása az Azure tároló tároló PowerShell.
 - 2 fázis Transact-SQL nyelvben az Azure tároló tárolót használó.


## <a name="transact-sql-differences"></a>A Transact-SQL nyelvben különbségek


- A [Munkamenet esemény létrehozása](http://msdn.microsoft.com/library/bb677289.aspx) parancs SQL Server végrehajtásakor használhatja a **A kiszolgáló** záradék. De SQL-adatbázisban, használja a **ON adatbázis** záradék.


- A **Bekapcsolva adatbázis** záradék és is érinti a [Munkamenet ESEMÉNYRE ALTER](http://msdn.microsoft.com/library/bb630368.aspx) [HÚZHATJA esemény munkamenet](http://msdn.microsoft.com/library/bb630257.aspx) Transact-SQL-parancsok.


- Egy célszerű, amelyet fel szeretne venni az esemény munkamenet választógombot **STARTUP_STATE = ON** a **Munkamenet esemény létrehozása** vagy **ESEMÉNYRE ALTER munkamenet** kimutatásokban.
 - **= ON** értékét támogatja a egy automatikus újraindítása után a logikai adatbázis feladatátvevő miatt a konfigurálás.


## <a name="new-catalog-views"></a>Az új alkalmazáskatalógus-nézetek


A bővített események funkció több [katalógus nézetek](http://msdn.microsoft.com/library/ms174365.aspx)által támogatott. Katalógus nézetek tájékoztatása, a *metaadat-alapú vagy definíciók* a felhasználó által létrehozott események munkamenetek az aktuális adatbázisban. A nézetek adnak információt példányát aktív esemény munkamenetek.


| Neve<br/>katalógus megtekintése | Leírás |
| :-- | :-- |
| **sys.database_event_session_actions** | Az egyes műveletek sor egyes események esemény-munkamenet adja vissza. |
| **sys.database_event_session_events** | Az egyes események sor egy esemény munkamenet adja eredményül. |
| **sys.database_event_session_fields** | Egy sort a események és -célokat explicit módon beállított testreszabása – tud oszlopok ad eredményül. |
| **sys.database_event_session_targets** | Egy esemény munkamenet mindegyik esemény tárolóhoz sor számítja ki. |
| **sys.database_event_sessions** | Az SQL-adatbázis adatbázis minden esemény munkamenethez sor számítja ki. |


A Microsoft SQL Server hasonló katalógus nézeteket tartalmazó névvel rendelkeznek *.server\_ * helyett *.database\_*. A név minta **sys.server_event_%**hasonlít.


## <a name="new-dynamic-management-views-dmvshttpmsdnmicrosoftcomlibraryms188754aspx"></a>[(DMVs)](http://msdn.microsoft.com/library/ms188754.aspx) tekint meg az új dinamikus kezelése


Azure SQL-adatbázis [kezelése dinamikus nézetek (DMVs)](http://msdn.microsoft.com/library/bb677293.aspx) , amelyek támogatják a bővített események rendelkezik. DMVs mondani *aktív* esemény-munkamenetek.


| DMV nevét | Leírás |
| :-- | :-- |
| **sys.dm_xe_database_session_event_actions** | Ad az esemény munkamenet műveletek felvilágosítást. |
| **sys.dm_xe_database_session_events** | Információ az eseményekről munkamenet lekérdezése |
| **sys.dm_xe_database_session_object_columns** | Az objektumok, amelyek a munkamenet kötött konfigurációs értékeit mutatja. |
| **sys.dm_xe_database_session_targets** | Ad a munkamenet célok felvilágosítást. |
| **sys.dm_xe_database_sessions** | Egy sort minden esemény munkamenethez, hogy megfelelően változik az aktuális adatbázis ad eredményül. |


A Microsoft SQL Server hasonló katalógus nézetek nevű nélkül a * \_adatbázis* részét a nevét, például:


- **sys.dm_xe_sessions**név helyett<br/>**sys.dm_xe_database_sessions**.


### <a name="dmvs-common-to-both"></a>Gyakori mindkét DMVs


Bővített események vannak, amelyek megegyeznek a Azure SQL-adatbázis és a Microsoft SQL Server további DMVs:


- **sys.dm_xe_map_values**
- **sys.dm_xe_object_columns**
- **sys.dm_xe_objects**
- **sys.dm_xe_packages**



 <a name="sqlfindseventsactionstargets" id="sqlfindseventsactionstargets"></a>

## <a name="find-the-available-extended-events-actions-and-targets"></a>Keresse meg a rendelkezésre álló bővített eseményeket, műveletek és cél


Egy egyszerű SQL **Jelölje be** a rendelkezésre álló események, műveletek és cél listájának beszerzése futtathatja.


```
SELECT
        o.object_type,
        p.name         AS [package_name],
        o.name         AS [db_object_name],
        o.description  AS [db_obj_description]
    FROM
                   sys.dm_xe_objects  AS o
        INNER JOIN sys.dm_xe_packages AS p  ON p.guid = o.package_guid
    WHERE
        o.object_type in
            (
            'action',  'event',  'target'
            )
    ORDER BY
        o.object_type,
        p.name,
        o.name;
```



<a name="AzureXEventsTargets" id="AzureXEventsTargets"></a>

&nbsp;

## <a name="targets-for-your-sql-database-event-sessions"></a>Az SQL-adatbázis esemény munkamenetek cél


Az alábbiakban a cél, rögzítheti az esemény SQL-adatbázis-munkamenetek származó találatok jelenjenek meg:


- [Csengetés pufferelési cél](http://msdn.microsoft.com/library/ff878182.aspx) - eseményadatok röviden tárolja a memóriában.
- [Esemény számláló cél](http://msdn.microsoft.com/library/ff878025.aspx) - bővített események munkamenet során fellépő összes eseményének számolja meg.
- [Esemény fájl cél](http://msdn.microsoft.com/library/ff878115.aspx) - írások teljes puffer az Azure tároló tárolóhoz.


[Eseményvezérelt Tracing a Windows (esemény-nyomkövetés)](http://msdn.microsoft.com/library/ms751538.aspx) API az SQL-adatbázis bővített események számára nem érhető el.


## <a name="restrictions"></a>Korlátozások


Létezik néhány befitting a felhőalapú környezetben SQL-adatbázis biztonsági különbség:


- Bővített események alapja a egyetlen-bérlői elkülönítési modell. Egy esemény munkamenet egy adatbázist nem tud hozzáférni másik adatbázisból származó adatok vagy események.

- A **fő** adatbázist az környezetben **Munkamenet esemény létrehozása** kimutatást nem hiba.


## <a name="permission-model"></a>Jogosultsági modellhez


**Vezérlőelem** engedéllyel kell rendelkeznie az adatbázishoz az **Esemény létrehozása munkamenet** nyilatkozatot. Az adatbázis tulajdonosa (dbo) **hozzáférési** jogosultsággal rendelkezik.


### <a name="storage-container-authorizations"></a>Tárterület tároló engedélyek


A Társítások jogkivonat hoz létre az Azure tároló tároló **rwl** jogosultságok meg kell adnia. A **rwl** értéket tartalmaz, a következő engedélyeket:


- Olvasás
- Az írás
- Lista


## <a name="performance-considerations"></a>Teljesítménnyel kapcsolatos szempontok


Vannak olyan esetek, ahol a bővített események intenzív felhasználása gyűjteniük több aktív memóriát, mint a teljes rendszer megfelelő is. Az SQL Azure-adatbázis rendszer ezért dinamikusan állítja be, és megadja, hogy egy esemény munkamenet lehet halmozni aktív memória vonatkozó korlátok. Számos tényező dinamikus számítás ismertetőt találhat.


Ha olyan hibaüzenetet kap, amely szerint a maximális memóriaméret lett vannak érvényben, néhány korrekciós műveletek a következők:


- Futtassa a kevesebb egyidejű esemény-munkamenetek.


- Az esemény munkamenetek **létrehozása** és **módosítása** kimutatások, keresztül Rövidítse le a memória meg kell adnia a **MAX\_memória** záradék.


### <a name="network-latency"></a>Hálózati késés


Az **Esemény fájl** cél hálózati késés és a sikertelen adatait tároló Azure BLOB-való közben esetleg tapasztalható. Más események SQL-adatbázisban tarthat, miközben a hálózati kommunikáció befejezéséhez várnak. A késleltetés a terhelést is csökkentheti.

- A teljesítmény kockázat csökkentése érdekében, ne a **EVENT_RETENTION_MODE** a beállítást **NO_EVENT_LOSS** az eseményt a munkamenet-definíciók.


## <a name="related-links"></a>Kapcsolódó hivatkozások


- [Azure adathordozós Azure PowerShell használatával](../storage/storage-powershell-guide-full.md).
- [Azure tároló parancsmagok](http://msdn.microsoft.com/library/dn806401.aspx)


- [Azure PowerShell használatá Azure adathordozós](../storage/storage-powershell-guide-full.md) - PowerShell és Azure tárhelyszolgáltatáshoz átfogó információt tartalmaz.
- [A .NET Blob-tárolóhoz használata](../storage/storage-dotnet-how-to-use-blobs.md)


- [Hitelesítő adatok (a Transact-SQL nyelvben) létrehozása](http://msdn.microsoft.com/library/ms189522.aspx)
- [ESEMÉNY munkamenet (Transact-SQL nyelvben) létrehozása](http://msdn.microsoft.com/library/bb677289.aspx)


- [Jonathan Kehayias blogbejegyzéseket a Microsoft SQL Server-alapú bővített eseményekről](http://www.sqlskills.com/blogs/jonathan/category/extended-events/)


Az alábbi hivatkozásokat a bővített események minta kód témakörök érhetők el. Azonban rendszeresen ellenőrizni kell minden olyan minta kattintva megtekintheti, hogy a minta célként Microsoft SQL Server Azure SQL-adatbázis és. Ezután eldöntheti, hogy kisebb változások a minta futtatásához szükséges.


<!--
('lock_acquired' event.)

- Code sample for SQL Server: [Determine Which Queries Are Holding Locks](http://msdn.microsoft.com/library/bb677357.aspx)
- Code sample for SQL Server: [Find the Objects That Have the Most Locks Taken on Them](http://msdn.microsoft.com/library/bb630355.aspx)
-->
