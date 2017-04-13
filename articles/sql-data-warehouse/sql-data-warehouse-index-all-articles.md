<properties
    pageTitle="Az összes témakörök SQL adatraktár szolgáltatás |} Microsoft Azure"
    description="Táblázat összes témakörök az Azure szolgáltatás nevű SQL adatraktár http://azure.microsoft.com/documentation/articles/, a cím és leírás megtalálható."
    services="sql-data-warehouse"
    documentationCenter=""
    authors="barbkess"
    manager="jhubbard"
    editor="MightyPen"/>

<tags
    ms.service="sql-data-warehouse"
    ms.workload="sql-data-warehouse"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="barbkess"/>


# <a name="all-topics-for-azure-sql-data-warehouse-service"></a>Az összes témakörök Azure SQL-adatraktár szolgáltatás

Ez a témakör felsorolja minden témakör, amely közvetlenül az Azure **SQL-adatraktár** szolgáltatás vonatkozik. Kulcsszavak a weblap az aktuális érdeklődésre számot tartó témakörök keresése a **Ctrl + F**, használatával is kereshet.




## <a name="new"></a>Új

| &nbsp; | Cím | Leírás |
| --: | :-- | :-- |
| 1 | [SQL-adatraktár biztonsági másolatok](sql-data-warehouse-backups.md) | Tudjon meg többet az SQL-adatraktár beépített adatbázis biztonsági mentése, amely lehetővé teszi az Azure SQL-adatraktár visszaállítása visszaállítási pont vagy egy másik földrajzi területhez tartozik. |


## <a name="updated-articles-sql-data-warehouse"></a>Frissített cikkek, SQL adatraktár

Ez a szakasz tárgyak, amelyek a frissített nemrégiben, ha a frissítés nagy, vagy jelentős lett-e. Az egyes frissített cikk durva kódtöredékének, a hozzáadott markdown szöveg jelenik meg. A cikkek lettek frissítve a dátumtartomány **2016-08-22-es** való **2016 – 10-05**belül.

| &nbsp; | A cikk | Módosított szöveget, kódtöredékének | Mikor frissülnek |
| --: | :-- | :-- | :-- |
| 2 | [Adatok betöltése SQL adatraktár (PolyBase) az Azure blob-tárolóhoz](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md) | /-Bájt és fájlok kijelölése r.command, s.request_id, r.status, a Darabszám (eltérők input_name) nbr_files, mint nyomon követéséhez SZUM (s.bytes_processed) / 1024/1024 mint sys.dm_pdw_exec_requests r belső illesztés sys.dm_pdw_dms_external_work s r.request_id gb_processed = s.request_id WHERE r. címke = "CTAS: betöltés cso. DimProduct "OR r. címke = "CTAS: betöltés cso. FactOnlineSales' GROUP BY r.command, s.request_id, r.status ORDER BY nbr_files desc, gb_processed desc;  | 2016-09-07 |
| 3 | [SQL-adatraktár visszaállítása](sql-data-warehouse-restore-database-overview.md) | **Visszaállíthatok egy felfüggesztett adatraktár?** A felfüggesztett adatraktár visszaállításához kell először ismét online állapotba. Miután a adatraktár visszatérés online üzemmódba, ha hét napig visszaállítási pontok közül választhat. **Geo felesleges területére visszaállítása** Geo felesleges tárolására használja, ha a adatraktár visszaállíthatja a különböző földrajzi régióban párosított adatközpont. A adatraktár visszaáll az utolsó napi biztonsági másolatból. **Ütemterv visszaállítása** Adatbázis visszaállítása bármikor az utóbbi hét napban visszaállíthatja. Egy adott időpontban érvényes megkezdése négy és nyolc óránként és hét napig. Ha 7 napnál régebbi pillanatkép, lejárta, és a visszaállítási pont már nem érhető el. **Költségeket visszaállítása** A visszaállított adatraktár tároló díja van az Azure prémium tároló díjazott számlát kapni. Ha az egérmutatót a visszaállított adatraktár, az előfizetést terhelő az Azure prémium tároló díjazott tároló. A szüneteltetés előnye Ön nem költség | 2016-09-29 |





## <a name="get-started"></a>Első lépések

