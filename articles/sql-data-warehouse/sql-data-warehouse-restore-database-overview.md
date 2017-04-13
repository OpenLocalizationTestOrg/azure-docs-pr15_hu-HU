<properties
   pageTitle="SQL-adatraktár visszaállítása |} Microsoft Azure"
   description="Áttekintés: Azure SQL-adatraktár az adatbázis visszaállítása az adatbázis visszaállítása lehetőségek."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="Lakshmi1812"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/29/2016"
   ms.author="lakshmir;barbkess;sonyama"/>


# <a name="sql-data-warehouse-restore"></a>SQL-adatraktár visszaállítása

> [AZURE.SELECTOR]
- [– Áttekintés][]
- [Portál][]
- [A PowerShell][]
- [TÖBBI][]

SQL-adatraktár felajánlja a különféle helyi és a földrajzi visszaállítása az adatok raktári katasztrófa részeként helyreállítási lehetőségeket. Adatok raktári biztonsági mentés visszaállítása a adatraktár elsődleges régióban visszaállítási pontra, vagy az geo felesleges biztonsági másolatok Ha vissza szeretne állítani egy másik földrajzi területhez tartozik. Ez a cikk ismerteti, részletei határozzák meg, a adatraktár visszaállítása.

## <a name="what-is-a-data-warehouse-restore"></a>Mi az adatok raktári visszaállítás?

Adatok raktári visszaállítása egy biztonsági másolatot készít egy meglévő alapján létrehozott új adatraktár vagy törölt adatraktár. A visszaállított adatraktár ismét létrehozza a biztonsági mentésben adatraktár egy adott időpontban. SQL adatraktár rendszer elosztott, mivel az adatok raktári visszaállítás biztonságimásolat-fájlokból számos, az Azure BLOB tárolt jön létre. 

Adatbázis visszaállítása bármely üzleti folytonosságot és katasztrófa helyreállítási stratégia alapvető részét képezi, mert azt véletlen sérülést vagy törlését követően újból létrehozza az adatok.

További tudnivalókért lásd:

-  [SQL-adatraktár biztonsági másolatok](sql-data-warehouse-backups.md)
-  [Üzleti folytonosságot – áttekintés](../sql-database/sql-database-business-continuity.md)

## <a name="data-warehouse-restore-points"></a>Adatpontok raktári visszaállítása

Azure prémium tároló használata egy előnyeit, mint adatraktár SQL Azure Blob-tároló pillanatképek használja biztonsági mentése az elsődleges adatraktár. Minden egyes pillanatkép tartalmaz, amely a pillanatkép indításakor visszaállítási pont. Egy adatraktár visszaállításához válasszon egy visszaállítási pontot, és hiba visszaállítása parancsot.  

SQL-adatraktár mindig a biztonsági mentés visszaállítása egy új adatraktár. A visszaállított adatraktár és az aktuális sora, vagy törölje az egyik őket. Ha azt szeretné, az aktuális adatraktár lecserélése a visszaállított adatraktár, átnevezheti azt.

A törölt vagy felfüggesztett adatraktár visszaállítása létrehozhatunk [egy támogatási jegyek](sql-data-warehouse-get-started-create-support-ticket.md). 

<!-- 
### Can I restore a deleted data warehouse?

Yes, you can restore the last available restore point.

Yes, for the next seven calendar days. When you delete a data warehouse, SQL Data Warehouse actually keeps the data warehouse and its snapshots for seven days just in case you need the data. After seven days, you won't be able to restore to any of the restore points. -->

## <a name="geo-redundant-restore"></a>GEO felesleges visszaállítása

Geo felesleges tárolására használja, ha a adatraktár visszaállíthatja a [páronkénti adatközpont](../best-practices-availability-paired-regions.md) egy másik földrajzi területhez tartozik. A adatraktár visszaáll az utolsó napi biztonsági másolatból. 

## <a name="restore-timeline"></a>Ütemterv visszaállítása

Az utóbbi hét napban visszaállíthatja adatbázis tetszőleges elérhető visszaállítása pontra. Egy adott időpontban érvényes megkezdése négy és nyolc óránként és hét napig. Ha 7 napnál régebbi pillanatkép, lejárta, és a visszaállítási pont már nem érhető el.

## <a name="restore-costs"></a>Költségek visszaállítása

A visszaállított adatraktár tároló díja van az Azure prémium tároló díjazott számlát kapni. 

Ha az egérmutatót a visszaállított adatraktár, az előfizetést terhelő az Azure prémium tároló díjazott tároló. A szüneteltetés előnye nem kell fizetnie DWU számítógépes erőforrásait.

SQL adatraktár árak kapcsolatos további tudnivalókért olvassa el az [SQL-adatok raktári árak](https://azure.microsoft.com/pricing/details/sql-data-warehouse/)című témakört.

## <a name="uses-for-restore"></a>A visszaállítás felhasználási módjai

Az elsődleges adatok raktári visszaállításhoz használata adatok helyreállítása után véletlen adatvesztés vagy adatsérülés.

Adatok raktári visszaállítása biztonsági másolatot készít nincs hét napja megőrzéséhez is használhatja. Miután visszaállította a biztonsági mentés, a adatraktár online van, és mutasson határozatlan ideig, hogy mentse a számítási költségeket. A szüneteltetett adatbázist az Azure prémium tároló díjazott tároló költségeket vonz.. 

## <a name="related-topics"></a>Kapcsolódó témakörök

### <a name="scenarios"></a>Felhasználási területei

- Egy üzleti folytonosságot áttekintését a [üzleti folytonosságot áttekintése](../sql-database/sql-database-business-continuity.md) című témakörben találhat.


<!-- ### Tasks -->

Adatok raktári visszaállítás végrehajtásához használatával visszaállíthatja.

- Azure-portálon látható [egy adatraktár az Azure portálon visszaállítása](sql-data-warehouse-restore-database-portal.md)
- PowerShell-parancsmagok látható [egy PowerShell-parancsmagok használata adatraktár visszaállítása](sql-data-warehouse-restore-database-powershell.md)
- VIGYE az API-khoz című részen [visszaállítása egy adatraktár a REST API -k használata](sql-data-warehouse-restore-database-rest-api.md)

<!-- ### Tutorials -->

<!--Image references-->

<!--Article references-->
[Azure SQL Database business continuity overview]: ../sql-database/sql-database-business-continuity.md
[– Áttekintés]: ./sql-data-warehouse-restore-database-overview.md
[Portál]: ./sql-data-warehouse-restore-database-portal.md
[A PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[TÖBBI]: ./sql-data-warehouse-restore-database-rest-api.md

<!--MSDN references-->


<!--Other Web references-->
