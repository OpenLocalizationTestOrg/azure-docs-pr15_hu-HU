<properties
   pageTitle="A felhő üzemkészségének - törölt adatbázis - SQL-adatbázis visszaállítása |} Microsoft Azure"
   description="Tudjon meg többet a pont és az idő visszaállítása, amely lehetővé teszi, hogy visszaállíthatja az Azure SQL-adatbázis egy előző pont időben (legfeljebb 35 nap)."
   services="sql-database"
   documentationCenter=""
   authors="stevestein"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/01/2016"
   ms.author="sstein"/>

# <a name="recover-an-azure-sql-database-using-automated-database-backups"></a>Az Azure SQL-adatbázisokkal automatikus adatbázis biztonsági mentése helyreállítása

SQL-adatbázis használata [SQL-adatbázis automatikus biztonsági mentés](sql-database-automated-backups.md)adatbázis-helyreállítás három lehetőségeket is biztosít. A szolgáltatás által kezdeményezett biztonsági mentés adatbázis bármikor visszaállíthatja az [adatmegőrzési időszak](sql-database-service-tiers.md) alatt.

- Új adatbázis egy meghatározott pontra az adatmegőrzési időszak időbeli helyreállított azonos logikai kiszolgálón. 
- Ugyanazon a kiszolgálón logikai törölt adatbázis törlése ideje helyreállított adatbázis.
- Új adatbázis bármely logikai kiszolgálón bármilyen helyre a legutóbbi napi biztonsági másolatok geo replikált blob-tárolóhoz (TS-GRS) területen.

Is használhatja [az automatikus biztonsági mentés SQL-adatbázis](sql-database-automated-backups.md) kiszolgálón hozza létre a egy [adatbázis másolása](sql-database-copy.md) bármely logikai tranzakción keresztül konzisztens az aktuális SQL-adatbázis bármelyik tartományban lévő. Adatbázis-példányt, és [egy BACPAC exportálása](sql-database-export.md) is használhatja a archiválása az adatmegőrzési időszak túl hosszú távú tárolására adatbázis tranzakción keresztül egységes másolatát, illetve egy másolatot az adatbázisról átvitele egy helyszíni, vagy az SQL Server Azure virtuális példányát.

## <a name="recovery-time"></a>Helyreállítási idő

