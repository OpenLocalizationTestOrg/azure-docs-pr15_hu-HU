<properties
   pageTitle="Azure gépi tanulási használata SQL adatraktár |} Microsoft Azure"
   description="Az Azure gépi tanulási és az Azure SQL-adatraktár megoldások fejlesztésével oktatóprogram."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="kevinvngo"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/16/2016"
   ms.author="kevin;barbkess;sonyama"/>

# <a name="use-azure-machine-learning-with-sql-data-warehouse"></a>Azure gépi tanulási használata SQL adatraktár

Azure gépi tanulási olyan teljes körű felügyelt prediktív analytics szolgáltatás, az adatok ellen prediktív modellek létrehozása az SQL adatraktár, és felhasználja kész webszolgáltatásokhoz majd közzé. Megtudhatja, hogy a cserélendő analitikai és gépi tanulási [gépi tanulási Azure a bevezetés][]olvasásával alapjait.  Ezután elolvashatja, hogy miként hozhat létre, láthatja el ismeretekkel pontszám és gépi tanulási adatmodell használatával az [oktatóprogram kísérletezhet létrehozása][]tesztelése.

Ez a cikk megtanulhatja, hogyan hajtsa végre a következő az [Azure gépi tanulási Studio][]segítségével:

- Olvassa el az adatok hozhat létre, láthatja el ismeretekkel a és a prediktív modell pontszám az adatbázisból
- Adatok írásához a saját adatbázisra


## <a name="read-data-from-sql-data-warehouse"></a>SQL-adatraktár adatainak beolvasása

Azt adatainak beolvasása a AdventureWorksDW adatbázis táblájához termék.

### <a name="step-1"></a>Lépés: 1

Egy új kísérlet kattintva indítsa el a + új a gépi tanulási Studio ablak alján jelölje be a kísérlet, és válassza a üres kísérletezhet. Válassza az alapértelmezett kísérlet nevet a vászonra tetején, és nevezze egy kifejező nevet, például a kerékpárok ár szövegbevitel.

### <a name="step-2"></a>Lépés: 2

Keresse meg a paletta adatkészletek olvasó moduljának és modulok bal szélén a kísérlet vásznat. Húzza a modul a kísérlet vásznat.
![][drag_reader]

### <a name="step-3"></a>3 lépés

Jelölje ki a olvasó modult, és töltse ki a Tulajdonságok ablaktáblában.

1. Jelölje be az adatforrás Azure SQL-adatbázis.
2. Adatbázis-kiszolgáló neve: írja be a kiszolgáló nevét. Keresett elem az [Azure portálon][] is használhatja.

![][server_name]

3. Az adatbázis neve: írja be az adatbázis nevét a kiszolgálón, csak a megadott.
4. Kiszolgáló felhasználói fiók neve: írja be a felhasználó nevét, az adatbázis az access-engedélyekkel rendelkező fiókkal.
5. Kiszolgáló felhasználói fiók jelszava: Adja meg a jelszót az adott felhasználói fiók.
6. Fogadja el a bármely kiszolgálói tanúsítvány: ezt a beállítást (kevésbé biztonságos) használja, ha ki szeretné hagyni a webhely tanúsítvány megtekintésével, mielőtt, olvassa el az adatok.
7. Adatbázis-lekérdezés: Adjon meg egy SQL-utasítást, amely leírja az elolvasni kívánt adatokat. Ebben az esetben azt olvassa adatokat használ a következő lekérdezés termék táblából.


```SQL
SELECT ProductKey, EnglishProductName, StandardCost,
        ListPrice, Size, Weight, DaysToManufacture,
        Class, Style, Color
FROM dbo.DimProduct;
```

![][reader_properties]

### <a name="step-4"></a>Lépés: 4

1. Futtassa a kísérlet a kísérlet vászon a Futtatás gombra kattintva.
2. Amikor befejezte a kísérlet, a olvasó modul lesz egy zöld pipa jelzi, hogy azt sikeresen befejeződött. Figyelje meg, még a befejezett futási állapota a jobb felső sarokban.

![][run]

3. Ha látni szeretné az importált adatokat, kattintson a kimeneti port az rakodótere adatkészlet alján, és válassza a megjelenítés.


