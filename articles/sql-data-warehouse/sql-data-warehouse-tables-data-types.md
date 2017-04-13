<properties
   pageTitle="A táblákat az SQL adatraktár adattípusok |} Microsoft Azure"
   description="Első lépések az adattípusok Azure SQL-adatraktár táblákhoz."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="jrowlandjones"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/29/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="data-types-for-tables-in-sql-data-warehouse"></a>A táblákat az SQL adatraktár adattípusok

> [AZURE.SELECTOR]
- [– Áttekintés][]
- [Adattípusok][]
- [Terjesztése][]
- [Index][]
- [Partition][]
- [Statisztika][]
- [Ideiglenes][]

SQL-adatraktár támogatja a leggyakrabban használt adattípusok.  Az alábbi képen az SQL adatraktár által támogatott adattípusok listáját.  További részletekért adattípusa támogatja a [tábla létrehozása][]találhat.

|**Támogatott adattípusok**|||
|---|---|---|
[bigint][]|[decimális][]|[smallint][]|
[bináris][]|[Lebegtetés][]|[megjelenítésekor a rendszer][]|
[bit][]|[int][]|[sysname][]|
[karakter][]|[pénzt][]|[idő][]|
[dátum][]|[nchar][]|[tinyint][]|
[dátum és idő][]|[nvarchar][]|[UniqueIdentifier][]|
[datetime2][]|[valós][]|[varbinary][]|
[datetimeoffset][]|[smalldatetime][]|[varchar][]|


## <a name="data-type-best-practices"></a>Adattípus ajánlott eljárások

 Az oszlopban található információ típusa megadásakor a legkisebb adattípust, amely támogatja az adatok hatékonyabbá tehetők a lekérdezési teljesítmény. Ez különösen fontos karakter és VARCHAR oszlopokhoz. A leghosszabb oszlop értéke 25 karakteres, az oszlop majd határozza meg, VARCHAR(25). Kerülje az összes karakter oszlop meghatározása a nagy alapértelmezett hossza. Ezeken kívül meghatározásával oszlopok VARCHAR esetén, amelyek minden szükséges [NVARCHAR][]használata helyett.  Használja a NVARCHAR(4000) vagy VARCHAR(8000), amikor csak lehetséges, NVARCHAR(MAX), VARCHAR(MAX) vagy helyett.

## <a name="polybase-limitation"></a>Polybase korlátozása

Alkalmazás használatakor Polybase betöltése a táblázatok, határozza meg a táblák, hogy a lehetséges sor maximális méretét, többek között a teljes hossza változó hosszúságú oszlopokat, nem haladja meg a 32 767 bájt.  Haladja meg a szélesség is, és töltse be a sorok BCP változó hosszúságú adatokat tartalmazó sor határozhatja meg, amíg nem használhatja az Polybase betöltése ezeket az adatokat.  Számos sorok Polybase támogatása, amint hozzáadódik.

## <a name="unsupported-data-types"></a>Nem támogatott adattípusai

Ha szerint áttelepítése az adatbázis vannak áttérés Azure SQL-adatbázissal, például egy másik SQL-platform, előfordulhatnak néhány SQL adatraktár nem támogatott adattípusok.  Az alábbiakban nem támogatott adattípusok, valamint a néhány alternatívája nem támogatott adattípusok helyett használható.

|Adattípus|Megoldás:|
|---|---|
|[mértani][]|[varbinary][]|
|[földrajzi hely][]|[varbinary][]|
|[Hierarchiaazonosító][]|[nvarchar][] (4000)|
|[kép][ntext,text,image]|[varbinary][]|
|[szöveg][ntext,text,image]|[varchar][]|
|[ntext][ntext,text,image]|[nvarchar][]|
|[sql_variant][]|Adatoszlop felosztása a beírt erősen oszlopokra.|
|[táblázat][]|Ideiglenes táblák konvertálja.|
|[időbélyeg][]|Átdolgozási [datetime2][] használandó kódot és `CURRENT_TIMESTAMP` függvény.  Csak az állandók támogatott alapértelmezettként, ezért current_timestamp nem lehet besorolni a kényszer alapértelmezés szerint. Ha módosítani szeretné a sorértékek verzió áttelepítése időbélyeg beírt oszlop majd használatával [bináris][](8) vagy [VARBINARY][bináris](8) nem üres vagy NULL sor verzió értékeket.|
|[XML][]|[varchar][]|
|[felhasználó által definiált típusok][]|alakíthatja a natív típusok lehetőség szerint|
|alapértelmezett értékek|alapértelmezett értékek literálok, és csak az állandók támogatja.  Nem mérvadó kifejezések vagy függvények, például: `GETDATE()` vagy `CURRENT_TIMESTAMP`, nem támogatottak.|