| &nbsp; | Cím | Leírás |
| --: | :-- | :-- |
| 4 | [Adatraktár Azure SQL-hitelesítés](sql-data-warehouse-authentication.md) | Azure SQL-adatraktár Azure Active Directory (AAD) és az SQL Server hitelesítést. |
| 5 | [Gyakorlati tanácsok az Azure SQL-adatraktár](sql-data-warehouse-best-practices.md) | Javaslatok és gyakorlati tanácsok közben tudnivalók az Azure SQL-adatraktár lehetőség a megoldások fejlesztésére. Ezek segít a sikeres. |
| 6 | [Adatraktár Azure SQL-illesztőprogramok](sql-data-warehouse-connection-strings.md) | Kapcsolati karakterlánc és adatraktár SQL-illesztőprogramok |
| 7 | [Csatlakozás Azure SQL-adatraktár](sql-data-warehouse-connect-overview.md) | Keresheti meg a kiszolgáló neve és a kapcsolati karakterláncot az Azure SQL-adatraktár való |
| 8 | [Azure gépi tanulási adatok elemzése](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md) | Azure gépi tanulási létrehozásához használandó a cserélendő gépi tanulási modell Azure SQL-adatraktár tárolt adatok alapján. |
| 9 | [Lekérdezés Azure SQL-adatraktár (sqlcmd)](sql-data-warehouse-get-started-connect-sqlcmd.md) | Azure SQL-adatraktár lekérdezése a parancssori segédprogram sqlcmd együtt. |
| 10 | [Adatraktár SQL-adatbázis létrehozása a Transact-SQL nyelvben (TSQL) használatával](sql-data-warehouse-get-started-create-database-tsql.md) | Megtudhatja, hogy miként készíthet egy Azure SQL-adatraktár TSQL |
| 11 | [A támogatási jegyek SQL adatraktár létrehozása](sql-data-warehouse-get-started-create-support-ticket.md) | Hogyan lehet a támogatási jegyek létrehozása az Azure SQL-adatraktár. |
| 12 | [Adatok betöltése az Azure adatok gyár](sql-data-warehouse-get-started-load-with-azure-data-factory.md) | Ismerje meg, hogyan az Azure Data Factory adat betöltése |
| 13-mal | [Adatok betöltése és az SQL adatraktár PolyBase](sql-data-warehouse-get-started-load-with-polybase.md) | Ismerje meg, mi PolyBase és az esetek adattárolási használatához. |
| 14 | [Hozzon létre egy Azure SQL-adatraktár](sql-data-warehouse-get-started-provision.md) | Megtudhatja, hogy miként hozhat létre az Azure SQL-adatraktár az Azure-portálon |
| 15 | [Hozzon létre SQL adatraktár PowerShell használatával](sql-data-warehouse-get-started-provision-powershell.md) | Hozzon létre SQL adatraktár PowerShell használatával |
| 16 | [A Power BI adatok ábrázolása](sql-data-warehouse-get-started-visualize-with-power-bi.md) | A Power BI SQL adatraktár adatok ábrázolása |
| 17 | [Lekérdezés Azure SQL-adatraktár (Visual Studio)](sql-data-warehouse-query-visual-studio.md) | SQL-adatraktár lekérdezés a Visual Studio. |



## <a name="develop"></a>Kidolgozása

