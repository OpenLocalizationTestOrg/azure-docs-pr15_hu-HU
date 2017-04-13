<properties
    pageTitle="Kezelése és nyújtás adatbázis elhárítása |} Microsoft Azure"
    description="Megtudhatja, hogy miként kezelheti és nyújtás adatbázis – problémamegoldás."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/27/2016"
    ms.author="douglasl"/>

# <a name="manage-and-troubleshoot-stretch-database"></a>Kezelése és nyújtás adatbázis – problémamegoldás

Kezelése és hibaelhárítása Nyújtás adatbázis használhatja az eszközök és a cikkben ismertetett eljárások.

## <a name="manage-local-data"></a>Helyi adatok kezelése

### <a name="LocalInfo"></a>Infó helyi adatbázisok és a táblákkal kapcsolatosan a Nyújtás adatbázis engedélyezve
Nyissa meg a katalógus nézetek **sys.databases** és **sys.tables** Nyújtás kapcsolatos információ megjelenítéséhez\-engedélyezve van az SQL Server-adatbázisok és táblák. További című témakörben [sys.databases (Transact-SQL nyelvben)](https://msdn.microsoft.com/library/ms178534.aspx) és [sys.tables (Transact-SQL nyelvben)](https://msdn.microsoft.com/library/ms187406.aspx).

Mennyi hely megtekintéséhez a egy Nyújtás\-engedélyezett táblázat az SQL Server, futtassa a következő utasítást használja.

 ```tsql
USE <Stretch-enabled database name>;
GO
EXEC sp_spaceused '<Stretch-enabled table name>', 'true', 'LOCAL_ONLY';
GO
 ```
## <a name="manage-data-migration"></a>Adatok áttelepítése kezelése

### <a name="check-the-filter-function-applied-to-a-table"></a>Jelölje be a táblázat alkalmazott szűrő függvény
Nyissa meg a katalógus nézetet **sys.remote\_adatok\_archív\_táblák** , és jelölje be a értékét a **szűrő\_predikátumok** oszlop azonosítására használó Nyújtás adatbázis áttelepítése sorok jelölje ki a függvény. Ha az érték üres, a teljes táblázatot jogosult a telepíthető át. További információt a című témakörben talál [sys.remote_data_archive_tables (Transact-SQL nyelvben)](https://msdn.microsoft.com/library/dn935003.aspx) , és [Válassza a sorok szűrése függvény segítségével az áttelepítendő](sql-server-stretch-database-predicate-function.md).

### <a name="Migration"></a>Adatok áttelepítése állapotának ellenőrzése
Jelölje ki a tevékenységeket **|} Kitöltés |} Monitor** az SQL Server Management Studio Lync-adatok áttelepítése az adatbázis-figyelő Nyújtás adatbázis. További információt a című témakörben talál [Monitor és problémáinak megoldása adatok áttelepítése (Nyújtás adatbázis)](sql-server-stretch-database-monitor.md).

Nyissa meg a dinamikus szolgáló nézet **sys.dm\_db\_rda\_áttelepítési\_állapot** tekintheti meg, hány kötegekben és sornyi adatot áttelepítette.

### <a name="Firewall"></a>Adatok áttelepítése – problémamegoldás
Hibaelhárítási javaslatokkal, lásd: [Monitor és problémáinak megoldása adatok áttelepítése (Nyújtás adatbázis)](sql-server-stretch-database-monitor.md).

## <a name="manage-remote-data"></a>Távoli adatok kezelése