Az alábbi SQL futtatható a jelenlegi SQL-adatbázis oszlopokat, hogy melyik azonosítása nem támogatja a Azure SQL-adatraktár:

```sql
SELECT  t.[name], c.[name], c.[system_type_id], c.[user_type_id], y.[is_user_defined], y.[name]
FROM sys.tables  t
JOIN sys.columns c on t.[object_id]    = c.[object_id]
JOIN sys.types   y on c.[user_type_id] = y.[user_type_id]
WHERE y.[name] IN ('geography','geometry','hierarchyid','image','text','ntext','sql_variant','timestamp','xml')
 AND  y.[is_user_defined] = 1;
```

## <a name="next-steps"></a>Következő lépések

További információért lásd a cikkek [Táblázat áttekintése][– Áttekintés], [táblázat terjesztése][elosztás], [táblázat indexelés][Index], [táblázat szétválasztás][partíciót], [Táblázat statisztika fenntartása][Statisztika] és [Ideiglenes táblák][ideiglenes].  Ajánlott eljárások bővebben: [SQL adatok raktári gyakorlati tanácsokat][].

<!--Image references-->

<!--Article references-->
[– Áttekintés]: ./sql-data-warehouse-tables-overview.md
[Adattípusok]: ./sql-data-warehouse-tables-data-types.md
[Terjesztése]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statisztika]: ./sql-data-warehouse-tables-statistics.md
[Ideiglenes]: ./sql-data-warehouse-tables-temporary.md
[Ajánlott eljárások a SQL-adatok raktári]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->

<!--Other Web references-->
[táblázat létrehozása]: https://msdn.microsoft.com/library/mt203953.aspx
[bigint]: https://msdn.microsoft.com/library/ms187745.aspx
[bináris]: https://msdn.microsoft.com/library/ms188362.aspx
[bit]: https://msdn.microsoft.com/library/ms177603.aspx
[karakter]: https://msdn.microsoft.com/library/ms176089.aspx
[dátum]: https://msdn.microsoft.com/library/bb630352.aspx
[dátum és idő]: https://msdn.microsoft.com/library/ms187819.aspx
[datetime2]: https://msdn.microsoft.com/library/bb677335.aspx
[datetimeoffset]: https://msdn.microsoft.com/library/bb630289.aspx
[decimális]: https://msdn.microsoft.com/library/ms187746.aspx
[Lebegtetés]: https://msdn.microsoft.com/library/ms173773.aspx
[mértani]: https://msdn.microsoft.com/library/cc280487.aspx
[földrajzi hely]: https://msdn.microsoft.com/library/cc280766.aspx
[Hierarchiaazonosító]: https://msdn.microsoft.com/library/bb677290.aspx
[int]: https://msdn.microsoft.com/library/ms187745.aspx
[pénzt]: https://msdn.microsoft.com/library/ms179882.aspx
[nchar]: https://msdn.microsoft.com/library/ms186939.aspx
[nvarchar]: https://msdn.microsoft.com/library/ms186939.aspx
[ntext,text,image]: https://msdn.microsoft.com/library/ms187993.aspx
[valós]: https://msdn.microsoft.com/library/ms173773.aspx
[smalldatetime]: https://msdn.microsoft.com/library/ms182418.aspx
[smallint]: https://msdn.microsoft.com/library/ms187745.aspx
[megjelenítésekor a rendszer]: https://msdn.microsoft.com/library/ms179882.aspx
[sql_variant]: https://msdn.microsoft.com/library/ms173829.aspx
[sysname]: https://msdn.microsoft.com/library/ms186939.aspx
[táblázat]: https://msdn.microsoft.com/library/ms175010.aspx
[idő]: https://msdn.microsoft.com/library/bb677243.aspx
[időbélyeg]: https://msdn.microsoft.com/library/ms182776.aspx
[tinyint]: https://msdn.microsoft.com/library/ms187745.aspx
[UniqueIdentifier]: https://msdn.microsoft.com/library/ms187942.aspx
[varbinary]: https://msdn.microsoft.com/library/ms188362.aspx
[varchar]: https://msdn.microsoft.com/library/ms186939.aspx
[XML]: https://msdn.microsoft.com/library/ms187339.aspx
[felhasználó által definiált típusok]: https://msdn.microsoft.com/library/ms131694.aspx
