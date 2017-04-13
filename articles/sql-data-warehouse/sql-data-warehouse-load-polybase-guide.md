<properties
   pageTitle="Útmutató a PolyBase használata SQL adatraktár |} Microsoft Azure"
   description="Útmutatások és javaslatok a SQL adatraktár esetek PolyBase használatához."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="ckarst"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/30/2016"
   ms.author="cakarst;barbkess;sonyama"/>


# <a name="guide-for-using-polybase-in-sql-data-warehouse"></a>Útmutató az SQL adatraktár PolyBase használata

Ez az útmutató számára PolyBase használata SQL adatraktár hasznos információkat adja vissza.

Első lépésként olvassa el az [adatok betöltése PolyBase az][] oktatóprogram.


## <a name="rotating-storage-keys"></a>Tárterület billentyűk elforgatása

Időről-időre fog módosítani szeretné a hozzáférési billentyűt a blob-tárolóhoz biztonsági okokból.

A legtöbb elegáns a feladat végrehajtásához módja egy úgynevezett "elforgatása a billentyűk" folyamatot. Láthatta, hogy rendelkezik-e két tároló kulcsok blob-tároló fiókjához. Ez az, hogy akkor is az áttűnés

Az Azure tároló fiók billentyűk elforgatása egy három egyszerű lépésben

1. A másodlagos tároló hívóbetű alapján második hatóköre adatbázis hitelesítő létrehozása
2. Ki a új hitelesítő adatok alapján második külső adatforrás létrehozása
3. Húzza, és mutasson az új külső adatforráshoz a külső táblák létrehozása

Amikor megtörtént a külső táblázatok az új külső adatforráshoz, majd az eltávolítási feladatok hajthatók végre:

1. Húzza az első külső adatforrás
2. Első adatbázis legördülő hatóköre a hitelesítő adatok alapján, az elsődleges tároló hívóbetű
3. Jelentkezzen be az Azure, és készen áll a következő alkalommal elsődleges hívóbetű újragenerálása

## <a name="query-azure-blob-storage-data"></a>Azure blob-tároló adatok lekérdezése
Külső tábla lekérdezéseket egyszerűen használja a táblázat neve, mintha relációs táblázat volt.

```sql
-- Query Azure storage resident data via external table.
SELECT * FROM [ext].[CarSensor_Data]
;
```

> [AZURE.NOTE] Külső tábla a lekérdezés az *"A lekérdezés megszakított – a maximális elvetése küszöb külső forrásból olvasásakor elérte"*hiba történhet. Ez azt jelzi, hogy a külső adatok *dirty* rekordokat tartalmazza-e. Ha a tényleges adatok típusok/oszlopok száma nem egyezik meg az a oszlop meghatározása a külső táblában, vagy ha az adatok nem felelnek meg a megadott külső fájlformátum egy adatrekord tekintendő dirty". A hiba kijavításához győződjön meg arról, hogy a külső tábla és a külső fájl formátuma definíciók helyes, ezek meghatározása a külső adatok megfelel-e. Abban az esetben, ha a külső adatok rekordok részhalmazát dirty, megadhatja, ezeket a rekordokat, a lekérdezések elutasításához DDL külső tábla létrehozása az elutasítás parancs a beállítások használatával.


## <a name="load-data-from-azure-blob-storage"></a>Adatok betöltése Azure blob-tárolóhoz
Ebben a példában az Azure blob-tárolóhoz adatok betölti adatraktár SQL-adatbázishoz.

Az adatok átviteli idő lekérdezések adattárolás közvetlenül eltávolítja. A lekérdezési teljesítmény elemzése lekérdezések szerint legfeljebb 10 x columnstore index létrehozása az adatok tárolása javítja.

Ebben a példában a hozzon létre táblázat AS SELECT utasítás adat betöltése. Az új tábla a lekérdezésben oszlop örökli. Az oszlopok adattípusát örökli a külső tábla definícióját.

