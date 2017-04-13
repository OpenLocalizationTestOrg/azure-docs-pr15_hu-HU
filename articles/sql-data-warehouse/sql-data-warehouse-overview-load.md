   <properties
   pageTitle="Adatok betöltése Azure SQL-adatraktár |} Microsoft Azure"
   description="Ismerje meg az SQL adatraktár betöltése adatok esetei. Ide tartoznak a PolyBase, Azure blob-tárolóhoz, strukturálatlan fájl vagy lemez szállítási használatával. Külső eszközök is használhatja."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="07/12/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="load-data-into-azure-sql-data-warehouse"></a>Adatok betöltése Azure SQL-adatraktár

A javaslatok SQL adatraktár az adatok betöltése és forgatókönyv beállításokat összefoglalását.

Adatok betöltése nagyon nehéz részét általában van előkészítése az adatokat a betöltés. Azure egyszerűbbé teszi a betöltés Azure blob-tárolóhoz használata a közös adattár készült, a szolgáltatások, és Azure Data Factory téve a kommunikációt és az adatok mozgás az Azure szolgáltatásai között. Ezek a folyamatok integrált PolyBase technológiával, amely Azure blob-tárolóhoz párhuzamosan adatok betöltése SQL adatraktár nagymértékben párhuzamos feldolgozási (MPP) segítségével. 

Minta adatbázisok betöltött oktatóanyagok a [mintavállalati adatbázis betöltése][]című témakör tartalmaz.

## <a name="load-from-azure-blob-storage"></a>Azure blob-tárolóhoz betöltése
Leggyorsabban úgy importálni az adatokat az SQL adatraktár adat betöltése Azure blob-tárolóhoz PolyBase használatával. PolyBase párhuzamosan adatok betöltése Azure blob-tárolóhoz SQL adatok raktári nagymértékben párhuzamos feldolgozási (MPP) tervezés használja. Használja a PolyBase, T – SQL-parancsok vagy egy Azure Data Factory folyamat is használhatja.

### <a name="1-use-polybase-and-t-sql"></a>1. PolyBase és az SQL-T használata

Betöltési folyamat összefoglalása:

2. Az adatok formázása UTF-8, mivel PolyBase UTF-16 jelenleg nem támogatja.
2. Adatok áthelyezése Azure blob-tárolóhoz, és tárolhatja, szövegfájlokból.
3. Állítsa be a külső objektumok SQL adatraktár meghatározása a helyét és az adatok formázása
4. Az adatok párhuzamosan betöltése adatbázis új táblába az SQL-T parancs futtatására.

<!-- 5. Schedule and run a loading job. --> 

Oktatóanyagot olvassa el a [Betöltés adatainak SQL adatraktár (PolyBase) az Azure blob-tárolóhoz][]című témakört.

### <a name="2-use-azure-data-factory"></a>2. a Azure Data Factory használata

Egyszerűbb módon PolyBase használatára egy Azure Data Factory folyamat Azure blob-tárolóhoz adatok betöltése SQL adatraktár PolyBase használó is létrehozhat. Ez a gyors, mivel az nem kell az SQL-T objektumok definiálása konfigurálása. Ha anélkül, hogy importálja a fájlt a külső adatokat lekérdezni, használja az SQL-T. 

Betöltési folyamat összefoglalása:

2. Az adatok formázása UTF-8, mivel PolyBase UTF-16 jelenleg nem támogatja.
2. Adatok áthelyezése Azure blob-tárolóhoz, és tárolhatja, szövegfájlokból.
3. Hozzon létre egy Azure Data Factory folyamat való ingest az adatokat. Adja meg a PolyBase beállítást.
4. Ütemezése, és futtassa a folyamat.

Oktatóanyagot olvassa el a [Betöltés adatainak SQL adatraktár (Azure Data Factory) az Azure blob-tárolóhoz][]című témakört.


## <a name="load-from-sql-server"></a>Az SQL Server betöltése
Adatok terhelés SQL adatraktár az SQL Server Integration Services (SSIS), strukturálatlan fájlok átvitele vagy Microsoft szállítási lemezt is használhatja. Lásd: a különböző összefoglalását megjelenítheti olvasható betöltése, folyamatok és oktatóanyagok mutató hivatkozásokat.

Az SQL adatraktár az SQL Server teljes adatok-áttelepítés megtervezéséhez, a az [áttelepítési – áttekintés][]című témakörben találhat. 

### <a name="use-integration-services-ssis"></a>Használja az Integration Services (SSIS)
Ha történő betöltése az SQL Server Integration Services (SSIS) csomagok már esetén, frissítheti az SQL Server tartományként a forrás- és SQL adatraktár célként csomagok. Ez az gyors és egyszerű műveleteket szeretne végezni, és akkor hasznos, ha a nem kívánt áttelepítése a betöltés használandó folyamat adatok már a felhőben. A útján, a betöltés az PolyBase, mert az Ez az SSIS ne hajtsa végre a betöltés párhuzamosan lassabb lesz.

Betöltési folyamat összefoglalása:

1. Javítsa ki az Integration Services csomag, mutasson az SQL Server-példányt a forrás- és az SQL adatraktár adatbázist a cél.
2. A séma áttelepítése az SQL-adatraktár, ha még nem szerepel ott.
3. A hozzárendelés a csomagokban módosítása használata csak a SQL adatraktár által támogatott adattípusok.
3. Ütemezés, és futtassa a csomagot.

Oktatóanyagot olvassa el az [adatok betöltése az SQL Server Azure SQL adatok raktári (SSIS)][]című témakört.