### <a name="RemoteInfo"></a>Távoli adatbázis és tábla Nyújtás adatbázis által használt kapcsolatos adatok beolvasása
Nyissa meg a katalógus nézetek **sys.remote\_adatok\_archív\_adatbázisok** és **sys.remote\_adatok\_archív\_táblák** a távoli adatbázis és tábla, amely az áttelepített adatokat tárolja kapcsolatos információ megjelenítéséhez. További című témakörben [sys.remote_data_archive_databases (Transact-SQL nyelvben)](https://msdn.microsoft.com/library/dn934995.aspx) és [sys.remote_data_archive_tables (Transact-SQL nyelvben)](https://msdn.microsoft.com/library/dn935003.aspx).

Mennyi hely megtekintéséhez Nyújtás engedélyező táblázat Azure, futtassa a következő utasítást használja.

 ```tsql
USE <Stretch-enabled database name>;
GO
EXEC sp_spaceused '<Stretch-enabled table name>', 'true', 'REMOTE_ONLY';
GO
 ```

### <a name="delete-migrated-data"></a>Törli az áttelepített adatokat  
Ha törölni szeretné az Azure már áttelepített adatokat, kövesse a [sys.sp_rda_reconcile_batch](https://msdn.microsoft.com/library/mt707768.aspx)ismertetett.

## <a name="manage-table-schema"></a>Táblázat séma kezelése

### <a name="dont-change-the-schema-of-the-remote-table"></a>A távoli táblázat a séma nem változnak.
Ne módosítsa a séma távoli Azure táblázat, amely egy SQL Server-tábla Nyújtás adatbázis konfigurálva van társítva. Mindenekelőtt ne módosítsa a nevet vagy egy oszlop adattípusát. A kitöltés adatbázis-szolgáltatás feltételezésekkel különböző a séma ismertetése a távoli táblázat az SQL Server-tábla sémájának viszonyított. Ha módosítja a távoli séma, nyújtás adatbázis működése leáll a módosított tábla.

### <a name="reconcile-table-columns"></a>Táblázat oszlopainak egyeztetése  
Ha véletlenül törölt oszlopokat a távoli táblából, futtassa a **sp_rda_reconcile_columns** hasábok hozzáadása a távoli tábla megtalálható a Nyújtás\-engedélyezve van az SQL Server-tábla, de nem a távoli táblában. Ha további információra olvassa el a [sys.sp_rda_reconcile_columns](https://msdn.microsoft.com/library/mt707765.aspx)című témakört.  

  > [!IMPORTANT] Amikor **sp_rda_reconcile_columns** létrehozza az oszlopokat, amelyeket a távoli táblából véletlenül törölni, állítsa vissza az adatokat, amely a korábban a törölt oszlopokat.

**sp_rda_reconcile_columns** nem oszlopok törlése a távoli tábla megtalálható a távoli táblázatban, de nem a Nyújtás\-engedélyezve van az SQL Server-tábla. Ha vannak olyan oszlopok a távoli Azure táblában, amely már nem szerepel a Nyújtás\-engedélyezve van az SQL Server táblázat, ezek a további oszlopok akadályozza meg, hogy Nyújtás adatbázis megfelelően működik. Tetszés szerint eltávolíthatja a további oszlopok manuálisan.  

## <a name="manage-performance-and-costs"></a>A teljesítmény és a költségek kezelése  

### <a name="troubleshoot-query-performance"></a>A lekérdezési teljesítmény – problémamegoldás
Kitöltés tartalmazó lekérdezések\-támogatású táblák várhatóan mint azok előtt Nyújtás számára engedélyezett a táblák lassan működik. A lekérdezési teljesítmény jelentősen csökkenti, olvassa el a következő lehetséges problémák.

-   Az Azure kiszolgáló tartalmaz az SQL Server másik földrajzi területhez tartozik? A beállítása Azure el szeretné helyezni az SQL Servert futtató csökkentheti a hálózati késés azonos földrajzi területhez tartozik.

-   A hálózati feltételek előfordulhat, hogy rendelkezik csökkent. Lépjen kapcsolatba a hálózati rendszergazda tudnivalókat a legutóbbi problémák vagy kimaradások.

### <a name="increase-azure-performance-level-for-resource-intensive-operations-such-as-indexing"></a>Az erőforrás Azure teljesítmény növelése\-intenzív műveleteket, például az indexelés
Ha össze, újraépítéséhez vagy nagy táblázatra, hogy be van állítva az adatbázis Nyújtás index átrendezése, és várhatóan sok nagyméretű lekérdezése az áttelepített adatok Azure-ban a megadott időszakban, érdemes megfontolni teljesítményével kapcsolatos a megfelelő távoli Azure-adatbázis a művelet időtartamára. További tudnivalókat a teljesítményszintjét és árak olvassa el az [SQL Server Nyújtás adatbázis árak](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/)című témakört.

### <a name="you-cant-pause-the-sql-server-stretch-database-service-on-azure"></a>Az SQL Server-adatbázishoz Nyújtás Azure szolgáltatás nem szüneteltetése  
 Győződjön meg arról, hogy ki a megfelelő teljesítmény és árak szintet. Ha az ideiglenes az adott erőforrás teljesítményszint növelheti\-időigényes művelet, visszaállíthatja az előző szintre, a művelet befejezése után. További tudnivalókat a teljesítményszintjét és árak olvassa el az [SQL Server Nyújtás adatbázis árak](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/)című témakört.  

## <a name="change-the-scope-of-queries"></a>A lekérdezés hatókörének módosítása  
 Kitöltés lekérdezéseket\-támogatású táblák visszatérési helyi és a távoli adatok alapértelmezés szerint. Lekérdezések hatókör minden lekérdezés az összes felhasználó számára, vagy csak egyetlen lekérdezés rendszergazda módosíthatja.  

### <a name="change-the-scope-of-queries-for-all-queries-by-all-users"></a>Az összes felhasználó összes lekérdezések lekérdezés hatókörének módosítása  
 Az összes felhasználó minden lekérdezés hatókörének módosítása, futtassa a tárolt eljárás **sys.sp_rda_set_query_mode**. Csökkentheti a hatókört csak a helyi adatok a lekérdezés, minden lekérdezés letiltása és visszaállítása az alapértelmezett. Ha további információra olvassa el a [sys.sp_rda_set_query_mode](https://msdn.microsoft.com/library/mt703715.aspx)című témakört.  

### <a name="queryHints"></a>A lekérdezések rendszergazda egyetlen lekérdezés hatókörének módosítása  
 Egy tag jelentkeztünk egyetlen lekérdezés hatókörének módosítása, vegye fel a * *WITH \( REMOTE_DATA_ARCHIVE_OVERRIDE = *érték* \)** lekérdezés tipp a SELECT utasítás a. A lekérdezés REMOTE_DATA_ARCHIVE_OVERRIDE tipp beállíthatja, hogy az alábbi értékeket.  
 -   **LOCAL_ONLY**. Csak a helyi adatok lekérdezéséhez.  

 -   **REMOTE_ONLY**. A lekérdezés csak távoli adatforrásból elemet.  

 -   **STAGE_ONLY**. A lekérdezés csak az adatokat a táblázat, ahol Nyújtás adatbázis többszakaszos áttelepítési használatára jogosult a sorok és az áttelepített sorok megtartja az áttelepítés után az adott időszakra vonatkozóan. A lekérdezés tipp módja a csak a lekérdezés az átmeneti tárolásra szolgáló táblázatban.  

Az alábbi lekérdezés például csak a helyi eredményt adja eredményül.  

 ```tsql  
 USE <Stretch-enabled database name>;
 GO
 SELECT * FROM <Stretch_enabled table name> WITH (REMOTE_DATA_ARCHIVE_OVERRIDE = LOCAL_ONLY) WHERE ... ;
 GO
```  

## <a name="adminHints"></a>Felügyeleti frissítéseket és törlése  
 Nem lehet FRISSÍTENI alapértelmezett áttelepítési használatára jogosult sorok törlése vagy sorokat, amelyeket már áttelepítette, kattintson a kitöltés\-engedélyezve van a táblázatot. Ha a probléma megoldásához, jelentkeztünk tagja futtatását is lehetővé teszi a frissítés, vagy a Törlés művelet hozzáadásával a * *WITH \( REMOTE_DATA_ARCHIVE_OVERRIDE = *érték* \)** lekérdezés tipp a kimutatáshoz. A lekérdezés REMOTE_DATA_ARCHIVE_OVERRIDE tipp beállíthatja, hogy az alábbi értékeket.  
 -   **LOCAL_ONLY**. Frissítés, vagy csak a helyi adatok törlése.  

 -   **REMOTE_ONLY**. Frissítés, vagy törölje a csak a távoli adatforrásból.  

 -   **STAGE_ONLY**. Frissítés, vagy törölje a csak az adatokat a táblázat, ahol Nyújtás adatbázis többszakaszos áttelepítési használatára jogosult a sorok és az áttelepített sorok megtartja az áttelepítés után az adott időszakra vonatkozóan.  

## <a name="see-also"></a>Lásd még:

[Monitor Nyújtás adatbázis](sql-server-stretch-database-monitor.md)

[Biztonsági másolat Nyújtás engedélyező adatbázisok](sql-server-stretch-database-backup.md)

[Adatbázisok Nyújtás engedélyező visszaállítása](sql-server-stretch-database-restore.md)