Hozzon létre táblázat szerint jelölje be egy erősen performant az adatokat, az SQL adatraktár számítási csomópontok párhuzamosan betöltő Transact-SQL-utasítást.  A nagymértékben párhuzamos feldolgozási (MPP) motor Analytics Platform rendszerben eredetileg kifejlesztett és a már a SQL adatraktár.

```sql
-- Load data from Azure blob storage to SQL Data Warehouse

CREATE TABLE [dbo].[Customer_Speed]
WITH
(   
    CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([CarSensor_Data].[CustomerKey])
)
AS
SELECT *
FROM   [ext].[CarSensor_Data]
;
```

Lásd: [létrehozása táblában szerint jelölje be (Transact-SQL nyelvben)][].

## <a name="create-statistics-on-newly-loaded-data"></a>Újonnan betöltött adatok statisztikai létrehozása

Azure SQL-adatraktár támogatási automatikus létrehozása és az automatikus frissítés statisztika még nem tartalmaz.  Jelentős módosításokat fordul elő, az adatok vagy annak érdekében, hogy a legjobb teljesítmény elérése érdekében a lekérdezések juthat, fontos, hogy statisztika hozható létre az összes tábla összes oszlop az első betöltése után.  Statisztikai részletes leírását témakört [Statisztika][] témakörök fejlesztése csoportjában található.  Az alábbi képen egy gyors példa bemutatja, hogyan hozhat létre betöltött ebben a példában a táblázatos statisztikáját.

```sql
create statistics [SensorKey] on [Customer_Speed] ([SensorKey]);
create statistics [CustomerKey] on [Customer_Speed] ([CustomerKey]);
create statistics [GeographyKey] on [Customer_Speed] ([GeographyKey]);
create statistics [Speed] on [Customer_Speed] ([Speed]);
create statistics [YearMeasured] on [Customer_Speed] ([YearMeasured]);
```

## <a name="export-data-to-azure-blob-storage"></a>Adatok exportálása az Azure blob-tárolóhoz
Ez a szakasz adatok exportálása adatraktár SQL Azure blob-tárolóhoz mutatja. Ez a példa létrehozása külső tábla AS KIJELÖLHETI, hogy van egy erősen performant Transact-SQL-utasítás párhuzamosan az adatok exportálása az összes számítási csomópontot.

Az alábbi példa létrehoz egy külső táblában Weblogs2014 oszlop definíciókra és az adatok dbo használatával. Webnaplókra táblázat. A külső tábla definícióját SQL adatraktár tárolja, és a SELECT utasítás eredményeit exportálja a program az adatforrás által megadott blob-tárolóban a "/ archiválása/log2014 /" címtárhoz. Az adatok exportálása a megadott szöveg fájlformátumban.

```sql
CREATE EXTERNAL TABLE Weblogs2014 WITH
(
    LOCATION='/archive/log2014/',
    DATA_SOURCE=azure_storage,
    FILE_FORMAT=text_file_format
)
AS
SELECT
    Uri,
    DateRequested
FROM
    dbo.Weblogs
WHERE
    1=1
    AND DateRequested > '12/31/2013'
    AND DateRequested < '01/01/2015';
```


## <a name="working-around-the-polybase-utf-8-requirement"></a>Munka PolyBase UTF-8 kötelező körül
A bemutató PolyBase támogatja az adatok fájlok, amelyeket UTF-8 kódolású betöltése. UTF-8 használ az azonos karakter kódolással ASCII PolyBase is támogatási betöltése-adatok ASCII-címként kódolt. Azonban PolyBase nem támogatja az egyéb karakter kódolás például UTF-16 / Unicode vagy kiterjesztett ASCII-karakterek. Bővített ASCII ékezetes, amely közös német umlautot például karaktereket tartalmaz.

Ez a követelmény kerülhető a legjobb válasz újból írni UTF-8 kódolást.

Többféleképpen ehhez. Alatti állnak két megközelítés Powershell használatával:

### <a name="simple-example-for-small-files"></a>Kisméretű fájlok egyszerű példa

Az alábbi képen egy egyszerű vonalat Powershell-parancsprogramot, amely hoz létre a fájlt.