| &nbsp; | Cím | Leírás |
| --: | :-- | :-- |
| 18 | [Az SQL adatraktár tranzakciók optimalizálása](sql-data-warehouse-develop-best-practices-transactions.md) | Ajánlott gyakorlati tanácsokkal hatékony tranzakció frissítések írási az Azure SQL-adatraktár |
| 19 | [Az SQL adatraktár feldolgozási és a terhelést kezelése](sql-data-warehouse-develop-concurrency.md) | Feldolgozási és a terhelést-kezelés az Azure SQL-adatraktár megoldások fejlesztésére ismertetése. |
| 20 | [Táblázat létrehozása SQL adatraktár, jelölje be (CTAS)](sql-data-warehouse-develop-ctas.md) | Tippek kódolásának a tábla létrehozása az Azure SQL-adatraktár (CTAS) select utasítás mint megoldások fejlesztésére. |
| 21 | [Az SQL adatraktár dinamikus SQL](sql-data-warehouse-develop-dynamic-sql.md) | Tippek dinamikus SQL Azure SQL-adatraktár a megoldások fejlesztésével. |
| 22-es hibakód | [Csoportosítási szempont SQL adatraktár beállításai](sql-data-warehouse-develop-group-by-options.md) | Ötletek a csoport beállításai az Azure SQL-adatraktár megoldások fejlesztésével. |
| 23 | [SQL-adatraktár instrument lekérdezések címkék használata](sql-data-warehouse-develop-label.md) | Tippek az Azure SQL-adatraktár címkék instrument lekérdezések használatának megoldások fejlesztésére. |
| 24 | [Az SQL adatraktár hurkok](sql-data-warehouse-develop-loops.md) | Tippek a Transact-SQL nyelvben hurkok és a megoldások fejlesztésére az Azure SQL-adatraktár tagjára kurzorok. |
| 25 | [Az SQL adatraktár tárolt eljárások](sql-data-warehouse-develop-stored-procedures.md) | Ötletek a tárolt eljárásokat az Azure SQL-adatraktár megoldások fejlesztésével. |
| 26 | [Az SQL adatraktár tranzakciók](sql-data-warehouse-develop-transactions.md) | Ötletek a tranzakciók az Azure SQL-adatraktár megoldások fejlesztésével. |
| 27 | [SQL-adatraktár a felhasználó által definiált sémák](sql-data-warehouse-develop-user-defined-schemas.md) | Tippek a Transact-SQL nyelvben sémák az Azure SQL-adatraktár, a megoldások fejlesztésére. |
| 28 | [Az SQL adatraktár változók hozzárendelése](sql-data-warehouse-develop-variable-assignment.md) | Tippek a Transact-SQL Azure SQL-adatraktár változók kiosztása megoldások fejlesztésére. |
| 29 | [Az SQL adatraktár nézetek](sql-data-warehouse-develop-views.md) | Tippek a Transact-SQL nyelvben nézetek az Azure SQL-adatraktár, a megoldások fejlesztésére. |
| 30 | [Tervező döntéseket és az SQL adatraktár coding módszerek](sql-data-warehouse-overview-develop.md) | Fejlesztési fogalmak, tervezés döntések, javaslatok és SQL adatraktár coding technikákat. |



## <a name="manage"></a>Kezelése

| &nbsp; | Cím | Leírás |
| --: | :-- | :-- |
| 31. | [Az Azure SQL-adatraktár (áttekintés) számítási power kezelése](sql-data-warehouse-manage-compute-overview.md) | Teljesítmény skála funkciók az Azure SQL-adatraktár meg. Eredményhez tartozó DWUs méretezése, vagy mutasson az egérrel, és folytassa a számítási erőforrások költségek mentéséhez. |
| 32 | [Az Azure SQL-adatraktár (Azure portal) számítási power kezelése](sql-data-warehouse-manage-compute-portal.md) | Azure portál tevékenységek kezelése a power számítja ki. Méretezés erőforrások eredményhez tartozó DWUs számítja ki. Vagy mutasson az egérrel, és folytathatja a számítási erőforrások költségeinek mentése. |
| 33 | [Az Azure SQL-adatraktár (PowerShell) számítási power kezelése](sql-data-warehouse-manage-compute-powershell.md) | A tevékenységek kezelése a PowerShell power számítja ki. Méretezés erőforrások eredményhez tartozó DWUs számítja ki. Vagy mutasson az egérrel, és folytathatja a számítási erőforrások költségeinek mentése. |
| 34 | [Számítási power az Azure SQL adatok raktári (REST) kezelése](sql-data-warehouse-manage-compute-rest-api.md) | A tevékenységek kezelése a PowerShell power számítja ki. Méretezés erőforrások eredményhez tartozó DWUs számítja ki. Vagy mutasson az egérrel, és folytathatja a számítási erőforrások költségeinek mentése. |
| 35-tel | [Azure SQL-adatraktár (T-SQL) a számítási power kezelése](sql-data-warehouse-manage-compute-tsql.md) | Méretezési teljesítmény DWUs eredményhez tartozó tevékenységek Transact-SQL nyelvben (T-SQL). Mentse a költségek méretezéssel vissza nem csúcs időszakokban. |
| 36 | [Figyelheti a terhelést DMVs használatával](sql-data-warehouse-manage-monitor.md) | Megtudhatja, hogy miként figyelheti a terhelést DMVs használatával. |
| 37 | [Azure SQL-adatraktár az adatbázisok kezelése](sql-data-warehouse-overview-manage.md) | Áttekintése adatraktár SQL-adatbázisok kezelésében. DWUs és méretezési teljesítmény elérése érdekében a lekérdezési teljesítmény, jó biztonsági házirendek létrehozása és az adatbázis visszaállítása az adatvesztést vagy egy területi üzemszünetek hibaelhárítási-kezelő eszközök tartalmazza. |
| 38 | [Az Azure SQL-adatraktár felhasználói lekérdezés figyelése](sql-data-warehouse-overview-manage-user-queries.md) | A kapcsolatos szempontok ajánlott eljárások és az Azure SQL-adatraktár felhasználói lekérdezés figyelemmel kísérésére feladatok – áttekintés |
| 39 | [SQL-adatraktár visszaállítása](sql-data-warehouse-restore-database-overview.md) | Áttekintés: Azure SQL-adatraktár az adatbázis visszaállítása az adatbázis visszaállítása lehetőségek. |
| 40 | [Az Azure SQL-adatraktár (Portal) visszaállítása](sql-data-warehouse-restore-database-portal.md) | Azure portál feladatok az Azure SQL-adatraktár visszaállításához. |
| 41 | [Az Azure SQL-adatraktár (PowerShell) visszaállítása](sql-data-warehouse-restore-database-powershell.md) | A PowerShell feladatok az Azure SQL-adatraktár visszaállításához. |
| 42 | [Az Azure SQL-adatraktár (REST API-val) visszaállítása](sql-data-warehouse-restore-database-rest-api.md) | Az Azure SQL-adatraktár visszaállításához feladatok REST API-t. |