## <a name="create-train-and-score-a-model"></a>Létrehozása, láthatja el ismeretekkel a és a modell pontszám

Ezután már használhatja ezt az adatkészlet:

- Hozzon létre egy a modellben: adatfeldolgozás és szolgáltatások meghatározása
- A modell képzése: kiválasztása és alkalmazása egy tanuló algoritmus
- Pontszám és a modell tesztelése: új kerékpárok ár előrejelzésére


![][model]

További információk, hogy miként hozhat létre, képzése, pontszámhoz, és tesztelje a egy gépi tanulási modell használata [létrehozása kísérletezhet oktatóprogram][].

## <a name="write-data-to-azure-sql-data-warehouse"></a>Adatok írásához a Azure SQL-adatraktár

A AdventureWorksDW adatbázis táblájához ProductPriceForecast az eredményhalmaz írja azt.

### <a name="step-1"></a>Lépés: 1

Keresse meg a paletta adatkészletek író moduljának és modulok bal szélén a kísérlet vásznat. Húzza a modul a kísérlet vásznat.

![][drag_writer]

### <a name="step-2"></a>Lépés: 2

Jelölje ki a író modult, és töltse ki a Tulajdonságok ablaktáblában.

1. Jelölje be az adatok cél Azure SQL-adatbázis.
2. Adatbázis-kiszolgáló neve: írja be a kiszolgáló nevét. Keresett elem az [Azure portálon][] is használhatja.
3. Az adatbázis neve: írja be az adatbázis nevét a kiszolgálón, csak a megadott.
4. Kiszolgáló felhasználói fiók neve: írja be a felhasználó nevét, az adatbázis írási engedélyekkel rendelkező fiókkal.
5. Kiszolgáló felhasználói fiók jelszava: Adja meg a jelszót az adott felhasználói fiók.
6. Fogadja el a bármely kiszolgálói tanúsítvány (nem biztonságos): válassza ezt a lehetőséget, ha nem szeretné a tanúsítvány megtekintése.
7. Oszlopok szeretné menteni vesszővel elválasztott listája: Adja meg a kimeneti kívánt adatkészlet vagy eredménye oszlopok listája.
8. Adatok táblázat neve: Adja meg a nevét az adattáblát.
9. Adattábla oszlopok vesszővel elválasztott listája: Adja meg az oszlop nevét az új táblázat. Az oszlopnevek az adatforrás adathalmazban szerint a lehetőségekből eltérő lehet, de Ön szerepelnie kell a ugyanannyi oszlopot itt, amely csak a kimeneti tábla.
10. Egy SQL Azure-művelet írt sorok száma: beállíthatja, hogy egy lépésben SQL-adatbázishoz írt sorok számát.

![][writer_properties]

### <a name="step-3"></a>3 lépés

1. Futtassa a kísérlet a kísérlet vászon a Futtatás gombra kattintva.
2. Amikor befejezte a kísérlet, minden modul lesz egy zöld pipa jelzi, hogy sikeresen befejeződött.

## <a name="next-steps"></a>Következő lépések

Fejlesztési vonatkozó további tippek [SQL adatraktár fejlesztési áttekintése][]című témakörben találhat.

<!--Image references-->

[drag_reader]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-drag-reader.png
[server_name]: ./media/sql-data-warehouse-integrate-azure-machine-learning/dw-server-name.png
[reader_properties]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-reader-properties.png
[run]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-finished-running.png
[model]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-create-train-score-model.png
[drag_writer]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-drag-writer.png
[writer_properties]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-writer-properties.png

<!--Article references-->

[SQL-adatraktár fejlesztése – áttekintés]: ./sql-data-warehouse-overview-develop.md
[Kísérlet oktatóanyagból létrehozása]: https://azure.microsoft.com/documentation/articles/machine-learning-create-experiment/
[A gép a Azure learning – bevezetés]: https://azure.microsoft.com/documentation/articles/machine-learning-what-is-machine-learning/
[Azure gépi tanulási Studio]: https://studio.azureml.net/Home
[Azure portál]: https://portal.azure.com/

<!--MSDN references-->

<!--Other Web references-->

[Azure Machine Learning documentation]: http://azure.microsoft.com/documentation/services/machine-learning/