```PowerShell
Get-Content <input_file_name> -Encoding Unicode | Set-Content <output_file_name> -Encoding utf8
```

Miközben egy egyszerű ugyanígy az adatok újbóli kódolását célszerű azonban semmiképpen a lehető leghatékonyabb. A io adatfolyam az alábbi példában nagy, sokkal gyorsabb, ugyanezt az eredményt adja.

### <a name="io-streaming-example-for-larger-files"></a>Példa a folyamatos átvitelű IO nagyobb méretű fájlok

Az alábbi kód minta összetettebb, de lehetőséget nyújt a sorokat, jelölje ki azt a forrásból származó adatok sokkal hatékonyabban. A nagyobb méretű fájlok ezt a módszert használja.

```PowerShell
#Static variables
$ascii = [System.Text.Encoding]::ASCII
$utf16le = [System.Text.Encoding]::Unicode
$utf8 = [System.Text.Encoding]::UTF8
$ansi = [System.Text.Encoding]::Default
$append = $False

#Set source file path and file name
$src = [System.IO.Path]::Combine("C:\input_file_path\","input_file_name.txt")

#Set source file encoding (using list above)
$src_enc = $ansi

#Set target file path and file name
$tgt = [System.IO.Path]::Combine("C:\output_file_path\","output_file_name.txt")

#Set target file encoding (using list above)
$tgt_enc = $utf8

$read = New-Object System.IO.StreamReader($src,$src_enc)
$write = New-Object System.IO.StreamWriter($tgt,$append,$tgt_enc)

while ($read.Peek() -ne -1)
{
    $line = $read.ReadLine();
    $write.WriteLine($line);
}
$read.Close()
$read.Dispose()
$write.Close()
$write.Dispose()
```

## <a name="next-steps"></a>Következő lépések
Adatok áthelyezése SQL adatraktár kapcsolatos további információért lásd: az [adatok áttelepítési áttekintése][].

<!--Image references-->

<!--Article references-->
[Load data with bcp]: ./sql-data-warehouse-load-with-bcp.md
[Adatok betöltése a PolyBase]: ./sql-data-warehouse-get-started-load-with-polybase.md
[Statisztika]: ./sql-data-warehouse-tables-statistics.md
[adatok áttelepítése – áttekintés]: ./sql-data-warehouse-overview-migrate.md

<!--MSDN references-->
[supported source/sink]: https://msdn.microsoft.com/library/dn894007.aspx
[copy activity]: https://msdn.microsoft.com/library/dn835035.aspx
[SQL Server destination adapter]: https://msdn.microsoft.com/library/ms141095.aspx
[SSIS]: https://msdn.microsoft.com/library/ms141026.aspx

[CREATE EXTERNAL DATA SOURCE (Transact-SQL)]: https://msdn.microsoft.com/library/dn935022.aspx
[Létrehozása külső FÁJLFORMÁTUM (Transact-SQL nyelvben)]: https://msdn.microsoft.com/library/dn935026) [külső tábla létrehozása (a Transact-SQL nyelvben)] .aspx: https://msdn.microsoft.com/library/dn935021.aspxx

[DROP EXTERNAL DATA SOURCE (Transact-SQL)]: https://msdn.microsoft.com/library/mt146367.aspx
[DROP EXTERNAL FILE FORMAT (Transact-SQL)]: https://msdn.microsoft.com/library/mt146379.aspx
[DROP EXTERNAL TABLE (Transact-SQL)]: https://msdn.microsoft.com/library/mt130698.aspx

[TÁBLÁZAT kijelölése szerint (Transact-SQL nyelvben) létrehozása]: https://msdn.microsoft.com/library/mt204041.aspx
[INSERT...SELECT (Transact-SQL)]: https://msdn.microsoft.com/library/ms174335.aspx
[CREATE MASTER KEY (Transact-SQL)]: https://msdn.microsoft.com/library/ms174382.aspx
[CREATE CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/ms189522.aspx
[CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/mt270260.aspx
[DROP CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/ms189450.aspx

<!-- External Links -->