## <a name="tables-and-indexes"></a>Táblázatok és az indexek

| &nbsp; | Cím | Leírás |
| --: | :-- | :-- |
| 43 | [A táblákat az SQL adatraktár adattípusok](sql-data-warehouse-tables-data-types.md) | Első lépések az adattípusok Azure SQL-adatraktár táblákhoz. |
| 44 | [Az SQL adatraktár táblák terjesztése](sql-data-warehouse-tables-distribute.md) | Első lépések az Azure SQL-adatraktár táblák terjesztése. |
| 45 | [Az indexelés táblákat az SQL adatraktár](sql-data-warehouse-tables-index.md) | Első lépések az Azure SQL-adatraktár indexelés táblázat. |
| 46 | [Az SQL adatraktár táblázatok – áttekintés](sql-data-warehouse-tables-overview.md) | Első lépések az Azure SQL adattáblák raktári. |
| 47 | [Az SQL adatraktár táblák szétválasztás](sql-data-warehouse-tables-partition.md) | Első lépések az Azure SQL-adatraktár szétválasztás táblázat. |
| 48 | [A táblákat az SQL adatraktár statisztikák kezelése](sql-data-warehouse-tables-statistics.md) | Első lépések az Azure SQL-adatraktár táblázataira statisztikáját. |
| 49 | [Ideiglenes táblákat az SQL adatraktár](sql-data-warehouse-tables-temporary.md) | Első lépések az Azure SQL-adatraktár ideiglenes táblákat. |



## <a name="integrate"></a>Integráció

| &nbsp; | Cím | Leírás |
| --: | :-- | :-- |
| 50 | [Azure Data Factory használata SQL adatraktár](sql-data-warehouse-integrate-azure-data-factory.md) | Tippek az Azure adatok gyári (ADF) és az Azure SQL-adatraktár megoldások fejlesztésével. |
| 51 | [Azure gépi tanulási használata SQL adatraktár](sql-data-warehouse-integrate-azure-machine-learning.md) | Az Azure gépi tanulási és az Azure SQL-adatraktár megoldások fejlesztésével oktatóprogram. |
| 52 | [Azure Értékáram-elemzés használata SQL adatraktár](sql-data-warehouse-integrate-azure-stream-analytics.md) | Tippek az Azure Értékáram-elemzés és az Azure SQL-adatraktár megoldások fejlesztésével. |
| 53 | [A Power BI használata SQL adatraktár](sql-data-warehouse-integrate-power-bi.md) | Tippek a Power BI Azure SQL-adatraktár való, a megoldások fejlesztésére. |
| 54 | [Kihasználhatja az SQL adatraktár más szolgáltatások](sql-data-warehouse-overview-integrate.md) | Az eszközök és a partnerek SQL adatraktár integrálása megoldásokkal.  |



## <a name="load"></a>Betöltése