### <a name="use-azcopy-recommended-for--10-tb-data"></a>Használja a AZCopy (< 10 TB adataihoz ajánlott)
Ha az adatok mérete < 10 TB, is exportálja az adatokat az SQL Server strukturálatlan fájlokat, másolja a fájlok Azure blob-tárolóhoz, és PolyBase használatával az adatok betöltése SQL adatraktár

Betöltési folyamat összefoglalása:

1. A segédprogrammal bcp parancssori az SQL Server adatainak exportálása strukturálatlan fájlokat.
2. A segédprogrammal AZCopy parancssori egyszerű fájlok Azure blob-tárolóhoz adatok másolása.
3. PolyBase segítségével SQL adatraktár betöltése.

Oktatóanyagot olvassa el a [Betöltés adatainak SQL adatraktár (PolyBase) az Azure blob-tárolóhoz][]című témakört.

### <a name="use-bcp"></a>Bcp használata
Ha van egy kis mennyiségű adattal bcp közvetlenül az Azure SQL-adatraktár betöltése is használhatja.

Betöltési folyamat összefoglalása:
1. A segédprogrammal bcp parancssori az SQL Server adatainak exportálása strukturálatlan fájlokat.
2. Adatok betöltése egyszerű fájlok közvetlenül az SQL adatraktár bcp használata

Oktatóanyagot lásd: [az SQL Server Azure SQL-adatraktár (bcp) az adatok betöltése][].


### <a name="use-importexport-recommended-for--10-tb-data"></a>Az importálás/exportálás (> 10 TB adataihoz ajánlott) használata
Ha az adatok mérete > 10 TB és Azure helyezze át azt szeretné, azt javasoljuk, hogy használja-e az [Importálás/Exportálás][]szolgáltatás szállítási lemez. 

Betöltési folyamat összefoglalása
2. A segédprogrammal bcp parancssori az SQL Server adatainak exportálása átvihető lemezen strukturálatlan fájlokat.
3. Kiszállítása a lemezt a Microsoftnak.
4. A Microsoft betölti az adatokat az SQL adatraktár

## <a name="load-from-hdinsight"></a>Hdinsight betöltése
SQL-adatraktár betöltése adatok hdinsight keresztül PolyBase támogatja. A folyamat megegyezik az Azure Blob-tárolóhoz adatok betöltése - PolyBase használatával csatlakozhat betöltése az adatok HDInsight. 

### <a name="1-use-polybase-and-t-sql"></a>1. PolyBase és az SQL-T használata

Betöltési folyamat összefoglalása:

2. Az adatok formázása UTF-8, mivel PolyBase UTF-16 jelenleg nem támogatja.
2. Ugrás az adatok hdinsight szolgáltatásból lehetőségre, és tárolja azt a szöveget a fájlok ORC vagy Parquet formátumban.
3. Állítsa be a külső objektumok a SQL adatraktár meghatározása a helyét és az adatok formátumát.
4. Az adatok párhuzamosan betöltése adatbázis új táblába az SQL-T parancs futtatására.

Oktatóanyagot olvassa el a [Betöltés adatainak SQL adatraktár (PolyBase) az Azure blob-tárolóhoz][]című témakört.

## <a name="recommendations"></a>Javaslatok

A partnerek számos van betöltése megoldásokat. Több, olvassa el a [megoldást partnerek][]listáját. 

Ha az adatok nem relációs forrásból származó elérhető lesz, és töltse be kell alakíthat ki, akkor a sorok és oszlopok előtt tölteni az SQL-adatraktár szeretne. Az átalakított adatokat nem kell tárolni az adatbázisban, szövegfájlokból tárolható.

Hozzon létre statisztika újonnan betöltött adatot. Azure SQL-adatraktár támogatási automatikus létrehozása és az automatikus frissítés statisztika még nem tartalmaz.  Jelentős módosításokat fordul elő, az adatok vagy annak érdekében, hogy a legjobb teljesítmény elérése érdekében a lekérdezések juthat, fontos, hogy az összes tábla összes oszlop statisztika létrehozása az első betöltése után.  A részletekért lásd: [statisztikai][].


## <a name="next-steps"></a>Következő lépések
Fejlesztési vonatkozó további tippek a a [fejlesztés – áttekintés][]című témakörben találhat.

<!--Image references-->

<!--Article references-->
[Azure blob-tárolóhoz SQL adatraktár (PolyBase) az adatok betöltése]: ./sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md
[Azure blob-tárolóhoz SQL adatraktár (Azure Data Factory) az adatok betöltése]: ./sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md
[Adatok betöltése az SQL Server Azure SQL adatok raktári (SSIS)]: ./sql-data-warehouse-load-from-sql-server-with-integration-services.md
[Adatok az SQL Server Azure SQL-adatraktár (bcp) terhelés]: ./sql-data-warehouse-load-from-sql-server-with-bcp.md
[Load data from SQL Server to Azure SQL Data Warehouse (AZCopy)]: ./sql-data-warehouse-load-from-sql-server-with-azcopy.md

[Minta adatbázisok betöltése]: ./sql-data-warehouse-load-sample-databases.md
[Áttelepítési – áttekintés]: ./sql-data-warehouse-overview-migrate.md
[megoldás partnerek]: ./sql-data-warehouse-partner-business-intelligence.md
[fejlesztési – áttekintés]: ./sql-data-warehouse-overview-develop.md
[Statisztika]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->

<!--Other Web references-->
[Az importálás/exportálás]: https://azure.microsoft.com/documentation/articles/storage-import-export-service/