Számos tényező hatással van a helyreállítási idő automatikus adatbázis biztonsági mentése használatával adatbázis visszaállítása: 
 - Az adatbázis mérete
 - Az adatbázis teljesítményével
 - Az érintett tranzakció naplók száma
 - Lejátszás, újra állíthatja helyre a visszaállítási pontra kell kell, hogy a tevékenység összege
 - A hálózati sávszélességet, ha a visszaállítás más területére 
 - A cél régióban feldolgozása egyidejű visszaállítási kérelmek száma. 
 
 A visszaállítás nagyon nagy, illetve az aktív adatbázis több óráig is eltarthat. Ha egy tartomány hosszan üzemszünetek, akkor lehet, hogy az egyéb régiók által feldolgozott Geo-visszaállítási kérelmek nagyszámú lesz. Ha nagyszámú Ez növelheti a helyreállítási időpontot tudja kiválasztani az adott régióban adatbázisok kérések. A legtöbb adatbázis visszaállítása teljes 12 órán belül.

 Tömeges visszaállítása nem beépített funkció nem. A [Azure SQL-adatbázis: a teljes kiszolgáló helyreállítási](https://gallery.technet.microsoft.com/Azure-SQL-Database-Full-82941666) parancsfájl példája egyik módja a feladat célra.

> [AZURE.IMPORTANT] Helyreállítása, használja az automatikus biztonsági mentést, az SQL Server munkatársi szerepkörök, az előfizetés tagja vagy az előfizetés tulajdonosa. Visszaállíthatja az Azure portálon PowerShell vagy a REST API-t. Nem használhatja a Transact-SQL nyelvben. 

## <a name="point-in-time-restore"></a>Pont és az idő visszaállítása

Pont és az idő visszaállítása lehetővé teszi a meglévő adatbázis visszaállítása szerint egy korábbi pontra az új adatbázis időben, ugyanazon a kiszolgálón logikai használata [SQL-adatbázis automatikus biztonsági mentést](sql-database-automated-backups.md). A meglévő adatbázist nem írható felül. Vissza tudja állítani egy korábbi pontra az idő [az [Azure portál](sql-database-point-in-time-restore-portal.md), PowerShell](sql-database-point-in-time-restore-powershell.md) alrendszerrel vagy a [REST API -t](https://msdn.microsoft.com/library/azure/mt163685.aspx).

> [AZURE.SELECTOR]
- [Pont és az idő visszaállítása: Azure portal](sql-database-point-in-time-restore-portal.md)
- [Pont és az idő visszaállítása: PowerShell](sql-database-point-in-time-restore-powershell.md)

Az adatbázis bármelyik teljesítményszint vagy elasztikus feltölthet készlet. Szükség van-e a logikai kiszolgálón vagy a készlet rugalmas elegendő DTU kvóta. Ne feledje, hogy a visszaállítás létrehoz egy új adatbázist, és a visszaállított adatbázist szolgáltatás réteg és a teljesítmény szintjét lehet ugyanaz, mint az élő adatbázis aktuális állapotát. Miután elkészült, a visszaállított adatbázist egy olyan normál teljes körűen elérhető online adatbázis, normál kamatlába szolgáltatás réteg és a teljesítmény szintekre alapján számítja fel. Díjak nem merülnek fel, amíg befejeződik az adatbázis visszaállítása.

Adatbázis helyreállítási célokra earler pontjához általában visszaállítása. Ha így tesz, kezeli a visszaállított adatbázist az eredeti adatbázis helyettesítőként, vagy annak segítségével adatok beolvasásához, és frissítse a az eredeti adatbázis. 

- ***Adatbázis-helyettesítő:*** A visszaállított adatbázist az eredeti adatbázis helyettesítőként készült, ha ellenőrizni kell a teljesítményszint és/vagy szolgáltatási réteg megfelelőek, és méretezze szükség esetén az adatbázis. Nevezze át az eredeti adatbázist, és majd nevezze el a visszaállított adatbázist az eredeti az adatbázis módosítása parancs használata a T-SQL nyelvben. 
- ***Adatok helyreállítása:*** Ha egy felhasználó vagy az alkalmazás hibát helyreállítása a visszaállított adatbázisból adatok beolvasásához, külön-külön kell írni, majd hajtsa végre a függetlenül adatok helyreállítás parancsfájlok van szüksége az adatok kinyerése a visszaállított adatbázist az eredeti adatbázishoz. Bár a visszaállítási művelet hosszú időt vesz igénybe vehet, a visszaállítás adatbázist is látható lesz a teljes adatbázis listában. A visszaállítás alatt törölje az adatbázist, ha megszakítja a művelet, és Ön nem ráterheljük az adatbázist, amely nem fejeződött be a visszaállítás. 

Részletes információt a pont és az idő visszaállítása használata a felhasználó és az alkalmazás hibát helyreállítása lásd: [pont igény visszaállítás](sql-database-recovery-using-backups.md#point-in-time-restore)

## <a name="deleted-database-restore"></a>A törölt adatbázis visszaállítása

Törölt adatbázis visszaállítása lehetővé teszi a törölt adatbázis visszaállítása ugyanazon a kiszolgálón logikai használata [SQL-adatbázis automatikus biztonsági mentés](sql-database-automated-backups.md)törölt adatbázis törlése ideje. 

> [AZURE.IMPORTANT] Ha töröl egy SQL Azure-adatbázis-kiszolgálói példány, minden az adatbázisok is törlődik, és nem állíthatók. Nem támogatja a Törölt kiszolgáló visszaállítása egyelőre nem.

Ugyanazon vagy egy új adatbázis neve a visszaállított adatbázist is használhatja. Az [Azure portál](sql-database-restore-deleted-database-portal.md) [PowerShell](sql-database-restore-deleted-database-powershell.md) használható vagy a [többi (createMode = visszaállítás)](https://msdn.microsoft.com/library/azure/mt163685.aspx). 

> [AZURE.SELECTOR]
- [Adatbázis visszaállítása törölt: Azure portál](sql-database-restore-deleted-database-portal.md)
- [Adatbázis visszaállítása törölt: PowerShell](sql-database-restore-deleted-database-powershell.md)

## <a name="geo-restore"></a>GEO-visszaállítása

GEO-visszaállítási lehetővé teszi, hogy egy SQL-adatbázis bármelyik Azure régióban bármely kiszolgálón visszaállítani a legutóbbi geo replikált [automatikus napi biztonsági másolat](sql-database-automated-backups.md). GEO-visszaállítás geo felesleges biztonsági adatforrása, és helyreállítása egy adatbázist, még akkor is, ha az adatbázis vagy az Adatközpont miatt nem érhető el egy üzemszünetek használható. Használhatja az [Azure portál](sql-database-geo-restore-portal.md) [PowerShell](sql-database-geo-restore-powershell.md), vagy a [többi (createMode = helyreállítás)](https://msdn.microsoft.com/library/azure/mt163685.aspx) 

> [AZURE.SELECTOR]
- [GEO-visszaállítása: Azure portál](sql-database-geo-restore-portal.md)
- [GEO-visszaállítása: PowerShell](sql-database-geo-restore-powershell.md)

GEO-visszaállítása az alapértelmezett helyreállítási beállítás esetén a hol az adatbázisban vannak tárolva régióban esemény miatt az adatbázis nem érhető el. Ha nagyméretű esemény, a régió eredményezi az adatbázis-alkalmazás elérhetetlenség, a Geo-visszaállítása az adatbázis visszaállítása a legújabb biztonsági másolatból más régióban-kiszolgálón is használhatja. Az összes biztonsági másolatok geo replikált, és beállíthatja, hogy a késleltetést között a biztonsági mentés esetén vett és geo-replikált az Azure blob egy másik régióbeli. A késleltetés lehet akár egy óra, így katasztrófa esetén is lehet meg az 1 óra értéket az adatok elvesztését, tehát a Készletben legfeljebb 1 óra. Az alábbi példában látható az adatbázis visszaállítása az utolsó napi biztonsági másolatból.

![GEO-visszaállítása](./media/sql-database-geo-restore/geo-restore-2.png)

Geo-visszaállítási használatával helyreállítása egy üzemszünetek részletes információt talál [az üzemszünetek helyreállítása](sql-database-disaster-recovery.md)

> [AZURE.IMPORTANT] Az összes szolgáltatási rétegek Geo-visszaállítási érhető el, azt is a legalapvetőbb a katasztrófa helyreállítási megoldások SQL-adatbázisban a leghosszabb Készletben és becsült helyreállítási idő (Beszúrása). A 2 legnagyobb méretű egyszerű adatbázisok GB Geo-visszaállítása a 12 órás egy Beszúrása lehetővé teszi DR megoldást is kínál. Normál vagy prémium összeállítására ha jelentősen rövidebb helyreállítási idővel van szükség, vagy annak valószínűségét, hogy az adatok elvesztését csökkentése érdekében fontolja meg az aktív Geo-replikáció. Aktív Geo replikációs sokkal alsó Készletben és Beszúrása kínál, akkor csak igényel egy folyamatosan replikált másodlagos áttérni kezdeményezhet. A részletekért lásd: [Aktív Geo-replikáció](sql-database-geo-replication-overview.md).

## <a name="programmatically-performing-recovery-using-automated-backups"></a>Az automatikus biztonsági mentés helyreállítási programozás útján végrehajtása

Az Azure-portálra addiition a fent ismertetett módon adatbázis helyreállítási végezhető el programmically Azure PowerShell és a REST API-t. Az alábbi táblázatoknak ismertetik az elérhető parancsok megadása.

### <a name="powershell"></a>A PowerShell

|Parancsmag|Leírás|
|------|-----------|
|[Get-AzureRmSqlDatabase](https://msdn.microsoft.com/en-us/library/azure/mt603648.aspx)|Egy vagy több adatbázis kap.|
|[Get-AzureRMSqlDeletedDatabaseBackup](https://msdn.microsoft.com/en-us/library/azure/mt693387.aspx)|Visszaállíthatja a törölt adatbázisok kap.|
|[Get-AzureRmSqlDatabaseGeoBackup](https://msdn.microsoft.com/library/azure/mt693388.aspx)|Adatbázis geo felesleges biztonsági másolatot kap.|
|[Visszaállítás-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt693390.aspx)|SQL-adatbázis visszaállítása.|
||||

### <a name="rest-api"></a>REST API-VAL

|API|Leírás|
|---|-----------|
|[TÖBBI (createMode = helyreállítás)](https://msdn.microsoft.com/library/azure/mt163685.aspx)|Adatbázis visszaállítása|
|[Get létrehozása, vagy az adatbázis állapotának módosítása](https://msdn.microsoft.com/library/azure/mt643934.aspx)|Az állapot eredménye a visszaállítási művelet közben|
||||



## <a name="summary"></a>Összefoglalás

Automatikus biztonsági mentést az adatbázisok megóvni a felhasználó és alkalmazáshibákat, adatbázis véletlen törlését és hosszan kimaradások. A nulla-költsége nulla-rendszergazdai megoldás az SQL-adatbázisait érhető el. 

## <a name="next-steps"></a>Következő lépések

- Egy üzleti folytonosságot – áttekintés és -esetek [áttekintése](sql-database-business-continuity.md) című témakörben találhat üzleti folytonosságot
- Azure SQL-adatbázis automatikus biztonsági mentés kapcsolatos további tudnivalókért lásd: [az automatikus biztonsági mentés SQL-adatbázis](sql-database-automated-backups.md)
- Gyorsabb helyreállítási lehetőségeket kapcsolatos további tudnivalókért lásd: [A replikáció Geo aktív](sql-database-geo-replication-overview.md)  
- Az automatikus biztonsági mentés archiválásra használatáról című témakörben talál [adatbázis másolása](sql-database-copy.md)