| &nbsp; | Cím | Leírás |
| --: | :-- | :-- |
| 55 | [Adatok betöltése Azure SQL-adatraktár (Azure Data Factory) az Azure blob-tárolóhoz](sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md) | Ismerje meg, hogyan az Azure Data Factory adat betöltése |
| 56 | [Adatok betöltése SQL adatraktár (PolyBase) az Azure blob-tárolóhoz](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md) | Megtudhatja, hogy miként Azure blob-tárolóhoz adatok betöltése SQL adatraktár PolyBase használatával. Néhány táblák nyilvános adatokból betöltése a Contoso kiskereskedelmi adatraktár sémában. |
| 57 | [Adatok betöltése Azure SQL-adatraktár (AZCopy) az SQL Server](sql-data-warehouse-load-from-sql-server-with-azcopy.md) | Adatok exportálása az SQL Server egyszerű fájlok AZCopy Azure blob-tárolóhoz adatokat importálhatja, és szeretné az adatokat ingest az Azure SQL-adatraktár PolyBase bcp használja. |
| 58 | [Adatok betöltése Azure SQL-adatraktár (egyszerű, strukturálatlan fájl) az SQL Server](sql-data-warehouse-load-from-sql-server-with-bcp.md) | A kis adatok méret használja bcp adatok exportálása az SQL Server strukturálatlan fájlokat, és importálja az adatokat közvetlenül Azure SQL-adatraktár. |
| 59 | [Adatok betöltése az SQL Server Azure SQL adatok raktári (SSIS) be](sql-data-warehouse-load-from-sql-server-with-integration-services.md) | Megtudhatja, hogy miként hozhat létre egy SQL Server Integration Services (SSIS) csomag számos különböző adatforrásból származó adatok áthelyezése SQL adatraktár. |
| 60 | [Adatok betöltése és az SQL adatraktár PolyBase](sql-data-warehouse-load-from-sql-server-with-polybase.md) | Adatok exportálása az SQL Server egyszerű fájlok AZCopy Azure blob-tárolóhoz adatokat importálhatja, és szeretné az adatokat ingest az Azure SQL-adatraktár PolyBase bcp használja. |
| 61 | [Útmutató az SQL adatraktár PolyBase használata](sql-data-warehouse-load-polybase-guide.md) | Útmutatások és javaslatok a SQL adatraktár esetek PolyBase használatához. |
| 62 | [Mintaadatok betöltése SQL adatraktár](sql-data-warehouse-load-sample-databases.md) | Mintaadatok betöltése SQL adatraktár |
| 63 | [Adatok betöltése a bcp](sql-data-warehouse-load-with-bcp.md) | Megtudhatja, milyen bcp, és az esetek adattárolási használatához. |
| 64 | [Adatok betöltése Azure SQL-adatraktár](sql-data-warehouse-overview-load.md) | Ismerje meg az SQL adatraktár betöltése adatok esetei. Ide tartoznak a PolyBase, Azure blob-tárolóhoz, strukturálatlan fájl vagy lemez szállítási használatával. Külső eszközök is használhatja. |



## <a name="migrate"></a>Áttelepítése

| &nbsp; | Cím | Leírás |
| --: | :-- | :-- |
| 65 | [Az SQL-kódot áttelepítése SQL adatraktár](sql-data-warehouse-migrate-code.md) | Tippek az áttelepítés az SQL-kódot az Azure SQL-adatraktár, a megoldások fejlesztésére. |
| 66 | [Az adatok áttelepítése](sql-data-warehouse-migrate-data.md) | Tippek az adatok áttelepítését Azure SQL-adatraktár megoldások fejlesztésével. |
| 67 | [Adatok raktári áttelepítési segédprogram (előzetes verzió)](sql-data-warehouse-migrate-migration-utility.md) | SQL-adatraktár áttelepítése. |
| 68 | [A séma áttelepítése SQL adatraktár](sql-data-warehouse-migrate-schema.md) | Tippek a séma áttelepítés Azure SQL-adatraktár megoldások fejlesztésével. |
| 69 | [A megoldás áttelepítése SQL adatraktár](sql-data-warehouse-overview-migrate.md) | Áttérési útmutató a Azure SQL-adatraktár platform, hogy a megoldás. |



## <a name="partners"></a>Partnerek

| &nbsp; | Cím | Leírás |
| --: | :-- | :-- |
| a 70 | [SQL-adatraktár üzleti intelligencia partnerek](sql-data-warehouse-partner-business-intelligence.md) | Külső üzleti intelligencia partnerek SQL adatraktár támogató megoldásokkal listája. |
| 71 | [SQL-adatraktár adatok integrálása partnerek](sql-data-warehouse-partner-data-integration.md) | Külső partnerek az Azure SQL-adatraktár támogató adatok integrálása megoldások listája. |
| 72 | [SQL-adatraktár adatok kezelése partnerek](sql-data-warehouse-partner-data-management.md) | Külső adatok kezelése partnerek SQL adatraktár támogató megoldásokkal listája. |



## <a name="reference"></a>Hivatkozás

| &nbsp; | Cím | Leírás |
| --: | :-- | :-- |
| 73 | [Az SQL adatraktár referenciaanyagokat](sql-data-warehouse-overview-reference.md) | Tartalom mutató hivatkozások találhatók az SQL adatraktár. |
| 74 | [PowerShell-parancsmagok és az SQL adatraktár REST API-khoz](sql-data-warehouse-reference-powershell-cmdlets.md) | Keresse meg a felső PowerShell-parancsmagok az Azure SQL adatraktár, például hogy miként mutasson az egérrel, és folytassa az adatbázis. |
| 75 | [Nyelvi elemek](sql-data-warehouse-reference-tsql-language-elements.md) | Az SQL adatraktár a Transact-SQL nyelv elemeket segédletekre mutató hivatkozások listája. |
| 76 | [A Transact-SQL nyelvben témakörök](sql-data-warehouse-reference-tsql-statements.md) | Az SQL adatraktár által használt Transact-SQL nyelvben témakörök a hivatkozási tartalmakra mutató hivatkozásokat talál. |
| 77 | [Rendszer-nézetek](sql-data-warehouse-reference-tsql-system-views.md) | Rendszer mutató hivatkozásokat tartalma SQL adatraktár nézeteinek. |



## <a name="security"></a>Biztonsági

| &nbsp; | Cím | Leírás |
| --: | :-- | :-- |
| 78 | [SQL adatraktár – a régebbi ügyfelek maszkolás naplózási és dinamikus adatok támogatása](sql-data-warehouse-auditing-downlevel-clients.md) | További tudnivalók: SQL adatraktár régebbi ügyfelek támogatása adatok naplózás |
| 79 | [Az SQL Azure-adatraktár naplózás](sql-data-warehouse-auditing-overview.md) | Első lépések az Azure SQL-adatraktár naplózás |
| 80 | [Az áttetsző adatok titkosítási (TDE) SQL adatraktár az első lépések](sql-data-warehouse-encryption-tde.md) | SQL-adatraktár átlátszó adatok titkosítás (TDE) |
| 81 | [Első lépések az áttetsző adatok titkosítási (TDE)](sql-data-warehouse-encryption-tde-tsql.md) | Áttetsző adatok titkosítás (TDE) SQL adatraktár (T – SQL) |
| 82 | [Az SQL adatraktár az adatbázis védelmét](sql-data-warehouse-overview-manage-security.md) | Tippek az Azure SQL-adatraktár adatbázisok biztonságossá megoldások fejlesztésére. |



## <a name="miscellaneous"></a>Vegyes

| &nbsp; | Cím | Leírás |
| --: | :-- | :-- |
| 83 | [Visual Studio 2015 és SSDT SQL adatraktár telepítése](sql-data-warehouse-install-visual-studio.md) | Az SQL Azure-adatraktár Visual Studio és az SQL Server Fejlesztőeszközök (SSDT) telepítése |
| 84 | [Áttelepítés az prémium tároló részletei](sql-data-warehouse-migrate-to-premium-storage.md) | Egy meglévő SQL adatraktár áttelepítésének prémium tárolóhoz utasítások |
| 85 | [Első lépések a kockázatot kimutatására](sql-data-warehouse-security-threat-detection.md) | Hogyan veszélyt észlelése a használata |
| 86 | [SQL-adatraktár kapacitás korlátai](sql-data-warehouse-service-capacity-limits.md) | A kapcsolatok, táblák és lekérdezések SQL adatraktár maximális értékek. |
| 87 | [Az SQL Azure-adatraktár – hibaelhárítás](sql-data-warehouse-troubleshoot.md) | Az SQL Azure-adatraktár hibaelhárítás. |

